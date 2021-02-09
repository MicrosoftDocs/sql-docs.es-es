---
title: Descargar controlador ODBC para SQL Server
description: Descargue Microsoft ODBC Driver for SQL Server para desarrollar aplicaciones de código nativo que se conectan a SQL Server y Azure SQL Database.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 53b09784-bb9d-4fd4-99d3-0492b3308ac4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 29f67c57bb6609d037d6e7077a91e5c7229e93fb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195321"
---
# <a name="download-odbc-driver-for-sql-server"></a>Descargar controlador ODBC para SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Microsoft ODBC Driver for SQL Server es una biblioteca de vínculos dinámicos (DLL) compatible con el entorno de ejecución para aplicaciones que usan API de código nativo para conectarse a SQL Server. Use Microsoft ODBC Driver 17 for SQL Server para crear aplicaciones, o bien para mejorar las existentes que deban aprovechar las ventajas de las características más recientes de SQL Server.

## <a name="download-for-windows"></a>Descargar para Windows

El instalador redistribuible de Microsoft ODBC Driver 17 for SQL Server instala los componentes de cliente, que son necesarios durante el tiempo de ejecución para aprovechar las características más recientes de SQL Server. Opcionalmente, instala los archivos de encabezado necesarios para desarrollar una aplicación que use la API ODBC. A partir de la versión 17.4.2, el instalador también incluye e instala la Biblioteca de autenticación de Microsoft Active Directory (ADAL.dll).

La versión 17.7.1 es la versión de disponibilidad general (GA) más reciente. Si tiene instalada una versión anterior de Microsoft ODBC Driver 17 for SQL Server, al instalar la versión 17.7.1 se actualizará a esta última.

**[![Descargar](../../ssms/media/download-icon.png) Descarga de Microsoft ODBC Driver 17 for SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2153471)**  
**[![Descargar](../../ssms/media/download-icon.png) Descarga de Microsoft ODBC Driver 17 for SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2153469)**  

### <a name="version-information"></a>Información de la versión

- Número de versión: 17.7.1.1
- Fecha de publicación: 29 de enero de 2021

> [!Note]
> Si accede a esta página desde una versión de idioma que no es el inglés y quiere ver el contenido más actualizado, visite la [versión en inglés de EE. UU. del sitio](). Puede descargar distintos idiomas en el sitio de la versión en inglés de EE. UU. si selecciona [idiomas disponibles](#available-languages).

## <a name="available-languages"></a>Idiomas disponibles

Esta versión de Microsoft ODBC Driver for SQL Server se puede instalar en los idiomas siguientes:

Microsoft ODBC Driver 17.7.1 for SQL Server (x64):  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2153471&clcid=0x40a)

Microsoft ODBC Driver 17.7.1 for SQL Server (x86):  
[Chino (simplificado)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x804) | [Chino (tradicional)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x404) | [Inglés (Estados Unidos)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x409) | [Francés](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40c) | [Alemán](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x407) | [Italiano](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x410) | [Japonés](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x411) | [Coreano](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x412) | [Portugués (Brasil)](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x416) | [Ruso](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x419) | [Español](https://go.microsoft.com/fwlink/?linkid=2153469&clcid=0x40a)

### <a name="release-notes-for-windows"></a>Notas de la versión para Windows

Para obtener detalles sobre esta versión en Windows, vea las [notas de la versión de Windows](windows\release-notes-odbc-sql-server-windows.md).

### <a name="previous-releases-for-windows"></a>Versiones anteriores para Windows

Para descargar versiones anteriores para Windows, vea las [versiones anteriores de Microsoft ODBC Driver for SQL Server](windows\release-notes-odbc-sql-server-windows.md#previous-releases).

## <a name="download-for-linux-and-macos"></a>Descarga para Linux y macOS

Microsoft ODBC Driver for SQL Server se puede descargar e instalar mediante administradores de paquetes para Linux y macOS con las instrucciones de instalación pertinentes:  
[Instalación de ODBC para SQL Server (Linux)](linux-mac\installing-the-microsoft-odbc-driver-for-sql-server.md)  
[Instalación de ODBC para SQL Server (macOS)](linux-mac\install-microsoft-odbc-driver-sql-server-macos.md)  

Si tiene que descargar los paquetes para la instalación sin conexión, todas las versiones están disponibles a través de los vínculos siguientes.

> [!Note]
> Los paquetes denominados `msodbcsql17-*` son la versión más reciente. Los paquetes denominados `msodbcsql-*` son la versión 13 del controlador.

### <a name="alpine"></a>Alpine

- [Paquete 17.7.1.1 para Alpine](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.apk) ([firma PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.1.1-1_amd64.sig))
- [Paquete 17.6.1.1 para Alpine](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.apk) ([firma PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.6.1.1-1_amd64.sig))
- [Paquete 17.5.2.2 para Alpine](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.apk) ([firma PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.2-1_amd64.sig))
- [Paquete 17.5.2.1 para Alpine](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.apk) ([firma PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.2.1-1_amd64.sig))
- [Paquete 17.5.1.1 para Alpine](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.apk) ([firma PGP](https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.5.1.1-1_amd64.sig))

### <a name="debian"></a>Debian

- [Paquetes de Debian 10 .deb](https://packages.microsoft.com/debian/10/prod/pool/main/m/msodbcsql17/)
- [Paquetes de Debian 9 .deb](https://packages.microsoft.com/debian/9/prod/pool/main/m/msodbcsql17/)
- [Paquetes de Debian 8 .deb](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql17/)
- [Paquetes .deb de Debian 8 (msodbcsql 13.x)](https://packages.microsoft.com/debian/8/prod/pool/main/m/msodbcsql/)

### <a name="redhat"></a>RedHat

- [Paquetes .rpm de RedHat 8](https://packages.microsoft.com/rhel/8/prod/)
- [Paquetes .rpm de RedHat 7](https://packages.microsoft.com/rhel/7/prod/)
- [Paquetes .rpm de RedHat 6](https://packages.microsoft.com/rhel/6/prod/)

### <a name="suse"></a>Suse

- [Paquetes .rpm de SuSE 15](https://packages.microsoft.com/sles/15/prod/)
- [Paquetes .rpm de SuSE 12](https://packages.microsoft.com/sles/12/prod/)
- [Paquetes .rpm de SuSE 11](https://packages.microsoft.com/sles/11/prod/)

### <a name="ubuntu"></a>Ubuntu

- [Paquetes .deb de Ubuntu 20.10](https://packages.microsoft.com/ubuntu/20.10/prod/pool/main/m/msodbcsql17/)
- [Paquetes .deb de Ubuntu 20.04](https://packages.microsoft.com/ubuntu/20.04/prod/pool/main/m/msodbcsql17/)
- [Paquetes de Ubuntu 18.04 .deb](https://packages.microsoft.com/ubuntu/18.04/prod/pool/main/m/msodbcsql17/)
- [Paquetes de Ubuntu 16.04 .deb](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql17/)
- [Paquetes de Ubuntu 14.04 .deb](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql17/)
- [Paquetes .deb de Ubuntu 16.04 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/)
- [Paquetes .deb de Ubuntu 14.04 (msodbcsql 13.x)](https://packages.microsoft.com/ubuntu/14.04/prod/pool/main/m/msodbcsql/)

Vea también [Instalación del controlador de Linux](linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

### <a name="macos"></a>macOS

- Para obtener más información, vea [Fórmulas de Homebrew](https://github.com/Microsoft/homebrew-mssql-release).

Vea también [Instalación del controlador para macOS](linux-mac/install-microsoft-odbc-driver-sql-server-macos.md).

### <a name="older-linux-releases"></a>Versiones anteriores de Linux

- **Red Hat Enterprise Linux 5 y 6 (64 bits)**  - [Descargar Microsoft ODBC Driver 11 for SQL Server - Red Hat Linux](https://go.microsoft.com/fwlink/?LinkId=267321)  
- **SUSE Linux Enterprise 11 Service Pack 2 (64 bits)**  - [Descargar Microsoft ODBC Driver 11 Preview for SQL Server - SUSE Linux](https://go.microsoft.com/fwlink/?LinkId=264916)

### <a name="release-notes-for-linux-and-macos"></a>Notas de la versión para Linux y macOS

Para más información sobre las versiones de Linux y macOS, vea [las notas de la versión de Linux y macOS](linux-mac\release-notes-odbc-sql-server-linux-mac.md).
