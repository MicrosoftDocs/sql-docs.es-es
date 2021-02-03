---
description: 'Índices de almacén de columnas: Guía de carga de datos'
title: 'Índices de almacén de columnas: Guía de carga de datos | Microsoft Docs'
ms.custom: ''
ms.date: 12/03/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
ms.assetid: b29850b5-5530-498d-8298-c4d4a741cdaf
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 37827d1de9b99acb585762da094fd6cdaf905298
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235389"
---
# <a name="columnstore-indexes---data-loading-guidance"></a>Índices de almacén de columnas: Guía de carga de datos

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

En este artículo se describen las opciones y las recomendaciones para cargar datos en un índice de almacén de columnas mediante los métodos de inserción gradual y carga masiva SQL estándar. La carga de datos en un índice de almacén de columnas es una parte esencial de cualquier proceso de almacenamiento de datos, ya que mueve la información al índice que se está preparando para el análisis.
  
 ¿Es la primera vez que utiliza índices de almacén de columnas? Vea [Introducción a los índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md) y [Arquitectura de los índices de almacén de columnas](../../relational-databases/sql-server-index-design-guide.md#columnstore_index).
  
## <a name="what-is-bulk-loading"></a>¿Qué es la carga masiva?
Con "*carga masiva*" nos referimos a la forma en que se agregan grandes cantidades de filas a un almacén de datos. Se trata de la forma más eficaz para mover datos a un índice de almacén de columnas, ya que funciona en lotes de filas. La carga masiva llena los grupos de filas hasta su capacidad máxima y los comprime directamente en el almacén de columnas. Solo las filas al final de una carga que no cumplan los requisitos mínimos de 102 400 filas por grupo de filas pasan al almacén delta.  

Para realizar una carga masiva, puede usar la [utilidad bcp](../../tools/bcp-utility.md), [Integration Services](../../integration-services/sql-server-integration-services.md) o seleccionar las filas de una tabla de almacenamiento provisional.

![Carga en un índice de almacén de columnas agrupado](../../relational-databases/indexes/media/sql-server-pdw-columnstore-loadprocess.gif "Carga en un índice de almacén de columnas agrupado")  
  
Tal y como se recomienda en el diagrama, una carga masiva:
  
- No ordena los datos previamente. Los datos se insertan en grupos de filas en el orden en que se reciben.
- Si el tamaño de lote es mayor o igual que 102400, las filas estarán directamente en el grupo de filas comprimido. Se recomienda elegir un tamaño de lote mayor o igual que 102400 para que la importación en bloque se realice de forma eficaz, ya que puede evitar que se muevan las filas de datos a un grupo de filas delta antes de que las filas se terminen moviendo a grupos de filas comprimidos mediante el motor de tupla (TM), un subproceso en segundo plano.
- Si el tamaño del lote o el de las filas restantes es menor que 102 400, las filas se cargarán en grupos de filas delta.

> [!NOTE]
> En una tabla de almacén de filas con datos de índices de almacén de columnas no agrupados, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siempre inserta los datos en la tabla base. Nunca se insertan directamente en el índice de almacén de columnas.  

La carga masiva tiene estas optimizaciones de rendimiento integradas:
-   **Cargas en paralelo:** Puede realizar varias importaciones simultáneas masivas (bcp o inserción masiva) cargando un archivo de datos independiente. A diferencia de las cargas masivas del almacén de filas en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], no hay que especificar `TABLOCK`, ya que cada subproceso de importación en bloque cargará los datos solamente en grupos de filas independientes (grupos de filas delta o comprimidos) aplicando un bloqueo exclusivo. 

-   **Registro reducido:** Los datos que se cargan directamente en grupos de filas comprimidos conducen a una reducción significativa del tamaño del registro. Por ejemplo, si los datos se han comprimido 10 veces, el registro de transacciones correspondiente será aproximadamente 10 veces más pequeño sin necesidad de TABLOCK o de un modelo de recuperación simple u optimizado para cargas masivas de registros. Los datos que van a un grupo de filas delta se registran por completo, incluidos los tamaños de lote con menos de 102 400 filas.  El procedimiento recomendado es usar batchsize >= 102400. Dado que no se requiere TABLOCK, puede cargar los datos en paralelo. 

-   **Registro mínimo:** Puede obtener una mayor reducción del registro si sigue los requisitos previos para [el registro mínimo](../import-export/prerequisites-for-minimal-logging-in-bulk-import.md). Sin embargo, a diferencia de la carga de datos en un almacén de filas, TABLOCK conduce a un bloqueo X en la tabla en lugar de un bloqueo BU (actualización masiva) y, por lo tanto, no se puede realizar la carga de datos paralela. Para obtener más información sobre el bloqueo, consulte [Versiones de fila y bloqueo](../sql-server-transaction-locking-and-row-versioning-guide.md).

-   **Optimization de bloqueo:** El bloqueo X de un grupo de filas se adquiere automáticamente al cargar los datos en un grupo de filas comprimido. Sin embargo, cuando la carga masiva se realiza en un grupo de filas delta, se obtiene un bloqueo X en el grupo de filas, pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] continúa bloqueando los bloqueos de página y extensión, ya que el bloqueo de grupos de filas X no forma parte de la jerarquía de bloqueo.  
  
Si tiene un índice no agrupado de árbol B en un índice de almacén de columnas, no habrá ninguna optimización de registro ni de bloqueo para el propio índice. Sin embargo, las optimizaciones del índice agrupado de almacén de columnas se seguirán aplicando, tal como se ha descrito anteriormente.  

Tenga en cuenta que DML (insert, delete, update) no es una operación de modo por lotes porque no es paralela.
  
## <a name="plan-bulk-load-sizes-to-minimize-delta-rowgroups"></a>Planeación de los tamaños de carga masiva para minimizar los grupos de filas delta
Los índices de almacén de columnas funcionan mejor cuando la mayoría de las filas se comprimen en el almacén de columnas y no se ubican en grupos de filas delta. Se recomienda cambiar el tamaño de las cargas para que las filas pasen directamente al almacén de columnas y omitan lo máximo posible el almacén delta.

En estos escenarios siguientes se describe cuándo las filas cargadas pasan directamente al almacén de columnas o cuándo van al almacén delta. En este ejemplo, cada grupo de filas puede tener de 102.400 a 1.048.576 filas. En la práctica, el tamaño máximo de un grupo de filas puede ser inferior a 1 048 576 filas cuando hay presión de memoria.  
  
|Filas que se cargarán de forma masiva|Filas agregadas al grupo de filas comprimido|Filas agregadas al grupo de filas delta|  
|-----------------------|-------------------------------------------|--------------------------------------|  
|102 000|0|102 000|  
|145,000|145,000<br /><br /> Tamaño del grupo de filas: 145,000|0|  
|1,048,577|1,048,576<br /><br /> Tamaño del grupo de filas: 1 048 576.|1|  
|2,252,152|2,252,152<br /><br /> Tamaños de los grupos de filas: 1 048 576, 1 048 576, 155 000.|0|  
| &nbsp; | &nbsp; | &nbsp; |
  
 En el ejemplo siguiente se muestran los resultados de cargar 1 048 577 filas en una tabla. Los resultados muestran un grupo de filas COMPRESSED en el almacén de columnas (como segmentos de columna comprimidos) y una fila en el almacén delta.  
  
```sql  
SELECT object_id, index_id, partition_number, row_group_id, delta_store_hobt_id, 
  state, state_desc, total_rows, deleted_rows, size_in_bytes   
FROM sys.dm_db_column_store_row_group_physical_stats  
```  
  
 ![Grupo de filas y almacenamiento delta para una carga por lotes](../../relational-databases/indexes/media/sql-server-pdw-columnstore-batchload.gif "Grupo de filas y almacenamiento delta para una carga por lotes")  
  
## <a name="use-a-staging-table-to-improve-performance"></a>Uso de una tabla de almacenamiento provisional para mejorar el rendimiento
Si está cargando datos solo para colocarlos en etapa antes de ejecutar más transformaciones, será mucho más rápido cargar la tabla en una tabla del montón que cargar los datos en una tabla agrupada de almacén de columnas. Además, si carga los datos en una [tabla temporal][Temporary], el proceso también será mucho más rápido que cargar una tabla en un almacenamiento permanente.  

 Un patrón común de carga de datos consiste en cargar los datos en una tabla de almacenamiento provisional, realizar alguna transformación y, después, cargarlos en la tabla de destino con el siguiente comando:  
  
```sql  
INSERT INTO <columnstore index>  
SELECT <list of columns> FROM <Staging Table>  
```  
  
 Este comando carga los datos en el índice de almacén de columnas de una forma similar a bcp o la tarea de inserción masiva, solo que en un único lote. Si el número de filas de la tabla de almacenamiento provisional es menor que 102400, las filas se cargarán en un grupo de filas delta; en caso contrario, se cargarán directamente en un grupo de filas comprimido. Antes existía una limitación importante: esta operación `INSERT` era de un solo subproceso. Para cargar los datos en paralelo, podía crear varias tablas de almacenamiento provisional o ejecutar `INSERT`/`SELECT` con intervalos no superpuestos de filas desde la tabla de almacenamiento provisional. Esta limitación desaparece en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. El siguiente comando carga los datos en paralelo de la tabla de almacenamiento provisional, pero tendrá que especificar `TABLOCK`. Puede que le resulte contradictorio con respecto a lo que se dijo anteriormente de la carga masiva, pero la principal diferencia es que la carga de datos en paralelo desde la tabla de almacenamiento provisional se ejecuta en la misma transacción.
  
```sql  
INSERT INTO <columnstore index> WITH (TABLOCK) 
SELECT <list of columns> FROM <Staging Table>  
```  
  
 Las siguientes optimizaciones están disponibles cuando se cargan datos en índices de almacén de columnas agrupados desde tablas de almacenamiento provisional:
-   **Optimización del registro:** Registro reducido cuando los datos se cargan en el grupo de filas comprimido.   
-   **Optimization de bloqueo:** Cuando se cargan datos en un grupo de filas comprimido, se obtiene el bloqueo X en el grupo de filas. Sin embargo, con el grupo de filas delta, se obtiene un bloqueo X en el grupo de filas, pero [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] continúa bloqueando los bloqueos de página y extensión, ya que el bloqueo de grupos de filas X no forma parte de la jerarquía de bloqueo.  
  
 Si tiene uno o varios índices no agrupados, no habrá ninguna optimización de registro o bloqueo para el propio índice, pero las optimizaciones en el índice de almacén de columnas agrupado siguen estando disponibles, tal como se describió anteriormente.  
  
## <a name="what-is-trickle-insert"></a>¿Qué es la inserción gradual?

Con "*inserción gradual*" nos referimos a la forma de mover filas individuales al índice de almacén de columnas. Esta característica usa la instrucción [INSERT INTO](../../t-sql/statements/insert-transact-sql.md). Gracias a la inserción gradual, todas las filas pasan al almacén delta. Es útil para un número reducido de filas, pero no es práctico para cargas de gran tamaño.
  
```sql  
INSERT INTO <table-name> VALUES (<set of values>)  
```  
  
 > [!NOTE]
 > Los subprocesos simultáneos que usan INSERT INTO para insertar valores en un índice de almacén de columnas agrupado pueden insertar filas en el mismo grupo de filas de almacén delta.  
  
 Una vez que el grupo de filas contiene 1 048 576 filas, el grupo de filas delta se marca como cerrado, pero sigue estando disponible para las consultas y las operaciones de actualización o eliminación. No obstante, las filas recién insertadas se pasan a un grupo de filas de almacén delta existente o recién creado. Hay un subproceso en segundo plano, el *motor de tupla (TM)* , que comprime los grupos de filas delta cerrados cada 5 minutos aproximadamente. Puede invocar expresamente el siguiente comando para comprimir el grupo de filas delta cerrado.  
  
```sql  
ALTER INDEX <index-name> on <table-name> REORGANIZE  
```  
  
 Si desea forzar el cierre o la compresión de un grupo de filas delta, ejecute el siguiente comando. Se recomienda ejecutar este comando si ha terminado de cargar las filas y no espera insertar nuevas filas. Al cerrar y comprimir expresamente el grupo de filas delta, puede ahorrar más espacio de almacenamiento y mejorar el rendimiento de las consultas de análisis. Sugerimos invocar este comando si no espera insertar nuevas filas.  
  
```sql  
ALTER INDEX <index-name> on <table-name> REORGANIZE with (COMPRESS_ALL_ROW_GROUPS = ON)  
```  
  
## <a name="how-loading-into-a-partitioned-table-works"></a>Funcionamiento de la carga en una tabla con particiones  
 Para los datos con particiones, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] primero asigna cada fila a una partición y después realiza operaciones del almacén de columnas con los datos de la partición. Cada partición tiene sus propios grupos de filas y, al menos, un grupo de filas delta.  
  
## <a name="next-steps"></a>Pasos siguientes

Entrada de blog hospedada ahora en _techcommunity_, escrita el 11 de marzo de 2015: [Data Loading performance considerations with Clustered Columnstore indexes](https://techcommunity.microsoft.com/t5/DataCAT/Data-Loading-performance-considerations-with-Clustered/ba-p/305223) (Consideraciones de rendimiento con los índices de almacén de columnas en clúster de carga de datos).
