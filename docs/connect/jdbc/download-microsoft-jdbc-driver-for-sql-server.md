---
title: Descarga de Microsoft JDBC Driver para SQL Server
description: Descargue Microsoft JDBC Driver para SQL Server para desarrollar aplicaciones Java que se conectan a SQL Server y Azure SQL Database.
ms.date: 03/02/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 451181b8-11e6-4d01-b547-9ac5aada8238
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bfb200c1b29f15489f8500ccb02d5bf066bf6937
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575227"
---
# <a name="download-microsoft-jdbc-driver-for-sql-server"></a>Descarga de Microsoft JDBC Driver para SQL Server

Microsoft JDBC Driver para SQL Server es un controlador JDBC de tipo 4 que proporciona conectividad con bases de datos a través de las interfaces de programación de aplicaciones (API) estándar de JDBC disponibles en la plataforma Java. Las descargas del controlador están disponibles para todos los usuarios sin cargo adicional. Proporcionan acceso a SQL Server desde cualquier aplicación Java, servidor de aplicaciones o applet habilitado para Java.

## <a name="download"></a>Descargar

La versión 9.2 es la versión de disponibilidad general (GA) más reciente. Admite Java 8, 11 y 15. Si tiene que ejecutar en un runtime de Java anterior, vea la [matriz de compatibilidad de la especificación Java y JDBC](microsoft-jdbc-driver-for-sql-server-support-matrix.md#java-and-jdbc-specification-support) para comprobar si existe una versión de controlador compatible que puede usar. Estamos trabajando continuamente para mejorar la compatibilidad con la conectividad de Java. Por tanto, se recomienda encarecidamente trabajar con la versión más reciente de Microsoft JDBC Driver.

**[![Descargar](../../ssms/media/download-icon.png) Descarga de Microsoft JDBC Driver 9.2 para SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2155948)**  
**[![Descargar](../../ssms/media/download-icon.png) Descarga de Microsoft JDBC Driver 9.2 para SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2155949)**  

### <a name="version-information"></a>Información de la versión

- Número de versión: 9.2.1
- Fecha de publicación: 2 de marzo de 2021

Al descargar el controlador, hay varios archivos JAR. El nombre del archivo JAR indica la versión de Java que admite.

> [!Note]
> Si accede a esta página desde una versión de idioma que no es el inglés y quiere ver el contenido más actualizado, visite la [versión en inglés de EE. UU. del sitio](https://aka.ms/downloadmssqljdbcenglish). Puede descargar distintos idiomas en el sitio de la versión en inglés de EE. UU. si selecciona [idiomas disponibles](#available-languages).

## <a name="available-languages"></a>Idiomas disponibles

Esta versión de Microsoft JDBC Driver para SQL Server está disponible en los idiomas siguientes:

Microsoft JDBC Driver 9.2.1 para SQL Server (zip): [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2155948&clcid=0x40a)

Microsoft JDBC Driver 9.2.1 para SQL Server (tar.gz): [Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2155949&clcid=0x40a)

### <a name="release-notes"></a>Notas de la versión

Para obtener más detalles sobre esta versión, vea las [notas de la versión](release-notes-for-the-jdbc-driver.md) y los [requisitos del sistema](system-requirements-for-the-jdbc-driver.md).

### <a name="previous-releases"></a>Versiones anteriores

Para descargar versiones anteriores, vea las [versiones anteriores de Microsoft JDBC Driver para SQL Server](release-notes-for-the-jdbc-driver.md#previous-releases).

## <a name="using-the-jdbc-driver-with-maven-central"></a>Uso del controlador JDBC con Maven Central

El controlador JDBC se puede agregar a un proyecto de Maven si se incorpora como una dependencia en el archivo POM.xml con el siguiente código:

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```  

## <a name="unsupported-drivers"></a>Controladores no compatibles

Las versiones de controladores no compatibles no están disponibles para descargarse aquí. Estamos trabajando continuamente para mejorar la compatibilidad con la conectividad de Java. Por tanto, se recomienda encarecidamente trabajar con la versión más reciente de Microsoft JDBC Driver.  
  
## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Microsoft JDBC Driver para SQL Server, consulte [Introducción al controlador JDBC](overview-of-the-jdbc-driver.md) y el [repositorio de GitHub del controlador JDBC](https://github.com/microsoft/mssql-jdbc/blob/dev/README.md).
