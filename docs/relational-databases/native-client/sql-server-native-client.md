---
title: Información
description: Obtenga información sobre las características de SQL Server Native Client (SNAC). SQL Server Native Client hace referencia a los controladores ODBC y OLE DB para SQL Server.
ms.date: 04/14/2017
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: native-client
ms.topic: conceptual
ms.assetid: e4d4fe39-0090-42a7-8405-6378370d11cb
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b65b009d5dc88dc9d5a0cd5cc6f5592c8160c8bb
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467506"
---
# <a name="sql-server-native-client"></a>SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SNAC, o SQL Server Native Client, es un término que se ha usado indistintamente para hacer referencia a los controladores ODBC y OLE DB para SQL Server.

> [!IMPORTANT] 
> El SQL Server Native Client (SQLNCLI) sigue en desuso y no se recomienda usarlo para los nuevos trabajos de desarrollo. En su lugar, use el nuevo [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL), que se actualizará con las características de servidor más recientes.

> [!NOTE]
> Para obtener más información y descargar los controladores de SNAC o ODBC, vea la [entrada de blog explicación del ciclo de vida de snac](/archive/blogs/sqlreleaseservices/snac-lifecycle-explained).
> Para obtener más información sobre el controlador ODBC para SQL Server, vea [Microsoft ODBC driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md).  

 Información sobre las [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] características de Native Client publicadas con [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , la última versión disponible de SQL Server Native Client:

-   [Compatibilidad de SQL Server Native Client con LocalDB](../../relational-databases/native-client/features/sql-server-native-client-support-for-localdb.md)  

-   [Detección de metadatos](../../relational-databases/native-client/features/metadata-discovery.md)  

-   [Compatibilidad con UTF-16 en SQL Server Native Client 11.0](../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)  

-   [Compatibilidad de SQL Server Native Client para la alta disponibilidad con recuperación de desastres](../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)  

-   [Obtener acceso a información de diagnóstico en el registro de eventos extendidos](../../relational-databases/native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)  

ODBC en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client admite tres características que se agregaron a ODBC estándar en el SDK de Windows 7:  

-   Ejecución asincrónica en operaciones relacionadas con conexión. Para obtener más información, vea el tema que trata sobre [ejecución asincrónica](../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).  

-   Extensibilidad del tipo de datos C. Para obtener más información, vea el tema sobre [tipos de datos C en ODBC](../../odbc/reference/develop-app/c-data-types-in-odbc.md).  

     Para admitir esta característica en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, SQLGetDescField puede devolver **SQL_C_SS_TIME2** (para tipos de **hora** ) o **SQL_C_SS_TIMESTAMPOFFSET** (para **DateTimeOffset**) en lugar de **SQL_C_BINARY**, si la aplicación usa ODBC 3,8. Para obtener más información, vea [compatibilidad de tipos de datos con las mejoras de fecha y hora de ODBC](../../relational-databases/native-client-odbc-date-time/data-type-support-for-odbc-date-and-time-improvements.md).  

-   Llamada varias veces a **SQLGetData** con un búfer pequeño para recuperar un valor de parámetro grande. Para obtener más información, vea el tema que trata sobre [recuperar parámetros de salida mediante SQLGetData](../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  

 En los siguientes temas se describen los cambios de comportamiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  

-   Al llamar a **ICommandWithParameters::SetParameterInfo**, el valor pasado al parámetro *pwszName* debe ser un identificador válido. Para obtener más información, vea [ICommandWithParameters](../../relational-databases/native-client-ole-db-interfaces/icommandwithparameters.md).  

-   **SQLDescribeParam** devolverá de forma coherente un valor compatible con la especificación ODBC. Para obtener más información, vea [SQLDescribeParam](../../relational-databases/native-client-odbc-api/sqldescribeparam.md).  

-   [Cambio de comportamiento del controlador ODBC al administrar las conversiones de caracteres](../../relational-databases/native-client/features/odbc-driver-behavior-change-when-handling-character-conversions.md)  

## <a name="see-also"></a>Vea también  
[Instalar SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
 [Características de SQL Server Native Client](../../relational-databases/native-client/features/sql-server-native-client-features.md)