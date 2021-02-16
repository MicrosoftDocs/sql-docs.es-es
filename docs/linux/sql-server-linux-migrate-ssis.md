---
title: Extracción, transformación y carga de datos en Linux con SSIS
description: Obtenga información sobre cómo ejecutar paquetes de SQL Server Integration Services (SSIS) en Linux. Además, obtenga información sobre dónde encontrar más información de las funcionalidades de SSIS.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 01/09/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: e21864da00069c6a41138a8506c98ab49e07bf3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339123"
---
# <a name="extract-transform-and-load-data-on-linux-with-ssis"></a>Extracción, transformación y carga de datos en Linux con SSIS

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se describe cómo ejecutar paquetes de SQL Server Integration Services (SSIS) en Linux. SSIS resuelve problemas de integración de datos complejos mediante la extracción de datos de varios orígenes y formatos, así como la transformación y limpieza de los datos y la carga de datos en varios destinos. 

Los paquetes SSIS que se ejecutan en Linux pueden conectarse a Microsoft SQL Server en ejecución en Windows en el entorno local o en la nube, en Linux o en Docker. También pueden conectarse a Azure SQL Database, Azure Synapse Analytics, orígenes de datos ODBC, archivos planos y otros orígenes de datos, incluidos orígenes ADO.NET, archivos XML y servicios OData.

Para obtener más información sobre las capacidades de SSIS, vea [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

## <a name="prerequisites"></a>Requisitos previos

Para ejecutar paquetes SSIS en un equipo Linux, primero tiene que instalar SQL Server Integration Services. SSIS no se incluye en la instalación de SQL Server para equipos Linux. Para obtener instrucciones de instalación, consulte [Instalación de SQL Server Integration Services](sql-server-linux-setup-ssis.md).

También debe tener un equipo Windows para crear y mantener paquetes. Las herramientas de administración y diseño de SSIS son aplicaciones de Windows que no están disponibles actualmente para equipos Linux. 

## <a name="run-an-ssis-package"></a>Ejecutar un paquete SSIS

Para ejecutar un paquete SSIS en un equipo Linux, haga lo siguiente:

1.  Copie el paquete SSIS en el equipo Linux.
2.  Ejecute el comando siguiente:
    ```
    $ dtexec /F \<package name \> /DE <protection password>
    ```

## <a name="run-an-encrypted-password-protected-package"></a>Ejecute un paquete cifrado (protegido por contraseña)
Hay tres formas de ejecutar un paquete SSIS cifrado con una contraseña:

1.  Establezca el valor de la variable de entorno `SSIS_PACKAGE_DECRYPT`, como se muestra en el siguiente ejemplo:

    ```
    SSIS_PACKAGE_DECRYPT=test /opt/ssis/bin/dtexec /f package.dtsx
    ```

2.  Especifique la opción `/de[crypt]` para escribir la contraseña de forma interactiva, tal como se muestra en el siguiente ejemplo:

    ```
    /opt/ssis/bin/dtexec /f package.dtsx /de
    
    Enter decryption password:
    ```

3.  Especifique la opción `/de` para proporcionar la contraseña en la línea de comandos, tal y como se muestra en el siguiente ejemplo. No se recomienda este método porque almacena la contraseña de descifrado con el comando en el historial de comandos.

    ```
    opt/ssis/bin/dtexec /f package.dtsx /de test
    
    Warning: Using /De[crypt] <password> may store decryption password in command history.
    
    You can use /De[crypt] instead to enter interactive mode,
    or use environment variable SSIS_PACKAGE_DECRYPT to set decryption password.
    ```

## <a name="design-packages"></a>Diseñar paquetes

**Conectarse a orígenes de datos ODBC**. Con SSIS en la actualización de CTP 2.1 de Linux y versiones posteriores, los paquetes SSIS pueden usar conexiones ODBC en Linux. Esta funcionalidad se ha probado con SQL Server y los controladores ODBC de MySQL, pero también se espera que funcione con cualquier controlador ODBC de Unicode que respete la especificación de ODBC. En tiempo de diseño, puede proporcionar un DSN o una cadena de conexión para conectarse a los datos ODBC; también puede usar la autenticación de Windows. Para obtener más información, vea [la entrada de blog que anuncia la compatibilidad con ODBC en Linux](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/).

**Rutas**. Proporcione rutas de estilo Windows en los paquetes SSIS. SSIS en Linux no admite rutas de estilo Linux, sino que asigna rutas de estilo Windows a rutas de estilo Linux en tiempo de ejecución. Después, por ejemplo, SSIS en Linux asigna la ruta de estilo Windows `C:\test` a la ruta de estilo Linux `/test`.

## <a name="deploy-packages"></a>Implementar paquetes
En esta versión, solo puede almacenar paquetes en el sistema de archivos de Linux. La base de datos del catálogo de SSIS y el servicio SSIS heredado no están disponibles en Linux para la implementación y el almacenamiento de paquetes.

## <a name="schedule-packages"></a>Programar paquetes
Puede usar herramientas de programación del sistema Linux como `cron` para programar paquetes. No se puede usar el Agente SQL en Linux para programar la ejecución de paquetes en esta versión. Para obtener más información, vea [Programar paquetes SSIS en Linux con cron](sql-server-linux-schedule-ssis-packages.md).

## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

Para obtener información detallada sobre las limitaciones y los problemas conocidos de SSIS en Linux, consulte [Limitaciones y problemas conocidos de SSIS en Linux](sql-server-linux-ssis-known-issues.md).

## <a name="more-info-about-ssis-on-linux"></a>Más información sobre SSIS en Linux

Para obtener más información sobre SSIS en Linux, consulte las siguientes entradas de blog:

-   [SSIS en Linux está disponible en SQL Server CTP 2.1](https://blogs.msdn.microsoft.com/ssis/2017/05/17/ssis-helsinki-is-available-in-sql-server-vnext-ctp2-1/)
-   [ODBC es compatible con SSIS en Linux (actualización de SQL Server CTP 2.1)](https://blogs.msdn.microsoft.com/ssis/2017/06/16/odbc-is-supported-in-ssis-on-linux-ssis-helsinki-ctp2-1-refresh/)

## <a name="more-info-about-ssis"></a>Más información sobre SSIS

Microsoft SQL Server Integration Services (SSIS) es una plataforma que permite generar soluciones de integración de datos de alto rendimiento, entre las que se incluyen paquetes de extracción, transformación y carga de datos (ETL) para el almacenamiento de datos. Para obtener más información sobre SSIS, vea [SQL Server Integration Services](../integration-services/sql-server-integration-services.md).

SSIS incluye las características siguientes:
- Herramientas y asistentes gráficos para compilar y depurar paquetes en Windows
- Una amplia gama de tareas para realizar funciones de flujo de trabajo, como operaciones FTP, ejecutar instrucciones SQL y enviar mensajes de correo electrónico
- Diversos orígenes de datos y destinos para extraer y cargar datos
- Diversas transformaciones para limpiar, agregar, combinar y copiar datos
- Interfaces de programación de aplicaciones (API) para extender SSIS con sus propios scripts y componentes personalizados

Para comenzar con SSIS, descargue la versión más reciente de [SQL Server Data Tools (SSDT)](../integration-services/ssis-how-to-create-an-etl-package.md).

Para más información sobre SSIS, vea los siguientes artículos:
- [Más información sobre SQL Server Integration Services](../integration-services/sql-server-integration-services.md)
- [Herramientas de implementación y administración de SQL Server Integration Services (SSIS)](../integration-services/integration-services-ssis-development-and-management-tools.md)
- [Tutoriales de SQL Server Integration Services](../integration-services/integration-services-tutorials.md)

## <a name="related-content-about-ssis-on-linux"></a>Contenido relacionado sobre SSIS en Linux
-   [Instalar SQL Server Integration Services (SSIS) en Linux](sql-server-linux-setup-ssis.md)
-   [Configuración de SQL Server Integration Services en Linux con ssis-conf](sql-server-linux-configure-ssis.md)
-   [Limitaciones y problemas conocidos de SSIS en Linux](sql-server-linux-ssis-known-issues.md)
-   [Programación de la ejecución de paquetes de SQL Server Integration Services en Linux con cron](sql-server-linux-schedule-ssis-packages.md)