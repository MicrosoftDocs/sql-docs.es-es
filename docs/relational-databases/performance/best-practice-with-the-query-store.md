---
title: Procedimientos recomendados con el almacén de consultas | Microsoft Docs
description: Obtenga información sobre las prácticas recomendadas para utilizar el Almacén de consultas de SQL Server con su carga de trabajo, como usar las versiones más recientes de SQL Server Management Studio e Información de rendimiento de consultas.
ms.custom: ''
ms.date: 1/7/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store, best practices
ms.assetid: 5b13b5ac-1e4c-45e7-bda7-ebebe2784551
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83985f8cb34b9efa8485f9827dd9692a38f91a65
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100344797"
---
# <a name="best-practices-with-query-store"></a>Procedimientos recomendados con el almacén de consultas

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

En este artículo se describen los procedimientos recomendados para usar el almacén de consultas de SQL Server con la carga de trabajo.

## <a name="use-the-latest-ssmanstudiofull"></a><a name="SSMS"></a> Utilice la versión más reciente de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]

[!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] tiene un conjunto de interfaces de usuario diseñadas para configurar el almacén de consultas y para consumir datos recopilados sobre la carga de trabajo. Descargue la última versión de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] [aquí](../../ssms/download-sql-server-management-studio-ssms.md).

Para obtener una descripción rápida sobre cómo usar el almacén de consultas en escenarios de solución de problemas, vea los [blogs de @Azure del Almacén de consultas](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

## <a name="use-query-performance-insight-in-azure-sql-database"></a><a name="Insight"></a> Uso de Información de rendimiento de consultas en Azure SQL Database

Si ejecuta Almacén de consultas en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede usar [Información de rendimiento de consultas](/azure/sql-database/sql-database-query-performance) para analizar el consumo de recursos a lo largo del tiempo. Aunque se puede usar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] y [Azure Data Studio](../../azure-data-studio/what-is-azure-data-studio.md) para obtener el consumo de recursos detallado para todas las consultas (CPU, memoria y E/S) Información de rendimiento de consultas ofrece una forma rápida y eficaz de determinar su impacto en el consumo global de DTU de la base de datos. Para obtener más información, vea [Información de rendimiento de consultas de Base de datos SQL de Azure](/azure/azure-sql/database/query-performance-insight-use).

En esta sección se describen los valores predeterminados de configuración óptimos que están diseñados para garantizar el funcionamiento confiable del Almacén de consultas y de las características que dependen de él. La configuración predeterminada está optimizada para una recopilación continua de los datos, es decir, un tiempo mínimo en los estados OFF y READ_ONLY. Para obtener más información sobre todas las opciones disponibles del Almacén de consultas, vea [Opciones de ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store).

| Configuración | Descripción | Valor predeterminado | Comentario |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Especifica el límite del espacio de datos que puede tomar el Almacén de consultas dentro de la base de datos de cliente. |100 |Se aplica a nuevas bases de datos. |
| INTERVAL_LENGTH_MINUTES |Define el tamaño de la ventana de tiempo durante la que se agregan y conservan las estadísticas recopiladas en tiempo de ejecución para los planes de consulta. Todos los planes de consulta activa tienen al menos una fila durante un período de tiempo definido con esta configuración. |60 |Se aplica a nuevas bases de datos. |
| STALE_QUERY_THRESHOLD_DAYS |Directiva de limpieza basada en el tiempo que controla el período de retención de las estadísticas en tiempo de ejecución guardadas y las consultas inactivas. |30 |Se aplica a nuevas bases de datos y bases de datos con la configuración predeterminada anterior (367). |
| SIZE_BASED_CLEANUP_MODE |Especifica si limpieza automática de los datos se lleva a cabo cuando el tamaño de los datos del Almacén de consultas se aproxima al límite. |AUTO |Se aplica a todas las bases de datos. |
| QUERY_CAPTURE_MODE |Especifica si se realiza el seguimiento de todas las consultas o solo de un subconjunto de estas. |AUTO |Se aplica a todas las bases de datos. |
| FLUSH_INTERVAL_SECONDS |Especifica el período máximo durante el que las estadísticas en tiempo de ejecución capturadas se conservan en memoria, antes de vaciarlas en el disco. |900 |Se aplica a nuevas bases de datos. |
| | | | |

> [!IMPORTANT]
> Estos valores predeterminados se aplican automáticamente en la fase final de la activación del Almacén de consultas en todo [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Una vez habilitado, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] no cambia los valores de configuración establecidos por los clientes, a no ser que influyan de manera negativa en la carga de trabajo principal o en las operaciones de confianza del Almacén de consultas.

> [!NOTE]  
> El Almacén de consultas no se puede deshabilitar en la base de datos única de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ni Grupo elástico. Al ejecutar `ALTER DATABASE [database] SET QUERY_STORE = OFF` se devuelve la advertencia `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

Si quiere permanecer con su configuración personalizada, utilice [ALTER DATABASE con las opciones del Almacén de consultas](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store) para revertir la configuración al estado anterior. Vea [Procedimientos recomendados con el almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md) para aprender a elegir los parámetros de configuración óptimos.

## <a name="use-query-store-with-elastic-pool-databases"></a>Uso del Almacén de consultas con bases de datos de Grupo elástico

Puede usar el Almacén de consultas en todas las bases de datos sin problemas, incluso en grupos densamente empaquetados. Se han solucionado todas las incidencias relacionadas con el uso excesivo de los recursos que es posible que hayan surgido cuando el almacén de consultas estaba habilitado para el gran número de bases de datos en los grupos elásticos.

## <a name="keep-query-store-adjusted-to-your-workload"></a><a name="Configure"></a>Mantener el Almacén de consultas ajustado a la carga de trabajo

Configure el Almacén de consultas en función de la carga de trabajo y los requisitos de solución de problemas de rendimiento.
Los parámetros predeterminados son suficientemente buenos como punto de partida, pero debe supervisar el comportamiento del almacén de consultas en el tiempo y ajustar su configuración en consecuencia.

 ![Propiedades del almacén de consultas](../../relational-databases/performance/media/query-store-properties.png "query-store-properties")

 A continuación se indican algunas instrucciones para establecer valores de parámetro:

**Tamaño máximo (MB)** : especifica el límite del espacio de datos que el almacén de consultas toma dentro de la base de datos. Es el valor de configuración más importante que afecta directamente al modo de operación del almacén de consultas.

Mientras que el almacén de consultas recopila consultas, planes de ejecución y estadísticas, su tamaño en la base de datos crece hasta que se alcanza este límite. Cuando esto ocurre, el Almacén de consultas cambia automáticamente el modo de operación a solo lectura y deja de recopilar datos nuevos, lo que significa que el análisis de rendimiento ya no es preciso.

El valor predeterminado en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] es 100 MB. Es posible que este tamaño no sea suficiente si la carga de trabajo genera gran cantidad de planes y consultas diferentes, o bien si quiere conservar el historial de consultas durante un período de tiempo más largo. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el valor predeterminado es de 1 GB. Realice el seguimiento del uso de espacio actual y aumente el valor de **Tamaño máximo (MB)** para impedir que el almacén de consultas cambie al modo de solo lectura.

> [!IMPORTANT]
> El límite **Tamaño máximo (MB)** no se aplica de forma estricta. El tamaño de almacenamiento solo se comprueba cuando el almacén de consultas escribe datos en el disco. Este intervalo lo establece el valor de **Intervalo de vaciado de datos (minutos)** . Si Almacén de consultas ha infringido el límite de tamaño máximo entre las comprobaciones de tamaño de almacenamiento, pasa al modo de solo lectura. Si **Modo de limpieza basada en tamaño** está habilitado, también se desencadena el mecanismo de limpieza para aplicar el límite de tamaño máximo.

Utilice [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o ejecute el siguiente script para obtener la información más reciente sobre el tamaño del Almacén de consultas:

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
 max_storage_size_mb, readonly_reason
FROM sys.database_query_store_options;
```

En el script siguiente se establece un nuevo valor para **Tamaño de máximo (MB)** :

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = 1024);
```

 **Intervalo del vaciado de datos (minutos)** : define la frecuencia para conservar en disco las estadísticas en tiempo de ejecución recopiladas. Se expresa en minutos en la interfaz gráfica de usuario (GUI), pero en [!INCLUDE[tsql](../../includes/tsql-md.md)] se expresa en segundos. El valor predeterminado es 900 segundos, equivalente a 15 minutos en la interfaz gráfica de usuario. Considere la posibilidad de usar un valor más alto si la carga de trabajo no genera gran cantidad de consultas y planes diferentes, o bien si puede soportar más tiempo de conservación de los datos antes de cerrar la base de datos.

> [!NOTE]
> El uso de la marca de seguimiento 7745 impide que los datos del almacén de consultas se escriban en el disco en el caso de un comando de conmutación por error o apagado. Para obtener más información, vea la sección [Uso de marcas de seguimiento en servidores críticos](#Recovery).

Use [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] para establecer otro valor para **Intervalo de vaciado de datos**:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS = 900);
```

**Intervalo de la recopilación de estadísticas**: define el nivel de granularidad de la estadística en tiempo de ejecución recopilada y se expresa en minutos. El valor predeterminado es 60 minutos. Considere la posibilidad de usar un valor más bajo si necesita una granularidad más precisa o menos tiempo para detectar y mitigar las incidencias. Recuerde que el valor afecta directamente al tamaño de los datos del almacén de consultas. Use [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)] para establecer otro valor para **Intervalo de recopilación de estadísticas**:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 60);
```

**Umbral de consultas obsoletas (días)** : directiva de limpieza basada en el tiempo que controla el período de retención de las estadísticas en tiempo de ejecución persistentes y las consultas inactivas; se expresa en días. De forma predeterminada, el almacén de consultas se configura para conservar los datos durante 30 días, que podría ser un período innecesariamente largo para su escenario.

Evite mantener datos históricos que no tenga pensado usar. Este procedimiento reduce los cambios al estado de solo lectura. El tamaño de los datos del almacén de consultas y el tiempo para detectar y mitigar la incidencia serán más predecibles. Use [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o el siguiente script para configurar la directiva de limpieza basada en el tiempo:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 90));
```

**Modo de limpieza basado en el tamaño**: especifica si la limpieza automática de los datos se lleva a cabo cuando el tamaño de datos del almacén de consultas se aproxime al límite. Active la limpieza basada en el tamaño para asegurarse de que el almacén de consultas siempre se ejecuta en modo de lectura y escritura, y recopila los datos más recientes.  Tenga en cuenta que no hay ninguna garantía en las cargas de trabajo pesadas de que la limpieza de Almacén de consultas mantenga el tamaño de los datos en el límite. Es posible que la limpieza automática de datos quede fuera y cambie (temporalmente) al modo de solo lectura.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (SIZE_BASED_CLEANUP_MODE = AUTO);
```

**Modo de captura de Almacén de consultas**: especifica la directiva de captura de consultas para el almacén de consultas.

- **Todos**: Captura todas las consultas. Es la opción predeterminada en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].
- **Automático**: se omiten las consultas poco frecuentes y aquellas con una duración de compilación y ejecución insignificante. Los umbrales para la duración del tiempo de ejecución, compilación y recuento de ejecuciones se determinan de forma interna. A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], esta es la opción predeterminada.
- **Ninguna**: el almacén de consultas deja de capturar consultas nuevas.
- **Personalizado**: permite un control adicional y la capacidad de ajustar la directiva de recopilación de datos. La nueva configuración personalizada define lo que sucede durante el umbral de tiempo de la directiva de captura interna. Es un límite de tiempo durante el que se evalúan las condiciones configurables y, si alguna de ellas es verdadera, la consulta se puede registrar en el almacén de consultas.

> [!IMPORTANT]
> Los cursores, las consultas dentro de procedimientos almacenados y las consultas compiladas de forma nativa siempre se capturan cuando Modo de captura de Almacén de consultas se establece en **Todo**, **Automático** o **Personalizado**. Para capturar consultas compiladas de forma nativa, habilite la recopilación de estadísticas por consulta mediante [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

 En el script siguiente se establece QUERY_CAPTURE_MODE en AUTO:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (QUERY_CAPTURE_MODE = AUTO);
```

### <a name="examples"></a>Ejemplos

En el ejemplo siguiente, se establece QUERY_CAPTURE_MODE en AUTO y se configuran otras opciones recomendadas en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

En el ejemplo siguiente, se establece QUERY_CAPTURE_MODE en AUTO y se configuran otras opciones recomendadas en [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] para incluir estadísticas de espera:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON
    );
```

En el ejemplo siguiente, se establece QUERY_CAPTURE_MODE en AUTO y se configuran otras opciones recomendadas en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. *De manera opcional*, se establece la directiva de captura CUSTOM con los valores predeterminados, en lugar del nuevo modo de captura AUTO predeterminado:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1000,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
      QUERY_CAPTURE_MODE = CUSTOM,
      QUERY_CAPTURE_POLICY = (
        STALE_CAPTURE_POLICY_THRESHOLD = 24 HOURS,
        EXECUTION_COUNT = 30,
        TOTAL_COMPILE_CPU_TIME_MS = 1000,
        TOTAL_EXECUTION_CPU_TIME_MS = 100
      )
    );
```

## <a name="start-with-query-performance-troubleshooting"></a>Inicio de la solución de problemas de rendimiento de consultas

El flujo de trabajo de solución de problemas con el almacén de consultas es sencillo, como se muestra en el diagrama siguiente:

![Solución de problemas del almacén de consultas](../../relational-databases/performance/media/query-store-troubleshooting.png "query-store-troubleshooting")

Habilite el almacén de consultas mediante [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] como se ha descrito en la sección anterior, o bien ejecute la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] siguiente:

```sql
ALTER DATABASE [DatabaseOne] SET QUERY_STORE = ON;
```

El almacén de consultas tarda algún tiempo en recopilar el conjunto de datos que representa con precisión la carga de trabajo. Normalmente, un día es suficiente incluso para cargas de trabajo muy complejas. Pero puede empezar a explorar los datos e identificar las consultas que requieran atención inmediatamente después de habilitar la característica. Vaya a la subcarpeta Almacén de consultas del nodo de la base de datos en el Explorador de objetos de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para abrir las vistas de solución de problemas de escenarios concretos.

El Almacén de consultas de[!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] funciona con el conjunto de métricas de ejecución, cada una expresada como cualquiera de las siguientes funciones estadísticas:

|Versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Métrica de ejecución|Función estadística|
|----------------------|----------------------|------------------------|
|[!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]|Tiempo de CPU, Duración, Recuento de ejecuciones, Lecturas lógicas, Escrituras lógicas, Consumo de memoria, Lecturas físicas, Tiempo de CLR, Grado de paralelismo (DOP) y Recuento de filas|Promedio, máximo, mínimo, desviación estándar y total|
|[!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]|Tiempo de CPU, Duración, Recuento de ejecuciones, Lecturas lógicas, Escrituras lógicas, Consumo de memoria, Lecturas físicas, Tiempo de CLR, Grado de paralelismo, Recuento de filas, Memoria de registro, Memoria de TempDB y Tiempos de espera|Promedio, máximo, mínimo, desviación estándar y total|

En el gráfico siguiente se muestra cómo localizar vistas del Almacén de consultas:

![Vistas del almacén de consultas](../../relational-databases/performance/media/objectexplorerquerystore_sql17.png "Vistas del almacén de consultas")

En la siguiente tabla se explica cuándo usar cada una de las vistas del Almacén de consultas:

|Vista de SQL Server Management Studio|Escenario|
|---------------|--------------|
|**Consultas devueltas**|Localice consultas para las que las métricas de ejecución se han devuelto recientemente (es decir, han cambiado a peor). <br />Use esta vista para poner en correlación los problemas de rendimiento observados en la aplicación con las consultas reales que se deben corregir o mejorar.|
|**Consumo total de recursos**|Analice el consumo total de recursos para la base de datos para cualquiera de las métricas de ejecución.<br />Use esta vista para identificar patrones de recursos (cargas de trabajo por el día frente a cargas de trabajo por la noche) y optimizar el consumo total para la base de datos.|
|**Consultas que consumen más recursos**|Elija una métrica de ejecución de interés e identifique las consultas que tenían los valores más extremos para un intervalo de tiempo proporcionado. <br />Use esta vista para centrar la atención en las consultas más importantes que tienen el mayor impacto en el consumo de recursos de base de datos.|
|**Consultas con planes forzados**|Enumera los planes forzados anteriormente mediante el Almacén de consultas. <br />Use esta vista para obtener acceso rápidamente a todos los planes forzados actualmente.|
|**Consultas con gran variación**|Analice consultas con una gran variación de ejecución en lo referente a cualquiera de las dimensiones disponibles, como la duración, el tiempo de CPU, la E/S y el uso de memoria, en el intervalo de tiempo deseado.<br />Use esta vista para identificar consultas con un rendimiento muy variable que puedan afectar a la experiencia del usuario en las aplicaciones.|
|**Estadísticas de espera de consulta**|Analice las categorías de espera más activas de una base de datos y qué consultas contribuyen más a la categoría de espera seleccionada.<br />Use esta vista para analizar las estadísticas de espera e identificar las consultas que puedan afectar a la experiencia del usuario en las aplicaciones.<br /><br />Se aplica a: A partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18.0 y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Consultas con seguimiento**|Realice un seguimiento de la ejecución de las consultas más importantes en tiempo real. Normalmente, esta vista se utiliza cuando tiene consultas con planes forzados y desea asegurarse de que el rendimiento de las mismas es estable.|

> [!TIP]
> Para obtener una descripción detallada sobre cómo usar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para identificar las consultas que consumen más recursos y corregir aquellas devueltas debido al cambio de una opción de plan, vea los [blogs de @Azure sobre el almacén de consultas](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database/).

Cuando identifique una consulta con un rendimiento deficiente, la acción depende de la naturaleza del problema.

- Si la consulta se ha ejecutado con varios planes y el último plan es mucho peor que el anterior, puede usar el mecanismo que fuerza el plan. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta forzar el plan en el optimizador. Si se produce un error al exigir el plan, se producirá un evento XEvent y el optimizador realizará su trabajo de forma normal.

  ![Forzar plan del almacén de consultas](../../relational-databases/performance/media/query-store-force-plan.png "query-store-force-plan")

  > [!NOTE]
  > Es posible que en el gráfico anterior se presenten otras formas para planes de consulta específicos, con los significados siguientes para cada estado posible:<br />
  >
  > |Forma|Significado|
  > |-------------------|-------------|
  > |Circle|Consulta completada, lo que significa que una ejecución normal ha finalizado correctamente.|
  > |Square|Cancelada, lo que significa una ejecución iniciada por el cliente anulada.|
  > |Triangle|Con error, lo que significa que una excepción ha anulado la ejecución.|
  >
  > Además, el tamaño de la forma refleja el recuento de ejecución de consultas dentro del intervalo de tiempo especificado. El tamaño aumenta con un número mayor de ejecuciones.

- Se podría concluir que a la consulta le falta un índice para que la ejecución sea óptima. Esta información aparece en el plan de ejecución de la consulta. Cree el índice que falta y compruebe el rendimiento de la consulta mediante el almacén de consultas.

   ![Mostrar plan del almacén de consultas](../../relational-databases/performance/media/query-store-show-plan.png "query-store-show-plan")

Si ejecuta la carga de trabajo en [!INCLUDE[ssSDS](../../includes/sssds-md.md)], suscríbase al Asesor de índices de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] para recibir automáticamente las recomendaciones de índices.

- En algunos casos, podría exigir la recompilación estadística si ve que la diferencia entre el número de filas estimado y el real en el plan de ejecución es significativa.
- Vuelva a escribir las consultas con problemas, por ejemplo, para aprovechar las ventajas de la parametrización de la consulta o implementar lógica más óptima.

## <a name="verify-that-query-store-collects-query-data-continuously"></a><a name="Verify"></a> Comprobación de que el almacén de consultas recopila datos de consulta de forma continua

El almacén de consultas puede cambiar el modo de operación automáticamente. Supervise periódicamente el estado del almacén de consulta para asegurarse de que funciona y tomar medidas para evitar errores debido a causas evitables. Ejecute la siguiente consulta para determinar el modo de operación y ver los parámetros más importantes:

```sql
USE [QueryStoreDB];
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

La diferencia entre `actual_state_desc` y `desired_state_desc` indica que se ha producido un cambio de modo de operación automáticamente. El cambio más frecuente es que el almacén de consultas cambie al modo de solo lectura automáticamente. En circunstancias extremadamente raras, el almacén de consultas puede terminar en el estado ERROR debido a errores internos.

Cuando el estado real es de solo lectura, use la columna **readonly_reason** para determinar la causa raíz. Normalmente, encontrará que el almacén de consultas ha cambiado al modo de solo lectura porque se ha superado la cuota de tamaño. En ese caso, **readonly_reason** se establece en 65536. Por otros motivos, vea [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).

Tenga en cuenta los pasos siguientes para cambiar el Almacén de consultas al modo de lectura y escritura y activar la recopilación de datos:

- Aumente el tamaño de almacenamiento máximo mediante la opción **MAX_STORAGE_SIZE_MB** de **ALTER DATABASE**.
- Limpie los datos del Almacén de consultas mediante la siguiente instrucción:

  ```sql
  ALTER DATABASE [QueryStoreDB] SET QUERY_STORE CLEAR;
  ```

Puede aplicar uno de estos pasos o los dos si ejecuta la instrucción siguiente que vuelve a cambiar de forma explícita el modo de operación a lectura y escritura:

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
```

Siga estos pasos para ser proactivo:

- Puede evitar cambios automáticos de modo de operación aplicando procedimientos recomendados. Asegúrese de que el tamaño del almacén de consultas es siempre menor que el valor máximo permitido para reducir considerablemente la posibilidad de cambiar al modo de solo lectura. Active la directiva basada en el tamaño como se describe en la sección [Configuración del almacén de consultas](#Configure) para que el almacén de consultas limpie automáticamente los datos cuando el tamaño se aproxime al límite.
- Para asegurarse de que se retienen los datos más recientes, configure la directiva basada en tiempo para quitar información obsoleta frecuentemente.
- Por último, considere la posibilidad de establecer **Modo de captura de Almacén de consultas** en **Automático** ya que filtra las consultas que suelen ser menos relevantes para la carga de trabajo.

### <a name="error-state"></a>Estado ERROR

Para recuperar el almacén de consultas, intente establecer de forma explícita el modo de lectura y escritura, y vuelva a comprobar el estado real.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

Si el problema continúa, significa que los datos del almacén de consultas siguen dañados en el disco.

A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], el Almacén de consultas se puede recuperar si se ejecuta el procedimiento **sp_query_store_consistency_check** almacenado en la base de datos afectada. El almacén de consultas se debe deshabilitar antes de intentar la operación de recuperación. Para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], tendrá que borrar los datos del almacén de consultas, como se muestra a continuación.

Si la recuperación no se ha realizado correctamente, puede intentar borrar el almacén de consultas antes de establecer el modo de lectura y escritura.

```sql
ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE CLEAR;
GO

ALTER DATABASE [QueryStoreDB]
SET QUERY_STORE (OPERATION_MODE = READ_WRITE);
GO

SELECT actual_state_desc, desired_state_desc, current_storage_size_mb,
    max_storage_size_mb, readonly_reason, interval_length_minutes,
    stale_query_threshold_days, size_based_cleanup_mode_desc,
    query_capture_mode_desc
FROM sys.database_query_store_options;
```

## <a name="set-the-optimal-query-store-capture-mode"></a>Establecimiento del modo óptimo de captura del almacén de consultas

Mantenga los datos más relevantes en el Almacén de consultas. En la tabla siguiente se describen los escenarios típicos para cada modo de captura del almacén de consultas:

|Modo de captura del almacén de consultas|Escenario|
|------------------------|--------------|
|**Todo**|Analice la carga de trabajo exhaustivamente en cuanto a todas las formas de las consultas y sus frecuencias de ejecución, y otras estadísticas.<br /><br /> Identifique nuevas consultas en la carga de trabajo.<br /><br /> Detecte si las consultas ad-hoc se usan para identificar oportunidades de parametrización automática o manual.<br /><br />Nota: Este es el modo de captura predeterminado en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].|
|**Automático**|Centre su atención en las consultas pertinentes y accionables. Un ejemplo son las que se ejecutan con regularidad o las que tienen un consumo significativo de recursos.<br /><br />Nota: A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], este es el modo de captura predeterminado.|
|**None**|Ya ha capturado el conjunto de consultas que quiere supervisar en tiempo de ejecución y quiere eliminar los objetos innecesarios que otras consultas podrían introducir.<br /><br /> El modo Ninguno es adecuado para entornos de pruebas y evaluación comparativa.<br /><br /> El modo Ninguno también es adecuado para los proveedores de software que incluyen la configuración del Almacén de consultas definida para supervisar la carga de trabajo de la aplicación.<br /><br /> El modo Ninguno se debe usar con precaución, ya que podría perder la oportunidad de realizar el seguimiento de consultas nuevas importantes y de optimizarlas. Evite el uso del modo Ninguno a menos que tenga un escenario específico que lo requiera.|
|**Personalizada**|[!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] introduce un modo de captura Personalizado en el comando `ALTER DATABASE SET QUERY_STORE`. Cuando se habilita, una nueva configuración de la directiva de captura del almacén de consultas incluye más configuraciones del almacén de consultas para ajustar la recopilación de datos en un servidor específico.<br /><br />La nueva configuración personalizada define lo que sucede durante el umbral de tiempo de la directiva de captura interna. Es un límite de tiempo durante el que se evalúan las condiciones configurables y, si alguna de ellas es verdadera, la consulta se puede registrar en el almacén de consultas. Para obtener más información, vea [ALTER DATABASE SET Options &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).|

> [!NOTE]
> Los cursores, las consultas dentro de procedimientos almacenados y las consultas compiladas de forma nativa siempre se capturan cuando Modo de captura de Almacén de consultas se establece en **Todo**, **Automático** o **Personalizado**. Para capturar consultas compiladas de forma nativa, habilite la recopilación de estadísticas por consulta mediante [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

## <a name="keep-the-most-relevant-data-in-query-store"></a>Conservación de los datos más relevantes en el Almacén de consultas

Configure el almacén de consultas para que contenga solo los datos pertinentes de modo que se ejecute de forma continua y proporcione una magnífica experiencia de solución de problemas con un impacto mínimo en la carga de trabajo normal.
La tabla siguiente proporciona prácticas recomendadas:

|Procedimiento recomendado|Configuración|
|-------------------|-------------|
|Limitar los datos históricos retenidos.|Configurar la directiva basada en tiempo para activar la limpieza automática.|
|Filtrar las consultas no pertinentes.|Configurar **Modo de captura de Almacén de consultas** en **Automático**.|
|Eliminar las consultas menos relevantes cuando se alcanza el tamaño máximo.|Activar la directiva de limpieza basada en el tamaño.|

## <a name="avoid-using-non-parameterized-queries"></a><a name="Parameterize"></a> Evitar el uso de consultas sin parámetros

El uso de consultas sin parámetros cuando no es necesario no es un procedimiento recomendado. Un ejemplo es el caso del análisis ad hoc. Los planes en caché no se pueden reutilizar, lo que obliga al optimizador de consultas a compilar consultas para cada texto de consulta única. Para más información, vea [Instrucciones para usar la parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParamGuide).

Además, el almacén de consultas puede superar rápidamente la cuota de tamaño debido a la posibilidad de un gran número de textos de consulta diferentes y, por tanto, un gran número de planes de ejecución distintos con forma similar. Como resultado, el rendimiento de la carga de trabajo será deficiente y el almacén de consultas podría cambiar al modo de solo lectura o eliminar datos constantemente en un intente de mantenerse al día con las consultas entrantes.

Considere las opciones siguientes:

- Parametrice las consultas cuando proceda. Por ejemplo, encapsule las consultas dentro de un procedimiento almacenado o sp_executesql. Para más información, vea [Parámetros y reutilización de un plan de ejecución](../../relational-databases/query-processing-architecture-guide.md#PlanReuse).
- Use la opción [Optimizar para cargas de trabajo ad hoc](../../database-engine/configure-windows/optimize-for-ad-hoc-workloads-server-configuration-option.md) si la carga de trabajo contiene muchos lotes ad hoc de un solo uso con distintos planes de consulta.
  - Compare el número de valores query_hash distintos con el número total de entradas en sys.query_store_query. Si la relación es cercana a 1, la carga de trabajo ad hoc genera consultas diferentes.
- Aplique la [parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParam) para la base de datos o para un subconjunto de consultas si el número de planes de consulta diferentes no es grande.
  - Use una [guía de plan](../../relational-databases/performance/specify-query-parameterization-behavior-by-using-plan-guides.md) para forzar la parametrización solo para la consulta seleccionada.
  - Configure la parametrización forzada mediante el comando [opción de base de datos de parametrización](../../relational-databases/databases/database-properties-options-page.md#miscellaneous) si hay un pequeño número de planes de consulta diferentes en la carga de trabajo. Un ejemplo es cuando la relación entre el recuento de valores query_hash distintos y el número total de entradas de sys.query_store_query es mucho menor que 1.
- Establezca QUERY_CAPTURE_MODE en AUTO para filtrar de forma automática las consultas ad hoc con un consumo de recursos reducido.

## <a name="avoid-a-drop-and-create-pattern-for-containing-objects"></a><a name="Drop"></a> Evitar un patrón DROP y CREATE para objetos contenedores

El almacén de consultas asocia la entrada de consulta a un objeto contenedor, como un procedimiento almacenado, una función y un desencadenador. Cuando se vuelve a crear un objeto contenedor, se genera una nueva entrada de consulta para el mismo texto de consulta. Esto impide realizar el seguimiento de las estadísticas de rendimiento para esa consulta a lo largo del tiempo y usar un mecanismo para forzar el plan. Para evitar esta situación, use el proceso `ALTER <object>` para cambiar la definición de un objeto contenedor siempre que sea posible.

## <a name="check-the-status-of-forced-plans-regularly"></a><a name="CheckForced"></a> Comprobación periódica del estado de los planes forzados

Forzar el plan es un mecanismo conveniente para corregir el rendimiento de las consultas críticas y hacer que sean más predecibles. Como sucede con las sugerencias de plan y las guías de plan, forzar un plan no es una garantía de que se va a usar en ejecuciones futuras. Normalmente, cuando se cambia el esquema de base de datos de forma que se modifican o se quitan objetos a los que hace referencia el plan de ejecución, al forzar el plan se empiezan a generar errores. En ese caso, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a la recompilación de consultas mientras el motivo real del error de la operación de forzado aparece en [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). La siguiente consulta devuelve información sobre planes forzados.

```sql
USE [QueryStoreDB];
GO

SELECT p.plan_id, p.query_id, q.object_id as containing_object_id,
    force_failure_count, last_force_failure_reason_desc
FROM sys.query_store_plan AS p
JOIN sys.query_store_query AS q on p.query_id = q.query_id
WHERE is_forced_plan = 1;
```

Para obtener una lista completa de los motivos, vea [sys.query_store_plan](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md). También puede usar el XEvent **query_store_plan_forcing_failed** para realizar un seguimiento de los errores forzados del plan y solucionarlos.

## <a name="avoid-renaming-databases-for-queries-with-forced-plans"></a><a name="Renaming"></a> Evitar el cambio de nombre de las bases de datos para las consultas con planes forzados

Los planes de ejecución hacen referencia a objetos mediante nombres de tres partes como `database.schema.object`.

Si cambia el nombre de una base de datos, al forzar el plan se produce un error que provoca la recompilación en todas las ejecuciones de consulta posteriores.

## <a name="using-query-store-in-mission-critical-servers"></a><a name="Recovery"></a> Uso de Almacén de consultas en servidores críticos

Las marcas de seguimiento globales 7745 y 7752 se pueden usar para mejorar la disponibilidad de las bases de datos mediante el almacén de consultas. Para más información, vea [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

- La marca de seguimiento 7745 evita el comportamiento predeterminado en el que el almacén de consultas escribe datos en el disco antes de que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se pueda apagar. Esto significa que los datos del almacén de consultas que se han recopilado, pero que todavía no han almacenado en el disco, se perderán, hasta la ventana de tiempo definida con `DATA_FLUSH_INTERVAL_SECONDS`.
- La marca de seguimiento 7752 permite la carga asincrónica del Almacén de consultas. Esto permite que una base de datos vuelva a estar en línea y que las consultas se ejecuten antes de que el almacén de consultas se haya recuperado completamente. El comportamiento predeterminado consiste en realizar la carga sincrónica del Almacén de consultas. Este comportamiento impide que se ejecuten las consultas antes de que el almacén de consultas se haya recuperado, pero también impide que se pierdan consultas en la colección de datos.

> [!NOTE]
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], este comportamiento se controla mediante el motor, y la marca de seguimiento 7752 no tiene ningún efecto.

> [!IMPORTANT]
> Si va a usar el Almacén de consultas para resultados de la carga de trabajo Just-In-Time en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], prevea la instalación de las mejoras de escalabilidad de rendimiento descritas en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP2 CU2 ([KB 4340759](https://support.microsoft.com/help/4340759)) lo antes posible. Sin estas mejoras, cuando la base de datos está sometida a cargas de trabajo intensas, puede producirse la contención de bloqueo por subproceso y el rendimiento del servidor puede resultar lento. En concreto, puede ver una contención intensa en el bloqueo por subproceso `QUERY_STORE_ASYNC_PERSIST` o `SPL_QUERY_STORE_STATS_COOKIE_CACHE`. Después de aplicar esta mejora, el Almacén de consultas ya no producirá la contención de bloqueo por subproceso.

> [!IMPORTANT]
> Si va a usar el Almacén de consultas para los resultados de la carga de trabajo Just-In-Time en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] a través de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), prevea la instalación de la mejora de escalabilidad de rendimiento descrita en SP2 CU15 de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], CU22 de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y CU8 de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] lo antes posible. Sin esta mejora, cuando la base de datos se encuentra sometida a cargas de trabajo ad hoc intensas, el Almacén de consultas puede usar una gran cantidad de memoria y el rendimiento del servidor puede ser lento. Después de aplicar esta mejora, el Almacén de consultas impone límites internos a la cantidad de memoria que pueden usar sus distintos componentes y puede cambiar automáticamente el modo de operación a solo lectura hasta que se devuelva suficiente memoria a [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Tenga en cuenta que los límites de memoria interna del Almacén de consultas no están documentados porque están sujetos a cambios.  


## <a name="using-query-store-in-azure-sql-database-active-geo-replication"></a><a name="geosyncreplicas"></a> Uso de Almacén de consultas en la replicación geográfica activa de Azure SQL Database

Almacén de consultas en una réplica de replicación geográfica activa secundaria de Azure SQL Database será una copia de solo lectura de la actividad en la réplica principal. 

Evite los niveles no coincidentes de las instancias de Azure SQL Database que participan en la replicación geográfica. Una base de datos secundaria debe tener el mismo tamaño de proceso que la base de datos principal o similar, y debe estar en el mismo nivel de servicio. Busque el tipo de espera HADR_THROTTLE_LOG_RATE_MISMATCHED_SLO en [sys.dm_db_wait_stats](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-wait-stats-azure-sql-database) que indica la limitación de velocidad del registro de transacciones en la réplica principal debido a un retraso secundario.

Para obtener más información sobre la estimación y configuración del tamaño de la base de datos de Azure SQL secundaria de replicación geográfica activa, vea [Configuración de la base de datos secundaria](/azure/azure-sql/database/active-geo-replication-overview#configuring-secondary-database).


## <a name="see-also"></a>Consulte también

- [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)
- [Vistas de catálogo del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Procedimientos almacenados del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Uso del almacén de consultas con OLTP en memoria](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Supervisión del rendimiento mediante el almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)
- [Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md)