---
title: ALTER DATABASE SCOPED CONFIGURATION
description: Permite varios valores de configuración de base de datos en el nivel de base de datos individual.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 02/08/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: conceptual
f1_keywords:
- ALTER_DATABASE_SCOPED_CONFIGURATION
- ALTER_DATABASE_SCOPED_CONFIGURATION_TSQL
- DATABASE_SCOPED_CONFIGURATION_TSQL
- SCOPED_CONFIGURATION_TSQL
- SCOPED_TSQL
- ALTER_DATABASE_SCOPED_TSQL
- DATABASE_SCOPED_TSQL
helpviewer_keywords:
- ALTER DATABASE SCOPED CONFIGURATION statement
- configuration [SQL Server], ALTER DATABASE SCOPED CONFIGURATION statement
ms.assetid: 63373c2f-9a0b-431b-b9d2-6fa35641571a
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017 ||=azure-sqldw-latest
ms.openlocfilehash: 647e68fa167b24bc713266fd45527337bdd6b9e6
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100348542"
---
# <a name="alter-database-scoped-configuration-transact-sql"></a>ALTER DATABASE SCOPED CONFIGURATION (Transact-SQL)

[!INCLUDE[tsql-appliesto-ss2016-asdb-asdw-xxx-md.md](../../includes/tsql-appliesto-ss2016-asdb-asdw-xxx-md.md)]

Este comando permite varios valores de configuración de base de datos en el nivel de **base de datos individual**. 

Los valores de configuración siguientes se admiten en [!INCLUDE[sssdsfull](../../includes/sssdsfull-md.md)], en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] y en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], tal como se indica en la línea **SE APLICA A** para cada configuración de la sección [Argumentos](#arguments): 

- Borrar la caché de procedimientos.
- Establecer el parámetro MAXDOP en un valor arbitrario (1, 2, ...) para la base de datos principal en función del valor que funciona mejor para esa base de datos concreta y establecer un valor distinto (por ejemplo, 0) para todas las bases de datos secundarias que se usan (por ejemplo, para notificar consultas).
- Definir el modelo de estimación de la cardinalidad del optimizador de consultas independiente de la base de datos en el nivel de compatibilidad.
- Habilitar o deshabilitar el examen de parámetros en el nivel de base de datos.
- Habilitar o deshabilitar las revisiones de optimización de consulta en el nivel de base de datos.
- Habilitar o deshabilitar la caché de identidad en el nivel de base de datos.
- Habilitar o deshabilitar un código auxiliar de plan compilado que se almacenará en caché cuando se compile un lote por primera vez.
- Habilitar o deshabilitar la recopilación de estadísticas de ejecución para los módulos de [!INCLUDE[tsql](../../includes/tsql-md.md)] compilados de forma nativa.
- Habilitar o deshabilitar las opciones en línea de forma predeterminada para las instrucciones de DDL que admiten la sintaxis `ONLINE =`.
- Habilitar o deshabilitar las opciones reanudables de forma predeterminada para las instrucciones de DDL que admiten la sintaxis `RESUMABLE =`.
- Habilitar o deshabilitar características de [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md).
- Habilitar o deshabilitar la opción plan acelerado para forzar.
- Habilitar o deshabilitar la funcionalidad para quitar automáticamente las tablas temporales globales.
- Habilitar o deshabilitar la [infraestructura de generación de perfiles ligera de consultas](../../relational-databases/performance/query-profiling-infrastructure.md).
- Habilitar o deshabilitar el nuevo mensaje de error `String or binary data would be truncated`.
- Habilitar o deshabilitar la recopilación del último plan de ejecución real en [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).
- Especificar el número de minutos que está en pausa una operación de índice reanudable antes de que [!INCLUDE[ssde_md](../../includes/ssde_md.md)] la anule automáticamente.
- Habilitar o deshabilitar la espera de bloqueos con prioridad baja para la actualización asincrónica de las estadísticas.

Esta configuración solo está disponible en [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)].
- Establecimiento del nivel de compatibilidad de la base de datos de un usuario

![icono de vínculo](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```syntaxsql
-- Syntax for SQL Server, Azure SQL Database and Azure SQL Managed Instance

ALTER DATABASE SCOPED CONFIGURATION
{
    { [ FOR SECONDARY] SET <set_options>}
}
| CLEAR PROCEDURE_CACHE [plan_handle]
| SET < set_options >
[;]

< set_options > ::=
{
    MAXDOP = { <value> | PRIMARY}
    | LEGACY_CARDINALITY_ESTIMATION = { ON | OFF | PRIMARY}
    | PARAMETER_SNIFFING = { ON | OFF | PRIMARY}
    | QUERY_OPTIMIZER_HOTFIXES = { ON | OFF | PRIMARY}
    | IDENTITY_CACHE = { ON | OFF }
    | INTERLEAVED_EXECUTION_TVF = { ON | OFF }
    | BATCH_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ADAPTIVE_JOINS = { ON | OFF }
    | TSQL_SCALAR_UDF_INLINING = { ON | OFF }
    | ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | ELEVATE_RESUMABLE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }
    | OPTIMIZE_FOR_AD_HOC_WORKLOADS = { ON | OFF }
    | XTP_PROCEDURE_EXECUTION_STATISTICS = { ON | OFF }
    | XTP_QUERY_EXECUTION_STATISTICS = { ON | OFF }
    | ROW_MODE_MEMORY_GRANT_FEEDBACK = { ON | OFF }
    | BATCH_MODE_ON_ROWSTORE = { ON | OFF }
    | DEFERRED_COMPILATION_TV = { ON | OFF }
    | ACCELERATED_PLAN_FORCING = { ON | OFF }
    | GLOBAL_TEMPORARY_TABLE_AUTO_DROP = { ON | OFF }
    | LIGHTWEIGHT_QUERY_PROFILING = { ON | OFF }
    | VERBOSE_TRUNCATION_WARNINGS = { ON | OFF }
    | LAST_QUERY_PLAN_STATS = { ON | OFF }
    | PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = <time>
    | ISOLATE_SECURITY_POLICY_CARDINALITY  = { ON | OFF }
    | EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS = { ON | OFF }
    | ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY = { ON | OFF }
}
```

> [!IMPORTANT]
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], algunos nombres de opciones han cambiado:      
> -  `DISABLE_INTERLEAVED_EXECUTION_TVF` se ha cambiado por `INTERLEAVED_EXECUTION_TVF`
> -  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` se ha cambiado por `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
> -  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` se ha cambiado por `BATCH_MODE_ADAPTIVE_JOINS`

```syntaxsql
-- Syntax for Azure Synapse Analytics

ALTER DATABASE SCOPED CONFIGURATION
{
    SET <set_options>
}
[;]

< set_options > ::=
{
    DW_COMPATIBILITY_LEVEL = { AUTO | 10 | 20 } 
}
```

## <a name="arguments"></a>Argumentos

FOR SECONDARY

Especifica la configuración de las bases de datos secundarias (todas las bases de datos secundarias deben tener valores idénticos).

CLEAR PROCEDURE_CACHE [plan_handle]

Borra la memoria caché de procedimiento (plan) de la base de datos, y se puede ejecutar tanto en la principal como en las secundarias.

Especifique un identificador de plan de consulta para borrar un plan de consulta único de la caché de planes.

**SE APLICA A**: Se puede especificar un manipulador de plan de consulta a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)].

MAXDOP **=** {\<value> | PRIMARY } **\<value>**

Especifica el valor predeterminado **Grado máximo de paralelismo (MAXDOP)** que se debe usar para las instrucciones. 0 es el valor predeterminado e indica que en su lugar se usará la configuración del servidor. En el ámbito de la base de datos, MAXDOP reemplaza (a menos que se establezca en 0) el **grado máximo de paralelismo** establecido en el nivel de servidor mediante sp_configure. Las sugerencias de consulta aún pueden reemplazar el valor MAXDOP con ámbito de base de datos con el fin de optimizar las consultas específicas que requieran otra configuración. Todas estas configuraciones están limitadas por el valor MAXDOP establecido para el [grupo de cargas de trabajo](create-workload-group-transact-sql.md).

Puede utilizar la opción MAXDOP para limitar el número de procesadores utilizados en la ejecución de planes paralelos. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] considera los planes de ejecución en paralelo para las consultas, las operaciones de lenguaje de definición de datos (DDL) de índice, la inserción en paralelo, la modificación de columna en línea, la colección de estadísticas en paralelo y el rellenado de cursor estático y controlado por conjuntos de claves.

> [!NOTE]
> El límite del **grado máximo de paralelismo (MAXDOP)** se establece por [tarea](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). No es un límite por [solicitud](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md) ni por consulta. Esto significa que durante una ejecución de consultas en paralelo, una sola solicitud puede generar varias tareas que se asignan a un [programador](../../relational-databases/system-dynamic-management-views/sys-dm-os-tasks-transact-sql.md). Para más información, consulte la [guía de arquitectura de subprocesos y tareas](../../relational-databases/thread-and-task-architecture-guide.md).

Para establecer esta opción en el nivel de instancia, vea [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md).

> [!NOTE]
> En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], la configuración de ámbito de base de datos MAXDOP de las nuevas bases de datos de grupos simples y elásticos se establece en 8 de forma predeterminada. MAXDOP se puede configurar para cada base de datos tal y como se describe en el artículo actual. Para obtener recomendaciones sobre la configuración óptima de MAXDOP, vea la sección [Recursos adicionales](#additional-resources).

> [!TIP]
> Para llevar a cabo esta acción en el nivel de consulta, use la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md) **MAXDOP**.    
> Para llevar a cabo esta acción en el nivel de servidor, use la [opción de configuración del servidor](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md) **Grado máximo de paralelismo (MAXDOP)** .     
> Para llevar a cabo esta acción en el nivel de carga de trabajo, use la [opción de configuración del grupo de cargas de trabajo de Resource Governor](../../t-sql/statements/create-workload-group-transact-sql.md) **MAX_DOP**.    

PRIMARY

Solo se puede establecer para las secundarias, mientras la base de datos está en la principal, e indica que la configuración será la definida para la principal. Si cambia la configuración de la principal, el valor en las secundarias cambiará en consecuencia sin necesidad de establecer explícitamente el valor de las secundarias. **PRIMARY** es la configuración predeterminada para las secundarias.

LEGACY_CARDINALITY_ESTIMATION **=** { ON | **OFF** | PRIMARY }

Permite establecer el modelo de estimación de la cardinalidad del optimizador de consultas en SQL Server 2012 y versiones anteriores, independientemente del nivel de compatibilidad de la base de datos. El valor predeterminado es **OFF**, que establece el modelo de estimación de la cardinalidad del optimizador de consultas en función del nivel de compatibilidad de la base de datos. El establecimiento de LEGACY_CARDINALITY_ESTIMATION en **ON** equivale a habilitar la [marca de seguimiento 9481](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Para llevar a cabo esta acción en el nivel de consulta, agregue la [sugerencia de consulta](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, para realizar esta acción en el nivel de consulta, agregue la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT** en lugar de usar la marca de seguimiento.

PRIMARY

Este valor solo es válido en las bases de datos secundarias mientras la base de datos se encuentra en la principal y especifica que la configuración del modelo de estimación de cardinalidad del optimizador de consultas en todas las bases de datos será el valor establecido para la principal. Si cambia la configuración en la base de datos principal para el modelo de estimación de cardinalidad del optimizador de consultas, el valor en las bases de datos secundarias cambiará en consecuencia. **PRIMARY** es la configuración predeterminada para las secundarias.

PARAMETER_SNIFFING **=** { **ON** | OFF | PRIMARY }

Habilita o deshabilita el [examen de parámetros](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing). El valor predeterminado es ON. Establecer PARAMETER_SNIFFING en OFF equivale a habilitar la [marca de seguimiento 4136](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

> [!TIP]
> Para llevar a cabo esta acción en el nivel de consulta, vea la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md) **OPTIMIZE FOR UNKNOWN**.
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, para realizar esta acción en el nivel de consulta, también está disponible la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md#use_hint) **USE HINT**.

PRIMARY

Este valor solo es válido en las bases de datos secundarias mientras la base de datos se encuentra en la principal y especifica que el valor de esta configuración en todas las bases de datos secundarias será el valor establecido para la principal. Si cambia la configuración de la principal para usar el [examen de parámetros](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing), el valor en las secundarias cambiará en consecuencia sin necesidad de establecer explícitamente el valor de las secundarias. PRIMARY es la configuración predeterminada para las secundarias.

<a name="qo_hotfixes"></a> QUERY_OPTIMIZER_HOTFIXES **=** { ON | **OFF** | PRIMARY }

Habilita o deshabilita las revisiones de optimización de consulta independientemente del nivel de compatibilidad de la base de datos. El valor predeterminado es **OFF**, que deshabilita las revisiones de optimización de consulta que se publicaron después de que se introdujo el máximo nivel de compatibilidad disponible para una versión específica (posterior a RTM). Establecer este valor en **ON** es equivalente a habilitar la [marca de seguimiento 4199](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

> [!TIP]
> Para llevar a cabo esta acción en el nivel de consulta, agregue la [sugerencia de consulta](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) **QUERYTRACEON**.
> A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] SP1, para realizar esta acción en el nivel de consulta, agregue la [sugerencia de consulta](../../t-sql/queries/hints-transact-sql-query.md#use_hint) USE HINT en lugar de usar la marca de seguimiento.

PRIMARY

Este valor solo es válido en las bases de datos secundarias mientras la base de datos se encuentra en la principal y especifica que el valor de esta configuración en todas las bases de datos secundarias es el valor establecido para la principal. Si cambia la configuración de la principal, el valor en las secundarias cambia en consecuencia sin necesidad de establecer explícitamente el valor de las secundarias. PRIMARY es la configuración predeterminada para las secundarias.

IDENTITY_CACHE **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Habilita o deshabilita la caché de identidad en el nivel de base de datos. El valor predeterminado es **ON**. El almacenamiento en caché de la identidad se usa para mejorar el rendimiento de INSERT en tablas con columnas de identidad. Para evitar lagunas en los valores de una columna de identidad en los casos en que el servidor se reinicia inesperadamente o conmuta por error a un servidor secundario, deshabilite la opción IDENTITY_CACHE. Esta opción es similar a la [marca de seguimiento 272](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) existente, excepto en que se puede establecer en el nivel de base de datos, en lugar de hacerlo solo en el nivel de servidor.

> [!NOTE]
> Esta opción solo se puede establecer para PRIMARY. Para obtener más información, vea las [columnas de identidad](create-table-transact-sql-identity-property.md).

INTERLEAVED_EXECUTION_TVF **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar la ejecución intercalada de funciones con valores de tabla de múltiples instrucciones en el ámbito de base de datos o de instrucción, a la vez que se mantiene el nivel de compatibilidad de la base de datos 140 y superior. La ejecución intercalada es una característica que forma parte del procesamiento de consultas adaptable en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Para obtener más información, consulte [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 130 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.
>
> Solo en SQL Server 2017 (14.x), la opción INTERLEAVED_EXECUTION_TVF tenía el nombre anterior, **DISABLE** _INTERLEAVED_EXECUTION_TVF.

BATCH_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar los comentarios de concesión de memoria en modo por lotes en el ámbito de base de datos a la vez que se mantiene el nivel de compatibilidad de la base de datos 140 y superior. Los comentarios de concesión de memoria en modo por lotes son una característica que forma parte del [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md) incorporado en [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 130 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

BATCH_MODE_ADAPTIVE_JOINS **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar las combinaciones adaptables en modo por lotes lote en el ámbito de base de datos a la vez que se mantiene el nivel de compatibilidad de la base de datos 140 y superior. Las combinaciones adaptables en modo por lotes son una característica que forma parte del [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md) incorporado en [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)].

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 130 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

TSQL_SCALAR_UDF_INLINING **=** { **ON** | OFF }

**SE APLICA A**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] (la característica está en versión preliminar pública)

Permite habilitar o deshabilitar la inserción UDF escalar de T-SQL del ámbito de la base de datos y, a la vez, mantener el nivel de compatibilidad de la base de datos 150 y superior. La inserción UDF escalar de T-SQL forma parte de la familia de características [Procesamiento de consultas inteligente](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 140 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

ELEVATE_ONLINE = { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Le permite seleccionar opciones que hacen que el motor eleve automáticamente las operaciones admitidas a ONLINE. El valor predeterminado es OFF, que significa que las operaciones no se elevarán a ONLINE a menos que se especifique en la instrucción. [Sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) refleja el valor actual de ELEVATE_ONLINE. Estas opciones solo se aplicarán a las operaciones que son compatibles con ONLINE.

FAIL_UNSUPPORTED

Este valor eleva todas las operaciones DDL compatibles a ONLINE. Se producirá un error en las operaciones que no admiten la ejecución ONLINE y se generará una advertencia.

WHEN_SUPPORTED

Este valor eleva las operaciones que admiten ONLINE. Las operaciones que no admiten ONLINE se ejecutarán sin conexión.

> [!NOTE]
> Puede invalidar la configuración predeterminada enviando una instrucción con la opción ONLINE especificada.

ELEVATE_RESUMABLE= { OFF | WHEN_SUPPORTED | FAIL_UNSUPPORTED }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Le permite seleccionar opciones que hacen que el motor eleve automáticamente las operaciones admitidas a RESUMABLE. El valor predeterminado es OFF, que significa que las operaciones no se elevarán a RESUMABLE a menos que se especifique en la instrucción. [Sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) refleja el valor actual de ELEVATE_RESUMABLE. Estas opciones solo se aplican a las operaciones que son compatibles con RESUMABLE.

FAIL_UNSUPPORTED

Este valor eleva todas las operaciones DDL compatibles a RESUMABLE. Se produce un error en las operaciones que no admiten la ejecución RESUMABLE y se genera una advertencia.

WHEN_SUPPORTED

Este valor eleva las operaciones que admiten RESUMABLE. Las operaciones que no son compatibles con RESUMABLE se ejecutan como no reanudables.

> [!NOTE]
> Puede invalidar la configuración predeterminada enviando una instrucción con la opción RESUMABLE especificada.

OPTIMIZE_FOR_AD_HOC_WORKLOADS **=** { ON | **OFF** }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Habilita o deshabilita un código auxiliar de plan compilado que se almacenará en caché cuando se compile un lote por primera vez. El valor predeterminado es OFF. Una vez que la configuración de ámbito de base de datos OPTIMIZE_FOR_AD_HOC_WORKLOADS está habilitada para una base de datos, se almacena un código auxiliar de plan compilado en caché al compilar por primera vez un lote. Los códigos auxiliares de plan tienen una superficie de memoria menor en comparación con el tamaño del plan compilado completo. Si un lote se compila o se ejecuta de nuevo, se quitará el código auxiliar del plan compilado y se reemplazará con un plan compilado completo.

XTP_PROCEDURE_EXECUTION_STATISTICS **=** { ON | **OFF** }

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Habilita o deshabilita la recopilación de estadísticas de ejecución a nivel de módulo para los módulos de T-SQL compilados de forma nativa en la base de datos actual. El valor predeterminado es OFF. Las estadísticas de ejecución se reflejan en [sys.dm_exec_procedure_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-procedure-stats-transact-sql.md).

Las estadísticas de ejecución a nivel de módulo de los módulos de T-SQL compilados de forma nativa se recopilan si esta opción está activada o si se habilita la recopilación de estadísticas mediante [sp_xtp_control_proc_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-proc-exec-stats-transact-sql.md).

XTP_QUERY_EXECUTION_STATISTICS **=** { ON | **OFF** }

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Habilita o deshabilita la recopilación de estadísticas de ejecución a nivel de instrucción para los módulos de T-SQL compilados de forma nativa en la base de datos actual. El valor predeterminado es OFF. Las estadísticas de ejecución se reflejan en [sys.dm_exec_query_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md) y en el [Almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

Las estadísticas de ejecución a nivel de instrucción de los módulos de T-SQL compilados de forma nativa se recopilan si esta opción está activada o si se habilita la recopilación de estadísticas mediante [sp_xtp_control_query_exec_stats](../../relational-databases/system-stored-procedures/sys-sp-xtp-control-query-exec-stats-transact-sql.md).

Para más información sobre la supervisión del rendimiento de los módulos de [!INCLUDE[tsql](../../includes/tsql-md.md)] compilados de forma nativa, vea [Supervisión del rendimiento de los procedimientos almacenados compilados de forma nativa](../../relational-databases/in-memory-oltp/monitoring-performance-of-natively-compiled-stored-procedures.md).

ROW_MODE_MEMORY_GRANT_FEEDBACK **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar los comentarios de concesión de memoria en modo de fila en el ámbito de base de datos a la vez que se mantiene el nivel de compatibilidad de la base de datos 150 y superior. Los comentarios de concesión de memoria en modo de fila son una característica que forma parte del [Procesamiento de consultas inteligentes](../../relational-databases/performance/intelligent-query-processing.md) incorporado en [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] ([!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] admiten el modo de fila).

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 140 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

BATCH_MODE_ON_ROWSTORE **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar el modo por lotes en el almacenamiento de filas del ámbito de base de datos a la vez que se mantiene el nivel de compatibilidad de la base de datos 150 y superior. El modo por lotes en el almacenamiento de filas es una característica que forma parte de la familia de características [Procesamiento de consultas inteligente](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 140 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

DEFERRED_COMPILATION_TV **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar la compilación diferida de variables de tabla en el ámbito de base de datos mientras se mantiene el nivel de compatibilidad de base de datos 150 y superior. La compilación diferida de variables de tabla es una característica que forma parte de la familia de características [Procesamiento de consultas inteligente](../../relational-databases/performance/intelligent-query-processing.md).

> [!NOTE]
> Para un nivel de compatibilidad de base de datos de 140 o inferior, esta configuración de ámbito de base de datos no tiene ningún efecto.

ACCELERATED_PLAN_FORCING **=** { **ON** | OFF }

**Se aplica a**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Habilita un mecanismo optimizado para forzar el plan de consulta, aplicable a todos los formularios de plan para forzar, como [Plan para forzar el almacén de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#Regressed), [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md#automatic-plan-correction) o la sugerencia de consulta [USE PLAN](../../t-sql/queries/hints-transact-sql-query.md#use-plan). El valor predeterminado es ON.

> [!NOTE]
> No se recomienda deshabilitar la opción plan acelerado para forzar.

GLOBAL_TEMPORARY_TABLE_AUTO_DROP **=** { **ON** | OFF }

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite la configuración de la funcionalidad para colocar automáticamente las [tablas temporales globales](../../t-sql/statements/create-table-transact-sql.md#temporary-tables). El valor predeterminado es ON, lo que significa que las tablas temporales globales se quitan automáticamente cuando no están en uso en ninguna sesión. Cuando se establece en OFF, las tablas temporales globales deben quitarse explícitamente mediante una instrucción DROP TABLE o se quitarán automáticamente al reiniciar el servidor.

- En los grupos elásticos y bases de datos únicas de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta opción se puede establecer en las bases de datos de usuario individuales del servidor de SQL Database.
- En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y Azure SQL Managed Instance, esta opción se establece en `TempDB` y la configuración de las bases de datos de usuario individuales no surte ningún efecto.

<a name="lqp"></a>

LIGHTWEIGHT_QUERY_PROFILING **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar la [infraestructura ligera de generación de perfiles de consulta](../../relational-databases/performance/query-profiling-infrastructure.md). La infraestructura ligera de generación de perfiles de consulta (LWP) está habilitada de forma predeterminada y proporciona datos de rendimiento de consulta de una forma más eficaz que los mecanismos de generación de perfiles estándar.

<a name="verbose-truncation"></a>

VERBOSE_TRUNCATION_WARNINGS **=** { **ON** | OFF }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar el nuevo mensaje de error `String or binary data would be truncated`. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] presenta un mensaje de error nuevo y más específico (2628) para este escenario:

`String or binary data would be truncated in table '%.*ls', column '%.*ls'. Truncated value: '%.*ls'.`

Al establecerlo en ON bajo el nivel de compatibilidad de base de datos 150, los errores de truncamiento generan el nuevo mensaje de error 2628 para proporcionar más contexto y simplificar el proceso de solución de problemas.

Al establecerlo en OFF bajo el nivel de compatibilidad de base de datos 150, los errores de truncamiento generan el mensaje de error 8152 anterior.

Para el nivel de compatibilidad de base de datos 140 o inferior, el mensaje de error 2628 sigue siendo uno opcional que requiere que la [marca de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) 460 esté habilitada; esta configuración con ámbito de base de datos no tiene ningún efecto.

LAST_QUERY_PLAN_STATS **=** { ON | **OFF** }

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite habilitar o deshabilitar la recopilación de las estadísticas del último plan de consulta (equivalente a un plan de ejecución real) en [sys.dm_exec_query_plan_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).

PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

La opción `PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES` determina cuánto tiempo (en minutos) está en pausa el índice reanudable antes de que el motor lo anule automáticamente.

- El valor predeterminado es 1 día (1440 minutos).
- La duración mínima es 1 minuto.
- La duración máxima es 71582 minutos.
- Si se establece en 0, la operación en pausa no se anulará automáticamente.

El valor actual de esta opción se muestra en [sys. database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md).

ISOLATE_SECURITY_POLICY_CARDINALITY **=** { ON | **OFF**}

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite controlar si un predicado de [seguridad de nivel de fila](../../relational-databases/security/row-level-security.md) (RLS) afecta a la cardinalidad del plan de ejecución de la consulta de usuario general. Cuando ISOLATE_SECURITY_POLICY_CARDINALITY está activada, un predicado RLS no afecta a la cardinalidad de un plan de ejecución. Por ejemplo, imagine una tabla que contiene 1 millón de filas y un predicado RLS que restringe el resultado a 10 filas para un usuario específico que emite la consulta. Con esta configuración de ámbito de base de datos desactivada, la estimación de cardinalidad de este predicado será 10. Cuando esta configuración de ámbito de base de datos está activada, la optimización de consultas calculará 1 millón de filas. Se recomienda usar el valor predeterminado de la mayoría de las cargas de trabajo.

DW_COMPATIBILITY_LEVEL **=** {**AUTO** | 10 | 20 }

**SE APLICA A**: solo [!INCLUDE[ssazuresynapse_md](../../includes/ssazuresynapse_md.md)]

Establece [!INCLUDE[tsql](../../includes/tsql-md.md)] y los comportamientos del procesamiento de consultas para que sean compatibles con la versión especificada del motor de base de datos. Una vez establecido, cuando se ejecuta una consulta en esa base de datos, solo se usarán las características compatibles.  El nivel de compatibilidad de una base de datos se establece en AUTO de forma predeterminada cuando se crea por primera vez. El nivel de compatibilidad se conserva incluso después de las operaciones de pausar o reanudar y de copia de seguridad o restauración de la base de datos. 

|Nivel de compatibilidad    |   Comentarios|  
|-----------------------|--------------|
|**AUTO**| Predeterminada.  El motor de Synapse Analytics actualiza automáticamente su valor.  El valor actual es 20.|
|**10**| Usa los comportamientos de procesamiento de consultas y de Transact-SQL antes de la introducción del nivel de compatibilidad.|
|**20**| Primer nivel de compatibilidad que incluye los comportamientos de procesamiento de consultas y de Transact-SQL controlados. |

EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS **=** { **ON** | OFF }

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Permite controlar si las estadísticas de ejecución de las funciones escalares definidas por el usuario (UDF) se muestran en la vista del sistema [sys.dm_exec_function_stats](../../relational-databases/system-dynamic-management-views/sys-dm-exec-function-stats-transact-sql.md). En el caso de algunas cargas de trabajo intensivas que son escalares y se basan en UDF en gran medida, la recopilación de estadísticas de ejecución de funciones podría provocar una sobrecarga notable en cuanto a rendimiento. Esto se puede evitar estableciendo la configuración con ámbito de base de datos `EXEC_QUERY_STATS_FOR_SCALAR_FUNCTIONS` en `OFF`.

ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY **=** { ON | **OFF** }

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] (característica en versión preliminar pública)

Si la actualización asincrónica de las estadísticas está habilitada, la habilitación de esta configuración provocará que la solicitud en segundo plano actualice las estadísticas para esperar un bloqueo Sch-M en una cola de prioridad baja, a fin de evitar el bloqueo de otras sesiones en escenarios de simultaneidad elevada. Para obtener más información, vea [AUTO_UPDATE_STATISTICS_ASYNC](../../relational-databases/statistics/statistics.md#auto_update_statistics_async).

## <a name="permissions"></a><a name="Permissions"></a> Permisos

Requiere `ALTER ANY DATABASE SCOPED CONFIGURATION` en la base de datos. Este permiso puede concederlo un usuario que tenga el permiso `CONTROL` en una base de datos.

## <a name="general-remarks"></a>Notas generales

Aunque se pueden configurar bases de datos secundarias con valores de configuración de ámbito diferentes a los de su principal, en todas las bases de datos secundarias se usa la misma configuración. No se pueden configurar otros valores para bases de datos secundarias individuales.

La ejecución de esta instrucción borra la caché de procedimientos en la base de datos actual, lo que significa que se tendrán que volver a compilar todas las consultas.

Para las consultas con nombres de tres partes, la configuración de la conexión de base de datos actual para la consulta se respeta, menos para los módulos SQL (como procedimientos, funciones y desencadenadores) que se compilan en otro contexto de base de datos y, por tanto, se usan las opciones de la base de datos en la que residen. Del mismo modo, al actualizar las estadísticas de forma asincrónica, se respeta la configuración de ASYNC_STATS_UPDATE_WAIT_AT_LOW_PRIORITY para la base de datos en la que residen las estadísticas.

El evento `ALTER_DATABASE_SCOPED_CONFIGURATION` se agrega como un evento de DDL que se puede usar para activar un desencadenador DDL, y es un elemento secundario del grupo de desencadenadores de `ALTER_DATABASE_EVENTS`.

Las opciones de configuración con ámbito de la base de datos se transfieren con la base de datos, lo que significa que, cuando una base de datos se restaura o se adjunta, permanecen las opciones de configuración existentes.

A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)], algunos nombres de opciones han cambiado:      
-  `DISABLE_INTERLEAVED_EXECUTION_TVF` se ha cambiado por `INTERLEAVED_EXECUTION_TVF`
-  `DISABLE_BATCH_MODE_MEMORY_GRANT_FEEDBACK` se ha cambiado por `BATCH_MODE_MEMORY_GRANT_FEEDBACK`
-  `DISABLE_BATCH_MODE_ADAPTIVE_JOINS` se ha cambiado por `BATCH_MODE_ADAPTIVE_JOINS`

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones

### <a name="maxdop"></a>MAXDOP

La configuración granular puede invalidar las globales y Resource Governor puede limitar todas las demás opciones de MAXDOP. La lógica para el valor MAXDOP es la siguiente:

- La sugerencia de consulta invalida tanto `sp_configure` como la configuración con ámbito de base de datos. Si el valor MAXDOP del grupo de recursos se establece para el grupo de cargas de trabajo:

  - Si la sugerencia de consulta se establece en 0, se reemplaza por la configuración de Resource Governor.

  - Si la sugerencia de consulta no es 0, se limita por la configuración de Resource Governor.

- La configuración con ámbito de base de datos (a menos que sea 0) reemplaza el valor de `sp_configure`, a menos que haya una sugerencia de consulta y esté limitada por la configuración de Resource Governor.

- La configuración de Resource Governor reemplaza el valor de `sp_configure`.

### <a name="query_optimizer_hotfixes"></a>QUERY_OPTIMIZER_HOTFIXES

Cuando se usa la sugerencia `QUERYTRACEON` para habilitar el optimizador de consultas predeterminado de SQL Server 7.0 hasta las versiones [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o las revisiones del optimizador de consultas, sería una condición OR entre la sugerencia de consulta y el valor de configuración con ámbito de base de datos, lo que significa que, si una de las dos está habilitada, se aplican las configuraciones con ámbito de base de datos.

### <a name="geo-dr"></a>Geo DR

Las bases de datos secundarias legibles (los Grupos de disponibilidad Always On, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y las bases de datos con replicación geográfica de [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]) usan el valor secundario comprobando el estado de la base de datos. Aunque la nueva compilación no se produce en la conmutación por error y técnicamente la nuevo base de datos principal tiene consultas que usan la configuración de la secundaria, la idea es que la configuración entre principal y secundaria solo varía cuando la carga de trabajo es diferente y, por tanto, las consultas en caché usan la configuración óptima, mientras que las consultas nuevas eligen la configuración nueva que es adecuada para ellas.

### <a name="dacfx"></a>DacFx

Como `ALTER DATABASE SCOPED CONFIGURATION` es una característica nueva de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)] y [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)]) que afecta el esquema de base de datos, las exportaciones del esquema (con o sin datos) no se pueden importar a una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] o [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]. Por ejemplo, una exportación a un [DACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md) o un [BACPAC](../../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) desde una base de datos de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] o [!INCLUDE[ssSQL16](../../includes/sssql16-md.md)] que usara esta nueva característica no podría importarse a un servidor de nivel inferior.

### <a name="elevate_online"></a>ELEVATE_ONLINE

Esta opción solo se aplica a las instrucciones de DDL que admiten `WITH (ONLINE = <syntax>)`. Los índices XML no se ven afectados.

### <a name="elevate_resumable"></a>ELEVATE_RESUMABLE

Esta opción solo se aplica a las instrucciones de DDL que admiten `WITH (RESUMABLE = <syntax>)`. Los índices XML no se ven afectados.

## <a name="metadata"></a>Metadatos

En la vista del sistema [sys.database_scoped_configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md) se proporciona información sobre las configuraciones con ámbito en una base de datos. Las opciones de configuración con ámbito de base de datos solo se muestran en sys.database_scoped_configurations por ser invalidaciones de la configuración predeterminada de todo el servidor. En la vista del sistema [sys.configurations &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) solo se muestra la configuración de todo el servidor.

## <a name="examples"></a>Ejemplos

En estos ejemplos se muestra el uso de ALTER DATABASE SCOPED CONFIGURATION

### <a name="a-grant-permission"></a>A. Conceder permiso

En este ejemplo se concede el permiso necesario para ejecutar ALTER DATABASE SCOPED CONFIGURATION para el usuario Joe.

```sql
GRANT ALTER ANY DATABASE SCOPED CONFIGURATION to [Joe] ;
```

### <a name="b-set-maxdop"></a>B. Configuración de MAXDOP

En este ejemplo se establece MAXDOP = 1 para una base de datos principal y MAXDOP = 4 para una base de datos secundaria en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET MAXDOP = 1 ;
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = 4 ;
```

En este ejemplo se establece MAXDOP para una base de datos secundaria de la misma forma que se establece para su base de datos principal en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET MAXDOP = PRIMARY ;
```

### <a name="c-set-legacy_cardinality_estimation"></a>C. Configuración de LEGACY_CARDINALITY_ESTIMATION

En este ejemplo se establece LEGACY_CARDINALITY_ESTIMATION en ON para una base de datos secundaria en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = ON ;
```

En este ejemplo se establece LEGACY_CARDINALITY_ESTIMATION para una base de datos secundaria de la misma forma que para su base de datos principal en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET LEGACY_CARDINALITY_ESTIMATION = PRIMARY ;
```

### <a name="d-set-parameter_sniffing"></a>D. Configuración de PARAMETER_SNIFFING

En este ejemplo se establece PARAMETER_SNIFFING en OFF para una base de datos principal en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET PARAMETER_SNIFFING = OFF ;
```

En este ejemplo se establece PARAMETER_SNIFFING en OFF para una base de datos secundaria en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = OFF ;
```

En este ejemplo se establece PARAMETER_SNIFFING para una base de datos secundaria de la misma forma que en la base de datos principal en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION FOR SECONDARY SET PARAMETER_SNIFFING = PRIMARY ;
```

### <a name="e-set-query_optimizer_hotfixes"></a>E. Configuración de QUERY_OPTIMIZER_HOTFIXES

Se establece QUERY_OPTIMIZER_HOTFIXES en ON para una base de datos principal en un escenario de replicación geográfica.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET QUERY_OPTIMIZER_HOTFIXES = ON ;
```

### <a name="f-clear-procedure-cache"></a>F. Borrado de la caché de procedimientos

En este ejemplo se borra la caché de procedimientos (solo es posible para una base de datos principal).

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
```

### <a name="g-set-identity_cache"></a>G. Configuración de IDENTITY_CACHE

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

En este ejemplo se deshabilita la caché de identidad.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET IDENTITY_CACHE = OFF ;
```

### <a name="h-set-optimize_for_ad_hoc_workloads"></a>H. Configuración de OPTIMIZE_FOR_AD_HOC_WORKLOADS

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

En este ejemplo se habilita o deshabilita un código auxiliar de plan compilado que se almacenará en caché cuando se compile un lote por primera vez.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET OPTIMIZE_FOR_AD_HOC_WORKLOADS = ON;
```

### <a name="i-set-elevate_online"></a>I. Establecer ELEVATE_ONLINE

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Este ejemplo establece ELEVATE_ONLINE en FAIL_UNSUPPORTED.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_ONLINE = FAIL_UNSUPPORTED ;
```

### <a name="j-set-elevate_resumable"></a>J. Establecer ELEVATE_RESUMABLE

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

Este ejemplo establece ELEVATE_RESUMABLE en WHEN_SUPPORTED.

```sql
ALTER DATABASE SCOPED CONFIGURATION SET ELEVATE_RESUMABLE = WHEN_SUPPORTED ;
```

### <a name="k-clear-a-query-plan-from-the-plan-cache"></a>K. Borrado de un plan de consulta de la caché de planes

**SE APLICA A**: En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]), en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y en [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

En este ejemplo se borra un plan específico de la caché de procedimientos.

```sql
ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE 0x06000500F443610F003B7CD12C02000001000000000000000000000000000000000000000000000000000000;
```

### <a name="l-set-paused-duration"></a>L. Establecer la duración de la pausa

**SE APLICA A**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDSMIfull](../../includes/sssdsmifull-md.md)]

En este ejemplo, la duración de la pausa del índice reanudable se establece en 60 minutos.

```sql
ALTER DATABASE SCOPED CONFIGURATION
SET PAUSED_RESUMABLE_INDEX_ABORT_DURATION_MINUTES = 60
```

## <a name="additional-resources"></a>Recursos adicionales

### <a name="maxdop-resources"></a>Recursos de MAXDOP

- [Grado de paralelismo](../../relational-databases/query-processing-architecture-guide.md#DOP)
- [Recommendations and guidelines for the "max degree of parallelism" configuration option in SQL Server](https://support.microsoft.com/kb/2806535) (Recomendaciones y directrices relativas a la opción de configuración "grado máximo de paralelismo" en SQL Server)

### <a name="legacy_cardinality_estimation-resources"></a>Recursos de LEGACY_CARDINALITY_ESTIMATION

- [Estimación de cardinalidad (SQL Server)](../../relational-databases/performance/cardinality-estimation-sql-server.md)
- [Optimizing Your Query Plans with the SQL Server 2014 Cardinality Estimator](/previous-versions/dn673537(v=msdn.10))

### <a name="parameter_sniffing-resources"></a>Recursos de PARAMETER_SNIFFING

- [Examen de parámetros](../../relational-databases/query-processing-architecture-guide.md#ParamSniffing)
- ["I smell a parameter!"](/archive/blogs/queryoptteam/i-smell-a-parameter) ("¡Huelo un parámetro!")

### <a name="query_optimizer_hotfixes-resources"></a>Recursos de QUERY_OPTIMIZER_HOTFIXES

- [Marcas de seguimiento](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md)
- [SQL Server query optimizer hotfix trace flag 4199 servicing model](https://support.microsoft.com/kb/974006) (Modelo de servicios de la marca de seguimiento 4199 de revisión del optimizador de consultas de SQL Server)

### <a name="elevate_online-resources"></a>Recursos ELEVATE_ONLINE

[Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

### <a name="elevate_resumable-resources"></a>Recursos ELEVATE_RESUMABLE

[Directrices para operaciones de índices en línea](../../relational-databases/indexes/guidelines-for-online-index-operations.md)

## <a name="more-information"></a>Más información   
 [sys.database_scoped_configurations](../../relational-databases/system-catalog-views/sys-database-scoped-configurations-transact-sql.md)      
 [Recomendaciones y directrices para la opción de configuración "grado máximo de paralelismo" en SQL Server](../../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines)      
 [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md)    
 [Vistas de catálogo de archivos y bases de datos (Transact-SQL)](../../relational-databases/system-catalog-views/databases-and-files-catalog-views-transact-sql.md)    
 [Opciones de configuración del servidor](../../database-engine/configure-windows/server-configuration-options-sql-server.md)    
 [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md)    
 [Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)    
 [ALTER INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-index-transact-sql.md)    
 [CREATE INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-index-transact-sql.md)
