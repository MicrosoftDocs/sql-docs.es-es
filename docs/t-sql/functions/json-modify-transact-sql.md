---
description: JSON_MODIFY (Transact-SQL)
title: JSON_MODIFY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
ms.assetid: 96bc8255-a037-4907-aec4-1a9c30814651
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 121c84fb00abaaec2466813fe161f928713200fd
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100338002"
---
# <a name="json_modify-transact-sql"></a>JSON_MODIFY (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Actualiza el valor de una propiedad en una cadena JSON y devuelve la cadena JSON actualizada.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
JSON_MODIFY ( expression , path , newValue )  
```  
  
## <a name="arguments"></a>Argumentos

 *expression*  
 Expresión. Suele ser el nombre de una variable o una columna con texto JSON.  
  
 **JSON_MODIFY** devuelve un error si *expression* no contiene un valor JSON válido.  
  
 *path*  
 Expresión de ruta de acceso JSON que especifica la propiedad que se va a actualizar.

 *path* tiene la siguiente sintaxis:  
  
 `[append] [ lax | strict ] $.<json path>`  
  
- *append*  
    Modificador opcional que especifica que el valor nuevo se debe anexar a la matriz a la que hace referencia *\<json path>* .  
  
- *lax*  
    Especifica que la propiedad a la que hace referencia *\<json path>* no tiene que existir necesariamente. Si la propiedad no está presente, JSON_MODIFY intenta insertar el nuevo valor en la ruta de acceso especificada. Es posible que la inserción no se realice si la propiedad no se puede insertar en la ruta de acceso. Si no se especifica *lax* o *strict*, *lax* es el modo predeterminado.  
  
- *strict*  
    Especifica que la propiedad a la que hace referencia *\<json path>* debe estar en la expresión JSON. Si la propiedad no está presente, JSON_MODIFY devuelve un error.  
  
- *\<json path>*  
    Especifica la ruta de acceso de la propiedad que se va a actualizar. Para más información, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
En [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y en [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], puede proporcionar una variable como el valor de *path*.

**JSON_MODIFY** devuelve un error si el formato de *path* no es válido.  
  
 *newValue*  
 El nuevo valor de la propiedad especificada por *path*.  
 El nuevo valor debe ser de tipo [n]varchar o text.
  
 En el modo lax, JSON_MODIFY elimina la clave especificada si el nuevo valor es NULL.  
  
JSON_MODIFY convierte todos los caracteres especiales en el nuevo valor si el tipo del valor es NVARCHAR o VARCHAR. No se aplicará escape a un valor de texto si presenta un formato JSON correcto generado por FOR JSON, JSON_QUERY o JSON_MODIFY.  
  
## <a name="return-value"></a>Valor devuelto

 Devuelve el valor actualizado de *expression* como texto con formato JSON correcto.  
  
## <a name="remarks"></a>Observaciones

 La función JSON_MODIFY permite actualizar el valor de una propiedad existente, insertar un nuevo par clave-valor o eliminar una clave según una combinación de modos y valores proporcionados.  
  
 En la siguiente tabla se compara el comportamiento de **JSON_MODIFY** en modo lax y en modo strict. Para más información sobre la especificación del modo de ruta de acceso opcional (lax o strict), vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
|Valor existente|La ruta de acceso existe|Modo lax|Modo strict|  
|--------------------|-----------------|--------------|-----------------|  
|Not NULL|Sí|Se actualiza el valor existente.|Se actualiza el valor existente.|  
|Not NULL|No|Se intenta crear un par clave-valor en la ruta de acceso especificada.<br /><br /> Esto puede producir un error. Por ejemplo, si se especifica la ruta de acceso `$.user.setting.theme`, JSON_MODIFY no inserta la clave `theme` si los objetos `$.user` o `$.user.settings` no existen, o bien si la configuración es una matriz o un valor escalar.|Error: INVALID_PROPERTY|  
|NULL|Sí|Se elimina la propiedad existente.|El valor actual se establece en NULL.|  
|NULL|No|No sucede nada. El primer argumento se devuelve como resultado.|Error: INVALID_PROPERTY|  
  
 En el modo lax, JSON_MODIFY intenta crear un par clave-valor, pero en algunos casos se podría producir un error.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="example---basic-operations"></a>Ejemplo: operaciones básicas

 En el siguiente ejemplo se muestran las operaciones básicas que se pueden realizar con texto JSON.  
  
 **Consultar**
  
```sql

DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update name  

SET @info=JSON_MODIFY(@info,'$.name','Mike')

PRINT @info

-- Insert surname  

SET @info=JSON_MODIFY(@info,'$.surname','Smith')

PRINT @info

-- Delete name  

SET @info=JSON_MODIFY(@info,'$.name',NULL)

PRINT @info

-- Add skill  

SET @info=JSON_MODIFY(@info,'append $.skills','Azure')

PRINT @info
```  
  
 **Resultados**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL"],
    "surname": "Smith"
} {
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---multiple-updates"></a>Ejemplo: varias actualizaciones

 Con JSON_MODIFY solo se puede actualizar una propiedad. Si tiene que realizar varias actualizaciones, puede usar varias llamadas JSON_MODIFY.  
  
 **Consultar**
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Multiple updates  

SET @info=JSON_MODIFY(JSON_MODIFY(JSON_MODIFY(@info,'$.name','Mike'),'$.surname','Smith'),'append $.skills','Azure')

PRINT @info
```  
  
 **Resultados**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "Mike",
    "skills": ["C#", "SQL", "Azure"],
    "surname": "Smith"
}
```  
  
### <a name="example---rename-a-key"></a>Ejemplo: cambiar una clave de nombre  
 En el siguiente ejemplo se muestra cómo cambiar el nombre de una propiedad en texto JSON con la función JSON_MODIFY. En primer lugar, puede tomar el valor de una propiedad existente e insertarlo como un nuevo par clave-valor. Luego, puede eliminar la antigua clave estableciendo el valor de la propiedad anterior en NULL.  
  
 **Consultar**
  
```sql  
DECLARE @product NVARCHAR(100)='{"price":49.99}'

PRINT @product

-- Rename property  

SET @product=
 JSON_MODIFY(
  JSON_MODIFY(@product,'$.Price',CAST(JSON_VALUE(@product,'$.price') AS NUMERIC(4,2))),
  '$.price',
  NULL
 )

PRINT @product
```  
  
 **Resultados**
  
```json  
{
    "price": 49.99
} {
    "Price": 49.99
}
```  
  
 Si no convierte el nuevo valor a un tipo numérico, JSON_MODIFY lo considera como texto y lo coloca entre comillas dobles.  
  
### <a name="example---increment-a-value"></a>Ejemplo: aumentar un valor

 En el siguiente ejemplo se muestra cómo aumentar el valor de una propiedad en texto JSON con la función JSON_MODIFY. En primer lugar, puede tomar el valor de la propiedad existente e insertarlo como un nuevo par clave-valor. Luego, puede eliminar la antigua clave estableciendo el valor de la propiedad anterior en NULL.  
  
 **Consultar**
  
```sql  
DECLARE @stats NVARCHAR(100)='{"click_count": 173}'

PRINT @stats

-- Increment value  

SET @stats=JSON_MODIFY(@stats,'$.click_count',
 CAST(JSON_VALUE(@stats,'$.click_count') AS INT)+1)

PRINT @stats
```  
  
 **Resultados**
  
```json  
{
    "click_count": 173
} {
    "click_count": 174
}
```  
  
### <a name="example---modify-a-json-object"></a>Ejemplo: modificar un objeto JSON

 JSON_MODIFY trata el argumento *newValue* como texto sin formato incluso cuando contiene texto con formato JSON correcto. Como resultado, la salida JSON de la función se inserta entre comillas dobles y todos los caracteres especiales son caracteres de escape, tal y como se muestra en el siguiente ejemplo.  
  
 **Consultar**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array

SET @info=JSON_MODIFY(@info,'$.skills','["C#","T-SQL","Azure"]')

PRINT @info
```  
  
 **Resultados**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": "["C#","T-SQL","Azure"]"
}
```  
  
 Para evitar el escape automático, proporcione un *newValue* con la función JSON_QUERY. JSON_MODIFY sabe que el valor devuelto por JSON_MODIFY tiene un formato JSON correcto, por lo que aplica escape.  
  
 **Consultar**  
  
```sql
DECLARE @info NVARCHAR(100)='{"name":"John","skills":["C#","SQL"]}'

PRINT @info

-- Update skills array  

SET @info=JSON_MODIFY(@info,'$.skills',JSON_QUERY('["C#","T-SQL","Azure"]'))

PRINT @info
```  
  
 **Resultados**
  
```json  
{
    "name": "John",
    "skills": ["C#", "SQL"]
} {
    "name": "John",
    "skills": ["C#", "T-SQL", "Azure"]
}
```  
  
### <a name="example---update-a-json-column"></a>Ejemplo: actualizar una columna JSON

 En el siguiente ejemplo se actualiza el valor de una propiedad en una columna de tabla que contiene JSON.  
  
```sql  
UPDATE Employee
SET jsonCol=JSON_MODIFY(jsonCol,'$.info.address.town','London')
WHERE EmployeeID=17
```  
  
## <a name="see-also"></a>Consulte también

- [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Datos JSON &#40;SQL Server&#41;](../../relational-databases/json/json-data-sql-server.md)  
  
