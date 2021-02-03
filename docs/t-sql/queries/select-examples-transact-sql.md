---
description: Ejemplos de SELECT (Transact-SQL)
title: Ejemplos de SELECT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- parentheses [SQL Server]
- GROUP BY clause, SELECT statement
- query hints [SQL Server]
- ALL keyword
- ROLLUP operator
- SELECT statement [SQL Server], examples
- correlated subqueries, SELECT statement
- SELECT INTO statement
- ORDER BY clause [Transact-SQL]
- GROUPING function
- index hints [SQL Server]
- HAVING clause, SELECT statement
- DISTINCT keyword
- CUBE operator
- UNION operator [SQL Server]
- computed sums
- WHERE clause, SELECT statement
ms.assetid: 9b9caa3d-e7d0-42e1-b60b-a5572142186c
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e2e12a155eb1a3c710b3289912f9e9e12f3642ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207585"
---
# <a name="select-examples-transact-sql"></a>Ejemplos de SELECT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En este tema se proporcionan ejemplos de uso de la instrucción [SELECT](../../t-sql/queries/select-transact-sql.md).  
  
## <a name="a-using-select-to-retrieve-rows-and-columns"></a>A. Usar SELECT para recuperar filas y columnas  
 En el siguiente ejemplo se muestran tres fragmentos de código. En el primer ejemplo de código, se devuelven todas las filas (no se especifica la cláusula WHERE) y todas las columnas (con `*`) de la tabla `Product` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)].  
  
 [!code-sql[Select#SelectExamples1](../../t-sql/queries/codesnippet/tsql/select-examples-transact_1.sql)]  
  
 En este ejemplo se devuelven todas las filas (no se ha especificado la cláusula WHERE) y solo un subconjunto de las columnas (`Name`, `ProductNumber`, `ListPrice`) de la tabla `Product` de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)]. Además, se agrega un encabezado de columna.  
  
 [!code-sql[Select#SelectExamples2](../../t-sql/queries/codesnippet/tsql/select-examples-transact_2.sql)]  
  
 En este ejemplo solo se devuelven las filas de `Product` que tienen una línea de productos de `R` y cuyo valor correspondiente a los días para fabricar es inferior a `4`.  
  
 [!code-sql[Select#SelectExamples3](../../t-sql/queries/codesnippet/tsql/select-examples-transact_3.sql)]  
  
## <a name="b-using-select-with-column-headings-and-calculations"></a>B. Usar SELECT con encabezados de columna y cálculos  
 En los siguientes ejemplos se devuelven todas las filas de la tabla `Product`. En el primer ejemplo se devuelven las ventas totales y los descuentos de cada producto. En el segundo ejemplo se calculan los beneficios totales de cada producto.  
  
 [!code-sql[Select#SelectExamples4](../../t-sql/queries/codesnippet/tsql/select-examples-transact_4.sql)]  
  
 Ésta es la consulta que calcula el beneficio de cada producto de cada pedido de venta.  
  
 [!code-sql[Select#SelectExamples5](../../t-sql/queries/codesnippet/tsql/select-examples-transact_5.sql)]  
  
## <a name="c-using-distinct-with-select"></a>C. Usar DISTINCT con SELECT  
 En el siguiente ejemplo se utiliza `DISTINCT` para evitar la recuperación de títulos duplicados.  
  
 [!code-sql[Select#SelectExamples6](../../t-sql/queries/codesnippet/tsql/select-examples-transact_6.sql)]  
  
## <a name="d-creating-tables-with-select-into"></a>D. Crear tablas con SELECT INTO  
 En el primer ejemplo se crea una tabla temporal denominada `#Bicycles` en `tempdb`.  
  
 [!code-sql[Select#SelectExamples7](../../t-sql/queries/codesnippet/tsql/select-examples-transact_7.sql)]  
  
 En el segundo ejemplo se crea la tabla permanente `NewProducts`.  
  
 [!code-sql[Select#SelectExamples8](../../t-sql/queries/codesnippet/tsql/select-examples-transact_8.sql)]  
  
## <a name="e-using-correlated-subqueries"></a>E. Usar subconsultas correlacionadas
Una subconsulta correlacionada es una consulta que depende de la consulta externa para obtener sus valores. Se ejecuta varias veces, una vez por cada fila que la consulta externa pueda seleccionar.

En el primer ejemplo se muestran consultas que son semánticamente equivalentes para demostrar la diferencia entre el uso de la palabra clave `EXISTS` y la palabra clave `IN`. Ambos son ejemplos de subconsultas válidas que recuperan una instancia de cada nombre de producto cuyo modelo es un jersey de manga larga con logotipo y cuyos números de `ProductModelID` coinciden en las tablas `Product` y `ProductModel`.  
  
 [!code-sql[Select#SelectExamples9](../../t-sql/queries/codesnippet/tsql/select-examples-transact_9.sql)]  
  
 En el ejemplo siguiente se usa `IN` y se recupera una instancia del nombre y apellido de cada empleado cuya bonificación en la tabla `SalesPerson` sea de `5000.00` y cuyos números de identificación coincidan en las tablas `Employee` y `SalesPerson`.  
  
 [!code-sql[Select#SelectExamples10](../../t-sql/queries/codesnippet/tsql/select-examples-transact_10.sql)]  
  
 La subconsulta anterior de esta instrucción no se puede evaluar independientemente de la consulta externa. Necesita el valor `Employee.EmployeeID`, aunque este valor cambia a medida que el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] examina diferentes filas de `Employee`.  
  
 Una subconsulta correlativa se puede usar también en la cláusula `HAVING` de una consulta externa. En este ejemplo se buscan los modelos cuyo precio máximo es superior al doble de la media del modelo.  
  
 [!code-sql[Select#SelectExamples11](../../t-sql/queries/codesnippet/tsql/select-examples-transact_11.sql)]  
  
 En este ejemplo se utilizan dos subconsultas correlativas para buscar los nombres de los empleados que han vendido un producto específico.  
  
 [!code-sql[Select#SelectExamples12](../../t-sql/queries/codesnippet/tsql/select-examples-transact_12.sql)]  
  
## <a name="f-using-group-by"></a>F. Usar GROUP BY  
 En este ejemplo se busca el total de cada pedido de venta de la base de datos.  
  
 [!code-sql[Select#SelectExamples13](../../t-sql/queries/codesnippet/tsql/select-examples-transact_13.sql)]  
  
 Debido a la cláusula `GROUP BY`, solo se devuelve una fila que contiene la suma de todas las ventas por cada pedido de venta.  
  
## <a name="g-using-group-by-with-multiple-groups"></a>G. Usar GROUP BY con varios grupos  
 En este ejemplo se busca el precio medio y la suma de las ventas anuales hasta la fecha, agrupados por Id. de producto e Id. de oferta especial.  
  
 [!code-sql[Select#SelectExamples14](../../t-sql/queries/codesnippet/tsql/select-examples-transact_14.sql)]  
  
## <a name="h-using-group-by-and-where"></a>H. Usar GROUP BY y WHERE  
 En el siguiente ejemplo se colocan los resultados en grupos después de recuperar únicamente las filas con precios superiores a `$1000`.  
  
 [!code-sql[Select#SelectExamples15](../../t-sql/queries/codesnippet/tsql/select-examples-transact_15.sql)]  
  
## <a name="i-using-group-by-with-an-expression"></a>I. Usar GROUP BY con una expresión  
 En este ejemplo se agrupa por una expresión. Puede agrupar por una expresión si ésta no incluye funciones de agregado.  
  
 [!code-sql[Select#SelectExamples16](../../t-sql/queries/codesnippet/tsql/select-examples-transact_16.sql)]  
  
## <a name="j-using-group-by-with-order-by"></a>J. Usar GROUP BY con ORDER BY  
 En este ejemplo se busca el precio medio de cada tipo de producto y se ordenan los resultados por precio medio.  
  
 [!code-sql[Select#SelectExamples18](../../t-sql/queries/codesnippet/tsql/select-examples-transact_17.sql)]  
  
## <a name="k-using-the-having-clause"></a>K. Usar la cláusula HAVING  
 En el primer ejemplo se muestra una cláusula `HAVING` con una función de agregado. Agrupa las filas de la tabla `SalesOrderDetail` por Id. de producto y elimina aquellos productos cuyas cantidades de pedido medias son cinco o menos. En el segundo ejemplo se muestra una cláusula `HAVING` sin funciones de agregado.  
  
 [!code-sql[Select#SelectExamples19](../../t-sql/queries/codesnippet/tsql/select-examples-transact_18.sql)]  
  
 En esta consulta se utiliza la cláusula `LIKE` en la cláusula `HAVING`.  
  
```sql
USE AdventureWorks2012 ;  
GO  
SELECT SalesOrderID, CarrierTrackingNumber   
FROM Sales.SalesOrderDetail  
GROUP BY SalesOrderID, CarrierTrackingNumber  
HAVING CarrierTrackingNumber LIKE '4BD%'  
ORDER BY SalesOrderID ;  
GO  
```  
  
## <a name="l-using-having-and-group-by"></a>L. Usar HAVING y GROUP BY  
 En el siguiente ejemplo se muestra el uso de las cláusulas `GROUP BY`, `HAVING`, `WHERE` y `ORDER BY` en una instrucción `SELECT`. Genera grupos y valores de resumen pero lo hace tras eliminar los productos cuyos precios superan los 25 $ y cuyas cantidades de pedido medias son inferiores a 5. También organiza los resultados por `ProductID`.  
  
 [!code-sql[Select#SelectExamples21](../../t-sql/queries/codesnippet/tsql/select-examples-transact_19.sql)]  
  
## <a name="m-using-having-with-sum-and-avg"></a>M. Usar HAVING con SUM y AVG  
 En el siguiente ejemplo se agrupa la tabla `SalesOrderDetail` por Id. de producto y solo se incluyen aquellos grupos de productos cuyos pedidos suman más de `$1000000.00` y cuyas cantidades de pedido medias son inferiores a `3`.  
  
 [!code-sql[Select#SelectExamples22](../../t-sql/queries/codesnippet/tsql/select-examples-transact_20.sql)]  
  
 Para ver los productos cuyas ventas totales son superiores a `$2000000.00`, utilice esta consulta:  
  
 [!code-sql[Select#SelectExamples23](../../t-sql/queries/codesnippet/tsql/select-examples-transact_21.sql)]  
  
 Si desea asegurarse de que hay al menos mil quinientos elementos para los cálculos de cada producto, use `HAVING COUNT(*) > 1500` para eliminar los productos que devuelven totales inferiores a `1500` elementos vendidos. La consulta tiene este aspecto:  
  
 [!code-sql[Select#SelectExamples24](../../t-sql/queries/codesnippet/tsql/select-examples-transact_22.sql)]  
  
## <a name="n-using-the-index-optimizer-hint"></a>Hora Usar la sugerencia del optimizador INDEX  
 En el ejemplo siguiente se muestran dos formas de usar la sugerencia del optimizador `INDEX`. En el primer ejemplo se muestra cómo obligar al optimizador a que use un índice no clúster para recuperar filas de una tabla, mientras que en el segundo ejemplo se obliga a realizar un recorrido de tabla mediante un índice igual a 0.  
  
 [!code-sql[Select#SelectExamples45](../../t-sql/queries/codesnippet/tsql/select-examples-transact_23.sql)]  
  
## <a name="m-using-option-and-the-group-hints"></a>M. Usar OPTION y las sugerencias GROUP  
 En el ejemplo siguiente se muestra cómo se usa la cláusula `OPTION (GROUP)` con una cláusula `GROUP BY`.  
  
 [!code-sql[Select#SelectExamples46](../../t-sql/queries/codesnippet/tsql/select-examples-transact_24.sql)]  
  
## <a name="o-using-the-union-query-hint"></a>O. Usar la sugerencia de consulta UNION  
 En el ejemplo siguiente se usa la sugerencia de consulta `MERGE UNION`.  
  
 [!code-sql[Select#SelectExamples47](../../t-sql/queries/codesnippet/tsql/select-examples-transact_25.sql)]  
  
## <a name="p-using-a-simple-union"></a>P. Usar una instrucción UNION simple  
 En el ejemplo siguiente, el conjunto de resultados incluye el contenido de las columnas `ProductModelID` y `Name` de las tablas `ProductModel` y `Gloves`.  
  
 [!code-sql[Select#SelectExamples48](../../t-sql/queries/codesnippet/tsql/select-examples-transact_26.sql)]  
  
## <a name="q-using-select-into-with-union"></a>Q. Usar SELECT INTO con UNION  
 En el ejemplo siguiente, la cláusula `INTO` de la segunda instrucción `SELECT` especifica que la tabla denominada `ProductResults` contiene el conjunto final de resultados de la unión de las columnas designadas de las tablas `ProductModel` y `Gloves`. Tenga en cuenta que la tabla `Gloves` se crea en la primera instrucción `SELECT`.  
  
 [!code-sql[Select#SelectExamples49](../../t-sql/queries/codesnippet/tsql/select-examples-transact_27.sql)]  
  
## <a name="r-using-union-of-two-select-statements-with-order-by"></a>R. Usar UNION con dos instrucciones SELECT y ORDER BY  
 El orden de algunos parámetros empleados con la cláusula UNION es importante. En el ejemplo siguiente se muestra el uso correcto e incorrecto de `UNION` en dos instrucciones `SELECT` en las que se va a cambiar el nombre de una columna en el resultado.  
  
 [!code-sql[Select#SelectExamples50](../../t-sql/queries/codesnippet/tsql/select-examples-transact_28.sql)]  
  
## <a name="s-using-union-of-three-select-statements-to-show-the-effects-of-all-and-parentheses"></a>S. Usar UNION de tres instrucciones SELECT para mostrar los efectos de ALL y los paréntesis  
 En los siguientes ejemplos se utiliza `UNION` para combinar los resultados de tres tablas que tienen las mismas 5 filas de datos. En el primer ejemplo se utiliza `UNION ALL` para mostrar los registros duplicados y se devuelven las 15 filas. En el segundo ejemplo se utiliza `UNION` sin `ALL` para eliminar las filas duplicadas de los resultados combinados de las tres instrucciones `SELECT` y se devuelven 5 filas.  
  
 En el tercer ejemplo se utiliza `ALL` con el primer `UNION` y los paréntesis incluyen al segundo `UNION` que no utiliza `ALL`. El segundo `UNION` se procesa en primer lugar porque se encuentra entre paréntesis. Devuelve 5 filas porque no se utiliza la opción `ALL` y se quitan los duplicados. Estas 5 filas se combinan con los resultados del primer `SELECT` mediante las palabras clave `UNION ALL`. Esto no quita los duplicados entre los dos conjuntos de 5 filas. El resultado final es de 10 filas.  
  
 [!code-sql[Select#SelectExamples51](../../t-sql/queries/codesnippet/tsql/select-examples-transact_29.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [CREATE TRIGGER &#40;Transact-SQL&#41;](../../t-sql/statements/create-trigger-transact-sql.md)   
 [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Expresiones &#40;Transact-SQL&#41;](../../t-sql/language-elements/expressions-transact-sql.md)   
 [INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)   
 [LIKE &#40;Transact-SQL&#41;](../../t-sql/language-elements/like-transact-sql.md)   
 [UNION &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-union-transact-sql.md)   
 [EXCEPT e INTERSECT &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-operators-except-and-intersect-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [WHERE &#40;Transact-SQL&#41;](../../t-sql/queries/where-transact-sql.md)   
 [PathName &#40;Transact-SQL&#41;](../../relational-databases/system-functions/pathname-transact-sql.md)   
 [INTO Clause &#40;Transact-SQL&#41;](../../t-sql/queries/select-into-clause-transact-sql.md)  
  
  
