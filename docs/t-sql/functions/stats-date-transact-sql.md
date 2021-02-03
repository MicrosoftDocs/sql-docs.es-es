---
description: STATS_DATE (Transact-SQL)
title: STATS_DATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- STATS_DATE_TSQL
- STATS_DATE
dev_langs:
- TSQL
helpviewer_keywords:
- statistical information [SQL Server], last time updated
- STATS_DATE function
- query optimization statistics [SQL Server], last time updated
- last time statistics updated
- stats update date
ms.assetid: f9ec3101-1e41-489d-b519-496a0d6089fb
author: julieMSFT
ms.author: jrasnick
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ca416d3de8d295cd9f7f535ab2d3b8b65df150f6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181254"
---
# <a name="stats_date-transact-sql"></a>STATS_DATE (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve la fecha de la actualización más reciente de las estadísticas de una tabla o vista indizada.  
  
 Para más información sobre cómo actualizar las estadísticas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
STATS_DATE ( object_id , stats_id )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *object_id*  
 Identificador de la tabla o vista indizada con las estadísticas.  
  
 *stats_id*  
 Identificador del objeto de estadísticas.  
  
## <a name="return-types"></a>Tipos de valor devueltos  
 Devuelve **datetime** si es correcto. Devuelve **NULL** si no se creó un blob de estadísticas.  
  
## <a name="remarks"></a>Observaciones  
 Las funciones del sistema se pueden utilizar en la lista de selección, en la cláusula WHERE y en cualquier lugar donde se permita una expresión.  
 
 La fecha de actualización de estadísticas se almacena en el [objeto BLOB de estadísticas](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) junto con el [histograma](../../relational-databases/statistics/statistics.md#histogram) y el [vector de densidad](../../relational-databases/statistics/statistics.md#density), pero no en los metadatos. Cuando no se lee ningún dato con el que generar datos de estadísticas, el BLOB de estadísticas no se crea y la fecha no está disponible. Esto sucede en las estadísticas filtradas, en las que el predicado no devuelve ninguna fila, o en las tablas nuevas vacías.
 
 Si las estadísticas corresponden a un índice, el valor de *stats_id* de la vista de catálogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) es el mismo que el valor de *index_id* de la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).
  
## <a name="permissions"></a>Permisos  
 Es necesaria la pertenencia al rol fijo de servidor db_owner o al rol fijo de base de datos maestra o al rol fijo de servidor sysadmin.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-return-the-dates-of-the-most-recent-statistics-for-a-table"></a>A. Devolver las fechas de las estadísticas más recientes de una tabla  
 En el ejemplo siguiente, se devuelve la fecha de la actualización más reciente de cada objeto de estadísticas de la tabla `Person.Address`.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_update_date  
FROM sys.stats   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
 Si las estadísticas corresponden a un índice, el valor de *stats_id* de la vista de catálogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) es el mismo que el valor de *index_id* de la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md), y la consulta siguiente devuelve los mismos resultados que la consulta precedente. Si las estadísticas no corresponden a un índice, aparecen en los resultados de sys.stats pero no en los de sys.indexes.  
  
```sql  
USE AdventureWorks2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('Person.Address');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="b-learn-when-a-named-statistics-was-last-updated"></a>B. Saber cuándo se actualizaron las estadísticas con nombre por última vez  
 En el siguiente ejemplo se crean estadísticas basadas en la columna LastName de la tabla DimCustomer. Luego, se ejecuta una consulta para mostrar la fecha de las estadísticas. Por último, las estadísticas se actualizan y la consulta se vuelve ejecutar para mostrar la fecha de actualización.  
  
```sql
--First, create a statistics object  
USE AdventureWorksPDW2012;  
GO  
CREATE STATISTICS Customer_LastName_Stats  
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName)  
WITH SAMPLE 50 PERCENT;  
GO  
  
--Return the date when Customer_LastName_Stats was last updated  
USE AdventureWorksPDW2012;  
GO  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO  
  
--Update Customer_LastName_Stats so it will have a different timestamp in the next query  
GO  
UPDATE STATISTICS dbo.dimCustomer (Customer_LastName_Stats);  
  
--Return the date when Customer_LastName_Stats was last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer')  
    AND s.name = 'Customer_LastName_Stats';  
GO    
```  
  
### <a name="c-view-the-date-of-the-last-update-for-all-statistics-on-a-table"></a>C. Ver la fecha de última actualización de todas las estadísticas en una tabla  
 En este ejemplo se devuelve la fecha de última actualización de cada objeto de estadísticas en la tabla DimCustomer.  
  
```sql  
--Return the dates all statistics on the table were last updated.  
SELECT stats_id, name AS stats_name,   
    STATS_DATE(object_id, stats_id) AS statistics_date  
FROM sys.stats s  
WHERE s.object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
 Si las estadísticas corresponden a un índice, el valor de *stats_id* de la vista de catálogo [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md) es el mismo que el valor de *index_id* de la vista de catálogo [sys.indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md), y la consulta siguiente devuelve los mismos resultados que la consulta precedente. Si las estadísticas no corresponden a un índice, aparecen en los resultados de sys.stats pero no en los de sys.indexes.  
  
```sql  
USE AdventureWorksPDW2012;  
GO  
SELECT name AS index_name,   
    STATS_DATE(object_id, index_id) AS statistics_update_date  
FROM sys.indexes   
WHERE object_id = OBJECT_ID('dbo.DimCustomer');  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md)    
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)   
 [sys.stats](../../relational-databases/system-catalog-views/sys-stats-transact-sql.md)   
  
  

