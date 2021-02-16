---
description: JSON_QUERY (Transact-SQL)
title: JSON_QUERY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JSON_QUERY
- JSON_QUERY_TSQL
helpviewer_keywords:
- JSON, extracting
- JSON, querying
- JSON_QUERY function
ms.assetid: 1ab0d90f-19b6-4988-ab4f-22fdf28b7c79
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: f5f81325f27a37f1e6d30ea7c8fb6bdb5dad096b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100337983"
---
# <a name="json_query-transact-sql"></a>JSON_QUERY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Extrae un objeto o una matriz desde una cadena JSON.  
  
 Para extraer un valor escalar de una cadena JSON en lugar de un objeto o una matriz, vea [JSON_VALUE &#40;Transact-SQL&#41;](../../t-sql/functions/json-value-transact-sql.md). Para información sobre las diferencias entre **JSON_VALUE** y **JSON_QUERY**, vea [Comparación de JSON_VALUE y JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
JSON_QUERY ( expression [ , path ] )  
```  
  
## <a name="arguments"></a>Argumentos

 *expression*  
 Expresión. Suele ser el nombre de una variable o una columna con texto JSON.  
  
 Si **JSON_QUERY** detecta JSON que no es válido en *expresión* antes de encontrar el valor identificado por *path*, la función devuelve un error. Si **JSON_QUERY** no encuentra el valor identificado por *path*, examina todo el texto y devuelve un error si detecta JSON que no es válido en algún lugar de *expresión*.  
  
 *path*  
 Ruta de acceso JSON que especifica el objeto o la matriz que se va a extraer.

En [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y en [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], puede proporcionar una variable como el valor de *path*.

La ruta de acceso JSON puede especificar el modo lax o strict para el análisis. Si no se especifica el modo de análisis, el modo lax es el valor predeterminado. Para más información, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  

El valor predeterminado para *path* es "$". Como resultado, si no se proporciona un valor para *path*, **JSON_QUERY** devuelve la *expresión* de entrada.

Si el formato de *path* no es válido, **JSON_QUERY** devuelve un error.  
  
## <a name="return-value"></a>Valor devuelto

 Devuelve un fragmento de JSON de tipo nvarchar(max). La intercalación del valor devuelto es la misma que la intercalación de la expresión de entrada.  
  
 Si el valor no es un objeto o una matriz:  
  
- En el modo lax, **JSON_QUERY** devuelve NULL.  
  
- En el modo strict, **JSON_QUERY** devuelve un error.  
  
## <a name="remarks"></a>Observaciones  

### <a name="lax-mode-and-strict-mode"></a>Modo lax y modo strict

 Observe el siguiente texto JSON:  
  
```json  
{
   "info": {
      "type": 1,
      "address": {
         "town": "Bristol",
         "county": "Avon",
         "country": "England"
      },
      "tags": ["Sport", "Water polo"]
   },
   "type": "Basic"
} 
```  
  
 En la tabla siguiente se compara el comportamiento de **JSON_QUERY** en modo lax y modo strict. Para más información sobre la especificación del modo de ruta de acceso opcional (lax o strict), vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Path|Valor devuelto en el modo lax|Valor devuelto en el modo strict|Más información|  
|----------|------------------------------|---------------------------------|---------------|  
|$|Devuelve todo el texto JSON.|Devuelve todo el texto JSON.|N/a|  
|$.info.type|NULL|Error|No es un objeto o una matriz.<br /><br /> Use **JSON_VALUE** en su lugar.|  
|$.info.address.town|NULL|Error|No es un objeto o una matriz.<br /><br /> Use **JSON_VALUE** en su lugar.|  
|$.info."address"|N"{ "town":"Bristol", "county":"Avon", "country":"England" }"|N"{ "town":"Bristol", "county":"Avon", "country":"England" }"|N/a|  
|$.info.tags|N"[ "Sport", "Water polo"]"|N"[ "Sport", "Water polo"]"|N/a|  
|$.info.type[0]|NULL|Error|No es una matriz.|  
|$.info.none|NULL|Error|La propiedad no existe.|  

### <a name="using-json_query-with-for-json"></a>Uso de JSON_QUERY con FOR JSON

**JSON_QUERY** devuelve un fragmento de JSON válido. Como resultado, **FOR JSON** no incluye entre caracteres de escape los caracteres especiales del valor devuelto de **JSON_QUERY**.

Si se van a devolver resultados con FOR JSON y se van a incluir datos que ya están en formato JSON (en una columna o como resultado de una expresión), encapsule los datos JSON con **JSON_QUERY** sin el parámetro *path*.

## <a name="examples"></a>Ejemplos  
  
### <a name="example-1"></a>Ejemplo 1

 En el ejemplo siguiente se muestra cómo devolver un fragmento de JSON de una columna `CustomFields` en los resultados de la consulta.  
  
```sql  
SELECT PersonID,FullName,
  JSON_QUERY(CustomFields,'$.OtherLanguages') AS Languages
FROM Application.People
```  
  
### <a name="example-2"></a>Ejemplo 2

En el ejemplo siguiente se muestra cómo incluir fragmentos de JSON en la salida de la cláusula FOR JSON.  
  
```sql  
SELECT StockItemID, StockItemName,
         JSON_QUERY(Tags) as Tags,
         JSON_QUERY(CONCAT('["',ValidFrom,'","',ValidTo,'"]')) ValidityPeriod
FROM Warehouse.StockItems
FOR JSON PATH
```  
  
## <a name="see-also"></a>Consulte también

- [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Datos JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
