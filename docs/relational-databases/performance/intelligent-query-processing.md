---
title: Procesamiento de consultas inteligentes
description: Características de procesamiento de consultas inteligente para mejorar el rendimiento de las consultas en SQL Server y Azure SQL Database.
ms.custom: seo-dt-2019
ms.date: 11/27/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords: ''
author: joesackmsft
ms.author: josack
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 40a2ba2ff9e3955c09a8d8529a67ec3275e17222
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338962"
---
# <a name="intelligent-query-processing-in-sql-databases"></a>Procesamiento de consultas inteligente en bases de datos SQL

[!INCLUDE [SQL Server Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

La familia de características de procesamiento de consultas inteligentes incluye características con un gran impacto que mejoran el rendimiento de las cargas de trabajo existentes con un esfuerzo de implementación mínimo. 

![Procesamiento de consultas inteligentes](./media/iqp-feature-family.png)

Vea este vídeo de seis minutos para obtener información general sobre el procesamiento de consultas inteligentes:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Intelligent-Query-processing-in-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


Puede hacer que las cargas de trabajo sean aptas automáticamente para el procesamiento de consultas inteligentes si habilita el nivel de compatibilidad de base de datos pertinente en la base de datos. Puede establecerlo con [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por ejemplo:  

```sql
ALTER DATABASE [WideWorldImportersDW] SET COMPATIBILITY_LEVEL = 150;
```

En la siguiente tabla se detallan todas las características de procesamiento de consultas inteligentes, así como cualquier requisito que tengan en cuanto a nivel de compatibilidad de base de datos.

| **Característica de procesamiento de consultas inteligentes** | **Se admite en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]** | **Se admite en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]** |**Descripción** |
| ---------------- | ------- | ------- | ---------------- |
| [Combinaciones adaptables (modo por lotes)](#batch-mode-adaptive-joins) | Sí, en el nivel de compatibilidad 140| Sí, a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] en el nivel de compatibilidad 140|Las combinaciones adaptables seleccionan dinámicamente un tipo de combinación en tiempo de ejecución según las filas de entrada reales.|
| [Count Distinct aproximada](#approximate-query-processing) | Sí| Sí, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]|Proporcione un valor de COUNT DISTINCT aproximado en escenarios de macrodatos, con la ventaja de un alto rendimiento y una baja superficie de memoria. |
| [Modo por lotes en el almacén de filas](#batch-mode-on-rowstore) | Sí, en el nivel de compatibilidad 150| Sí, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] en el nivel de compatibilidad 150|Proporcione el modo por lotes en las cargas de trabajo de almacenamiento de datos relacionales enlazadas a la CPU, sin necesidad de índices de almacén de columnas.  | 
| [Ejecución intercalada](#interleaved-execution-for-mstvfs) | Sí, en el nivel de compatibilidad 140| Sí, a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] en el nivel de compatibilidad 140|Use la cardinalidad real de la función con valores de tabla y múltiples instrucciones detectada en la primera compilación en lugar de una estimación fija.|
| [Comentarios de concesión de memoria (modo por lotes)](#batch-mode-memory-grant-feedback) | Sí, en el nivel de compatibilidad 140| Sí, a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] en el nivel de compatibilidad 140|Si una consulta de modo por lotes tiene operaciones que escriben en disco, agregue más memoria para las ejecuciones consecutivas. Si una consulta desperdicia más del 50 % de memoria que tiene asignada, reduzca el lado de concesión de memoria en las ejecuciones consecutivas.|
| [Comentarios de concesión de memoria (modo de fila)](#row-mode-memory-grant-feedback) | Sí, en el nivel de compatibilidad 150| Sí, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] en el nivel de compatibilidad 150|Si una consulta de modo de fila tiene operaciones que escriben en disco, agregue más memoria para las ejecuciones consecutivas. Si una consulta desperdicia más del 50 % de memoria que tiene asignada, reduzca el lado de concesión de memoria en las ejecuciones consecutivas.|
| [Inserción de UDF escalar](#scalar-udf-inlining) | No | Sí, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] en el nivel de compatibilidad 150|Los UDF escalares se transforman en expresiones relacionales equivalentes que se "insertan" en la consulta que realiza la llamada, lo que a menudo supone una notable mejora del rendimiento.|
| [Compilación diferida de variables de tabla](#table-variable-deferred-compilation) | Sí, en el nivel de compatibilidad 150| Sí, a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] en el nivel de compatibilidad 150|Use la cardinalidad real de la variable de tabla detectada en la primera compilación en lugar de una estimación fija.|

## <a name="batch-mode-adaptive-joins"></a>Combinaciones adaptables del modo por lotes
La característica de combinaciones adaptables del modo por lotes permite elegir un método de [combinación hash o combinación de bucles anidados](../../relational-databases/performance/joins.md) que se aplace hasta **después** de que se haya examinado la primera entrada, mediante un único plan en caché. El operador de combinaciones adaptables define un umbral que se usa para decidir cuándo cambiar a un plan de bucles anidados. El plan, por tanto, puede cambiar de forma dinámica para una mejor estrategia de combinación durante la ejecución.

Para más información, incluido cómo deshabilitar las combinaciones adaptables sin cambiar el nivel de compatibilidad, vea [Descripción de las combinaciones adaptables](../../relational-databases/performance/joins.md#adaptive).

## <a name="batch-mode-memory-grant-feedback"></a>Comentarios de concesión de memoria de modo de proceso por lotes
El plan posterior a la ejecución de una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye la memoria mínima necesaria para la ejecución y el tamaño de concesión de memoria ideal para que todas las filas quepan en la memoria. El rendimiento se ve afectado si los tamaños de concesión de memoria son incorrectos. A su vez, unas concesiones excesivas se traducen en memoria desperdiciada y en simultaneidad reducida. Las concesiones de memoria insuficientes provocan un costoso desbordamiento en disco. Al ocuparse de las cargas de trabajo repetidas, los comentarios de concesión de memoria de modo de proceso por lotes vuelven a calcular la memoria real necesaria para una consulta y luego actualizan el valor de la concesión del plan almacenado en caché. Cuando se ejecuta una instrucción de consulta idéntica, la consulta usa el tamaño de concesión de memoria revisado, con lo que se reducen las concesiones de memoria excesivas que afectan a la simultaneidad y se solucionan las concesiones de memoria subestimadas que provocan costosos desbordamientos en disco.
El gráfico siguiente muestra un ejemplo de uso de los comentarios de concesión de memoria adaptable de modo de proceso por lotes. Para la primera ejecución de la consulta, la duración es de **88 segundos**, debido a los grandes desbordamientos:   

```sql
DECLARE @EndTime datetime = '2016-09-22 00:00:00.000';
DECLARE @StartTime datetime = '2016-09-15 00:00:00.000';
SELECT TOP 10 hash_unique_bigint_id
FROM dbo.TelemetryDS
WHERE Timestamp BETWEEN @StartTime and @EndTime
GROUP BY hash_unique_bigint_id
ORDER BY MAX(max_elapsed_time_microsec) DESC;
```

![Grandes desbordamientos](./media/2_AQPGraphHighSpills.png)

Con los comentarios de concesión de memoria habilitados para la segunda ejecución, la duración es de **1 segundo** (partiendo de 88 segundos), los desbordamientos se eliminan por completo y la concesión es superior: 

![Sin desbordamientos](./media/3_AQPGraphNoSpills.png)

### <a name="memory-grant-feedback-sizing"></a>Tamaño de los comentarios de concesión de memoria
En el caso de una condición de concesión de memoria excesiva, si la memoria concedida es más de dos veces el tamaño de la memoria usada real, los comentarios de concesión de memoria volverán a calcular la concesión de memoria y actualizarán el plan almacenado en caché. Los planes con concesiones de memoria por debajo de 1 MB no se vuelven a calcular para usos por encima del límite.
En el caso de condiciones de concesión de memoria de tamaño insuficiente, que provocan un desbordamiento en disco para operadores de modo de proceso por lotes, los comentarios de concesión de memoria desencadenarán un nuevo cálculo de la concesión de memoria. Los eventos de desbordamiento se notifican a los comentarios de concesión de memoria y se pueden mostrar a través del evento de xEvent *spilling_report_to_memory_grant_feedback*. Este evento devuelve el identificador de nodo del plan y el tamaño de los datos desbordados de ese nodo.

### <a name="memory-grant-feedback-and-parameter-sensitive-scenarios"></a>Comentarios de concesión de memoria y escenarios confidenciales de parámetros
Los distintos valores de parámetros también pueden necesitar diferentes planes de consulta para seguir siendo óptimos. Este tipo de consulta se define como "sensible a parámetros". En el caso de los planes sensibles a parámetros, los comentarios de concesión de memoria se deshabilitarán en una consulta si esta tiene requisitos de memoria inestables. El plan se deshabilita después de que se ejecute la consulta de forma repetida y esto puede observarse mediante la supervisión de xEvent *memory_grant_feedback_loop_disabled*. Para obtener más información sobre la sensibilidad y el examen de los parámetros, consulte la [Guía de arquitectura de procesamiento de consulta](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing).

### <a name="memory-grant-feedback-caching"></a>Almacenamiento en caché de los comentarios de concesión de memoria
Los comentarios pueden almacenarse en el plan almacenado en caché para una sola ejecución. Pero son las ejecuciones consecutivas de esa instrucción las que se benefician de los ajustes de los comentarios de concesión de memoria. Esta característica se aplica a la ejecución repetida de instrucciones. Los comentarios de concesión de memoria solo cambian el plan almacenado en caché. Los cambios no se capturan actualmente en el almacén de consultas.
Si el plan se elimina de la memoria caché, no se conservan los comentarios. Los comentarios también se pierden si se produce una conmutación por error. Una instrucción con `OPTION (RECOMPILE)` crea un plan y no lo almacena en caché. Puesto que no se almacena, no se generan comentarios de concesión de memoria y no se almacenan para esa compilación y ejecución. Pero si una instrucción equivalente (es decir, con el mismo hash de consulta) que **no** ha usado `OPTION (RECOMPILE)` se ha almacenado en caché y luego se ha vuelto a ejecutar, la instrucción consecutiva puede beneficiarse de los comentarios de concesión de memoria.

### <a name="tracking-memory-grant-feedback-activity"></a>Seguimiento de la actividad de los comentarios de concesión de memoria
Puede realizar un seguimiento de los eventos de comentarios de concesión de memoria mediante el evento de xEvent *memory_grant_updated_by_feedback*. Este evento realiza un seguimiento del historial de recuentos de ejecución actual, del número de veces que los comentarios de concesión de memoria han provocado una actualización del plan, de la concesión de memoria adicional ideal antes de la modificación y de la concesión de memoria adicional ideal después de que los comentarios de concesión de memoria hayan modificado el plan almacenado en caché.

### <a name="memory-grant-feedback-resource-governor-and-query-hints"></a>Comentarios de concesión de memoria, regulador de recursos y sugerencias de consulta
La memoria real concedida respeta el límite de memoria de consulta determinado por el regulador de recursos o la sugerencia de consulta.

### <a name="disabling-batch-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Deshabilitar los comentarios de concesión de memoria en modo de lote sin cambiar el nivel de compatibilidad
Los comentarios de concesión de memoria se pueden deshabilitar en el ámbito de base de datos o de instrucción mientras se mantiene el nivel de compatibilidad de base de datos 140 o posterior. Para deshabilitar los comentarios de concesión de memoria en modo por lotes para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Cuando se habilita, esta opción aparecerá como habilitada en [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

Para volver a habilitar los comentarios de concesión de memoria en modo por lotes para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK = OFF;

-- Azure SQL Database, SQL Server 2019 and higher
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

También puede deshabilitar los comentarios de concesión de memoria en modo por lotes para una consulta específica si designa `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` como [sugerencia de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Por ejemplo:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Una sugerencia de consulta USE HINT tiene prioridad sobre una configuración de ámbito de base de datos o una opción de marca de seguimiento.

## <a name="row-mode-memory-grant-feedback"></a>Comentarios de concesión de memoria del modo de fila

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

Los comentarios de concesión de memoria del modo de fila se expanden en la característica de comentarios de concesión de memoria de modo de proceso por lotes al ajustar los tamaños de concesión de memoria tanto para los operadores del modo de proceso por lotes como del modo de fila.  

Para habilitar los comentarios de concesión de memoria del modo de fila en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], habilite el nivel 150 de compatibilidad de la base de datos para la base de datos a la que se conecta cuando ejecuta la consulta.

La actividad de comentarios de concesión de memoria del modo de fila será visible a través del evento extendido **memory_grant_updated_by_feedback**. 

A partir de los comentarios de concesión de memoria del modo de fila, se mostrarán dos nuevos atributos de plan de consulta para planes reales posteriores a la ejecución: ***IsMemoryGrantFeedbackAdjusted** _ y _*_LastRequestedMemory_*_, que se agregan al elemento XML del plan de consulta MemoryGrantInfo*. 

*LastRequestedMemory* muestra la memoria concedida en Kilobytes (KB) desde la ejecución de consulta anterior. El atributo *IsMemoryGrantFeedbackAdjusted* permite comprobar el estado de los comentarios de concesión de memoria para la instrucción dentro de un plan de ejecución de consulta real. Los valores que se exponen en este atributo son los siguientes:

| Valor IsMemoryGrantFeedbackAdjusted | Descripción |
|---|---|
| No: First Execution | Los comentarios de concesión de memoria no ajustan la memoria para la primera compilación y la ejecución asociada.  |
| No: Accurate Grant | Si no hay ningún desbordamiento al disco y la instrucción usa al menos 50 % de la memoria concedida, no se desencadenan los comentarios de concesión de memoria. |
| No: Feedback disabled | Si los comentarios de concesión de memoria se desencadenan de manera continúa y fluctúan entre operaciones de aumento de memoria y operaciones de disminución de memoria, se deshabilitarán los comentarios de concesión de memoria para la instrucción. |
| Yes: Adjusting | Se aplicaron los comentarios de concesión de memoria y se pueden seguir ajustando para la próxima ejecución. |
| Yes: Stable | Se aplicaron los comentarios de concesión de memoria y ahora la memoria concedida es estable, lo que significa que lo último que se concedió para la ejecución anterior es lo que se concedió para la ejecución actual. |

### <a name="disabling-row-mode-memory-grant-feedback-without-changing-the-compatibility-level"></a>Deshabilitar los comentarios de concesión de memoria en modo de fila sin cambiar el nivel de compatibilidad
Los comentarios de concesión de memoria en modo de fila se pueden deshabilitar en el ámbito de base de datos o de instrucción mientras se mantiene el nivel de compatibilidad de base de datos 150 o posterior. Para deshabilitar los comentarios de concesión de memoria en modo de fila para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = OFF;
```

Para volver a habilitar los comentarios de concesión de memoria en modo de fila para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ROW_MODE_MEMORY_GRANT_FEEDBACK = ON;
```

También puede deshabilitar los comentarios de concesión de memoria en modo de fila para una consulta específica si designa `DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK` como [sugerencia de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Por ejemplo:

```sql
SELECT * FROM Person.Address  
WHERE City = 'SEATTLE' AND PostalCode = 98104
OPTION (USE HINT ('DISABLE_ROW_MODE_MEMORY_GRANT_FEEDBACK')); 
```

Una sugerencia de consulta USE HINT tiene prioridad sobre una configuración de ámbito de base de datos o una opción de marca de seguimiento.

## <a name="interleaved-execution-for-mstvfs"></a>Ejecución intercalada de MSTVF
Con la ejecución intercalada se usan los recuentos de filas reales de la función para tomar decisiones fundamentadas sobre los planes de consulta descendentes. Para obtener más información sobre las funciones con valores de tabla de múltiples instrucciones (MSTVF), consulte [ Funciones con valores de tabla](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

La ejecución intercalada cambia el límite unidireccional entre las fases de optimización y ejecución de una ejecución de una sola consulta y permite que los planes se adapten en función de las estimaciones de cardinalidad revisadas. Durante la optimización, si se detecta un candidato para la ejecución intercalada, que son actualmente las **funciones con valores de tabla de múltiples instrucciones (MSTVF)** , se detiene la optimización, se ejecuta el subárbol aplicable, se capturan las estimaciones de cardinalidad precisas y luego se reanuda la optimización de las operaciones de nivel inferior.   

Las MSTVF tienen una estimación de cardinalidad fija de 100 a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], y de 1 en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La ejecución intercalada ayuda con los problemas de rendimiento de las cargas de trabajo debidos a estas estimaciones de cardinalidad fijas asociadas a las MSTVF. Para obtener más información sobre las MSTVF, vea [Creación de funciones definidas por el usuario (motor de base de datos)](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#TVF).

En la imagen siguiente se muestran los resultados de [estadísticas de consultas dinámicas](../../relational-databases/performance/live-query-statistics.md), un subconjunto de un plan de ejecución global que refleja el impacto de las estimaciones de cardinalidad fijas de las MSTVF. Puede ver el flujo de filas real frente a las filas estimadas. Hay tres áreas reseñables del plan (el flujo va de derecha a izquierda):
1. El recorrido de tabla de MSTVF tiene una estimación fija de 100 filas. Pero en este ejemplo hay 527.597 filas que pasan por este recorrido de tabla de MSTVF, como se muestra en las estadísticas de consultas dinámicas a través de *527597 de 100* reales de estimados, por lo que la estimación fija está considerablemente desviada.
1. En el caso de la operación de bucles anidados, se supone que el lado externo de la combinación solo devuelve 100 filas. Dado el gran número de filas que las MSTVF devuelven en realidad, es probable que lo mejor sea un algoritmo de combinación totalmente diferente.
1. En el caso de la operación de coincidencia de hash, observe el pequeño símbolo de advertencia, que en este caso indica un desbordamiento en el disco.

![Flujo de filas frente a filas estimadas](./media/7_AQPFlowThreeAreas.png)

Compare el plan anterior con el plan real generado con la ejecución intercalada habilitada:

![Plan intercalado](./media/8_AQPInterleavedEnabledPlan.png)

1. Observe que el recorrido de tabla de MSTVF ahora refleja una estimación de cardinalidad precisa. Observe también la reordenación de este recorrido de tabla y las demás operaciones.
1. Con respecto a los algoritmos de combinación, se ha pasado de una operación de bucle anidado a una operación de coincidencia de hash, que es más adecuada dado el gran número de filas implicadas.
1. Observe también que ya no hay advertencias de desbordamiento, dado que se ha concedido más memoria en función del recuento real de filas que pasan desde el recorrido de tabla de MSTVF.

### <a name="interleaved-execution-eligible-statements"></a>Instrucciones aptas de ejecución intercalada
Las instrucciones que hacen referencia a las MSTVF en la ejecución intercalada de momento deben ser de solo lectura y no formar parte de ninguna operación de modificación de datos. Además, las MSTVF no son aptas para la ejecución intercalada si no usan constantes en tiempo de ejecución.

### <a name="interleaved-execution-benefits"></a>Ventajas de la ejecución intercalada
En general, cuanta mayor sea la distorsión entre el número de filas real y el estimado, además del número de operaciones de nivel inferior del plan, mayor será el impacto sobre el rendimiento.
En general, la ejecución intercalada beneficia a las consultas donde:
1. Hay una gran distorsión entre el número real de filas y el estimado del conjunto de resultados intermedio (en este caso, las MSTVF).
1. Y la consulta global es sensible a un cambio en el tamaño del resultado intermedio. Esto suele suceder cuando hay un árbol complejo por encima de ese subárbol en el plan de consulta.
Un simple instrucción `SELECT *` de una MSTVF no se beneficiará de la ejecución intercalada.

### <a name="interleaved-execution-overhead"></a>Sobrecarga de la ejecución intercalada
La sobrecarga debe ser de mínima a ninguna. Las MSTVF ya se estaban materializando antes de la introducción de la ejecución intercalada; la diferencia es que ahora se permite la optimización diferida y luego se aprovecha la estimación de cardinalidad del conjunto de filas materializado.
Al igual que cualquier plan que afecte a los cambios, algunos planes podrían cambiar de modo que con la mejor cardinalidad del subárbol se obtuviera un plan peor para la consulta en general. La mitigación puede incluir la reversión del nivel de compatibilidad o el uso del Almacén de consultas para aplicar la versión no revertida del plan.

### <a name="interleaved-execution-and-consecutive-executions"></a>Ejecución intercalada y ejecuciones consecutivas
Una vez que se almacena en caché un plan de ejecución intercalada, el plan con las estimaciones revisadas en la primera ejecución se usa para las ejecuciones consecutivas sin volver a crear una instancia de ejecución intercalada.

### <a name="tracking-interleaved-execution-activity"></a>Seguimiento de la actividad de ejecución intercalada
Puede ver los atributos de uso en el plan de ejecución de consulta real:

| Atributo del plan de ejecución | Descripción |
| --- | --- |
| ContainsInterleavedExecutionCandidates | Se aplica al nodo *QueryPlan*. Si *true* significa que el plan contiene candidatos de ejecución intercalada. |
| IsInterleavedExecuted | Atributo del elemento *RuntimeInformation* bajo el elemento RelOp del nodo de TVF. Si es *true*, significa que la operación se ha materializado como parte de una operación de ejecución intercalada. |

También puede realizar el seguimiento de repeticiones de ejecución intercalada mediante los siguientes eventos de xEvents:

| xEvent | Descripción |
| ---- | --- |
| interleaved_exec_status | Este evento se desencadena cuando se está produciendo la ejecución intercalada. |
| interleaved_exec_stats_update | Este evento describe las estimaciones de cardinalidad actualizadas por la ejecución intercalada. |
| Interleaved_exec_disabled_reason | Este evento se desencadena cuando una consulta con un posible candidato para la ejecución intercalada no consigue realmente la ejecución intercalada. |

Se debe ejecutar una consulta para permitir que la ejecución intercalada revise las estimaciones de cardinalidad de MSTVF. Pero el plan de ejecución estimado aún muestra cuándo hay candidatos de ejecución intercalada a través del atributo del plan de presentación de `ContainsInterleavedExecutionCandidates`.    

### <a name="interleaved-execution-caching"></a>Almacenamiento en caché de la ejecución intercalada
Si un plan se borra o se elimina de la memoria caché, al ejecutarse la consulta se genera una nueva compilación que usa la ejecución intercalada.
Una instrucción con `OPTION (RECOMPILE)` creará un plan con ejecución intercalada y no lo almacenará en la memoria caché.     

### <a name="interleaved-execution-and-query-store-interoperability"></a>Ejecución intercalada e interoperabilidad del Almacén de consultas
Los planes con ejecución intercalada se pueden aplicar. El plan es la versión que ha corregido las estimaciones de cardinalidad basándose en la ejecución inicial.    

### <a name="disabling-interleaved-execution-without-changing-the-compatibility-level"></a>Deshabilitar la ejecución intercalada sin cambiar el nivel de compatibilidad
La ejecución intercalada se puede deshabilitar en el ámbito de base de datos o de instrucción mientras se mantiene el nivel de compatibilidad de base de datos 140 o posterior.  Para deshabilitar la ejecución intercalada para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = ON;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = OFF;
```

Cuando se habilita, esta opción aparecerá como habilitada en [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).
Para volver a habilitar la ejecución intercalada para todas las ejecuciones de consultas que se originan en la base de datos, ejecute lo siguiente en el contexto de la base de datos aplicable:

```sql
-- SQL Server 2017
ALTER DATABASE SCOPED CONFIGURATION SET DISABLE_INTERLEAVED_EXECUTION_TVF = OFF;

-- Starting with SQL Server 2019, and in Azure SQL Database
ALTER DATABASE SCOPED CONFIGURATION SET INTERLEAVED_EXECUTION_TVF = ON;
```

También puede deshabilitar la ejecución intercalada para una consulta específica si designa `DISABLE_INTERLEAVED_EXECUTION_TVF` como una [sugerencia de consulta USE HINT](../../t-sql/queries/hints-transact-sql-query.md#use_hint). Por ejemplo:

```sql
SELECT [fo].[Order Key], [fo].[Quantity], [foo].[OutlierEventQuantity]
FROM [Fact].[Order] AS [fo]
INNER JOIN [Fact].[WhatIfOutlierEventQuantity]('Mild Recession',
                            '1-01-2013',
                            '10-15-2014') AS [foo] ON [fo].[Order Key] = [foo].[Order Key]
                            AND [fo].[City Key] = [foo].[City Key]
                            AND [fo].[Customer Key] = [foo].[Customer Key]
                            AND [fo].[Stock Item Key] = [foo].[Stock Item Key]
                            AND [fo].[Order Date Key] = [foo].[Order Date Key]
                            AND [fo].[Picked Date Key] = [foo].[Picked Date Key]
                            AND [fo].[Salesperson Key] = [foo].[Salesperson Key]
                            AND [fo].[Picker Key] = [foo].[Picker Key]
OPTION (USE HINT('DISABLE_INTERLEAVED_EXECUTION_TVF'));
```

Una sugerencia de consulta USE HINT tiene prioridad sobre una configuración de ámbito de base de datos o una opción de marca de seguimiento.

## <a name="table-variable-deferred-compilation"></a>Compilación diferida de variables de tabla

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

La **compilación diferida de variables de tabla** mejora la calidad del plan y el rendimiento general de las consultas que hacen referencia a las variables de tabla. Durante la optimización y la compilación del plan inicial, esta característica propagará las estimaciones de cardinalidad que se basan en los recuentos de filas de variables de tabla reales. Esta información exacta de recuento de filas se usará para optimizar las operaciones del plan de bajada.

Con la compilación aplazada variable de tabla, la compilación de una instrucción que hace referencia a una variable de tabla se aplaza hasta que la primera ejecución real de la instrucción. Este comportamiento de compilación diferida es idéntico al comportamiento de las tablas temporales. Este cambio se traduce en el uso de la cardinalidad real en lugar de la estimación de una fila original. 

Para habilitar la compilación diferida de variables de tabla, habilite el nivel de compatibilidad 150 de la base de datos a la que se conecta cuando ejecuta la consulta.

La compilación diferida de variables de tabla **no** cambia ninguna otra característica de las variables de tabla. Por ejemplo, esta característica no agrega estadísticas de columna a las variables de tabla.

La compilación diferida de variables de tabla **no aumenta la frecuencia de recompilación**. En su lugar, se desplaza a donde se produce la compilación inicial. El plan en caché resultante se genera en función del recuento de filas de variables de tabla de la compilación diferida. Este plan se reutiliza en consultas sucesivas, y hasta que se expulsa o se vuelve a compilar. 

El recuento de filas de variables de tabla que se usa para la compilación inicial del plan representa un valor típico que podría ser diferente de una estimación del recuento de filas fijo. Si es diferente, las operaciones de bajada saldrán beneficiadas. Si el recuento de filas de variables de tabla varía considerablemente entre ejecuciones, es posible que esta característica no mejore el rendimiento.

### <a name="disabling-table-variable-deferred-compilation-without-changing-the-compatibility-level"></a>Deshabilitación de la compilación diferida de variables de tabla sin cambiar el nivel de compatibilidad
Deshabilite la compilación diferida de variables de tabla en el ámbito de base de datos o de instrucción mientras se mantiene el nivel 150 o posterior de compatibilidad de base de datos. Para deshabilitar la compilación diferida de variables de tabla para todas las ejecuciones de consultas que se originan en la base de datos, ejecute el siguiente ejemplo en el contexto de la base de datos aplicable:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = OFF;
```

Para volver a habilitar la compilación diferida de variables de tabla para todas las ejecuciones de consultas que se originan en la base de datos, ejecute el siguiente ejemplo en el contexto de la base de datos aplicable:

```sql
ALTER DATABASE SCOPED CONFIGURATION SET DEFERRED_COMPILATION_TV = ON;
```

También puede deshabilitar la compilación diferida de variables de tabla para una consulta específica mediante la designación de DISABLE_DEFERRED_COMPILATION_TV como una sugerencia de consulta USE HINT.  Por ejemplo:

```sql
DECLARE @LINEITEMS TABLE 
    (L_OrderKey INT NOT NULL,
     L_Quantity INT NOT NULL
    );

INSERT @LINEITEMS
SELECT L_OrderKey, L_Quantity
FROM dbo.lineitem
WHERE L_Quantity = 5;

SELECT O_OrderKey,
    O_CustKey,
    O_OrderStatus,
    L_QUANTITY
FROM    
    ORDERS,
    @LINEITEMS
WHERE    O_ORDERKEY    =    L_ORDERKEY
    AND O_OrderStatus = 'O'
OPTION (USE HINT('DISABLE_DEFERRED_COMPILATION_TV'));
```

## <a name="scalar-udf-inlining"></a>Inserción de UDF escalar

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

La inserción de UDF escalar transforma automáticamente las [UDF escalares](../../relational-databases/user-defined-functions/create-user-defined-functions-database-engine.md#Scalar) en expresiones relacionales. Las inserta en la consulta SQL de llamada. Esta transformación mejora el rendimiento de las cargas de trabajo que aprovechan las UDF escalares. La inserción de UDF escalar facilita la optimización basada en costos de las operaciones dentro de las UDF. Los resultados son eficaces, orientados a conjuntos y paralelos en lugar de tratarse de planes de ejecución seriales, iterativos e ineficaces. Esta característica está habilitada de forma predeterminada en el nivel de compatibilidad de base de datos 150.

Para obtener más información, vea [Inserción de UDF escalares](../user-defined-functions/scalar-udf-inlining.md).

## <a name="approximate-query-processing"></a>Procesamiento de consultas aproximado

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

El procesamiento de consultas aproximado es una nueva familia de características. Agrega conjuntos de datos de gran tamaño en los que la capacidad de respuesta resulta más fundamental que la precisión absoluta. Un ejemplo es calcular el valor **COUNT(DISTINCT())** entre 10 mil millones de filas para mostrar en un panel. En este caso, la precisión absoluta no es importante, pero la capacidad de respuesta es fundamental. La función de agregado **APPROX_COUNT_DISTINCT** nueva devuelve el número aproximado de valores no nulos únicos de un grupo.

Para más información, consulte [APPROX_COUNT_DISTINCT (Transact-SQL)](../../t-sql/functions/approx-count-distinct-transact-sql.md).

## <a name="batch-mode-on-rowstore"></a>Modo por lotes en el almacén de filas 

**Se aplica a:** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 

El modo por lotes en el almacén de filas permite la ejecución en modo por lotes de las cargas de trabajo de análisis sin necesidad de índices de almacén de columnas.  Esta característica admite la ejecución en modo por lotes y filtros de mapa de bits para montones en disco e índices de árbol B. El modo por lotes en el almacén de filas permite la compatibilidad con todos operadores habilitados para el modo por lotes existentes.

### <a name="background"></a>Información previa
[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] introdujo una nueva característica para acelerar las cargas de trabajo de análisis: los índices de almacén de columnas. Ampliamos los casos de uso y mejoramos el rendimiento de los índices de almacén de columnas en las versiones posteriores. Hasta ahora, se han descubierto y documentado todas estas funciones como una sola característica. Crea los índices de almacén de columnas en sus tablas. Y su carga de trabajo de análisis va más rápido. Sin embargo, hay dos conjuntos diferentes de tecnologías, aunque guardan relación:
- Con los índices de **almacén de columnas**, las consultas analíticas tienen acceso solo a los datos de las columnas que necesitan. La compresión de página en formato de almacén de columnas también es más eficaz que la compresión en los índices de **almacén de filas** tradicionales. 
- Con el procesamiento de **modo por lotes**, los operadores de consulta procesan los datos con mayor eficacia. Funcionan en un lote de filas en lugar de una fila cada vez. Hay más mejoras de escalabilidad relacionadas con el proceso en modo por lotes. Para obtener más información sobre el modo por lotes, consulte [Modos de ejecución](../../relational-databases/query-processing-architecture-guide.md#execution-modes).

Los dos conjuntos de características funcionan conjuntamente para mejorar la utilización de entrada y salida (E/S) y CPU:
- Mediante el uso de índices de almacén de columnas, más datos suyos caben en la memoria. Esto reduce la carga de trabajo de E/S.
- El proceso en modo por lotes utiliza la CPU de manera más eficaz.

Las dos tecnologías se apoyan entre sí siempre que es posible. Por ejemplo, es posible evaluar agregados del modo por lotes como parte de una exploración del índice de almacén de columnas. Asimismo, los datos comprimidos del almacén de columnas se procesan mediante la codificación en longitud del recorrido de forma mucho más eficiente con combinaciones y agregados de modo por lotes. 
 
Sin embargo, es importante comprender que las dos características son independientes:
* Puede obtener planes de modo de fila que usan índices de almacén de columnas.
* Puede obtener planes de modo por lotes que usan índices de almacén de filas. 

Normalmente obtiene los mejores resultados al usar las dos características conjuntamente. Así pues, hasta ahora, el optimizador de consultas de SQL Server ha tenido en cuenta el procesamiento de modo por lotes solo para aquellas consultas que implican al menos una tabla con un índice de almacén de columnas.

Es posible que los índices de almacén de columnas no sean adecuados para algunas aplicaciones. Una aplicación podría usar cualquier otra característica no compatible con los índices de almacén de columnas. Por ejemplo, las modificaciones en contexto no son compatibles con la compresión del almacén de columnas. Por tanto, los desencadenadores no se admiten en tablas con índices de almacén de columnas en clúster. Y, lo que es más importante, los índices de almacén de columnas agregan sobrecarga para las instrucciones **DELETE** y **UPDATE**. 

Para algunas cargas de trabajo híbridas transaccionales y analíticas, la sobrecarga de una carga de trabajo transaccional supera las ventajas que se obtienen al usar índices de almacén de columnas. Estos escenarios se pueden beneficiar del uso de CPU mejorado mediante el procesamiento de modo por lotes por sí solo. Por esa razón la característica de modo por lotes en almacén de filas considera el modo por lotes para todas las consultas, independientemente del tipo de índices implicados.

### <a name="workloads-that-might-benefit-from-batch-mode-on-rowstore"></a>Cargas de trabajo que pueden beneficiarse del modo por lotes en el almacén de filas
Las siguientes cargas de trabajo pueden beneficiarse del modo por lotes en el almacén de filas:
* una parte significativa de la carga de trabajo consta de consultas analíticas. Normalmente, estas consultas usan operadores como combinaciones o agregados que procesan cientos de miles de filas o más.
* La carga de trabajo está enlazada a la CPU. Si el cuello de botella es de E/S, se sigue recomendando tener en cuenta un índice de almacén de columnas, siempre que sea posible.
* La creación de un índice de almacén de columnas agrega demasiada sobrecarga al elemento transaccional de su carga de trabajo. O bien, la creación de un índice de almacén de columnas no es factible porque la aplicación depende de una característica que todavía no es compatible con los índices de almacén de columnas.


> [!NOTE]
> El modo por lotes en el almacén de filas solo sirve para reducir el consumo de CPU. Si el cuello de botella está relacionado con la E/S y los datos todavía no están almacenados en caché (caché "en frío"), el modo por lotes en el almacén de filas no mejorará el tiempo transcurrido de la consulta. De forma similar, si no hay memoria suficiente en el equipo para almacenar en caché todos los datos, es poco probable que mejore el rendimiento.

### <a name="what-changes-with-batch-mode-on-rowstore"></a>¿Qué cambios se producirán con el modo por lotes en el almacén de filas?

Establezca la base de datos en el nivel de compatibilidad 150. No se requiere ningún cambio más.

Incluso si una consulta no accede a ninguna tabla con índice de almacén de columnas, el procesador de consultas, mediante la heurística, decidirá si se va a considerar el modo por lotes. La heurística consiste en estas comprobaciones:
1. Una comprobación inicial de tamaños de tablas, operadores utilizados y cardinalidades estimadas en la consulta de entrada.
2. Puntos de control adicionales, a medida que el optimizador detecta planes nuevos y más baratos para la consulta. Si estos planes alternativos no hacen un uso considerable del modo por lotes, el optimizador dejará explorar alternativas al modo por lotes.

Si se usa el modo por lotes en el almacén de filas, verá el modo de ejecución real como **modo por lotes** en el plan de consulta. El operador de examen usa el modo por lotes para montones en disco e índices de árbol B. Esta exploración del modo por lotes puede evaluar los filtros de mapa de bits del modo por lotes. También podría ver otros operadores del modo por lotes en el plan. Entre los ejemplos se incluyen combinaciones hash, agregados basados en hash, ordenaciones, agregados de ventana, filtros, concatenación y operadores Compute Scalar.

### <a name="remarks"></a>Observaciones

Los planes de consulta no siempre usan el modo por lotes. Es posible que el optimizador de consultas decida que el modo por lotes no es beneficioso para la consulta. 

El espacio de búsqueda del optimizador de consultas está cambiando. Así pues, si obtiene un plan de modo de fila, podría no ser igual al plan obtenido en un nivel de compatibilidad más bajo. Y, si obtiene un plan de modo por lotes, podría no ser igual al plan obtenido con un índice de almacén de columnas. 

Los planes también pueden cambiar para las consultas que combinan los índices de almacén de columnas y de almacén de filas como consecuencia de una nueva exploración del almacén de filas en modo por lotes.

Existen limitaciones actuales para el nuevo examen de modo por lotes en el almacén de filas: 
- no se iniciará para tablas OLTP en memoria, ni para los índices que no sean de montones en disco y árboles B. 
- Tampoco se iniciará si se captura o se filtra una columna de objetos de gran tamaño (LOB). Esta limitación incluye conjuntos de columnas dispersas y columnas XML.

Hay consultas para las que no se usa el modo por lotes incluso con índices de almacén de columnas. Entre los ejemplos se incluyen consultas que implican cursores. Estas mismas exclusiones también se amplían al modo por lotes en el almacén de filas.

### <a name="configure-batch-mode-on-rowstore"></a>Configuración del modo por lotes en el almacén de filas
La configuración de ámbito de base de datos **BATCH_MODE_ON_ROWSTORE** está activada de forma predeterminada. Deshabilita el modo por lotes en el almacén de filas sin necesidad de un cambio en el nivel de compatibilidad de la base de datos:

```sql
-- Disabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = OFF;

-- Enabling batch mode on rowstore
ALTER DATABASE SCOPED CONFIGURATION SET BATCH_MODE_ON_ROWSTORE = ON;
```

puede deshabilitar el modo por lotes en el almacén de filas a través de la configuración de ámbito de base de datos. Pero aún puede invalidar la configuración en el nivel de consulta con la sugerencia de consulta **ALLOW_BATCH_MODE**. El ejemplo siguiente habilita el modo por lotes en el almacén de filas incluso con la característica deshabilitada a través de la configuración de ámbito de base de datos:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('ALLOW_BATCH_MODE'));
```

También puede deshabilitar el modo por lotes en el almacén de filas para una consulta específica mediante el uso de la sugerencia de consulta **DISALLOW_BATCH_MODE**. Vea el ejemplo siguiente:

```sql
SELECT [Tax Rate], [Lineage Key], [Salesperson Key], SUM(Quantity) AS SUM_QTY, SUM([Unit Price]) AS SUM_BASE_PRICE, COUNT(*) AS COUNT_ORDER
FROM Fact.OrderHistoryExtended
WHERE [Order Date Key]<=DATEADD(dd, -73, '2015-11-13')
GROUP BY [Tax Rate], [Lineage Key], [Salesperson Key]
ORDER BY [Tax Rate], [Lineage Key], [Salesperson Key]
OPTION(RECOMPILE, USE HINT('DISALLOW_BATCH_MODE'));
```

## <a name="see-also"></a>Consulte también
[Performance Center for SQL Server Database Engine and Azure SQL Database](../../relational-databases/performance/performance-center-for-sql-server-database-engine-and-azure-sql-database.md)    (Centro de rendimiento para el motor de base de datos SQL Server y Azure SQL Database)  
[Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md)    
[Referencia de operadores lógicos y físicos del plan de presentación](../../relational-databases/showplan-logical-and-physical-operators-reference.md)    
[Combinaciones](../../relational-databases/performance/joins.md)       
[Demostración del procesamiento de consultas inteligentes](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/intelligent-query-processing)   
