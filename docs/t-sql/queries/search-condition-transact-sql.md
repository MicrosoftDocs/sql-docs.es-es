---
description: Condiciones de búsqueda (Transact-SQL)
title: Condiciones de búsqueda (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/15/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- search
- Search Condition
- condition
dev_langs:
- TSQL
helpviewer_keywords:
- OR operator [Transact-SQL]
- CONTAINS predicate (Transact-SQL)
- ESCAPE keyword
- operators [Transact-SQL], search condition
- search conditions [SQL Server]
- WHERE clause, search conditions
- ALL keyword
- FREETEXT predicate (Transact-SQL)
- EXISTS keyword
- search conditions [SQL Server], about search conditions
- NOT operator [Transact-SQL]
- BETWEEN operator
- SOME | ANY keyword
- predicates [full-text search]
- AND, about search conditions
- logical operators [SQL Server], about search conditions
- precedence [SQL Server], logical operators
- logical operators [SQL Server], precedence
- LIKE comparisons
ms.assetid: 09974469-c5d2-4be8-bc5a-78e404660b2c
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 09a53b0a4e227a4fd11ae0fa3ae82855b5f57524
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207624"
---
# <a name="search-condition-transact-sql"></a>Condiciones de búsqueda (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Es una combinación de uno o varios predicados que utilizan los operadores lógicos AND, OR y NOT.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
<search_condition> ::=  
    MATCH(<graph_search_pattern>) | <search_condition_without_match> | <search_condition> AND <search_condition>

<search_condition_without_match> ::= 
    { [ NOT ] <predicate> | ( <search_condition_without_match> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition_without_match> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | ! > | < | < = | ! < } expression   
    | string_expression [ NOT ] LIKE string_expression   
  [ ESCAPE 'escape_character' ]   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | CONTAINS   
  ( { column | * } , '<contains_search_condition>' )   
    | FREETEXT ( { column | * } , 'freetext_string' )   
    | expression [ NOT ] IN ( subquery | expression [ ,...n ] )   
    | expression { = | < > | ! = | > | > = | ! > | < | < = | ! < }   
  { ALL | SOME | ANY} ( subquery )   
    | EXISTS ( subquery )     }   
    
<graph_search_pattern> ::=
    { <node_alias> { 
                      { <-( <edge_alias> )- } 
                    | { -( <edge_alias> )-> }
                    <node_alias> 
                   } 
    }
  
<node_alias> ::=
    node_table_name | node_table_alias 

<edge_alias> ::=
    edge_table_name | edge_table_alias
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
< search_condition > ::=   
    { [ NOT ] <predicate> | ( <search_condition> ) }   
    [ { AND | OR } [ NOT ] { <predicate> | ( <search_condition> ) } ]   
[ ...n ]   
  
<predicate> ::=   
    { expression { = | < > | ! = | > | > = | < | < = } expression   
    | string_expression [ NOT ] LIKE string_expression   
    | expression [ NOT ] BETWEEN expression AND expression   
    | expression IS [ NOT ] NULL   
    | expression [ NOT ] IN (subquery | expression [ ,...n ] )   
    | expression [ NOT ] EXISTS (subquery)     }   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 \<search_condition>  
 Especifica las condiciones de las filas devueltas en el conjunto de resultados de una instrucción SELECT, una expresión de consulta o una subconsulta. En una instrucción UPDATE, especifica las filas que se van a actualizar. En una instrucción DELETE, especifica las filas que se van a eliminar. No hay límite en el número de predicados que se pueden incluir en una condición de búsqueda de una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 \<graph_search_pattern>  
 Especifica el patrón de coincidencia de gráficos. Para obtener más información sobre los argumentos de esta cláusula, vea [MATCH &#40;Transact-SQL&#41;](../../t-sql/queries/match-sql-graph.md)
 
 NOT  
 Niega la expresión booleana que especifica el predicado. Para obtener más información, vea [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
 y  
 Combina dos condiciones y se evalúa como TRUE cuando ambas condiciones son TRUE. Para obtener más información, vea [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md).  
  
 O BIEN  
 Combina dos condiciones y se evalúa como TRUE cuando alguna de las condiciones es TRUE. Para obtener más información, vea [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md).  
  
 \< predicate >  
 Es una expresión que devuelve TRUE, FALSE o UNKNOWN.  
  
 *expression*  
 Es un nombre de columna, una constante, una función, una variable, una subconsulta escalar o cualquier combinación de nombres de columna, constantes y funciones conectados mediante uno o varios operadores o una subconsulta. La expresión también puede contener la expresión CASE.  
  
> [!NOTE]  
>  Las constantes y las variables de cadena no Unicode usan la página de códigos que corresponde a la intercalación predeterminada de la base de datos. Pueden producirse conversiones de página de códigos cuando se trabaja únicamente con datos de caracteres no Unicode y se hace referencia a los tipos de datos de caracteres no Unicode **char**, **varchar** y **text**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] convierte las variables y constantes de cadena no Unicode a la página de códigos que corresponde a la intercalación de la columna a la que se hace referencia o que se ha especificado mediante COLLATE, si esa página de códigos es diferente a la que corresponde a la intercalación predeterminada de la base de datos. Los caracteres que no se encuentran en la nueva página de códigos se traducen en un carácter similar, si se encuentra una [coincidencia](https://www.unicode.org/Public/MAPPINGS/VENDORS/MICSFT/WindowsBestFit/), o bien se convierten en el carácter de sustitución predeterminado "?".  
>  
> Cuando se trabaja con varias páginas de códigos, las constantes de caracteres pueden llevar un prefijo con la letra mayúscula 'N' y se pueden usar variables Unicode para evitar las conversiones de páginas de códigos.  
  
 =  
 Es el operador que se utiliza para probar la igualdad entre dos expresiones.  
  
 <>  
 Es el operador que se utiliza para probar si dos expresiones no son iguales entre sí.  
  
 !=  
 Es el operador que se utiliza para probar si dos expresiones no son iguales entre sí.  
  
 \>  
 Es el operador que se utiliza para probar si una expresión es mayor que la otra.  
  
 \>=  
 Es el operador que se utiliza para probar si una expresión es mayor o igual que la otra expresión.  
  
 !>  
 Es el operador que se utiliza para probar si una expresión no es mayor que la otra expresión.  
  
 <  
 Es el operador que se utiliza para probar si una expresión es menor que la otra.  
  
 <=  
 Es el operador que se utiliza para probar si una expresión es menor o igual que la otra expresión.  
  
 !<  
 Es el operador que se utiliza para probar si una expresión no es menor que la otra expresión.  
  
 *string_expression*  
 Es una cadena de caracteres y caracteres comodín.  
  
 [ NOT ] LIKE  
 Indica que la siguiente cadena de caracteres se utilizará con la coincidencia de patrón. Para obtener más información, vea [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md).  
  
 ESCAPE **"** _carácter_ de escape_ *_"_*  
 Permite buscar un carácter comodín en una cadena de caracteres sin que funcione como carácter comodín. *escape_character* es el carácter que se coloca delante del carácter comodín para indicar este uso especial.  
  
 [ NOT ] BETWEEN  
 Especifica un intervalo inclusivo de valores. Utilice AND para separar los valores inicial y final. Para obtener más información, vea [BETWEEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/between-transact-sql.md).  
  
 IS [ NOT ] NULL  
 Especifica una búsqueda de valores NULL o de valores que no son NULL, en función de las palabras clave utilizadas. Una expresión que contenga un operador bit a bit o aritmético se evalúa como NULL si uno de los operandos es NULL.  
  
 CONTAINS  
 Busca en columnas que contienen datos basados en caracteres coincidencias precisas o menos precisas (*parciales*) con palabras o frases, a una cierta distancia las unas de las otras y coincidencias ponderadas. Esta opción solo puede usarse con instrucciones SELECT. Para obtener más información, vea [CONTAINS &#40;Transact-SQL&#41;](../../t-sql/queries/contains-transact-sql.md).  
  
 FREETEXT  
 Proporciona una forma sencilla de realizar consultas en lenguaje natural al buscar, en columnas con datos basados en caracteres, valores que coincidan con el significado en lugar de con las palabras exactas del predicado. Esta opción solo puede usarse con instrucciones SELECT. Para obtener más información, vea [FREETEXT &#40;Transact-SQL&#41;](../../t-sql/queries/freetext-transact-sql.md).  
  
 [ NOT ] IN  
 Especifica la búsqueda de una expresión, basada en si la expresión está incluida en una lista o excluida de ella. La expresión de búsqueda puede ser una constante o un nombre de columna, y la lista puede ser un conjunto de constantes o, más normalmente, una subconsulta. Encierre la lista de valores entre paréntesis. Para más información, vea [IN &#40;Transact-SQL&#41;](../../t-sql/language-elements/in-transact-sql.md).  
  
 *subquery*  
 Se puede considerar como una instrucción SELECT restringida y es similar a \<query_expression> en la instrucción SELECT. No se permiten la cláusula ORDER BY ni la palabra clave INTO. Para obtener más información, vea [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
  
 ALL  
 Se utiliza con un operador de comparación y una subconsulta. Devuelve TRUE para \<predicate> si todos los valores obtenidos de la subconsulta satisfacen la operación de comparación, o FALSE cuando no todos los valores satisfacen la comparación o si la subconsulta no devuelve filas a la instrucción externa. Para obtener más información, vea [ALL &#40;Transact-SQL&#41;](../../t-sql/language-elements/all-transact-sql.md).  
  
 { SOME | ANY }  
 Se utiliza con un operador de comparación y una subconsulta. Devuelve TRUE para \<predicate> si algún valor obtenido de la subconsulta satisface el operador de comparación, o FALSE cuando ningún valor de la subconsulta satisface la comparación o si la subconsulta no devuelve filas a la instrucción externa. En caso contrario, la expresión es UNKNOWN. Para obtener más información, vea [SOME &#124; ANY &#40;Transact-SQL&#41;](../../t-sql/language-elements/some-any-transact-sql.md).  
  
 EXISTS  
 Se utiliza con una subconsulta para probar la existencia de filas devueltas por la subconsulta. Para obtener más información, vea [EXISTS &#40;Transact-SQL&#41;](../../t-sql/language-elements/exists-transact-sql.md).  
  
## <a name="remarks"></a>Observaciones  
 El orden de prioridad de los operadores lógicos es NOT (el más alto), seguido de AND y OR. Se pueden utilizar paréntesis para invalidar esta prioridad en una condición de búsqueda. El orden de evaluación de los operadores lógicos puede variar dependiendo de las opciones elegidas por el optimizador de consultas. Para obtener más información sobre el funcionamiento de los operadores lógicos en valores lógicos, vea [AND &#40;Transact-SQL&#41;](../../t-sql/language-elements/and-transact-sql.md), [OR &#40;Transact-SQL&#41;](../../t-sql/language-elements/or-transact-sql.md) y [NOT &#40;Transact-SQL&#41;](../../t-sql/language-elements/not-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-where-with-like-and-escape-syntax"></a>A. Usar WHERE con la sintaxis de LIKE y ESCAPE  
 En el siguiente ejemplo se buscan filas en las que la columna `LargePhotoFileName` tenga los caracteres `green_` y se usa la opción `ESCAPE` porque _ es un carácter comodín. Sin especificar la opción `ESCAPE`, la consulta buscaría valores de descripción que contuvieran la palabra `green` seguida de cualquier carácter distinto del carácter _.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT *   
FROM Production.ProductPhoto  
WHERE LargePhotoFileName LIKE '%greena_%' ESCAPE 'a' ;  
```  
  
### <a name="b-using-where-and-like-syntax-with-unicode-data"></a>B. Usar la sintaxis de WHERE y LIKE con datos Unicode  
 En el siguiente ejemplo se utiliza la cláusula `WHERE` para recuperar la dirección de correo de una empresa que está fuera de los Estados Unidos (`US`) y en una ciudad cuyo nombre empieza con `Pa`.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
SELECT AddressLine1, AddressLine2, City, PostalCode, CountryRegionCode    
FROM Person.Address AS a  
JOIN Person.StateProvince AS s ON a.StateProvinceID = s.StateProvinceID  
WHERE CountryRegionCode NOT IN ('US')  
AND City LIKE N'Pa%' ;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-where-with-like"></a>C. Usar WHERE con LIKE  
 En el siguiente ejemplo se buscan filas en las que la columna `LastName` tenga los caracteres `and`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE '%and%';  
```  
  
### <a name="d-using-where-and-like-syntax-with-unicode-data"></a>D. Usar la sintaxis de WHERE y LIKE con datos Unicode  
 En el ejemplo siguiente se usa la cláusula `WHERE` para realizar una búsqueda Unicode en la columna `LastName`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT EmployeeKey, LastName  
FROM DimEmployee  
WHERE LastName LIKE N'%and%';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones de agregado &#40;Transact-SQL&#41;](../../t-sql/functions/aggregate-functions-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CONTAINSTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/containstable-transact-sql.md)   
 [Cursores &#40;Transact-SQL&#41;](../../t-sql/language-elements/cursors-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [FREETEXTTABLE &#40;Transact-SQL&#41;](../../relational-databases/system-functions/freetexttable-transact-sql.md)   
 [FROM &#40;Transact-SQL&#41;](../../t-sql/queries/from-transact-sql.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)  
  
  

