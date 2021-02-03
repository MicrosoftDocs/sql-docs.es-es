---
description: Prioridad de intercalación
title: Prioridad de intercalación | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- coercible-default collation label
- precedence [SQL Server], collations
- collation sensitive
- collations [SQL Server], precedence
- explicit collation label [SQL Server]
- implicit collation label
- no-collation label
- collation insensitive
- operators [Transact-SQL], collations
- collation labels
- collation coercion rules
- rules [SQL Server], collations
ms.assetid: 58c4e64b-5634-4c29-aa22-33193282dd27
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 167a870a59319ea28c19545d241af6cb2dd943c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184500"
---
# <a name="collation-precedence"></a>Prioridad de intercalación
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  La prioridad de intercalación, o reglas de coerción de intercalación, determina lo siguiente:  
  
-   La intercalación del resultado final de una expresión que se evalúa como cadena de caracteres.  
  
-   La intercalación utilizada por los operadores que distinguen la intercalación y que utilizan entradas de cadenas de caracteres pero no devuelven una cadena de caracteres, como LIKE e IN.  
  
Las reglas de prioridad de intercalación solo se aplican a los tipos de datos de cadena de caracteres: **char**, **varchar**, **text**, **nchar**, **nvarchar** y **ntext**. Los objetos con otros tipos de datos no participan en las evaluaciones de intercalación.  
  
## <a name="collation-labels"></a>Etiquetas de intercalación  
En la tabla siguiente se enumeran y describen las cuatro categorías en las que se identifican las intercalaciones de todos los objetos. El nombre de cada categoría se denomina etiqueta de intercalación.  
  
|Etiqueta de intercalación|Tipos de objetos|  
|---------------------|----------------------|  
|Coercible-default|Cualquier variable, parámetro o valor literal de cadena de caracteres [!INCLUDE[tsql](../../includes/tsql-md.md)], o la salida de una función de catálogo integrada, o una función integrada que no utiliza entradas de cadena, pero genera una salida de cadena.<br /><br /> Si el objeto se declara en una función definida por el usuario, procedimiento almacenado o desencadenador, se le asigna la intercalación predeterminada de la base de datos en la que se crea la función, el procedimiento almacenado o el desencadenador. Si el objeto se declara en un lote, se le asigna la intercalación predeterminada de la base de datos actual para la conexión.|  
|Implicit X|Referencia de columna. La intercalación de la expresión (X) se obtiene de la intercalación definida para la columna en la tabla o vista.<br /><br /> Aunque se haya asignado una intercalación a la columna explícitamente mediante una cláusula COLLATE en la instrucción CREATE TABLE o CREATE VIEW, la referencia de columna se clasifica como implícita.|  
|Explicit X|Expresión que se convierte explícitamente a una intercalación específica (X) utilizando una cláusula COLLATE en la expresión.|  
|No-collation|Indica que el valor de una expresión es el resultado de una operación entre dos cadenas que tienen intercalaciones incompatibles de la etiqueta de intercalación implícita. El resultado de la expresión se define como carente de intercalación.|  
  
## <a name="collation-rules"></a>Reglas de intercalación  
La etiqueta de intercalación de una expresión simple que hace referencia a un único objeto de cadena de caracteres es la etiqueta de intercalación del objeto al que se hace referencia.  
  
La etiqueta de intercalación de una expresión compleja que hace referencia a dos expresiones de operando con la misma etiqueta de intercalación es la etiqueta de intercalación de las expresiones de operando.  
  
La etiqueta de intercalación del resultado final de una expresión compleja que hace referencia a dos expresiones de operando con distintas intercalaciones se basa en las reglas siguientes:  
  
-   Explicit tiene prioridad sobre Implicit. Implicit tiene prioridad sobre Coercible-default:  
  
     Explicit > Implicit > Coercible-default  
  
-   La combinación de dos expresiones Explicit a las que se han asignado intercalaciones distintas genera un error:  
  
     Explicit X + Explicit Y = Error  
  
-   La combinación de dos expresiones Implicit que tienen intercalaciones distintas genera el resultado No-collation:  
  
     Implicit X + Implicit Y = No-collation   
  
-   La combinación de una expresión con la etiqueta No-collation y una expresión con cualquier etiqueta excepto Explicit (vea la regla siguiente), genera un resultado con la etiqueta No-collation:  
  
     No-collation + cualquier etiqueta = No-collation   
  
-   La combinación de una expresión con la etiqueta No-collation y una expresión con intercalación Explicit genera una expresión con una etiqueta Explicit:  
  
     No-collation + Explicit X = Explicit   
  
En la tabla siguiente se resumen las reglas.  
  
|Etiqueta de coerción de operandos|Explicit X|Implicit X|Coercible-default|No-collation|  
|----------------------------|----------------|----------------|------------------------|-------------------|  
|**Explicit Y**|Genera un error|Da como resultado Explicit Y|Da como resultado Explicit Y|Da como resultado Explicit Y|  
|**Implicit Y**|Da como resultado Explicit X|Da como resultado No-collation|Da como resultado Implicit Y|Da como resultado No-collation|  
|**Coercible-default**|Da como resultado Explicit X|Da como resultado Implicit X|Da como resultado Coercible-default|Da como resultado No-collation|  
|**No-collation**|Da como resultado Explicit X|Da como resultado No-collation|Da como resultado No-collation|Da como resultado No-collation|  
  
Las siguientes reglas adicionales también se aplican a la prioridad de intercalación:  
  
-   No puede haber varias cláusulas COLLATE en una expresión que ya sea una expresión explícita. Por ejemplo, la cláusula `WHERE` siguiente no es válida porque se ha especificado una cláusula `COLLATE` para una expresión que ya es una expresión explícita:  
  
     `WHERE ColumnA = ( 'abc' COLLATE French_CI_AS) COLLATE French_CS_AS`  
  
-   No se permiten las conversiones de páginas de códigos para los tipos de datos **text**. No puede convertir una expresión **text** de una intercalación a otra si tienen páginas de códigos diferentes. El operador de asignación no puede asignar valores cuando la intercalación del operando de texto de la derecha tiene una página de códigos distinta de la del operando de texto de la izquierda.  
  
La prioridad de intercalación se determina después de la conversión de los tipos de datos. El operando del que se obtiene la intercalación resultante puede ser distinto del operando que proporciona el tipo de datos del resultado final. Considere, por ejemplo, el siguiente lote:  
  
```sql  
CREATE TABLE TestTab  
   (PrimaryKey int PRIMARY KEY,  
    CharCol char(10) COLLATE French_CI_AS  
   )  
  
SELECT *  
FROM TestTab  
WHERE CharCol LIKE N'abc'  
```  
  
El tipo de datos Unicode de la expresión simple `N'abc'` tiene una prioridad de tipo de datos superior. Por tanto, la expresión resultante tiene el tipo de datos Unicode asignado a `N'abc'`. Sin embargo, la expresión `CharCol` tiene una etiqueta de intercalación Implicit y `N'abc'` tiene una etiqueta de coerción Coercible-default, que es inferior. Por lo tanto, la intercalación que se utiliza es la intercalación `French_CI_AS` de `CharCol`.  
  
### <a name="examples-of-collation-rules"></a>Ejemplos de reglas de intercalación  
 En los ejemplos siguientes se muestra el funcionamiento de las reglas de intercalación. Para ejecutar los ejemplos, cree la siguiente tabla de prueba.  
  
```sql  
USE tempdb;  
GO  
  
CREATE TABLE TestTab (  
   id int,   
   GreekCol nvarchar(10) collate greek_ci_as,   
   LatinCol nvarchar(10) collate latin1_general_cs_as  
   )  
INSERT TestTab VALUES (1, N'A', N'a');  
GO  
```  
  
#### <a name="collation-conflict-and-error"></a>Conflictos y errores de intercalación  
 El predicado de la siguiente consulta presenta un conflicto de intercalación y genera un error.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 448, Level 16, State 9, Line 2  
Cannot resolve collation conflict between 'Latin1_General_CS_AS' and 'Greek_CI_AS' in equal to operation.  
```  
  
#### <a name="explicit-label-vs-implicit-label"></a>Etiqueta Explicit y etiqueta Implicit  
 El predicado de la siguiente consulta se evalúa en la intercalación `greek_ci_as` porque la expresión de la derecha tiene la etiqueta Explicit. Ésta tiene prioridad sobre la etiqueta Implicit de la expresión de la izquierda.  
  
```sql  
SELECT *   
FROM TestTab   
WHERE GreekCol = LatinCol COLLATE greek_ci_as;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
id          GreekCol             LatinCol  
----------- -------------------- --------------------  
          1 A                    a  
  
(1 row affected)  
```  
  
#### <a name="no-collation-labels"></a>Etiquetas No-Collation  
Las expresiones `CASE` de las siguientes consultas tienen una etiqueta No-collation, por lo que no pueden aparecer en la lista de selección ni funcionar con operadores que distinguen la intercalación. No obstante, las expresiones pueden funcionar con operadores que no distinguen la intercalación.  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END)   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 451, Level 16, State 1, Line 1  
Cannot resolve collation conflict for column 1 in SELECT statement.  
```  
  
```sql  
SELECT PATINDEX((CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END), 'a')  
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Msg 446, Level 16, State 9, Server LEIH2, Line 1  
Cannot resolve collation conflict for patindex operation.  
```  
  
```sql  
SELECT (CASE WHEN id > 10 THEN GreekCol ELSE LatinCol END) COLLATE Latin1_General_CI_AS   
FROM TestTab;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
--------------------  
a  
  
(1 row affected)  
```  
  
## <a name="collation-sensitive-and-collation-insensitive"></a>Distinción y no distinción de la intercalación  
Los operadores y las funciones pueden distinguir o no las intercalaciones.  
  
Distinción de la intercalación  
Significa que, al especificar un operando No-collation, se genera un error en tiempo de compilación. El resultado de la expresión no puede ser No-collation.  
  
No distinción de la intercalación  
Significa que los operandos y el resultado pueden ser No-collation.  
  
### <a name="operators-and-collation"></a>Operadores e intercalación  
Los operadores de comparación, así como los operadores MAX, MIN, BETWEEN, LIKE e IN, distinguen la intercalación. La cadena que utilizan los operadores se asigna a la etiqueta de intercalación del operando que tiene mayor prioridad. La instrucción UNION también distingue la intercalación, y todos los operandos de cadena y el resultado final reciben la intercalación del operando que tiene la prioridad más alta. La prioridad de intercalación del operando UNION y del resultado se evalúan columna a columna.  
  
El operador de asignación no distingue la intercalación y la expresión de la derecha se convierte a la intercalación de la izquierda.  
  
El operador de concatenación de cadenas distingue la intercalación, por lo que a los dos operandos de cadena y al resultado se les asigna la etiqueta de intercalación del operando que tiene la prioridad de intercalación más alta. Las instrucciones UNION ALL y CASE no distinguen la intercalación, y a todos los operandos de cadena y al resultado final se les asigna la etiqueta de intercalación del operando que tiene la prioridad más alta. La prioridad de intercalación de los operandos UNION ALL y del resultado se evalúan columna a columna.  
  
### <a name="functions-and-collation"></a>Funciones e intercalación  
Las funciones CAST, CONVERT y COLLATE distinguen la intercalación para los tipos de datos **char**, **varchar** y **text**. Si la entrada y la salida de las funciones CAST y CONVERT son cadenas de caracteres, la cadena de salida tiene la etiqueta de intercalación de la cadena de entrada. Si la entrada no es una cadena de caracteres, la cadena de salida es Coercible-default, y se le asigna la intercalación de la base de datos actual para la conexión o de la base de datos que contiene la función definida por el usuario, el procedimiento almacenado o el desencadenador en el que se hace referencia a CAST o CONVERT.  
  
 Para las funciones integradas que devuelven una cadena pero no reciben una entrada de cadena, la cadena de resultado es Coercible-default, y se le asigna la intercalación de la base de datos actual o la intercalación de la base de datos que contiene la función definida por el usuario, el procedimiento almacenado o el desencadenador en el que se hace referencia a la función.  
  
 Las funciones siguientes distinguen la intercalación y sus cadenas de salida tienen la etiqueta de intercalación de la cadena de entrada:  

:::row:::
    :::column:::
        CHARINDEX
    :::column-end:::
    :::column:::
        REPLACE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        DIFFERENCE
    :::column-end:::
    :::column:::
        REVERSE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        ISNUMERIC
    :::column-end:::
    :::column:::
        RIGHT
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEFT
    :::column-end:::
    :::column:::
        SOUNDEX
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LEN
    :::column-end:::
    :::column:::
        STUFF
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        LOWER
    :::column-end:::
    :::column:::
        SUBSTRING
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        PATINDEX
    :::column-end:::
    :::column:::
        UPPER
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Consulte también  
 [COLLATE &#40;Transact-SQL&#41;](~/t-sql/statements/collations.md)   
 [Conversión de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)  
  
  
