---
title: CONTAINS (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para el elemento de lenguaje CONTAINS. Se usa para buscar palabras o frases dentro de otra expresión.
ms.date: 08/23/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CONTAINS_TSQL
- CONTAINS
dev_langs:
- TSQL
helpviewer_keywords:
- precise or fuzzy (less precise) matches [full-text search]
- CONTAINS predicate (Transact-SQL)
- conditions [SQL Server], CONTAINS
- fuzzy (less precise) word or phrase search [full-text search]
- word weighting values [full-text search]
- word searches [full-text search]
- weighted values [full-text search]
- LANGUAGE option
- word inflectionally generated from another [full-text search]
- NEAR option [full-text search]
- phrase searches [full-text search]
- word near another word search [full-text search]
- full-text search [SQL Server], searching on a property
- proximity searches [full-text search]
- less precise (fuzzy) searches [full-text search]
- property searching [SQL Server], searching on a property
- inflectional forms [full-text search]
- prefix searches [full-text search]
ms.assetid: 996c72fc-b1ab-4c96-bd12-946be9c18f84
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 680cbfab28064f92d446705de0ca2b2f778735df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207789"
---
# <a name="contains-transact-sql"></a>CONTAINS (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Busca coincidencias precisas o aproximadas (menos precisas) de palabras o frases, palabras que se encuentran a cierta distancia de otra o coincidencias ponderadas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. CONTAINS es un predicado que se usa en la [cláusula WHERE](../../t-sql/queries/where-transact-sql.md) de una instrucción SELECT de [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizar una búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en las columnas indizadas de texto completo que contienen tipos de datos basados en caracteres.  
  
 CONTAINS puede buscar:  
  
-   Una palabra o una frase.  
  
-   El prefijo de una palabra o una frase.  
  
-   Una palabra cerca de otra palabra.  
  
-   Una palabra que sea una inflexión de otra (por ejemplo, las palabras controles, controladores, controlando y controlado son inflexiones de control).  
  
-   Una palabra que sea un sinónimo de otra mediante un diccionario de sinónimos (por ejemplo, la palabra "metal" puede tener sinónimos como "aluminio" y "acero").  
  
 Para obtener información sobre las formas de búsqueda de texto completo que se admiten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Consulta con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md).  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CONTAINS (   
     {   
        column_name | ( column_list )   
      | *   
      | PROPERTY ( { column_name }, 'property_name' )    
     }   
     , '<contains_search_condition>'  
     [ , LANGUAGE language_term ]  
   )   
  
<contains_search_condition> ::=   
  {   
      <simple_term>   
    | <prefix_term>   
    | <generation_term>   
    | <generic_proximity_term>   
    | <custom_proximity_term>   
    | <weighted_term>   
    }   
  |   
    { ( <contains_search_condition> )   
        [ { <AND> | <AND NOT> | <OR> } ]   
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
   ( {   
        {   
          <simple_term>   
        | <prefix_term>   
        | <generation_term>   
        | <proximity_term>   
        }   
      [ WEIGHT ( weight_value ) ]   
      } [ ,...n ]   
   )   
  
<AND> ::=   
  { AND | & }  
  
<AND NOT> ::=   
  { AND NOT | &! }  
  
<OR> ::=   
  { OR | | }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *column_name*  
 Es el nombre de una columna indizada de texto completo de la tabla especificada en la cláusula FROM. Las columnas pueden ser de tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** o **varbinary(max)** .  
  
 *lista_de_columnas*  
 Especifica dos o más columnas, separadas por comas. *column_list* debe ir entre paréntesis. A menos que se especifique *language_term*, el idioma de todas las columnas de *column_list* debe ser el mismo.  
  
 \*  
 Especifica que la consulta busca la condición de búsqueda especificada en todas las columnas indizadas de texto completo de la tabla especificada en la cláusula FROM. Las columnas de la cláusula CONTAINS deben proceder de una tabla única que tenga un índice de texto completo. A menos que se especifique *language_term*, el idioma de todas las columnas de la tabla debe ser el mismo.  
  
 PROPERTY ( *column_name*, '*property_name*')  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores. 
  
 Especifica la propiedad de un documento por la que buscar la condición de búsqueda especificada.  
  
> [!IMPORTANT]  
>  Para que la consulta devuelva alguna fila, *property_name* se debe especificar en la lista de propiedades de búsqueda del índice de texto completo y el índice de texto completo debe contener entradas específicas de la propiedad *property_name*. Para obtener más información, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
 LANGUAGE *language_term*  
 Es el idioma que se usará para la separación de palabras, la lematización, las expansiones y reemplazos del diccionario de sinónimos y la eliminación de [palabras irrelevantes](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) como parte de la consulta. Este parámetro es opcional.  
  
 Si se almacenan juntos documentos de idiomas diferentes como objetos binarios (BLOB) en una sola columna, el identificador de configuración regional (LCID) de un documento determinado determina qué idioma se usa para indizar su contenido. Al consultar este tipo de columna, especificar LANGUAGE *language_term* puede aumentar la probabilidad de encontrar una coincidencia acertada.  
  
 *language_term* se puede especificar como una cadena, un entero o un valor hexadecimal correspondiente al identificador de configuración regional (LCID) de un idioma. Si se especifica *language_term*, el idioma que representa se aplica a todos los elementos de la condición de búsqueda. Si no se especifica ningún valor, se utiliza el idioma de texto completo de la columna.  
  
 Cuando se especifica como una cadena, *language_term* corresponde al valor de columna **alias** de la vista de compatibilidad [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). La cadena debe estar delimitada con comillas sencillas, como en '*language_term*'. Cuando se especifica como un entero, *language_term* es el LCID real que identifica el idioma. Cuando se especifica como un valor hexadecimal, *language_term* es 0x seguido del valor hexadecimal del LCID. El valor hexadecimal no puede superar los ocho dígitos, incluidos los ceros a la izquierda.  
  
 Si el valor está en formato de juego de caracteres de doble byte (DBCS), [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirá a Unicode.  
  
 Si el idioma especificado no es válido o no hay recursos instalados que se correspondan con dicho idioma, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error. Para usar recursos de idioma neutro, especifique 0x0 como *language_term*.  
  
 \<*contains_search_condition*>  
 Especifica el texto que se va a buscar en *column_name* y las condiciones para obtener coincidencias.  
  
*\<contains_search_condition>* es **nvarchar**. Se realiza una conversión implícita cuando se usa otro tipo de datos de carácter como entrada. No se pueden usar los tipos de datos de cadena grande varchar(max) y nvarchar(max). En el siguiente ejemplo, la variable `@SearchWord`, definida como una variable de tipo `varchar(30)`, provoca una conversión implícita en el predicado `CONTAINS`.
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 Como el "examen de parámetros" no funciona con la conversión, use **nvarchar** para lograr un mejor rendimiento. En el ejemplo, declare `@SearchWord` como `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
```  
  
 También puede usar la sugerencia de consulta OPTIMIZE FOR para los casos en los que se genera un plan poco óptimo.  
  
 *word*  
 Es una cadena de caracteres sin espacios ni signos de puntuación.  
  
 *phrase*  
 Es una o varias palabras con espacios entre cada una de ellas.  
  
> [!NOTE]  
>  Algunos idiomas, como los de algunas partes de Asia, pueden tener frases que contengan una o varias palabras sin espacios entre ellas.  
  
\<simple_term>  
Especifica una coincidencia para una palabra o frase exactas. Ejemplos de términos simples válidos son "blue berry", blueberry y "Microsoft SQL Server". Las frases tienen que ir entre comillas dobles (""). Las palabras de una frase tienen que aparecer en la columna de la base de datos en el mismo orden que el especificado en *\<contains_search_condition>* . La búsqueda de caracteres en la palabra o la frase no distingue mayúsculas de minúsculas. Las [palabras irrelevantes](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) (como un, y, el o la) de las columnas indizadas de texto completo no se almacenan en el índice de texto completo. Si se utiliza una palabra irrelevante en la búsqueda de una sola palabra, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un mensaje de error que indica que la consulta contiene solo palabras irrelevantes. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye una lista estándar de palabras irrelevantes en el directorio \Mssql\Binn\FTERef de cada instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Los signos de puntuación se omiten. Por lo tanto, `CONTAINS(testing, "computer failure")` coincide con una fila que contiene el valor "Where is my computer? Failure to find it would be expensive". Para más información sobre el comportamiento de los separadores de palabras, vea [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
 \<prefix_term>  
 Especifica la coincidencia de palabras o frases que comiencen con el texto especificado. Incluya un término prefijo entre comillas dobles ("") y agregue un asterisco (\*) delante de las comillas de cierre, de modo que se haga coincidir con cualquier texto que comience con el término sencillo especificado antes del asterisco. La cláusula debe especificarse de esta manera:`CONTAINS (column, '"text*"')`. El asterisco representa cero, uno o más caracteres (de la palabra raíz o de las palabras de la palabra o la frase). Si el texto y el asterisco no se delimitan con comillas dobles de modo que el predicado sea `CONTAINS (column, 'text*')`, la búsqueda de texto completo considera el asterisco un carácter y busca coincidencias exactas con `text*`. El motor de texto completo no encontrará palabras con el carácter de asterisco (\*) porque los separadores de palabras suelen omitir dichos caracteres.  
  
 Cuando *\<prefix_term>* es una frase, todas las palabras que contiene se consideran prefijos independientes. Por tanto, una consulta que especifique el prefijo "local wine*" hace que se devuelvan todas las filas que contengan el texto "local winery", "locally wined and dined", etc.  
  
 \<generation_term>  
 Especifica la coincidencia de palabras cuando los términos simples incluyen variaciones de la palabra original que se busca.  
  
 INFLECTIONAL  
 Especifica que se va a utilizar el analizador lingüístico dependiente del idioma en el término simple especificado. El comportamiento del analizador lingüístico se define en función de las reglas de análisis lingüístico de cada idioma concreto. El idioma neutro no tiene ningún analizador lingüístico asociado. El idioma de las columnas que se van a consultar se utiliza para hacer referencia al analizador lingüístico deseado. Si se especifica *language_term*, se usa el analizador lingüístico correspondiente a dicho idioma.  
  
 Un *\<simple_term>* determinado dentro de un *\<generation_term>* no coincidirá con nombres y verbos.  
  
 THESAURUS  
 Especifica que se utiliza el diccionario de sinónimos correspondiente al idioma de texto completo de la columna o el idioma especificado en la consulta. El patrón o patrones más largos de *\<simple_term>* se hacen coincidir con el diccionario de sinónimos y se generan términos adicionales para expandir o reemplazar el patrón original. Si no se encuentra ninguna coincidencia para la totalidad o parte de *\<simple_term>* , la parte que no coincide se trata como un elemento *simple_term*. Para más información sobre los sinónimos de búsqueda de texto completo, vea [Configurar y administrar archivos de sinónimos para búsquedas de texto completo](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md).  
  
 \<generic_proximity_term>  
 Especifica una coincidencia de palabras o frases que deben estar en el documento en el que se busca.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Se recomienda usar \<custom_proximity_term>.  
  
 NEAR | ~  
 Indica que la palabra o frase de cada lado del operador NEAR o ~ debe aparecer en un documento para que se devuelva una coincidencia. Debe especificar dos términos de búsqueda. Un término de búsqueda determinado puede ser una sola palabra o una frase delimitada por comillas dobles ("*phrase*").  
  
 Se pueden encadenar varias condiciones de proximidad, como en `a NEAR b NEAR c` o `a ~ b ~ c`. Las condiciones de proximidad encadenadas deben estar todas en el documento para que se devuelva una coincidencia.  
  
 Por ejemplo, `CONTAINS(*column_name*, 'fox NEAR chicken')` y `CONTAINSTABLE(*table_name*, *column_name*, 'fox ~ chicken')` devolverían ambos cualquier documento de la columna especificada que contuviera "fox" y "chicken". Además, CONTAINSTABLE devuelve un rango para cada documento según la proximidad de "zorro" y "pollo". Por ejemplo, si un documento contiene la frase "The fox ate the chicken", su clasificación sería alta porque los términos se encuentran más próximos que en otros documentos.  
  
 Para más información sobre los términos de proximidad genéricos, vea [Buscar palabras cerca de otra palabra con NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<custom_proximity_term>  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.
  
 Especifica una coincidencia entre palabras o frases y, opcionalmente, la distancia máxima permitida entre los términos de búsqueda. También se puede especificar que los términos de búsqueda deben estar en el orden exacto en que se indican (\<match_order>).  
  
 Un término de búsqueda determinado puede ser una sola palabra o una frase delimitada por comillas dobles ("*phrase*"). Todos los términos especificados deben estar en el documento para que se devuelva una coincidencia. Debe especificar dos términos de búsqueda como mínimo. El número máximo de términos de búsqueda es 64.  
  
 De forma predeterminada, el término de proximidad personalizado devuelve todas las filas que contengan los términos especificados, independientemente de la distancia a la que se encuentren y su orden. Por ejemplo, en el caso de la siguiente consulta, un documento tendría que contener `term1` y "`term3 term4`" en cualquier parte y en cualquier orden:  
  
```sql  
CONTAINS(column_name, 'NEAR(term1,"term3 term4")')  
```  
  
 Los parámetros opcionales son los siguientes:  
  
 \<maximum_distance>  
 Especifica la distancia máxima permitida entre los términos de búsqueda inicial y final de una cadena para que la cadena se considere una coincidencia.  
  
 *integer*  
 Especifica un entero positivo de 0 a 4294967295. Este valor controla el número de términos que no son de búsqueda que puede haber entre los términos primero y último, excluyendo cualquier otro término de búsqueda especificado.  
  
 Por ejemplo, la siguiente consulta busca `AA` y `BB`, en cualquier orden, dentro de una distancia máxima de cinco.  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB),5)')  
```  
  
 La cadena `AA one two three four five BB` se consideraría una coincidencia. En el siguiente ejemplo, la consulta especifica tres términos de búsqueda (`AA`, `BB` y `CC`) con una distancia máxima de cinco:  
  
```sql  
CONTAINS(column_name, 'NEAR((AA,BB,CC),5)')  
```  
  
 Esta consulta consideraría como coincidencia la siguiente cadena, en la que la distancia total es cinco:  
  
 `BB   one two   CC   three four five A  A`  
  
 Observe que no se cuenta el término de búsqueda interno, `CC`.  
  
 **MAX**  
 Devuelve todas las filas que contengan los términos especificados sin tener en cuenta la distancia a la que se encuentran. Este es el valor predeterminado.  
  
 \<match_order>  
 Especifica si los términos deben encontrarse en el orden especificado para que los devuelva una consulta de búsqueda. Para especificar \<match_order>, también debe especificar \<maximum_distance>.  
  
 \<match_order> toma uno de los valores siguientes:  
  
 **TRUE**  
 Exige que los términos estén en el orden especificado. Por ejemplo, `NEAR(A,B)` solo consideraría como coincidencia `A ... B`.  
  
 **FALSE**  
 No tiene en cuenta el orden especificado. Por ejemplo, `NEAR(A,B)` consideraría como coincidencias `A ... B` y `B ... A`.  
  
 Este es el valor predeterminado.  
  
 Por ejemplo, el siguiente término de proximidad busca las palabras "`Monday`", "`Tuesday`" y "`Wednesday`" en el orden especificado, sin tener en cuenta la distancia a la que se encuentren:  
  
```sql  
CONTAINS(column_name, 'NEAR ((Monday, Tuesday, Wednesday), MAX, TRUE)')  
```  
  
 Para más información sobre el uso de términos de proximidad personalizados, vea [Buscar palabras cerca de otra palabra con NEAR](../../relational-databases/search/search-for-words-close-to-another-word-with-near.md).  
  
 \<weighted_term>  
 Especifica que las filas coincidentes (devueltas por la consulta) coinciden con una lista de palabras y frases a las que se asigna opcionalmente un valor ponderado.  
  
 ISABOUT  
 Especifica la palabra clave *\<weighted_term>* .  
  
 WEIGHT(*weight_value*)  
 Especifica el valor de ponderación como un número entre 0,0 y 1,0. Cada componente de *\<weighted_term>* puede incluir un elemento *weight_value*. *weight_value* es una forma de modificar cómo varias partes de una consulta afectan al valor de rango asignado a cada fila que coincide con la consulta. WEIGHT no influye en los resultados de las consultas CONTAINS, pero sí en el rango de las consultas [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md).  
  
> [!NOTE]  
> El separador decimal siempre es un punto, independientemente de la configuración regional del sistema operativo.  
  
 { AND \| & } \| { AND NOT \| &! } \| { OR \| \| }  
 Especifica una operación lógica entre dos condiciones de búsqueda.  
  
 { AND \| & }  
 Indica que en ambos casos se incluyen condiciones de búsqueda que se deben cumplir para encontrar coincidencias. Se puede usar el símbolo de "y" comercial (&) en lugar de la palabra clave AND para representar el operador AND.  
  
 { AND NOT \| &! }  
 Indica que la segunda condición de búsqueda no puede estar presente para encontrar coincidencias. Se puede usar el símbolo de "y" comercial seguido del signo de admiración (&!) en lugar de la palabra clave AND NOT para representar el operador AND NOT.  
  
 { OR \| \| }  
 Indica que en uno de los dos casos se incluyen condiciones de búsqueda que se deben cumplir para encontrar coincidencias. Se puede utilizar el símbolo de barra (|) en lugar de la palabra clave OR para representar el operador OR.  
  
 Cuando *\<contains_search_condition>* contiene grupos entre paréntesis, estos se evalúan primero. Después de evaluar los grupos entre paréntesis, se aplican las reglas siguientes cuando se utilizan estos operadores lógicos con condiciones de búsqueda:  
  
-   NOT se aplica antes que AND.  
  
-   NOT solo puede estar a continuación de AND, como en AND NOT. No se acepta el operador OR NOT. NOT no se puede especificar antes del primer término. Por ejemplo, `CONTAINS (mycolumn, 'NOT "phrase_to_search_for" ' )` no es válido.  
  
-   AND se aplica antes que OR.  
  
-   Los operadores booleanos del mismo tipo (AND, OR) son asociativos y, por tanto, se pueden aplicar en cualquier orden.  
  
 *n*  
 Es un marcador de posición que indica que se pueden especificar varias condiciones de búsqueda de CONTAINS y términos dentro de ellas.  
  
## <a name="general-remarks"></a>Notas generales  
 Los predicados y las funciones de texto completo operan en una única tabla, que se obtiene del predicado FROM. Para buscar en varias tablas, utilice una tabla combinada en la cláusula FROM a fin de buscar en un conjunto de resultados que sea el producto de dos o más tablas.  
  
 Los predicados de texto completo no se pueden usar en la [cláusula OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) cuando el nivel de compatibilidad de la base de datos está establecido en 100.  
  
## <a name="querying-remote-servers"></a>Consultar servidores remotos  
 Se puede usar un nombre de cuatro partes en el predicado CONTAINS o [FREETEXT](../../t-sql/queries/freetext-transact-sql.md) para consultar columnas indizadas de texto completo de las tablas de destino en un servidor vinculado. Para preparar un servidor remoto para recibir consultas de texto completo, es necesario crear un índice de texto completo en las tablas y columnas de destino en el servidor remoto y, posteriormente, agregar el servidor remoto como un servidor vinculado.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Comparación de LIKE con la búsqueda de texto completo  
 A diferencia de la búsqueda de texto completo, el predicado [LIKE](../../t-sql/language-elements/like-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] funciona solamente en patrones de caracteres. Además, no es posible utilizar el predicado de LIKE para consultar datos binarios con formato. Por otro lado, una consulta LIKE contra una cantidad grande de datos de texto no estructurados es mucho más lenta que una consulta de texto completo equivalente contra los mismos datos. Una consulta LIKE realizada en millones de filas de datos de texto puede tardar minutos en devolver resultados, mientras que una consulta de texto completo en los mismos datos puede tardar únicamente segundos, en función del número de filas que se devuelvan y su tamaño. También hay que tener en cuenta que LIKE realiza solo un análisis de patrón simple de toda una tabla. Por el contrario, una consulta de texto completo reconoce el lenguaje y aplica transformaciones concretas al realizar el índice y la consulta. Por ejemplo, filtra las palabras irrelevantes y amplía el diccionario de sinónimos y las inflexiones. Estas transformaciones contribuyen a mejorar los resultados de las consultas de texto completo y su orden final.  
  
## <a name="querying-multiple-columns-full-text-search"></a>Consultar varias columnas (búsqueda de texto completo)  
 Puede consultar varias columnas especificando una lista de columnas en las que realizar la búsqueda. Las columnas deben ser de la misma tabla.  
  
 Por ejemplo, la siguiente consulta CONTAINS busca el término `Red` en las columnas `Name` y `Color` de la tabla `Production.Product` de la base de datos de ejemplo [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Name, Color   
FROM Production.Product  
WHERE CONTAINS((Name, Color), 'Red');  
```  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-contains-with-simple_term"></a>A. Uso de CONTAINS con \<simple_term>  
 En este ejemplo se buscan todos los productos con un precio de `$80.99` que contengan la palabra `Mountain`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name, ListPrice  
FROM Production.Product  
WHERE ListPrice = 80.99  
   AND CONTAINS(Name, 'Mountain');  
GO  
```  
  
### <a name="b-using-contains-and-phrase-with-simple_term"></a>B. Uso de CONTAINS y una frase con \<simple_term>  
 En el siguiente ejemplo se obtienen todos los productos que contienen la palabra `Mountain` o `Road`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' Mountain OR Road ')  
GO  
```  
  
### <a name="c-using-contains-with-prefix_term"></a>C. Uso de CONTAINS con \<prefix_term>  
 En el siguiente ejemplo se obtienen todos los nombres de producto con una palabra como mínimo que empiece por el prefijo "chain" en la columna `Name`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, ' "Chain*" ');  
GO  
```  
  
### <a name="d-using-contains-and-or-with-prefix_term"></a>D. Uso de CONTAINS y OR con \<prefix_term>  
 En el siguiente ejemplo se obtienen todas las descripciones de categorías que contienen cadenas con los prefijos `chain` o `full`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Name  
FROM Production.Product  
WHERE CONTAINS(Name, '"chain*" OR "full*"');  
GO  
```  
  
### <a name="e-using-contains-with-proximity_term"></a>E. Uso de CONTAINS con \<proximity_term>  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores. 
  
 En el siguiente ejemplo se buscan en la tabla `Production.ProductReview` todos los comentarios que contengan la palabra `bike` a una distancia de diez términos de la palabra "`control`" y en el orden especificado (es decir, donde "`bike`" preceda a "`control`").  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Comments  
FROM Production.ProductReview  
WHERE CONTAINS(Comments , 'NEAR((bike,control), 10, TRUE)');  
GO  
```  
  
### <a name="f-using-contains-with-generation_term"></a>F. Uso de CONTAINS con \<generation_term>  
 En el siguiente ejemplo se buscan todos los productos que tengan palabras derivadas de `ride`: riding, ridden, etc.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, ' FORMSOF (INFLECTIONAL, ride) ');  
GO  
```  
  
### <a name="g-using-contains-with-weighted_term"></a>G. Uso de CONTAINS con \<weighted_term>  
 En el siguiente ejemplo se buscan todos los nombres de productos que contengan las palabras `performance`, `comfortable` o `smooth`. Cada palabra tiene asignado un peso distinto.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE CONTAINS(Description, 'ISABOUT (performance weight (.8),   
comfortable weight (.4), smooth weight (.2) )' );  
GO  
```  
  
### <a name="h-using-contains-with-variables"></a>H. Usar CONTAINS con variables  
 En el siguiente ejemplo se utiliza una variable en lugar de un término de búsqueda específico.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'Performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE CONTAINS(Description, @SearchWord);  
GO  
```  
  
### <a name="i-using-contains-with-a-logical-operator-and"></a>I. Usar CONTAINS con un operador lógico (AND)  
 En el ejemplo siguiente se utiliza la tabla ProductDescription de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] . La consulta usa el predicado CONTAINS para buscar las descripciones en las que el identificador de la descripción no es igual a 5 y la descripción contiene las palabras `Aluminum` y `spindle`. La condición de búsqueda usa el operador booleano AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Description  
FROM Production.ProductDescription  
WHERE ProductDescriptionID <> 5 AND  
   CONTAINS(Description, 'Aluminum AND spindle');  
GO  
```  
  
### <a name="j-using-contains-to-verify-a-row-insertion"></a>J. Usar CONTAINS para comprobar una inserción de fila  
 En el ejemplo siguiente se usa CONTAINS dentro de una subconsulta SELECT. Si se utiliza la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], la consulta obtiene el valor de comentario de todos los comentarios de la tabla ProductReview en un determinado ciclo. La condición de búsqueda usa el operador booleano AND.  
  
```sql  
USE AdventureWorks2012;  
GO  
INSERT INTO Production.ProductReview   
  (ProductID, ReviewerName, EmailAddress, Rating, Comments)   
VALUES  
  (780, 'John Smith', 'john@fourthcoffee.com', 5,   
'The Mountain-200 Silver from AdventureWorks2008 Cycles meets and exceeds expectations. I enjoyed the smooth ride down the roads of Redmond');  
  
-- Given the full-text catalog for these tables is Adv_ft_ctlg,   
-- with change_tracking on so that the full-text indexes are updated automatically.  
WAITFOR DELAY '00:00:30';     
-- Wait 30 seconds to make sure that the full-text index gets updated.  
  
SELECT r.Comments, p.Name  
FROM Production.ProductReview AS r  
JOIN Production.Product AS p   
    ON r.ProductID = p.ProductID  
    AND r.ProductID = (SELECT ProductID  
FROM Production.ProductReview  
WHERE CONTAINS (Comments,   
    ' AdventureWorks2008 AND   
    Redmond AND   
    "Mountain-200 Silver" '));  
GO  
```  
  
### <a name="k-querying-on-a-document-property"></a>K. Realizar consultas por una propiedad de documento  
  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores. 
  
 La siguiente consulta busca una propiedad indizada, `Title`, en la columna `Document` de la tabla `Production.Document`. La consulta solo devuelve documentos cuya propiedad `Title` contengan las cadenas `Maintenance` o `Repair`.  
  
> [!NOTE]  
>  Para que una búsqueda por propiedad devuelva filas, los filtros que analizan la columna durante la indización deben extraer la propiedad especificada. Asimismo, el índice de texto completo de la tabla especificada debe estar configurado de modo que contenga la propiedad. Para obtener más información, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
```sql  
Use AdventureWorks2012;  
GO  
SELECT Document 
FROM Production.Document  
WHERE CONTAINS(PROPERTY(Document,'Title'), 'Maintenance OR Repair');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Introducción a la búsqueda de texto completo](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Creación y administración de catálogos de texto completo](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Crear y administrar índices de texto completo](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)   
 [Crear consultas de búsqueda de texto completo &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
