---
title: Cambios de aislamiento para Windows
description: En este artículo se describen los cambios en el mecanismo de aislamiento de Machine Learning Services en SQL Server 2019 en Windows. Estos cambios afectan a SQLRUserGroup, las reglas de firewall, los permisos de archivo y la autenticación implícita.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/05/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ace298ff0e69ef3e3ee964c15d894280d6ef1b99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082406"
---
# <a name="sql-server-2019-on-windows-isolation-changes-for-machine-learning-services"></a>SQL Server 2019 en Windows: Cambios de aislamiento para Machine Learning Services
[!INCLUDE [SQL Server 2019 - Windows only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

En este artículo se describen los cambios en el mecanismo de aislamiento de Machine Learning Services en SQL Server 2019 en Windows. Estos cambios afectan a **SQLRUserGroup**, las reglas de firewall, los permisos de archivo y la autenticación implícita.

Para obtener más información, vea cómo instalar [SQL Server Machine Learning Services en Windows](sql-machine-learning-services-windows-install.md).

## <a name="changes-to-isolation-mechanism"></a>Cambios en el mecanismo de aislamiento

En Windows, el programa de instalación de SQL Server 2019 cambia el mecanismo de aislamiento para los procesos externos. Este cambio reemplaza las cuentas de trabajo locales por contenedores [AppContainer](/windows/desktop/secauthz/appcontainer-isolation), una tecnología de aislamiento para las aplicaciones cliente que se ejecutan en Windows. 

No hay elementos de acción específicos para el administrador como resultado de la modificación. En un servidor nuevo o actualizado, todos los scripts y código externos ejecutados desde [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) siguen automáticamente el nuevo modelo de aislamiento. 

En resumen, las principales diferencias en esta versión son las siguientes:

+ Las cuentas de usuario locales en el **grupo de usuarios restringidos de SQL (SQLRUserGroup)** ya no se crean ni se usan para ejecutar procesos externos. Los contenedores AppContainer las reemplazan.
+ La pertenencia a **SQLRUserGroup** ha cambiado. En lugar de varias cuentas de usuario locales, la pertenencia consta únicamente de la cuenta de servicio de SQL Server Launchpad. Los procesos de R y Python ahora se ejecutan con la identidad de servicio de Launchpad, aislada a través de contenedores AppContainer.

Aunque el modelo de aislamiento ha cambiado, en SQL Server 2019 el Asistente para la instalación y los parámetros de la línea de comandos siguen siendo los mismos. Para obtener ayuda con la instalación, vea [Instalación de SQL Server Machine Learning Services (Python y R) en Windows](sql-machine-learning-services-windows-install.md).

## <a name="about-appcontainer-isolation"></a>Acerca del aislamiento de AppContainer

En las versiones anteriores, **SQLRUserGroup** contenía un grupo de cuentas de usuario de Windows locales (MSSQLSERVER00-MSSQLSERVER20) que se usaban para aislar y ejecutar procesos externos. Cuando se necesitaba un proceso externo, el servicio de SQL Server Launchpad tomaba una cuenta disponible y la usaba para ejecutar un proceso. 

En SQL Server 2019, el programa de instalación ya no crea cuentas profesionales locales. En cambio, el aislamiento se consigue a través de contenedores [AppContainer](/windows/desktop/secauthz/appcontainer-isolation). En tiempo de ejecución, cuando se detecta un código o script insertado en un procedimiento almacenado o una consulta, SQL Server llama a Launchpad con una solicitud para un iniciador específico de la extensión. Launchpad invoca el entorno de runtime adecuado en un proceso bajo su identidad y crea una instancia de AppContainer para que lo contenga. Este cambio es beneficioso porque ya no se requiere la administración de cuentas y contraseñas locales. Además, en las instalaciones en las que las cuentas de usuario locales están prohibidas, la eliminación de la dependencia de la cuenta de usuario local comporta la posibilidad de usar esta característica.

Según la implementación de SQL Server, los contenedores AppContainer son un mecanismo interno. Aunque no verá ninguna evidencia física de los contenedores AppContainer en el monitor de procesos, podrá encontrarlos en las reglas de firewall de salida creadas por el programa de instalación para evitar que los procesos realicen llamadas de red.

## <a name="firewall-rules-created-by-setup"></a>Reglas de firewall creadas por el programa de instalación

De forma predeterminada, SQL Server deshabilita las conexiones salientes mediante la creación de reglas de firewall. Antiguamente, estas reglas se basaban en cuentas de usuario locales, en las que el programa de instalación creó una regla de salida para **SQLRUserGroup** que denegaba a sus miembros el acceso a la red (cada cuenta profesional aparecía como un principio local sujeto a la regla). 

Como parte del cambio a contenedores AppContainer, hay nuevas reglas de firewall que se basan en los SID de AppContainer: una para cada uno de los 20 contenedores AppContainer creados por el programa de instalación de SQL Server. Las convenciones de nomenclatura para el nombre de la regla de firewall se corresponden a **Block network access for AppContainer-00 in SQL Server instance MSSQLSERVER** (Bloquear el acceso a la red para AppContainer-00 en la instancia MSSQLSERVER de SQL Server), en el que 00 es el número del contenedor AppContainer (00-20 de forma predeterminada) y MSSQLSERVER es el nombre de la instancia de SQL Server. 

> [!Note]
> Si se requieren llamadas de red, puede deshabilitar las reglas de salida en Firewall de Windows.

<a name="file-permissions"></a>

## <a name="file-permissions"></a>Permisos de archivo

De forma predeterminada, los scripts externos de Python y R solo tienen permiso de acceso de lectura en sus directorios de trabajo. 

Si los scripts de Python o R necesitan acceso a cualquier otro directorio, debe proporcionar permisos de **Lectura y ejecución** y/o **Escritura** en la cuenta de usuario del servicio **NT Service\MSSQLLaunchpad** y **TODOS LOS PAQUETES DE APLICACIONES** en este directorio.

Siga los pasos siguientes para conceder el acceso.

1. En el Explorador de archivos, haga clic con el botón derecho en la carpeta que quiera usar como directorio de trabajo y seleccione **Propiedades**.
1. Seleccione **Seguridad** y haga clic en **Editar...** para cambiar los permisos.
1. Haga clic en **Agregar...** .
1. Asegúrese de que el valor de **Desde esta ubicación** sea el nombre del equipo local.
1. Escriba **TODOS LOS PAQUETES DE APLICACIONES** en **Escriba los nombres de objeto que desea seleccionar** y haga clic en **Comprobar nombres**. Haga clic en **OK**.
1. Seleccione **Leer y ejecutar** en la columna **Permitir**.
1. Seleccione **Escribir** en la columna **Permitir**, si quiere conceder permisos de escritura.
1. Haga clic en **Aceptar** y en **Aceptar**.

### <a name="program-file-permissions"></a>Permisos de archivos de programa

Al igual que en las versiones anteriores, el **SQLRUserGroup** sigue proporcionando permisos de lectura y ejecución en los archivos ejecutables de los directorios de SQL Server **Binn**, **R_SERVICES** y **PYTHON_SERVICES**. En esta versión, el único miembro de **SQLRUserGroup** es la cuenta del servicio SQL Server Launchpad.  Cuando el servicio Launchpad inicia un entorno de ejecución de R o Python, el proceso se ejecuta como servicio LaunchPad.

## <a name="implied-authentication"></a>Autenticación implícita

Como anteriormente, sigue siendo necesaria una configuración adicional para la *autenticación implícita* en los casos en los que el script o código tiene que volver a conectarse a SQL Server con autenticación de confianza para recuperar datos o recursos. La configuración adicional implica la creación de un inicio de sesión de base de datos para **SQLRUserGroup**, cuyo único miembro ahora es la única cuenta de servicio de SQL Server Launchpad, en lugar de varias cuentas de trabajo. Para obtener más información sobre esta tarea, vea [Agregar SQLRUserGroup como usuario de base de datos](../security/create-a-login-for-sqlrusergroup.md).


## <a name="symbolic-link-created-by-setup"></a>Vínculo simbólico creado por el programa de instalación

Se crea un vínculo simbólico al valor predeterminado actual **R_SERVICES** y **PYTHON_SERVICES**, como parte del programa de instalación de SQL Server. Si no quiere crear este vínculo, una alternativa es conceder el permiso de lectura "todos los paquetes de aplicaciones" a la jerarquía que conduce a la carpeta.


## <a name="see-also"></a>Consulte también

+ [Instalación de SQL Server Machine Learning Services (Python y R) en Windows](sql-machine-learning-services-windows-install.md)
+ [Instalación de SQL Server Machine Learning Services (Python y R) en Linux](../../linux/sql-server-linux-setup-machine-learning.md)