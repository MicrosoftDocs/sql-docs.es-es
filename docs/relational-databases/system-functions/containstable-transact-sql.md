---
description: CONTAINSTABLE (Transact-SQL)
title: CONTAINSTABLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/24/2015
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CONTAINSTABLE
- CONTAINSTABLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- fuzzy (less precise) word or phrase search [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- values [SQL Server], ranked
- LANGUAGE option
- NEAR option [full-text search]
- RANK column
- phrase searches [full-text search]
- conditions [SQL Server], CONTAINSTABLE
- relevance ranking values [full-text search]
- proximity searches [full-text search]
- CONTAINSTABLE function (Transact-SQL)
- ranked results [full-text search]
- rankings [full-text search]
- less precise (fuzzy) searches [full-text search]
ms.assetid: e580c210-cf57-419d-9544-7f650f2ab814
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0884b34e2da3eaa6a081da8bebaae06f4d16ddc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196144"
---
# <a name="containstable-transact-sql"></a>CONTAINSTABLE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una tabla con una o más filas para aquellas columnas que contengan coincidencias exactas o aproximadas (menos precisas) con palabras simples o frases, palabras próximas a otra dada (dentro de una cierta distancia) o bien coincidencias ponderadas. CONTAINSTABLE se usa en la [cláusula FROM](../../t-sql/queries/from-transact-sql.md) de una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción SELECT y se hace referencia a él como si fuera un nombre de tabla normal. Realiza una búsqueda de texto completo en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en las columnas indizadas de texto completo que contienen tipos de datos basados en caracteres.  
  
 CONTAINSTABLE es útil para los mismos tipos de coincidencias que el [predicado CONTAINS](../../t-sql/queries/contains-transact-sql.md) y utiliza las mismas condiciones de búsqueda que Contains.  
  
 A diferencia de CONTAINS, las consultas que usan CONTAINSTABLE devuelven un valor de clasificación por relevancia (RANK) y un valor de clave de texto completo (KEY) por cada fila.  Para obtener información sobre las formas de búsqueda de texto completo que se admiten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Consulta con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
CONTAINSTABLE   
( table , { column_name | ( column_list ) | * } , ' <contains_search_condition> '   
     [ , LANGUAGE language_term]   
  [ , top_n_by_rank ]   
)   
  
<contains_search_condition> ::=   
    { <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    |  <weighted_term>   
    }   
    | { ( <contains_search_condition> )   
    { { AND | & } | { AND NOT | &! } | { OR | | } }   
     <contains_search_condition> [ ...n ]   
    }  
  
<simple_term> ::=   
     { word | "phrase" }  
<prefix term> ::=   
     { "word*" | "phrase*" }   
<generation_term> ::=   
     FORMSOF ( { INFLECTIONAL | THESAURUS } , <simple_term> [ ,...n ] )   
  
<generic_proximity_term> ::=   
     { <simple_term> | <prefix_term> } { { { NEAR | ~ }   
     { <simple_term> | <prefix_term> } } [ ...n ] }  
  
<custom_proximity_term> ::=   
  NEAR (   
     {  
        { <simple_term> | <prefix_term> } [ ,...n ]  
     |  
        ( { <simple_term> | <prefix_term> } [ ,...n ] )   
      [, <maximum_distance> [, <match_order> ] ]  
     }  
       )   
  
      <maximum_distance> ::= { integer | MAX }  
      <match_order> ::= { TRUE | FALSE }   
  
<weighted_term> ::=   
     ISABOUT  
    ( { {   
  <simple_term>   
  | <prefix_term>   
  | <generation_term>   
  | <proximity_term>   
  }   
   [ WEIGHT ( weight_value ) ]   
   } [ ,...n ]   
    )  
  
```  
  
## <a name="arguments"></a>Argumentos  
 *table*  
 Es el nombre de una tabla que se ha sometido a una indización de texto completo. la *tabla* puede ser un nombre de objeto de base de datos de una, dos, tres o cuatro partes. Al consultar una vista, puede incluirse solo una tabla base indizada de texto completo.  
  
 la *tabla* no puede especificar un nombre de servidor y no se puede usar en consultas en servidores vinculados.  
  
 *column_name*  
 Es el nombre de una o más columnas indizadas para la búsqueda de texto completo. Las columnas pueden ser de tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** o **varbinary(max)** .  
  
 *lista_de_columnas*  
 Indica que se pueden especificar varias columnas, separadas por una coma. *column_list* debe ir entre paréntesis. A menos que se especifique *language_term*, el idioma de todas las columnas de *column_list* debe ser el mismo.  
  
 \*  
 Especifica que todas las columnas indizadas de texto completo de la *tabla* se deben usar para buscar la condición de búsqueda determinada. A menos que se especifique *language_term*, el idioma de todas las columnas de la tabla debe ser el mismo.  
  
 LANGUAGE *language_term*  
 Es el lenguaje cuyos recursos se utilizarán para la separación de palabras, la lematización, el Diccionario de sinónimos y la eliminación de palabras irrelevantes (o [palabra irrelevante](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)) como parte de la consulta. Este parámetro es opcional y puede especificarse como un valor hexadecimal, un entero o una cadena correspondiente al identificador de configuración regional (LCID) de un idioma. Si se especifica *language_term*, el idioma que representa se aplicará a todos los elementos de la condición de búsqueda. Si no se especifica ningún valor, se utiliza el idioma de texto completo de la columna.  
  
 Si se almacenan juntos documentos de idiomas diferentes como objetos binarios grandes (BLOB) en una sola columna, el identificador de configuración regional (LCID) de un documento determinado determina qué idioma se usa para indizar su contenido. Al consultar este tipo de columna, especificar *LANGUAGE**language_term* puede aumentar la probabilidad de encontrar una coincidencia acertada.  
  
 Cuando se especifica como una cadena, *language_term* corresponde al valor de la columna **alias** en la vista de compatibilidad de los [ lenguajes desys.sys](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md) .  La cadena debe estar delimitada con comillas sencillas, como en '*language_term*'. Cuando se especifica como un entero, *language_term* es el LCID real que identifica el idioma. Cuando se especifica como un valor hexadecimal, *language_term* es 0x seguido del valor hexadecimal del LCID. El valor hexadecimal no puede superar los ocho dígitos, incluidos los ceros a la izquierda.  
  
 Si el valor está en formato DBCS (juego de caracteres de doble byte), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirá a Unicode.  
  
 Si el idioma especificado no es válido o no hay recursos instalados que se correspondan con dicho idioma, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error. Para usar recursos de idioma neutro, especifique 0x0 como *language_term*.  
  
 *top_n_by_rank*  
 Especifica que solo se devuelvan las *n* coincidencias con la clasificación más alta, en orden descendente. Solo se aplica cuando se especifica un valor entero, *n*. Si se combina *top_n_by_rank* con otros parámetros, es posible que la consulta devuelva menos filas de las que en realidad coinciden con todos los predicados. *top_n_by_rank* le permite aumentar el rendimiento de las consultas al recallar solo las coincidencias más relevantes.  
  
 <contains_search_condition>  
 Especifica el texto que se va a buscar en *column_name* y las condiciones para obtener coincidencias. Para obtener información acerca de las condiciones de búsqueda, vea [Contains &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 Los predicados y las funciones de texto completo operan en una única tabla, que se obtiene del predicado FROM. Para buscar en varias tablas, utilice una tabla combinada en la cláusula FROM a fin de buscar en un conjunto de resultados que sea el producto de dos o más tablas.  
  
 La tabla devuelta tiene una columna denominada **key** que contiene valores de clave de texto completo. Cada tabla indizada de texto completo tiene una columna cuyos valores se garantiza que son únicos y los valores devueltos en la columna de **clave** son los valores de clave de texto completo de las filas que coinciden con los criterios de selección especificados en la condición de búsqueda Contains. La propiedad **TableFulltextKeyColumn** , obtenida a partir de la función OBJECTPROPERTYEX, proporciona la identidad de esta columna de clave única. Para obtener el identificador de la columna asociada a la clave de texto completo del índice de texto completo, use **Sys.fulltext_indexes**. Para obtener más información, vea [sys.fulltext_indexes &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
 Para obtener las filas que desee de la tabla original, especifique una combinación con las filas de CONTAINSTABLE. La forma típica de la cláusula FROM de una instrucción SELECT que utiliza CONTAINSTABLE es la siguiente:  
  
```  
SELECT select_list  
FROM table AS FT_TBL INNER JOIN  
   CONTAINSTABLE(table, column, contains_search_condition) AS KEY_TBL  
   ON FT_TBL.unique_key_column = KEY_TBL.[KEY];  
```  
  
 La tabla generada por CONTAINSTABLE incluye una columna denominada **Rank**. La columna **Rank** es un valor (de 0 a 1000) para cada fila que indica el grado de coincidencia de la fila con los criterios de selección. Este valor de clasificación suele utilizarse en las instrucciones SELECT de una de estas maneras:  
  
-   En la cláusula ORDER BY, para devolver las filas de mayor valor al principio de la tabla.  
  
-   En la lista de selección, para ver el valor de clasificación asignado a cada fila.  
  
## <a name="permissions"></a>Permisos  
 Solamente disponen de permisos de ejecución los usuarios que tienen los permisos SELECT adecuados en la tabla o en las columnas de la tabla a las que se hace referencia.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-simple-example"></a>A. Ejemplo sencillo  
 En el ejemplo siguiente se crea y rellena una tabla simple de dos columnas, que enumera tres condados y los colores de sus marcas. Crea y rellena un catálogo de texto completo y un índice en la tabla. A continuación, se muestra la sintaxis de **CONTAINSTABLE** . En este ejemplo se muestra cómo el valor de rango crece más alto cuando el valor de búsqueda se alcanza varias veces. En la última consulta, el valor de Tanzania que contiene tanto el verde como el negro tiene un rango superior a Italia que solo contiene uno de los colores consultados.  
  
```  
CREATE TABLE Flags (Country nvarchar(30) NOT NULL, FlagColors varchar(200));  
CREATE UNIQUE CLUSTERED INDEX FlagKey ON Flags(Country);  
INSERT Flags VALUES ('France', 'Blue and White and Red');  
INSERT Flags VALUES ('Italy', 'Green and White and Red');  
INSERT Flags VALUES ('Tanzania', 'Green and Yellow and Black and Yellow and Blue');  
SELECT * FROM Flags;  
GO  
  
CREATE FULLTEXT CATALOG TestFTCat;  
CREATE FULLTEXT INDEX ON Flags(FlagColors) KEY INDEX FlagKey ON TestFTCat;  
GO   
  
SELECT * FROM Flags;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green') ORDER BY RANK DESC;  
SELECT * FROM CONTAINSTABLE (Flags, FlagColors, 'Green or Black') ORDER BY RANK DESC;  
```  
  
### <a name="b-returning-rank-values"></a>B. Devolver valores de clasificación  
 En el ejemplo siguiente se buscan todos los nombres de producto que contienen las palabras “frame”, “wheel” o “tire” y se asignan ponderaciones distintas a cada palabra. Por cada fila devuelta que cumpla estos criterios de búsqueda, se muestra la precisión relativa (el valor de clasificación) de la coincidencia. Además, las filas cuyo valor de clasificación es más alto se devuelven primero.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.Name, KEY_TBL.RANK  
    FROM Production.Product AS FT_TBL   
        INNER JOIN CONTAINSTABLE(Production.Product, Name,   
        'ISABOUT (frame WEIGHT (.8),   
        wheel WEIGHT (.4), tire WEIGHT (.2) )' ) AS KEY_TBL  
            ON FT_TBL.ProductID = KEY_TBL.[KEY]  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
### <a name="c-returning-rank-values-greater-than-a-specified-value"></a>C. Devolver valores de clasificación más altos que un valor especificado  
  
||  
|-|  
|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.|  
  
 En el ejemplo siguiente se usa NEAR para buscar “`bracket`” y “`reflector`” cercanos entre sí en la tabla `Production.Document`. Solo se devuelven las filas con un valor de rango de 50 o superior.  
  
```  
USE AdventureWorks2012  
GO  
  
SELECT DocumentNode, Title, DocumentSummary  
FROM Production.Document AS DocTable   
INNER JOIN CONTAINSTABLE(Production.Document, Document,  
  'NEAR(bracket, reflector)' ) AS KEY_TBL  
  ON DocTable.DocumentNode = KEY_TBL.[KEY]  
WHERE KEY_TBL.RANK > 50  
ORDER BY KEY_TBL.RANK DESC;  
GO  
```  
  
> [!NOTE]  
>  Si en una consulta de texto completo no se especifica un entero como la distancia máxima, un documento que contiene solo los aciertos cuyo intervalo es mayor que 100 términos lógicos no cumplirá los requisitos NEAR, y la clasificación será 0.  
  
### <a name="d-returning-top-5-ranked-results-using-top_n_by_rank"></a>D. Devolver los 5 mejores resultados utilizando top_n_by_rank  
 En el ejemplo siguiente se devuelve la descripción de los 5 productos más destacados, donde la columna `Description` contiene la palabra “aluminum” cerca de la palabra “light” o de la palabra “lightweight”.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
 `GO`  
  
### <a name="e-specifying-the-language-argument"></a>E. Especificar el argumento LANGUAGE  
 En el siguiente ejemplo se muestra la forma de utilizar el argumento `LANGUAGE`.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT FT_TBL.ProductDescriptionID,  
   FT_TBL.Description,   
   KEY_TBL.RANK  
FROM Production.ProductDescription AS FT_TBL INNER JOIN  
   CONTAINSTABLE (Production.ProductDescription,  
      Description,   
      '(light NEAR aluminum) OR  
      (lightweight NEAR aluminum)',  
      LANGUAGE N'English',  
      5  
   ) AS KEY_TBL  
   ON FT_TBL.ProductDescriptionID = KEY_TBL.[KEY];  
GO  
```  
  
> [!NOTE]  
>  El argumento LANGUAGE *language_term* no es necesario para usar *top_n_by_rank.*  
  
## <a name="see-also"></a>Consulte también  
 [Limitar los resultados de la búsqueda con rango](../../relational-databases/search/limit-search-results-with-rank.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [Crear consultas de búsqueda de texto completo &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)  
  
