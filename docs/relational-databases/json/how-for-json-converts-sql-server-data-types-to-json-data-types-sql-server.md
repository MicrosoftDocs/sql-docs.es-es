---
description: Conversión por parte de FOR JSON de tipos de datos de SQL Server en tipos de datos JSON (SQL Server)
title: Conversión por parte de FOR JSON de tipos de datos de SQL Server en tipos de datos JSON
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, data type conversion
ms.assetid: da356f06-efd0-4ea3-8157-77395bf790d7
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ff06a77af1592bf9bf2386742a53033ade76aecd
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478126"
---
# <a name="how-for-json-converts-sql-server-data-types-to-json-data-types-sql-server"></a>Conversión por parte de FOR JSON de tipos de datos de SQL Server en tipos de datos JSON (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  La cláusula **FOR JSON** usa las siguientes reglas para convertir tipos de datos SQL Server en tipos JSON en la salida JSON.  
  
|Category|Tipos de datos de SQL Server|Tipo de datos JSON|  
|--------------|--------------|---------------|  
|Tipos de carácter y cadena|char, nchar, varchar, nvarchar|string|  
|Tipos numéricos|int, bigint, float, decimal, numeric|number|  
|Tipo de bit|bit|Booleano (true o false)|  
|Tipos de fecha y hora|date, datetime, datetime2, time, datetimeoffset|string|  
|Tipos binarios|varbinary, binary, image, timestamp, rowversion|Cadena codificada en BASE64|  
|Tipos CLR|geometry, geography, otros tipos CLR|No compatible. Estos tipos devuelven un error.<br /><br /> En la instrucción SELECT, use CAST o CONVERT, o bien use un método o propiedad CLR, para convertir los datos de origen en un tipo de datos SQL Server que pueda convertirse correctamente a un tipo JSON. Por ejemplo, use **STAsText()** para el tipo geometry o **ToString()** para cualquier tipo CLR. El tipo del valor de salida JSON se deriva del tipo de valor devuelto de la conversión aplicada en la instrucción SELECT.|  
|Otros tipos|uniqueidentifier, money|string|  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
  
## <a name="see-also"></a>Consulte también  
 [Format Query Results as JSON with FOR JSON &#40;SQL Server&#41; (Dar formato JSON a los resultados de consulta con FOR JSON &#40;SQL Server&#41;)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
