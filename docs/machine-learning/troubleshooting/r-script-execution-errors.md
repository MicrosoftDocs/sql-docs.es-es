---
title: Errores comunes de scripting de R
description: En este artículo se documentan varios errores comunes de scripting que podría encontrar cuando ejecuta el código de R en SQL Server.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 05/31/2018
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 0f9f086ec09830e65f85ca2df5232e8b401cb9eb
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186422"
---
# <a name="common-r-scripting-errors-in-sql-server"></a>Errores comunes de scripting de R en SQL Server
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

En este artículo se documentan varios errores comunes de scripting cuando se ejecuta el código de R en SQL Server. Esta lista no está completa. Hay muchos paquetes y los errores pueden variar entre las distintas versiones del mismo paquete.

## <a name="valid-script-fails-in-t-sql-or-in-stored-procedures"></a>Se produce un error en un script válido en T-SQL o en los procedimientos almacenados

Antes de ajustar el código de R en un procedimiento almacenado, es una buena idea ejecutar el código de R en un IDE externo o en una de las herramientas de R, como RTerm o RGui. Mediante el uso de estos métodos, puede probar y depurar el código mediante los mensajes de error detallados devueltos por R.

Sin embargo, a veces es posible que un código que funciona perfectamente en un IDE o una utilidad externos no se ejecute en un procedimiento almacenado o en un contexto de cálculo de SQL Server. Si esto ocurre, hay una serie de problemas que hay que buscar antes de que pueda asumir que el paquete no funciona en SQL Server.

1. Compruebe si Launchpad se está ejecutando.

2. Revise los mensajes para ver si los datos de entrada o los datos de salida contienen columnas con tipos de datos incompatibles o no admitidos. Por ejemplo, las consultas en una base de datos SQL suelen devolver GUID o ROWGUID, y no se admiten. Para obtener más información, consulte [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

3. Revise las páginas de ayuda de las funciones individuales de R para determinar si se admiten todos los parámetros para el contexto de cálculo de SQL Server. Para obtener ayuda sobre ScaleR, use los comandos de ayuda de R en línea o consulte [Referencia de paquete](/r-server/r-reference/revoscaler/revoscaler).

Si el tiempo de ejecución de R funciona pero el script devuelve errores, se recomienda que intente depurar el script en un entorno de desarrollo de R dedicado, como Herramientas de R para Visual Studio.

También se recomienda revisar y reescribir ligeramente el script para corregir cualquier problema con los tipos de datos que pueden surgir al trasladar datos entre R y el motor de base de datos. Para obtener más información, consulte [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

Además, puede usar el paquete sqlrutils para agrupar el script de R en un formato que se consuma más fácilmente como un procedimiento almacenado. Para más información, consulte:
* [Paquete sqlrutils](../r/ref-r-sqlrutils.md)
* [Creación de un procedimiento almacenado mediante sqlrutils](/machine-learning-server/r-reference/sqlrutils/how-to-create-a-stored-procedure-from-r)

## <a name="script-returns-inconsistent-results"></a>El script devuelve resultados incoherentes

Los scripts de R pueden devolver valores diferentes en un contexto de SQL Server, por varias razones:

- La conversión de tipos implícita se realiza automáticamente en algunos tipos de datos, cuando los datos se pasan entre SQL Server y R. Para obtener más información, vea [Bibliotecas de R y tipos de datos](../r/r-libraries-and-data-types.md).

- Determinación de si el valor de bits es un factor. Por ejemplo, a menudo hay diferencias en los resultados de las operaciones matemáticas de las bibliotecas de punto flotante de 32 y 64 bits.

- Determinación de si se han producido NaN en cualquier operación. Esto puede invalidar los resultados.

- Se pueden amplificar pequeñas diferencias cuando se toma un valor recíproco de un número cercano a cero.

- Los errores de redondeo acumulados pueden provocar valores menores que cero en lugar de cero.

## <a name="implied-authentication-for-remote-execution-via-odbc"></a>Autenticación implícita para la ejecución remota mediante ODBC

Si se conecta al equipo con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ejecutar comandos de R mediante las funciones **RevoScaleR**, puede aparecer un error al usar llamadas ODBC que escriben datos en el servidor. Este error solo se produce cuando se usa la autenticación de Windows.

La razón es que las cuentas de trabajo que se crean para R Services no tienen permiso para conectarse al servidor. Por lo tanto, no se pueden ejecutar llamadas ODBC en su nombre. El problema no se produce con los inicios de sesión de SQL porque, con los inicios de sesión de SQL, las credenciales se pasan explícitamente desde el cliente de R a la instancia de SQL Server y, a continuación, a ODBC. Sin embargo, el uso de inicios de sesión de SQL también es menos seguro que el uso de la autenticación de Windows.

Para permitir que las credenciales de Windows se pasen de forma segura desde un script que se inicia de forma remota, SQL Server debe emular las credenciales. Este proceso se denomina _autenticación implícita_. Para realizar este trabajo, las cuentas de trabajo que ejecutan scripts de R o Python en el equipo de SQL Server deben tener los permisos correctos.

1. Abra [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] como administrador en la instancia donde quiera ejecutar el código de R.

2. Ejecute el siguiente script. Asegúrese de editar el nombre del grupo de usuarios, si ha cambiado el valor predeterminado, y el nombre de equipo y de instancia.

    ```sql
    USE [master]
    GO
    
    CREATE LOGIN [computername\\SQLRUserGroup] FROM WINDOWS WITH
    DEFAULT_DATABASE=[master], DEFAULT_LANGUAGE=[language]
    GO
    ```

## <a name="avoid-clearing-the-workspace-while-youre-running-r-in-a-sql-compute-context"></a>No borrado del área de trabajo mientras se ejecuta R en un contexto de cálculo de SQL

Aunque es habitual borrar el área de trabajo cuando se trabaja en la consola de R, puede tener consecuencias no deseadas en un contexto de cálculo de SQL.

`revoScriptConnection` es un objeto del área de trabajo de R que contiene información sobre una sesión de R a la que se llama desde SQL Server. Pero si su código de R incluye un comando para borrar el área de trabajo (como `rm(list=ls())`), también se borrarán toda la información sobre la sesión y otros objetos del área de trabajo de R.

Como solución alternativa, evite borrar indiscriminadamente las variables y otros objetos mientras ejecuta R en SQL Server. Elimine variables específicas mediante la función **eliminar**:

```R
remove('name1', 'name2', ...)
```

Si quiere eliminar varias variables, se recomienda guardar los nombres de las variables temporales en una lista y, a continuación, realizar periódicamente la recolección de elementos no utilizados en una lista.



## <a name="next-steps"></a>Pasos siguientes

[Solución de problemas y problemas conocidos de Machine Learning Services](machine-learning-troubleshooting-overview.md)

[Recopilación de datos para la solución de problemas de aprendizaje automático](data-collection-ml-troubleshooting-process.md)

[Instalación de SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md)

[Solución de problemas de conexiones de motor de base de datos](../../database-engine/configure-windows/troubleshoot-connecting-to-the-sql-server-database-engine.md)