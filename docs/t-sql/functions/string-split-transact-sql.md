---
title: STRING_SPLIT (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para la función STRING_SPLIT. Esta función con valores de tabla divide una cadena en subcadenas según un delimitador de caracteres.
ms.date: 11/28/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jrasnick
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STRING_SPLIT
- STRING_SPLIT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- STRING_SPLIT function
ms.assetid: 3273dbf3-0b4f-41e1-b97e-b4f67ad370b9
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current||=azure-sqldw-latest||>= sql-server-2016 || >= sql-server-linux-2017
ms.openlocfilehash: 472da04cc9a93a8f7846927ad9ae4595766e9116
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207886"
---
# <a name="string_split-transact-sql"></a>STRING_SPLIT (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Una función con valores de tabla que divide una cadena en filas de subcadenas, según un carácter separador especificado.

#### <a name="compatibility-level-130"></a>Nivel de compatibilidad 130

STRING_SPLIT requiere que el nivel de compatibilidad sea al menos 130. Cuando el nivel es inferior a 130, SQL Server no puede encontrar la función STRING_SPLIT.

Para cambiar el nivel de compatibilidad de una base de datos, vea [Ver o cambiar el nivel de compatibilidad de una base de datos](../../relational-databases/databases/view-or-change-the-compatibility-level-of-a-database.md).

> [!NOTE]
> No es necesario configurar la compatibilidad para STRING_SPLIT en Azure Synapse Analytics.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  

```syntaxsql
STRING_SPLIT ( string , separator )  
```

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *string*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo de carácter (por ejemplo **nvarchar**, **varchar**, **nchar** o **char**).  
  
 *separator*  
 Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de carácter único de cualquier tipo de caracteres (por ejemplo, **nvarchar(1)** , **varchar(1)** , **nchar(1)** o **char(1)** ) que se usa como separador para subcadenas concatenadas.  
  
## <a name="return-types"></a>Tipos de valor devuelto  

Devuelve una tabla de una sola columna cuyas filas son las subcadenas. El nombre de la columna es **value**. Devuelve **nvarchar** si cualquiera de los argumentos de entrada es **nvarchar** o **nchar**. De lo contrario, devuelve **varchar**. La longitud del tipo de valor devuelto es igual a la longitud del argumento de cadena.  
  
## <a name="remarks"></a>Observaciones  

**STRING_SPLIT** introduce una cadena que tiene subcadenas delimitadas y un carácter que se usará como el delimitador o el separador. STRING_SPLIT da como resultado una tabla de una sola columna cuyas filas contienen las subcadenas. El nombre de la columna de resultados es **value**.

Las filas de salida pueden estar en cualquier orden. _No_ se garantiza que el orden coincida con el de las subcadenas de la cadena de entrada. Puede invalidar el orden final usando una cláusula ORDER BY en la instrucción SELECT (`ORDER BY value`).

0x0000 (**char(0)** ) es un carácter no definido en las intercalaciones de Windows y no se puede incluir en STRING_SPLIT.

Las subcadenas vacías de longitud cero están presentes cuando la cadena de entrada contiene dos o más repeticiones consecutivas del carácter delimitador. Las subcadenas vacías se tratan de la misma forma que las subcadenas sin formato. Puede filtrar las filas que contienen la subcadena vacía usando la cláusula WHERE (`WHERE value <> ''`). Si la cadena de entrada es NULL, la función con valores de tabla STRING_SPLIT devuelve una tabla vacía.  

Por ejemplo, la siguiente instrucción SELECT utiliza el carácter de espacio como el separador:

```sql
SELECT value FROM STRING_SPLIT('Lorem ipsum dolor sit amet.', ' ');
```

En una ejecución práctica, la instrucción SELECT anterior devolvió la siguiente tabla de resultados:  
  
|value|  
| :-- |  
|Lorem|  
|ipsum|  
|dolor|  
|sit|  
|amet.|  
| &nbsp; |

## <a name="examples"></a>Ejemplos  
  
### <a name="a-split-comma-separated-value-string"></a>A. Dividir una cadena de valores separados por coma

Se analiza una lista de valores separados por coma y se devuelven todos los tokens que no están vacíos:  

```sql
DECLARE @tags NVARCHAR(400) = 'clothing,road,,touring,bike'  
  
SELECT value  
FROM STRING_SPLIT(@tags, ',')  
WHERE RTRIM(value) <> '';
```

STRING_SPLIT devolverá una cadena vacía si no hay nada entre el separador. La condición "RTRIM(value) <>" quitará los tokens vacíos.  
  
### <a name="b-split-comma-separated-value-string-in-a-column"></a>B. Dividir una cadena de valores separados por coma en una columna

La tabla Product tiene una columna con una lista de etiquetas separadas por comas que se muestran en el siguiente ejemplo:  
  
|ProductId|Nombre|Etiquetas|  
|---------------|----------|----------|  
|1|Guantes|clothing,road,touring,bike|  
|2|Auriculares LL|bike|  
|3|HL Mountain Frame|bike,mountain|  
  
Con la siguiente consulta se transforma cada lista de etiquetas y las combina con la fila original:  

```sql  
SELECT ProductId, Name, value  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',');  
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|ProductId|Nombre|value|  
|---------------|----------|-----------|  
|1|Guantes|clothing|  
|1|Guantes|carreteras|  
|1|Guantes|touring|  
|1|Guantes|bike|  
|2|Auriculares LL|bike|  
|3|HL Mountain Frame|bike|  
|3|HL Mountain Frame|mountain|  

  >[!NOTE]
  > El orden de la salida puede variar, ya que _no_ se garantiza que el orden coincida con el de las subcadenas de la cadena de entrada.
  
### <a name="c-aggregation-by-values"></a>C. Agregación por valores

Los usuarios deben crear un informe en el que se muestre el número de productos por etiqueta, ordenadas por número de productos, y filtrar solo las etiquetas con más de dos productos.  

```sql  
SELECT value as tag, COUNT(*) AS [Number of articles]  
FROM Product  
    CROSS APPLY STRING_SPLIT(Tags, ',')  
GROUP BY value  
HAVING COUNT(*) > 2  
ORDER BY COUNT(*) DESC;  
```

### <a name="d-search-by-tag-value"></a>D. Buscar por el valor de tabla

Los desarrolladores deben crear consultas que hallen artículos a partir de palabras clave. Pueden usar las siguientes consultas:  
  
Para encontrar productos con una sola etiqueta (clothing):  

```sql
SELECT ProductId, Name, Tags  
FROM Product  
WHERE 'clothing' IN (SELECT value FROM STRING_SPLIT(Tags, ','));  
```

Para encontrar productos con dos etiquetas especificadas (clothing y road):  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE EXISTS (SELECT *  
    FROM STRING_SPLIT(Tags, ',')  
    WHERE value IN ('clothing', 'road'));  
```

### <a name="e-find-rows-by-list-of-values"></a>E. Encontrar filas por la lista de valores

Los desarrolladores deben crear una consulta que busque los artículos a partir de una lista de identificadores. Pueden usar la siguiente consulta:  

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
JOIN STRING_SPLIT('1,2,3',',')
    ON value = ProductId;  
```  

El uso de STRING_SPLIT anterior es un reemplazo para un antipatrón común. Este tipo de antipatrón puede implicar la creación de una cadena SQL dinámica en el nivel de aplicación o en Transact-SQL. O bien un antipatrón puede lograrse mediante el operador LIKE. Vea la instrucción SELECT de ejemplo siguiente:

```sql  
SELECT ProductId, Name, Tags  
FROM Product  
WHERE ',1,2,3,' LIKE '%,' + CAST(ProductId AS VARCHAR(20)) + ',%';  
```

## <a name="see-also"></a>Consulte también

- [LEFT &#40;Transact-SQL&#41;](../../t-sql/functions/left-transact-sql.md)
- [LTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/ltrim-transact-sql.md)
- [RIGHT &#40;Transact-SQL&#41;](../../t-sql/functions/right-transact-sql.md)
- [RTRIM &#40;Transact-SQL&#41;](../../t-sql/functions/rtrim-transact-sql.md)
- [SUBSTRING &#40;Transact-SQL&#41;](../../t-sql/functions/substring-transact-sql.md)
- [TRIM &#40;Transact-SQL&#41;](../../t-sql/functions/trim-transact-sql.md)
- [Funciones de cadena &#40;Transact-SQL&#41;](../../t-sql/functions/string-functions-transact-sql.md)
