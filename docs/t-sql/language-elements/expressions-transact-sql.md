---
description: Expresiones (Transact-SQL)
title: Expresiones (Transact-SQL) | Microsoft Docs
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
- Boolean expressions
- expressions [SQL Server], about expressions
- combining expressions
- Transact-SQL expressions
- expressions [SQL Server], combining
- simple expressions [SQL Server]
- complex expressions [SQL Server]
ms.assetid: ee53c5c8-e36c-40f9-8cd1-d933791b98fa
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 5f7e8b7de0c45c445fb10ddac4c2dbd764b639d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99116465"
---
# <a name="expressions-transact-sql"></a>Expresiones (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Se trata de una combinación de símbolos y operadores que [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] evalúa para obtener un único valor de datos. Las expresiones simples pueden ser una sola constante, variable, columna o función escalar. Los operadores se pueden usar para combinar dos o más expresiones simples y formar una expresión compleja.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
{ constant | scalar_function | [ table_name. ] column | variable   
    | ( expression ) | ( scalar_subquery )   
    | { unary_operator } expression   
    | expression { binary_operator } expression   
    | ranking_windowed_function | aggregate_windowed_function  
}  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

-- Expression in a SELECT statement  
<expression> ::=   
{  
    constant   
    | scalar_function   
    | column  
    | variable  
    | ( expression  )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE Windows_collation_name ]  
  
-- Scalar Expression in a DECLARE, SET, IF...ELSE, or WHILE statement  
<scalar_expression> ::=  
{  
    constant   
    | scalar_function   
    | variable  
    | ( expression  )  
    | (scalar_subquery )  
    | { unary_operator } expression   
    | expression { binary_operator } expression   
}  
[ COLLATE { Windows_collation_name ]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
  
|Término|Definición|  
|----------|----------------|  
|*constant*|Es un símbolo que representa un único valor de datos específico. Para más información, vea [Constantes &#40;Transact-SQL&#41;](../../t-sql/data-types/constants-transact-sql.md).|  
|*scalar_function*|Es una unidad de sintaxis [!INCLUDE[tsql](../../includes/tsql-md.md)] que proporciona un servicio específico y devuelve un valor único. *scalar_function* puede incluir funciones escalares integradas, como las funciones SUM, GETDATE o CAST, o funciones escalares definidas por el usuario.|  
|[ _table_name_ **.** ]|Es el nombre o alias de una tabla.|  
|*column*|Es el nombre de una columna. En una expresión solo se admite el nombre de la columna.|  
|*variable*|Es el nombre de una variable o un parámetro. Para obtener más información, vea [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md).|  
| **(** _expression_  **)**|Es cualquier expresión válida tal como se define en este tema. Los paréntesis son operadores de agrupación que garantizan que todos los operadores de la expresión escritos entre paréntesis se evalúen antes de que la expresión resultante se combine con otra.|  
|**(** _scalar_subquery_ **)**|Es una subconsulta que devuelve un valor. Por ejemplo:<br /><br /> `SELECT MAX(UnitPrice)`<br /><br /> `FROM Products`|  
|{ *unary_operator* }|Los operadores unarios solo se pueden aplicar a las expresiones que se evalúen como un tipo de datos numérico. Es un operador que solo tiene un operando numérico:<br /><br /> + indica que es un número positivo.<br /><br /> - indica que es un número negativo.<br /><br /> ~ indica el operador complementario.|  
|{ *binary_operator* }|Es un operador que define la forma en que deben combinarse dos expresiones para producir un único resultado. *binary_operator* puede ser un operador aritmético, el operador de asignación (=), un operador bit a bit, un operador de comparación, un operador lógico, el operador de concatenación de cadena (+) o un operador unario. Para obtener más información sobre operadores, vea [Operadores &#40;Transact-SQL&#41;](../../t-sql/language-elements/operators-transact-sql.md).|  
|*ranking_windowed_function*|Es cualquier función de categoría de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Para obtener más información, vea [Funciones de categoría &#40;Transact-SQL&#41;](../../t-sql/functions/ranking-functions-transact-sql.md).|  
|*aggregate_windowed_function*|Es cualquier función de agregado de [!INCLUDE[tsql](../../includes/tsql-md.md)] con la cláusula OVER. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).|  
  
## <a name="expression-results"></a>Resultados de la expresión  
 Para una expresión sencilla creada con una constante, variable, función escalar o nombre de columna, el tipo de datos, intercalación, precisión, escala y valor de la expresión es el tipo de datos, intercalación, precisión, escala y valor del elemento de referencia.  
  
 Si se combinan dos expresiones mediante operadores de comparación o lógicos, el tipo de datos resultante es booleano y el valor es uno de los siguientes: TRUE, FALSE o UNKNOWN. Para obtener más información sobre los tipos de datos booleanos, vea [Operadores de comparación &#40;Transact-SQL&#41;](../../t-sql/language-elements/comparison-operators-transact-sql.md).  
  
 Cuando dos expresiones se combinan mediante operadores aritméticos, bit a bit o de cadena, el operador determina el tipo de datos resultante.  
  
 Las expresiones complejas formadas por varios símbolos y operadores se evalúan como un resultado formado por un solo valor. El tipo de datos, intercalación, precisión y valor de la expresión resultante se determina al combinar las expresiones componentes de dos en dos, hasta que se alcanza un resultado final. La prioridad de los operadores de la expresión define la secuencia en que se combinan las expresiones.  
  
## <a name="remarks"></a>Comentarios  
 Dos expresiones pueden combinarse mediante un operador si ambas tienen tipos de datos admitidos por el operador y se cumple al menos una de estas condiciones:  
  
-   Las expresiones tienen el mismo tipo de datos.  
  
-   El tipo de datos de menor prioridad se puede convertir implícitamente al tipo de datos de mayor prioridad.  
  
 Si las expresiones no cumplen estas condiciones, se pueden usar las funciones CAST o CONVERT para convertir explícitamente el tipo de datos de menor prioridad al tipo de datos de mayor prioridad o a un tipo de datos intermedio que se puede convertir implícitamente al tipo de datos de mayor prioridad.  
  
 Si no se admite la conversión implícita o explícita admitida, las dos expresiones no se pueden combinar.  
  
 La intercalación de cualquier expresión que se evalúa como una cadena de caracteres se establece según las reglas de prioridad de intercalación. Para más información, vea [Prioridad de intercalación &#40;Transact-SQL&#41;](../../t-sql/statements/collation-precedence-transact-sql.md).  
  
 En un lenguaje de programación como C o [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], una expresión siempre se evalúa como un resultado único. Las expresiones de una lista de selección de [!INCLUDE[tsql](../../includes/tsql-md.md)] constituyen una variación de esta regla: la expresión se evalúa individualmente para cada fila del conjunto de resultados. Una única expresión puede tener un valor distinto en cada fila del conjunto de resultados, pero cada fila tiene un único valor para la expresión. Por ejemplo, en esta instrucción `SELECT`, tanto la referencia a `ProductID` como el término `1+2` de la lista seleccionada son expresiones:  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT ProductID, 1+2  
FROM Production.Product;  
GO  
```  
  
 La expresión `1+2` se evalúa como `3` en cada fila del conjunto de resultados. Aunque la expresión `ProductID` genera un valor único en cada fila del conjunto de resultados, cada fila tiene solo un valor para `ProductID`.  
 
- [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] asigna una cantidad de memoria máxima fija a cada subproceso, para que ningún subproceso pueda usar toda la memoria.  Parte de esta memoria se usa para almacenar las expresiones de las consultas.  Si una consulta tiene demasiadas expresiones y su memoria necesaria supera el límite interno, el motor no la ejecutará.  Para evitar este problema, los usuarios pueden cambiar la consulta en varias consultas con un número menor de expresiones en cada una. Por ejemplo, tiene una consulta con una larga lista de expresiones en la cláusula WHERE: 

```sql
DELETE FROM dbo.MyTable 
WHERE
(c1 = '0000001' AND c2 = 'A000001') or
(c1 = '0000002' AND c2 = 'A000002') or
(c1 = '0000003' AND c2 = 'A000003') or
...

```
Cambie esta consulta por:

```sql
DELETE FROM dbo.MyTable WHERE (c1 = '0000001' AND c2 = 'A000001');
DELETE FROM dbo.MyTable WHERE (c1 = '0000002' AND c2 = 'A000002');
DELETE FROM dbo.MyTable WHERE (c1 = '0000003' AND c2 = 'A000003');
...
```

## <a name="see-also"></a>Vea también  
 [AT TIME ZONE &#40;Transact-SQL&#41;](../../t-sql/queries/at-time-zone-transact-sql.md)   
 [CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)   
 [CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [COALESCE &#40;Transact-SQL&#41;](../../t-sql/language-elements/coalesce-transact-sql.md)   
 [Conversión de tipos de datos &#40;motor de base de datos&#41;](../../t-sql/data-types/data-type-conversion-database-engine.md)   
 [Precedencia de tipo de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md)   
 [Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [NULLIF &#40;Transact-SQL&#41;](../../t-sql/language-elements/nullif-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)  
  
  
