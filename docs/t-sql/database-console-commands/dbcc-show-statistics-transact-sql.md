---
description: DBCC SHOW_STATISTICS (Transact-SQL)
title: DBCC SHOW_STATISTICS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/18/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SHOW_STATISTICS_TSQL
- DBCC SHOW_STATISTICS
- SHOW_STATISTICS
- DBCC_SHOW_STATISTICS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- query optimization statistics [SQL Server], densities
- histograms [SQL Server]
- statistical information [SQL Server], viewing statistics objects
- current distribution statistics
- query optimization statistics [SQL Server], histograms
- DBCC SHOW_STATISTICS statement
- distribution statistics
- statistical information [SQL Server], densities
- statistics objects
- statistical information [SQL Server], histograms
- query optimization statistics [SQL Server], viewing statistics objects
- statistical information [SQL Server], distribution
- densities [SQL Server]
- displaying distribution statistics
ms.assetid: 12be2923-7289-4150-b497-f17e76a50b2e
author: pmasl
ms.author: umajay
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 375a31ab7ec07bdbc35251d22940a17df047e578
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237466"
---
# <a name="dbcc-show_statistics-transact-sql"></a>DBCC SHOW_STATISTICS (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

DBCC SHOW_STATISTICS muestra las estadísticas de optimización de consulta actuales de una tabla o vista indizada. El optimizador de consultas usa las estadísticas para estimar la cardinalidad o el número de filas del resultado de la consulta, lo que permite que el optimizador de consultas cree un plan de consultas de alta calidad. Por ejemplo, el optimizador de consultas podría utilizar las estimaciones de cardinalidad para elegir el operador index seek en lugar del operador index scan en el plan de consulta, lo que mejoraría el rendimiento de las consultas al evitar el examen de índices con una gran cantidad de recursos.
  
El optimizador de consultas almacena las estadísticas de una tabla o vista indizada en un objeto de estadísticas. En una tabla, el objeto de estadísticas se crea en un índice o en una lista de columnas de la tabla. El objeto de estadísticas incluye un encabezado con metadatos sobre las estadísticas, un histograma con la distribución de valores de la primera columna de clave del objeto de estadísticas y un vector de la densidad para medir la correlación entre las columnas. [!INCLUDE[ssDE](../../includes/ssde-md.md)] puede calcular las estimaciones de cardinalidad con cualquiera de los datos del objeto de estadísticas. Para obtener más información, vea [Estadísticas](../../relational-databases/statistics/statistics.md) y [Estimación de cardinalidad (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md).
  
DBCC SHOW_STATISTICS muestra el encabezado, el histograma y el vector de densidad en función de los datos guardados en el objeto de estadísticas. La sintaxis le permite especificar una tabla o vista indizada junto con un nombre del índice de destino, un nombre de las estadísticas o un nombre de columna. En este tema se describe cómo se muestran las estadísticas y cómo se interpretan los resultados mostrados.

> [!IMPORTANT]
> A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, la vista de administración dinámica [sys.dm_db_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md) está disponible para recuperar mediante programación la información de encabezado incluida en el objeto de estadísticas para las estadísticas no incrementales.

> [!IMPORTANT]
> A partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] SP2 y [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, la vista de administración dinámica [sys.dm_db_incremental_stats_properties](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md) está disponible para recuperar mediante programación la información de encabezado incluida en el objeto de estadísticas para las estadísticas incrementales.

> [!IMPORTANT]
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU2, la vista de administración dinámica [sys.dm_db_stats_histogram](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md) está disponible para recuperar mediante programación la información del histograma incluida en el objeto de estadísticas.

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)
  
## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server and Azure SQL Database  
  
DBCC SHOW_STATISTICS ( table_or_indexed_view_name , target )   
[ WITH [ NO_INFOMSGS ] < option > [ , n ] ]  
< option > :: =  
    STAT_HEADER | DENSITY_VECTOR | HISTOGRAM | STATS_STREAM  
```  
  
```syntaxsql
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  

DBCC SHOW_STATISTICS ( table_name , target )   
    [ WITH { STAT_HEADER | DENSITY_VECTOR | HISTOGRAM } [ ,...n ] ]  
[;]
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

 *table_or_indexed_view_name*  
 Nombre de la tabla o de la vista indizada cuya información de estadísticas se va a presentar.  
  
 *table_name*  
 Nombre de la tabla que contiene las estadísticas que mostrar. La tabla no puede ser una tabla externa.  
  
 *Destino*  
 Nombre del índice, estadística o columna cuya información de estadísticas se va a presentar. *target* se incluye entre paréntesis, entre comillas simples, entre comillas dobles o sin comillas. Si *target* es el nombre de un índice o estadística existentes en una tabla o vista indizada, se devuelve la información de estadísticas acerca de este destino. Si *target* es el nombre de una columna existente y dicha columna contiene una estadística creada automáticamente, se devuelve información sobre dicha estadística. Si una estadística creada automáticamente no existe para el destino de una columna, se devuelve el mensaje de error 2767.  
 En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)], *target* no puede ser un nombre de columna.  
  
 NO_INFOMSGS  
 Suprime todos los mensajes informativos con niveles de gravedad entre 0 y 10.  
  
 STAT_HEADER \| DENSITY_VECTOR \| HISTOGRAM \| STATS_STREAM [ **,** _n_ ] La especificación de una o varias de estas opciones limita los conjuntos de resultados que devuelve la instrucción a la opción u opciones especificadas. Si no se especifican opciones, se devuelve información de todas las estadísticas.  

 STATS_STREAM es [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>Conjuntos de resultados

En la tabla siguiente se describen las columnas devueltas en el conjunto de resultados si se especifica STAT_HEADER.
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|Nombre|Nombre del objeto de estadísticas.|  
|Actualizado|Fecha y hora de la última actualización de las estadísticas. La función [STATS_DATE](../../t-sql/functions/stats-date-transact-sql.md) constituye otro modo de recuperar esta información. Para más información, vea la sección [Comentarios](#Remarks) en esta página.|  
|Filas|Número total de filas que tenía la tabla o vista indizada la última vez que se actualizaron las estadísticas. Si las estadísticas se filtran o corresponden a un índice filtrado, el número de filas puede ser inferior al número de filas de la tabla. Para más información, vea [Estadísticas](../../relational-databases/statistics/statistics.md).|  
|Rows Sampled|Número total de filas muestreadas para cálculos de estadísticas. Si Rows Sampled < Rows, el histograma y los resultados de la densidad que se muestren serán estimaciones extraídas de las filas muestreadas.|  
|Pasos|Número de pasos del histograma. Cada paso abarca un intervalo de valores de columna seguido de un valor de columna límite superior. Los pasos del histograma se definen en la primera columna de clave de las estadísticas. El número máximo de pasos es 200.|  
|Densidad|Se calcula como 1 / *valores distintos* en todos los valores de la primera columna de clave del objeto de estadísticas, excepto en los valores límite del histograma. El optimizador de consultas no usa este valor de densidad y solo se muestra por motivos de compatibilidad con versiones anteriores a [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)].|  
|Promedio de longitud de clave|Número promedio de bytes por cada uno de los valores de las columnas de clave del objeto de estadísticas.|  
|String Index|Sí indica que el objeto de estadísticas contiene estadísticas de resumen de las cadenas para mejorar los cálculos de cardinalidad de los predicados de consulta que utilizan el operador LIKE; por ejemplo, `WHERE ProductName LIKE '%Bike'`. Las estadísticas de resumen de cadenas se almacenan de forma independiente del histograma y se crean en la primera columna de clave del objeto de estadísticas cuando es de tipo **char**, **varchar**, **nchar**, **nvarchar**, **varchar(max)** , **nvarchar(max)** , **text** o **ntext**.|  
|Expresión de filtro|Predicado del subconjunto de filas de la tabla incluido en el objeto de estadísticas. NULL = Estadísticas no filtradas. Para más información sobre los predicados filtrados, vea [Crear índices filtrados](../../relational-databases/indexes/create-filtered-indexes.md). Para más información sobre las estadísticas filtradas, vea [Estadísticas](../../relational-databases/statistics/statistics.md).|  
|Filas sin filtrar|Número total de filas de la tabla antes de aplicar la expresión de filtro. Si Expresión de filtro es NULL, las Filas sin filtrar son iguales a Filas.|  
|Persisted Sample Percent|Porcentaje de ejemplo persistente empleado en las actualizaciones de estadísticas en las que no se especifica explícitamente un porcentaje de muestreo. Si el valor es cero, significa que no hay establecido ningún porcentaje de ejemplo persistente para esta estadística.<br /><br /> **Se aplica a:** [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1 CU4| 
  
En la tabla siguiente se describen las columnas devueltas en el conjunto de resultados si se especifica DENSITY_VECTOR.
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|Toda la densidad|La densidad es 1 / *valores distintos*. Los resultados muestran la densidad de cada prefijo de columnas del objeto de estadísticas (una fila por cada densidad). Un valor distinto es una lista Distinct de los valores de columna de cada fila y prefijo de columna. Por ejemplo, si el objeto de estadísticas contiene las columnas de clave (A, B, C), los resultados indican la densidad de las listas de valores distintos de cada uno de estos prefijos de columna: (A), (A,B) y (A, B, C). Si se usa el prefijo (A, B, C), cada una de estas listas es una lista de valores distintos: (3, 5, 6), (4, 4, 6), (4, 5, 6), (4, 5, 7). Si se usa el prefijo (A, B) los valores de la misma columna tendrán estas listas de valores distintos: (3, 5), (4, 4) y (4, 5)|  
|Promedio de longitud|Promedio de longitud, en bytes, para almacenar una lista de los valores de columna del prefijo de columna. Por ejemplo, si cada valor de la lista (3, 5, 6) necesita 4 bytes, la longitud es 12 bytes.|  
|Columnas|Nombres de las columnas en el prefijo para las que se muestran Toda la densidad y Promedio de longitud.|  
  
En la tabla siguiente se describen las columnas devueltas en el conjunto de resultados si se especifica la opción HISTOGRAM.
  
|Nombre de la columna|Descripción|  
|---|---|
|RANGE_HI_KEY|Valor de columna límite superior de un paso del histograma. El valor de columna también se denomina valor de clave.|  
|RANGE_ROWS|Número calculado de filas cuyo valor de columna está comprendido en un paso del histograma, sin incluir el límite superior.|  
|EQ_ROWS|Número calculado de filas cuyo valor de columna es igual al límite superior del paso del histograma.|  
|DISTINCT_RANGE_ROWS|Número calculado de filas que tienen un valor de columna distinto en un paso del histograma, sin incluir el límite superior.|  
|AVG_RANGE_ROWS|Promedio de filas con valores de columna duplicados en un paso del histograma, sin incluir el límite superior. Cuando DISTINCT_RANGE_ROWS es mayor que 0, AVG_RANGE_ROWS se calcula dividiendo RANGE_ROWS por DISTINCT_RANGE_ROWS. Cuando DISTINCT_RANGE_ROWS es 0, AVG_RANGE_ROWS devuelve 1 para el paso del histograma.| 
  
## <a name="remarks"></a><a name="Remarks"></a> Comentarios 

La fecha de actualización de estadísticas se almacena en el [objeto BLOB de estadísticas](../../relational-databases/statistics/statistics.md#DefinitionQOStatistics) junto con el [histograma](#histogram) y el [vector de densidad](#density), pero no en los metadatos. Cuando no se lee ningún dato con el que generar datos de estadísticas, el BLOB de estadísticas no se crea, la fecha no está disponible y la columna *updated* es NULL. Esto sucede en las estadísticas filtradas, en las que el predicado no devuelve ninguna fila, o en las tablas nuevas vacías.
  
## <a name="histogram"></a><a name="histogram"></a> Histograma

Un histograma mide la frecuencia de aparición de cada valor distinto en un conjunto de datos. El optimizador de consultas calcula un histograma de los valores de la primera columna de clave del objeto de estadísticas; para ello, selecciona los valores de la columna tomando una muestra estadística de las filas o realizando un análisis completo de todas las filas de la tabla o vista. Si el histograma se crea a partir de muestras de un conjunto de filas, los totales almacenados para el número de filas y el número de valores distintos son las estimaciones y no es necesario que sean números enteros.
  
Para crear el histograma, el optimizador de consultas ordena los valores de columna, calcula el número de valores que coinciden con cada valor de columna distinto y, a continuación, agrupa los valores de columna en un máximo de 200 pasos de histograma contiguos. Cada paso incluye un intervalo de valores de columna seguido de un valor de columna de límite superior. El intervalo incluye todos los valores de columna posibles comprendidos entre los valores límite (sin incluir los propios valores límite). El valor de columna ordenado más pequeño es el valor del límite superior del primer paso del histograma.
  
En el diagrama siguiente se muestra un histograma con seis pasos. El área a la izquierda del primer valor límite superior es el primer paso.
  
![Histograma](../../relational-databases/system-dynamic-management-views/media/a0ce6714-01f4-4943-a083-8cbd2d6f617a.gif "a0ce6714-01f4-4943-a083-8cbd2d6f617a")
  
En cada paso del histograma:
-   La línea gruesa representa el valor de límite superior (RANGE_HI_KEY) y el número de veces que tiene lugar (EQ_ROWS).  
-   El área de color sólido situada a la izquierda RANGE_HI_KEY representa el intervalo de valores de columna y el número medio de veces que tiene lugar cada valor de columna (AVG_RANGE_ROWS). El valor de AVG_RANGE_ROWS en el primer paso del histograma siempre es 0.  
-   Las líneas de puntos representan los valores de las muestras utilizados para estimar el número total de valores distintos que hay en el intervalo (DISTINCT_RANGE_ROWS) y el número total de valores que hay en el intervalo (RANGE_ROWS). El optimizador de consultas utiliza RANGE_ROWS y DISTINCT_RANGE_ROWS para calcular AVG_RANGE_ROWS y no almacena los valores de las muestras.  
  
El optimizador de consultas define los pasos del histograma en función de su importancia estadística. Utiliza un algoritmo de diferencias máximas para minimizar el número de pasos del histograma a la vez que minimiza las diferencias entre los valores límite. El número máximo de pasos es 200. El número de pasos del histograma puede ser menor que el número de valores distintos, incluso para las columnas con menos de 200 puntos de límite. Por ejemplo, una columna con 100 valores distintos puede tener un histograma con menos de 100 puntos de límite.
  
## <a name="density-vector"></a><a name="density"></a> Vector de densidad  
El optimizador de consultas utiliza las densidades para mejorar las estimaciones de cardinalidad de las consultas que devuelven varias columnas de la misma tabla o vista indizada. El vector de densidad contiene una densidad para cada prefijo de columnas del objeto de estadísticas. Por ejemplo, si un objeto de estadísticas tiene las columnas de clave `CustomerId`, `ItemId` y `Price`, la densidad se calcula en cada uno de los siguientes prefijos de columna.
  
|Prefijo de columna|Densidad calculada en|  
|---|---|
|(IdCliente)|Filas con valores que se corresponden con IdCliente|  
|(IdCliente, IdArtículo)|Filas con valores que se corresponden con IdCliente e IdArtículo|  
|(IdCliente, IdArtículo, Precio)|Filas con valores que se corresponden con IdCliente, IdArtículo y Precio|  
  
## <a name="restrictions"></a>Restricciones  
 DBCC SHOW_STATISTICS no proporciona estadísticas de índices espaciales ni índices de almacén de columnas optimizados para memoria.  
  
## <a name="permissions-for-ssnoversion-and-sssds"></a>Permisos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
Para ver el objeto de estadísticas, el usuario debe tener el permiso `SELECT` en la tabla.
Tenga en cuenta los siguientes requisitos para que los permisos SELECT sean suficientes para ejecutar el comando:
-   Los usuarios deben tener permisos en todas las columnas del objeto de estadísticas  
-   Los usuarios deben tener permiso en todas las columnas de una condición de filtro (si existe alguna)  
-   La tabla no puede tener una directiva de seguridad de nivel de fila.
-   Si alguna de las columnas de un objeto de estadísticas se enmascara con reglas de Enmascaramiento dinámico de datos, además del permiso `SELECT`, el usuario debe tener el permiso `UNMASK`.

En versiones anteriores a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, el usuario debe ser propietario de la tabla o miembro del rol fijo de servidor `sysadmin`, o bien de los roles fijos de base de datos `db_owner` o `db_ddladmin`.

 > [!NOTE]
 > Para volver a cambiar el comportamiento al anterior a [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] SP1, use la marca de seguimiento 9485.
  
## <a name="permissions-for-sssdw-and-sspdw"></a>Permisos de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS requiere el permiso `SELECT` en la tabla o la pertenencia al rol fijo de servidor `sysadmin`, el rol fijo de base de datos `db_owner` o el rol fijo de base de datos `db_ddladmin`.  
  
## <a name="limitations-and-restrictions-for-sssdw-and-sspdw"></a>Limitaciones y restricciones de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
DBCC SHOW_STATISTICS muestra las estadísticas almacenadas en la base de datos de shell en el nivel del nodo de control. No muestra las estadísticas que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea automáticamente en los nodos de ejecución.
  
DBCC SHOW_STATISTICS no se admite en tablas externas.
  
## <a name="examples-ssnoversion-and-sssds"></a>Ejemplos: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]  
### <a name="a-returning-all-statistics-information"></a>A. Devolver información de todas las estadísticas  
En el siguiente ejemplo se muestra toda la información de estadísticas del índice `AK_Address_rowguid` de la tabla `Person.Address` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].
  
```sql
DBCC SHOW_STATISTICS ("Person.Address", AK_Address_rowguid);  
GO  
```  
  
### <a name="b-specifying-the-histogram-option"></a>B. Especificar la opción HISTOGRAM  
Esto limita la información de estadísticas mostrada en relación con el índice Customer_LastName a únicamente los datos de HISTOGRAM.
  
```sql
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName) WITH HISTOGRAM;  
GO  
```  
  
## <a name="examples-sssdw-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
### <a name="c-display-the-contents-of-one-statistics-object"></a>C. Mostrar el contenido de un objeto de estadísticas  
 En el siguiente ejemplo se muestra el contenido de las estadísticas de Customer_LastName en la tabla DimCustomer.  
  
```sql
-- Uses AdventureWorks  
--First, create a statistics object  
CREATE STATISTICS Customer_LastName   
ON AdventureWorksPDW2012.dbo.DimCustomer (LastName);  
GO  
DBCC SHOW_STATISTICS ("dbo.DimCustomer",Customer_LastName);  
GO  
```  
  
Los resultados muestran el encabezado, el vector de densidad y parte del histograma.
  
![Resultados de DBCC SHOW_STATISTICS](../../t-sql/database-console-commands/media/aps-sql-dbccshow-statistics.JPG "Resultados de DBCC SHOW_STATISTICS")
  
## <a name="see-also"></a>Consulte también  
[estadísticas](../../relational-databases/statistics/statistics.md)  
[CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)  
[CREATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/create-statistics-transact-sql.md)  
[DROP STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/drop-statistics-transact-sql.md)  
[sp_autostats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-autostats-transact-sql.md)  
[sp_createstats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-createstats-transact-sql.md)  
[STATS_DATE &#40;Transact-SQL&#41;](../../t-sql/functions/stats-date-transact-sql.md)  
[UPDATE STATISTICS &#40;Transact-SQL&#41;](../../t-sql/statements/update-statistics-transact-sql.md)  
[sys.dm_db_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-properties-transact-sql.md)  
[sys.dm_db_stats_histogram (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-stats-histogram-transact-sql.md)   
[sys.dm_db_incremental_stats_properties (Transact-SQL)](../../relational-databases/system-dynamic-management-views/sys-dm-db-incremental-stats-properties-transact-sql.md)   
