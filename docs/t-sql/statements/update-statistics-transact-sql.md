---
description: UPDATE STATISTICS (Transact-SQL)
title: UPDATE STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/04/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- UPDATE STATISTICS
- UPDATE_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- updating statistics
- query optimization statistics [SQL Server], updating
- UPDATE STATISTICS statement
- statistical information [SQL Server], updating
ms.assetid: 919158f2-38d0-4f68-82ab-e1633bd0d308
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ab5b7fac995e54160798d2f2c3113e391a84340d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100349986"
---
# <a name="update-statistics-transact-sql"></a>UPDATE STATISTICS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Actualiza las estadísticas de optimización de consulta para una tabla o vista indizada. De forma predeterminada, el optimizador de consultas ya actualiza las estadísticas como requisito para mejorar el plan de consulta; en algunos casos puede mejorar el rendimiento de las consultas usando `UPDATE STATISTICS` o el procedimiento almacenado [sp_updatestats](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md) para actualizar las estadísticas con más frecuencia que la de las actualizaciones predeterminadas.  
  
La actualización de las estadísticas asegura que las consultas se compilan con estadísticas actualizadas. Sin embargo, la actualización de las estadísticas hace que las consultas se vuelvan a compilar. Recomendamos no actualizar las estadísticas con demasiada frecuencia, porque hay que elegir el punto válido entre la mejora de los planes de consulta y el tiempo empleado en volver a compilar las consultas. Las compensaciones específicas dependen de su aplicación. `UPDATE STATISTICS` puede usar tempdb para ordenar la muestra de filas con fines de creación de estadísticas.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
UPDATE STATISTICS table_or_indexed_view_name   
    [   
        {   
            { index_or_statistics__name }  
          | ( { index_or_statistics_name } [ ,...n ] )   
                }  
    ]   
    [    WITH   
        [  
            FULLSCAN   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | SAMPLE number { PERCENT | ROWS }   
              [ [ , ] PERSIST_SAMPLE_PERCENT = { ON | OFF } ]    
            | RESAMPLE   
              [ ON PARTITIONS ( { <partition_number> | <range> } [, ...n] ) ]  
            | <update_stats_stream_option> [ ,...n ]  
        ]   
        [ [ , ] [ ALL | COLUMNS | INDEX ]   
        [ [ , ] NORECOMPUTE ]   
        [ [ , ] INCREMENTAL = { ON | OFF } ] 
        [ [ , ] MAXDOP = max_degree_of_parallelism ] 
    ] ;  
  
<update_stats_stream_option> ::=  
    [ STATS_STREAM = stats_stream ]  
    [ ROWCOUNT = numeric_constant ]  
    [ PAGECOUNT = numeric_contant ]  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
UPDATE STATISTICS [ schema_name . ] table_name   
    [ ( { statistics_name | index_name } ) ]  
    [ WITH   
       {  
              FULLSCAN   
            | SAMPLE number PERCENT   
            | RESAMPLE   
        }  
    ]  
[;]  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Argumentos
 *table_or_indexed_view_name*  
 Es el nombre de la tabla o la vista indizada que contiene el objeto de estadísticas.  
  
 *index_or_statistics_name*  
 Es el nombre del índice cuyas estadísticas se van a actualizar o el nombre de las estadísticas que actualizar. Si no se especifica *index_or_statistics_name*, el optimizador de consultas actualiza todas las estadísticas para la tabla o la vista indizada. Esto incluye las estadísticas creadas usando la instrucción CREATE STATISTICS en las columnas, las estadísticas de columna única creadas cuando se usa AUTO_CREATE_STATISTICS y las estadísticas creadas para los índices.  
  
 Para más información sobre AUTO_CREATE_STATISTICS, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md). Para ver todos los índices para una tabla o vista, puede usar [sp_helpindex](../../relational-databases/system-stored-procedures/sp-helpindex-transact-sql.md).  
  
 FULLSCAN  
 Para calcular las estadísticas, examine todas las filas de la tabla o la vista indizada. FULLSCAN y SAMPLE 100 PERCENT tienen los mismos resultados. FULLSCAN no se puede utilizar con la opción SAMPLE.  
  
 SAMPLE *number* { PERCENT | ROWS }  
 Especifique el porcentaje aproximado o número de filas de la tabla o vista indizada que el optimizador de consultas usará al actualizar las estadísticas. En PERCENT, *number* puede tener un valor comprendido entre 0 y 100, mientras que en ROWS, *number* puede tener un valor comprendido entre 0 y el número total de filas. El porcentaje o número de filas real de los ejemplos del optimizador de consultas podría no coincidir con el porcentaje o el número especificado. Por ejemplo, el optimizador de consultas examina todas las filas en una página de datos.  
  
 SAMPLE es útil para los casos especiales en los que el plan de consulta, basado en el muestreo predeterminado, no es óptimo. En la mayoría de las situaciones, no es necesario especificar SAMPLE porque el optimizador de consultas utiliza el muestreo y determina el tamaño de muestra estadísticamente significativo de forma predeterminada, tal y como se exige para crear planes de consulta de alta calidad. 
 
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el muestreo de datos para generar estadísticas se realiza en paralelo (en el nivel de compatibilidad 130) para mejorar el rendimiento de recopilación de estadísticas. El optimizador de consultas usará las estadísticas de ejemplo paralelas cada vez que un tamaño de tabla supere un determinado umbral. 
   
 SAMPLE no se puede utilizar con la opción FULLSCAN. Cuando no se especifica SAMPLE ni FULLSCAN, el optimizador de consultas utiliza los datos muestreados y calcula el tamaño de la muestra de forma predeterminada.  
  
 Recomendamos no especificar 0 PERCENT ni 0 ROWS. Cuando se especifican 0 PERCENT o ROWS, el objeto de estadísticas se actualiza pero no contiene datos de estadísticas.  
  
 Para la mayoría de las cargas de trabajo, no es necesario hacer un examen completo, sino tan solo un muestreo predeterminado.  
Pero para ciertas cargas de trabajo que son sensibles a distribuciones de datos muy diferentes, puede que sea necesario un tamaño de muestreo mayor o incluso un examen completo.  
Para más información, vea el blog [CSS SQL Server Engineers](/archive/blogs/psssql/sampling-can-produce-less-accurate-statistics-if-the-data-is-not-evenly-distributed) (Ingenieros de CSS SQL Server).  
  
 RESAMPLE  
 Se actualiza cada estadística utilizando su velocidad de muestra más reciente.  
  
 El uso de RESAMPLE puede producir un recorrido de tabla completo. Por ejemplo, las estadísticas de los índices utilizan un recorrido de tabla completo como su velocidad de muestra. Cuando no se especifica ninguna de las opciones de muestreo (SAMPLE, FULLSCAN ni RESAMPLE), el optimizador de consultas muestrea los datos y calcula el tamaño de la muestra de forma predeterminada.  

PERSIST_SAMPLE_PERCENT = { ON | OFF }  
Cuando es **ON**, las estadísticas conservan el porcentaje de muestreo definido para las actualizaciones posteriores que no especifican explícitamente un porcentaje de muestreo. Cuando es **OFF**, el porcentaje de muestreo de estadísticas se restablece al muestreo predeterminado en actualizaciones posteriores que no especifiquen explícitamente un porcentaje de muestreo. El valor predeterminado es **OFF**. 
 
 > [!NOTE]
 > Si se ejecuta AUTO_UPDATE_STATISTICS, usa el porcentaje de muestreo persistente (si está disponible) o, de no ser así, el porcentaje de muestreo predeterminado.
 > El comportamiento de RESAMPLE no se ve afectado por esta opción.
 
 > [!NOTE]
 > Si se trunca la tabla, todas las estadísticas creadas en el HoBT truncado volverán a usar el porcentaje de muestreo predeterminado.
 
 > [!TIP] 
 > [DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md) y [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) exponen el valor de porcentaje de muestreo persistente para la estadística seleccionada.
 
 **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4) y versiones posteriores (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU1)  
 
 ON PARTITIONS ( { \<partition_number> | \<range> } [, ...n] ) ] Obliga a que se recalculen las estadísticas de nivel de hoja que abarcan las particiones especificadas en la cláusula ON PARTITIONS y, después, se combinen para generar las estadísticas globales. WITH RESAMPLE es necesario porque no se pueden combinar estadísticas de partición generadas con distintas frecuencias de muestreo.  
  
**Válido para** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.
  
 ALL | COLUMNS | INDEX  
 Actualice todas las estadísticas existentes, las estadísticas creadas en una o más columnas, o las estadísticas creadas para los índices. Si no se especifica ninguna de las opciones, la instrucción UPDATE STATISTICS actualiza todas las estadísticas en la tabla o vista indizada.  
  
 NORECOMPUTE  
 Deshabilite la opción automática de actualización de las estadísticas, AUTO_UPDATE_STATISTICS, para las estadísticas especificadas. Si se especifica esta opción, el optimizador de consultas completa esta actualización de estadísticas y deshabilita las actualizaciones futuras.  
  
 Para rehabilitar el comportamiento de la opción AUTO_UPDATE_STATISTICS, ejecute de nuevo UPDATE STATISTICS sin la opción NORECOMPUTE o ejecute **sp_autostats**.  
  
> [!WARNING]  
> Utilizar esta opción puede producir planes de consulta poco óptimos. Se recomienda usar esta opción con moderación y que lo haga únicamente un administrador de sistemas cualificado.  
  
 Para obtener más información sobre la opción AUTO_STATISTICS_UPDATE, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
 INCREMENTAL = { ON | OFF }  
 Cuando se establece en **ON**, las estadísticas se vuelven a crear como estadísticas por partición. Cuando se establece en **OFF**, se quita el árbol de estadísticas y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] recalcula las estadísticas. El valor predeterminado es **OFF**.  
  
 Si no se admiten las estadísticas por partición, se genera un error. Las estadísticas incrementales no se admiten para los siguientes tipos de estadísticas:  
  
-   Estadísticas creadas con índices que no están alineados por partición con la tabla base.  
-   Estadísticas creadas sobre bases de datos secundarias legibles AlwaysOn.  
-   Estadísticas creadas sobre bases de datos de solo lectura.  
-   Estadísticas creadas sobre índices filtrados.  
-   Estadísticas creadas sobre vistas.  
-   Estadísticas creadas sobre tablas internas.  
-   Estadísticas creadas con índices espaciales o índices XML.  
  
**Válido para** [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] y versiones posteriores.

MAXDOP = *max_degree_of_parallelism*  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] CU3).  
  
 Reemplaza la opción de configuración **max degree of parallelism** durante la operación estadística. Para obtener más información, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md). Utilice MAXDOP para establecer un límite para el número de procesadores utilizados en la ejecución de un plan paralelo. El máximo es 64 procesadores.  
  
 *max_degree_of_parallelism* puede tener estos valores:  
  
 1  
 Suprime la generación de planes paralelos.  
  
 \>1  
 Restringe el número máximo de procesadores usados en una operación estadística paralela al número especificado o a un número inferior, en función de la carga de trabajo actual del sistema.  
  
 0 (predeterminado)  
 Usa el número real de procesadores o menos, según la carga de trabajo actual del sistema.  
  
 \<update_stats_stream_option> 
 
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  

## <a name="remarks"></a>Observaciones  
  
### <a name="when-to-use-update-statistics"></a>Cuándo utilizar UPDATE STATISTICS  
 Para obtener más información sobre cuándo usar `UPDATE STATISTICS`, vea [Estadísticas](../../relational-databases/statistics/statistics.md).  

### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
* No se admite la actualización de estadísticas en tablas externas. Para actualizar las estadísticas en una tabla externa, quite las estadísticas y vuelva a crearlas.  
* La opción `MAXDOP` no es compatible con las opciones `STATS_STREAM`, `ROWCOUNT` y `PAGECOUNT`.
* La opción `MAXDOP` está limitada por la configuración `MAX_DOP` del grupo de cargas de trabajo de Resource Governor, si se usa.

### <a name="updating-all-statistics-with-sp_updatestats"></a>Actualizar todas las estadísticas con sp_updatestats  
Para obtener más información sobre cómo actualizar las estadísticas para todas las tablas internas y definidas por el usuario de la base de datos, vea el procedimiento almacenado [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md). Por ejemplo, el comando siguiente llama a sp_updatestats para actualizar todas las estadísticas de la base de datos.  
  
```sql  
EXEC sp_updatestats;  
```  

### <a name="automatic-index-and-statistics-management"></a>Administración automática de índice y estadísticas
Aproveche soluciones como la [desfragmentación de índice adaptable](https://github.com/Microsoft/tigertoolbox/tree/master/AdaptiveIndexDefrag) para administrar automáticamente las actualizaciones de estadísticas y la desfragmentación de índices para una o varias bases de datos. Este procedimiento elige automáticamente si se debe volver a generar o reorganizar un índice según su nivel de fragmentación, entre otros parámetros y actualiza las estadísticas con un umbral lineal.
  
### <a name="determining-the-last-statistics-update"></a>Determinar la actualización de estadísticas más reciente  
 Para saber cuándo se actualizaron las estadísticas por última vez, use la función [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) .  
  
### <a name="pdw--azure-synapse-analytics"></a>PDW / Azure Synapse Analytics  
 La sintaxis siguiente no es compatible con [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] / [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)].  
  
```sql
UPDATE STATISTICS t1 (a,b);   
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH SAMPLE 10 ROWS;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH NORECOMPUTE;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH INCREMENTAL = ON;  
```  
  
```sql  
UPDATE STATISTICS t1 (a) WITH stats_stream = 0x01;  
```  
  
## <a name="permissions"></a>Permisos  
 Debe tener un permiso de `ALTER` sobre la tabla o vista.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-update-all-statistics-on-a-table"></a>A. Actualizar todas las estadísticas en una tabla  
 En este ejemplo se actualizan las estadísticas de todos los índices de la tabla `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail;  
GO  
```  
  
### <a name="b-update-the-statistics-for-an-index"></a>B. Actualizar las estadísticas para un índice  
 En este ejemplo se actualizan las estadísticas del índice `AK_SalesOrderDetail_rowguid` de la tabla `SalesOrderDetail`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Sales.SalesOrderDetail AK_SalesOrderDetail_rowguid;  
GO  
```  
  
### <a name="c-update-statistics-by-using-50-percent-sampling"></a>C. Actualizar las estadísticas con un muestreo del 50 %  
 En este ejemplo se crean y, después, se actualizan las estadísticas de las columnas `Name` y `ProductNumber` de la tabla `Product`.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE STATISTICS Products  
    ON Production.Product ([Name], ProductNumber)  
    WITH SAMPLE 50 PERCENT  
-- Time passes. The UPDATE STATISTICS statement is then executed.  
UPDATE STATISTICS Production.Product(Products)   
    WITH SAMPLE 50 PERCENT;  
```  
  
### <a name="d-update-statistics-by-using-fullscan-and-norecompute"></a>D. Actualizar estadísticas utilizando FULLSCAN y NORECOMPUTE  
 En este ejemplo se actualizan las estadísticas de `Products` de la tabla `Product`, se exige un examen completo de todas las filas de la tabla `Product` y se desactivan las estadísticas automáticas para las estadísticas de `Products`.  
  
```sql  
USE AdventureWorks2012;  
GO  
UPDATE STATISTICS Production.Product(Products)  
    WITH FULLSCAN, NORECOMPUTE;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="e-update-statistics-on-a-table"></a>E. Actualizar estadísticas en una tabla  
 En este ejemplo se actualizan las estadísticas de `CustomerStats1` en la tabla `Customer`.  
  
```sql  
UPDATE STATISTICS Customer ( CustomerStats1 );  
```  
  
### <a name="f-update-statistics-by-using-a-full-scan"></a>F. Actualizar estadísticas mediante un examen completo  
 En este ejemplo se actualizan las estadísticas de `CustomerStats1`, en función del examen de todas las filas de la tabla `Customer`.  
  
```sql  
UPDATE STATISTICS Customer (CustomerStats1) WITH FULLSCAN;  
```  
  
### <a name="g-update-all-statistics-on-a-table"></a>G. Actualizar todas las estadísticas en una tabla  
 En este ejemplo se actualizan todas las estadísticas en la tabla `Customer`.  
  
```sql  
UPDATE STATISTICS Customer;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)   
 [DBCC SHOW_STATISTICS &#40;Transact-SQL&#41;](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md)   
 [DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)   
 [sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)   
 [sp_updatestats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-updatestats-transact-sql.md)   
 [STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)  
 [sys.dm_db_stats_properties &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)    
 [sys.dm_db_stats_histogram &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)