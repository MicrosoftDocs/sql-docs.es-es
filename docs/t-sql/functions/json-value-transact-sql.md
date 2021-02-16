---
description: JSON_VALUE (Transact-SQL)
title: JSON_VALUE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- JSON_VALUE
- JSON_VALUE_TSQL
helpviewer_keywords:
- JSON_VALUE function
- JSON, extracting
- JSON, querying
ms.assetid: cd016e14-11eb-4eaf-bf05-c7cfcc820a10
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: a3187c24ef4e17ccf4edd0f2d937a0fb8d1aa79f
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100337997"
---
# <a name="json_value-transact-sql"></a>JSON_VALUE (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

 Extrae un valor escalar de una cadena JSON.  
  
 Para extraer un objeto o una matriz de una cadena JSON en lugar de un valor escalar, vea [JSON_QUERY &#40;Transact-SQL&#41;](../../t-sql/functions/json-query-transact-sql.md). Para información sobre las diferencias entre **JSON_VALUE** y **JSON_QUERY**, vea [Comparación de JSON_VALUE y JSON_QUERY](../../relational-databases/json/validate-query-and-change-json-data-with-built-in-functions-sql-server.md#JSONCompare).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
JSON_VALUE ( expression , path )  
```  
  
## <a name="arguments"></a>Argumentos

 *expression*  
 Expresión. Suele ser el nombre de una variable o una columna con texto JSON.  

 Si **JSON_VALUE** detecta JSON que no es válido en *expression* antes de encontrar el valor identificado por *path*, la función devuelve un error. Si **JSON_VALUE** no encuentra el valor identificado por *path*, examina todo el texto y devuelve un error si detecta un JSON que no sea válido en cualquier lugar de *expression*.
  
 *path*  
 Ruta de acceso JSON que especifica la propiedad que se va a extraer. Para más información, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  

En [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y en [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], puede proporcionar una variable como el valor de *path*.
  
 Si el formato de *path* no es válido, **JSON_VALUE** devuelve un error.  
  
## <a name="return-value"></a>Valor devuelto

 Devuelve un solo valor de texto de tipo nvarchar (4000). La intercalación del valor devuelto es la misma que la intercalación de la expresión de entrada.  
  
 Si el valor es mayor que 4000 caracteres:  
  
- En el modo lax, **JSON_VALUE** devuelve NULL.  
  
- En el modo strict, **JSON_VALUE** devuelve un error.  
  
 Si tiene que devolver valores escalares mayores que 4000 caracteres, use **OPENJSON** en lugar de **JSON_VALUE**. Para obtener más información, vea [OPENJSON &#40;Transact-SQL&#41;](../../t-sql/functions/openjson-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones

### <a name="lax-mode-and-strict-mode"></a>Modo lax y modo strict

 Observe el siguiente texto JSON:  
  
```json  
DECLARE @jsonInfo NVARCHAR(MAX)

SET @jsonInfo=N'{  
     "info":{    
       "type":1,  
       "address":{    
         "town":"Bristol",  
         "county":"Avon",  
         "country":"England"  
       },  
       "tags":["Sport", "Water polo"]  
    },  
    "type":"Basic"  
 }'  
```  
  
 En la tabla siguiente se compara el comportamiento de **JSON_VALUE** en modo lax y modo strict. Para más información sobre la especificación del modo de ruta de acceso opcional (lax o strict), vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Path|Valor devuelto en el modo lax|Valor devuelto en el modo strict|Más información|  
|----------|------------------------------|---------------------------------|---------------|  
|$|NULL|Error|No es un valor escalar.<br /><br /> Use **JSON_QUERY** en su lugar.|  
|$.info.type|N'1'|N'1'|N/a|  
|$.info.address.town|N'Bristol'|N'Bristol'|N/a|  
|$.info."address"|NULL|Error|No es un valor escalar.<br /><br /> Use **JSON_QUERY** en su lugar.|  
|$.info.tags|NULL|Error|No es un valor escalar.<br /><br /> Use **JSON_QUERY** en su lugar.|  
|$.info.type[0]|NULL|Error|No es una matriz.|  
|$.info.none|NULL|Error|La propiedad no existe.|  
| &nbsp; | &nbsp; | &nbsp; | &nbsp; |
  
## <a name="examples"></a>Ejemplos  
  
### <a name="example-1"></a>Ejemplo 1
 En el ejemplo siguiente se usan los valores de las propiedades JSON `town` y `state` en los resultados de la consulta. Puesto que **JSON_VALUE** conserva la intercalación del origen, el criterio de ordenación de los resultados depende de la intercalación de la columna `jsonInfo`. 

> [!NOTE]
> (En este ejemplo se da por supuesto que una tabla denominada `Person.Person` contiene una columna de texto JSON `jsonInfo` y que esta columna tiene la estructura que se ha mostrado anteriormente en el análisis del modo lax y el modo strict. En la base de datos de ejemplo de AdventureWorks, la tabla `Person` realmente no contiene una columna `jsonInfo`).
  
```sql  
SELECT FirstName, LastName,
 JSON_VALUE(jsonInfo,'$.info.address[0].town') AS Town
FROM Person.Person
WHERE JSON_VALUE(jsonInfo,'$.info.address[0].state') LIKE 'US%'
ORDER BY JSON_VALUE(jsonInfo,'$.info.address[0].town')
```  
  
### <a name="example-2"></a>Ejemplo 2
 En el ejemplo siguiente se extrae el valor de la propiedad JSON `town` en una variable local.  
  
```sql
DECLARE @jsonInfo NVARCHAR(MAX)
DECLARE @town NVARCHAR(32)

SET @jsonInfo=N'{"info":{"address":[{"town":"Paris"},{"town":"London"}]}}';

SET @town=JSON_VALUE(@jsonInfo,'$.info.address[0].town'); -- Paris
SET @town=JSON_VALUE(@jsonInfo,'$.info.address[1].town'); -- London
```  
  
### <a name="example-3"></a>Ejemplo 3
 En el ejemplo siguiente se crean columnas calculadas en función de los valores de propiedades JSON.  
  
```sql  
CREATE TABLE dbo.Store
 (
  StoreID INT IDENTITY(1,1) NOT NULL,
  Address VARCHAR(500),
  jsonContent NVARCHAR(4000),
  Longitude AS JSON_VALUE(jsonContent, '$.address[0].longitude'),
  Latitude AS JSON_VALUE(jsonContent, '$.address[0].latitude')
 )
```  
  
## <a name="see-also"></a>Vea también
 [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
 [Datos JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
