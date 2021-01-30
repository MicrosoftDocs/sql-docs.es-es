---
description: sys.dm_fts_parser (Transact-SQL)
title: sys.dm_fts_parser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_parser_TSQL
- dm_fts_parser
- dm_fts_parser_TSQL
- sys.dm_fts_parser
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_parser dynamic management function
- troubleshooting [SQL Server], full-text search
ms.assetid: 2736d376-fb9d-4b28-93ef-472b7a27623a
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: ff5dac01f1fa48db60a384b8cc8bfb2216a5bab5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184322"
---
# <a name="sysdm_fts_parser-transact-sql"></a>sys.dm_fts_parser (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve el resultado de la tokenización final después de aplicar una combinación determinada de separador de [palabras](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md), [Diccionario de sinónimos](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)y lista de [palabras irrelevantes](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) a una entrada de cadena de consulta. El resultado de la tokenización es equivalente al que produce el motor de búsqueda de texto completo para la cadena de consulta especificada.  
  
 sys.dm_fts_parser es una función de administración dinámica.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_fts_parser('query_string', lcid, stoplist_id, accent_sensitivity)  
```  
  
## <a name="arguments"></a>Argumentos  
 *query_string*  
 Consulta que se desea analizar. *query_string* puede ser una cadena de cadenas que [contenga](../../t-sql/queries/contains-transact-sql.md) compatibilidad con la sintaxis. Por ejemplo, se pueden incluir formas con inflexión, un diccionario de sinónimos y operadores lógicos.  
  
 *lcid*  
 Identificador de configuración regional (LCID) del separador de palabras que se va a utilizar para analizar *query_string*.  
  
 *stoplist_id*  
 IDENTIFICADOR de la lista de palabras irrelevantes, si existe, que va a usar el separador de palabras identificado por *LCID*. *stoplist_id* es de **tipo int**. Si especifica ' NULL ', no se utiliza ninguna lista de palabras irrelevantes. Si se especifica 0, se utiliza la lista de palabras irrelevantes del sistema.  
  
 Un identificador de lista de palabras irrelevantes es único dentro de una base de datos. Para obtener el identificador de la lista de palabras irrelevantes de un índice de texto completo en una tabla determinada, use la vista de catálogo [Sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) .  
  
 *accent_sensitivity*  
 Valor booleano que controla si la búsqueda de texto completo distingue o no los signos diacríticos. *accent_sensitivity* es de **bits**, con uno de los siguientes valores:  
  
|Value|La distinción de acentos es...|  
|-----------|----------------------------|  
|0|Sin distinción<br /><br /> Palabras como "café" y "cafe" se tratan de forma idéntica.|  
|1|Sensible<br /><br /> Palabras como "café" y "cafe" se tratan de forma diferente.|  
  
> [!NOTE]  
>  Para ver la configuración actual de este valor para un catálogo de texto completo, ejecute la siguiente [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción: `SELECT fulltextcatalogproperty('` *catalog_name* `', 'AccentSensitivity');` .  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|palabra clave|**varbinary(128)**|Representación hexadecimal de una palabra clave determinada devuelta por un separador de palabras. Esta representación se utiliza para almacenar la palabra clave en el índice de texto completo. Este valor no es legible para el usuario, pero es útil para relacionar una palabra clave determinada con la salida devuelta por otras vistas de administración dinámica que devuelven el contenido de un índice de texto completo, como [Sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md) y [Sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md).<br /><br /> **Nota:** OxFF representa el carácter especial que indica el final de un archivo o conjunto de archivos.|  
|group_id|**int**|Contiene un valor entero que es útil para diferenciar el grupo lógico a partir del cual se generó un término determinado. Por ejemplo, '`Server AND DB OR FORMSOF(THESAURUS, DB)"`' genera los valores de group_id siguientes en inglés:<br /><br /> 1: servidor<br />2: BASE DE<br />3: BASE DE|  
|phrase_id|**int**|Contiene un valor entero que resulta útil para diferenciar los casos en los que el separador de palabras emite formatos alternativos de palabras compuestas, tales como texto completo. A veces, con la presencia de palabras compuestas ('multi-million') el separador de palabras emite formatos alternativos. En ocasiones, es necesario diferenciar estos formatos alternativos (frases).<br /><br /> Por ejemplo, '`multi-million`' genera los valores de phrase_id siguientes en inglés:<br /><br /> 1 para `multi`<br />1 para `million`<br />2 para `multimillion`|  
|occurrence|**int**|Indica el orden de cada término en el resultado del análisis. Por ejemplo, para la repetición de la frase "`SQL Server query processor`", contendría los valores de repetición siguientes para los términos de la frase, en inglés:<br /><br /> 1 para `SQL`<br />2 para `Server`<br />3 para `query`<br />4 para `processor`|  
|special_term|**nvarchar(4000)**|Contiene información sobre las características del término que el separador de palabras emite; por ejemplo:<br /><br /> Coincidencia exacta<br /><br /> Palabra irrelevante<br /><br /> Fin de frase<br /><br /> Fin de párrafo<br /><br /> Fin de capítulo|  
|display_term|**nvarchar(4000)**|Contiene el formato legible de la palabra clave. Al igual que con las funciones diseñadas para tener acceso al contenido del índice de texto completo, este término mostrado podría no ser idéntico al original, debido a la limitación de la desnormalización. Sin embargo, debería ser suficientemente preciso para ayudar a identificarlo con respecto a la entrada original.|  
|expansion_type|**int**|Contiene información sobre la naturaleza de la expansión de un término determinado; a saber:<br /><br /> 0 =caso de una sola palabra<br /><br /> 2=expansión con inflexión<br /><br /> 4=expansión o sustitución de diccionario de sinónimos<br /><br /> Por ejemplo, considere un caso en que el diccionario de sinónimos define run como una expansión de `jog`:<br /><br /> `<expansion>`<br /><br /> `<sub>run</sub>`<br /><br /> `<sub>jog</sub>`<br /><br /> `</expansion>`<br /><br /> El término `FORMSOF (FREETEXT, run)` genera la salida siguiente:<br /><br /> `run` con expansion_type=0<br /><br /> `runs` con expansion_type=2<br /><br /> `running` con expansion_type=2<br /><br /> `ran` con expansion_type=2<br /><br /> `jog` con expansion_type=4|  
|source_term|**nvarchar(4000)**|Término o frase a partir de la cual se generó o analizó un término determinado. Por ejemplo, una consulta de '"`word breakers" AND stemmers'` genera los valores de source_term siguientes en inglés:<br /><br /> `word breakers` para el display_term`word`<br />`word breakers` para el display_term`breakers`<br />`stemmers` para el display_term`stemmers`|  
  
## <a name="remarks"></a>Observaciones  
 **Sys.dm_fts_parser** admite la sintaxis y las características de los predicados de texto completo, como [Contains](../../t-sql/queries/contains-transact-sql.md) y [FREETEXT](../../t-sql/queries/freetext-transact-sql.md), y las funciones, como [CONTAINSTABLE](../../relational-databases/system-functions/containstable-transact-sql.md) y [FREETEXTTABLE](../../relational-databases/system-functions/freetexttable-transact-sql.md).  
  
## <a name="using-unicode-for-parsing-special-characters"></a>Usar Unicode para analizar caracteres especiales  
 Cuando se analiza una cadena de consulta, **Sys.dm_fts_parser** usa la intercalación de la base de datos a la que está conectado, a menos que se especifique la cadena de consulta como Unicode. Por lo tanto, para una cadena no Unicode que contenga caracteres especiales, como ü o ç, la salida podría ser inesperada, dependiendo de la intercalación de la base de datos. Para procesar una cadena de consulta independientemente de la intercalación de la base de datos, anteponga a la cadena `N` , es decir, `N'` *query_string* `'` .  
  
 Para obtener más información, vea "C. Mostrar el resultado de una cadena que contiene caracteres especiales", más adelante en este tema.  
  
## <a name="when-to-use-sysdm_fts_parser"></a>Cuándo usar sys.dm_fts_parser  
 **Sys.dm_fts_parser** puede ser muy eficaz para la depuración. Algunos de los escenarios de uso principales son:  
  
-   Entender cómo trata un separador de palabras determinado una entrada dada  
  
     Cuando una consulta devuelve resultados inesperados, una causa probable es la manera en que el separador de palabras está analizando y dividiendo los datos. Mediante sys.dm_fts_parser, se puede saber el resultado que un separador de palabras pasa al índice de texto completo. Además, se puede ver qué términos son palabras irrelevantes, que no se buscan en el índice de texto completo. El hecho de que un término sea un palabra irrelevante para un idioma determinado depende de si está en la lista de palabras irrelevantes especificada por el valor *stoplist_id* que se declara en la función.  
  
     Tenga en cuenta también la marca de distinción de acentos, que permitirá al usuario ver cómo el separador de palabras analizará la entrada teniendo en cuenta esta información.  
  
-   Entender cómo funciona el lematizador en una entrada determinada  
  
     Puede averiguar cómo el separador de palabras y el lematizador analizan un término de la consulta y sus formas de lematización especificando una consulta CONTAINS o CONTAINSTABLE que contenga la cláusula FORMSOF siguiente:  
  
    ```  
    FORMSOF( INFLECTIONAL, query_term )  
    ```  
  
     Los resultados indican qué términos se pasan al índice de texto completo.  
  
-   Entender cómo el diccionario de sinónimos expande o reemplaza la totalidad o una parte de la entrada  
  
     También puede especificar:  
  
    ```  
    FORMSOF( THESAURUS, query_term )  
    ```  
  
     Los resultados de esta consulta muestran cómo interactúan el separador de palabras y el diccionario de sinónimos para el término de la consulta. Puede ver la expansión o los reemplazos del diccionario de sinónimos, e identificar la consulta resultante que se emite realmente frente al índice de texto completo.  
  
     Tenga en cuenta que si el usuario emite  
  
    ```  
    FORMSOF( FREETEXT, query_term )  
    ```  
  
     Las funciones de identificación de formas flexivas y diccionario de sinónimos se ejecutarán automáticamente.  
  
 Además de los escenarios de uso anteriores, sys.dm_fts_parser puede ayudar significativamente a entender y solucionar muchos otros problemas de la consulta de texto completo.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **sysadmin** y a los derechos de acceso a la lista de palabras irrelevantes especificada.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-displaying-the-output-of-a-given-word-breaker-for-a-keyword-or-phrase"></a>A. Mostrar el resultado de un separador de palabras determinado para una palabra clave o una frase  
 En el ejemplo siguiente se devuelve el resultado obtenido cuando se usa el separador de palabras en inglés, cuyo LCID es 1033, y no se utiliza ninguna lista de palabras irrelevantes en la cadena de consulta siguiente:  
  
 `The Microsoft business analysis`  
  
 La distinción de acentos está deshabilitada.  
  
```sql
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis" ', 1033, 0, 0);  
```  
  
### <a name="b-displaying-the-output-of-a-given-word-breaker-in-the-context-of-stoplist-filtering"></a>B. Mostrar el resultado de un separador de palabras determinado en el contexto de filtrado de la lista de palabras irrelevantes  
 En el ejemplo siguiente se devuelve el resultado de utilizar el separador de palabras en inglés, cuyo LCID es 1033, y una lista de palabras irrelevantes en inglés, cuyo identificador es 77, en la cadena de consulta siguiente:  
  
 `"The Microsoft business analysis" OR "MS revenue"`  
  
 La distinción de acentos está deshabilitada.  
  
```sql
SELECT * FROM sys.dm_fts_parser (' "The Microsoft business analysis"  OR " MS revenue" ', 1033, 77, 0);  
```  
  
### <a name="c-displaying-the-output-of-a-string-that-contains-special-characters"></a>C. Mostrar el resultado de una cadena que contiene caracteres especiales  
 En el ejemplo siguiente, se usa Unicode para analizar la cadena francesa siguiente:  
  
 `français`  
  
 En el ejemplo, se especifica el LCID para el idioma francés, `1036` y el identificador de una lista de palabras irrelevantes definida por el usuario, `5`. La distinción de acentos está habilitada.  
  
```sql
SELECT * FROM sys.dm_fts_parser(N'français', 1036, 5, 1);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)   
 [Configurar y administrar separadores de palabras y lematizadores para la búsqueda](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md)   
 [Configurar y administrar archivos de sinónimos para búsquedas de Full-Text](../../relational-databases/search/configure-and-manage-thesaurus-files-for-full-text-search.md)   
 [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [Consultar con búsqueda de texto completo](../../relational-databases/search/query-with-full-text-search.md)   
 [Elementos protegibles](../../relational-databases/security/securables.md)  
  
  
