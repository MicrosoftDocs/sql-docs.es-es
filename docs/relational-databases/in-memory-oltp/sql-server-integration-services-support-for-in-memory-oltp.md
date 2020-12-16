---
title: Compatibilidad de SSIS con OLTP en memoria
description: Obtenga información sobre el uso de procedimientos almacenados compilados de forma nativa como componentes de origen y destino en un paquete de SQL Server Integration Services.
ms.custom: seo-dt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: ea82a9b9-e9ed-4d6f-b3fd-917f6c687ae3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3f638de7d98cd8e322174172ce918152cbe4623a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485197"
---
# <a name="sql-server-integration-services-support-for-in-memory-oltp"></a>Compatibilidad de SQL Server Integration Services con OLTP en memoria
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  Puede usar una tabla optimizada para memoria, una vista que haga referencia a tablas optimizadas para memoria o un procedimiento almacenado compilado de forma nativa como el origen o el destino del paquete de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] (SSIS). Puede usar [ADO NET Source](../../integration-services/data-flow/ado-net-source.md), [OLE DB Source](../../integration-services/data-flow/ole-db-source.md)o [ODBC Source](../../integration-services/data-flow/odbc-source.md) en el flujo de datos de un paquete SSIS y configurar el componente de origen para recuperar datos de una tabla optimizada para memoria o de una vista, o especificar una instrucción SQL para ejecutar un procedimiento almacenado compilado de forma nativa. Del mismo modo, puede usar [ADO NET Destination](../../integration-services/data-flow/ado-net-destination.md), [OLE DB Destination](../../integration-services/data-flow/ole-db-destination.md)o [ODBC Destination](../../integration-services/data-flow/odbc-destination.md) para cargar datos en una tabla optimizada para memoria o en una vista, o especificar una instrucción SQL para ejecutar un procedimiento almacenado compilado de forma nativa.  
  
 Puede configurar los componentes de origen y de destino mencionados anteriormente en un paquete SSIS para leer y escribir en las tablas optimizadas para memoria y en las vistas de la misma manera que en cualquier otra tabla o vista de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Sin embargo, cuando utilice procedimientos almacenados compilados de forma nativa, deberá tener en cuenta los puntos importantes que se mencionan en la siguiente sección.  
  
## <a name="invoking-a-natively-compiled-stored-procedure-from-an-ssis-package"></a>Invocar un procedimiento almacenado compilado de forma nativa desde un paquete SSIS  
 Para invocar un procedimiento almacenado compilado de forma nativa desde un paquete SSIS, es recomendable usar un origen ODBC o un destino ODBC con una instrucción SQL con el formato **\<procedure name>** sin la palabra clave **EXEC**. Si utiliza la palabra clave EXEC en la instrucción SQL, verá un mensaje de error porque el administrador de conexiones ODBC interpreta el texto del comando SQL como una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] en lugar de un procedimiento almacenado, y utiliza cursores, que no están admitidos para la ejecución de procedimientos almacenados compilados de forma nativa. El administrador de conexiones trata la instrucción SQL sin la palabra clave EXEC como una llamada a un procedimiento almacenado y no utiliza ningún cursor.  
  
 También puede utilizar el origen de ADO .NET y el origen de OLE DB para invocar un procedimiento almacenado compilado de forma nativa, pero es recomendable utilizar el origen ODBC. Si configura el origen de ADO .NET para ejecutar un procedimiento almacenado compilado de forma nativa, verá un mensaje de error porque el proveedor de datos para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (SqlClient), que el origen de ADO .NET utiliza de forma predeterminada, no admite la ejecución de procedimientos almacenados compilados de forma nativa. Puede configurar el origen de ADO .NET para que utilice el proveedor de datos ODBC, el proveedor OLE DB para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]o [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Sin embargo, tenga en cuenta que el origen ODBC se comporta mejor que el origen de ADO .NET con el proveedor de datos ODBC.  
  
## <a name="see-also"></a>Consulte también  
 [Compatibilidad de SQL Server con OLTP en memoria](./transact-sql-support-for-in-memory-oltp.md)  
  
