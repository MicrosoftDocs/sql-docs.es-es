---
description: OPENJSON (Transact-SQL)
title: OPENJSON (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2020
ms.prod: sql
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPENJSON
- OPENJSON_TSQL
helpviewer_keywords:
- OPENJSON rowset function
- JSON, importing
- JSON, converting from
ms.assetid: 233d0877-046b-4dcc-b5da-adeb22f78531
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: chadam
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: b29315567520dff0d4700aeb476279bf6cfe24b7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100340641"
---
# <a name="openjson-transact-sql"></a>OPENJSON (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

**OPENJSON** es una función con valores de tabla que analiza el texto JSON y devuelve objetos y propiedades de la entrada JSON como filas y columnas. En otras palabras, **OPENJSON** proporciona una vista de conjunto de filas de un documento JSON. Las columnas del conjunto de filas y las rutas de acceso de propiedades JSON que se usan para rellenar las columnas se pueden especificar de forma explícita. Como **OPENJSON** devuelve un conjunto de filas, se puede usar **OPENJSON** en la cláusula `FROM` de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] al igual que se puede usar cualquier otra tabla, vista o función con valores de tabla.  
  
Use **OPENJSON** para importar datos JSON a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o convertir datos JSON a formato relacional para una aplicación o servicio que no pueda usar JSON directamente.  
  
> [!NOTE]  
>  La función **OPENJSON** solo está disponible en el nivel de compatibilidad 130 o superior. Si el nivel de compatibilidad de la base de datos es inferior a 130, SQL Server no podrá encontrar ni ejecutar la función **OPENJSON**. Hay otras funciones JSON que sí están disponibles en todos los niveles de compatibilidad.
> 
> Puede comprobar el nivel de compatibilidad en la vista `sys.databases` o en las propiedades de la base de datos. Se puede cambiar el nivel de compatibilidad de una base de datos mediante el comando siguiente:  
> 
> `ALTER DATABASE DatabaseName SET COMPATIBILITY_LEVEL = 130`
>
> El nivel de compatibilidad 120 puede ser el valor predeterminado incluso en una base de datos nueva de Azure SQL Database.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema")[Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
OPENJSON( jsonExpression [ , path ] )  [ <with_clause> ]

<with_clause> ::= WITH ( { colName type [ column_path ] [ AS JSON ] } [ ,...n ] )
```  

La función con valores de tabla **OPENJSON** analiza la *expresiónJSON* proporcionada como primer argumento y devuelve una o varias filas que contienen datos de los objetos JSON en la expresión. *expresiónJSON* puede contener objetos secundarios anidados. Si quiere analizar un subobjeto desde *expresiónJSON*, puede especificar un parámetro **path** para el subobjeto JSON.

### <a name="openjson"></a>openjson

![Sintaxis de TVF de OPENJSON](../../relational-databases/json/media/openjson-syntax.png "Sintaxis de OPENJSON")  

De forma predeterminada, la función con valores de tabla **OPENJSON** devuelve tres columnas, que contienen el nombre de clave, el valor y el tipo de cada par de {clave: valor} que se encuentra en *expresiónJSON*. Como alternativa, se puede especificar explícitamente el esquema del conjunto de resultados que **OPENJSON** devuelve proporcionando *cláusula_with*.
  
### <a name="with_clause"></a>cláusula_with
  
![Sintaxis de la cláusula WITH en TVF OPENJSON](../../relational-databases/json/media/openjson-shema-syntax.png "Sintaxis de WITH de OPENJSON")

La *cláusula_with* contiene una lista de columnas con sus tipos que **OPENJSON** tiene que devolver. De forma predeterminada, **OPENJSON** hace coincidir las claves en *expresiónJSON* con los nombres de columna en la *cláusula_with* (en este caso, la coincidencia de claves implica que distingue mayúsculas de minúsculas). Si un nombre de columna no coincide con un nombre de clave, se puede proporcionar una función opcional *column_path*, que es una [expresión de ruta de acceso JSON](../../relational-databases/json/json-path-expressions-sql-server.md) que hace referencia a una clave en la *expresiónJSON*. 

## <a name="arguments"></a>Argumentos

### <a name="jsonexpression"></a>*expresiónJSON*

Es una expresión de caracteres Unicode que contiene texto JSON.  
  
OPENJSON recorre en iteración los elementos de la matriz o las propiedades del objeto en la expresión JSON y devuelve una fila por cada elemento o propiedad. En el ejemplo siguiente se devuelve cada propiedad del objeto proporcionado como *expresiónJSON*:  

```sql
DECLARE @json NVARCHAR(2048) = N'{
   "String_value": "John",
   "DoublePrecisionFloatingPoint_value": 45,
   "DoublePrecisionFloatingPoint_value": 2.3456,
   "BooleanTrue_value": true,
   "BooleanFalse_value": false,
   "Null_value": null,
   "Array_value": ["a","r","r","a","y"],
   "Object_value": {"obj":"ect"}
}';

SELECT * FROM OpenJson(@json);
```

**Resultados:**

| key                                | value                 | type |
| :--                                | :----                 | :--- |
| String_value                       | John                  | 1 |
| DoublePrecisionFloatingPoint_value | 45                    | 2 |
| DoublePrecisionFloatingPoint_value | 2.3456                | 2 |
| BooleanTrue_value                  | true                  | 3 |
| BooleanFalse_value                 | false                 | 3 |
| Null_value                         | NULL                  | 0 |
| Array_value                        | ["a","r","r","a","y"] | 4 |
| Object_value                       | {"obj":"ect"}         | 5 |
| &nbsp; | &nbsp; | &nbsp; |

- DoublePrecisionFloatingPoint_value se ajusta a IEEE-754.

### <a name="path"></a>*path*

Es una expresión de ruta de acceso JSON opcional que hace referencia a un objeto o una matriz dentro de *expresiónJSON*. **OPENJSON** busca en el texto de JSON en la posición especificada y analiza solo el fragmento al que se hace referencia. Para más información, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).

En [!INCLUDE [sssql17-md](../../includes/sssql17-md.md)] y en [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)], puede proporcionar una variable como el valor de *path*.
  
En el ejemplo siguiente se devuelve un objeto anidado especificando la *path*:  

```sql  
DECLARE @json NVARCHAR(4000) = N'{  
      "path": {  
            "to":{  
                 "sub-object":["en-GB", "en-UK","de-AT","es-AR","sr-Cyrl"]  
                 }  
              }  
 }';

SELECT [key], value
FROM OPENJSON(@json,'$.path.to."sub-object"')
```  
  
 **Resultados**  
  
|Clave|Value|  
|---------|-----------|  
|0|en-GB|  
|1|en-UK|  
|2|de-AT|  
|3|es-AR|  
|4|sr-Cyrl|  

Cuando **OPENJSON** analiza una matriz JSON, la función devuelve los índices de los elementos en el texto JSON como claves.

La comparación que se usa para hacer coincidir los pasos de ruta de acceso con las propiedades de la expresión JSON distingue mayúsculas de minúsculas, e ignora la intercalación (es decir, una comparación BIN2). 

### <a name="with_clause"></a>*cláusula_with*

Define explícitamente el esquema de salida que va a devolver la función **OPENJSON**. La *cláusula_with* opcional puede contener los elementos siguientes:

*colName* es el nombre de la columna de salida.  
  
De forma predeterminada, **OPENJSON** usa el nombre de la columna para que coincida con una propiedad en el texto JSON. Por ejemplo, si especifica la columna *nombre* en el esquema, OPENJSON intenta rellenar esta columna con la propiedad "nombre" en el texto JSON. Puede invalidar esta asignación predeterminada mediante el argumento *column_path*.  
  
*type*  
Es el tipo de datos para la columna de salida.  

> [!NOTE]
> Si también usa la opción **AS JSON**, el *tipo* de columna debe ser `NVARCHAR(MAX)`.
  
*column_path*  
Es la ruta de acceso JSON que especifica la propiedad que se va a devolver en la columna especificada. Para obtener más información, vea la descripción del parámetro *path* anteriormente en este tema.  
  
Use *ruta_de_columna* para invalidar las reglas de asignación predeterminadas cuando el nombre de una columna de salida no coincida con el nombre de la propiedad.  
  
La comparación que se usa para hacer coincidir los pasos de ruta de acceso con las propiedades de la expresión JSON distingue mayúsculas de minúsculas e ignora la intercalación (es decir, una comparación BIN2).  
  
Para obtener más información sobre las rutas de acceso, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).  
  
*AS JSON*  
Use la opción **AS JSON** en una definición de columna para especificar que la propiedad a la que se hace referencia contiene un objeto JSON interno o una matriz. Si se especifica la opción **AS JSON**, el tipo de la columna debe ser NVARCHAR(MAX).

- Si no se especifica **AS JSON** para una columna, la función devuelve un valor escalar (por ejemplo, int, string, true, false) de la propiedad JSON especificada en la ruta de acceso especificada. Si la ruta de acceso representa un objeto o una matriz, y no se puede encontrar la propiedad en la ruta de acceso especificada, la función devuelve NULL en modo lax o devuelve un error en modo strict. Este comportamiento es similar al de la función **JSON_VALUE**.  
  
- Si se especifica **AS JSON** para una columna, la función devuelve un fragmento de JSON de la propiedad JSON especificada en la ruta de acceso especificada. Si la ruta de acceso representa un valor escalar y no se puede encontrar la propiedad en la ruta de acceso especificada, la función devuelve NULL en modo lax o devuelve un error en modo strict. Este comportamiento es similar al de la función **JSON_QUERY**.  
  
> [!NOTE]  
> Si quiere devolver un fragmento de JSON anidado de una propiedad JSON, tendrá que proporcionar la marca **AS JSON**. Sin esta opción, si no se puede encontrar la propiedad, OPENJSON devuelve un valor NULL en lugar de la matriz o el objeto JSON al que se hace referencia, o bien devuelve un error en tiempo de ejecución en modo strict.  
  
Por ejemplo, en la consulta siguiente se devuelve y se da formato a los elementos de una matriz:  
  
```sql  
DECLARE @json NVARCHAR(MAX) = N'[  
  {  
    "Order": {  
      "Number":"SO43659",  
      "Date":"2011-05-31T00:00:00"  
    },  
    "AccountNumber":"AW29825",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":1  
    }  
  },  
  {  
    "Order": {  
      "Number":"SO43661",  
      "Date":"2011-06-01T00:00:00"  
    },  
    "AccountNumber":"AW73565",  
    "Item": {  
      "Price":2024.9940,  
      "Quantity":3  
    }  
  }
]'  
   
SELECT *
FROM OPENJSON ( @json )  
WITH (   
              Number   VARCHAR(200)   '$.Order.Number',  
              Date     DATETIME       '$.Order.Date',  
              Customer VARCHAR(200)   '$.AccountNumber',  
              Quantity INT            '$.Item.Quantity',  
              [Order]  NVARCHAR(MAX)  AS JSON  
 )
```  
  
**Resultados**
  
|Number|Date|Customer|Cantidad|Pedido de|  
|------------|----------|--------------|--------------|-----------|  
|SO43659|2011-05-31T00:00:00|AW29825|1|{"Number":"SO43659","Date":"2011-05-31T00:00:00"}|  
|SO43661|2011-06-01T00:00:00|AW73565|3|{"Number":"SO43661","Date":"2011-06-01T00:00:00"}|  
  
## <a name="return-value"></a>Valor devuelto
Las columnas que devuelve la función OPENJSON dependen de la opción WITH.  
  
1. Cuando se llama a OPENJSON con el esquema predeterminado, es decir, cuando no se especifica un esquema explícito en la cláusula WITH, la función devuelve una tabla con las columnas siguientes:  
    1.  **Clave**. Un valor nvarchar(4000) que contiene el nombre de la propiedad especificada o el índice del elemento en la matriz especificada. La columna de clave tiene una intercalación BIN2.  
    2.  **Valor**. Un valor nvarchar(max) que contiene el valor de la propiedad. La columna Valor hereda la intercalación de *expresiónJSON*.
    3.  **Tipo**. Un valor int que contiene el tipo del valor. La columna **Tipo** solo se devuelve cuando se usa OPENJSON con el esquema predeterminado. La columna Tipo tiene uno de los valores siguientes:  
  
        |Valor de la columna Tipo|Tipo de datos JSON|  
        |------------------------------|--------------------|  
        |0|null|  
        |1|string|  
        |2|number|  
        |3|true/false|  
        |4|array|  
        |5|object|  
  
     Solo se devuelven las propiedades de primer nivel. Se produce un error en la instrucción si el texto JSON no tiene el formato correcto.  

2. Cuando se llama a OPENJSON y se especifica un esquema explícito en la cláusula WITH, la función devuelve una tabla con el esquema que se haya definido en la cláusula WITH.

> [!NOTE]  
> Las columnas **Clave**, **Valor** y **Tipo** se devuelven solo cuando se usa OPENJSON con el esquema predeterminado y no están disponibles con un esquema explícito.

## <a name="remarks"></a>Observaciones  

La *json_path* que se usa en el segundo argumento de **OPENJSON** o en la *cláusula_with* puede comenzar con la palabra clave **lax** o **strict**.

- En modo **lax**, **OPENJSON** no genera un error si no se encuentra el objeto o el valor en la ruta de acceso especificada. Si no se puede encontrar la ruta de acceso, **OPENJSON** devuelve un conjunto de resultados vacío o un valor NULL.
- En modo **strict**, **OPENJSON** devuelve un error si no se encuentra la ruta de acceso.

En algunos de los ejemplos de esta página se especifica explícitamente el modo de ruta de acceso, lax o strict. El modo de ruta de acceso es opcional. Si no especifica explícitamente un modo de ruta de acceso, el modo lax es el valor predeterminado. Para más información sobre las expresiones de modo de ruta de acceso y de ruta de acceso, vea [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md).    

Los nombres de columna en la *cláusula_with* se hacen coincidir con las claves en el texto JSON. Si se especifica el nombre de columna `[Address.Country]`, se compara con la clave `Address.Country`. Si quiere hacer referencia a una clave anidada `Country` dentro del objeto `Address`, tendrá que especificar la ruta de acceso `$.Address.Country` en la ruta de acceso de columna.

*json_path* puede contener claves con caracteres alfanuméricos. Use comillas dobles como caracteres de escape para el nombre de clave en *json_path* si hay caracteres especiales en las claves. Por ejemplo, `$."my key $1".regularKey."key with . dot"` coincide con el valor 1 en el siguiente texto JSON:

```json
{
  "my key $1": {
    "regularKey":{
      "key with . dot": 1
    }
  }
}
```  

## <a name="examples"></a>Ejemplos  
  
### <a name="example-1---convert-a-json-array-to-a-temporary-table"></a>Ejemplo 1: conversión de una matriz JSON en una tabla temporal

En el ejemplo siguiente se proporciona una lista de identificadores como una matriz JSON de números. La consulta convierte la matriz JSON en una tabla de identificadores y filtra todos los productos con los identificadores especificados.  
  
```sql  
DECLARE @pSearchOptions NVARCHAR(4000) = N'[1,2,3,4]'

SELECT *
FROM products
INNER JOIN OPENJSON(@pSearchOptions) AS productTypes
 ON product.productTypeID = productTypes.value
```  
  
Esta consulta equivale al ejemplo siguiente. Pero en el ejemplo siguiente, se deben insertar números en la consulta en lugar de pasarlos como parámetros.  
  
```sql  
SELECT *
FROM products
WHERE product.productTypeID IN (1,2,3,4)
```  
  
### <a name="example-2---merge-properties-from-two-json-objects"></a>Ejemplo 2: combinación de propiedades de dos objetos JSON

En el ejemplo siguiente se selecciona una unión de todas las propiedades de dos objetos JSON. Los dos objetos tienen una propiedad *nombre* duplicada. En el ejemplo se usa el valor de clave para excluir la fila duplicada de los resultados.  
  
```sql  
DECLARE @json1 NVARCHAR(MAX),@json2 NVARCHAR(MAX)

SET @json1=N'{"name": "John", "surname":"Doe"}'

SET @json2=N'{"name": "John", "age":45}'

SELECT *
FROM OPENJSON(@json1)
UNION ALL
SELECT *
FROM OPENJSON(@json2)
WHERE [key] NOT IN (SELECT [key] FROM OPENJSON(@json1))
```  
  
### <a name="example-3---join-rows-with-json-data-stored-in-table-cells-using-cross-apply"></a>Ejemplo 3: combinación de filas con datos JSON almacenados en celdas de tabla con CROSS APPLY

En el ejemplo siguiente, la tabla `SalesOrderHeader` tiene una columna de texto `SalesReason` que contiene una matriz de `SalesOrderReasons` en formato JSON. Los objetos `SalesOrderReasons` contienen propiedades como *Calidad* y *Fabricante*. En el ejemplo se crea un informe que combina todas las filas de pedido de ventas y las razones de ventas relacionadas. El operador OPENJSON expande la matriz JSON de razones de venta como si las razones se almacenaran en una tabla secundaria independiente. Después, el operador CROSS APPLY combina cada fila de pedido de ventas con las filas devueltas por la función con valores de tabla OPENJSON.  
  
```sql  
SELECT SalesOrderID,OrderDate,value AS Reason
FROM Sales.SalesOrderHeader
CROSS APPLY OPENJSON(SalesReasons)
```  
  
> [!TIP] 
> Cuando es necesario expandir matrices JSON almacenadas en campos individuales y combinarlas con sus filas primarias, normalmente se usa el operador CROSS APPLY de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información sobre CROSS APPLY, vea [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md).  
  
Se puede rescribir la misma consulta mediante `OPENJSON` con un esquema definido explícitamente de las filas que se van a devolver:  
  
```sql  
SELECT SalesOrderID, OrderDate, value AS Reason  
FROM Sales.SalesOrderHeader  
     CROSS APPLY OPENJSON (SalesReasons) WITH (value NVARCHAR(100) '$')
```  
  
En este ejemplo, la ruta de acceso `$` hace referencia a cada elemento de la matriz. Si quiere convertir explícitamente el valor devuelto, puede usar este tipo de consulta.  
  
### <a name="example-4---combine-relational-rows-and-json-elements-with-cross-apply"></a>Ejemplo 4: combinación de filas relacionales y elementos de JSON con CROSS APPLY

En la consulta siguiente se combinan filas relacionales y elementos de JSON en los resultados mostrados en la tabla siguiente.  
  
```sql  
SELECT store.title, location.street, location.lat, location.long  
FROM store  
CROSS APPLY OPENJSON(store.jsonCol, 'lax $.location')   
     WITH (street VARCHAR(500) ,  postcode VARCHAR(500) '$.postcode' ,  
     lon int '$.geo.longitude', lat int '$.geo.latitude')  
     AS location
```  
  
**Resultados**
  
|title|street|postcode|lon|lat|  
|-----------|------------|--------------|---------|---------|  
|Whole Food Markets|17991 Redmond Way|WA  98052|47.666124|-122.10155|  
|Sears|148th Ave NE|WA  98052|47.63024|-122.141246,17|  
  
### <a name="example-5---import-json-data-into-sql-server"></a>Paso 5: importación de datos JSON a tablas de SQL Server

En el ejemplo siguiente se carga un objeto JSON completo en una tabla de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```sql  
DECLARE @json NVARCHAR(max)  = N'{  
  "id" : 2,  
  "firstName": "John",  
  "lastName": "Smith",  
  "isAlive": true,  
  "age": 25,  
  "dateOfBirth": "2015-03-25T12:00:00",  
  "spouse": null  
  }';  
   
  INSERT INTO Person  
  SELECT *   
  FROM OPENJSON(@json)  
  WITH (id INT,  
        firstName NVARCHAR(50), lastName NVARCHAR(50),   
        isAlive BIT, age INT,  
        dateOfBirth DATETIME, spouse NVARCHAR(50))
```  

### <a name="example-6---simple-example-with-json-content"></a>Ejemplo 6: Ejemplo sencillo con contenido JSON

```sql
--simple cross apply example
DECLARE @JSON NVARCHAR(MAX) = N'[
{
"OrderNumber":"SO43659",
"OrderDate":"2011-05-31T00:00:00",
"AccountNumber":"AW29825",
"ItemPrice":2024.9940,
"ItemQuantity":1
},
{
"OrderNumber":"SO43661",
"OrderDate":"2011-06-01T00:00:00",
"AccountNumber":"AW73565",
"ItemPrice":2024.9940,
"ItemQuantity":3
}
]'

SELECT root.[key] AS [Order],TheValues.[key], TheValues.[value]
FROM OPENJSON ( @JSON ) AS root
CROSS APPLY OPENJSON ( root.value) AS TheValues
```

## <a name="see-also"></a>Consulte también

- [Expresiones de ruta de acceso JSON &#40;SQL Server&#41;](../../relational-databases/json/json-path-expressions-sql-server.md)   
- [Convert JSON Data to Rows and Columns with OPENJSON &#40;SQL Server&#41; (Convertir datos JSON en filas y columnas con OPENJSON &#40;SQL Server&#41;)](../../relational-databases/json/convert-json-data-to-rows-and-columns-with-openjson-sql-server.md)   
- [Uso de OPENJSON con el esquema predeterminado &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-the-default-schema-sql-server.md)   
- [Uso de OPENJSON con un esquema explícito &#40;SQL Server&#41;](../../relational-databases/json/use-openjson-with-an-explicit-schema-sql-server.md)  
  
