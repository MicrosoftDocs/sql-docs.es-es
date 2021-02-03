---
description: FREETEXT (Transact-SQL)
title: FREETEXT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FREETEXT
- FREETEXT_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], meaning matches
- meaning matches [full-text search]
- FREETEXT predicate (Transact-SQL)
- words in predicate [full-text search]
- column searches [full-text search]
ms.assetid: 2f199d3c-440e-4bcf-bdb5-82bb3994005d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 927f11ff77a50a3a7b7a04983fa197510e88232f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207744"
---
# <a name="freetext-transact-sql"></a>FREETEXT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Es un predicado que se usa en la [cláusula WHERE](../../t-sql/queries/where-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] de una instrucción SELECT de [!INCLUDE[tsql](../../includes/tsql-md.md)] para realizar una búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en las columnas indexadas de texto completo que contienen tipos de datos basados en caracteres. Este predicado busca valores que coincidan con el significado y no solo con la redacción exacta de las palabras en la condición de búsqueda. Cuando se usa FREETEXT, el motor de consultas de texto completo realiza internamente las siguientes acciones en *cadena_freetext*, asigna a cada uno de los términos una ponderación y busca las coincidencias:  
  
-   Separa la cadena en palabras individuales basándose en límites de palabras (separación de palabras).  
  
-   Genera formas no flexionadas de las palabras (lematización).  
  
-   Identifica una lista de expansiones o reemplazos de los términos basándose en coincidencias en el diccionario de sinónimos.  
  
> [!NOTE]  
>  Para obtener información sobre las formas de búsqueda de texto completo que se admiten en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Consulta con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md).  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FREETEXT ( { column_name | (column_list) | * }   
          , 'freetext_string' [ , LANGUAGE language_term ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *column_name*  
 Es el nombre de una o varias columnas indizadas de texto completo de la tabla especificada en la cláusula FROM. Las columnas pueden ser de tipo **char**, **varchar**, **nchar**, **nvarchar**, **text**, **ntext**, **image**, **xml**, **varbinary** o **varbinary(max)** .  
  
 *lista_de_columnas*  
 Indica que se pueden especificar varias columnas, separadas por una coma. *column_list* debe ir entre paréntesis. A menos que se especifique *language_term*, el idioma de todas las columnas de *column_list* debe ser el mismo.  
  
 \*  
 Especifica que todas las columnas que se hayan registrado para la búsqueda de texto completo se tienen que usar para buscar la *cadena_freetext* determinada. Si en la cláusula FROM hay más de una tabla, \* se tiene que especificar con el nombre de la tabla. A menos que se especifique *language_term*, el idioma de todas las columnas de la tabla debe ser el mismo.  
  
 *cadena_freetext*  
 Es el texto que se va a buscar en *column_name*. Se puede escribir cualquier texto, incluidas palabras, frases y oraciones. Se generarán coincidencias si se encuentra algún término o las formas de algún término en el índice de texto completo.  
  
 A diferencia de lo que sucede en la condición de búsqueda CONTAINS y CONTAINSTABLE, donde AND es una palabra clave, cuando la palabra "and" se usa en *cadena_freetext*, se considera una [palabra irrelevante](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) y no se tiene en cuenta.  
  
 No se permite el uso de WEIGHT, FORMSOF, caracteres comodín, NEAR y otros elementos sintácticos. *cadena_freetext* se separa en palabras, de las que se extraen las desinencias y se pasan por el diccionario de sinónimos.  
  
 *cadena_freetext* es de tipo **nvarchar**. Se realiza una conversión implícita cuando se usa otro tipo de datos de carácter como entrada. No se pueden usar los tipos de datos de cadena grande varchar(max) y nvarchar(max). En el siguiente ejemplo, la variable `@SearchWord`, definida como una variable de tipo `varchar(30)`, provoca una conversión implícita en el predicado `FREETEXT`.  
  
```sql
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord VARCHAR(30)  
SET @SearchWord ='performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 Como el "examen de parámetros" no funciona con la conversión, use **nvarchar** para lograr un mejor rendimiento. En el ejemplo, declare `@SearchWord` como `nvarchar(30)`.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30)  
SET @SearchWord = N'performance'  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
  
```  
  
 También puede usar la sugerencia de consulta OPTIMIZE FOR para los casos en que se genera un plan que no es óptimo.  
  
 LANGUAGE *language_term*  
 Es el idioma cuyos recursos se utilizarán en la separación de palabras, la lematización, los diccionarios de sinónimos y la eliminación de palabras irrelevantes como parte de la consulta. Este parámetro es opcional y puede especificarse como un valor hexadecimal, un entero o una cadena correspondiente al identificador de configuración regional (LCID) de un idioma. Si se especifica *language_term*, el idioma que representa se aplicará a todos los elementos de la condición de búsqueda. Si no se especifica ningún valor, se utiliza el idioma de texto completo de la columna.  
  
 Si se almacenan juntos documentos de idiomas diferentes como objetos binarios grandes (BLOB) en una sola columna, el identificador de configuración regional (LCID) de un documento determinado determina qué idioma se usa para indizar su contenido. Al consultar este tipo de columna, especificar LANGUAGE *language_term* puede aumentar la probabilidad de encontrar una coincidencia acertada.  
  
 Cuando se especifica como una cadena, *language_term* corresponde al valor de columna **alias** de la vista de compatibilidad [sys.syslanguages &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md).  La cadena debe estar delimitada con comillas sencillas, como en '*language_term*'. Cuando se especifica como un entero, *language_term* es el LCID real que identifica el idioma. Cuando se especifica como un valor hexadecimal, *language_term* es 0x seguido del valor hexadecimal del LCID. El valor hexadecimal no puede superar los ocho dígitos, incluidos los ceros a la izquierda.  
  
 Si el valor está en formato DBCS (juego de caracteres de doble byte), [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] lo convertirá a Unicode.  
  
 Si el idioma especificado no es válido o no hay recursos instalados que se correspondan con dicho idioma, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve un error. Para usar recursos de idioma neutro, especifique 0x0 como *language_term*.  
  
## <a name="general-remarks"></a>Notas generales  
 Los predicados y las funciones de texto completo operan en una única tabla, que se obtiene del predicado FROM. Para buscar en varias tablas, utilice una tabla combinada en la cláusula FROM a fin de buscar en un conjunto de resultados que sea el producto de dos o más tablas.  
  
Las consultas de texto completo que utilizan FREETEXT son menos precisas que las consultas de texto completo que utilizan CONTAINS. El motor de búsqueda de texto completo de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifica las palabras y las frases importantes. No se le da significado especial a ninguna de las palabras clave reservadas o caracteres comodín que suelen tener significado cuando se especifican en el parámetro \<contains_search_condition> del predicado CONTAINS.
  
 Los predicados de texto completo no se pueden usar en la [cláusula OUTPUT](../../t-sql/queries/output-clause-transact-sql.md) cuando el nivel de compatibilidad de la base de datos está establecido en 100.  
  
> [!NOTE]  
>  La función FREETEXTTABLE es útil para los mismos tipos de coincidencias que el predicado FREETEXT. Se puede hacer referencia a esta función como un nombre de tabla normal en la [cláusula FROM](../../t-sql/queries/from-transact-sql.md) de una instrucción SELECT. Para obtener más información, vea [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="querying-remote-servers"></a>Consultar servidores remotos  
 Se puede usar un nombre de cuatro elementos en el predicado [CONTAINS](../../t-sql/queries/contains-transact-sql.md) o FREETEXT para consultar columnas indexadas de texto completo de las tablas de destino en un servidor vinculado. Para preparar un servidor remoto para recibir consultas de texto completo, es necesario crear un índice de texto completo en las tablas y columnas de destino en el servidor remoto y, posteriormente, agregar el servidor remoto como un servidor vinculado.  
  
## <a name="comparison-of-like-to-full-text-search"></a>Comparación de LIKE con la búsqueda de texto completo  
 A diferencia de la búsqueda de texto completo, el predicado [LIKE](../../t-sql/language-elements/like-transact-sql.md) de [!INCLUDE[tsql](../../includes/tsql-md.md)] funciona solamente en patrones de caracteres. Además, no es posible utilizar el predicado de LIKE para consultar datos binarios con formato. Por otro lado, una consulta LIKE contra una cantidad grande de datos de texto no estructurados es mucho más lenta que una consulta de texto completo equivalente contra los mismos datos. Una consulta LIKE realizada en millones de filas de datos de texto puede tardar minutos en devolver resultados, mientras que una consulta de texto completo en los mismos datos puede tardar únicamente segundos, en función del número de filas que se devuelvan.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-freetext-to-search-for-words-containing-specified-character-values"></a>A. Usar FREETEXT para buscar palabras que contengan los valores de carácter especificados  
 En el siguiente ejemplo se buscan todos los documentos que contienen las palabras relacionadas con vital, safety, components.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT Title  
FROM Production.Document  
WHERE FREETEXT (Document, 'vital safety components' );  
GO  
```  
  
### <a name="b-using-freetext-with-variables"></a>B. Usar FREETEXT con variables  
 En el siguiente ejemplo se utiliza una variable en lugar de un término de búsqueda específico.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SearchWord NVARCHAR(30);  
SET @SearchWord = N'high-performance';  
SELECT Description   
FROM Production.ProductDescription   
WHERE FREETEXT(Description, @SearchWord);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Introducción a la búsqueda de texto completo](../../relational-databases/search/get-started-with-full-text-search.md)   
 [Creación y administración de catálogos de texto completo](../../relational-databases/search/create-and-manage-full-text-catalogs.md)   
 [CREATE FULLTEXT CATALOG &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-catalog-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [Crear y administrar índices de texto completo](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [Crear consultas de búsqueda de texto completo &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/create-full-text-search-queries-visual-database-tools.md)   
 [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
