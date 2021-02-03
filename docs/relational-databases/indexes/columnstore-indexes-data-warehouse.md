---
description: Almacenamiento de datos de índices de almacén de columnas
title: Almacenamiento de datos de índices de almacén de columnas | Microsoft Docs
ms.custom: ''
ms.date: 12/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: 21fd153b-116d-47fc-a926-f1528299a391
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: cf8e82f8ebaa0a8b5b30cc4ffd460f7db7d4d121
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234864"
---
# <a name="columnstore-indexes---data-warehouse"></a>Almacenamiento de datos de índices de almacén de columnas
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Los índices de almacén de columnas, junto con las particiones, son esenciales para crear un almacenamiento de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="whats-new"></a>Novedades  
 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] presenta estas características para mejorar el rendimiento del almacén de columnas:  
  
-   AlwaysOn permite consultar un índice de almacén de columnas en una réplica secundaria legible.  
-   Multiple Active Result Sets (MARS) admite índices de almacén de columnas.  
-   Una nueva vista de administración dinámica [sys.dm_db_column_store_row_group_physical_stats &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-column-store-row-group-physical-stats-transact-sql.md) proporciona información de solución de problemas de rendimiento en el nivel de grupo de filas.  
-   Las consultas uniproceso de los índices de almacén de columnas se pueden ejecutar en el modo por lotes. Antes, solo las consultas multiproceso se ejecutaban por lotes.  
-   El operador `SORT` se ejecuta en el modo por lotes.  
-   Las operaciones `DISTINCT` múltiples se ejecutan en el modo por lotes.  
-   Los agregados de ventana ahora se ejecutan en modo por lotes para el nivel de compatibilidad de la base de datos 130 y niveles superiores.  
-   Aplicación de agregado para un procesamiento eficaz de los agregados. Se admite en todos los niveles de compatibilidad con bases de datos.  
-   Aplicación de predicado de cadena para el procesamiento eficaz de los predicados de cadena. Se admite en todos los niveles de compatibilidad con bases de datos.  
-   Aislamiento de instantánea en el nivel de compatibilidad de base de datos 130 y niveles superiores.  
  
## <a name="improve-performance-by-combining-nonclustered-and-columnstore-indexes"></a>Mejora del rendimiento al combinar índices de almacén de columnas y no agrupados  
 A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], puede definir índices no agrupados en un índice de almacén de columnas agrupado.   
  
### <a name="example-improve-efficiency-of-table-seeks-with-a-nonclustered-index"></a>Ejemplo: mejorar la eficacia de las búsquedas de tabla con un índice no agrupado  
 Para mejorar la eficacia de las búsquedas de tabla en un almacenamiento de datos, puede crear un índice no agrupado diseñado para ejecutar consultas que realicen las mejores búsquedas de tabla. Por ejemplo, las consultas que buscan valores coincidentes o que devuelven un pequeño intervalo de valores tienen un rendimiento mejor en un índice de árbol B que en un índice de almacén de columnas. No requieren un recorrido de tabla completo a través del índice de almacén de columnas y devolverán el resultado correcto más rápidamente si se realiza una búsqueda binaria a través de un índice de árbol B.  
  
```sql  
--BASIC EXAMPLE: Create a nonclustered index on a columnstore table.  
  
--Create the table  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int  
);  
GO  
  
--Store the table as a columnstore.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account;  
GO  
  
--Add a nonclustered index.  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
```  
  
### <a name="example-use-a-nonclustered-index-to-enforce-a-primary-key-constraint-on-a-columnstore-table"></a>Ejemplo: usar un índice no agrupado para aplicar una restricción de clave principal en una tabla de almacén de columnas  
 Por diseño, una tabla de almacén de columnas no permite una restricción de clave principal agrupada. Ahora puede usar un índice no agrupado en una tabla de almacén de columnas para aplicar restricciones de clave principal. Una clave principal equivale a una restricción UNIQUE en una columna no NULL y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa una restricción UNIQUE como un índice no agrupado. Combinando ambos factores, en el siguiente ejemplo se define una restricción UNIQUE en la accountkey de la columna no NULL. El resultado es un índice no agrupado que aplica una restricción de clave principal como una restricción UNIQUE en una columna no NULL.  
  
 Luego, la tabla se convierte en un índice de almacén de columnas agrupado. Durante la conversión, se conserva el índice no agrupado. El resultado es un índice de almacén de columnas agrupado con un índice no agrupado que aplica una restricción de clave principal. Dado que cualquier actualización o inserción en la tabla de almacén de columnas también afectará al índice no agrupado, todas las operaciones que infrinjan la restricción UNIQUE y el valor no NULL harán que se produzca un error en toda la operación.  
  
 El resultado es un índice de almacén de columnas con un índice no agrupado que aplica una restricción de clave principal en ambos índices.  
  
```sql
--EXAMPLE: Enforce a primary key constraint on a columnstore table.   
  
--Create a rowstore table with a unique constraint.  
--The unique constraint is implemented as a nonclustered index.  
CREATE TABLE t_account (  
    AccountKey int NOT NULL,  
    AccountDescription nvarchar (50),  
    AccountType nvarchar(50),  
    UnitSold int,  
  
    CONSTRAINT uniq_account UNIQUE (AccountKey)  
);  
  
--Store the table as a columnstore.   
--The unique constraint is preserved as a nonclustered index on the columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX t_account_cci ON t_account  
  
--By using the previous two steps, every row in the table meets the UNIQUE constraint  
--on a non-NULL column.  
--This has the same end-result as having a primary key constraint  
--All updates and inserts must meet the unique constraint on the nonclustered index or they will fail.  
  
--If desired, add a foreign key constraint on AccountKey.  
  
ALTER TABLE [dbo].[t_account]  
WITH CHECK ADD FOREIGN KEY([AccountKey]) REFERENCES my_dimension(Accountkey); 
```  
  
### <a name="improve-performance-by-enabling-row-level-and-row-group-level-locking"></a>Mejorar el rendimiento al habilitar el bloqueo de nivel de fila y de grupo de filas  
 Para complementar al índice no agrupado en una característica de índice de almacén de columnas [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] ofrece una función de bloqueo pormenorizado para seleccionar, actualizar y eliminar operaciones. Se pueden ejecutar consultas con un bloqueo de nivel de fila en las búsquedas de índice en un índice no agrupado y con un bloqueo de nivel de grupo de filas en los recorridos de tabla completa en el índice de almacén de columnas. Use el bloqueo de nivel de fila y el bloqueo de nivel de grupo de filas adecuadamente para conseguir una mayor simultaneidad de lectura y escritura.  
  
```sql  
--Granular locking example  
--Store table t_account as a columnstore table.  
CREATE CLUSTERED COLUMNSTORE INDEX taccount_cci ON t_account  
GO  
  
--Add a nonclustered index for use with this example  
CREATE UNIQUE INDEX taccount_nc1 ON t_account (AccountKey);  
GO  
  
--Look at locking with access through the nonclustered index  
SET TRANSACTION ISOLATION LEVEL repeatable read;  
GO  
  
BEGIN TRAN  
    -- The query plan chooses a seek operation on the nonclustered index  
    -- and takes the row lock  
    SELECT * FROM t_account WHERE AccountKey = 100;  
END TRAN  
```  
  
### <a name="snapshot-isolation-and-read-committed-snapshot-isolations"></a>Aislamiento de instantánea y aislamientos de instantánea de lectura confirmada  
 Use el aislamiento de instantánea (SI) para garantizar la coherencia transaccional y los aislamientos de instantánea de lectura confirmada (RCSI) para garantizar la coherencia de nivel de instrucción de las consultas en índices de almacén de columnas. Esto permite que las consultas se ejecuten sin bloquear escrituras de datos. Este comportamiento de no bloqueo también reduce notablemente la probabilidad de que se produzcan interbloqueos en las transacciones complejas. Para obtener más información, consulte [Aislamiento de instantáneas en SQL Server](https://msdn.microsoft.com/library/tcbchxcb\(v=vs.110\).aspx) en MSDN.  
  
## <a name="see-also"></a>Consulte también  
 [Guía de diseño de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-design-guidance.md)   
 [Guía de carga de datos de los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Rendimiento de las consultas de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Introducción al almacén de columnas para análisis operativos en tiempo real](../../relational-databases/indexes/get-started-with-columnstore-for-real-time-operational-analytics.md)   
 [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)    
 [Diseño de los índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index) 
  
