---
title: Controlador ODBC de Microsoft para SQL Server
description: Microsoft ODBC Driver for SQL Server proporciona conectividad a SQL Server y Azure SQL Database a través de API de ODBC estándar.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 9f2ae91b-06af-4c9a-9d24-062df7bc4662
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b107155d9672b2a1d7b905a387e48373905dab55
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464196"
---
# <a name="microsoft-odbc-driver-for-sql-server"></a>Controlador ODBC de Microsoft para SQL Server

[!INCLUDE[ODBC_Current_Version](../../includes/odbc-latest-release.md)]

[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

ODBC es la API nativa y principal de acceso a datos de las aplicaciones escritas en C y C++ para SQL Server. Hay disponible un controlador ODBC para la mayoría de los orígenes de datos. Otros lenguajes que pueden usar ODBC incluyen COBOL, Perl, PHP y Python. ODBC se usa ampliamente en escenarios de integración de datos.

El controlador ODBC incluye herramientas como [**sqlcmd**](../../tools/sqlcmd-utility.md) y [**bcp**](../../tools/bcp-utility.md). La utilidad **sqlcmd** permite ejecutar instrucciones Transact-SQL, procedimientos del sistema y scripts SQL. La utilidad **bcp** copia datos de forma masiva entre una instancia de Microsoft SQL Server y un archivo de datos en el formato que especifique el usuario. Puede usar **bcp** para importar muchas filas nuevas en tablas de SQL Server o exportar datos de tablas a archivos de datos.  

## <a name="code-example-in-c"></a>Ejemplo de código en C++

El siguiente ejemplo de C++ muestra cómo usar las API de ODBC para conectarse y acceder a una base de datos:

- [Ejemplo de código de C++, con ODBC](../../odbc/reference/sample-odbc-program.md)

## <a name="download"></a>Descargar

- ![Download-DownArrow-Circled](../../ssms/media/download-icon.png)[Para descargar un controlador ODBC](download-odbc-driver-for-sql-server.md)

## <a name="documentation"></a>Documentación

### <a name="features"></a>Características

- [Proveedores de almacén de claves personalizados](../../connect/odbc/custom-keystore-providers.md)
- [Clasificación de datos](../../connect/odbc/data-classification.md)
- [Atributos y palabras clave de cadena de conexión y DSN](dsn-connection-string-attribute.md)
- [SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md) (las características disponibles también se aplican, sin OLEDB, al controlador ODBC para SQL Server)
- [Uso de Always Encrypted](../../connect/odbc/using-always-encrypted-with-the-odbc-driver.md)
- [Uso de Azure Active Directory](../../connect/odbc/using-azure-active-directory.md)
- [Uso de resolución de IP de red transparente](../../connect/odbc/using-transparent-network-ip-resolution.md)
- [Uso de las transacciones XA](../../connect/odbc/use-xa-with-dtc.md)

### <a name="linux-and-macos"></a>Linux y macOS

- [Instalación del controlador en Linux](../../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md)
- [Instalación del controlador en macOS](../../connect/odbc/linux-mac/install-microsoft-odbc-driver-sql-server-macos.md)
- [Conexión a SQL Server](../../connect/odbc/linux-mac/connection-string-keywords-and-data-source-names-dsns.md)
- [Conexión con **bcp**](../../connect/odbc/linux-mac/connecting-with-bcp.md)
- [Conexión con **sqlcmd**](../../connect/odbc/linux-mac/connecting-with-sqlcmd.md)
- [Seguimiento de acceso a datos](../../connect/odbc/linux-mac/data-access-tracing-with-the-odbc-driver-on-linux.md)
- [Preguntas más frecuentes](../../connect/odbc/linux-mac/frequently-asked-questions-faq-for-odbc-linux.yml)
- [Instalación del Administrador de controladores](../../connect/odbc/linux-mac/installing-the-driver-manager.md)
- [Problemas conocidos](../../connect/odbc/linux-mac/known-issues-in-this-version-of-the-driver.md)
- [Instrucciones de programación](../../connect/odbc/linux-mac/programming-guidelines.md)
- [Notas de la versión](../../connect/odbc/linux-mac/release-notes-odbc-sql-server-linux-mac.md)
- [Notas de la versión (mssql-tools)](../../connect/odbc/linux-mac/release-notes-tools.md)
- [Soporte para alta disponibilidad y recuperación ante desastres](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
- [Uso de la autenticación integrada (Kerberos)](../../connect/odbc/linux-mac/using-integrated-authentication.md)

### <a name="windows"></a>Windows

- [Ejemplo de ejecución asincrónica (método de notificación)](../../connect/odbc/windows/asynchronous-execution-notification-method-sample.md)
- [Resistencia de conexión en el controlador Windows ODBC](../../connect/odbc/windows/connection-resiliency-in-the-windows-odbc-driver.md)
- [Agrupación de conexiones dependientes del controlador](../../connect/odbc/windows/driver-aware-connection-pooling-in-the-odbc-driver-for-sql-server.md)
- [Características y cambios de comportamiento](../../connect/odbc/windows/features-of-the-microsoft-odbc-driver-for-sql-server-on-windows.md)
- [Notas de la versión de ODBC para SQL Server en Windows](windows/release-notes-odbc-sql-server-windows.md)
- [Requisitos del sistema, instalación y archivos del controlador](../../connect/odbc/windows/system-requirements-installation-and-driver-files.md)

## <a name="community"></a>Comunidad

- [Blog de controladores de SQL Server](https://techcommunity.microsoft.com/t5/SQL-Server/bg-p/SQLServer/label-name/SQLServerDrivers)  
- [Foro para el acceso a los datos de SQL Server](https://social.technet.microsoft.com/Forums/en/sqldataaccess/threads)  
