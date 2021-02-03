---
description: OPTION (cláusula de Transact-SQL)
title: OPTION (cláusula de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- OPTION clause
- OPTION_TSQL
- OPTION
- OPTION_clause_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- clauses [SQL Server], OPTION
- OPTION clause
ms.assetid: f47e2f3f-9302-4711-9d66-16b1a2a7ffe3
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: da4c8380b4370e8194da299b498d1934b29d6917
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207689"
---
# <a name="option-clause-transact-sql"></a>OPTION (cláusula de Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Especifica que en toda la consulta se debe utilizar la sugerencia de consulta especificada. Solo se puede especificar cada sugerencia de consulta una vez, aunque se permiten varias sugerencias de consulta. Solo se puede especificar una cláusula OPTION con la instrucción.  
  
 Esta cláusula se puede especificar en las instrucciones SELECT, DELETE, UPDATE y MERGE.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
[ OPTION ( <query_hint> [ ,...n ] ) ]   
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
OPTION ( <query_option> [ ,...n ] )  
  
<query_option> ::=  
    LABEL = label_name |  
    <query_hint>  
  
<query_hint> ::=  
    HASH JOIN   
    | LOOP JOIN   
    | MERGE JOIN  
    | FORCE ORDER  
    | { FORCE | DISABLE } EXTERNALPUSHDOWN  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *query_hint*  
 Palabras clave que indican qué sugerencias del optimizador se emplean para personalizar la forma en que el Motor de base de datos procesa la instrucción. Para obtener más información, vea [Sugerencias de consulta &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql-query.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-an-option-clause-with-a-group-by-clause"></a>A. Usar una cláusula OPTION con una cláusula GROUP BY  
 En el ejemplo siguiente se muestra cómo se usa la cláusula `OPTION` con una cláusula `GROUP BY`.  
  
```sql
USE AdventureWorks2012;  
GO  
SELECT ProductID, OrderQty, SUM(LineTotal) AS Total  
FROM Sales.SalesOrderDetail  
WHERE UnitPrice < $5.00  
GROUP BY ProductID, OrderQty  
ORDER BY ProductID, OrderQty  
OPTION (HASH GROUP, FAST 10);  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-select-statement-with-a-label-in-the-option-clause"></a>B. Instrucción SELECT con una etiqueta en la cláusula OPTION  
 En el ejemplo siguiente se muestra una instrucción SELECT simple de [!INCLUDE[ssDW](../../includes/ssdw-md.md)] con una etiqueta en la cláusula OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT * FROM FactResellerSales  
  OPTION ( LABEL = 'q17' );  
```  
  
### <a name="c-select-statement-with-a-query-hint-in-the-option-clause"></a>C. Instrucción SELECT con una sugerencia de consulta en la cláusula OPTION  
 En el ejemplo siguiente se muestra una instrucción SELECT que usa una sugerencia de consulta HASH JOIN en la cláusula OPTION.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
```  
  
### <a name="d-select-statement-with-a-label-and-multiple-query-hints-in-the-option-clause"></a>D. Instrucción SELECT con una etiqueta y varias sugerencias de consulta en la cláusula OPTION  
 El ejemplo siguiente es una instrucción SELECT de [!INCLUDE[ssDW](../../includes/ssdw-md.md)] que contiene una etiqueta y varias sugerencias de consulta. Cuando se ejecuta la consulta en los nodos de proceso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aplica una combinación hash o una combinación de mezcla, según la estrategia que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considere óptima.  
  
```sql
-- Uses AdventureWorks  
  
SELECT COUNT (*) FROM dbo.DimCustomer a  
INNER JOIN dbo.FactInternetSales b   
ON (a.CustomerKey = b.CustomerKey)  
OPTION ( Label = 'CustJoin', HASH JOIN, MERGE JOIN);  
```  
  
### <a name="e-using-a-query-hint-when-querying-a-view"></a>E. Usar una sugerencia de consulta al consultar a una vista  
 En el ejemplo siguiente se crea una vista denominada CustomerView y luego se usa una sugerencia de consulta HASH JOIN en una consulta que hace referencia a una vista y a una tabla.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView  
AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT COUNT (*) FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON (a.CustomerKey = b.CustomerKey)  
OPTION (HASH JOIN);  
  
DROP VIEW CustomerView;
```  
  
### <a name="f-query-with-a-subselect-and-a-query-hint"></a>F. Consultar con una subselección y una sugerencia de consulta  
 En el ejemplo siguiente se muestra una consulta que contiene una subselección y una sugerencia de consulta. La sugerencia de consulta se aplica de forma global. No se permite anexar sugerencias de consulta a la instrucción de subselección.  
  
```sql
-- Uses AdventureWorks  
  
CREATE VIEW CustomerView AS  
SELECT CustomerKey, FirstName, LastName FROM ssawPDW..DimCustomer;  
  
SELECT * FROM (  
SELECT COUNT (*) AS a FROM dbo.CustomerView a  
INNER JOIN dbo.FactInternetSales b  
ON ( a.CustomerKey = b.CustomerKey )) AS t  
OPTION (HASH JOIN);  
```  
  
### <a name="g-force-the-join-order-to-match-the-order-in-the-query"></a>G. Hacer coincidir el orden de combinación con el orden de la consulta  
 En el ejemplo siguiente se usa la sugerencia FORCE ORDER para hacer que el plan de consulta emplee el orden de combinación especificado por la consulta. Esto mejora el rendimiento de algunas consultas, no de todas.  
  
```sql
-- Uses AdventureWorks  
  
-- Obtain partition numbers, boundary values, boundary value types, and rows per boundary  
-- for the partitions in the ProspectiveBuyer table of the ssawPDW database.  
SELECT sp.partition_number, prv.value AS boundary_value, lower(sty.name) AS boundary_value_type, sp.rows   
FROM sys.tables st JOIN sys.indexes si ON st.object_id = si.object_id AND si.index_id <2  
JOIN sys.partitions sp ON sp.object_id = st.object_id AND sp.index_id = si.index_id  
JOIN sys.partition_schemes ps ON ps.data_space_id = si.data_space_id   
JOIN sys.partition_range_values prv ON prv.function_id = ps.function_id   
JOIN sys.partition_parameters pp ON pp.function_id = ps.function_id   
JOIN sys.types sty ON sty.user_type_id = pp.user_type_id AND prv.boundary_id = sp.partition_number   
WHERE st.object_id = (SELECT object_id FROM sys.objects WHERE name = 'FactResellerSales')   
ORDER BY sp.partition_number  
OPTION ( FORCE ORDER )  
;  
```  
  
### <a name="h-using-externalpushdown"></a>H. Usar EXTERNALPUSHDOWN  
 En el ejemplo siguiente se fuerza a la aplicación de la cláusula WHERE al trabajo MapReduce en la tabla externa de Hadoop.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 1000000  
OPTION (FORCE EXTERNALPUSHDOWN);  
```  
  
 En el ejemplo siguiente se evita la aplicación de la cláusula WHERE al trabajo MapReduce en la tabla externa de Hadoop. Todas las filas se devuelven a PDW, donde se aplica la cláusula WHERE.  
  
```sql
SELECT ID FROM External_Table_AS A   
WHERE ID < 10  
OPTION (DISABLE EXTERNALPUSHDOWN);  
```  
  
## <a name="see-also"></a>Consulte también  
 [Hints &#40;Transact-SQL&#41;](../../t-sql/queries/hints-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)   
 [MERGE &#40;Transact-SQL&#41;](../../t-sql/statements/merge-transact-sql.md)   
 [DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
  
  

