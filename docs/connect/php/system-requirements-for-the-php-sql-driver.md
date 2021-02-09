---
title: Requisitos del sistema para los controladores de Microsoft para PHP
description: Los controladores de Microsoft para PHP para SQL Server admiten una amplia gama de versiones de PHP, sistemas operativos y versiones de SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- requirements
ms.assetid: 5db4b75f-c605-4785-9560-399a533c0fc9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06824f62740653eddeee6d3484e7eb8914404110
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076447"
---
# <a name="system-requirements-for-the-microsoft-drivers-for-php-for-sql-server"></a>Requisitos del sistema para los controladores de Microsoft para PHP para SQL Server

[!INCLUDE[Driver_PHP_Download](../../includes/driver_php_download.md)]

En este artículo se enumeran los componentes que se deben instalar en el sistema para tener acceso a los datos de una instancia de SQL Server o Azure SQL Database con [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].

Oficialmente, se admiten las versiones 4.0 y posteriores de los controladores PHP de Microsoft para SQL Server. Para información completa sobre los requisitos y los ciclos de vida de soporte técnico de los controladores PHP, consulte la [matriz de compatibilidad](microsoft-php-drivers-for-sql-server-support-matrix.md).

## <a name="php"></a>PHP

Para obtener información sobre cómo descargar e instalar los archivos binarios estables más recientes, vea [el sitio web de PHP](https://php.net).  Los controladores de Microsoft para PHP para SQL Server requieren las versiones correctas de PHP, tal como se detallan en [Compatibilidad de versiones de PHP](microsoft-php-drivers-for-sql-server-support-matrix.md#php-version-support).

-   La versión correcta del archivo del controlador debe estar habilitada con su versión de PHP correspondiente. Consulte [Versiones del controlador](#driver-versions) para información sobre los distintos archivos de controlador. Para descargar los controladores, vea [Download the Microsoft Drivers for PHP for SQL Server](download-drivers-php-sql-server.md) (Descargar los controladores de Microsoft para PHP para SQL Server). Para obtener información sobre cómo configurar el controlador para el runtime PHP, vea [Loading the Microsoft Drivers for PHP for SQL Server](loading-the-php-sql-driver.md) (Cargar los controladores de Microsoft para PHP para SQL Server).

-   Se requiere un servidor web. El servidor web debe estar configurado para ejecutar PHP. Para información sobre el hospedaje de aplicaciones PHP con IIS, consulte [el tutorial en el sitio web de PHP](http://docs.php.net/manual/da/install.windows.iis7.php).

    Los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)] se han probado utilizando IIS 10 con FastCGI.

> [!NOTE]
> Microsoft solo proporciona compatibilidad con IIS.

## <a name="odbc-driver"></a>Controlador ODBC

Se requiere la versión correcta de Microsoft ODBC Driver for SQL Server en el equipo en el que se está ejecutando PHP. Puede descargar todas las versiones compatibles del controlador para plataformas compatibles en [esta página](../odbc/download-odbc-driver-for-sql-server.md).

Si está descargando la versión de Windows del controlador en una versión de 64 bits de Windows, el instalador de ODBC 64 bits instala los controladores ODBC de 32 bits y de 64 bits. Si usa una versión de Windows de 32 bits, use el instalador de ODBC x86. En plataformas que no son de Windows, solo están disponibles las versiones de 64 bits del controlador.

|Versión del controlador PHP &#8594;<br />&#8595; Versión del controlador ODBC|5.9|5.8|5.6|5.3|5.2|4.3|4.0|3.2|
|---|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|ODBC Driver 17+ |Sí|Sí|Sí|Sí|Sí|   |   |   |
|ODBC Driver 13.1|Sí|Sí|Sí|Sí|Sí|Sí|Sí|   |
|ODBC Driver 13  |   |   |   |   |   |   |Sí|   |
|ODBC Driver 11  |Sí|Sí|Sí|Sí|Sí|Sí|Sí|Sí|

Si usa el controlador SQLSRV, [sqlsrv_client_info](sqlsrv-client-info.md) devuelve información sobre qué versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Microsoft ODBC Driver for SQL Server que utiliza el [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Si está usando el controlador PDO_SQLSRV, puede usar [PDO::getAttribute](pdo-getattribute.md) para detectar la versión.

## <a name="sql-server"></a>SQL Server

Vea las [versiones de base de datos compatibles](microsoft-php-drivers-for-sql-server-support-matrix.md#sql-server-version-certified-compatibility) para obtener más información sobre las versiones de SQL Server que se admiten.

## <a name="operating-systems"></a>Sistemas operativos

Consulte los [sistemas operativos compatibles](microsoft-php-drivers-for-sql-server-support-matrix.md#supported-operating-systems) para más información sobre qué sistemas operativos son compatibles.

## <a name="driver-versions"></a>Versiones del controlador

En esta sección se enumeran los archivos de controladores que se incluyen con cada versión de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)]. Cada paquete de instalación contiene los archivos de los controladores SQLSRV y PDO_SQLSRV en las variantes de subprocesos y de no subproceso. En Windows, también están disponibles en variantes de 32 bits y de 64 bits. Para configurar el controlador para su uso con el tiempo de ejecución de PHP, siga las instrucciones de instalación que aparecen en [Carga de los controladores de Microsoft para PHP para SQL Server](loading-the-php-sql-driver.md).

En las versiones admitidas de Linux y macOS, se pueden instalar los controladores adecuados con el sistema de paquetes PECL de PHP, siguiendo las [instrucciones de instalación de Linux y macOS](installation-tutorial-linux-mac.md). Como alternativa, puede descargar los archivos binarios precompilados para su plataforma en la página del proyecto [Controladores de Microsoft para PHP para SQL Server](https://github.com/Microsoft/msphpsql/releases) de GitHub. En las tablas siguientes se enumeran los archivos que se encuentran en los paquetes binarios precompilados.

**Controladores de Microsoft 5.9 para PHP para SQL Server:**

En Windows, se proporcionan los siguientes archivos de controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_73_nts.dll de 32 bits<br />php_pdo_sqlsrv_73_nts.dll de 32 bits|7.3|no |php7.dll de 32 bits|
|php_sqlsrv_73_ts.dll de 32 bits <br />php_pdo_sqlsrv_73_ts.dll de 32 bits |7.3|sí|php7ts.dll de 32 bits|
|php_sqlsrv_73_nts.dll de 64 bits<br />php_pdo_sqlsrv_73_nts.dll de 64 bits|7.3|no |php7.dll de 64 bits|
|php_sqlsrv_73_ts.dll de 64 bits <br />php_pdo_sqlsrv_73_ts.dll de 64 bits |7.3|sí|php7ts.dll de 64 bits|
|php_sqlsrv_74_nts.dll de 32 bits<br />php_pdo_sqlsrv_74_nts.dll de 32 bits|7.4|no |php7.dll de 32 bits|
|php_sqlsrv_74_ts.dll de 32 bits <br />php_pdo_sqlsrv_74_ts.dll de 32 bits |7.4|sí|php7ts.dll de 32 bits|
|php_sqlsrv_74_nts.dll de 64 bits<br />php_pdo_sqlsrv_74_nts.dll de 64 bits|7.4|no |php7.dll de 64 bits|
|php_sqlsrv_74_ts.dll de 64 bits <br />php_pdo_sqlsrv_74_ts.dll de 64 bits |7.4|sí|php7ts.dll de 64 bits|
|php_sqlsrv_80_nts.dll de 32 bits<br />php_pdo_sqlsrv_80_nts.dll de 32 bits|8.0|no |php8.dll de 32 bits|
|php_sqlsrv_80_ts.dll de 32 bits <br />php_pdo_sqlsrv_80_ts.dll de 32 bits |8.0|sí|php8ts.dll de 32 bits|
|php_sqlsrv_80_nts.dll de 64 bits<br />php_pdo_sqlsrv_80_nts.dll de 64 bits|8.0|no |php8.dll de 64 bits|
|php_sqlsrv_80_ts.dll de 64 bits <br />php_pdo_sqlsrv_80_ts.dll de 64 bits |8.0|sí|php8ts.dll de 64 bits|

En Linux, se proporcionan los siguientes archivos de controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|no |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|sí|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|no |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|sí|
|php_sqlsrv_80_nts.so<br />php_pdo_sqlsrv_80_nts.so|8.0|no |
|php_sqlsrv_80_ts.so <br />php_pdo_sqlsrv_80_ts.so |8.0|sí|

**Controladores 5.8 de Microsoft para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_72_nts.dll de 32 bits<br />php_pdo_sqlsrv_72_nts.dll de 32 bits|7.2|no |php7.dll de 32 bits|
|php_sqlsrv_72_ts.dll de 32 bits <br />php_pdo_sqlsrv_72_ts.dll de 32 bits |7.2|sí|php7ts.dll de 32 bits|
|php_sqlsrv_72_nts.dll de 64 bits<br />php_pdo_sqlsrv_72_nts.dll de 64 bits|7.2|no |php7.dll de 64 bits|
|php_sqlsrv_72_ts.dll de 64 bits <br />php_pdo_sqlsrv_72_ts.dll de 64 bits |7.2|sí|php7ts.dll de 64 bits|
|php_sqlsrv_73_nts.dll de 32 bits<br />php_pdo_sqlsrv_73_nts.dll de 32 bits|7.3|no |php7.dll de 32 bits|
|php_sqlsrv_73_ts.dll de 32 bits <br />php_pdo_sqlsrv_73_ts.dll de 32 bits |7.3|sí|php7ts.dll de 32 bits|
|php_sqlsrv_73_nts.dll de 64 bits<br />php_pdo_sqlsrv_73_nts.dll de 64 bits|7.3|no |php7.dll de 64 bits|
|php_sqlsrv_73_ts.dll de 64 bits <br />php_pdo_sqlsrv_73_ts.dll de 64 bits |7.3|sí|php7ts.dll de 64 bits|
|php_sqlsrv_74_nts.dll de 32 bits<br />php_pdo_sqlsrv_74_nts.dll de 32 bits|7.4|no |php7.dll de 32 bits|
|php_sqlsrv_74_ts.dll de 32 bits <br />php_pdo_sqlsrv_74_ts.dll de 32 bits |7.4|sí|php7ts.dll de 32 bits|
|php_sqlsrv_74_nts.dll de 64 bits<br />php_pdo_sqlsrv_74_nts.dll de 64 bits|7.4|no |php7.dll de 64 bits|
|php_sqlsrv_74_ts.dll de 64 bits <br />php_pdo_sqlsrv_74_ts.dll de 64 bits |7.4|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|no |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|sí|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|no |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|sí|
|php_sqlsrv_74_nts.so<br />php_pdo_sqlsrv_74_nts.so|7.4|no |
|php_sqlsrv_74_ts.so <br />php_pdo_sqlsrv_74_ts.so |7.4|sí|

**Controladores de Microsoft 5.6 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|no |php7.dll de 32 bits|
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|sí|php7ts.dll de 32 bits|
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|no |php7.dll de 64 bits|
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|sí|php7ts.dll de 64 bits|
|php_sqlsrv_72_nts.dll de 32 bits<br />php_pdo_sqlsrv_72_nts.dll de 32 bits|7.2|no |php7.dll de 32 bits|
|php_sqlsrv_72_ts.dll de 32 bits <br />php_pdo_sqlsrv_72_ts.dll de 32 bits |7.2|sí|php7ts.dll de 32 bits|
|php_sqlsrv_72_nts.dll de 64 bits<br />php_pdo_sqlsrv_72_nts.dll de 64 bits|7.2|no |php7.dll de 64 bits|
|php_sqlsrv_72_ts.dll de 64 bits <br />php_pdo_sqlsrv_72_ts.dll de 64 bits |7.2|sí|php7ts.dll de 64 bits|
|php_sqlsrv_73_nts.dll de 32 bits<br />php_pdo_sqlsrv_73_nts.dll de 32 bits|7.3|no |php7.dll de 32 bits|
|php_sqlsrv_73_ts.dll de 32 bits <br />php_pdo_sqlsrv_73_ts.dll de 32 bits |7.3|sí|php7ts.dll de 32 bits|
|php_sqlsrv_73_nts.dll de 64 bits<br />php_pdo_sqlsrv_73_nts.dll de 64 bits|7.3|no |php7.dll de 64 bits|
|php_sqlsrv_73_ts.dll de 64 bits <br />php_pdo_sqlsrv_73_ts.dll de 64 bits |7.3|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|no |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|sí|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|no |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|sí|
|php_sqlsrv_73_nts.so<br />php_pdo_sqlsrv_73_nts.so|7.3|no |
|php_sqlsrv_73_ts.so <br />php_pdo_sqlsrv_73_ts.so |7.3|sí|

**Controladores de Microsoft 5.3 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts.dll de 32 bits <br />php_pdo_sqlsrv_7_nts.dll de 32 bits |7.0|no |php7.dll de 32 bits|
|php_sqlsrv_7_ts.dll de 32 bits  <br />php_pdo_sqlsrv_7_ts.dll de 32 bits  |7.0|sí|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts.dll de 64 bits <br />php_pdo_sqlsrv_7_nts.dll de 64 bits |7.0|no |php7.dll de 64 bits|
|php_sqlsrv_7_ts.dll de 64 bits  <br />php_pdo_sqlsrv_7_ts.dll de 64 bits  |7.0|sí|php7ts.dll de 64 bits|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|no |php7.dll de 32 bits|
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|sí|php7ts.dll de 32 bits|
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|no |php7.dll de 64 bits|
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|sí|php7ts.dll de 64 bits|
|php_sqlsrv_72_nts.dll de 32 bits<br />php_pdo_sqlsrv_72_nts.dll de 32 bits|7.2|no |php7.dll de 32 bits|
|php_sqlsrv_72_ts.dll de 32 bits <br />php_pdo_sqlsrv_72_ts.dll de 32 bits |7.2|sí|php7ts.dll de 32 bits|
|php_sqlsrv_72_nts.dll de 64 bits<br />php_pdo_sqlsrv_72_nts.dll de 64 bits|7.2|no |php7.dll de 64 bits|
|php_sqlsrv_72_ts.dll de 64 bits <br />php_pdo_sqlsrv_72_ts.dll de 64 bits |7.2|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|no |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|sí|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|no |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|sí|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|no |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|sí|

**Controladores de Microsoft 5.2 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts.dll de 32 bits <br />php_pdo_sqlsrv_7_nts.dll de 32 bits |7.0|no |php7.dll de 32 bits|
|php_sqlsrv_7_ts.dll de 32 bits  <br />php_pdo_sqlsrv_7_ts.dll de 32 bits  |7.0|sí|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts.dll de 64 bits <br />php_pdo_sqlsrv_7_nts.dll de 64 bits |7.0|no |php7.dll de 64 bits|
|php_sqlsrv_7_ts.dll de 64 bits  <br />php_pdo_sqlsrv_7_ts.dll de 64 bits  |7.0|sí|php7ts.dll de 64 bits|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|no |php7.dll de 32 bits|
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|sí|php7ts.dll de 32 bits|
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|no |php7.dll de 64 bits|
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|sí|php7ts.dll de 64 bits|
|php_sqlsrv_72_nts.dll de 32 bits<br />php_pdo_sqlsrv_72_nts.dll de 32 bits|7.2|no |php7.dll de 32 bits|
|php_sqlsrv_72_ts.dll de 32 bits <br />php_pdo_sqlsrv_72_ts.dll de 32 bits |7.2|sí|php7ts.dll de 32 bits|
|php_sqlsrv_72_nts.dll de 64 bits<br />php_pdo_sqlsrv_72_nts.dll de 64 bits|7.2|no |php7.dll de 64 bits|
|php_sqlsrv_72_ts.dll de 64 bits <br />php_pdo_sqlsrv_72_ts.dll de 64 bits |7.2|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|no |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|sí|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|no |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|sí|
|php_sqlsrv_72_nts.so<br />php_pdo_sqlsrv_72_nts.so|7.2|no |
|php_sqlsrv_72_ts.so <br />php_pdo_sqlsrv_72_ts.so |7.2|sí|

**Controladores de Microsoft 4.3 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts.dll de 32 bits <br />php_pdo_sqlsrv_7_nts.dll de 32 bits |7.0|no |php7.dll de 32 bits|
|php_sqlsrv_7_ts.dll de 32 bits  <br />php_pdo_sqlsrv_7_ts.dll de 32 bits  |7.0|sí|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts.dll de 64 bits <br />php_pdo_sqlsrv_7_nts.dll de 64 bits |7.0|no |php7.dll de 64 bits|
|php_sqlsrv_7_ts.dll de 64 bits  <br />php_pdo_sqlsrv_7_ts.dll de 64 bits  |7.0|sí|php7ts.dll de 64 bits|
|php_sqlsrv_71_nts.dll de 32 bits<br />php_pdo_sqlsrv_71_nts.dll de 32 bits|7.1|no |php7.dll de 32 bits|
|php_sqlsrv_71_ts.dll de 32 bits <br />php_pdo_sqlsrv_71_ts.dll de 32 bits |7.1|sí|php7ts.dll de 32 bits|
|php_sqlsrv_71_nts.dll de 64 bits<br />php_pdo_sqlsrv_71_nts.dll de 64 bits|7.1|no |php7.dll de 64 bits|
|php_sqlsrv_71_ts.dll de 64 bits <br />php_pdo_sqlsrv_71_ts.dll de 64 bits |7.1|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|no |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|sí|
|php_sqlsrv_71_nts.so<br />php_pdo_sqlsrv_71_nts.so|7.1|no |
|php_sqlsrv_71_ts.so <br />php_pdo_sqlsrv_71_ts.so |7.1|sí|

**Controladores de Microsoft 4.0 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_7_nts_x86.dll<br />php_pdo_sqlsrv_7_nts_x86.dll|7.0|no|php7.dll de 32 bits|
|php_sqlsrv_7_ts_x86.dll<br />php_pdo_sqlsrv_7_ts_x86.dll|7.0|sí|php7ts.dll de 32 bits|
|php_sqlsrv_7_nts_x64.dll<br />php_pdo_sqlsrv_7_nts_x64.dll|7.0|no|php7.dll de 64 bits|
|php_sqlsrv_7_ts_x64.dll<br />php_pdo_sqlsrv_7_ts_x64.dll|7.0|sí|php7ts.dll de 64 bits|

En Linux, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|
|---------------|:---------------:|:----------------:|
|php_sqlsrv_7_nts.so <br />php_pdo_sqlsrv_7_nts.so |7.0|no |
|php_sqlsrv_7_ts.so  <br />php_pdo_sqlsrv_7_ts.so  |7.0|sí|

**Controladores de Microsoft 3.2 para PHP para SQL Server:**

En Windows, se incluyen las versiones siguientes del controlador:

|Archivo de controlador|Versión de PHP|¿Seguridad para subprocesos?|Uso con PHP.dll|
|---------------|:---------------:|:----------------:|---------------------|
|php_sqlsrv_54_nts.dll<br />php_pdo_sqlsrv_54_nts.dll|5.4|no|php5.dll|
|php_sqlsrv_54_ts.dll<br />php_pdo_sqlsrv_54_ts.dll|5.4|sí|php5ts.dll|
|php_sqlsrv_55_nts.dll<br />php_pdo_sqlsrv_55_nts.dll|5.5|no|php5.dll|
|php_sqlsrv_55_ts.dll<br />php_pdo_sqlsrv_55_ts.dll|5.5|sí|php5ts.dll|
|php_sqlsrv_56_nts.dll<br />php_pdo_sqlsrv_56_nts.dll|5.6|no|php5.dll|
|php_sqlsrv_56_ts.dll<br />php_pdo_sqlsrv_56_ts.dll|5.6|sí|php5ts.dll|

## <a name="see-also"></a>Consulte también

- [Introducción a los controladores de Microsoft para PHP para SQL Server](getting-started-with-the-php-sql-driver.md)
- [Guía de programación para los controladores de Microsoft para PHP para SQL Server](programming-guide-for-php-sql-driver.md)
- [Referencia de API del controlador SQLSRV](sqlsrv-driver-api-reference.md)
- [Referencia de la API del controlador PDO_SQLSRV](pdo-sqlsrv-driver-reference.md)
