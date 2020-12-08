---
description: Cómo FOR JSON inserta caracteres de escape en los caracteres especiales y caracteres de control (SQL Server)
title: Cómo FOR JSON inserta caracteres de escape en los caracteres especiales y caracteres de control
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, special characters
ms.assetid: 4ba90025-5a09-4f0a-836a-54c886324530
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d7a99590fc36d246f19dbee3ebc227cfa10052d2
ms.sourcegitcommit: 28fecbf61ae7b53405ca378e2f5f90badb1a296a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2020
ms.locfileid: "96595196"
---
# <a name="how-for-json-escapes-special-characters-and-control-characters-sql-server"></a>Cómo FOR JSON inserta caracteres de escape en los caracteres especiales y caracteres de control (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sqlserver2016-asdb.md)]

  En este tema se describe cómo la cláusula **FOR JSON** de una instrucción **SELECT** de SQL Server inserta caracteres de escape en caracteres especiales y representa caracteres de control en la salida JSON.  

> [!IMPORTANT]
> En esta página se describe la compatibilidad integrada con JSON en Microsoft SQL Server. Para obtener información general sobre la inserción de caracteres de escape y la codificación en JSON, vea la sección 2.5 de JSON RFC: [https://www.ietf.org/rfc/rfc4627.txt](https://www.ietf.org/rfc/rfc4627.txt).

## <a name="escaping-of-special-characters"></a>Secuencia de caracteres de escape en los caracteres especiales  
Si los datos de origen contienen caracteres especiales, la cláusula **FOR JSON** inserta caracteres de escape en ellos en la salida de JSON con `\`, tal y como se muestra en la tabla siguiente. Esta secuencia de escape se produce tanto en los nombres de propiedades como en sus valores.  
  
|**Carácter especial**|**Salida con escape**|  
|---------------------------|--------------------------|  
|Comillas (")|\\"|  
|Barra diagonal inversa (\\)|\\\\|  
|Barra diagonal (/)|\\/|  
|Retroceso|\b|  
|Avance de página|\f|  
|Nueva línea|\n|  
|Retorno de carro|\r|  
|Tabulación horizontal|\t|  
  
## <a name="control-characters"></a>Caracteres de control  
Si los datos de origen contienen caracteres de control, la cláusula **FOR JSON** los codifica en la salida de JSON en formato `\u<code>`, tal y como se muestra en la tabla siguiente.  
  
|**Carácter de control**|**Salida codificada**|  
|---------------------------|--------------------------|  
|CHAR(0)|\u0000|  
|CHAR(1)|\u0001|  
|…|...|  
|CHAR(31)|\u001f|  
  
## <a name="example"></a>Ejemplo  
 Se trata de un ejemplo de la salida de **FOR JSON** en datos de origen que incluye caracteres especiales y caracteres de control.  
  
 Consulta:  
  
```sql  
SELECT  
  'VALUE\    /  
  "' as [KEY\/"],  
  CHAR(0) as '0',  
  CHAR(1) as '1',  
  CHAR(31) as '31'  
FOR JSON PATH  
```  
  
 Resultado:  
  
```json  
{
    "KEY\\\t\/\"": "VALUE\\\t\/\r\n\"",
    "0": "\u0000",
    "1": "\u0001",
    "31": "\u001f"
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>Más información sobre JSON en SQL Server y Azure SQL Database  
  
### <a name="microsoft-videos"></a>Vídeos de Microsoft

Para obtener una introducción visual a la compatibilidad integrada de JSON en SQL Server y Azure SQL Database, vea los siguientes vídeos:

-   [SQL Server 2016 and JSON Support](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support) (SQL Server 2016 y compatibilidad con JSON)

-   [Using JSON in SQL Server 2016 and Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database) (Uso de JSON en SQL Server 2016 y Azure SQL Database)

-   [JSON as a bridge between NoSQL and relational worlds](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds) (JSON como puente entre los universos NoSQL y relacional)
  
## <a name="see-also"></a>Consulte también  
 [Format Query Results as JSON with FOR JSON &#40;SQL Server&#41; (Dar formato JSON a los resultados de consulta con FOR JSON &#40;SQL Server&#41;)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
[Cláusula FOR](../../t-sql/queries/select-for-clause-transact-sql.md)
