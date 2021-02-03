---
description: 'Operadores de conjuntos: EXCEPT e INTERSECT (Transact-SQL)'
title: EXCEPT e INTERSECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- INTERSECT_TSQL
- EXCEPT_TSQL
- INTERSECT
- EXCEPT
dev_langs:
- TSQL
helpviewer_keywords:
- EXCEPT operator [Transact-SQL]
- queries [SQL Server], comparing
- comparing queries
- INTERSECT operator
ms.assetid: b1019300-171a-4a1a-854f-e1e751de3565
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 6b1c748199ea322a86f14f84525a6ed049c00b73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172538"
---
# <a name="set-operators---except-and-intersect-transact-sql"></a>Operadores de conjuntos: EXCEPT e INTERSECT (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Devuelve filas distintas al comparar los resultados de dos consultas.  
  
EXCEPT devuelve filas distintas de la consulta de entrada izquierda que no son de salida en la consulta de entrada derecha.  
 
INTERSECT devuelve filas distintas que son el resultado del operador de las consultas de entrada izquierda y derecha.  
  
Para combinar los conjuntos de resultados de dos consultas que usan EXCEPT o INTERSECT, las reglas básicas son:  
  
-   El número y el orden de las columnas debe ser el mismo en todas las consultas.  
  
-   Los tipos de datos deben ser compatibles.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
{ <query_specification> | ( <query_expression> ) }   
{ EXCEPT | INTERSECT }  
{ <query_specification> | ( <query_expression> ) }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
\<_query\_specification_> | ( \<_query\_expression_> )  
Es una especificación o expresión de consulta que devuelve datos que se van a comparar con los de otra especificación o expresión de consulta. No es preciso que las definiciones de las columnas que forman parte de una operación EXCEPT o INTERSECT sean idénticas. Sin embargo, deben ser comparables mediante una conversión implícita. Cuando los tipos de datos difieren, las reglas de [prioridad de tipo de datos](../../t-sql/data-types/data-type-precedence-transact-sql.md) determinan el tipo de datos que se ejecuta para la comparación.  
  
El resultado se basa en las mismas reglas para combinar expresiones cuando los tipos son los mismos pero varían en cuanto a precisión, escala o longitud. Para más información, vea [Precisión, escala y longitud &#40;Transact-SQL&#41;](../../t-sql/data-types/precision-scale-and-length-transact-sql.md).  
  
La especificación o expresión de consulta no puede devolver columnas de tipo **xml**, **text**, **ntext**, **image** o no binario definido por el usuario CLR, ya que estos tipos de datos no son comparables.  
  
EXCEPT  
Devuelve todos los valores distintos de la consulta del lado izquierdo del operador EXCEPT. Esos valores se devuelven siempre que la consulta correcta no los devuelva también.  
  
INTERSECT  
Devuelve los valores distintos devueltos por las consultas situadas a los lados izquierdo y derecho del operador INTERSECT.  
  
## <a name="remarks"></a>Observaciones  
Los tipos de datos de las columnas comparables los devuelven las consultas de los lados izquierdo y derecho de los operadores EXCEPT o INTERSECT. Estos tipos de datos pueden incluir tipos de datos de caracteres con intercalaciones diferentes. Cuando lo hacen, la comparación requerida se ejecuta según las reglas de [prioridad de intercalación](../../t-sql/statements/collation-precedence-transact-sql.md). Si no puede ejecutar esta conversión, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] devuelve un error.  
  
Cuando se comparan los valores de columna para determinar filas DISTINCT, dos valores NULL se consideran equivalentes.  
  
EXCEPT e INTERSECT devuelven los nombres de columna del conjunto de resultados que son los mismos que los nombres de columna devueltos por la consulta del lado izquierdo del operador.  
  
Los nombres o alias de columna de las cláusulas ORDER BY deben hacer referencia a los nombres de columna devueltos por la consulta del lado izquierdo.  
  
La nulabilidad de cualquier columna del conjunto de resultados devueltos por EXCEPT o INTERSECT es la misma que la de la columna correspondiente devuelta por la consulta del lado izquierdo del operador.  
  
Si EXCEPT o INTERSECT se utilizan con otros operadores en una expresión, esta se evalúa en el contexto de la siguiente prioridad:  
  
1.  Expresiones entre paréntesis  
  
2.  El operador INTERSECT  
  
3.  EXCEPT y UNION se evalúan de izquierda a derecha según su posición en la expresión  
  
Puede usar EXCEPT o INTERSECT para comparar más de dos conjuntos de consultas. Al hacerlo, la conversión del tipo de datos se determina al comparar dos consultas a la vez y mediante las reglas de evaluación de expresiones mencionadas anteriormente.  
  
EXCEPT e INTERSECT no se pueden usar en definiciones de vistas distribuidas con particiones ni en notificaciones de consultas.  
 
EXCEPT e INTERSECT se pueden utilizar en consultas distribuidas, pero solo se ejecutan en el servidor local y no se insertan en el servidor vinculado. De esta forma, el uso de EXCEPT e INTERSECT en consultas distribuidas puede afectar al rendimiento.  
  
Puede usar los cursores de solo avance rápido y estáticos en el conjunto de resultados si se utilizan con una operación EXCEPT o INTERSECT. También puede utilizar un cursor dinámico o controlado por conjunto de claves junto con una operación EXCEPT o INTERSECT. Al hacerlo, el cursor del conjunto de resultados de la operación se convierte en un cursor estático.  
  
Cuando una operación EXCEPT se muestra a través de la característica Plan de presentación gráfico de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], la operación aparece como un operador [left anti semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md) y la operación INTERSECT, como un operador [left semi join](../../relational-databases/showplan-logical-and-physical-operators-reference.md).  
  
## <a name="examples"></a>Ejemplos  
En los siguientes ejemplos se indica cómo usar los operadores `INTERSECT` y `EXCEPT`. La primera consulta devuelve todos los valores de la tabla `Production.Product` para comparar los resultados con `INTERSECT` y `EXCEPT`.  
  
```sql
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product ;  
--Result: 504 Rows  
```  
  
La siguiente consulta devuelve los valores distintos devueltos por las consultas situadas a los lados izquierdo y derecho del operador `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
INTERSECT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 238 Rows (products that have work orders)  
```  
  
La siguiente consulta devuelve los valores distintos de la consulta del lado izquierdo del operador `EXCEPT` que tampoco se encuentran en la consulta derecha.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.Product  
EXCEPT  
SELECT ProductID   
FROM Production.WorkOrder ;  
--Result: 266 Rows (products without work orders)  
```  
  
La siguiente consulta devuelve los valores distintos de la consulta del lado izquierdo del operador `EXCEPT` que tampoco se encuentran en la consulta derecha. Las tablas se invierten respecto al ejemplo anterior.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT ProductID   
FROM Production.WorkOrder  
EXCEPT  
SELECT ProductID   
FROM Production.Product ;  
--Result: 0 Rows (work orders without products)  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
En los siguientes ejemplos se muestra el modo de usar los operadores `INTERSECT` y `EXCEPT`. La primera consulta devuelve todos los valores de la tabla `FactInternetSales` para comparar los resultados con `INTERSECT` y `EXCEPT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales;  
--Result: 60398 Rows  
```  
  
La siguiente consulta devuelve los valores distintos devueltos por las consultas situadas a los lados izquierdo y derecho del operador `INTERSECT`.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
INTERSECT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9133 Rows (Sales to customers that are female.)  
```  
  
La siguiente consulta devuelve los valores distintos de la consulta del lado izquierdo del operador `EXCEPT` que tampoco se encuentran en la consulta derecha.  
  
```sql  
-- Uses AdventureWorks  
  
SELECT CustomerKey   
FROM FactInternetSales    
EXCEPT   
SELECT CustomerKey   
FROM DimCustomer   
WHERE DimCustomer.Gender = 'F'  
ORDER BY CustomerKey;  
--Result: 9351 Rows (Sales to customers that are not female.)  
```  
