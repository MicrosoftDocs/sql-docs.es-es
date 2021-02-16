---
title: Supervisión del rendimiento mediante el Almacén de consultas | Microsoft Docs
description: El Almacén de consultas de SQL Server proporciona datos detallados sobre el rendimiento y la elección del plan de consultas. El Almacén de consultas captura el historial de consultas, planes y estadísticas del entorno de ejecución.
ms.custom: ''
ms.date: 04/09/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Query Store
- Query Store, described
ms.assetid: e06344a4-22a5-4c67-b6c6-a7060deb5de6
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current||=azure-sqldw-latest
ms.openlocfilehash: c8a836f1a8daf2b4b6a9d5b76156191799013484
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341717"
---
# <a name="monitoring-performance-by-using-the-query-store"></a>Supervisión del rendimiento mediante el almacén de consultas

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

La característica del Almacén de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ofrece datos detallados sobre el rendimiento y la elección del plan de consultas. Esta característica simplifica la solución de problemas de rendimiento al permitirle encontrar rápidamente las diferencias de rendimiento provocadas por cambios en los planes de consulta. El Almacén de consultas captura automáticamente un historial de consultas, planes y estadísticas en tiempo de ejecución y las conserva para su revisión. Además, separa los datos por ventanas de tiempo, lo que permite ver patrones de uso de la base de datos y comprender cuándo se produjeron cambios del plan de consultas en el servidor. El almacén de consultas se puede configurar con la opción [ALTER DATABASE SET](../../t-sql/statements/alter-database-transact-sql-set-options.md) .

Para más información sobre el funcionamiento del Almacén de consultas de Azure [!INCLUDE[ssSDS](../../includes/sssds-md.md)], vea [Funcionamiento del almacén de consultas de Base de datos SQL de Azure](best-practice-with-the-query-store.md#Insight).

> [!IMPORTANT]
> Si usa el Almacén de consultas para conclusiones de la carga de trabajo just-in-time en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], tenga previsto instalar las correcciones de escalabilidad de rendimiento en [KB 4340759](https://support.microsoft.com/help/4340759) lo antes posible.

## <a name="enabling-the-query-store"></a><a name="Enabling"></a> Habilitar el Almacén de consultas

 El Almacén de consultas no está habilitado de forma predeterminada para las nuevas bases de datos de SQL Server y Azure Synapse Analytics y sí lo está para las nuevas bases de datos de Azure SQL Database.

### <a name="use-the-query-store-page-in-ssmanstudiofull"></a>Use la página del Almacén de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

1. En el Explorador de objetos, haga clic con el botón derecho en una base de datos y, luego, haga clic en **Propiedades**.

   > [!NOTE]
   > Se requiere como mínimo la versión 16 de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].

2. En el cuadro de diálogo **Propiedades de la base de datos** , seleccione la página **Almacén de consultas** .

3. En el cuadro **Modo de operación (solicitado)** seleccione **Lectura y escritura**.

### <a name="use-transact-sql-statements"></a>Uso de instrucciones Transact-SQL

Use la instrucción **ALTER DATABASE** para habilitar el almacén de consultas para una base de datos concreta. Por ejemplo:

```sql
SET QUERY_STORE = ON (OPERATION_MODE = READ_WRITE);
```

Para obtener más opciones de sintaxis relacionadas con el Almacén de consultas, vea [Opciones de ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).

> [!NOTE]
> No se puede habilitar el Almacén de consultas para las bases de datos **master** o **tempdb**.

> [!IMPORTANT]
> Para obtener información sobre cómo habilitar el Almacén de consultas y hacer que siga ajustándose a su carga de trabajo, consulte [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md#Configure).

## <a name="information-in-the-query-store"></a><a name="About"></a> Información del Almacén de consultas

Los planes de ejecución para cualquier consulta específica en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suelen evolucionar con el tiempo debido a una serie de motivos diferentes, como cambios en las estadísticas, cambios de esquema, creación o eliminación de los índices, etc. La caché de procedimientos (donde se almacenan los planes de consulta almacenados en caché) solo almacena el plan de ejecución más reciente. Los planes también se eliminan de la caché de planes debido a la presión de memoria. Como resultado, es posible que las regresiones de rendimiento de consultas provocadas por los cambios de planes de ejecución no sean triviales y que su resolución lleve mucho tiempo.

Como el Almacén de consultas conserva varios planes de ejecución por consulta, puede aplicar directivas para dirigir el procesador de consultas para que use un plan de ejecución concreto para una consulta. Esto se conoce como forzado de plan. El forzado de plan en la consulta de almacenes se ofrece mediante un mecanismo similar al de la sugerencia de consulta [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md) , pero no requiere ningún cambio en las aplicaciones de usuario. El plan de forzado puede resolver una regresión del rendimiento de consultas provocado por un cambio de plan en un período de tiempo muy breve.

> [!NOTE]
> El Almacén de consultas recopila los planes para las instrucciones DML, como SELECT, INSERT, UPDATE, DELETE, MERGE y BULK INSERT.
>
> De forma predeterminada, el Almacén de consultas no recopila datos para los procedimientos almacenados compilados de forma nativa. Use [sys.sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md) para habilitar la recopilación de datos para los procedimientos almacenados compilados de forma nativa.

Las **estadísticas de espera** son otra fuente de información que ayuda a solucionar problemas de rendimiento en [!INCLUDE[ssde_md](../../includes/ssde_md.md)]. Durante mucho tiempo, las estadísticas de espera solo han estado disponibles en el nivel de instancia, lo que dificultaba hacer un seguimiento hacia atrás de las esperas a una consulta específica. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], el Almacén de consultas incluye una dimensión que realiza un seguimiento de las estadísticas de espera. En el ejemplo siguiente se habilita el Almacén de consultas para recopilar estadísticas de espera.

```sql
SET QUERY_STORE = ON ( WAIT_STATS_CAPTURE_MODE = ON );
```

Entre los escenarios comunes para usar la característica Almacén de consultas se encuentran:

- Buscar y corregir rápidamente una regresión de rendimiento de plan forzando el plan de consulta anterior. Corregir las consultas de las que se ha realizado regresión recientemente en el rendimiento debido a cambios del plan de ejecución.
- Determinar el número de veces en que se ha ejecutado una consulta en una ventana de tiempo determinado, ayudando a un DBA en la solución de problemas de rendimiento de recursos.
- Identificar las principales *n* consultas (por tiempo de ejecución, consumo de memoria, etc.) en las últimas *x* horas.
- Auditar el historial de planes de consulta para una consulta determinada.
- Analizar los patrones de uso (CPU, E/S y memoria) de recursos para una base de datos determinada.
- Identificar las principales n consultas que están esperando recursos.
- Comprender la naturaleza de espera de una consulta o un plan determinados.

El Almacén de consultas contiene tres almacenes:

- un **almacén de planes** para conservar la información del plan de ejecución,
- un **almacén de estadísticas de runtime** para conservar la información de las estadísticas de ejecución,
- un **almacén de estadísticas de espera** para conservar la información de las estadísticas de espera.

El número de planes únicos que se pueden almacenar para una consulta en el almacén de planes se limita por la opción de configuración **max_plans_per_query** . Para mejorar el rendimiento, la información se escribe en los almacenes de forma asincrónica. Para minimizar el uso del espacio, se agregan las estadísticas de ejecución en tiempo de ejecución en el almacén de estadísticas de tiempo de ejecución en una ventana de tiempo fijo. La información de estos almacenes se puede ver al consultar las vistas del catálogo del Almacén de consultas.

La siguiente consulta devuelve información sobre las consultas y los planes del Almacén de consultas.

```sql
SELECT Txt.query_text_id, Txt.query_sql_text, Pl.plan_id, Qry.*
FROM sys.query_store_plan AS Pl
INNER JOIN sys.query_store_query AS Qry
    ON Pl.query_id = Qry.query_id
INNER JOIN sys.query_store_query_text AS Txt
    ON Qry.query_text_id = Txt.query_text_id ;
```

## <a name="use-the-regressed-queries-feature"></a><a name="Regressed"></a> Uso de la característica Consultas devueltas

Después de habilitar el Almacén de consultas, actualice la parte de la base de datos del panel del Explorador de objetos para agregar la sección **Almacén de consultas**.

![Árbol de Almacén de consultas de SQL Server 2016 en el Explorador de objetos de SSMS](../../relational-databases/performance/media/objectexplorerquerystore.PNG "Árbol de Almacén de consultas de SQL Server 2016 en Explorador de objetos de SSMS") ![Árbol de Almacén de consultas de SQL Server 2017 en el Explorador de objetos de SSMS](../../relational-databases/performance/media/objectexplorerquerystore_sql17.PNG "Árbol de Almacén de consultas de SQL Server 2017 en Explorador de objetos de SSMS")

Seleccione **Regressed Queries** (Consultas devueltas) para abrir el panel del mismo nombre **Regressed Queries** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. En el panel Regressed Queries (Consultas devueltas) se muestran las consultas y los planes del Almacén de consultas. Use los cuadros desplegables de la parte superior para filtrar las consultas en función de varios criterios: **Duración (ms)** (valor predeterminado), Tiempo de CPU (ms), Lecturas lógicas (KB), Escrituras lógicas (KB), Lecturas físicas (KB), Tiempo de CLR (ms), DOP, Consumo de memoria (KB), Recuento de filas, Memoria usada (KB), Memoria de base de datos temporal utilizada (KB) y Tiempo de espera (ms).

Seleccione un plan para ver el plan de consulta gráfica. Los botones están disponibles para ver la consulta de origen, forzar y no forzar un plan de consulta, alternar entre los formatos de cuadrícula y gráfico, comparar los planes seleccionados (si se ha seleccionado más de uno) y actualizar la pantalla.

![Consultas devueltas de SQL Server 2016 en Explorador de objetos de SSMS](../../relational-databases/performance/media/objectexplorerregressedqueries.PNG "Consultas devueltas de SQL Server 2016 en Explorador de objetos de SSMS")

Para aplicar un plan, seleccione una consulta y el plan y luego haga clic en **Force Plan**(Forzar plan). Solo puede forzar planes que se guardaron mediante la característica del plan de consulta y que todavía se conservan en la caché del plan de consulta.

## <a name="finding-waiting-queries"></a><a name="Waiting"></a> Buscar consultas en espera

A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], las estadísticas de espera por consulta a lo largo del tiempo están disponibles en el Almacén de consultas.

En el Almacén de consultas, los tipos de espera se combinan en **categorías de espera**. La asignación de categorías de espera a tipos de espera está disponible en [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md#wait-categories-mapping-table).

Haga clic en **Estadísticas de espera de consulta** para abrir el panel **Estadísticas de espera de consulta** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v18 o superior. En el panel Estadísticas de espera de consulta se muestra un gráfico de barras que contiene las categorías de espera principales del Almacén de consultas. Use la lista desplegable de la parte superior para seleccionar un criterio de agregado para el tiempo de espera: avg, max, min, std dev y **total** (valor predeterminado).

![Estadísticas de espera de consulta de SQL Server 2017 en Explorador de objetos de SSMS](../../relational-databases/performance/media/query-store-waits.PNG "Estadísticas de espera de consulta de SQL Server 2017 en Explorador de objetos de SSMS")

Para seleccionar una categoría de espera, haga clic en la barra; se mostrará una vista de detalle de la categoría de espera seleccionada. Este gráfico de barras nuevo contiene las consultas que han contribuido a esa categoría de espera.

![Vista de detalle de estadísticas de espera de consulta de SQL Server 2017 en Explorador de objetos de SSMS](../../relational-databases/performance/media/query-store-waits-detail.PNG "Vista de detalle de estadísticas de espera de consulta de SQL Server 2017 en Explorador de objetos de SSMS")

Use el cuadro desplegable de la parte superior para filtrar las consultas según diversos criterios de tiempo de espera para la categoría de espera seleccionada: avg, max, min, std dev y **total** (valor predeterminado). Seleccione un plan para ver el plan de consulta gráfica. Los botones están disponibles para ver la consulta de origen, aplicar y eliminar la aplicación de un plan de consulta, y actualizar la pantalla.

Las **categorías de espera** combinan distintos tipos de espera en cubos similares por naturaleza. Las distintas categorías de espera exigen un análisis de seguimiento diferente para resolver el problema, pero los tipos de espera de la misma categoría dan lugar a experiencias de solución de problemas muy similares; el proporcionar la consulta afectada además de las esperas sería la pieza que falta para completar correctamente la mayoría de las investigaciones de este tipo.

Estos son algunos ejemplos de cómo se puede obtener más información sobre la carga de trabajo antes y después de introducir categorías de espera en el Almacén de consultas:

|Experiencia anterior|Nueva experiencia|Acción|
|-|-|-|
|Altas esperas RESOURCE_SEMAPHORE por base de datos|Altas esperas de memoria en el Almacén de consultas para consultas concretas|Encuentre las consultas que más memoria consumen en el Almacén de consultas. Estas consultas probablemente retrasan el progreso de las consultas afectadas. Considere la posibilidad de usar la sugerencia de consulta MAX_GRANT_PERCENT para estas consultas o para las consultas afectadas.|
|Altas esperas LCK_M_X por base de datos|Altas esperas de bloqueo en el Almacén de consultas para consultas concretas|Compruebe los textos de consulta para las consultas afectadas e identifique las entidades de destino. En el Almacén de consultas, busque otras consultas que modifiquen la misma entidad, que se ejecuten con frecuencia o que tengan una gran duración. Tras identificar estas consultas, considere la posibilidad de cambiar la lógica de aplicación para mejorar la simultaneidad o usar un nivel de aislamiento menos restrictivo.|
|Altas esperas PAGEIOLATCH_SH por base de datos|Altas esperas de E/S del búfer en el Almacén de consultas para consultas concretas|Encuentre las consultas con un gran número de lecturas físicas en el Almacén de consultas. Si coinciden con las consultas con altas esperas de E/S, considere la posibilidad de introducir un índice en la entidad subyacente para llevar a cabo búsquedas en lugar de análisis y así minimizar la sobrecarga de E/S de las consultas.|
|Altas esperas SOS_SCHEDULER_YIELD por base de datos|Altas esperas de CPU en el Almacén de consultas para consultas concretas|Busque las consultas del Almacén de consultas que consumen más CPU. Entre ellas, identifique las consultas cuya alta tendencia de CPU se correlaciona con altas esperas de CPU para las consultas afectadas. Céntrese en la optimización de las consultas: podría ser una regresión de plan o posiblemente un índice que falta.|

## <a name="configuration-options"></a><a name="Options"></a> Opciones de configuración

Para consultar las opciones disponibles a fin de configurar los parámetros del Almacén de consultas, vea [Opciones de ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md#query-store).

Consulte la vista **sys.database_query_store_options** para determinar las opciones actuales del Almacén de consultas. Para obtener más información sobre los valores, vea [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md).

Para obtener ejemplos sobre cómo establecer opciones de configuración mediante instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], vea [Administración de opciones](#OptionMgmt).

## <a name="related-views-functions-and-procedures"></a><a name="Related"></a> Vistas, funciones y procedimientos relacionados

Vea y administre El Almacén de consultas mediante [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] o por medio de las siguientes vistas y procedimientos.

### <a name="query-store-functions"></a>Funciones del Almacén de consultas

Las funciones ayudan a las operaciones del Almacén de consultas.

:::row:::
    :::column:::
        [sys.fn_stmt_sql_handle_from_sql_stmt &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="query-store-catalog-views"></a>Vistas de catálogo del almacén de consultas

Las vistas de catálogo presentan información sobre el Almacén de consultas.

:::row:::
    :::column:::
        [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_context_settings &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_plan &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_query &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_query_text &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.query_store_runtime_stats_interval &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)
    :::column-end:::
:::row-end:::


### <a name="query-store-stored-procedures"></a>Procedimientos almacenados del almacén de consultas

Los procedimientos almacenados configuran el Almacén de consultas.

:::row:::
    :::column:::
        [sp_query_store_flush_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-flush-db-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_reset_exec_stats &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-reset-exec-stats-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_force_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-force-plan-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_unforce_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-unforce-plan-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        [sp_query_store_remove_plan &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-plan-transct-sql.md)
    :::column-end:::
    :::column:::
        [sp_query_store_remove_query &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-query-store-remove-query-transact-sql.md)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        sp_query_store_consistency_check &#40;Transact-SQL&#41;<sup>1</sup>
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

<sup>1</sup> En escenarios extremos, Almacén de consultas puede entrar en el estado ERROR debido a errores internos. A partir de SQL Server 2017 (14.x), si esto ocurre, el Almacén de consultas se puede recuperar si se ejecuta el procedimiento sp_query_store_consistency_check almacenado en la base de datos afectada. Consulte [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) para conocer los detalles de la descripción de la columna actual_state_desc.

## <a name="key-usage-scenarios"></a><a name="Scenarios"></a> Escenarios de uso clave

### <a name="option-management"></a><a name="OptionMgmt"></a> Administración de opciones

En esta sección se ofrecen algunas directrices sobre la administración de la propia característica Almacén de consultas.

**¿Está el Almacén de consultas activo actualmente?**

El Almacén de consultas almacena sus datos dentro de la base de datos del usuario, y ese es el motivo por el que tiene limitado el tamaño (configurado con `MAX_STORAGE_SIZE_MB`). Si los datos del Almacén de consultas alcanzan ese límite, el Almacén de consultas cambiará automáticamente el estado de lectura-escritura a solo lectura y dejará de recopilar datos nuevos.

Ejecute la consulta [sys.database_query_store_options](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md) para saber si el almacén de consultas está activo actualmente y si está recopilando estadísticas en tiempo de ejecución o no.

```sql
SELECT actual_state, actual_state_desc, readonly_reason,
    current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

El estado del Almacén de consultas viene determinado por la columna actual_state. Si es diferente al estado deseado, la columna `readonly_reason` puede proporcionar más información.
Cuando el tamaño del Almacén de consultas supere la cuota, la característica cambiará al modo read_only.

**Obtener opciones del Almacén de consultas**

Para averiguar información detallada sobre el estado del Almacén de consultas, ejecute lo siguiente en una base de datos de usuario.

```sql
SELECT * FROM sys.database_query_store_options;
```

**Establecimiento del intervalo del Almacén de consultas**

Puede invalidar el intervalo para agregar estadísticas de tiempo de ejecución de consultas (el valor predeterminado es 60 minutos).

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (INTERVAL_LENGTH_MINUTES = 15);
```

> [!NOTE]
> No se admiten valores arbitrarios para `INTERVAL_LENGTH_MINUTES`. Use uno de los siguientes: 1, 5, 10, 15, 30, 60 o 1440 minutos.

El nuevo valor de intervalo se expone a través de la vista **sys.database_query_store_options** .

**Uso de espacio en el Almacén de consultas**

El límite y el tamaño del Almacén de consultas se pueden comprobar con la siguiente instrucción en la base de datos de usuario.

```sql
SELECT current_storage_size_mb, max_storage_size_mb
FROM sys.database_query_store_options;
```

Si el almacenamiento del Almacén de consultas está completamente lleno, use la siguiente instrucción para ampliar el almacenamiento.

```sql
ALTER DATABASE <database_name>
SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <new_size>);
```

**Establecer opciones del Almacén de consultas**

Puede establecer varias opciones del Almacén de consultas a la vez con una sola instrucción ALTER DATABASE.

```sql
ALTER DATABASE <database name>
SET QUERY_STORE (
    OPERATION_MODE = READ_WRITE,
    CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 30),
    DATA_FLUSH_INTERVAL_SECONDS = 3000,
    MAX_STORAGE_SIZE_MB = 500,
    INTERVAL_LENGTH_MINUTES = 15,
    SIZE_BASED_CLEANUP_MODE = AUTO,
    QUERY_CAPTURE_MODE = AUTO,
    MAX_PLANS_PER_QUERY = 1000,
    WAIT_STATS_CAPTURE_MODE = ON
);
```

Para obtener la lista completa de opciones de configuración, vea [Opciones de ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).

**Limpieza del espacio**

Las tablas internas del Almacén de consultas se crean en el grupo de archivos PRIMARY durante la creación de la base de datos y esa configuración no se podrá cambiar más adelante. Si se está quedando sin espacio, puede que quiera borrar los datos más antiguos del Almacén de consultas con la siguiente instrucción.

```sql
ALTER DATABASE <db_name> SET QUERY_STORE CLEAR;
```

Como alternativa, puede que quiera borrar solo datos de consultas ad hoc, porque resulta menos relevantes para optimizaciones de consultas y análisis de plan, pero ocupa el mismo espacio.

**Eliminar consultas ad hoc**

Esto purga las consultas ad hoc e internas del almacén de consultas cada tres minutos para que no se quede sin espacio y se quiten las consultas de las que realmente es necesario realizar el seguimiento.

```sql
SET NOCOUNT ON
-- This purges adhoc and internal queries from the query store every 3 minutes so that the
-- query store does not run out of space and remove queries we really need to track
DECLARE @command varchar(1000)

SELECT @command = 'IF ''?'' NOT IN(''master'', ''model'', ''msdb'', ''tempdb'') BEGIN USE ?
EXEC(''
DECLARE @id int
DECLARE adhoc_queries_cursor CURSOR
FOR
SELECT q.query_id
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
ON q.query_text_id = qt.query_text_id
JOIN sys.query_store_plan AS p
ON p.query_id = q.query_id
JOIN sys.query_store_runtime_stats AS rs
ON rs.plan_id = p.plan_id
WHERE q.is_internal_query = 1 ' -- is it an internal query then we dont care to keep track of it

' OR q.object_id = 0' -- if it does not have a valid object_id then it is an adhoc query and we dont care about keeping track of it
' GROUP BY q.query_id
HAVING MAX(rs.last_execution_time) < DATEADD (minute, -5, GETUTCDATE()) ' -- if it has been more than 5 minutes since the adhoc query ran
' ORDER BY q.query_id ;
OPEN adhoc_queries_cursor ;
FETCH NEXT FROM adhoc_queries_cursor INTO @id;
WHILE @@fetch_status = 0
BEGIN
EXEC sp_query_store_remove_query @id
FETCH NEXT FROM adhoc_queries_cursor INTO @id
END
CLOSE adhoc_queries_cursor ;
DEALLOCATE adhoc_queries_cursor;
'') END' ;
EXEC sp_MSforeachdb @command
```

Puede definir su propio procedimiento con una lógica diferente para borrar los datos que ya no necesite.

En el ejemplo anterior se usa el procedimiento almacenado extendido **sp_query_store_remove_query** para quitar datos innecesarios. También puede usar:

- **sp_query_store_reset_exec_stats** para borrar las estadísticas de tiempo de ejecución de un plan determinado.
- **sp_query_store_remove_plan** para quitar un único plan.

### <a name="performance-auditing-and-troubleshooting"></a><a name="Peformance"></a> Auditoría del rendimiento y solución de problemas

El Almacén de consultas mantiene un historial de las métricas de compilación y tiempo de ejecución en todas las ejecuciones de consulta, lo que le permite realizar preguntas sobre la carga de trabajo.

**¿Últimas *n* consultas ejecutadas en la base de datos?**

```sql
SELECT TOP 10 qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
ORDER BY rs.last_execution_time DESC;
```

**¿Número de ejecuciones de cada consulta?**

```sql
SELECT q.query_id, qt.query_text_id, qt.query_sql_text,
    SUM(rs.count_executions) AS total_execution_count
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
GROUP BY q.query_id, qt.query_text_id, qt.query_sql_text
ORDER BY total_execution_count DESC;
```

**¿El número de consultas con el mayor tiempo medio de ejecución durante la última hora?**

```sql
SELECT TOP 10 rs.avg_duration, qt.query_sql_text, q.query_id,
    qt.query_text_id, p.plan_id, GETUTCDATE() AS CurrentUTCTime,
    rs.last_execution_time
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
WHERE rs.last_execution_time > DATEADD(hour, -1, GETUTCDATE())
ORDER BY rs.avg_duration DESC;
```

**¿El número de consultas que han tenido la media máxima de lecturas de E/S físicas durante las últimas 24 horas, con la correspondiente media del número de filas y el número de ejecuciones?**

```sql
SELECT TOP 10 rs.avg_physical_io_reads, qt.query_sql_text,
    q.query_id, qt.query_text_id, p.plan_id, rs.runtime_stats_id,
    rsi.start_time, rsi.end_time, rs.avg_rowcount, rs.count_executions
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_runtime_stats AS rs
    ON p.plan_id = rs.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi
    ON rsi.runtime_stats_interval_id = rs.runtime_stats_interval_id
WHERE rsi.start_time >= DATEADD(hour, -24, GETUTCDATE())
ORDER BY rs.avg_physical_io_reads DESC;
```

**¿Consultas con varios planes?** Estas consultas son especialmente interesantes porque son candidatas para las regresiones debido al cambio de elección del plan. La siguiente consulta identifica estas consultas junto con todos los planes:

```sql
WITH Query_MultPlans
AS
(
SELECT COUNT(*) AS cnt, q.query_id
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p
    ON p.query_id = q.query_id
GROUP BY q.query_id
HAVING COUNT(distinct plan_id) > 1
)

SELECT q.query_id, object_name(object_id) AS ContainingObject,
    query_sql_text, plan_id, p.query_plan AS plan_xml,
    p.last_compile_start_time, p.last_execution_time
FROM Query_MultPlans AS qm
JOIN sys.query_store_query AS q
    ON qm.query_id = q.query_id
JOIN sys.query_store_plan AS p
    ON q.query_id = p.query_id
JOIN sys.query_store_query_text qt
    ON qt.query_text_id = q.query_text_id
ORDER BY query_id, plan_id;
```

 **¿Consultas que se han devuelto recientemente por motivo de rendimiento (en comparación con otro momento dado)?** El siguiente ejemplo de consulta devuelve todas las consultas para las que se duplicó el tiempo de ejecución en las últimas 48 horas debido a un cambio de elección del plan. La consulta compara todos los intervalos de estadísticas en tiempo de ejecución en paralelo.

```sql
SELECT
    qt.query_sql_text,
    q.query_id,
    qt.query_text_id,
    rs1.runtime_stats_id AS runtime_stats_id_1,
    rsi1.start_time AS interval_1,
    p1.plan_id AS plan_1,
    rs1.avg_duration AS avg_duration_1,
    rs2.avg_duration AS avg_duration_2,
    p2.plan_id AS plan_2,
    rsi2.start_time AS interval_2,
    rs2.runtime_stats_id AS runtime_stats_id_2
FROM sys.query_store_query_text AS qt
JOIN sys.query_store_query AS q
    ON qt.query_text_id = q.query_text_id
JOIN sys.query_store_plan AS p1
    ON q.query_id = p1.query_id
JOIN sys.query_store_runtime_stats AS rs1
    ON p1.plan_id = rs1.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi1
    ON rsi1.runtime_stats_interval_id = rs1.runtime_stats_interval_id
JOIN sys.query_store_plan AS p2
    ON q.query_id = p2.query_id
JOIN sys.query_store_runtime_stats AS rs2
    ON p2.plan_id = rs2.plan_id
JOIN sys.query_store_runtime_stats_interval AS rsi2
    ON rsi2.runtime_stats_interval_id = rs2.runtime_stats_interval_id
WHERE rsi1.start_time > DATEADD(hour, -48, GETUTCDATE())
    AND rsi2.start_time > rsi1.start_time
    AND p1.plan_id <> p2.plan_id
    AND rs2.avg_duration > 2*rs1.avg_duration
ORDER BY q.query_id, rsi1.start_time, rsi2.start_time;
```

Si quiere ver el rendimiento de todas las regresiones (no solo de aquellas relacionadas con el cambio de elección de plan), solo tiene que eliminar la condición `AND p1.plan_id <> p2.plan_id` de la consulta anterior.

**¿Consultas que más esperan?**
Esta consulta devolverá las diez principales consultas que más esperan.

```sql
SELECT TOP 10
    qt.query_text_id,
    q.query_id,
    p.plan_id,
    sum(total_query_wait_time_ms) AS sum_total_wait_ms
FROM sys.query_store_wait_stats ws
JOIN sys.query_store_plan p ON ws.plan_id = p.plan_id
JOIN sys.query_store_query q ON p.query_id = q.query_id
JOIN sys.query_store_query_text qt ON q.query_text_id = qt.query_text_id
GROUP BY qt.query_text_id, q.query_id, p.plan_id
ORDER BY sum_total_wait_ms DESC
```

**¿Consultas que se han devuelto recientemente por motivo de rendimiento (comparando la ejecución de las recientes con las del historial)?** La siguiente consulta compara períodos de ejecución basados en ejecución de consultas. En este ejemplo concreto, la consulta compara la ejecución en el período reciente (1 hora) con el período del historial (último día) e identifica las que han especificado `additional_duration_workload`. Esta métrica se calcula como una diferencia entre la media de las ejecuciones recientes y la media de las ejecuciones del historial multiplicada por el número de ejecuciones recientes. En realidad representa la cantidad de ejecuciones recientes de duración adicional introducidas en comparación con el historial:

```sql
--- "Recent" workload - last 1 hour
DECLARE @recent_start_time datetimeoffset;
DECLARE @recent_end_time datetimeoffset;
SET @recent_start_time = DATEADD(hour, -1, SYSUTCDATETIME());
SET @recent_end_time = SYSUTCDATETIME();

--- "History" workload
DECLARE @history_start_time datetimeoffset;
DECLARE @history_end_time datetimeoffset;
SET @history_start_time = DATEADD(hour, -24, SYSUTCDATETIME());
SET @history_end_time = SYSUTCDATETIME();

WITH
hist AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
     FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE (rs.first_execution_time >= @history_start_time
               AND rs.last_execution_time < @history_end_time)
        OR (rs.first_execution_time <= @history_start_time
               AND rs.last_execution_time > @history_start_time)
        OR (rs.first_execution_time <= @history_end_time
               AND rs.last_execution_time > @history_end_time)
    GROUP BY p.query_id
),
recent AS
(
    SELECT
        p.query_id query_id,
        ROUND(ROUND(CONVERT(FLOAT, SUM(rs.avg_duration * rs.count_executions)) * 0.001, 2), 2) AS total_duration,
        SUM(rs.count_executions) AS count_executions,
        COUNT(distinct p.plan_id) AS num_plans
    FROM sys.query_store_runtime_stats AS rs
        JOIN sys.query_store_plan AS p ON p.plan_id = rs.plan_id
    WHERE  (rs.first_execution_time >= @recent_start_time
               AND rs.last_execution_time < @recent_end_time)
        OR (rs.first_execution_time <= @recent_start_time
               AND rs.last_execution_time > @recent_start_time)
        OR (rs.first_execution_time <= @recent_end_time
               AND rs.last_execution_time > @recent_end_time)
    GROUP BY p.query_id
)
SELECT
    results.query_id AS query_id,
    results.query_text AS query_text,
    results.additional_duration_workload AS additional_duration_workload,
    results.total_duration_recent AS total_duration_recent,
    results.total_duration_hist AS total_duration_hist,
    ISNULL(results.count_executions_recent, 0) AS count_executions_recent,
    ISNULL(results.count_executions_hist, 0) AS count_executions_hist
FROM
(
    SELECT
        hist.query_id AS query_id,
        qt.query_sql_text AS query_text,
        ROUND(CONVERT(float, recent.total_duration/
                   recent.count_executions-hist.total_duration/hist.count_executions)
               *(recent.count_executions), 2) AS additional_duration_workload,
        ROUND(recent.total_duration, 2) AS total_duration_recent,
        ROUND(hist.total_duration, 2) AS total_duration_hist,
        recent.count_executions AS count_executions_recent,
        hist.count_executions AS count_executions_hist
    FROM hist
        JOIN recent
            ON hist.query_id = recent.query_id
        JOIN sys.query_store_query AS q
            ON q.query_id = hist.query_id
        JOIN sys.query_store_query_text AS qt
            ON q.query_text_id = qt.query_text_id
) AS results
WHERE additional_duration_workload > 0
ORDER BY additional_duration_workload DESC
OPTION (MERGE JOIN);
```

### <a name="maintaining-query-performance-stability"></a><a name="Stability"></a> Mantener la estabilidad del rendimiento de consultas

En las consultas ejecutadas varias veces, puede observar que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa otros planes, lo que genera una duración y un uso diferentes de los recursos. Con el Almacén de consultas puede detectar cuándo se ha devuelto el rendimiento de las consultas y determinar el plan óptimo en un período de interés. Luego puede forzar ese plan óptimo para futuras ejecuciones de consultas.

También puede identificar el rendimiento incoherente de una consulta con parámetros (ya sea con parámetros automáticos o con parámetros manuales). Entre los distintos planes puede identificar el plan que es lo suficientemente rápido y óptimo para todos o la mayoría de los valores de parámetros y forzar ese plan, manteniendo un rendimiento predecible para el conjunto más amplio de escenarios de usuario.

### <a name="force-a-plan-for-a-query-apply-forcing-policy"></a>Forzar un plan para una consulta (aplicar directiva de forzado)

Cuando se fuerza un plan para una consulta determinada, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intenta forzarlo en el optimizador. Si se produce un error al exigir el plan, se producirá un evento XEvent y el optimizador realizará su trabajo de forma normal.

```sql
EXEC sp_query_store_force_plan @query_id = 48, @plan_id = 49;
```

Al usar **sp_query_store_force_plan** solo puede forzar los planes que se grabaron por el almacén de consultas como un plan para esa consulta. Es decir, los únicos planes disponibles para una consulta son aquellos que ya se han usado para ejecutar la consulta mientras el Almacén de consultas estaba activo.

#### <a name="a-namectp23-plan-forcing-support-for-fast-forward-and-static-cursors"></a><a name="ctp23"><a/> Plan para forzar la compatibilidad con cursores estáticos y de avance rápido

A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y Azure SQL Database (todos lo modelos de implementación), el Almacén de consultas admite la capacidad de forzar planes de ejecución de consulta para los cursores de API y [!INCLUDE[tsql](../../includes/tsql-md.md)] estáticos y de avance rápido. Se admite el forzado mediante `sp_query_store_force_plan` o a través de los informes del Almacén de consultas de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].

### <a name="remove-plan-forcing-for-a-query"></a>Quitar el forzado de un plan para una consulta

Para volver a confiar en el optimizador de consultas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para calcular el plan de consulta óptimo, use **sp_query_store_unforce_plan** para eliminar la aplicación del plan que se seleccionó para la consulta.

```sql
EXEC sp_query_store_unforce_plan @query_id = 48, @plan_id = 49;
```

## <a name="see-also"></a>Consulte también

- [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)
- [Uso del almacén de consultas con OLTP en memoria](../../relational-databases/performance/using-the-query-store-with-in-memory-oltp.md)
- [Query Store Usage Scenarios (Escenarios de uso del Almacén de consultas)](../../relational-databases/performance/query-store-usage-scenarios.md)
- [Introducción a la recopilación de datos del Almacén de consultas](../../relational-databases/performance/how-query-store-collects-data.md)
- [Procedimientos almacenados del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)
- [Query Store Catalog Views (Vistas de catálogo del almacén de consultas) &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/query-store-catalog-views-transact-sql.md)
- [Supervisión y optimización del rendimiento](../../relational-databases/performance/monitor-and-tune-for-performance.md)
- [Herramientas de optimización y supervisión del rendimiento](../../relational-databases/performance/performance-monitoring-and-tuning-tools.md)
- [Abrir el Monitor de actividad &#40;SQL Server Management Studio&#41;](../../relational-databases/performance-monitor/open-activity-monitor-sql-server-management-studio.md)
- [Estadísticas de consultas activas](../../relational-databases/performance/live-query-statistics.md)
- [Monitor de actividad](../../relational-databases/performance-monitor/activity-monitor.md)
- [sys.database_query_store_options &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)
