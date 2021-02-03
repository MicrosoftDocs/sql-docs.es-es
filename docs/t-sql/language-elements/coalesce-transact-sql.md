---
title: COALESCE (Transact-SQL) | Microsoft Docs
description: Referencia de Transact-SQL para COALESCE, que devuelve el valor de la primera expresión que no se evalúa como NULL.
ms.date: 08/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- COALESCE
- COALESCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- expressions [SQL Server], nonnull
- COALESCE function
- first nonnull expressions [SQL Server]
- nonnull expressions
ms.assetid: fafc0dba-f8a8-4aad-9b7f-908e34b74d88
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9d7058f8ba69571e89eaff4fb3721c3518fbe3e3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208831"
---
# <a name="coalesce-transact-sql"></a>COALESCE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Evalúa los argumentos en orden y devuelve el valor actual de la primera expresión que inicialmente no se evalúa como `NULL`. Por ejemplo, `SELECT COALESCE(NULL, NULL, 'third_value', 'fourth_value');` devuelve el tercer valor porque el tercer valor es el primer valor que no es NULL. 
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
COALESCE ( expression [ ,...n ] )   
```  
  
## <a name="arguments"></a>Argumentos  
_expression_  
Es una [expresión](../../t-sql/language-elements/expressions-transact-sql.md) de cualquier tipo.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
Devuelve el tipo de datos de _expresión_ con la prioridad de tipo de datos más alta. Si ninguna de las expresiones admiten valores NULL, el resultado tiene un tipo que no admite valores NULL.  
  
## <a name="remarks"></a>Observaciones  
Si todos los argumentos son `NULL`, `COALESCE` devuelve `NULL`. Al menos uno de los valores NULL debe ser `NULL` con tipo.  
  
## <a name="comparing-coalesce-and-case"></a>Comparar COALESCE y CASE  
La expresión `COALESCE` es un método abreviado sintáctico de la expresión `CASE`.  Es decir, el optimizador de consultas vuelve a escribir el código `COALESCE`(_expresión1_, _...n_) como la expresión `CASE` siguiente:  
  
```sql  
CASE  
WHEN (expression1 IS NOT NULL) THEN expression1  
WHEN (expression2 IS NOT NULL) THEN expression2  
...  
ELSE expressionN  
END  
```  
  
Esto significa que los valores de entrada (_expresión1_, _expresión2_, _expresiónN_, etc.) se evalúan varias veces. Una expresión de valor que contiene una subconsulta se considera no determinista, y la subconsulta se evalúa dos veces. El resultado está conforme con el estándar SQL. En cualquier caso, se pueden devolver resultados diferentes entre la primera evaluación y las evaluaciones posteriores.  
  
Por ejemplo, cuando se ejecuta el código `COALESCE((subquery), 1)`, la subconsulta se evalúa dos veces. En consecuencia, podrá obtener resultados diferentes en función del nivel de aislamiento de la consulta. Por ejemplo, en un entorno multiusuario, el código puede devolver `NULL` en el nivel de aislamiento `READ COMMITTED`. Para asegurarse de que se devuelven resultados estables, use el nivel de aislamiento `SNAPSHOT ISOLATION`, o bien reemplace `COALESCE` por la función `ISNULL`. Como alternativa, puede volver a escribir la consulta para insertar la subconsulta en una subselección como se muestra en el ejemplo siguiente:  
  
```sql  
SELECT CASE WHEN x IS NOT NULL THEN x ELSE 1 END  
FROM  
(  
SELECT (SELECT Nullable FROM Demo WHERE SomeCol = 1) AS x  
) AS T;  
  
```  
  
## <a name="comparing-coalesce-and-isnull"></a>Comparar COALESCE e ISNULL  
La función `ISNULL` y la expresión `COALESCE` tienen una finalidad similar, pero se pueden comportar de forma diferente.  
  
1.  Dado que `ISNULL` es una función, solo se evalúa una vez.  Como se describió anteriormente, los valores de entrada de la expresión `COALESCE` se pueden evaluar varias veces.  
  
2.  La determinación del tipo de datos de la expresión resultante es diferente. `ISNULL` usa el tipo de datos del primer parámetro, mientras que `COALESCE` sigue las reglas de la expresión `CASE` y devuelve el tipo de datos del valor con la prioridad más alta.  
  
3.  La nulabilidad de la expresión de resultado es diferente para `ISNULL` y `COALESCE`. El valor devuelto por `ISNULL` siempre se considera NOT NULL (suponiendo que el valor devuelto no admita valores NULL). Por el contrario, `COALESCE` con parámetros que no son NULL se considera `NULL`. Por tanto, aunque las expresiones `ISNULL(NULL, 1)` y `COALESCE(NULL, 1)` son equivalentes, tienen valores de nulabilidad diferentes. Estos valores marcan la diferencia si estas expresiones se usan en columnas calculadas, se crean restricciones de clave o se hace que el valor devuelto de un UDF escalar sea determinista para poder indexarlo, como se muestra en el ejemplo siguiente:  
  
    ```sql  
    USE tempdb;  
    GO  
    -- This statement fails because the PRIMARY KEY cannot accept NULL values  
    -- and the nullability of the COALESCE expression for col2   
    -- evaluates to NULL.  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0) PRIMARY KEY,   
      col3 AS ISNULL(col1, 0)   
    );   
  
    -- This statement succeeds because the nullability of the   
    -- ISNULL function evaluates AS NOT NULL.  
  
    CREATE TABLE #Demo   
    (   
      col1 INTEGER NULL,   
      col2 AS COALESCE(col1, 0),   
      col3 AS ISNULL(col1, 0) PRIMARY KEY   
    );  
    ```  
  
4.  Las validaciones de `ISNULL` y `COALESCE` también son diferentes. Por ejemplo, con `ISNULL`, un valor `NULL` se convierte en **int**, mientras que con `COALESCE` es necesario proporcionar un tipo de datos.  
  
5.  `ISNULL` toma solo dos parámetros. Por el contrario `COALESCE` toma un número variable de parámetros.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-running-a-simple-example"></a>A. Ejecutar un ejemplo sencillo  
En el ejemplo siguiente se muestra cómo `COALESCE` selecciona los datos de la primera columna que tiene un valor no nulo. En este ejemplo se usa la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```sql  
SELECT Name, Class, Color, ProductNumber,  
COALESCE(Class, Color, ProductNumber) AS FirstNotNull  
FROM Production.Product;  
```  
  
### <a name="b-running-a-complex-example"></a>B. Ejecutar un ejemplo complejo  
En este ejemplo, la tabla `wages` incluye tres columnas con información acerca del sueldo anual de los empleados: la tarifa por hora, el salario y la comisión. No obstante, un empleado recibe solo un tipo de sueldo. Para determinar el importe total pagado a todos los empleados, utilice `COALESCE` para obtener solo los valores no NULL que se encuentran en `hourly_wage`, `salary` y `commission`.  
  
```sql  
SET NOCOUNT ON;  
GO  
USE tempdb;  
IF OBJECT_ID('dbo.wages') IS NOT NULL  
    DROP TABLE wages;  
GO  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   IDENTITY,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
GO  
INSERT dbo.wages (hourly_wage, salary, commission, num_sales)  
VALUES  
    (10.00, NULL, NULL, NULL),  
    (20.00, NULL, NULL, NULL),  
    (30.00, NULL, NULL, NULL),  
    (40.00, NULL, NULL, NULL),  
    (NULL, 10000.00, NULL, NULL),  
    (NULL, 20000.00, NULL, NULL),  
    (NULL, 30000.00, NULL, NULL),  
    (NULL, 40000.00, NULL, NULL),  
    (NULL, NULL, 15000, 3),  
    (NULL, NULL, 25000, 2),  
    (NULL, NULL, 20000, 6),  
    (NULL, NULL, 14000, 4);  
GO  
SET NOCOUNT OFF;  
GO  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS money) AS 'Total Salary'   
FROM dbo.wages  
ORDER BY 'Total Salary';  
GO  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```   
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00  
  
(12 row(s) affected)
```  
  
### <a name="c-simple-example"></a>C. Ejemplo sencillo  
En el ejemplo siguiente se muestra cómo `COALESCE` selecciona los datos de la primera columna que tiene un valor que no es NULL. Para este ejemplo, se supone que la tabla `Products` contiene estos datos:  
  
```  
Name         Color      ProductNumber  
------------ ---------- -------------  
Socks, Mens  NULL       PN1278  
Socks, Mens  Blue       PN1965  
NULL         White      PN9876
```  
 
Después, se ejecuta la siguiente consulta COALESCE:  
  
```sql  
SELECT Name, Color, ProductNumber, COALESCE(Color, ProductNumber) AS FirstNotNull   
FROM Products ;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Name         Color      ProductNumber  FirstNotNull  
------------ ---------- -------------  ------------  
Socks, Mens  NULL       PN1278         PN1278  
Socks, Mens  Blue       PN1965         Blue  
NULL         White      PN9876         White
```  
  
Observe que en la primera fila, el valor `FirstNotNull` es `PN1278`, no `Socks, Mens`. Esto se debe a que en el ejemplo la columna `Name` no se especificó como un parámetro para `COALESCE`.  
  
### <a name="d-complex-example"></a>D. Ejemplo complejo  
En el ejemplo siguiente se usa `COALESCE` para comparar los valores de tres columnas y devolver solo el valor distinto de NULL que se encuentra en las columnas.  
  
```sql  
CREATE TABLE dbo.wages  
(  
    emp_id        TINYINT   NULL,  
    hourly_wage   DECIMAL   NULL,  
    salary        DECIMAL   NULL,  
    commission    DECIMAL   NULL,  
    num_sales     TINYINT   NULL  
);  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (1, 10.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (2, 20.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (3, 30.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (4, 40.00, NULL, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (5, NULL, 10000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (6, NULL, 20000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (7, NULL, 30000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (8, NULL, 40000.00, NULL, NULL);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (9, NULL, NULL, 15000, 3);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (10,NULL, NULL, 25000, 2);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (11, NULL, NULL, 20000, 6);  
  
INSERT INTO dbo.wages (emp_id, hourly_wage, salary, commission, num_sales)  
VALUES (12, NULL, NULL, 14000, 4);  
  
SELECT CAST(COALESCE(hourly_wage * 40 * 52,   
   salary,   
   commission * num_sales) AS DECIMAL(10,2)) AS TotalSalary   
FROM dbo.wages  
ORDER BY TotalSalary;  
```  
  
[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Total Salary  
------------  
10000.00  
20000.00  
20800.00  
30000.00  
40000.00  
41600.00  
45000.00  
50000.00  
56000.00  
62400.00  
83200.00  
120000.00
```  
  
## <a name="see-also"></a>Consulte también  
[ISNULL &#40;Transact-SQL&#41;](../../t-sql/functions/isnull-transact-sql.md)   
[CASE &#40;Transact-SQL&#41;](../../t-sql/language-elements/case-transact-sql.md)  
  
  
