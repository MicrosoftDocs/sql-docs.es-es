---
title: Estimación de cardinalidad (SQL Server) | Microsoft Docs
description: El optimizador de consultas de SQL Server selecciona los planes de consulta que tienen el costo de procesamiento estimado más bajo, determinado en función de las filas procesadas y de un modelo de costos.
ms.custom: ''
ms.date: 02/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- cardinality estimator
- CE (cardinality estimator)
- estimating cardinality
ms.assetid: baa8a304-5713-4cfe-a699-345e819ce6df
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 4d1af8a21bd8dc4ac0df08c45665ed37e74f7ef5
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237688"
---
# <a name="cardinality-estimation-sql-server"></a>Estimación de cardinalidad (SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

El Optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un optimizador basado en el costo. Esto significa que selecciona los planes de consulta cuya ejecución tiene el menor costo de procesamiento estimado. El optimizador de consultas determina el costo de ejecución de un plan de consulta en función de dos factores principales:

- El número total de filas procesadas en cada nivel de un plan de consulta, denominado cardinalidad del plan.
- El modelo de costos del algoritmo determinado por los operadores que se utiliza en la consulta.

El primer factor, la cardinalidad, se utiliza como parámetro de entrada del segundo factor, el modelo de costos. Por lo tanto, una cardinalidad mejorada se traduce en menores costos y, a su vez, en planes de ejecución más rápidos.

La estimación de cardinalidad en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] calcula la cardinalidad principalmente a partir de histogramas que se crean al crear, manual o automáticamente, índices o estadísticas. En ocasiones, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también utiliza información de restricciones y nuevas versiones lógicas de consultas para determinar la cardinalidad.

En los casos siguientes, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede calcular con precisión las cardinalidades. Esto deriva en cálculos de costos inexactos que pueden provocar planes de consulta de menor calidad. Si evita estas construcciones en las consultas, el rendimiento de las mismas podría mejora. En ocasiones es posible utilizar formulaciones de consulta alternativas u otras medidas; esto se indicará en esas ocasiones:

- Consultas con predicados que utilizan operadores de comparación entre distintas columnas de la misma tabla.
- Consultas con predicados que utilizan operadores y se cumple alguna de las siguientes condiciones:
  - No hay estadísticas en las columnas que se utilizan a uno u otro lado de los operadores.
  - La distribución de valores en las estadísticas no es uniforme, pero la consulta busca un conjunto de valores muy selectivos. Esta situación se cumple especialmente cuando el operador es distinto al operador de igualdad (=).
  - El predicado utiliza el operador de comparación No es igual a (!=) o el operador lógico `NOT`.
- Consultas que utilizan alguna de las funciones integradas de SQL Server o una función escalar definida por el usuario cuyo argumento no es un valor constante.
- Consultas que implican columnas de combinación por medio de operadores aritméticos o de concatenación de cadenas.
- Consultas que comparan variables cuyos valores no se conocen cuando la consulta se compila y optimiza.

En este artículo se explica cómo evaluar y elegir la mejor configuración de estimación de cardinalidad para su sistema. La mayoría de los sistemas sacan partido de la última estimación de cardinalidad, porque es la más precisa. Con la estimación de cardinalidad se predice cuántas filas va a devolver la consulta casi con toda seguridad. El optimizador de consultas usa la predicción de cardinalidad para generar el mejor plan de consulta posible. Con estimaciones más precisas, el optimizador de consultas normalmente puede hacer mejor su trabajo a la hora de generar un plan de consulta óptimo.  
  
Es bastante probable que el sistema de aplicaciones tenga una consulta importante cuyo plan cambie a un plan más lento debido a cambios en la estimación de cardinalidad a lo largo de las versiones. Existen diversas técnicas y herramientas para detectar una consulta que se ralentiza debido a problemas de la estimación de cardinalidad. También hay diversas opciones para abordar los problemas de rendimiento resultantes.
  
## <a name="versions-of-the-ce"></a>Versiones de la estimación de cardinalidad

En 1998, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 incorporó una actualización importante de la estimación de cardinalidad, para la que el nivel de compatibilidad fue 70. Esta versión del modelo de estimación de cardinalidad se establece sobre cuatro suposiciones básicas:

-  **Independencia:** se supone que las distribuciones de datos en otras columnas de métodos son independientes entre sí, a menos que la información de correlación esté disponible y se pueda usar.
-  **Uniformidad:** los distintos valores tienen un espaciado uniforme y todos tienen la misma frecuencia. Concretamente, dentro de cada paso del [histograma](../../relational-databases/statistics/statistics.md#histogram), los distintos valores se distribuyen uniformemente y cada valor tiene la misma frecuencia. 
-  **Contención (simple):** los usuarios consultan datos que existen. Por ejemplo, para una combinación de igualdad entre dos tablas, tenga en cuenta la selectividad de predicados <sup>1</sup> en cada histograma de entrada antes de unir histogramas para estimar la selectividad de combinación. 
-  **Inclusión:** para los predicados de filtro donde `Column = Constant`, se supone que la constante existe realmente para la columna asociada. Si un paso del histograma correspondiente no está vacío, se supone que uno de los valores distintos de los pasos coincide con el valor del predicado.

  Recuento de <sup>1</sup> filas que cumple el predicado.

Las actualizaciones posteriores empezaron por [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], que se tradujo en los niveles de compatibilidad 120 y posteriores. Las actualizaciones de estimación de cardinalidad correspondientes a los niveles 120 y posteriores incorporan suposiciones y algoritmos actualizados que funcionan bien en el almacenamiento de datos modernos y en las cargas de trabajo OLTP. Desde las suposiciones de estimación de cardinalidad de nivel 70, se cambiaron las siguientes suposiciones del modelo a partir de la estimación de cardinalidad de nivel 120:

-  La **independencia** se convierte en **correlación:** la combinación de los distintos valores de columna no son necesariamente independientes. Esto puede parecerse más a las consultas de datos reales.
-  La **contención simple** se convierte en **contención de base:** es posible que los usuarios consulten datos que no existen. Por ejemplo, para una combinación de igualdad entre dos tablas, usamos los histogramas de tablas base para calcular la selectividad de combinación y, después, el factor en la selectividad de predicados.
  
**Nivel de compatibilidad:** para asegurarse de que la base de datos esté en un nivel determinado, use el siguiente código de [!INCLUDE[tsql](../../includes/tsql-md.md)] para [COMPATIBILITY_LEVEL](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).  

```sql  
SELECT ServerProperty('ProductVersion');  
GO  
  
ALTER DATABASE <yourDatabase>  
SET COMPATIBILITY_LEVEL = 130;  
GO  
  
SELECT d.name, d.compatibility_level  
FROM sys.databases AS d  
WHERE d.name = 'yourDatabase';  
GO  
```  
  
En el caso de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con un nivel de compatibilidad 120 o superior, la activación de la [marca de seguimiento 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) obliga al sistema a usar la versión 70 de la estimación de cardinalidad.  
  
**Estimación de cardinalidad heredada:** en el caso de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el nivel de compatibilidad 120 y superior, la versión 70 de la estimación de cardinalidad se puede activar en el nivel de base de datos con la instrucción [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).
  
```sql  
ALTER DATABASE SCOPED CONFIGURATION 
SET LEGACY_CARDINALITY_ESTIMATION = ON;  
GO  
  
SELECT name, value  
FROM sys.database_scoped_configurations  
WHERE name = 'LEGACY_CARDINALITY_ESTIMATION';  
GO
```  
 
O bien, a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md#use_hint) `USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION')`.
 
 ```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01'
OPTION (USE HINT ('FORCE_LEGACY_CARDINALITY_ESTIMATION'));  
```
 
**Almacén de consultas:** a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el almacén de consultas es una herramienta muy útil para examinar el rendimiento de las consultas. En [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], en el **Explorador de objetos**, debajo del nodo de la base de datos, se muestra un nodo del **Almacén de consultas**, si este está habilitado.  
  
```sql  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE = ON;  
GO  
  
SELECT q.actual_state_desc AS [actual_state_desc_of_QueryStore],  
        q.desired_state_desc,  
        q.query_capture_mode_desc  
FROM sys.database_query_store_options AS q;  
GO  
  
ALTER DATABASE <yourDatabase>  
SET QUERY_STORE CLEAR;  
```  
  
> [!TIP] 
> Se recomienda instalar la última versión de [Management Studio](../../ssms/download-sql-server-management-studio-ssms.md) y actualizarlo con frecuencia.  

> [!IMPORTANT] 
> Asegúrese de que el almacén de consultas esté configurado correctamente para la base de datos y la carga de trabajo. Para más información, consulte [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md). 
  
Otra opción para llevar un seguimiento del proceso de estimación de cardinalidad consiste en usar el evento extendido denominado **query_optimizer_estimate_cardinality**. El siguiente código de ejemplo de [!INCLUDE[tsql](../../includes/tsql-md.md)] se ejecuta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Escribe un archivo .xel en `C:\Temp\` (aunque la ruta de acceso se puede cambiar). Cuando abra el archivo .xel en [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], podrá consultar la información detallada de forma muy sencilla.  
  
```sql  
DROP EVENT SESSION Test_the_CE_qoec_1 ON SERVER;  
go  
  
CREATE EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
ADD EVENT sqlserver.query_optimizer_estimate_cardinality  
    (  
        ACTION (sqlserver.sql_text)  
            WHERE (  
                sql_text LIKE '%yourTable%'  
                and sql_text LIKE '%SUM(%'  
            )  
    )  
ADD TARGET package0.asynchronous_file_target   
        (SET  
            filename = 'c:\temp\xe_qoec_1.xel',  
            metadatafile = 'c:\temp\xe_qoec_1.xem'  
        );  
GO  
  
ALTER EVENT SESSION Test_the_CE_qoec_1  
ON SERVER  
STATE = START;  --STOP;  
GO  
```  
  
Para obtener más información sobre los eventos extendidos adaptados para [!INCLUDE[ssSDS](../../includes/sssds-md.md)], consulte [Eventos extendidos en SQL Database](/azure/azure-sql/database/xevent-db-diff-from-svr).  
  
## <a name="steps-to-assess-the-ce-version"></a>Pasos para evaluar la versión de estimación de cardinalidad  
  
Estos son los pasos que se pueden realizar para saber si alguna de las consultas más importantes tiene un peor rendimiento tras la estimación de cardinalidad más reciente. Algunos de estos pasos se llevan a cabo ejecutando código de ejemplo incluido en una sección anterior.  
  
1.  Abra [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. Asegúrese de que la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está establecida en el nivel de compatibilidad más alto disponible.  
  
2.  Realice los siguientes pasos preliminares:  
  
    1.  Abra [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)].  
  
    2.  Ejecute el código T_SQL para asegurarse de que la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está establecida en el nivel de compatibilidad más alto disponible.  
  
    3.  Asegúrese de que la configuración `LEGACY_CARDINALITY_ESTIMATION` de la base de datos está desactivada.  
  
    4.  Borre el contenido del almacén de consultas. Asegúrese de que el almacén de consultas está activado.  
  
    5.  Ejecute la instrucción `SET NOCOUNT OFF;`.  
  
3.  Ejecute la instrucción `SET STATISTICS XML ON;`.  
  
4.  Ejecute la consulta importante.  
  
5.  En la pestaña **Mensajes** del panel de resultados, anote el número real de filas afectadas.  
  
6.  En la pestaña **Resultados** del panel de resultados, haga doble clic en la celda que contiene las estadísticas en formato XML. Se abre un plan de consulta gráfico.  
  
7.  Haga clic con el botón derecho en el primer cuadro del plan de consulta gráfico y, después, haga clic en **Propiedades**.  
  
8.  A fin de poder compararlos posteriormente con otra configuración, anote los valores de las siguientes propiedades:  
  
    -   **CardinalityEstimationModelVersion**.  
  
    -   **Número de filas estimado**.  
  
    -   **Costo de E/S estimado** y otras propiedades de *estimación* similares que tengan que ver con el rendimiento real más que con las predicciones de números de filas.  
  
    -   **Operación lógica** y **Operación física**. *Paralelismo* es un buen valor.  
  
    -   **Modo de ejecución real**. *Lote* es un buen valor, mejor que *Fila*.  
  
9. Compare el número estimado de filas con el número real de filas. ¿Existe una imprecisión de la estimación de cardinalidad de un 1 % (por encima o por debajo) o de un 10 %?  
  
10. Ejecute `SET STATISTICS XML OFF;`.  
  
11. Ejecute el código T-SQL para rebajar la base de datos un nivel de compatibilidad (por ejemplo, de 130 a 120).  
  
12. Vuelva a ejecutar todos los pasos no preliminares.  
  
13. Compare los valores de propiedad de la estimación de cardinalidad de ambos procesos.  
  
    - ¿Es el porcentaje de imprecisión con la nueva estimación de cardinalidad inferior que con la estimación de cardinalidad anterior?  
  
14. Por último, compare los distintos valores de propiedad de rendimiento de ambos procesos.  
  
    -   ¿Usó la consulta un plan diferente en cada una de las estimaciones de cardinalidad?  
  
    -   ¿Tuvo la consulta un rendimiento inferior con la estimación de cardinalidad más reciente?  
  
    -   A menos que la consulta muestre un mejor rendimiento y con un plan distinto en la estimación de cardinalidad anterior, lo más probable es que la estimación de cardinalidad más reciente sea la que más le convenga.  
  
    -   Pero si la consulta se ejecuta con un plan más rápido en la estimación de cardinalidad anterior, considere la posibilidad de obligar al sistema a usar el plan más rápido y omitir la estimación de cardinalidad. De este modo, dispondrá de la estimación de cardinalidad más reciente para todo y, al mismo tiempo, mantendrá el plan más rápido para el caso inusual.  
  
## <a name="how-to-activate-the-best-query-plan"></a>Cómo activar el mejor plan de consulta  
  
Supongamos que, con la estimación de cardinalidad de nivel 120 o superior, se genera un plan de consulta menos eficaz para la consulta. Estas son algunas opciones disponibles para activar el plan mejor plan:  
  
1. El nivel de compatibilidad se podría establecer en un nivel inferior que el último disponible para toda la base de datos.  
  
   - Por ejemplo, al establecer el nivel de compatibilidad 110 o inferior, se activa la estimación de cardinalidad de nivel 70, pero todas las consultas quedan sujetas al modelo de estimación de cardinalidad anterior.  
  
   - Además, si se ajusta un nivel de compatibilidad inferior también falta una serie de mejoras en el optimizador de consultas para las versiones más recientes.  
  
2. Puede usar la opción de base de datos `LEGACY_CARDINALITY_ESTIMATION` para que toda la base de datos use la estimación de cardinalidad anterior y conservar al mismo tiempo otras mejoras en el optimizador de consultas.   

3. Puede usar la opción de consulta `LEGACY_CARDINALITY_ESTIMATION` para que una sola consulta use la estimación de cardinalidad anterior y conservar al mismo tiempo otras mejoras en el optimizador de consultas.  
  
Para lograr el mayor control, podría *obligar* al sistema a usar el plan que se generó con la estimación de cardinalidad de 70 durante las pruebas. Después de *asignar* un plan de su elección, puede configurar toda la base de datos de forma que use el nivel de compatibilidad y la estimación de cardinalidad más recientes. Pasemos a explicar esta opción.  
  
### <a name="how-to-force-a-particular-query-plan"></a>Cómo forzar un plan de consulta particular  
  
El almacén de consultas ofrece diferentes formas para obligar al sistema a usar un plan de consulta en particular:  
  
- Ejecute **sp_query_store_force_plan**.  
  
- En [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)], expanda el nodo **Almacén de consultas**, haga clic con el botón derecho en **Consultas que más recursos consumen** y, después, haga clic en **Ver consultas que más recursos consumen**. La pantalla recoge los botones **Forzar plan** y **No forzar plan**.  
  
Para obtener más información sobre el almacén de consultas, vea [Supervisar el rendimiento mediante el almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).  
  
## <a name="examples-of-ce-improvements"></a>Ejemplos de mejoras en la estimación de cardinalidad  
  
En esta sección se muestran consultas de ejemplo en las que se saca partido de las mejoras implementadas en la estimación de cardinalidad en versiones recientes. Se trata de información general que no precisa de ninguna acción por su parte.  
  
### <a name="example-a-ce-understands-maximum-value-might-be-higher-than-when-statistics-were-last-gathered"></a>Ejemplo A. La estimación de cardinalidad entiende que el valor máximo podría ser superior al de las últimas estadísticas recopiladas  
  
Imagine que las estadísticas para `OrderTable` se recopilaron por última vez en `2016-04-30`, cuando el valor `OrderAddedDate` máximo era `2016-04-30`. La estimación de cardinalidad de nivel 120 (y versiones posteriores) entiende que las columnas en `OrderTable` con datos *ascendentes* podrían tener valores mayores que el máximo registrado por las estadísticas. Este entendimiento mejora el plan de consulta para instrucciones SELECT de [!INCLUDE[tsql](../../includes/tsql-md.md)] como la siguiente.  
  
```sql  
SELECT CustomerId, OrderAddedDate  
FROM OrderTable  
WHERE OrderAddedDate >= '2016-05-01';  
```  
  
### <a name="example-b-ce-understands-that-filtered-predicates-on-the-same-table-are-often-correlated"></a>Ejemplo B. La estimación de cardinalidad entiende que, a menudo, los predicados filtrados en una misma tabla se correlacionan  
  
En la siguiente instrucción SELECT vemos predicados filtrados en `Model` y `ModelVariant`. Deducimos de manera intuitiva que, cuando `Model` es "Xbox", hay una posibilidad de que `ModelVariant` sea "One", dado que Xbox tiene una variante que se llama One.  
  
A partir de la estimación de calidad de nivel 120, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entiende que podría haber una correlación entre las dos columnas en la misma tabla, `Model` y `ModelVariant`. La estimación de cardinalidad hace una estimación más precisa del número de filas que la consulta va a devolver, mientras que el [optimizador de consultas](../../relational-databases/query-processing-architecture-guide.md#optimizing-select-statements) genera un plan mucho mejor.  
  
```sql  
SELECT Model, Purchase_Price  
FROM dbo.Hardware  
WHERE Model = 'Xbox' AND  
      ModelVariant = 'One';  
```  
  
### <a name="example-c-ce-no-longer-assumes-any-correlation-between-filtered-predicates-from-different-tables"></a>Ejemplo C. La estimación de cardinalidad ya no da por hecho ninguna correlación entre los predicados filtrados de tablas distintas 
Las nuevas investigaciones exhaustivas en las cargas de trabajo y datos empresariales reales de hoy día han puesto de manifiesto que predicar filtros de tablas distintas no hace que se establezca ningún tipo de correlación entre sí. En la siguiente consulta, la estimación de cardinalidad da por hecho que no hay ninguna correlación entre `s.type` y `r.date`. Por tanto, hace una estimación más baja del número de filas devuelto.  
  
```sql  
SELECT s.ticket, s.customer, r.store  
FROM dbo.Sales AS s  
CROSS JOIN dbo.Returns AS r  
WHERE s.ticket = r.ticket AND  
      s.type = 'toy' AND  
      r.date = '2016-05-11';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)   
 [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](/previous-versions/dn673537(v=msdn.10))  
 [Sugerencias de consulta](../../t-sql/queries/hints-transact-sql-query.md)     
 [Sugerencias de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint)       
 [Actualización de bases de datos mediante el Asistente para la optimización de consultas](../../relational-databases/performance/upgrade-dbcompat-using-qta.md)           
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)    
 [Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md)