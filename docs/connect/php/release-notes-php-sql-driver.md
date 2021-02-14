---
title: Notas de la versión de los controladores de Microsoft para PHP
description: En esta página se describe lo que ha cambiado en cada versión de los controladores de Microsoft para PHP para SQL Server.
ms.custom: ''
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- what's new in version 1.1
ms.assetid: 91cca3d2-ba99-4a6d-b0de-beb9699cb3f8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8d53811bef7cb4c570d2aa60638474f815548a17
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058340"
---
# <a name="release-notes-for-the-microsoft-drivers-for-php-for-sql-server"></a>Notas de la versión de los controladores de Microsoft para PHP para SQL Server

En esta página se describe lo que se agregó en cada versión de [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  

<!--
Hello, We are standardizing the format of content inside our Release Notes (or What's New) articles.
Instead of bullets (or paragraphs), we have shifted to the 2-column format you see for H2 **What's New in Version 5.6**.
It is not necessary to reformat all the older H2 sections in this Release Notes file, but.....

Going forward, please be sure to use the 2-column format.

Also, all Release Notes .md file names now must begin with 'release-notes-*.md'.  And no filler words.
The 5.6 edition of this file is being renamed.....
FROM:  'release-notes-for-the-php-sql-driver.md'
TO  :  'release-notes-php-sql-driver.md'

For any questions, ask GeneMi or CraigG.
Thanks a lot.  2019-03-28  (DevO= 1467988)
-->

## <a name="59"></a>5.9

![Descargar](../../ssms/media/download-icon.png) [Descarga del paquete de Windows](https://go.microsoft.com/fwlink/?linkid=?????)
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.9.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.9.0<sup>1</sup>
- Fecha de publicación: 29 de enero de 2021

### <a name="whats-new-in-59"></a>Novedades de la versión 5.9

| Nuevo elemento | Detalles |
| :------- | :------ |
| Se ha agregado compatibilidad con PHP 8.0. | &nbsp; |
| Se ha quitado la compatibilidad con PHP 7.2. | &nbsp; |
| Se ha agregado compatibilidad con Microsoft ODBC Driver 17.7 en todas las plataformas. | &nbsp; |
| Se ha agregado compatibilidad con macOS Big Sur, Ubuntu 20.04, Ubuntu 20.10 y Alpine 3.12. | Algunos sistemas operativos requieren el controlador ODBC 17.5 o una versión posterior. |
| Se ha quitado la compatibilidad con macOS High Sierra, Debian 8 y Ubuntu 19.10. | &nbsp; |
| Compatibilidad con la configuración regional GB18030. | &nbsp; |
| Se ha ampliado la compatibilidad con PDO::errorInfo para incluir mensajes de ODBC adicionales, si están disponibles. | &nbsp; |
| Compatibilidad con la clasificación de datos con información de rango. | Requiere SQL Server 2019 y el controlador ODBC 17.4.2 o posterior. |
| Se ha agregado compatibilidad con la autenticación de la entidad de servicio de Azure Active Directory. | Requiere el controlador ODBC 17.7 o una versión posterior. |
| Se ha mejorado el rendimiento al usar números decimales como entradas o salidas y se han quitado las conversiones innecesarias de los valores numéricos. | &nbsp; |
| Se ha mejorado el rendimiento al capturar números mediante búferes de cliente. | &nbsp; |
| Establecimiento del tiempo de espera de la consulta sin usar el tiempo de expiración de bloqueo, lo que ahorra un viaje adicional al servidor. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> Esta versión requiere el controlador ODBC 17.4.2 o una versión posterior. De lo contrario, aparecerá una advertencia por no definir un atributo. Esta advertencia se puede suprimir si se usa un controlador ODBC anterior. Si usa SQLSRV, consulte [Procedimientos: Configuración del control de errores y advertencias con el controlador SQLSRV](https://docs.microsoft.com/sql/connect/php/how-to-configure-error-and-warning-handling-using-the-sqlsrv-driver). Si usa PDO_SQLSRV, las advertencias se suprimen de forma predeterminada, pero se pueden registrar. Consulte el articulo [Actividad de registro](https://docs.microsoft.com/sql/connect/php/logging-activity) para obtener más información.

## <a name="581"></a>5.8.1

Esta versión solo se aplica a Linux y macOS.

[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.1)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.8.1
- Fecha de publicación: 15 de abril de 2020

### <a name="whats-new-in-581"></a>Novedades de la versión 5.8.1

| Nuevo elemento | Detalles |
| :------- | :------ |
| Corrección de errores | Se corrigieron problemas de configuración regional predeterminados en Alpine Linux. |
| Corrección de errores | Se quitó la estructura de datos innecesaria para admitir la característica de cursores del lado cliente en Alpine Linux. |
| Corrección de errores | Se corrigieron problemas de registro cuando ambos controladores están habilitados en Alpine Linux. |
| &nbsp; | &nbsp; |

## <a name="58"></a>5.8

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120362)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.8.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.8.0
- Fecha de publicación: 31 de enero de 2020

### <a name="whats-new-in-58"></a>Novedades de la versión 5.8

| Nuevo elemento | Detalles |
| :------- | :------ |
| Se agregó compatibilidad con PHP 7.4. | &nbsp; |
| Se quitó la compatibilidad con PHP 7.1. | &nbsp; |
| Se agregó compatibilidad con Microsoft ODBC Driver 17.5 en todas las plataformas. | &nbsp; |
| Se agregó compatibilidad con Debian 10 y Red Hat 8. | Ambos requieren el controlador ODBC 17.4 o superior. |
| Se agregó compatibilidad con macOS Catalina, Alpine Linux 3.11<sup>1</sup> y Ubuntu 19.10. | Todos requieren el controlador ODBC 17.5 o superior. |
| Se quitó la compatibilidad con SQL Server 2008 R2, macOS Sierra, Ubuntu 18.10 y Ubuntu 19.04. | &nbsp; |
| Compatibilidad con la opción Language al conectarse a SQL Server. | &nbsp; |
| Compatibilidad con los tipos de cadenas extendidas de PHP que se introdujeron en PHP 7.2. | &nbsp; |
| Compatibilidad con la recuperación de metadatos de confidencialidad de clasificación de datos. | Requiere SQL Server 2019 y el controlador ODBC 17.4.2 o posterior. |
| Compatibilidad con Always Encrypted con enclaves seguros | Requiere el controlador ODBC 17.4 o superior. |
| Compatibilidad con opciones configurables para la configuración regional en Linux y macOS. |
| Rendimiento mejorado al almacenar en caché los metadatos en las capturas y al omitir las llamadas redundantes. | &nbsp; |
| &nbsp; | &nbsp; |

<sup>1</sup> La compatibilidad con Alpine Linux es experimental para la versión 5.8.

## <a name="previous-releases"></a>Versiones anteriores

## <a name="561"></a>5.6.1

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120446)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.1)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.6.1
- Fecha de publicación: 19 de marzo de 2019

### <a name="whats-new-in-561"></a>Novedades de la versión 5.6.1

| Nuevo elemento | Detalles |
| :------- | :------ |
| Corrección de errores | Se han corregido supuestos que se realizaban al calcular metadatos de campo o de columna que podrían haber dado lugar a la finalización de la aplicación. |
| Corrección de errores | Se modificó el archivo de configuración sqlsrv para que se pueda compilar de forma independiente de pdo_sqlsrv. |
| Corrección de errores | Se corrigió PDOStatement::getColumnMeta() para que se devuelva false cuando algo sale mal. |
| &nbsp; | &nbsp; |

## <a name="56"></a>5.6

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120450)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.6.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.6.0
- Fecha de publicación: 21 de febrero de 2019

### <a name="whats-new-in-56"></a>Novedades de la versión 5.6

| Nuevo elemento | Detalles |
| :------- | :------ |
| Admite PHP 7.3. | &nbsp; |
| Se quitó la compatibilidad con PHP 7.0. | &nbsp; |
| Compatibilidad con Microsoft ODBC Driver 17.3 en todas las plataformas. | &nbsp; |
| Compatibilidad con macOS Mojave. | Requiere el controlador ODBC 17.3 o superior. |
| Compatibilidad con Ubuntu 18.10 y SUSE Linux 15. | Ambos requieren el controlador ODBC 17.3 o superior. |
| Se quitó la compatibilidad con Linux Ubuntu 17.10 y macOS El Capitan. | &nbsp; |
| Compatibilidad con el token de acceso de Azure AD. | En Linux y macOS, requiere el controlador ODBC 17.2 y versiones posteriores y unixODBC 2.3.6 y versiones posteriores. |
| Compatibilidad con la autenticación con Azure AD mediante Identidad administrada para recursos de Azure. | Requiere el controlador ODBC 17.3 o versiones posteriores. |
| Nuevas funcionalidades de captura | &bull; &nbsp; Nueva marca PDO::SQLSRV_ATTR_FETCHES_DATETIME_TYPE para que pdo_sqlsrv devuelva datetime como objetos.<br/><br/>&bull; &nbsp; Agregue la opción ReturnDatesAsStrings en el nivel de instrucción para sqlsrv.<br/><br/>&bull; &nbsp; Opciones nuevas en los niveles de conexión y de instrucción para ambos controladores para dar formato a los valores decimales en los resultados capturados. |
| Compatibilidad con la compilación estática de controladores si los usuarios deciden compilar desde el origen. | &nbsp; |
| Rendimiento mejorado al almacenar en caché los metadatos en las capturas y acelerar las conversiones de cadenas Unicode. | &nbsp; |
| &nbsp; | &nbsp; |

## <a name="53"></a>5.3

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120447)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.3.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.3.0
- Fecha de publicación: 20 de julio de 2018

### <a name="whats-new-in-53"></a>Novedades de la versión 5.3

- Compatibilidad con Microsoft ODBC Driver 17.2 en todas las plataformas.
- Compatibilidad con macOS High Sierra (requiere el controlador ODBC 17 y versiones posteriores).
- Compatibilidad con Azure Key Vault para Always Encrypted para las funcionalidades CRUD básicas, de modo que la característica Always Encrypted esté disponible para todas las plataformas compatibles con Windows, Linux o macOS [Uso de Always Encrypted con los controladores PHP para SQL Server](using-always-encrypted-php-drivers.md)
- Compatibilidad con Ubuntu 18.04 LTS (requiere el controlador ODBC 17.2).
- Compatibilidad con la resistencia de conexión en Linux o macOS también (requiere el controlador ODBC 17.2).

## <a name="52"></a>5.2

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120451)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v5.2.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 5.2.0
- Fecha de publicación: 23 de marzo de 2018

### <a name="whats-new-in-52"></a>Novedades de la versión 5.2

- Compatibilidad con PHP 7.2.1 y superior en Windows, y 7.2.0 y superior en otras plataformas.
- Compatibilidad con Microsoft ODBC Driver 17
  - La versión 17 es ahora la versión predeterminada en todas las plataformas.
- Compatibilidad con Ubuntu 17.10, Debian 9 y SuSE Enterprise Linux 12.
- Se quitó la compatibilidad con Ubuntu 15.10.
- Compatibilidad con Always Encrypted con funcionalidades CRUD en Windows. Para obtener más información, vea [Using Always Encrypted with the PHP Drivers for SQL Server](using-always-encrypted-php-drivers.md) (Uso de Always Encrypted con los controladores PHP para SQL Server).
  - Compatibilidad con el Almacén de certificados de Windows.
  - Always Encrypted solo es compatible con Microsoft ODBC Driver 17 y versiones posteriores.
- Compatibilidad con configuraciones regionales no UTF8 en Linux y macOS.
  - Las configuraciones regionales no UTF8 en Linux y macOS solo se admiten con Microsoft ODBC Driver 17 y versiones posteriores.
- Compatibilidad de Azure Synapse Analytics
- Compatibilidad con Instancia administrada de Azure SQL.

## <a name="43"></a>4.3

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120616)  
[Etiqueta de versión de GitHub (los paquetes de Linux y macOS están disponibles aquí)](https://github.com/Microsoft/msphpsql/releases/tag/v4.3.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 4.3.0
- Fecha de publicación: 6 de julio de 2017

### <a name="whats-new-in-43"></a>Novedades de la versión 4.3

- Compatibilidad con PHP 7.1
- Compatibilidad con macOS Sierra y macOS el Capitan.
- Compatibilidad con Ubuntu 15.10 y Debian 8.
- Se quitó la compatibilidad con Ubuntu 15.04.
- Compatibilidad con grupos de disponibilidad de Always On a través de la resolución de IP de red transparente. Para obtener más información, consulte [Connection Options](connection-options.md).
- Compatibilidad agregada para el tipo de datos sql_variant con limitación.
- Compatibilidad con la resistencia de conexión inactiva en Windows. Para obtener más información, consulte [Connection Options](connection-options.md).
- Compatibilidad con la agrupación de conexiones para Linux y macOS. Para obtener más información, vea [Connection Pooling](connection-pooling-microsoft-drivers-for-php-for-sql-server.md) (Agrupación de conexiones).
- Compatibilidad con la autenticación de Azure Active Directory con ActiveDirectoryPassword y SqlPassword. Para obtener más información, consulte [Connection Options](connection-options.md).

## <a name="40"></a>4.0

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120448)  
[Etiqueta de versión de GitHub](https://github.com/microsoft/msphpsql/releases/tag/v4.0-RTW)

### <a name="version-information"></a>Información de la versión

- Número de versión: 4.0
- Fecha de publicación: 1 de julio de 2016

### <a name="whats-new-in-40"></a>Novedades de la versión 4.0

- Compatibilidad con PHP 7.0  
- Compatibilidad completa con 64 bits
- Compatibilidad con Ubuntu 15.04, Ubuntu 16.04 y RedHat 7

## <a name="32"></a>3.2

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2120449)  
[Etiqueta de versión de GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.2.0.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 3.2
- Fecha de publicación: 9 de marzo de 2015

### <a name="whats-new-in-32"></a>Novedades de la versión 3.2

- Admite PHP 5.6.  
- Incluye las actualizaciones más recientes para las versiones anteriores de PHP 5.5 y 5.4.  
- Requiere Microsoft ODBC Driver 11 for SQL Server  

## <a name="31"></a>3.1

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2143027)  
[Etiqueta de versión de GitHub](https://github.com/microsoft/msphpsql/releases/tag/v3.1.0.0)

### <a name="version-information"></a>Información de la versión

- Número de versión: 3.1
- Fecha de publicación: 12 de diciembre de 2014

### <a name="whats-new-in-31"></a>Novedades de la versión 3.1

- Admite PHP 5.5.  
- Requiere Microsoft ODBC Driver 11 for SQL Server. Las versiones anteriores requerían SQL Native Client.  

## <a name="30"></a>3.0

![descargar](../../ssms/media/download-icon.png) [Descargar el paquete de Windows](https://go.microsoft.com/fwlink/?linkid=2143026)  

### <a name="whats-new-in-30"></a>Novedades de la versión 3.0  

- Admite PHP 5.4.  PHP 5.2 no es compatible con la versión 3 de los [!INCLUDE[ssDriverPHP](../../includes/ssdriverphp_md.md)].  
- Se agrega la opción de conexión AttachDBFileName. Para obtener más información, consulte [Connection Options](connection-options.md).  
- Compatibilidad con LocalDB, que se agregó en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]. Para más información, consulte [Compatibilidad con LocalDB](php-driver-for-sql-server-support-for-localdb.md).
- Se agrega la opción de conexión AttachDBFileName. Para obtener más información, consulte [Connection Options](connection-options.md).  
- Admite las características de alta disponibilidad y recuperación ante desastres. Para más información, consulte [Compatibilidad con alta disponibilidad y recuperación ante desastres](php-driver-for-sql-server-support-for-high-availability-disaster-recovery.md).
- Admite cursores de cliente (almacenamiento en caché de un conjunto de resultados en memoria). Para obtener más información, vea [Cursor Types &#40;SQLSRV Driver&#41; (Tipos de cursor &#40;Controlador SQLSRV&#41;)](cursor-types-sqlsrv-driver.md) y [Cursor Types &#40;PDO_SQLSRV Driver&#41; (Tipos de cursor &#40;Controlador PDO_SQLSRV&#41;)](cursor-types-pdo-sqlsrv-driver.md).
- Se ha agregado el atributo PDO::ATTR_EMULATE_PREPARES. Para obtener más información, vea [PDO::prepare](pdo-prepare.md).  

## <a name="20"></a>2.0

### <a name="whats-new-in-20"></a>Novedades de la versión 2.0

En la versión 2.0, se ha agregado la compatibilidad con el controlador PDO_SQLSRV. Para obtener más información, vea [Referencia del controlador PDO_SQLSRV](pdo-sqlsrv-driver-reference.md).  

## <a name="see-also"></a>Consulte también

[Información general de los controladores de Microsoft para PHP para SQL Server](overview-of-the-php-sql-driver.md)
