---
description: Introducción al almacén de columnas para análisis operativos en tiempo real
title: Introducción al almacén de columnas para análisis operativos en tiempo real | Microsoft Docs
ms.custom: ''
ms.date: 03/08/2016
ms.prod: sql
ms.prod_service: table-view-index, sql-database
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: quickstart
ms.assetid: e1328615-6b59-4473-8a8d-4f360f73187d
author: MikeRayMSFT
ms.author: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: dbc0867b71cadb33b1eb9174132b2031134a72f0
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236941"
---
# <a name="get-started-with-columnstore-for-real-time-operational-analytics"></a>Introducción al almacén de columnas para análisis operativos en tiempo real
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] incorpora análisis operativo en tiempo real, esto es, la capacidad de ejecutar simultáneamente análisis y cargas de trabajo OLTP en las mismas tablas de base de datos. Aparte de poder ejecutar análisis en tiempo real, también puede prescindir del uso de ETL y de un almacén de datos.  
  
## <a name="real-time-operational-analytics-explained"></a>Explicación de los análisis operativos en tiempo real  
 Tradicionalmente, las empresas siempre han tenido sistemas independientes para las cargas de trabajo operativas (es decir, OLTP) y de análisis. En estos sistemas, los trabajos de extracción, transformación y carga de datos (ETL) mueven periódicamente los datos desde el almacén operativo a un almacenamiento de análisis. Los datos de análisis suelen residir en un almacén de datos o data mart dedicado a ejecutar consultas de análisis. Si bien esto ha venido siendo lo habitual, plantea tres retos importantes:  
  
-   **Complejidad.** La implementación de ETL puede conllevar una tarea de codificación considerable, especialmente para cargar solamente las filas modificadas. Saber qué filas se han modificado puede ser bastante complicado.  
  
-   **Costo.** La implementación de ETL requiere invertir en más licencias de software y hardware.  
  
-   **Latencia de datos.** La implementación de ETL conlleva un retraso de tiempo a la hora de ejecutar los análisis. Por ejemplo, si el trabajo de ETL tiene lugar al final de cada día laborable, las consultas de análisis se ejecutarán en datos que llevan como mínimo un día de desfase. Para muchas empresas, este retraso es inaceptable porque el negocio depende de poder analizar los datos en tiempo real. Por ejemplo, para poder detectar fraudes, es preciso analizar los datos operativos en tiempo real.  
  
 ![información general de análisis operativos en tiempo real](../../relational-databases/indexes/media/real-time-operational-analytics-overview.png "información general de análisis operativos en tiempo real")  
  
Los análisis operativos en tiempo real ofrecen una solución a estos retos.   
No comportan ningún retraso cuando las cargas de trabajo OLTP y de análisis se ejecutan en la misma tabla subyacente.   En las situaciones en las que se pueden usar análisis en tiempo real, los costos y la complejidad se reducen enormemente, ya que se pone fin a la necesidad de realizar trabajos ETL o de adquirir y mantener un almacén de datos independiente.  
  
> [!NOTE]  
>  Los análisis operativos en tiempo real abordan un escenario con un único origen de datos, como una aplicación de planificación de recursos empresariales (ERP) en la que se pueden ejecutar las cargas de trabajo tanto operativas como de análisis. Esto no significa que no pueda necesitarse un almacén de datos independiente cuando haya que integrar datos procedentes de varios orígenes antes de ejecutar la carga de trabajo de análisis, o cuando necesite disponer de un rendimiento de análisis extremo con datos previamente agregados, como los cubos.  
  
 Los análisis en tiempo real usan un índice de almacén de columnas actualizable en una tabla de almacén de filas. El índice de almacén de columnas mantiene una copia de los datos, por lo que las cargas de trabajo OLTP y de análisis ejecutarán copias de los datos distintas. Esto reduce el impacto en el rendimiento que supone ejecutar ambas cargas de trabajo al mismo tiempo. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene automáticamente los cambios en el índice a fin de que los cambios de OLTP siempre estén actualizados para los análisis. Con este diseño, es posible (y útil) ejecutar análisis en tiempo real en los datos actualizados. Esto es válido tanto para las tablas basadas en disco como para las tablas optimizadas para memoria.  
  
## <a name="get-started-example"></a>Ejemplo introductorio  
 Para empezar a usar análisis en tiempo real:  
  
1.  Identifique las tablas del esquema operativo que contienen los datos necesarios para el análisis.  
  
2.  En cada tabla, quite todos los índices de árbol b que están diseñados principalmente para acelerar el análisis existente en la carga de trabajo OLTP. Sustitúyalos por un índice de almacén de columnas único.  Esto mejorará el rendimiento general de la carga de trabajo OLTP, ya que habrá menos índices que mantener.  
  
    ```sql  
    --This example creates a nonclustered columnstore index on an existing OLTP table.  
    --Create the table  
    CREATE TABLE t_account (  
        accountkey int PRIMARY KEY,  
        accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int  
    );  
  
    --Create the columnstore index with a filtered condition  
    CREATE NONCLUSTERED COLUMNSTORE INDEX account_NCCI   
    ON t_account (accountkey, accountdescription, unitsold)   
    ;  
    ```  
  
     El índice de almacén de columnas de una tabla en memoria permite los análisis operativos al integrar las tecnologías de OLTP en memoria y almacén de columnas en memoria, con lo que se logra un alto rendimiento en las cargas de trabajo OLTP y de análisis. El índice de almacén de columnas de una tabla en memoria debe incluir todas las columnas.  
  
    ```sql  
    -- This example creates a memory-optimized table with a columnstore index.  
    CREATE TABLE t_account (  
        accountkey int NOT NULL PRIMARY KEY NONCLUSTERED,  
        Accountdescription nvarchar (50),  
        accounttype nvarchar(50),  
        unitsold int,  
        INDEX t_account_cci CLUSTERED COLUMNSTORE  
        )  
        WITH (MEMORY_OPTIMIZED = ON );  
    GO  
  
    ```  
  
3.  Esto es todo lo que hay que hacer.  

 Ya está listo para ejecutar análisis operativos en tiempo real, sin haber realizado ningún cambio en la aplicación.  Las consultas de análisis se ejecutarán en el índice de almacén de columnas y las operaciones OLTP seguirán ejecutándose en los índices de árbol b de OLTP. Las cargas de trabajo OLTP seguirán produciéndose, si bien con una ligera sobrecarga adicional para mantener el índice de almacén de columnas. Vea las optimizaciones de rendimiento en la siguiente sección.  
  
## <a name="blog-posts"></a>Entradas de blog  
 Lea las entradas de blog siguientes para obtener más información sobre el análisis operativo en tiempo real. Si lee estas entradas de blog primero, probablemente le será más fácil entender las secciones de consejos de rendimiento.  
  
-   [Business case for real-time operational analytics (Caso empresarial de análisis operativo en tiempo real)](https://blogs.technet.microsoft.com/dataplatforminsider/2015/12/09/real-time-operational-analytics-using-in-memory-technology/)  
  
-   [Using a nonclustered columnstore index for real-time operational analytics (Uso de un índice de almacén de columnas no agrupado para análisis operativos en tiempo real)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-using-nonclustered-columnstore-index)  
  
-   [A simple example using a nonclustered columnstore index (Un sencillo ejemplo de uso de un índice de almacén de columnas no agrupado)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-simple-example-using-nonclustered-clustered-columnstore-index-ncci)  
  
-   [How SQL Server maintains a nonclustered columnstore index on a transactional workload (Cómo SQL Server mantiene un índice de almacén de columnas no agrupado en una carga de trabajo transaccional)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-dml-operations-and-nonclustered-columnstore-index-ncci-in-sql-server-2016)  
  
-   [Minimizing the impact of nonclustered columnstore index maintenance by using a filtered index (Minimizar el impacto del mantenimiento de índices de almacén de columnas no agrupados mediante un índice filtrado)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci)  
  
-   [Minimizing the impact of nonclustered columnstore index maintenance by using a compression delay (Minimizar el impacto del mantenimiento de índices de almacén de columnas no agrupados mediante un retraso de compresión)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci)  
  
-   [Minimizing the impact of nonclustered columnstore index maintenance by using a compression delay - performance numbers (Minimizar el impacto del mantenimiento de índices de almacén de columnas no agrupados mediante un retraso de compresión: cifras de rendimiento)](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-with-ncci-and-the-performance)  
  
-   [Análisis operativos en tiempo real con tablas optimizadas para memoria](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-memory-optimized-table-and-columnstore-index)  
  
-   [Columnstore index and the merge policy for rowgroups (Índice de almacén de columnas y directiva de combinación de grupos de filas)](/archive/blogs/sqlserverstorageengine/columnstore-index-merge-policy-for-reorganize)  
  
## <a name="performance-tip-1-use-filtered-indexes-to-improve-query-performance"></a>Sugerencia de rendimiento 1: usar índices filtrados para mejorar el rendimiento de las consultas  
 Los análisis operativos en tiempo real pueden tener un impacto negativo en el rendimiento de la carga de trabajo OLTP. Este impacto debería ser mínimo. En el siguiente ejemplo se muestra cómo usar índices filtrados para minimizar el impacto del índice de almacén de columnas no agrupado en la carga de trabajo transaccional, mientras se siguen realizando análisis en tiempo real.  
  
 Para reducir la sobrecarga derivada de mantener un índice de almacén de columnas no agrupado en una carga de trabajo operativa, puede usar una condición de filtrado para crear un índice de almacén de columnas no agrupado únicamente de los datos *semiactivos* o de variación lenta. Por ejemplo, en una aplicación de administración de pedidos, puede crear un índice de almacén de columnas no agrupado de los pedidos que ya se hayan enviado. Una vez que un pedido se envía, este apenas si cambia y, por tanto, se puede considerar como un dato semiactivo. Con el índice filtrado, los datos del índice de almacén de columnas no agrupado requieren menos actualizaciones, lo que reduce el impacto en la carga de trabajo transaccional.  
  
 Las consultas de análisis tienen un acceso transparente a los datos tanto activos como semiactivos, según sea necesario para proporcionar análisis en tiempo real. Si una parte considerable de la carga de trabajo operativa usa los datos "de acceso frecuente", esas operaciones no requerirán un mantenimiento adicional del índice de almacén de columnas. Un procedimiento recomendado es tener un índice agrupado de almacén de filas en las columnas usadas en la definición del índice filtrado. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa el índice agrupado para examinar rápidamente las filas que no cumplen la condición de filtrado. Sin este índice agrupado, sería necesario realizar un recorrido de tabla completo de la tabla de almacén de filas para encontrar dichas filas, lo que puede repercutir negativamente y en gran medida en el rendimiento de las consultas de análisis. Si no hay un índice agrupado, podría crear un índice complementario de árbol b no agrupado filtrado para identificar esas filas, pero esto no es recomendable porque el acceso a un amplio rango de filas a través de índices de árbol b no agrupados es muy costoso.  
  
> [!NOTE]  
>  Un índice de almacén de columnas no agrupado filtrado solo se puede usar en tablas basadas en disco. No se admite en tablas optimizadas para memoria.  
  
### <a name="example-a-access-hot-data-from-btree-index-warm-data-from-columnstore-index"></a>Ejemplo A: acceso a datos activos del índice de árbol b y a datos semiactivos del índice de almacén de columnas  
 En este ejemplo se usa una condición filtrada (accountkey > 0) para establecer qué filas se van a incluir en el índice de almacén de columnas. El objetivo es diseñar la condición de filtrado y las consultas posteriores para acceder a los datos "activos" del índice de árbol b que cambian con frecuencia, así como para acceder a los datos "semiactivos" del índice de almacén de columnas, que son más estables.  
  
 ![Índices combinados para datos activos y semiactivos](../../relational-databases/indexes/media/de-columnstore-warmhotdata.png "Índices combinados para datos activos y de acceso frecuente")  
  
> [!NOTE]  
>  El Optimizador de consultas tendrá en cuenta el índice de almacén de columnas para el plan de consulta, pero no siempre lo elegirá. Cuando el optimizador de consultas elige el índice de almacén de columnas filtrado, combina de forma transparente tanto las filas del índice de almacén de columnas como las filas que no cumplen con la condición de filtrado para permitir los análisis en tiempo real. Esto difiere de un índice filtrado no agrupado regular, que solo se puede usar en las consultas limitadas a las filas existentes en el índice.  
  
```sql  
--Use a filtered condition to separate hot data in a rowstore table  
-- from "warm" data in a columnstore index.  
  
-- create the table  
CREATE TABLE  orders (  
         AccountKey         int not null,  
         Customername       nvarchar (50),  
        OrderNumber         bigint,  
        PurchasePrice       decimal (9,2),  
        OrderStatus         smallint not null,  
        OrderStatusDesc     nvarchar (50))  
  
-- OrderStatusDesc  
-- 0 => 'Order Started'  
-- 1 => 'Order Closed'  
-- 2 => 'Order Paid'  
-- 3 => 'Order Fullfillment Wait'  
-- 4 => 'Order Shipped'  
-- 5 => 'Order Received'  
  
CREATE CLUSTERED INDEX  orders_ci ON orders(OrderStatus)  
  
--Create the columnstore index with a filtered condition  
CREATE NONCLUSTERED COLUMNSTORE INDEX orders_ncci ON orders  (accountkey, customername, purchaseprice, orderstatus)  
where orderstatus = 5  
;  
  
-- The following query returns the total purchase done by customers for items > $100 .00  
-- This query will pick  rows both from NCCI and from 'hot' rows that are not part of NCCI  
SELECT top 5 customername, sum (PurchasePrice)  
FROM orders  
WHERE purchaseprice > 100.0   
Group By customername  
```  
  
 La consulta de análisis se ejecutará con el siguiente plan de consulta. Aquí se aprecia que el acceso a las filas que no cumplen con la condición de filtro se efectúa a través del índice de árbol b agrupado.  
  
 ![Plan de consulta](../../relational-databases/indexes/media/query-plan-columnstore.png "Plan de consulta")  
  
 Consulte el blog para obtener información detallada sobre los [índices de almacén de columnas no agrupado filtrado](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-filtered-nonclustered-columnstore-index-ncci).  
  
## <a name="performance-tip-2-offload-analytics-to-always-on-readable-secondary"></a>Sugerencia de rendimiento 2: descargar el análisis en una secundaria legible de AlwaysOn  
 Aunque puede usar un índice de almacén de columnas filtrado para minimizar el mantenimiento de los índices de almacén de columnas, las consultas de análisis seguirán necesitando importantes cantidades de recursos informáticos (CPU, E/S, memoria) que afectan al rendimiento de las cargas de trabajo operativas. Nuestra recomendación para la mayor parte de las cargas de trabajo críticas es usar la configuración de AlwaysOn. En esta configuración, puede eliminar el impacto de los análisis descargándolos en una secundaria legible.  
  
## <a name="performance-tip-3-reducing-index-fragmentation-by-keeping-hot-data-in-delta-rowgroups"></a>Sugerencia de rendimiento 3: reducir la fragmentación de índice conservando los datos activos en grupos de filas delta  
 Las tablas con índices de almacén de columnas pueden llegar a fragmentarse (es decir, se eliminan filas) de forma muy acusada si la carga de trabajo actualiza o elimina filas que se han comprimido. Un índice de almacén de columnas fragmentado conduce a un uso ineficaz de la memoria y el almacenamiento. Pero, aparte del uso ineficiente de los recursos, también repercute negativamente en el rendimiento de las consultas de análisis, dada la E/S adicional y la necesidad de filtrar las filas eliminadas del conjunto de resultados.  
  
 Las filas eliminadas no se quitarán físicamente hasta que se ejecute una desfragmentación del índice con el comando `REORGANIZE` o hasta que se recompile el índice de almacén de columnas en toda la tabla o en las particiones afectadas. Ambos comandos, `REORGANIZE` y `REBUILD`, son operaciones costosas que consumen recursos que, de otro modo, se podrían usar para la carga de trabajo. Además, si las filas se comprimen demasiado pronto, puede que haya que volver a comprimirlas varias veces debido a las actualizaciones, lo que daría lugar a una sobrecarga de compresión innecesaria.  
La fragmentación del índice se puede minimizar con la opción `COMPRESSION_DELAY`.  
  
```sql  
-- Create a sample table  
CREATE TABLE t_colstor (  
               accountkey                      int not null,  
               accountdescription              nvarchar (50) not null,  
               accounttype                     nvarchar(50),  
               accountCodeAlternatekey         int)  
  
-- Creating nonclustered columnstore index with COMPRESSION_DELAY. The columnstore index will keep the rows in closed delta rowgroup for 100 minutes   
-- after it has been marked closed  
CREATE NONCLUSTERED COLUMNSTORE index t_colstor_cci on t_colstor (accountkey, accountdescription, accounttype)   
                       WITH (DATA_COMPRESSION= COLUMNSTORE, COMPRESSION_DELAY = 100);  
  
;  
```  
  
 Consulte el blog para obtener información detallada sobre el [retraso de compresión](/archive/blogs/sqlserverstorageengine/real-time-operational-analytics-compression-delay-option-for-nonclustered-columnstore-index-ncci).  
  
 A continuación encontrará los procedimientos recomendados:  
  
-   **Carga de trabajo de inserción o de consulta**: si la carga de trabajo consiste principalmente en insertar datos y realizar consultas sobre estos, la opción recomendada predeterminada para COMPRESSION_DELAY es 0. Las filas recién insertadas se comprimirán cuando se haya insertado 1 millón de filas en un solo grupo de filas delta.  
    Algunos ejemplos de este tipo de carga de trabajo son: (a) carga de trabajo de DW tradicional o (b) análisis de secuencia de clics cuando hay que analizar el patrón de clics en una aplicación web.  
  
-   **Carga de trabajo OLTP:** si la carga de trabajo hace un uso profuso de DML (es decir, un uso combinado intensivo de actualizaciones, eliminaciones e inserciones), puede ver la fragmentación de índices de almacén de columnas examinando el sys de DMV. dm_db_column_store_row_group_physical_stats. Si ve que más de un 10 % de las filas se marcan como eliminadas en los grupos de filas comprimidos recientemente, puede usar la opción COMPRESSION_DELAY para agregar un retraso cuando las filas sean aptas para la compresión. Por ejemplo, si en la carga de trabajo las filas recién insertadas se mantienen como "activas" (es decir, se actualizan varias veces) durante, digamos, 60 minutos, conviene establecer la opción COMPRESSION_DELAY en 60.  
  
 Es de esperar que la mayoría de los clientes no tenga que hacer nada. El valor predeterminado de la opción COMPRESSION_DELAY debería valerles.  
En el caso de los usuarios avanzados, se recomienda ejecutar la siguiente consulta y recopilar un porcentaje de las filas eliminadas en los últimos 7 días.  
  
```sql  
SELECT row_group_id,cast(deleted_rows as float)/cast(total_rows as float)*100 as [% fragmented], created_time  
FROM sys. dm_db_column_store_row_group_physical_stats  
WHERE object_id = object_id('FactOnlineSales2')   
             AND  state_desc='COMPRESSED'   
             AND deleted_rows>0   
             AND created_time > GETDATE() - 7  
ORDER BY created_time DESC;  
```  
  
 Si el número de filas eliminadas en los grupos de filas comprimidas es mayor del 20 %, teniendo un nivel predefinido de los grupos de filas más antiguos con una variación inferior a 5 % (denominados grupos de filas inactivos), establezca COMPRESSION_DELAY = (youngest_rowgroup_created_time -  current_time). Tenga en cuenta que este método funciona mejor en cargas de trabajo estables y relativamente homogéneas.  
  
## <a name="see-also"></a>Consulte también  
 [Guía de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-overview.md)   
 [Carga de datos de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-data-loading-guidance.md)   
 [Rendimiento de las consultas de índices de almacén de columnas](../../relational-databases/indexes/columnstore-indexes-query-performance.md)   
 [Índices de almacén de columnas para el almacenamiento de datos](../../relational-databases/indexes/columnstore-indexes-data-warehouse.md)   
 [Reorganizar y volver a generar índices](../../relational-databases/indexes/reorganize-and-rebuild-indexes.md)
