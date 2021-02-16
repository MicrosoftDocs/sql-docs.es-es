---
title: Opciones de ALTER DATABASE SET (Transact-SQL) | Microsoft Docs
description: Obtenga información sobre cómo configurar opciones de base de datos como la optimización automática, el cifrado, el Almacén de consultas, en SQL Server y Azure SQL Database.
ms.custom: ''
ms.date: 09/04/2020
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- online database state [SQL Server]
- database options [SQL Server]
- emergency database state [SQL Server]
- databases [SQL Server], options
- read-only databases
- recovery models [SQL Server], switching
- ALTER DATABASE statement, SET options
- offline database state [SQL Server]
- snapshot isolation framework option
- checksums [SQL Server]
- Automatic tuning
- " Data Retention Policy"
- query plan regression correction
- auto_create_statistics
- auto_update_statistics
- Query Store options
ms.assetid: f76fbd84-df59-4404-806b-8ecb4497c9cc
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||=azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azure-sqldw-latest||=azuresqldb-mi-current
ms.openlocfilehash: 37ea2f4896d68f4ae5a47b14025d54a344febdc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016345"
---
# <a name="alter-database-set-options-transact-sql"></a>Opciones de ALTER DATABASE SET (Transact-SQL)

Establece opciones de base de datos en Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] y [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]. Para más información sobre otras opciones de ALTER DATABASE, consulte [ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql.md).

Seleccione una de las pestañas siguientes para obtener la sintaxis, los argumentos, los comentarios, los permisos y los ejemplos de una versión de SQL concreta con la que trabaja.

Para más información sobre las convenciones de sintaxis, vea [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).

> [!NOTE]
> Es posible que, para establecer algunas opciones con ALTER DATABASE, se necesite acceso exclusivo a la base de datos. Si la instrucción ALTER DATABASE no se completa a tiempo, compruebe que no haya otras sesiones en la base de datos que estén bloqueando la sesión de ALTER DATABASE.

## <a name="select-a-product"></a>Selección de un producto

En la siguiente fila, seleccione cualquier nombre de producto que le interese. Al hacerlo, en esta página web se muestra otro contenido, adecuado para el producto que seleccione.

::: moniker range=">=sql-server-2016||>=sql-server-linux-2017"

:::row:::
    :::column:::
        **_\* SQL Server \*_** &nbsp;
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-server"></a>SQL Server

La creación de reflejo de la base de datos, [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], y los niveles de compatibilidad son opciones de `SET`, pero se describen en otros artículos debido a su extensión. Para más información, vea [Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md), [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md) y [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

Las configuraciones con ámbito de base de datos se utilizan para establecer varias configuraciones de base de datos en el nivel de base de datos individual. Para obtener más información, vea [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).

> [!NOTE]
> Se pueden configurar muchas opciones SET de la base de datos para la sesión actual mediante [instrucciones SET](../../t-sql/statements/set-statements-transact-sql.md), aunque generalmente las configuran las aplicaciones al realizar la conexión. Las opciones SET de nivel de sesión reemplazan a los valores **ALTER DATABASE SET**. Las opciones de base de datos descritas en las secciones siguientes son valores que se pueden establecer para sesiones que no proporcionan de forma explícita otros valores de opciones SET.

## <a name="syntax"></a>Sintaxis

```syntaxsql
ALTER DATABASE { database_name | CURRENT }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}

<option_spec> ::=
{
    <accelerated_database_recovery>
  | <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <containment_option>
  | <cursor_option>
  | <database_mirroring_option>
  | <date_correlation_optimization_option>
  | <db_encryption_option>
  | <db_state_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <external_access_option>
  | FILESTREAM ( <FILESTREAM_option> )
  | <HADR_options>
  | <mixed_page_allocation_option>
  | <parameterization_option>
  | <query_store_options>
  | <recovery_option>
  | <remote_data_archive_option>
  | <service_broker_option>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
  | <data_retention_policy>
}
;

<accelerated_database_recovery> ::=
{
    ACCELERATED_DATABASE_RECOVERY = { ON | OFF }
     [ ( PERSISTENT_VERSION_STORE_FILEGROUP = { filegroup name } ) ];
}

<auto_option> ::=
{
    AUTO_CLOSE { ON | OFF }
  | AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
   {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
   }
}

<change_tracking_option_list> ::=
{
   AUTO_CLEANUP = { ON | OFF }
 | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
}

<containment_option> ::=
   CONTAINMENT = { NONE | PARTIAL }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
  | CURSOR_DEFAULT { LOCAL | GLOBAL }
}

<database_mirroring_option>
  ALTER DATABASE Database Mirroring

<date_correlation_optimization_option> ::=
    DATE_CORRELATION_OPTIMIZATION { ON | OFF }
  
<db_encryption_option> ::=
    ENCRYPTION { ON | OFF | SUSPEND | RESUME }

<db_state_option> ::=
    { ONLINE | OFFLINE | EMERGENCY }

<db_update_option> ::=
    { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
    { SINGLE_USER | RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::=
    DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<external_access_option> ::=
{
    DB_CHAINING { ON | OFF }
  | TRUSTWORTHY { ON | OFF }
  | DEFAULT_FULLTEXT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | DEFAULT_LANGUAGE = { <lcid> | <language name> | <language alias> }
  | NESTED_TRIGGERS = { OFF | ON }
  | TRANSFORM_NOISE_WORDS = { OFF | ON }
  | TWO_DIGIT_YEAR_CUTOFF = { 1753, ..., 2049, ..., 9999 }
}

<FILESTREAM_option> ::=
{
    NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL
  | DIRECTORY_NAME = <directory_name>
}

<HADR_options> ::=
    ALTER DATABASE SET HADR

<mixed_page_allocation_option> ::=
    MIXED_PAGE_ALLOCATION { OFF | ON }

<parameterization_option> ::=
    PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
    QUERY_STORE
    {
          = OFF [ ( FORCED ) ] 
        | = ON [ ( <query_store_option_list> [,...n] ) ]
        | ( < query_store_option_list> [,...n] )
        | CLEAR [ ALL ]
    }
}

<query_store_option_list> ::=
{
      OPERATION_MODE = { READ_WRITE | READ_ONLY }
    | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
    | DATA_FLUSH_INTERVAL_SECONDS = number
    | MAX_STORAGE_SIZE_MB = number
    | INTERVAL_LENGTH_MINUTES = number
    | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
    | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
    | MAX_PLANS_PER_QUERY = number
    | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
    | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
      STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<recovery_option> ::=
{
    RECOVERY { FULL | BULK_LOGGED | SIMPLE }
  | TORN_PAGE_DETECTION { ON | OFF }
  | PAGE_VERIFY { CHECKSUM | TORN_PAGE_DETECTION | NONE }
}

<remote_data_archive_option> ::=
{
    REMOTE_DATA_ARCHIVE =
    {
        ON ( SERVER = <server_name> ,
             { 
                  CREDENTIAL = <db_scoped_credential_name>
                  | FEDERATED_SERVICE_ACCOUNT = ON | OFF
             }
        )
        | OFF
    }
}

<service_broker_option> ::=
{
    ENABLE_BROKER
  | DISABLE_BROKER
  | NEW_BROKER
  | ERROR_BROKER_CONVERSATIONS
  | HONOR_BROKER_PRIORITY { ON | OFF}
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT { ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT = { ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<target_recovery_time_option> ::=
    TARGET_RECOVERY_TIME = target_recovery_time { SECONDS | MINUTES }

<termination>::=
{
    ROLLBACK AFTER number [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention> ::=
    TEMPORAL_HISTORY_RETENTION { ON | OFF }

<data_retention_policy> ::=
    DATA_RETENTION { ON | OFF }

```

## <a name="arguments"></a>Argumentos

*database_name*     
Nombre de la base de datos que se va a modificar.

CURRENT     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Ejecuta la acción de la base de datos actual. `CURRENT` no se admite para todas las opciones en todos los contextos. Si `CURRENT` produce un error, proporcione el nombre de la base de datos.

**\<accelerated_database_recovery> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Habilita la [recuperación de bases de datos acelerada](../../relational-databases/accelerated-database-recovery-management.md) (ADR) por base de datos. De forma predeterminada, ADR se establece en OFF en [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]. Mediante esta sintaxis, puede designar un grupo de archivos específico para los datos del almacén de versiones persistente (PVS). Si no se especifica ningún grupo de archivos, el PVS se almacenará en el grupo de archivos PRIMARY. Para ver más ejemplos e información, vea [Recuperación de bases de datos acelerada](../../relational-databases/accelerated-database-recovery-management.md).

**\<auto_option> ::=**      
Controla las opciones automáticas.

<a name="auto_close"></a> AUTO_CLOSE { ON | **OFF** }     
ACTIVAR     
La base de datos se cierra correctamente y se liberan sus recursos después de que salga el último usuario.

La base de datos se vuelve a abrir automáticamente cuando un usuario intenta utilizarla de nuevo. Por ejemplo, este comportamiento de reapertura se produce cuando un usuario emite una instrucción `USE database_name`. La base de datos puede cerrarse limpiamente con la opción AUTO_CLOSE establecida en ON. Si es así, la base de datos no se vuelve a abrir hasta que un usuario intente usar la base de datos la próxima vez que [!INCLUDE[ssDE](../../includes/ssde-md.md)] se reinicie.

Apagado     
La base de datos permanece abierta después de que haya salido el último usuario.

La opción AUTO_CLOSE es útil para las bases de datos de escritorio porque permite administrar los archivos de la base de datos como archivos normales. Se pueden mover, copiar para realizar copias de seguridad e incluso enviar por correo electrónico a otros usuarios. El proceso AUTO_CLOSE es asincrónico. La apertura y cierre repetidos de la base de datos no reduce el rendimiento.

> [!NOTE]
> La opción AUTO_CLOSE no está disponible en una base de datos independiente o en [!INCLUDE[ssSDS](../../includes/sssds-md.md)].
> Para determinar el estado de esta opción, examine la columna `is_auto_close_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsAutoClose` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).
>
> Si AUTO_CLOSE está establecido en ON, algunas columnas de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) y de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md) devolverán NULL porque la base de datos no está disponible para recuperar los datos. Para resolver esta incidencia, ejecute una instrucción USE para abrir la base de datos.
>
> La creación de reflejo de la base de datos requiere que se establezca AUTO_CLOSE en OFF.

Cuando la base de datos se establece en `AUTOCLOSE = ON`, una operación que inicia el cierre automático de la base de datos borra la memoria caché de planes para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] Service Pack 2, para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene el siguiente mensaje informativo: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ACTIVAR     
El optimizador de consultas crea las estadísticas en columnas únicas de los predicados de consulta, según sea necesario, para mejorar los planes de consulta y el rendimiento de las consultas. Estas estadísticas de columna única se crean cuando el optimizador de consultas las compila. Las estadísticas de columna única solamente se crean en las columnas que ya no son la primera columna de un objeto de estadísticas existente.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Apagado     
El optimizador de consultas no crea estadísticas en columnas únicas de los predicados de consulta cuando se compilan las consultas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna `is_auto_create_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoCreateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Uso de las opciones de estadísticas de toda la base de datos" de [Estadísticas](../../relational-databases/statistics/statistics.md).

INCREMENTAL = ON | **OFF**     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]

Establezca AUTO_CREATE_STATISTICS en ON y establezca INCREMENTAL en ON. Esto crea de forma automática estadísticas como incrementales siempre que se admitan estadísticas incrementales. El valor predeterminado es OFF. Para más información, consulte [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVAR     
Los archivos de la base de datos se pueden reducir periódicamente.

Los archivos de datos y los archivos de registro se pueden reducir de forma automática. AUTO_SHRINK reduce el tamaño del registro de transacciones solo si establece la base de datos en el modelo de recuperación SIMPLE o si realiza una copia de seguridad del registro. Cuando AUTO_SHRINK se establece en OFF, los archivos de la base de datos no se reducen de forma automática durante las comprobaciones periódicas del espacio no utilizado.

La opción AUTO_SHRINK reduce los archivos cuando no se usa más de un 25 % del espacio del archivo. Reduce el archivo a uno de dos tamaños (el que sea mayor):

- El tamaño al que el 25 % del archivo es espacio sin usar
- El tamaño del archivo cuando se creó

No puede reducir una base de datos de solo lectura.

Apagado     
Los archivos de la base de datos no se reducen automáticamente durante las comprobaciones periódicas del espacio no utilizado.

Para determinar el estado de esta opción, examine la columna `is_auto_shrink_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoShrink` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La opción AUTO_SHRINK no está disponible en una base de datos independiente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVAR     
Especifica que el optimizador de consultas actualiza las estadísticas cuando las usa una consulta y puedan estar obsoletas. Las estadísticas se vuelven obsoletas después de que operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indizada. Para determinar cuándo las estadísticas pueden quedar obsoletas, el optimizador de consultas cuenta el número de modificaciones de datos desde la actualización más reciente de las estadísticas y compara el número de modificaciones con un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.

El optimizador de consultas comprueba si hay estadísticas obsoletas antes de compilar una consulta y ejecuta un plan de consulta almacenado en caché. El optimizador de consultas usa las columnas, tablas y vistas indexadas del predicado de consulta para determinar qué estadísticas podrían estar obsoletas. El optimizador de consultas determina esta información antes de compilar una consulta. Antes de ejecutar un plan de consulta almacenado en caché, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba que el plan de consulta haga referencia a las estadísticas actualizadas.

La opción AUTO_UPDATE_STATISTICS se aplica a las estadísticas creadas para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción CREATE STATISTICS. Esta opción también se aplica a las estadísticas filtradas.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Utilice la opción AUTO_UPDATE_STATISTICS_ASYNC para especificar si las estadísticas se actualizan sincrónica o asincrónicamente.

Apagado     
Especifica que el optimizador de consultas no actualiza las estadísticas cuando una consulta las usa. El optimizador de consultas tampoco actualiza las estadísticas cuando podrían estar obsoletas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna `is_auto_update_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoUpdateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Uso de las opciones de estadísticas de toda la base de datos" de [Estadísticas](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVAR     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son asincrónicas. El optimizador de consultas no espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

La configuración de esta opción en ON no surte efecto a menos que AUTO_UPDATE_STATISTICS se establezca en ON.

De forma predeterminada, la opción AUTO_UPDATE_STATISTICS_ASYNC está configurada en OFF y el optimizador de consultas actualiza las estadísticas de manera sincrónica.

Apagado     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son sincrónicas. El optimizador de consultas espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

> [!NOTE]
> La configuración de esta opción en OFF no surte efecto a menos que AUTO_UPDATE_STATISTICS esté configurado en ON.

Para determinar el estado de esta opción, examine la columna `is_auto_update_stats_async_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Para más información sobre cuándo usar las actualizaciones de estadísticas sincrónicas o asincrónicas, vea la sección "Opciones de estadísticas" en [Estadísticas](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)])

Habilita o deshabilita la opción [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md) de `FORCE_LAST_GOOD_PLAN`.

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ACTIVAR     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] fuerza automáticamente el último buen plan conocido en las consultas de [!INCLUDE[tsql-md](../../includes/tsql-md.md)], donde el nuevo plan de consulta provoca regresiones de rendimiento. El parámetro [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervisa continuamente el rendimiento de la consulta [!INCLUDE[tsql-md](../../includes/tsql-md.md)] con el plan forzado.

Si hay mejoras de rendimiento, el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] seguirá usando el último buen plan conocido. Si no se detectan mejoras de rendimiento, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] generará un nuevo plan de consulta. Se producirá un error en la instrucción si el almacén de consultas no está habilitado o si no está en modo de *lectura y escritura*.

Apagado     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] informa de posibles regresiones de rendimiento de consultas provocadas por cambios del plan de consulta en la vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md), aunque estas recomendaciones no se aplican automáticamente. Para supervisar las recomendaciones activas y corregir los problemas identificados, los usuarios pueden aplicar los scripts de [!INCLUDE[tsql-md](../../includes/tsql-md.md)] que se muestran en la vista. El valor predeterminado es OFF.

**\<change_tracking_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y[!INCLUDE[ssSDSFull](../../includes/sssds-md.md)]

Controla las opciones de seguimiento de cambios. Puede habilitar el seguimiento de cambios, establecer y cambiar opciones, y deshabilitar el seguimiento de cambios. Para obtener ejemplos, vea la sección "Ejemplos" más adelante en este artículo.

ACTIVAR     
Habilita el seguimiento de cambios para la base de datos. Si habilita el seguimiento de cambios, también puede establecer las opciones AUTO CLEANUP y CHANGE RETENTION.

AUTO_CLEANUP = { ON | **OFF** }     
ACTIVAR     
La información sobre el seguimiento de cambios se quita de forma automática después del período de retención especificado.

Apagado     
Los datos del seguimiento de cambios no se quitan automáticamente de la base de datos.

CHANGE_RETENTION = *período_de_retención* { **DAYS** | HOURS | MINUTES }     
Especifica el período mínimo para mantener la información del seguimiento de cambios en la base de datos. Los datos solamente se quitan cuando el valor AUTO_CLEANUP es ON.

*retention_period* es un entero que especifica el componente numérico del período de retención.

El período de retención predeterminado es de **2 días**. El período de retención mínimo es de 1 minuto. El tipo de retención predeterminado es **DAYS**.

Apagado     
Deshabilita el seguimiento de cambios para la base de datos. Deshabilite el seguimiento de cambios en todas las tablas para poder deshabilitarlo en la base de datos.

**\<containment_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Controla las opciones de contención de la base de datos.

CONTAINMENT = { **NONE** | PARTIAL}     
Ninguno     
La base de datos no es una base de datos independiente.

PARTIAL     
La base de datos es una base de datos independiente. El establecimiento de la contención de la base de datos en parcial producirá un error si la base de datos tiene habilitados la replicación, la captura de datos modificados o el seguimiento de cambios. La comprobación de errores se detiene después de un error. Para obtener más información acerca de las bases de datos independientes, vea [Contained Databases](../../relational-databases/databases/contained-databases.md).

**\<cursor_option> ::=**     

Controla las opciones del cursor.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVAR     
Todos los cursores abiertos cuando confirma o deshace una transacción se cierran.

Apagado     
Los cursores permanecen abiertos cuando se confirma una transacción. Cuando se revierte se cierran todos los cursores, excepto los que están definidos como INSENSITIVE o STATIC.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada de la base de datos para CURSOR_CLOSE_ON_COMMIT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión que establece CURSOR_CLOSE_ON_COMMIT en OFF para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_cursor_close_on_commit_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsCloseCursorsOnCommitEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

CURSOR_DEFAULT { LOCAL | GLOBAL }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla si el ámbito del cursor utiliza LOCAL o GLOBAL.

LOCAL     
Cuando especifica LOCAL y no define un cursor como GLOBAL cuando crea el cursor, el alcance del cursor es local. Específicamente, el alcance es local para el proceso por lotes, el procedimiento almacenado o el activador en el que creó el cursor. El nombre del cursor solamente es válido dentro de este ámbito.

Es posible hacer referencia al cursor mediante variables de cursor locales del lote, procedimiento almacenado, desencadenador o parámetro OUTPUT del procedimiento almacenado. La asignación del cursor se desasigna implícitamente cuando el lote, el procedimiento almacenado o el desencadenador finaliza. El cursor se desasigna a menos que se haya pasado de nuevo en un parámetro OUTPUT. El cursor se podría pasar en un parámetro OUTPUT. Si el cursor se vuelve a pasar de esta forma, se desasigna cuando la última variable que hace referencia a él se desasigna o se sale del ámbito.

GLOBAL     
Si se especifica GLOBAL y no se define ningún cursor como LOCAL al crearlo, el ámbito del cursor es global para la conexión. Se puede hacer referencia al nombre del cursor en cualquier procedimiento almacenado o lote que se ejecute durante la conexión.

El cursor se desasigna implícitamente solamente cuando se realiza la desconexión. Para más información, consulte [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_local_cursor_default` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsLocalCursorsDefault` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<data_retention_policy> ::=**

**Se aplica a**: *solo* para Azure SQL Edge

DATA_RETENTION { ON | OFF }   
ACTIVAR    
Habilita la limpieza basada en la directiva de retención de datos en una base de datos.

Apagado   
Deshabilita la limpieza basada en la directiva de retención de datos en una base de datos.

**\<database_mirroring>**     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Para las descripciones del argumento, consulte [Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md).

**\<date_correlation_optimization_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla la opción date_correlation_optimization.

DATE_CORRELATION_OPTIMIZATION { ON | **OFF** }     
ACTIVAR     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mantiene estadísticas de correlación donde una restricción FOREIGN KEY vincula dos tablas cualesquiera en la base de datos y las tablas tienen columnas **datetime**.

Apagado     
No se mantiene ninguna estadística de correlación.

Para establecer DATE_CORRELATION_OPTIMIZATION en ON, no debe haber ninguna conexión activa con la base de datos, salvo la que ejecuta la instrucción ALTER DATABASE. Después se admitirán múltiples conexiones.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_date_correlation_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<db_encryption_option> ::=**      
Controla el estado del cifrado de la base de datos.

ENCRYPTION { ON \| **OFF** \| SUSPEND \| RESUME }     
ACTIVAR     
Establece la base de datos que se va a cifrar.

Apagado     
Establece la base de datos que no se va a cifrar.

SUSPEND     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])     

Puede utilizarse para pausar el examen de cifrado después de que el cifrado de datos transparente se haya habilitado o deshabilitado, o después de cambiar la clave de cifrado.

RESUME     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Se puede usar para reanudar un examen de cifrado pausado anteriormente.

Para más información sobre el cifrado de base de datos, consulte [Cifrado de datos transparente](../../relational-databases/security/encryption/transparent-data-encryption.md) y [Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Cuando el cifrado está habilitado en el nivel de la base de datos, se cifrarán todos los grupos de archivos. Todos los grupos de archivos nuevos heredarán la propiedad de cifrado. Si en la base de datos hay grupos de archivos establecidos en READ ONLY, se producirá un error en la operación de cifrado de la base de datos.

Puede ver el estado del cifrado de la base de datos, así como el estado del análisis de cifrado, mediante la vista de administración dinámica [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_state_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla el estado de la base de datos.

OFFLINE     
La base de datos está cerrada, se ha cerrado correctamente y se ha marcado como sin conexión. La base de datos no se puede modificar mientras está desconectada.

ONLINE     
La base de datos está abierta y disponible para su uso.

EMERGENCY     
La base de datos está marcada como READ_ONLY, el registro está deshabilitado y el acceso está limitado a miembros del rol fijo de servidor sysadmin. EMERGENCY se utiliza principalmente para la solución de problemas. Por ejemplo, una base de datos marcada como sospechosa debido a un archivo de registro dañado se puede establecer en el estado EMERGENCY. Este valor puede habilitar el acceso de solo lectura del administrador del sistema a la base de datos. Solamente los miembros del rol fijo de servidor sysadmin pueden establecer una base de datos en el estado EMERGENCY.

Necesita el permiso `ALTER DATABASE` para la base de datos de asunto, para cambiar una base de datos al estado sin conexión o de emergencia, y el permiso `ALTER ANY DATABASE` de nivel de servidor para conectar una base de datos que estaba sin conexión.

Para determinar el estado de esta opción, examine las columnas `state` y `state_desc` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `Status` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). Para más información, consulte [Database States](../../relational-databases/databases/database-states.md).

Una base de datos marcada como RESTORING no se puede establecer como OFFLINE, ONLINE o EMERGENCY. Es posible que una base de datos se encuentre en estado RESTORING durante una operación de restauración activa o cuando se produce un error en una operación de restauración de una base de datos o de un archivo de registro, debido a un archivo de copia de seguridad dañado.

**\<db_update_option> ::=**

Controla si se permiten las actualizaciones en la base de datos.

READ_ONLY     
Los usuarios pueden leer datos de la base de datos, pero no pueden modificarlos.

> [!NOTE]
> Para mejorar el rendimiento de las consultas, actualice las estadísticas antes de establecer una base de datos en READ_ONLY. Si se necesitan estadísticas adicionales después de establecer una base de datos en READ_ONLY, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] creará las estadísticas en tempdb. Para más información sobre las estadísticas para una base de datos de solo lectura, vea [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de datos está disponible para operaciones de lectura y escritura.

Para cambiar este estado, debe tener acceso exclusivo a la base de datos. Para obtener más información, vea la cláusula SINGLE_USER.

> [!NOTE]
> En las bases de datos federadas de [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], `SET { READ_ONLY | READ_WRITE }` está deshabilitado.

**\<db_user_access_option> ::=**      
Controla el acceso del usuario a la base de datos.

SINGLE_USER     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Especifica que solamente puede tener acceso a la base de datos un usuario cada vez. Si especifica SINGLE_USER y otros usuarios se conectan a la base de datos, la instrucción ALTER DATABASE se bloquea hasta que todos los usuarios se desconecten de la base de datos especificada. Para invalidar este comportamiento, vea la cláusula WITH \<termination>.

La base de datos permanece en modo SINGLE_USER incluso si el usuario que estableció la opción cierra la sesión. A partir de ese momento, un usuario distinto, pero solo uno, puede conectarse a la base de datos.

Antes de establecer la base de datos como SINGLE_USER, compruebe que la opción AUTO_UPDATE_STATISTICS_ASYNC está establecida en OFF. Cuando se establece en ON, el subproceso en segundo plano usado para actualizar las estadísticas realiza una conexión con la base de datos y no podrá tener acceso a la base de datos en modo de usuario único. Para ver el estado de esta opción, consulte la columna `is_auto_update_stats_async_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). Si la opción está establecida en ON, realice las tareas siguientes:

1. Establezca AUTO_UPDATE_STATISTICS_ASYNC en OFF.

2. Compruebe si hay trabajos de estadísticas asincrónicos consultando la vista de administración dinámica [sys.dm_exec_background_job_queue](../../relational-databases/system-dynamic-management-views/sys-dm-exec-background-job-queue-transact-sql.md).

Si hay trabajos activos, permita que estos se completen o termínelos de forma manual mediante [KILL STATS JOB](../../t-sql/language-elements/kill-stats-job-transact-sql.md).

RESTRICTED_USER     
Solo permite a los miembros del rol fijo de base de datos `db_owner` y de los roles fijos de servidor `dbcreator` y `sysadmin` conectarse a la base de datos. RESTRICTED_USER no limita su número. Desconecte todas las conexiones con la base de datos mediante el margen de tiempo especificado por la cláusula de terminación de la instrucción ALTER DATABASE. Una vez que la base de datos ha cambiado al estado RESTRICTED_USER, se rechazan los intentos de conexión por parte de usuarios no cualificados.

MULTI_USER     
Todos los usuarios que tengan los permisos correspondientes pueden conectarse a la base de datos.

Para determinar el estado de esta opción, examine la columna `user_access` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `UserAccess` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

Controla si las transacciones se confirman con perdurabilidad total o diferida.

DISABLED     
Todas las transacciones tras `SET DISABLED` son totalmente perdurables. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

ALLOWED     
Todas las transacciones tras `SET ALLOWED` son totalmente perdurables o de perdurabilidad diferida, dependiendo de la opción de perdurabilidad establecida en el bloque ATOMIC o la instrucción de confirmación.

FORCED     
Todas las transacciones tras `SET FORCED` son de perdurabilidad diferida. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

**\<external_access_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla si recursos externos como los objetos de otra base de datos pueden tener acceso a la base de datos.

DB_CHAINING { ON | **OFF** }     
ACTIVAR     
La base de datos puede ser el origen o el destino de un encadenamiento de propiedad entre bases de datos.

Apagado     
La base de datos no puede participar en el encadenamiento de propiedad entre bases de datos.

> [!IMPORTANT]
> La instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] reconoce esta configuración si la opción del servidor cross db ownership chaining server es 0 (OFF). Si cross db ownership chaining es 1 (ON), todas las bases de datos de usuario pueden participar en cadenas de propiedad entre bases de datos, independientemente del valor de esta opción. Esta opción se establece mediante [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md).

Para establecer esta opción, se requiere el permiso de `CONTROL SERVER` en la base de datos.

La opción DB_CHAINING no se puede establecer en estas bases de datos del sistema: master, model y tempdb.

Para determinar el estado de esta opción, examine la columna `is_db_chaining_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

TRUSTWORTHY { ON | **OFF** }     
ACTIVAR     
Los módulos de base de datos (por ejemplo, las funciones definidas por el usuario o los procedimientos almacenados) que utilizan un contexto de suplantación pueden tener acceso a recursos externos a la base de datos.

Apagado     
Los módulos de base de datos en un contexto de suplantación no pueden tener acceso a recursos externos a la base de datos.

TRUSTWORTHY se establece en OFF siempre que la base de datos se adjunta.

De forma predeterminada, el valor TRUSTWORTHY se establece en OFF en todas las bases de datos de sistema, excepto msdb. No es posible cambiar este valor para las bases de datos model y tempdb. Se recomienda no establecer la opción TRUSTWORTHY en ON en la base de datos maestra.

Para establecer esta opción, se requiere el permiso de `CONTROL SERVER` en la base de datos.

Para determinar el estado de esta opción, examine la columna `is_trustworthy_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

DEFAULT_FULLTEXT_LANGUAGE     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Especifica el valor de idioma predeterminado para las columnas indizadas de texto completo.

> [!IMPORTANT]
> Esta opción solo se permite cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

DEFAULT_LANGUAGE     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Especifica el idioma predeterminado de los nuevos inicios de sesión creados. El idioma se puede especificar proporcionando el identificador local (LCID), el nombre de idioma o el alias de idioma. Para una lista de nombres y alias de idioma aceptables, consulte [sys.syslanguages](../../relational-databases/system-compatibility-views/sys-syslanguages-transact-sql.md). Esta opción solo se permite cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

NESTED_TRIGGERS     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Especifica si un desencadenador AFTER puede actuar en cascada; es decir, realizar una acción que inicia otro desencadenador que, a su vez, inicia otro desencadenador, etc. Esta opción solo se permite cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

TRANSFORM_NOISE_WORDS     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Se utiliza para suprimir un mensaje de error si las palabras irrelevantes producen un error en una operación booleana en una consulta de texto completo. Esta opción solo se permite cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

TWO_DIGIT_YEAR_CUTOFF     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Especifica un número entero comprendido entre 1753 y 9999 que representa el año límite para interpretar años de dos dígitos como años de cuatro dígitos. Esta opción solo se permite cuando CONTAINMENT se ha establecido en PARTIAL. Si CONTAINMENT se establece en NONE, se producirán errores.

**\<FILESTREAM_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Controla los valores de objetos FileTable.

NON_TRANSACTED_ACCESS = { OFF | READ_ONLY | FULL }     
Apagado     
El acceso no transaccional a los datos de FileTable está deshabilitado.

READ_ONLY     
Los procesos no transaccionales pueden leer datos FILESTREAM en los objetos FileTable de esta base de datos.

FULL     
Permite el acceso no transaccional completo a datos de FILESTREAM en objetos FileTable.

DIRECTORY_NAME = *\<directory_name>*      
Un nombre de directorio compatible con Windows. Este nombre debe ser único entre todos los nombres de directorio de nivel de base de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La comparación de unicidad no distingue mayúsculas de minúsculas, independientemente de la configuración de intercalación. Esta opción se debe establecer antes de crear un objeto FileTable en esta base de datos.

**\<HADR_options> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Consulte [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md).

**\<mixed_page_allocation_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Controla si la base de datos puede crear páginas iniciales con una extensión mixta para las primeras ocho páginas de una tabla o un índice.

MIXED_PAGE_ALLOCATION { OFF | **ON** }     
Apagado     
La base de datos siempre crea páginas iniciales mediante extensiones uniformes. OFF Es el valor predeterminado.

ACTIVAR     
La base de datos puede crear páginas iniciales mediante extensiones mixtas.

Esta opción es ON para todas las bases de datos del sistema. **tempdb** es la única base de datos del sistema que admite OFF.

**\<PARAMETERIZATION_option> ::=**      
Controla la opción de parametrización. Para obtener más información sobre la parametrización, vea la [Guía de arquitectura de procesamiento de consultas](../../relational-databases/query-processing-architecture-guide.md#SimpleParam).

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Las consultas incluyen parámetros en función del comportamiento predeterminado de la base de datos.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye parámetros para todas las consultas de la base de datos.

El valor actual de esta opción se puede determinar si examina la columna `is_parameterization_forced column` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

ON | **OFF** [ ( FORCED )  ] | CLEAR [ ALL ]     
Controla si el almacén de consultas está habilitado en esta base de datos y también controla la eliminación del contenido del almacén de consultas. Para obtener más información, vea [Escenarios de uso del Almacén de consultas](../../relational-databases/performance/query-store-usage-scenarios.md).

ACTIVAR     
Habilita el Almacén de consultas.

OFF [ ( FORCED ) ]      
Deshabilita el Almacén de consultas. OFF Es el valor predeterminado. FORCED es opcional. FORCED anula todas las tareas en segundo plano del Almacén de consultas en ejecución y omite el vaciado sincrónico cuando se desactiva el Almacén de consultas. Hace que el Almacén de consultas se apague lo más rápido posible. FORCED se aplica a SP2 CU14 de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], CU21 de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], CU6 de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y a las compilaciones posteriores.

> [!NOTE]  
> El Almacén de consultas no se puede deshabilitar en la base de datos única de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ni Grupo elástico. Al ejecutar `ALTER DATABASE [database] SET QUERY_STORE = OFF` se devuelve la advertencia `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR [ ALL ]     
Quita los datos relacionados con las consultas del Almacén de consultas. ALL es opcional. ALL quita los datos y metadatos relacionados con las consultas del Almacén de consultas.

OPERATION_MODE { READ_ONLY | READ_WRITE }     
Describe el modo de operación del Almacén de consultas.

READ_WRITE     
El Almacén de consultas recopila y continúa el plan de consultas y la información de estadística del tiempo de ejecución.

READ_ONLY     
La información se puede leer del almacén de consultas, pero no se agrega información nueva. Si se ha agotado el espacio máximo emitido del Almacén de consultas, el Almacén de consultas cambiará el modo de operación a READ_ONLY.

CLEANUP_POLICY     
Describe la directiva de retención de datos del Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS determina el número de días durante los que se conserva la información de una consulta en el Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS es de tipo **bigint**. El valor predeterminado es 30.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frecuencia con la que los datos escritos en el Almacén de consultas se conservan en el disco. Para optimizar el rendimiento, los datos recopilados por el Almacén de consultas se escriben de manera asincrónica en el disco. La frecuencia con la que se produce esta transferencia asincrónica se configura mediante el argumento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS es de tipo **bigint**. El valor predeterminado es **900** (15 minutos).

MAX_STORAGE_SIZE_MB     
Determina el espacio emitido en el Almacén de consultas. MAX_STORAGE_SIZE_MB es de tipo **bigint**. El valor predeterminado es de **100 MB** para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]). A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], el valor predeterminado es **1 GB**.

> [!NOTE]
> El límite `MAX_STORAGE_SIZE_MB` no se aplica de forma estricta. El tamaño de almacenamiento solo se comprueba cuando el almacén de consultas escribe datos en el disco. Este intervalo se establece mediante la opción `DATA_FLUSH_INTERVAL_SECONDS` o la opción **Intervalo de vaciado de datos** del cuadro de diálogo Almacén de consultas de [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. El valor predeterminado de intervalo es 900 segundos (o 15 minutos).
> Si el Almacén de consultas ha infringido el límite de `MAX_STORAGE_SIZE_MB` entre las comprobaciones de tamaño de almacenamiento, pasará al modo de solo lectura. Si `SIZE_BASED_CLEANUP_MODE` está habilitado, también se desencadena el mecanismo de limpieza para aplicar el límite de `MAX_STORAGE_SIZE_MB`.
> Una vez que se haya borrado espacio suficiente, el modo de Almacén de consultas volverá a cambiar de forma automática a lectura y escritura.

> [!IMPORTANT]
> Si cree que la captura de la carga de trabajo necesitará más de 10 GB de espacio en disco, probablemente debería replantear y optimizar la carga de trabajo para reutilizar los planes de consulta (por ejemplo, mediante la [parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) o ajustar las configuraciones del Almacén de consultas.    
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede establecer `QUERY_CAPTURE_MODE` en PERSONALIZADO para obtener un control adicional sobre la directiva de captura de consultas.

INTERVAL_LENGTH_MINUTES     
Determina el intervalo de tiempo en el que se agregan los datos de estadísticas de ejecución en tiempo de ejecución al Almacén de consultas. Para optimizar el uso del espacio, se agregan las estadísticas de ejecución en tiempo de ejecución en el almacén de estadísticas de tiempo de ejecución en una ventana de tiempo fijo. Esta ventana de tiempo fijo se configura con el argumento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES es de tipo **bigint**. El valor predeterminado es **60**.

SIZE_BASED_CLEANUP_MODE { **AUTO** | OFF }     
Controla si la limpieza se activa de forma automática cuando la cantidad total de datos se acerca al tamaño máximo.

AUTO     
La limpieza según el tamaño se activará de forma automática cuando el tamaño en disco alcance el 90 % de **MAX_STORAGE_SIZE_MB**. La limpieza según el tamaño quita primero las consultas menos caras y más antiguas. Se detiene aproximadamente en el 80 % de **MAX_STORAGE_SIZE_MB**. Este es el valor de configuración predeterminado.

Apagado     
La limpieza según el tamaño no se activará automáticamente.

SIZE_BASED_CLEANUP_MODE es de tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Designa el modo de captura de consulta que está activo. Cada modo define directivas de captura de consulta específicas.

> [!NOTE]
> Los cursores, las consultas dentro de procedimientos almacenados y las consultas compiladas de forma nativa siempre se capturan cuando el modo de captura de consulta se establece en ALL, AUTO o CUSTOM.

ALL     
Captura todas las consultas. **ALL** es el valor de configuración predeterminado para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]).

AUTO     
Captura consultas pertinentes en función del consumo de recursos y el recuento de ejecuciones. Este es el valor de configuración predeterminado para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)]) y [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ninguno     
Detiene la captura de nuevas consultas. El almacén de consultas seguirá recopilando estadísticas de compilación y tiempo de ejecución para las consultas que ya se han capturado. Use esta configuración con precaución ya que podría omitir la captura de consultas importantes.

CUSTOM     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Permite controlar las opciones de QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE es de tipo **nvarchar**. 

max_plans_per_query     
Define el número máximo de planes que se tienen para cada consulta. MAX_PLANS_PER_QUERY es del tipo **int**. El valor predeterminado es **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)]))

Controla si se capturarán estadísticas de espera por consulta.

ACTIVAR    
Se captura información de estadísticas de espera por consulta. Este es el valor de configuración predeterminado.

Apagado    
No se capturará información de estadísticas de espera por consulta.

**\<query_capture_policy_option_list> :: =**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

Controla las opciones de directiva de captura de Almacén de consultas. Excepto en STALE_CAPTURE_POLICY_THRESHOLD, estas opciones definen las condiciones OR que se tienen que dar para que las consultas se capturen en el valor definido en el umbral de la directiva de capturas obsoletas.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Define el período de intervalo de evaluación para determinar si se debe capturar una consulta. El valor predeterminado es de 1 día y puede establecerse de 1 hora a 7 días. *number* es del tipo **int**.

EXECUTION_COUNT     
Define el número de veces que se ejecuta una consulta durante el período de evaluación. El valor predeterminado es 30, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta se tendrá que ejecutar al menos 30 veces en un día para que se conserve en el almacén de consultas. EXECUTION_COUNT es del tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Define el tiempo total de CPU de compilación transcurrido que usa una consulta durante el período de evaluación. El valor predeterminado es 1000, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos un segundo de tiempo de CPU durante la compilación de consulta en un día para conservarse en el almacén de consultas. TOTAL_COMPILE_CPU_TIME_MS es del tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Define el tiempo de ejecución total de CPU transcurrido que ha utilizado una consulta durante el período de evaluación. El valor predeterminado es 100, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos 100 ms de tiempo de CPU durante la ejecución en un día para que se conserve en el almacén de consultas. TOTAL_EXECUTION_CPU_TIME_MS es del tipo **int**.

**\<recovery_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla las opciones de recuperación de base de datos y la comprobación de errores de E/S de disco.

FULL     
Proporciona una restauración completa tras un error del medio, utilizando copias de seguridad del registro de transacciones. Si un archivo de datos está dañado, la recuperación del medio puede restaurar todas las transacciones confirmadas. Para más información, consulte [Modelos de recuperación](../../relational-databases/backup-restore/recovery-models-sql-server.md).

BULK_LOGGED     
Proporciona una recuperación tras un error del medio. Combina el máximo rendimiento y la mínima cantidad de uso de espacio de registro para determinadas operaciones a gran escala o masivas. Para información sobre las operaciones que se pueden registrar mínimamente, consulte [El registro de transacciones](../../relational-databases/logs/the-transaction-log-sql-server.md). En el modelo de recuperación BULK_LOGGED, el registro de estas operaciones es mínimo. Para más información, consulte [Modelos de recuperación](../../relational-databases/backup-restore/recovery-models-sql-server.md).

SIMPLE     
Se proporciona una estrategia de copia de seguridad sencilla que utiliza un espacio de registro mínimo. Se puede volver a utilizar el espacio de registro de forma automática cuando ya no se necesite para la recuperación tras errores del servidor. Para más información, consulte [Modelos de recuperación](../../relational-databases/backup-restore/recovery-models-sql-server.md).

> [!IMPORTANT]
> El modelo de recuperación simple es más fácil de administrar que los otros dos modelos, pero a costa de un mayor riesgo de perder los datos si se daña un archivo de datos. Todos los cambios se pierden, desde la copia de seguridad de base de datos más reciente o desde la copia de seguridad diferencial de la base de datos, y se deben volver a incluir de forma manual.

El modelo de recuperación predeterminado se determina mediante el modelo de recuperación de la base de datos **model** . Para más información sobre cómo seleccionar el modelo de recuperación adecuado, consulte [Modelos de recuperación](../../relational-databases/backup-restore/recovery-models-sql-server.md).

Puede determinar el estado de esta opción mediante las columnas **recovery_model** y **recovery_model_desc** en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `Recovery` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

TORN_PAGE_DETECTION { ON | **OFF** }     
ACTIVAR     
Las páginas incompletas se pueden detectar mediante el [!INCLUDE[ssDE](../../includes/ssde-md.md)].

Apagado     
Las páginas incompletas no se pueden detectar mediante el [!INCLUDE[ssDE](../../includes/ssde-md.md)].

> [!IMPORTANT]
> La estructura de sintaxis TORN_PAGE_DETECTION ON | OFF se quitará en una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar esta estructura de sintaxis en nuevos trabajos de desarrollo y prevea modificar las aplicaciones que actualmente la utilizan. Utilice la opción PAGE_VERIFY en su lugar.

<a name="page_verify"></a> PAGE_VERIFY { **CHECKSUM** | TORN_PAGE_DETECTION | NONE }     
Detecta páginas de bases de datos dañadas por errores de ruta de E/S de disco. Los errores de ruta de acceso de E/S de disco pueden ser la causa de problemas de daños en la base de datos. Estos errores suelen producirse por errores de alimentación o errores de hardware de disco que ocurren en el momento en el que la página se escribe en disco.

CHECKSUM     
Calcula una suma de comprobación del contenido de toda la página y almacena el valor en el encabezado de página si se escribe una página en el disco. Si la página se lee desde el disco, la suma de comprobación se vuelve a calcular y se compara con el valor de suma de comprobación almacenado en el encabezado de página. Si el valor no coincide, se genera el mensaje de error 824 (indica un error de la suma de comprobación) para el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], así como para el registro de eventos de Windows. Un error de la suma de comprobación indica un problema de ruta de E/S. Para determinar la causa, es necesario investigar el hardware, los controladores de firmware, el BIOS, los controladores de filtro (por ejemplo, software antivirus) y otros componentes de ruta de E/S.

TORN_PAGE_DETECTION     
Guarda una pauta específica de 2 bits por cada sector de 512 bytes en la página de base de datos de 8 kilobytes (KB) y la almacena en el encabezado de página de la base de datos al escribir la página en el disco. Si la página se lee desde el disco, los bits rasgados almacenados en el encabezado de página se comparan con la información del sector de la página real.

Los valores no coincidentes indican que solamente se ha escrito en el disco una parte de la página. En esta situación, se genera el mensaje de error 824 (indica un error de página rasgada) tanto para el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como para el registro de eventos de Windows. Las páginas rasgadas se suelen detectar mediante la recuperación de la base de datos si se trata realmente de la escritura incompleta de una página. No obstante, otros errores de ruta de E/S pueden generar una página rasgada en cualquier momento.

Ninguno     
Las escrituras de páginas de bases de datos no generarán un valor CHECKSUM o TORN_PAGE_DETECTION. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no comprobará ninguna suma de comprobación o página rasgada durante una lectura aunque haya un valor CHECKSUM o TORN_PAGE_DETECTION en el encabezado de página.

Tenga en cuenta los siguientes puntos importantes cuando utilice la opción PAGE_VERIFY:

- El valor predeterminado es **CHECKSUM**.
- Si una base de datos de usuario o del sistema se actualiza a [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] o una versión posterior, no cambia el valor de PAGE_VERIFY (NONE o TORN_PAGE_DETECTION). Se recomienda cambiar a CHECKSUM.

    > [!NOTE]
    > En las versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la opción de base de datos PAGE_VERIFY está establecida en NONE para la base de datos tempdb y no se puede modificar. A partir de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)], el valor predeterminado para la base de datos TempDB es CHECKSUM para las nuevas instalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Al actualizar una instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el valor predeterminado sigue siendo NONE. La opción se puede modificar. Se recomienda usar CHECKSUM para la base de datos tempdb.

- Es posible que TORN_PAGE_DETECTION utilice menos recursos, pero proporciona en cambio un subconjunto mínimo de la protección de CHECKSUM.
- PAGE_VERIFY se puede configurar sin dejar la base de datos sin conexión, ni bloquearla ni impedir la simultaneidad en ella.
- CHECKSUM y TORN_PAGE_DETECTION se excluyen mutuamente. No se pueden habilitar ambas opciones al mismo tiempo.

Si se detecta un error de suma de comprobación o de página rasgada, puede realizar una recuperación mediante la restauración de los datos o una regeneración del índice, si el error se limita únicamente a las páginas de índice. Si detecta un error de suma de comprobación, ejecute DBCC CHECKDB para determinar el tipo de página o páginas de base de datos afectadas. Para más información sobre las opciones de restauración, consulte [Argumentos RESTORE](../../t-sql/statements/restore-statements-arguments-transact-sql.md). Aunque la restauración de los datos resolverá el problema de los datos dañados, es necesario diagnosticar y corregir la causa (por ejemplo, un error del hardware de disco) lo antes posible, para evitar errores continuos.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelve a intentar cualquier lectura que genere un error con una suma de comprobación, una página rasgada u otros errores de E/S, en cuatro ocasiones. Si la lectura se realiza correctamente en cualquiera de los reintentos, se escribe un mensaje en el registro de errores. El comando que desencadenó la lectura continuará. El comando genera el mensaje de error 824 si los reintentos no se realizan correctamente.

Para más información sobre los mensajes de error 823, 824 y 825, vea:

- [Cómo solucionar un error 823 Msg en SQL Server](https://support.microsoft.com/help/2015755)
- [Cómo solucionar problemas de 824 Msg en SQL Server](https://support.microsoft.com/help/2015756)
- [Cómo solucionar problemas de Msg: reintento de lectura](https://support.microsoft.com/help/2015757).

La configuración actual de esta opción se puede determinar si examina la columna `page_verify_option` de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsTornPageDetectionEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<remote_data_archive_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

Habilita o deshabilita Stretch Database para la base de datos. Para obtener más información, vea [Stretch Database](../../sql-server/stretch-database/stretch-database.md).

REMOTE_DATA_ARCHIVE = { ON ( SERVER = \<server_name> , { CREDENTIAL = \<db_scoped_credential_name> \| FEDERATED_SERVICE_ACCOUNT = ON \| OFF } )\| **OFF**     
ACTIVAR     
Habilita Stretch Database para la base de datos. Para más información, incluidos los requisitos previos adicionales, vea [Enable Stretch Database for a database](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md) (Habilitar Stretch Database para una base de datos).

Necesita el permiso `db_owner` para habilitar Stretch Database para una tabla. Necesita los permisos `db_owner` y `CONTROL DATABASE` para habilitar Stretch Database para una tabla.

SERVER = \<server_name>     
Especifica la dirección del servidor de Azure. Incluye la parte `.database.windows.net` del nombre. Por ejemplo, `MyStretchDatabaseServer.database.windows.net`.

CREDENTIAL = \<db_scoped_credential_name>     
Especifica la credencial de ámbito de base de datos que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] usa para conectarse al servidor de Azure. Asegúrese de que la credencial existe antes de ejecutar este comando. Para más información, consulte [CREATE DATABASE SCOPED CREDENTIAL](../../t-sql/statements/create-database-scoped-credential-transact-sql.md).

FEDERATED_SERVICE_ACCOUNT = { ON | OFF }     
Puede usar una cuenta de servicio federado para que el servidor SQL Server local se comunique con el servidor remoto de Azure cuando se cumplan estas condiciones.

- La cuenta de servicio con la que se está ejecutando la instancia de SQL Server es una cuenta de dominio.
- La cuenta de dominio pertenece a un dominio cuyo Active Directory está federado con Azure Active Directory.
- El servidor remoto de Azure está configurado para admitir la autenticación de Azure Active Directory.
- La cuenta de servicio con la que se ejecuta la instancia de SQL Server debe configurarse como una cuenta `dbmanager` o `sysadmin` en el servidor remoto de Azure.

Si indica que la cuenta de servicio federada está activada, no podrá especificar además el argumento CREDENTIAL. Proporcione el argumento CREDENTIAL si especifica OFF.

Apagado     
Deshabilita Stretch Database para la base de datos. Para obtener más información, vea [Deshabilitar Stretch Database y recuperar datos remotos](../../sql-server/stretch-database/disable-stretch-database-and-bring-back-remote-data.md).

Solo puede deshabilitar Stretch Database para una base de datos una vez que la base de datos ya no contenga ninguna tabla que esté habilitada para Stretch Database. Después de deshabilitar Stretch Database, la migración de datos se detiene. Además, los resultados de la consulta ya no incluyen los resultados de las tablas remotas.

Al deshabilitar Stretch no se quita la base de datos remota. Para eliminar la base de datos remota, colóquela mediante Azure Portal.

**\<service_broker_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Controla las siguientes opciones de [!INCLUDE[ssSB](../../includes/sssb-md.md)]: habilita o deshabilita la entrega de mensajes, establece un nuevo identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] o establece prioridades de conversación en ON u OFF.

ENABLE_BROKER     
Indica que se habilite [!INCLUDE[ssSB](../../includes/sssb-md.md)] para la base de datos especificada. Se inicia la entrega de mensajes y la marca is_broker_enabled se establece en TRUE en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). La base de datos conserva el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente. Service Broker no puede habilitarse mientras la base de datos sea la entidad de seguridad en una configuración de creación de reflejo de la base de datos.

> [!NOTE]
> ENABLE_BROKER requiere un bloqueo exclusivo de base de datos. Si otras sesiones tienen recursos bloqueados en la base de datos, ENABLE_BROKER esperará hasta que las demás sesiones liberen sus bloqueos. Para habilitar [!INCLUDE[ssSB](../../includes/sssb-md.md)] en una base de datos de usuario, asegúrese de que ninguna otra sesión esté utilizando la base de datos antes de ejecutar la instrucción ALTER DATABASE SET ENABLE_BROKER, por ejemplo, colocando la base de datos en modo de usuario único. Para habilitar [!INCLUDE[ssSB](../../includes/sssb-md.md)] en la base de datos msdb, detenga en primer lugar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para que [!INCLUDE[ssSB](../../includes/sssb-md.md)] pueda obtener el bloqueo necesario.

DISABLE_BROKER     
Indica que se deshabilite [!INCLUDE[ssSB](../../includes/sssb-md.md)] para la base de datos especificada. Se detiene la entrega de mensajes y la marca is_broker_enabled se establece en FALSE en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). La base de datos conserva el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente.

NEW_BROKER     
Especifica que la base de datos debe recibir un identificador de agente nuevo. La base de datos actúa como un nuevo agente de servicio. Como tal, todas las conversaciones existentes en la base de datos se quitan inmediatamente sin generar mensajes de finalización de diálogo. Cualquier ruta que haga referencia al identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] anterior se debe volver a crear con el nuevo identificador.

ERROR_BROKER_CONVERSATIONS     
Especifica que la entrega de mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)] está habilitada. Este valor conserva el identificador de [!INCLUDE[ssSB](../../includes/sssb-md.md)] existente para la base de datos. [!INCLUDE[ssSB](../../includes/sssb-md.md)] finaliza todas las conversaciones de la base de datos con un error. Este valor permite que las aplicaciones realicen una ejecución regular de las conversaciones existentes.

HONOR_BROKER_PRIORITY {ON | OFF}     
ACTIVAR     
Las operaciones de envío tienen en cuenta los niveles de prioridad asignados a las conversaciones. Los mensajes de las conversaciones que tienen niveles de prioridad altos se envían antes que los que tienen asignados niveles de prioridad bajos.

Apagado     
Las operaciones de envío se ejecutan como si todas las conversaciones tuvieran el nivel de prioridad predeterminado.

Los cambios de la opción HONOR_BROKER_PRIORITY tienen efecto inmediato para los diálogos nuevos o los que no tiene ningún mensaje en espera de ser enviado. Los diálogos con mensajes que se van a enviar cuando se ejecuta ALTER DATABASE no adoptarán el nuevo valor hasta que se envíe alguno de los mensajes del diálogo. La cantidad de tiempo que transcurre hasta que se inicien todos los diálogos con el nuevo valor puede variar considerablemente.

El valor actual de esta propiedad se notifica en la columna `is_broker_priority_honored` de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<snapshot_option> ::=**      
Calcula el nivel de aislamiento de la transacción.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ACTIVAR     
Habilita la opción de instantánea en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones pueden especificar el nivel de aislamiento de transacción SNAPSHOT. Si se ejecuta una transacción en el nivel de aislamiento SNAPSHOT, todas las instrucciones verán una instantánea de los datos tal como estaban al inicio de la transacción. Si una transacción ejecutada en el nivel de aislamiento SNAPSHOT tiene acceso a los datos de varias bases de datos, ALLOW_SNAPSHOT_ISOLATION debe establecerse en ON en todas las bases de datos o cada instrucción de la transacción debe utilizar sugerencias de bloqueo en cualquier referencia de una cláusula FROM a una tabla de una base de datos donde ALLOW_SNAPSHOT_ISOLATION sea OFF.

Apagado     
Desactiva la opción de instantánea en el nivel de base de datos. Las transacciones no pueden especificar el nivel de aislamiento de la transacción SNAPSHOT.

Si se establece ALLOW_SNAPSHOT_ISOLATION en un estado nuevo (de ON a OFF o de OFF a ON), ALTER DATABASE no devuelve el control al autor de la llamada hasta confirmar todas las transacciones existentes de la base de datos. Si la base de datos ya se encuentra en el estado especificado en la instrucción ALTER DATABASE, se devuelve de inmediato el control al autor de la llamada. Use [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) para determinar si hay transacciones de ejecución prolongada si la instrucción ALTER DATABASE no devuelve resultados rápidamente. Si se cancela la instrucción ALTER DATABASE, la base de datos permanece en el estado en que estaba al iniciar ALTER DATABASE. La vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica el estado de las transacciones de aislamiento de instantáneas en la base de datos. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF se pausará durante seis segundos y reintentará la operación.

No puede cambiar el estado de ALLOW_SNAPSHOT_ISOLATION si la base de datos está establecida en OFFLINE.

Si establece ALLOW_SNAPSHOT_ISOLATION en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece posteriormente en READ_WRITE.

Puede cambiar la configuración de ALLOW_SNAPSHOT_ISOLATION para las bases de datos maestra, model, msdb y tempdb. La configuración se mantiene cada vez que la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se detiene y se reinicia si cambia la configuración para tempdb. Si cambia la configuración para la base de datos model, dicha configuración se convierte en la configuración predeterminada para todas las bases de datos nuevas que se crean, excepto para tempdb.

La opción predeterminada es ON para las bases de datos master y msdb.

El valor actual de esta opción se puede determinar mediante el examen de la columna `snapshot_isolation_state` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ACTIVAR     
Habilita la opción de instantánea de lectura confirmada en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones que especifican el nivel de aislamiento de lectura confirmada usan versiones de fila en lugar de bloqueos. Todas las instrucciones ven una instantánea de los datos tal y como existen al inicio de la instrucción si una transacción se ejecuta en el nivel de aislamiento READ COMMITTED.

Apagado     
Desactiva la opción de instantánea de lectura confirmada en el nivel de base de datos. Las transacciones que especifican el nivel de aislamiento READ COMMITTED utilizan el bloqueo.

Para establecer READ_COMMITTED_SNAPSHOT en ON u OFF, no puede haber ninguna conexión activa a la base de datos, excepto la que ejecuta el comando ALTER DATABASE. Sin embargo, no es necesario que la base de datos esté en modo de usuario único. No puede cambiar el estado de esta opción si la base de datos está establecida en OFFLINE.

Si establece READ_COMMITTED_SNAPSHOT en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece después en READ_WRITE.

READ_COMMITTED_SNAPSHOT no se puede cambiar a ON para las bases de datos maestra, tempdb o msdb. Si cambia la configuración para model, dicha configuración se convierte en predeterminada para todas las bases de datos creadas, excepto para tempdb.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_read_committed_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Cuando se crea una tabla con **DURABILITY = SCHEMA_ONLY** y posteriormente se cambia **READ_COMMITTED_SNAPSHOT** mediante **ALTER DATABASE**, se pierden los datos de la tabla.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)])

ACTIVAR     
Cuando el nivel de aislamiento de transacción se establece en uno inferior a SNAPSHOT, todas las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria se ejecutan con aislamiento SNAPSHOT. Los ejemplos de los niveles de aislamiento inferiores a la instantánea son READ COMMITTED o READ UNCOMMITTED. Estas operaciones se ejecutan si el nivel de aislamiento de transacción se establece explícitamente en el nivel de sesión o el valor predeterminado se utiliza de forma implícita.

Apagado     
No eleva el nivel de aislamiento de transacción para las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria.

No puede cambiar el estado de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de datos está establecida en OFFLINE.

De forma predeterminada, la opción está desactivada.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_memory_optimized_elevate_to_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controla las opciones de cumplimiento con ANSI en el nivel de base de datos.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Determina el valor predeterminado, NULL o NOT NULL, de una columna o del [tipo definido por el usuario CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) para los que no se ha definido la nulabilidad explícitamente en las instrucciones CREATE TABLE o ALTER TABLE. Las columnas para las que se hayan definido restricciones siguen las reglas de las restricciones, independientemente de esta configuración.

ACTIVAR     
El valor predeterminado de una columna sin definir es NULL.

Apagado     
El valor predeterminado de una columna sin definir es NOT NULL.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada del nivel de base de datos para ANSI_NULL_DEFAULT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULL_DEFAULT en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Para la compatibilidad con ANSI, si se establece la opción de base de datos ANSI_NULL_DEFAULT en ON, el valor predeterminado cambia a NULL.

Para determinar el estado de esta opción, examine la columna `is_ansi_null_default_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullDefault` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ACTIVAR     
Todas las comparaciones con un valor NULL se evalúan como UNKNOWN.

Apagado     
Las comparaciones de valores no UNICODE con un valor NULL se evalúan como TRUE si ambos valores son NULL.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS siempre estará ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_NULLS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULLS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> El valor de SET ANSI_NULLS también debe estar en ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_ansi_nulls_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVAR     
Las cadenas se rellenan a la misma longitud antes de la conversión. También se rellenan hasta la misma longitud antes de la inserción en un tipo de datos **varchar** o **nvarchar**.

Apagado     
Inserta espacios en blanco finales en los valores de caracteres en las columnas **varchar** o **nvarchar**. También deja los ceros a la derecha en los valores binarios insertados en columnas **varbinary**. Los valores no se rellenan hasta completar la longitud de la columna.

Si se especifica OFF, esta opción solamente afecta a la definición de las columnas nuevas.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING siempre estará en ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Se recomienda establecer siempre ANSI_PADDING en ON. ANSI_PADDING también debe estar en ON al crear o tratar índices en columnas calculadas o vistas indizadas.

**char(_n_)** y **binary(_n_)** las columnas que admiten valores NULL se rellenan hasta la longitud de la columna cuando ANSI_PADDING se establece en ON. Los ceros y los espacios en blanco finales se recortan si ANSI_PADDING es OFF. Las columnas **char(_n_)** y **binary(_n_)** que no permiten valores NULL siempre se rellenan hasta completar la longitud de la columna.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada del nivel de base de datos para ANSI_PADDING. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_PADDING en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_padding_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiPaddingEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVAR     
Se emiten errores o advertencias si se dan condiciones tales como la división por cero. También se emiten errores y advertencias cuando aparecen valores null en funciones de agregado.

Apagado     
No se genera ninguna advertencia ni se devuelven valores NULL si se producen condiciones como la división por cero.

> [!IMPORTANT]
> El valor de SET ANSI_WARNINGS debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_WARNINGS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_WARNINGS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_warnings_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiWarningsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVAR     
Se finaliza una consulta cuando se produce un error de desbordamiento o de división por cero durante su ejecución.

Apagado     
Aparece un mensaje de advertencia cuando se produce uno de estos errores. La consulta, el proceso por lotes o la transacción continúa procesándose como si no se hubiera producido ningún error aunque se muestre una advertencia.

> [!IMPORTANT]
> El valor de SET ARITHABORT debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_arithabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsArithmeticAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }     

Para más información, consulte [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVAR     
El resultado de una operación de concatenación es NULL si alguno de los operandos es NULL. Por ejemplo, la concatenación de la cadena de caracteres "Esto es" y NULL devuelve el valor NULL, en lugar del valor "Esto es".

Apagado     
El valor NULL se trata como una cadena de caracteres vacía.

> [IMPORTANTE] El valor de CONCAT_NULL_YIELDS_NULL también debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.
>
> En versiones futuras de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL siempre será ON y cualquier aplicación que establezca de forma explícita la opción en OFF generará un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para CONCAT_NULL_YIELDS_NULL. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión y establecen CONCAT_NULL_YIELDS_NULL en ON para la sesión al realizar la conexión con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_concat_null_yields_null_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNullConcat` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVAR     
Las comillas dobles se pueden usar para identificadores delimitados.

Todas las cadenas delimitadas por comillas dobles se interpretan como identificadores de objetos. Los identificadores entre comillas no tienen que adaptarse a las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para identificadores. Pueden ser palabras clave e incluir caracteres que no se permiten en los identificadores de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si una comilla simple (') forma parte de la cadena literal, puede representarse mediante comillas dobles (").

Apagado     
Los identificadores no se pueden incluir entre comillas y deben seguir todas las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para los identificadores. Los literales se pueden delimitar con comillas simples o dobles.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también permite delimitar los identificadores con corchetes ([ ]). Los identificadores entre corchetes pueden usarse siempre, independientemente del valor de QUOTED_IDENTIFIER. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

Al crear una tabla, la opción QUOTED IDENTIFIER siempre se almacena como ON en los metadatos de la tabla. La opción se almacena incluso si está establecida en OFF al crear la tabla.

La configuración en el nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para QUOTED_IDENTIFIER. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión estableciendo QUOTED_IDENTIFIER en ON. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_quoted_identifier_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsQuotedIdentifiersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVAR     
Se genera un error cuando se produce una pérdida de precisión en una expresión.

Apagado     
Las pérdidas de precisión no generan un mensaje de error y el resultado se redondea con la precisión de la columna o variable que lo almacena.

> [!IMPORTANT]
> El valor de NUMERIC_ROUNDABORT debe ser OFF al crear o realizar cambios en índices de columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_numeric_roundabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNumericRoundAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVAR     
Se permite la activación recursiva de desencadenadores AFTER.

Apagado     
Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La recursividad directa solo se evita cuando RECURSIVE_TRIGGERS se establece en OFF. Para deshabilitar la recursividad indirecta, también debe establecer la opción de servidor desencadenadores anidados en 0.

Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)])

Especifica la frecuencia de puntos de comprobación indirectos por base de datos. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el valor predeterminado para nuevas bases de datos es de **un minuto**, lo cual indica que la base de datos usará puntos de comprobación indirectos. Para versiones anteriores, el valor predeterminado es 0, lo cual indica que la base de datos usará puntos de comprobación automáticos, cuya frecuencia depende del valor de intervalo de recuperación de la instancia de servidor. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda un minuto para la mayoría de los sistemas.

TARGET_RECOVERY_TIME **=** *target_recovery_time* { SECONDS | MINUTES }     
*target_recovery_time*     
Especifica el límite máximo de tiempo para recuperar la base de datos especificada en caso de bloqueo. *target_recovery_time* es del tipo **int**.

SECONDS     
Indica que *target_recovery_time* se expresa como el número de segundos.

MINUTES     
Indica que *target_recovery_time* se expresa como el número de minutos.

Para más información sobre los puntos de control indirectos, consulte [Puntos de control de base de datos](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Especifica el momento en que se revierten las transacciones incompletas cuando la base de datos pasa de un estado a otro. Si se omite la cláusula de terminación, la instrucción ALTER DATABASE espera indefinidamente a que se produzca un bloqueo en la base de datos. Solamente se puede especificar una cláusula de terminación y debe seguir a las cláusulas SET.

> [!NOTE]
> No todas las opciones de base de datos usan la cláusula WITH \<termination>. Para más información, vea la tabla ubicada en "[Opciones de configuración](#SettingOptions) de la sección "Comentarios" de este artículo.

ROLLBACK AFTER *number* [SECONDS] | ROLLBACK IMMEDIATE     
Especifica si la operación de reversión se ejecuta transcurrido un número de segundos determinado o de forma inmediata. *number* es del tipo **int**.

NO_WAIT     
Especifica que la solicitud producirá un error si el cambio de opción o estado de la base de datos solicitado no puede completarse inmediatamente. Completarse inmediatamente significa que no se espera a que las transacciones se confirmen o reviertan por su cuenta.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuración de opciones
Para recuperar la configuración actual de las opciones de base de datos, use la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Una vez configurada una opción de la base de datos, la nueva configuración surte efecto de inmediato.

Puede cambiar los valores predeterminados de cualquiera de las opciones de las bases de datos recién creadas. Para ello, cambie la opción adecuada de base de datos en la base de datos de modelo.

No todas las opciones de base de datos utilizan la cláusula WITH \<termination> ni se pueden especificar en combinación con otras opciones. En la siguiente tabla se incluyen estas opciones, su estado y el estado de terminación.

|Categoría de opciones|Se puede especificar con otras opciones|Puede utilizar la cláusula WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<db_state_option>|Sí|Sí|
|\<db_user_access_option>|Sí|Sí|
|\<db_update_option>|Sí|Sí|
|\<delayed_durability_option>|Sí|Sí|
|\<external_access_option>|Sí|No|
|\<cursor_option>|Sí|No|
|\<auto_option>|Sí|No|
|\<sql_option>|Sí|No|
|\<recovery_option>|Sí|No|
|\<target_recovery_time_option>|No|Sí|
|\<database_mirroring_option>|No|No|
|ALLOW_SNAPSHOT_ISOLATION|No|No|
|READ_COMMITTED_SNAPSHOT|No|Sí|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Sí|Sí|
|\<service_broker_option>|Sí|No|
|DATE_CORRELATION_OPTIMIZATION|Sí|Sí|
|\<parameterization_option>|Sí|Sí|
|\<change_tracking_option>|Sí|Sí|
|\<db_encryption_option>|Sí|No|

La memoria caché de planes para la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se borra si se establece alguna de las opciones siguientes:

:::row:::
    :::column:::
        OFFLINE
    :::column-end:::
    :::column:::
        ONLINE
    :::column-end:::
    :::column:::
        MODIFY_NAME
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        COLLATE
    :::column-end:::
    :::column:::
        READ_ONLY
    :::column-end:::
    :::column:::
        READ_WRITE
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        MODIFY FILEGROUP DEFAULT
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_WRITE
    :::column-end:::
    :::column:::
        MODIFY FILEGROUP READ_ONLY
    :::column-end:::
:::row-end:::

La caché de planes también se vacía en los escenarios siguientes.

- Una base de datos tiene la opción de base de datos AUTO_CLOSE establecida en ON. Cuando ninguna conexión de usuario hace referencia a la base de datos ni la usa, la tarea de segundo plano intenta cerrar la base de datos y apagarla de modo automático.
- Ejecuta varias consultas con una base de datos que tiene opciones predeterminadas. Después, la base de datos se quita.
- Se quita una instantánea de base de datos para una base de datos de origen.
- Volvió a generar correctamente el registro de transacciones para una base de datos.
- Restaura una copia de seguridad de una base de datos
- Separa una base de datos.

Al borrar la memoria caché de planes, se provoca una nueva compilación de todos los planes de ejecución posteriores y puede ocasionar una disminución repentina y temporal del rendimiento de las consultas. Para cada almacén de caché borrado de la caché de planes, el registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene el siguiente mensaje informativo: `SQL Server has encountered %d occurrence(s) of cachestore flush for the '%s' cachestore (part of plan cache) due to some database maintenance or reconfigure operations`. Este mensaje se registra cada cinco minutos siempre que se vacíe la memoria caché dentro de ese intervalo de tiempo.

## <a name="examples"></a>Ejemplos

### <a name="a-setting-options-on-a-database"></a>A. Configurar opciones en una base de datos

En el siguiente ejemplo se establece el modelo de recuperación y las opciones de comprobación de páginas de datos para la base de datos de ejemplo [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RECOVERY FULL PAGE_VERIFY CHECKSUM;
GO

```

### <a name="b-setting-the-database-to-read_only"></a>B. Establecer la base de datos en READ_ONLY

El cambio del estado de una base de datos o un grupo de archivos a READ_ONLY o READ_WRITE requiere el acceso exclusivo a la base de datos. En el siguiente ejemplo la base de datos se establece en el modo `SINGLE_USER` para obtener acceso exclusivo. A continuación, el ejemplo establece el estado de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en `READ_ONLY` y devuelve el acceso a la base de datos a todos los usuarios.

> [!NOTE]
> En este ejemplo se utiliza la opción de terminación `WITH ROLLBACK IMMEDIATE` en la primera instrucción `ALTER DATABASE` . Todas las transacciones incompletas se revierten y las restantes conexiones con la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] se desconectan de inmediato.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET SINGLE_USER
WITH ROLLBACK IMMEDIATE;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="c-enabling-snapshot-isolation-on-a-database"></a>C. Habilitar el aislamiento de instantánea en una base de datos

En el siguiente ejemplo se habilita la opción del marco de aislamiento de instantánea para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO

```

El conjunto de resultados muestra que el marco de aislamiento de instantánea está habilitado.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ACTIVAR |

### <a name="d-enabling-modifying-and-disabling-change-tracking"></a>D. Habilitar, modificar y deshabilitar el seguimiento de cambios

En el ejemplo siguiente se habilita el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y se establece el período de retención en `2` días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

En el ejemplo siguiente se muestra cómo cambiar el período de retención a `3` días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

En el ejemplo siguiente se muestra cómo deshabilitar el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="e-enabling-the-query-store"></a>E. Habilitar el Almacén de consultas

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)])

En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="f-enabling-the-query-store-with-wait-statistics"></a>F. Habilitar el Almacén de consultas con las estadísticas de espera

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)])

En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="g-enabling-the-query-store-with-custom-capture-policy-options"></a>G. Habilitar el Almacén de consultas con las opciones de directiva de captura personalizadas

**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)])

En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
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

## <a name="see-also"></a>Consulte también

- [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [ALTER DATABASE SET HADR](../../t-sql/statements/alter-database-transact-sql-set-hadr.md)
- [estadísticas](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Habilitar y deshabilitar el seguimiento de cambios](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Database \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="sql-database"></a>SQL Database

Los niveles de compatibilidad son opciones de `SET`, pero se describe en [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> Es posible configurar muchas opciones SET de la base de datos para la sesión actual mediante [Instrucciones SET](../../t-sql/statements/set-statements-transact-sql.md), aunque generalmente las configuran las aplicaciones al realizar la conexión. Las opciones SET de nivel de sesión reemplazan a los valores **ALTER DATABASE SET**. Las opciones de base de datos descritas en las secciones siguientes son valores que se pueden establecer para sesiones que no proporcionan de forma explícita otros valores de opciones SET.

## <a name="syntax"></a>Sintaxis

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <option_spec> [ ,...n ] [ WITH <termination> ]
}
;

<option_spec> ::=
{
    <auto_option>
  | <automatic_tuning_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <db_update_option>
  | <db_user_access_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }
  | AUTOMATIC_TUNING ( CREATE_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( DROP_INDEX = { DEFAULT | ON | OFF } )
  | AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
        = OFF
      | = ON [ ( <change_tracking_option_list > [,...n] ) ]
      | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<db_update_option> ::=
  { READ_ONLY | READ_WRITE }

<db_user_access_option> ::=
  { RESTRICTED_USER | MULTI_USER }

<delayed_durability_option> ::= DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
      = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<termination>::=
{
    ROLLBACK AFTER integer [ SECONDS ]
  | ROLLBACK IMMEDIATE
  | NO_WAIT
}

<temporal_history_retention>::=TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argumentos

*database_name*     
Es el nombre de la base de datos que se va a modificar.

CURRENT     
`CURRENT` realiza la acción en la base de datos actual. `CURRENT` no se admite para todas las opciones en todos los contextos. Si `CURRENT` produce un error, proporcione el nombre de la base de datos.

**\<auto_option> ::=**      
Controla las opciones automáticas.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { ON | OFF }     
ACTIVAR     
El optimizador de consultas crea estadísticas en columnas únicas de los predicados de consulta, según sea necesario, para mejorar los planes de consulta y el rendimiento de las consultas. Estas estadísticas de columna única se crean cuando el optimizador de consultas las compila. Las estadísticas de columna única solamente se crean en las columnas que ya no son la primera columna de un objeto de estadísticas existente.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Apagado     
El optimizador de consultas no crea las estadísticas en columnas únicas de los predicados de consulta cuando compila las consultas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna `is_auto_create_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoCreateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Opciones de estadísticas" en [Estadísticas](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Establezca AUTO_CREATE_STATISTICS en ON y establezca INCREMENTAL en ON. Esta configuración crea automáticamente estadísticas como incrementales siempre que se admitan estadísticas incrementales. El valor predeterminado es OFF. Para más información, consulte [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVAR     
Los archivos de la base de datos se pueden reducir periódicamente.

Pueden reducirse automáticamente los archivos de datos y los archivos de registro. AUTO_SHRINK reduce el tamaño del registro de transacciones solo si establece la base de datos en el modelo de recuperación SIMPLE o si realiza una copia de seguridad del registro. Cuando se establece en OFF, los archivos de la base de datos no se reducen de forma automática durante las comprobaciones periódicas del espacio no utilizado.

La opción AUTO_SHRINK reduce los archivos cuando no se utiliza más de un 25% del espacio del archivo. La opción hace que el archivo se reduzca a uno de dos tamaños. Se reduce al que sea más grande de los siguientes:

- El tamaño donde el 25 por ciento del archivo es espacio no utilizado
- El tamaño del archivo cuando se creó

No puede reducir una base de datos de solo lectura.

Apagado     
Los archivos de la base de datos no se reducen automáticamente durante las comprobaciones periódicas del espacio no utilizado.

Para determinar el estado de esta opción, examine la columna `is_auto_shrink_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoShrink` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La opción AUTO_SHRINK no está disponible en una base de datos independiente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVAR     
Especifica que el optimizador de consultas actualiza las estadísticas cuando las usa una consulta y puedan estar obsoletas. Las estadísticas se vuelven obsoletas después de que operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indizada. Para determinar cuándo las estadísticas pueden quedar obsoletas, el optimizador de consultas cuenta el número de modificaciones de datos desde la actualización más reciente de las estadísticas y compara el número de modificaciones con un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.

El optimizador de consultas comprueba si hay estadísticas obsoletas antes de compilar una consulta y ejecuta un plan de consulta almacenado en caché. El optimizador de consultas usa las columnas, tablas y vistas indexadas del predicado de consulta para determinar qué estadísticas podrían estar obsoletas. El optimizador de consultas determina esta información antes de compilar una consulta. Antes de ejecutar un plan de consulta almacenado en caché, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba que el plan de consulta haga referencia a las estadísticas actualizadas.

La opción AUTO_UPDATE_STATISTICS se aplica a las estadísticas creadas para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción CREATE STATISTICS. Esta opción también se aplica a las estadísticas filtradas.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Utilice la opción AUTO_UPDATE_STATISTICS_ASYNC para especificar si las estadísticas se actualizan sincrónica o asincrónicamente.

Apagado     
Especifica que el optimizador de consultas no actualiza las estadísticas cuando una consulta las usa. El optimizador de consultas tampoco actualiza las estadísticas cuando podrían estar obsoletas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna `is_auto_update_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoUpdateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Opciones de estadísticas" en [Estadísticas](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVAR     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son asincrónicas. El optimizador de consultas no espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

La configuración de esta opción en ON no surte efecto a menos que AUTO_UPDATE_STATISTICS se establezca en ON.

De forma predeterminada, la opción AUTO_UPDATE_STATISTICS_ASYNC se establece en OFF y el optimizador de consultas actualiza las estadísticas de manera sincrónica.

Apagado     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son sincrónicas. El optimizador de consultas espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

La configuración de esta opción en OFF no surte efecto a menos que AUTO_UPDATE_STATISTICS esté configurado en ON.

Para determinar el estado de esta opción, examine la columna `is_auto_update_stats_async_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Para más información sobre cuándo usar las actualizaciones de estadísticas sincrónicas o asincrónicas, vea la sección "Opciones de estadísticas" en [Estadísticas](../../relational-databases/statistics/statistics.md#statistics-options).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Se aplica a**: [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Controla las opciones automáticas para el [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md).

AUTOMATIC_TUNING = { AUTO | INHERIT | CUSTOM }     
AUTO     
Si el valor de ajuste automático se establece en AUTO, se aplicarán los valores predeterminados de configuración de Azure al ajuste automático.

INHERIT     
Si se usa el valor INHERIT, se hereda la configuración predeterminada del servidor primario. Esto es especialmente útil si quiere personalizar la configuración de ajuste automático en un servidor primario y que todas las bases de datos del servidor hereden esta configuración personalizada. Tenga en cuenta que, para que la herencia funcione correctamente, las tres opciones de ajuste individuales, FORCE_LAST_GOOD_PLAN, CREATE_INDEX y DROP_INDEX, deben estar configuradas en DEFAULT en las bases de datos.

CUSTOM     
Si usa el valor CUSTOM, tendrá que configurar manualmente todas las opciones de ajuste automático disponibles en la base de datos.

Habilita o deshabilita la opción `CREATE_INDEX` de administración de índices automática del [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md).

CREATE_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Hereda la configuración predeterminada del servidor. En este caso, las opciones para habilitar o deshabilitar las características de Ajuste automático individuales están definidas en el nivel de servidor.

ACTIVAR     
Cuando se habilita, se generan automáticamente los índices que faltan en una base de datos. Después de la creación del índice, se comprueban las mejoras de rendimiento de la carga de trabajo. Cuando el índice creado ya no proporciona ventajas en el rendimiento de la carga de trabajo, se revierte automáticamente. Los índices creados automáticamente se marcan como un índice generado por el sistema.

Apagado     
No genera automáticamente índices que faltan en la base de datos.

Habilita o deshabilita la opción `DROP_INDEX` de administración de índices automática del [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md).

DROP_INDEX = { DEFAULT | ON | OFF }     
DEFAULT     
Hereda la configuración predeterminada del servidor. En este caso, las opciones para habilitar o deshabilitar las características de Ajuste automático individuales están definidas en el nivel de servidor.

ACTIVAR     
Quita de forma automática los índices duplicados o que ya no son útiles para la carga de trabajo de rendimiento.

Apagado     
No quita automáticamente índices que faltan en la base de datos.

Habilita o deshabilita la opción `FORCE_LAST_GOOD_PLAN` de corrección de planes automática del [ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md).

FORCE_LAST_GOOD_PLAN = { DEFAULT | ON | OFF }     
DEFAULT     
Hereda la configuración predeterminada del servidor. En este caso, las opciones para habilitar o deshabilitar las características de Ajuste automático individuales están definidas en el nivel de servidor.

ACTIVAR     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] fuerza automáticamente el último buen plan conocido en las consultas de [!INCLUDE[tsql-md](../../includes/tsql-md.md)], donde el nuevo plan de consulta provoca regresiones de rendimiento. El parámetro [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervisa continuamente el rendimiento de la consulta [!INCLUDE[tsql-md](../../includes/tsql-md.md)] con el plan forzado. Si hay mejoras de rendimiento, el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] seguirá usando el último buen plan conocido. Si no se detectan mejoras de rendimiento, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] generará un nuevo plan de consulta. Se producirá un error en la instrucción si el almacén de consultas no está habilitado o si no está en modo de *lectura y escritura*.

Apagado     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] informa de posibles regresiones de rendimiento de consultas provocadas por cambios del plan de consulta en la vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md), aunque estas recomendaciones no se aplican automáticamente. Para supervisar las recomendaciones activas y corregir los problemas identificados, los usuarios pueden aplicar los scripts de [!INCLUDE[tsql-md](../../includes/tsql-md.md)] que se muestran en la vista. Este es el valor predeterminado.

**\<change_tracking_option> ::=**      
Controla las opciones de seguimiento de cambios. Puede habilitar el seguimiento de cambios, establecer y cambiar opciones, y deshabilitar el seguimiento de cambios. Para obtener ejemplos, vea la sección "Ejemplos" más adelante en este artículo.

ACTIVAR     
Habilita el seguimiento de cambios para la base de datos. Si habilita el seguimiento de cambios, también puede establecer las opciones AUTO CLEANUP y CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ACTIVAR     
La información sobre el seguimiento de cambios se quita de forma automática después del período de retención especificado.

Apagado     
Los datos del seguimiento de cambios no se quitan de la base de datos.

CHANGE_RETENTION = *período_de_retención* { **DAYS** | HOURS | MINUTES }     
Especifica el período mínimo para mantener la información del seguimiento de cambios en la base de datos. Los datos solamente se quitan cuando el valor AUTO_CLEANUP es ON.

*retention_period* es un entero que especifica el componente numérico del período de retención.

El período de retención predeterminado es de **2 días**. El período de retención mínimo es de 1 minuto. El tipo de retención predeterminado es **DAYS**.

Apagado     
Deshabilita el seguimiento de cambios para la base de datos. Deshabilite el seguimiento de cambios en todas las tablas para poder deshabilitarlo en la base de datos.

**\<cursor_option> ::=**      
Controla las opciones del cursor.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVAR     
Todos los cursores abiertos cuando confirma o deshace una transacción se cierran.

Apagado     
Los cursores permanecen abiertos cuando se confirma una transacción. Cuando se revierte se cierran todos los cursores, excepto los que están definidos como INSENSITIVE o STATIC.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada de la base de datos para CURSOR_CLOSE_ON_COMMIT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión que establece CURSOR_CLOSE_ON_COMMIT en OFF para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_cursor_close_on_commit_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsCloseCursorsOnCommitEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). El cursor se desasigna implícitamente solamente cuando se realiza la desconexión. Para más información, consulte [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Controla el estado del cifrado de la base de datos.

ENCRYPTION { ON | OFF }     
Establece que se cifre (ON) o no se cifre (OFF) la base de datos. Para más información sobre el cifrado de base de datos, consulte [Cifrado de datos transparente](../../relational-databases/security/encryption/transparent-data-encryption.md) y [Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Cuando el cifrado está habilitado en el nivel de la base de datos, se cifrarán todos los grupos de archivos. Todos los grupos de archivos nuevos heredarán la propiedad de cifrado. Si en la base de datos hay grupos de archivos establecidos en READ ONLY, se producirá un error en la operación de cifrado de la base de datos.

Puede ver el estado del cifrado de la base de datos mediante la vista de administración dinámica [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Controla si se permiten las actualizaciones en la base de datos.

READ_ONLY     
Los usuarios pueden leer datos de la base de datos, pero no pueden modificarlos.

> [!NOTE]
> Para mejorar el rendimiento de las consultas, actualice las estadísticas antes de establecer una base de datos en READ_ONLY. Si se necesitan estadísticas adicionales después de establecer una base de datos en READ_ONLY, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] creará las estadísticas en tempdb. Para más información sobre las estadísticas para una base de datos de solo lectura, vea [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de datos está disponible para operaciones de lectura y escritura.

Para cambiar este estado, debe tener acceso exclusivo a la base de datos. Para obtener más información, vea la cláusula SINGLE_USER.

> [!NOTE]
> En las bases de datos federadas de [!INCLUDE[ssazure_md](../../includes/ssazure_md.md)], `SET { READ_ONLY | READ_WRITE }` está deshabilitado.

**\<db_user_access_option> ::=**      
Controla el acceso del usuario a la base de datos.

RESTRICTED_USER     
Solo permite a los miembros del rol fijo de base de datos `db_owner` y de los roles fijos de servidor `dbcreator` y `sysadmin` conectarse a la base de datos, pero no limita la cantidad de miembros. Todas las conexiones a la base de datos se desconectan en el margen de tiempo especificado por la cláusula de terminación de la instrucción ALTER DATABASE. Una vez que la base de datos ha cambiado al estado RESTRICTED_USER, se rechazan los intentos de conexión por parte de usuarios no cualificados. **RESTRICTED_USER** no se puede modificar con SQL Managed Instance.

MULTI_USER     
Todos los usuarios que tengan los permisos correspondientes pueden conectarse a la base de datos.

Para determinar el estado de esta opción, examine la columna `user_access`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `UserAccess` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Controla si las transacciones se confirman con perdurabilidad total o diferida.

DISABLED     
Todas las transacciones tras `SET DISABLED` son totalmente perdurables. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

ALLOWED     
Todas las transacciones tras `SET ALLOWED` son totalmente perdurables o de perdurabilidad diferida, dependiendo de la opción de perdurabilidad establecida en el bloque ATOMIC o la instrucción de confirmación.

FORCED     
Todas las transacciones tras `SET FORCED` son de perdurabilidad diferida. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

**\<PARAMETERIZATION_option> ::=**      
Controla la opción de parametrización.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Las consultas incluyen parámetros en función del comportamiento predeterminado de la base de datos.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye parámetros para todas las consultas de la base de datos.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_parameterization_forced` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Controla si el almacén de consultas está habilitado en esta base de datos y también controla la eliminación del contenido del almacén de consultas.

ACTIVAR     
Habilita el Almacén de consultas. ON es el valor predeterminado.

Apagado     
Deshabilita el Almacén de consultas. 

> [!NOTE]  
> El Almacén de consultas no se puede deshabilitar en la base de datos única de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] ni Grupo elástico. Al ejecutar `ALTER DATABASE [database] SET QUERY_STORE = OFF` se devuelve la advertencia `'QUERY_STORE=OFF' is not supported in this version of SQL Server.`. 

CLEAR     
Quita el contenido del Almacén de consultas.

OPERATION_MODE     
Describe el modo de operación del Almacén de consultas. Los valores válidos son READ_ONLY y READ_WRITE. En el modo READ_WRITE, el Almacén de consultas recopila y continúa el plan de consultas y la información de estadística del tiempo de ejecución. En el modo READ_ONLY, la información se puede leer del almacén de consultas, pero no se agrega información nueva. Si se ha agotado el espacio máximo del Almacén de consultas, el Almacén de consultas cambiará el modo de operación a READ_ONLY.

CLEANUP_POLICY     
Describe la directiva de retención de datos del Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS determina el número de días durante los que se conserva la información de una consulta en el Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS es de tipo **bigint**. El valor predeterminado es 30. En la edición [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, el valor predeterminado es **7** días.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frecuencia con la que los datos escritos en el Almacén de consultas se conservan en el disco. Para optimizar el rendimiento, los datos recopilados por el Almacén de consultas se escriben de manera asincrónica en el disco. La frecuencia con la que se produce esta transferencia asincrónica se configura mediante el argumento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS es de tipo **bigint**. El valor predeterminado es **900** (15 minutos).

MAX_STORAGE_SIZE_MB     
Determina el espacio asignado al Almacén de consultas. MAX_STORAGE_SIZE_MB es de tipo **bigint**. En la edición [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Premium, el valor predeterminado es **1 GB** y en la edición [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, el valor predeterminado es **10 MB**.

> [!NOTE]
> El límite del valor de `MAX_STORAGE_SIZE_MB` es de 10.240 MB en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. 

> [!NOTE]
> El límite `MAX_STORAGE_SIZE_MB` no se aplica de forma estricta. El tamaño de almacenamiento solo se comprueba cuando el almacén de consultas escribe datos en el disco. Este intervalo se establece mediante la opción `DATA_FLUSH_INTERVAL_SECONDS` o la opción **Intervalo de vaciado de datos** del cuadro de diálogo Almacén de consultas de [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. El valor predeterminado de intervalo es 900 segundos (o 15 minutos).
> Si el Almacén de consultas ha infringido el límite de `MAX_STORAGE_SIZE_MB` entre las comprobaciones de tamaño de almacenamiento, pasará al modo de solo lectura. Si `SIZE_BASED_CLEANUP_MODE` está habilitado, también se desencadena el mecanismo de limpieza para aplicar el límite de `MAX_STORAGE_SIZE_MB`.
> Una vez que se haya borrado espacio suficiente, el modo de Almacén de consultas volverá a cambiar de forma automática a lectura y escritura.

> [!IMPORTANT]
> Si cree que la captura de la carga de trabajo necesitará más de 10 GB de espacio en disco, probablemente debería replantear y optimizar la carga de trabajo para reutilizar los planes de consulta (por ejemplo, mediante la [parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) o ajustar las configuraciones del Almacén de consultas.    
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede establecer `QUERY_CAPTURE_MODE` en PERSONALIZADO para obtener un control adicional sobre la directiva de captura de consultas.

INTERVAL_LENGTH_MINUTES     
Determina el intervalo de tiempo en el que se agregan los datos de estadísticas de ejecución en tiempo de ejecución al Almacén de consultas. Para optimizar el uso del espacio, se agregan las estadísticas de ejecución en tiempo de ejecución en el almacén de estadísticas de tiempo de ejecución en una ventana de tiempo fijo. Esta ventana de tiempo fijo se configura con el argumento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES es de tipo **bigint**. El valor predeterminado es **60**.

SIZE_BASED_CLEANUP_MODE     
Controla si la limpieza se activará de forma automática cuando la cantidad total de datos se acerque al tamaño máximo.

Apagado     
La limpieza según el tamaño no se activará automáticamente.

AUTO     
La limpieza según el tamaño se activará automáticamente cuando el tamaño en disco alcance el 90 % de **max_storage_size_mb**. La limpieza según el tamaño quita primero las consultas menos caras y más antiguas. Se detiene aproximadamente en el 80 % de **max_storage_size_mb**. Es el valor de configuración predeterminado.

SIZE_BASED_CLEANUP_MODE es de tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Designa el modo de captura de consulta que está activo. Cada modo define directivas de captura de consulta específicas.   

> [!NOTE]
> Los cursores, las consultas dentro de procedimientos almacenados y las consultas compiladas de forma nativa siempre se capturan cuando el modo de captura de consulta se establece en ALL, AUTO o CUSTOM.

ALL     
Captura todas las consultas.

AUTO     
Captura consultas pertinentes en función del consumo de recursos y el recuento de ejecuciones. Es el valor de configuración predeterminado para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ninguno     
Detiene la captura de nuevas consultas. El almacén de consultas seguirá recopilando estadísticas de compilación y tiempo de ejecución para las consultas que ya se han capturado. Use esta configuración con precaución ya que podría omitir la captura de consultas importantes.

CUSTOM     
Permite controlar las opciones de QUERY_CAPTURE_POLICY.

QUERY_CAPTURE_MODE es de tipo **nvarchar**.

max_plans_per_query     
Define el número máximo de planes que se tienen para cada consulta. MAX_PLANS_PER_QUERY es del tipo **int**. El valor predeterminado es **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Controla si se capturarán estadísticas de espera por consulta.

ACTIVAR    
Se captura información de estadísticas de espera por consulta. Este es el valor de configuración predeterminado.

Apagado    
No se capturará información de estadísticas de espera por consulta.

**\<query_capture_policy_option_list> :: =**      
Controla las opciones de directiva de captura de Almacén de consultas. Excepto en STALE_CAPTURE_POLICY_THRESHOLD, estas opciones definen las condiciones OR que se tienen que dar para que las consultas se capturen en el valor definido en el umbral de la directiva de capturas obsoletas.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Define el período de intervalo de evaluación para determinar si se debe capturar una consulta. El valor predeterminado es de 1 día y puede establecerse de 1 hora a 7 días. *number* es del tipo **int**.

EXECUTION_COUNT     
Define el número de veces que se ejecuta una consulta durante el período de evaluación. El valor predeterminado es 30, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta se tendrá que ejecutar al menos 30 veces en un día para que se conserve en el almacén de consultas. EXECUTION_COUNT es del tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Define el tiempo total de CPU de compilación transcurrido que usa una consulta durante el período de evaluación. El valor predeterminado es 1000, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos un segundo de tiempo de CPU durante la compilación de consulta en un día para conservarse en el almacén de consultas. TOTAL_COMPILE_CPU_TIME_MS es del tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Define el tiempo de ejecución total de CPU transcurrido que ha utilizado una consulta durante el período de evaluación. El valor predeterminado es 100, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos 100 ms de tiempo de CPU durante la ejecución en un día para que se conserve en el almacén de consultas. TOTAL_EXECUTION_CPU_TIME_MS es del tipo **int**.

**\<snapshot_option> ::=**      
Determina el nivel de aislamiento de transacción.

ALLOW_SNAPSHOT_ISOLATION { ON | **OFF** }     
ACTIVAR     
Habilita la opción de instantánea en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones pueden especificar el nivel de aislamiento de transacción SNAPSHOT. Si se ejecuta una transacción en el nivel de aislamiento SNAPSHOT, todas las instrucciones verán una instantánea de los datos tal como estaban al inicio de la transacción. Si una transacción ejecutada en el nivel de aislamiento SNAPSHOT tiene acceso a los datos de varias bases de datos, ALLOW_SNAPSHOT_ISOLATION debe establecerse en ON en todas las bases de datos o cada instrucción de la transacción debe utilizar sugerencias de bloqueo en cualquier referencia de una cláusula FROM a una tabla de una base de datos donde ALLOW_SNAPSHOT_ISOLATION sea OFF.

Apagado     
Desactiva la opción de instantánea en el nivel de base de datos. Las transacciones no pueden especificar el nivel de aislamiento de la transacción SNAPSHOT.

Si se establece ALLOW_SNAPSHOT_ISOLATION en un estado nuevo (de ON a OFF o de OFF a ON), ALTER DATABASE no devuelve el control al autor de la llamada hasta confirmar todas las transacciones existentes de la base de datos. Si la base de datos ya se encuentra en el estado especificado en la instrucción ALTER DATABASE, se devuelve de inmediato el control al autor de la llamada. Use [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) para determinar si hay transacciones de ejecución prolongada si la instrucción ALTER DATABASE no devuelve resultados rápidamente. Si se cancela la instrucción ALTER DATABASE, la base de datos permanece en el estado en que estaba al iniciar ALTER DATABASE. La vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica el estado de las transacciones de aislamiento de instantáneas en la base de datos. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF se pausará durante seis segundos y reintentará la operación.

No puede cambiar el estado de ALLOW_SNAPSHOT_ISOLATION si la base de datos está establecida en OFFLINE.

Si establece ALLOW_SNAPSHOT_ISOLATION en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece posteriormente en READ_WRITE.

El valor actual de esta opción se puede determinar mediante el examen de la columna `snapshot_isolation_state` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | OFF }     
ACTIVAR     
Habilita la opción de instantánea de lectura confirmada en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones que especifican el nivel de aislamiento READ COMMITTED usan versiones de fila en lugar de bloqueos. Todas las instrucciones ven una instantánea de los datos tal y como existen al inicio de la instrucción si una transacción se ejecuta en el nivel de aislamiento READ COMMITTED.

Apagado     
Desactiva la opción de instantánea de lectura confirmada en el nivel de base de datos. Las transacciones que especifican el nivel de aislamiento READ COMMITTED utilizan el bloqueo.

Para establecer READ_COMMITTED_SNAPSHOT en ON u OFF, no puede haber ninguna conexión activa a la base de datos, excepto la que ejecuta el comando ALTER DATABASE. Sin embargo, no es necesario que la base de datos esté en modo de usuario único. No puede cambiar el estado de esta opción si la base de datos está establecida en OFFLINE.

Si establece READ_COMMITTED_SNAPSHOT en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece después en READ_WRITE.

READ_COMMITTED_SNAPSHOT no se puede cambiar a ON para las bases de datos maestra, tempdb o msdb. Si cambia la configuración para model, dicha configuración se convierte en predeterminada para todas las bases de datos creadas, excepto para tempdb.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_read_committed_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Cuando se crea una tabla con `DURABILITY = SCHEMA_ONLY` y posteriormente se cambia **READ_COMMITTED_SNAPSHOT** con `ALTER DATABASE`, se pierden los datos de la tabla.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ACTIVAR     
Cuando el nivel de aislamiento de transacción se establece en uno inferior a SNAPSHOT, todas las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria se ejecutan con aislamiento SNAPSHOT. Los ejemplos de los niveles de aislamiento inferiores a la instantánea son READ COMMITTED o READ UNCOMMITTED. Estas operaciones se ejecutan si el nivel de aislamiento de transacción se establece explícitamente en el nivel de sesión o el valor predeterminado se utiliza de forma implícita.

Apagado     
No eleva el nivel de aislamiento de transacción para las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria.

No puede cambiar el estado de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de datos está establecida en OFFLINE.

El valor predeterminado es OFF.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_memory_optimized_elevate_to_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controla las opciones de cumplimiento con ANSI en el nivel de base de datos.

ANSI_NULL_DEFAULT { ON | **OFF** }     
Determina el valor predeterminado, NULL o NOT NULL, de una columna o del [tipo definido por el usuario CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) para los que no se ha definido la nulabilidad explícitamente en las instrucciones CREATE TABLE o ALTER TABLE. Las columnas para las que se hayan definido restricciones siguen las reglas de las restricciones, independientemente de esta configuración.

ACTIVAR     
El valor predeterminado es NULL.

Apagado     
El valor predeterminado es NOT NULL.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada del nivel de base de datos para ANSI_NULL_DEFAULT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULL_DEFAULT en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Para la compatibilidad con ANSI, si se establece la opción de base de datos ANSI_NULL_DEFAULT en ON, el valor predeterminado cambia a NULL.

Para determinar el estado de esta opción, examine la columna `is_ansi_null_default_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullDefault` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | **OFF** }     
ACTIVAR     
Todas las comparaciones con un valor NULL se evalúan como UNKNOWN.

Apagado     
Las comparaciones de valores no UNICODE con un valor NULL se evalúan como TRUE si ambos valores son NULL.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS siempre estará ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_NULLS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULLS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!NOTE]
> El valor de SET ANSI_NULLS también debe estar en ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_ansi_nulls_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVAR     
Las cadenas se rellenan a la misma longitud antes de la conversión. También se rellenan hasta la misma longitud antes de la inserción en un tipo de datos **varchar** o **nvarchar**.

Apagado     
Inserta espacios en blanco finales en los valores de caracteres en las columnas **varchar** o **nvarchar**. También deja los ceros a la derecha en los valores binarios insertados en columnas **varbinary**. Los valores no se rellenan hasta completar la longitud de la columna.

Si se especifica OFF, esta opción solamente afecta a la definición de las columnas nuevas.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING siempre estará en ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Se recomienda establecer siempre ANSI_PADDING en ON. ANSI_PADDING también debe estar en ON al crear o tratar índices en columnas calculadas o vistas indizadas.

**char(_n_)** y **binary(_n_)** las columnas que admiten valores NULL se rellenan hasta la longitud de la columna cuando ANSI_PADDING se establece en ON. Los ceros y los espacios en blanco finales se recortan si ANSI_PADDING es OFF. Las columnas **char(_n_)** y **binary(_n_)** que no permiten valores NULL siempre se rellenan hasta completar la longitud de la columna.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada del nivel de base de datos para ANSI_PADDING. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_PADDING en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_padding_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiPaddingEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVAR     
Se emiten errores o advertencias si se dan condiciones tales como la división por cero. También se emiten errores y advertencias cuando aparecen valores null en funciones de agregado.

Apagado     
No se genera ninguna advertencia ni se devuelven valores NULL si se producen condiciones como la división por cero.

> [!NOTE]
> El valor de SET ANSI_WARNINGS debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_WARNINGS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_WARNINGS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_warnings_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiWarningsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVAR     
Se finaliza una consulta cuando se produce un error de desbordamiento o de división por cero durante su ejecución.

Apagado     
Aparece un mensaje de advertencia cuando se produce uno de estos errores. La consulta, el proceso por lotes o la transacción continúa procesándose como si no se hubiera producido ningún error aunque se muestre una advertencia.

> [!NOTE]
> El valor de SET ARITHABORT debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_arithabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsArithmeticAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Para más información, consulte [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVAR     
El resultado de una operación de concatenación es NULL si alguno de los operandos es NULL. Por ejemplo, la concatenación de la cadena de caracteres "Esto es" y NULL da como resultado el valor NULL, y no el valor "Esto es".

Apagado     
El valor NULL se trata como una cadena de caracteres vacía.

> [!NOTE]
> El valor de CONCAT_NULL_YIELDS_NULL también debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.
>
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL siempre estará en ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para CONCAT_NULL_YIELDS_NULL. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión y establecen CONCAT_NULL_YIELDS_NULL en ON para la sesión al realizar la conexión con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_concat_null_yields_null_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNullConcat` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVAR     
Las comillas dobles se pueden usar para identificadores delimitados.

Todas las cadenas delimitadas por comillas dobles se interpretan como identificadores de objetos. Los identificadores entre comillas no tienen que adaptarse a las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para identificadores. Pueden ser palabras clave e incluir caracteres que no se permiten en los identificadores de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si una comilla simple (') forma parte de la cadena literal, puede representarse mediante comillas dobles (").

Apagado     
Los identificadores no se pueden incluir entre comillas y deben seguir todas las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para los identificadores. Los literales se pueden delimitar con comillas simples o dobles.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también permite delimitar los identificadores con corchetes ([ ]). Los identificadores entre corchetes pueden usarse siempre, independientemente del valor de QUOTED_IDENTIFIER. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

Al crear una tabla, la opción QUOTED IDENTIFIER siempre se almacena como ON en los metadatos de la tabla. La opción se almacena incluso si está establecida en OFF al crear la tabla.

La configuración en el nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para QUOTED_IDENTIFIER. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión estableciendo QUOTED_IDENTIFIER en ON. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_quoted_identifier_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsQuotedIdentifiersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVAR     
Se genera un error cuando se produce una pérdida de precisión en una expresión.

Apagado     
Las pérdidas de precisión no generan un mensaje de error y el resultado se redondea con la precisión de la columna o variable que lo almacena.

> [!IMPORTANT]
> El valor de NUMERIC_ROUNDABORT debe ser OFF al crear o realizar cambios en índices de columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_numeric_roundabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNumericRoundAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVAR     
Se permite la activación recursiva de desencadenadores AFTER.

Apagado     
Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La recursividad directa solo se evita cuando RECURSIVE_TRIGGERS se establece en OFF. Para deshabilitar la recursividad indirecta, también debe establecer la opción de servidor desencadenadores anidados en 0.

Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Especifica la frecuencia de puntos de comprobación indirectos por base de datos. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el valor predeterminado para nuevas bases de datos es de un minuto, lo cual indica que la base de datos usará puntos de comprobación indirectos. Para versiones anteriores, el valor predeterminado es 0, lo cual indica que la base de datos usará puntos de comprobación automáticos, cuya frecuencia depende del valor de intervalo de recuperación de la instancia de servidor. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda un minuto para la mayoría de los sistemas.

TARGET_RECOVERY_TIME **=** target_recovery_time { SECONDS | MINUTES }     
*target_recovery_time*     
Especifica el límite máximo de tiempo para recuperar la base de datos especificada en caso de bloqueo.

SECONDS     
Indica que *target_recovery_time* se expresa como el número de segundos.

MINUTES     
Indica que *target_recovery_time* se expresa como el número de minutos.

Para más información sobre los puntos de control indirectos, consulte [Puntos de control de base de datos](../../relational-databases/logs/database-checkpoints-sql-server.md).

**WITH \<termination> ::=**      
Especifica el momento en que se revierten las transacciones incompletas cuando la base de datos pasa de un estado a otro. Si se omite la cláusula de terminación, la instrucción ALTER DATABASE espera indefinidamente a que se produzca un bloqueo en la base de datos. Solamente se puede especificar una cláusula de terminación y debe seguir a las cláusulas SET.

> [!NOTE]
> No todas las opciones de base de datos usan la cláusula WITH \<termination>. Para más información, vea la tabla ubicada en "[Opciones de configuración](#SettingOptions) de la sección "Comentarios" de este artículo.

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Especifica si la operación de reversión se ejecuta transcurrido un número de segundos determinado o de forma inmediata.

NO_WAIT     
Especifica que la solicitud producirá un error si el cambio de opción o estado de la base de datos solicitado no puede completarse inmediatamente. Completarse inmediatamente significa que no se espera a que las transacciones se confirmen o reviertan por su cuenta.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuración de opciones

Para recuperar la configuración actual de las opciones de base de datos, use la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Una vez configurada una opción de la base de datos, la nueva configuración surte efecto de inmediato.

Puede cambiar los valores predeterminados de cualquiera de las opciones de las bases de datos recién creadas. Para ello, cambie la opción adecuada de base de datos en la base de datos de modelo.

No todas las opciones de base de datos utilizan la cláusula WITH \<termination> ni se pueden especificar en combinación con otras opciones. En la siguiente tabla se incluyen estas opciones, su estado y el estado de terminación.

|Categoría de opciones|Se puede especificar con otras opciones|Puede utilizar la cláusula WITH \<termination>|
|----------------------|-----------------------------------------|---------------------------------------------|
|\<auto_option>|Sí|No|
|\<change_tracking_option>|Sí|Sí|
|\<cursor_option>|Sí|No|
|\<db_encryption_option>|Sí|No|
|\<db_update_option>|Sí|Sí|
|\<db_user_access_option>|Sí|Sí|
|\<delayed_durability_option>|Sí|Sí|
|\<parameterization_option>|Sí|Sí|
|ALLOW_SNAPSHOT_ISOLATION|No|No|
|READ_COMMITTED_SNAPSHOT|No|Sí|
|MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT|Sí|Sí|
|DATE_CORRELATION_OPTIMIZATION|Sí|Sí|
|\<sql_option>|Sí|No|
|\<target_recovery_time_option>|No|Sí|

## <a name="examples"></a>Ejemplos

### <a name="a-setting-the-database-to-read_only"></a>A. Establecer la base de datos en READ_ONLY
El cambio del estado de una base de datos o un grupo de archivos a READ_ONLY o READ_WRITE requiere el acceso exclusivo a la base de datos. En el siguiente ejemplo la base de datos se establece en el modo `RESTRICTED_USER` para limitar el acceso. A continuación, el ejemplo establece el estado de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en `READ_ONLY` y devuelve el acceso a la base de datos a todos los usuarios.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO

```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Habilitar el aislamiento de instantánea en una base de datos
En el siguiente ejemplo se habilita la opción del marco de aislamiento de instantánea para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

El conjunto de resultados muestra que el marco de aislamiento de instantánea está habilitado.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ACTIVAR |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Habilitar, modificar y deshabilitar el seguimiento de cambios
En el ejemplo siguiente se habilita el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y se establece el período de retención en `2` días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

En el ejemplo siguiente se muestra cómo cambiar el período de retención a 3 días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

En el ejemplo siguiente se muestra cómo deshabilitar el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Habilitar el Almacén de consultas
En el ejemplo siguiente se habilita el Almacén de consultas y configura los parámetros del Almacén de consultas.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Habilitar el Almacén de consultas con las estadísticas de espera
En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Habilitar el Almacén de consultas con las opciones de directiva de captura personalizadas
En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
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

## <a name="see-also"></a>Consulte también

- [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [estadísticas](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md)
- [Habilitar y deshabilitar el seguimiento de cambios](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azuresqldb-mi-current"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* SQL Managed Instance \*_** &nbsp;
    :::column-end:::
    :::column:::
        [Azure Synapse<br />Analytics](alter-database-transact-sql-set-options.md?view=azure-sqldw-latest&preserve-view=true)
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-sql-managed-instance"></a>Azure SQL Managed Instance

Los niveles de compatibilidad son opciones de `SET`, pero se describe en [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

> [!NOTE]
> Es posible configurar muchas opciones SET de la base de datos para la sesión actual mediante [Instrucciones SET](../../t-sql/statements/set-statements-transact-sql.md), aunque generalmente las configuran las aplicaciones al realizar la conexión. Las opciones SET de nivel de sesión reemplazan a los valores **ALTER DATABASE SET**. Las opciones de base de datos descritas en las secciones siguientes son valores que se pueden establecer para sesiones que no proporcionan de forma explícita otros valores de opciones SET.

## <a name="syntax"></a>Sintaxis

```syntaxsql
ALTER DATABASE { database_name | Current }
SET
{
    <optionspec> [ ,...n ]
}
;

<optionspec> ::=
{
    <auto_option>
  | <change_tracking_option>
  | <cursor_option>
  | <db_encryption_option>
  | <delayed_durability_option>
  | <parameterization_option>
  | <query_store_options>
  | <snapshot_option>
  | <sql_option>
  | <target_recovery_time_option>
  | <termination>
  | <temporal_history_retention>
}
;
<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON [ ( INCREMENTAL = { ON | OFF } ) ] }
  | AUTO_SHRINK { ON | OFF }
  | AUTO_UPDATE_STATISTICS { ON | OFF }
  | AUTO_UPDATE_STATISTICS_ASYNC { ON | OFF }
}

<automatic_tuning_option> ::=
{
    AUTOMATIC_TUNING ( FORCE_LAST_GOOD_PLAN = { ON | OFF } )
}

<change_tracking_option> ::=
{
    CHANGE_TRACKING
    {
       = OFF
     | = ON [ ( <change_tracking_option_list > [,...n] ) ]
     | ( <change_tracking_option_list> [,...n] )
    }
}

<change_tracking_option_list> ::=
   {
       AUTO_CLEANUP = { ON | OFF }
     | CHANGE_RETENTION = retention_period { DAYS | HOURS | MINUTES }
   }

<cursor_option> ::=
{
    CURSOR_CLOSE_ON_COMMIT { ON | OFF }
}

<db_encryption_option> ::=
  ENCRYPTION { ON | OFF }

<delayed_durability_option> ::=DELAYED_DURABILITY = { DISABLED | ALLOWED | FORCED }

<parameterization_option> ::=
  PARAMETERIZATION { SIMPLE | FORCED }

<query_store_options> ::=
{
  QUERY_STORE
  {
    = OFF
    | = ON [ ( <query_store_option_list> [,... n] ) ]
    | ( < query_store_option_list> [,... n] )
    | CLEAR [ ALL ]
  }
}

<query_store_option_list> ::=
{
  OPERATION_MODE = { READ_WRITE | READ_ONLY }
  | CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = number )
  | DATA_FLUSH_INTERVAL_SECONDS = number
  | MAX_STORAGE_SIZE_MB = number
  | INTERVAL_LENGTH_MINUTES = number
  | SIZE_BASED_CLEANUP_MODE = { AUTO | OFF }
  | QUERY_CAPTURE_MODE = { ALL | AUTO | CUSTOM | NONE }
  | MAX_PLANS_PER_QUERY = number
  | WAIT_STATS_CAPTURE_MODE = { ON | OFF }
  | QUERY_CAPTURE_POLICY = ( <query_capture_policy_option_list> [,...n] )
}

<query_capture_policy_option_list> :: =
{
    STALE_CAPTURE_POLICY_THRESHOLD = number { DAYS | HOURS }
    | EXECUTION_COUNT = number
    | TOTAL_COMPILE_CPU_TIME_MS = number
    | TOTAL_EXECUTION_CPU_TIME_MS = number
}

<snapshot_option> ::=
{
    ALLOW_SNAPSHOT_ISOLATION { ON | OFF }
  | READ_COMMITTED_SNAPSHOT {ON | OFF }
  | MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT {ON | OFF }
}
<sql_option> ::=
{
    ANSI_NULL_DEFAULT { ON | OFF }
  | ANSI_NULLS { ON | OFF }
  | ANSI_PADDING { ON | OFF }
  | ANSI_WARNINGS { ON | OFF }
  | ARITHABORT { ON | OFF }
  | COMPATIBILITY_LEVEL = { 150 | 140 | 130 | 120 | 110 | 100 }
  | CONCAT_NULL_YIELDS_NULL { ON | OFF }
  | NUMERIC_ROUNDABORT { ON | OFF }
  | QUOTED_IDENTIFIER { ON | OFF }
  | RECURSIVE_TRIGGERS { ON | OFF }
}

<temporal_history_retention>::= TEMPORAL_HISTORY_RETENTION { ON | OFF }
```

## <a name="arguments"></a>Argumentos

*database_name*     
Es el nombre de la base de datos que se va a modificar.

CURRENT     
`CURRENT` realiza la acción en la base de datos actual. `CURRENT` no se admite para todas las opciones en todos los contextos. Si `CURRENT` produce un error, proporcione el nombre de la base de datos.

**\<auto_option> ::=**      
Controla las opciones automáticas.

<a name="auto_create_statistics"></a> AUTO_CREATE_STATISTICS { **ON** | OFF }     
ACTIVAR     
El optimizador de consultas crea estadísticas en columnas únicas de los predicados de consulta, según sea necesario, para mejorar los planes de consulta y el rendimiento de las consultas. Estas estadísticas de columna única se crean cuando el optimizador de consultas las compila. Las estadísticas de columna única solamente se crean en las columnas que ya no son la primera columna de un objeto de estadísticas existente.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Apagado     
El optimizador de consultas no crea las estadísticas en columnas únicas de los predicados de consulta cuando compila las consultas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna `is_auto_create_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoCreateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Opciones de estadísticas" en [Estadísticas](../../relational-databases/statistics/statistics.md#statistics-options).

INCREMENTAL = ON | **OFF**     
Establezca AUTO_CREATE_STATISTICS en ON y establezca INCREMENTAL en ON. Esta configuración crea automáticamente estadísticas como incrementales siempre que se admitan estadísticas incrementales. El valor predeterminado es OFF. Para más información, consulte [CREATE STATISTICS](../../t-sql/statements/create-statistics-transact-sql.md).

<a name="auto_shrink"></a> AUTO_SHRINK { ON | **OFF** }     
ACTIVAR     
Los archivos de la base de datos se pueden reducir periódicamente.

Pueden reducirse automáticamente los archivos de datos y los archivos de registro. AUTO_SHRINK reduce el tamaño del registro de transacciones solo si establece la base de datos en el modelo de recuperación SIMPLE o si realiza una copia de seguridad del registro. Cuando se establece en OFF, los archivos de la base de datos no se reducen de forma automática durante las comprobaciones periódicas del espacio no utilizado.

La opción AUTO_SHRINK reduce los archivos cuando no se utiliza más de un 25% del espacio del archivo. La opción hace que el archivo se reduzca a uno de dos tamaños. Se reduce al que sea más grande de los siguientes:

- El tamaño donde el 25 por ciento del archivo es espacio no utilizado
- El tamaño del archivo cuando se creó

No puede reducir una base de datos de solo lectura.

Apagado     
Los archivos de la base de datos no se reducen automáticamente durante las comprobaciones periódicas del espacio no utilizado.

Para determinar el estado de esta opción, examine la columna `is_auto_shrink_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoShrink` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La opción AUTO_SHRINK no está disponible en una base de datos independiente.

<a name="auto_update_statistics"></a> AUTO_UPDATE_STATISTICS { **ON** | OFF }     
ACTIVAR     
Especifica que el optimizador de consultas actualiza las estadísticas cuando las usa una consulta y puedan estar obsoletas. Las estadísticas se vuelven obsoletas después de que operaciones de inserción, actualización, eliminación o combinación cambien la distribución de los datos en la tabla o la vista indizada. Para determinar cuándo las estadísticas pueden quedar obsoletas, el optimizador de consultas cuenta el número de modificaciones de datos desde la actualización más reciente de las estadísticas y compara el número de modificaciones con un umbral. El umbral se basa en el número de filas de la tabla o la vista indizada.

El optimizador de consultas comprueba si hay estadísticas obsoletas antes de compilar una consulta y ejecuta un plan de consulta almacenado en caché. El optimizador de consultas usa las columnas, tablas y vistas indexadas del predicado de consulta para determinar qué estadísticas podrían estar obsoletas. El optimizador de consultas determina esta información antes de compilar una consulta. Antes de ejecutar un plan de consulta almacenado en caché, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba que el plan de consulta haga referencia a las estadísticas actualizadas.

La opción AUTO_UPDATE_STATISTICS se aplica a las estadísticas creadas para índices y columnas únicas de los predicados de consulta, así como a las estadísticas creadas con la instrucción CREATE STATISTICS. Esta opción también se aplica a las estadísticas filtradas.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Utilice la opción AUTO_UPDATE_STATISTICS_ASYNC para especificar si las estadísticas se actualizan sincrónica o asincrónicamente.

Apagado     
Especifica que el optimizador de consultas no actualiza las estadísticas cuando una consulta las usa. El optimizador de consultas tampoco actualiza las estadísticas cuando podrían estar obsoletas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

Para determinar el estado de esta opción, examine la columna is_auto_update_stats_on en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoUpdateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Uso de las opciones de estadísticas de toda la base de datos" de [Estadísticas](../../relational-databases/statistics/statistics.md).

<a name="auto_update_statistics_async"></a> AUTO_UPDATE_STATISTICS_ASYNC { ON | **OFF** }     
ACTIVAR     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son asincrónicas. El optimizador de consultas no espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

La configuración de esta opción en ON no surte efecto a menos que AUTO_UPDATE_STATISTICS se establezca en ON.

De forma predeterminada, la opción AUTO_UPDATE_STATISTICS_ASYNC se establece en OFF y el optimizador de consultas actualiza las estadísticas de manera sincrónica.

Apagado     
Especifica que las actualizaciones de las estadísticas para la opción AUTO_UPDATE_STATISTICS son sincrónicas. El optimizador de consultas espera a que finalicen las actualizaciones de las estadísticas para compilar las consultas.

La configuración de esta opción en OFF no surte efecto a menos que AUTO_UPDATE_STATISTICS esté configurado en ON.

Para determinar el estado de esta opción, examine la columna is_auto_update_stats_async_on en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

Para obtener más información sobre cuándo usar las actualizaciones de estadísticas sincrónicas o asincrónicas, vea la sección "Uso de las opciones de estadísticas de toda la base de datos" de [Estadísticas](../../relational-databases/statistics/statistics.md).

<a name="auto_tuning"></a> **\<automatic_tuning_option> ::=**      
**Se aplica a**: [!INCLUDE[sssql17-md](../../includes/sssql17-md.md)]

Habilita o deshabilita la opción [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md) de `FORCE_LAST_GOOD_PLAN`.

FORCE_LAST_GOOD_PLAN = { ON | **OFF** }     
ACTIVAR     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] fuerza automáticamente el último buen plan conocido en las consultas de [!INCLUDE[tsql-md](../../includes/tsql-md.md)], donde el nuevo plan de consulta provoca regresiones de rendimiento. El parámetro [!INCLUDE[ssde_md](../../includes/ssde_md.md)] supervisa continuamente el rendimiento de la consulta [!INCLUDE[tsql-md](../../includes/tsql-md.md)] con el plan forzado. Si hay mejoras de rendimiento, el [!INCLUDE[ssde_md](../../includes/ssde_md.md)] seguirá usando el último buen plan conocido. Si no se detectan mejoras de rendimiento, [!INCLUDE[ssde_md](../../includes/ssde_md.md)] generará un nuevo plan de consulta. Se producirá un error en la instrucción si el almacén de consultas no está habilitado o si no está en modo de *lectura y escritura*.

Apagado     
[!INCLUDE[ssde_md](../../includes/ssde_md.md)] informa de posibles regresiones de rendimiento de consultas provocadas por cambios del plan de consulta en la vista [sys.dm_db_tuning_recommendations](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md), aunque estas recomendaciones no se aplican automáticamente. Para supervisar las recomendaciones activas y corregir los problemas identificados, los usuarios pueden aplicar los scripts de [!INCLUDE[tsql-md](../../includes/tsql-md.md)] que se muestran en la vista. Este es el valor predeterminado.

**\<change_tracking_option> ::=**      
Controla las opciones de seguimiento de cambios. Puede habilitar el seguimiento de cambios, establecer y cambiar opciones, y deshabilitar el seguimiento de cambios. Para obtener ejemplos, vea la sección "Ejemplos" más adelante en este artículo.

ON Habilita el seguimiento de cambios para la base de datos. Si habilita el seguimiento de cambios, también puede establecer las opciones AUTO CLEANUP y CHANGE RETENTION.

AUTO_CLEANUP = { ON | OFF }     
ACTIVAR     
La información sobre el seguimiento de cambios se quita de forma automática después del período de retención especificado.

Apagado     
Los datos del seguimiento de cambios no se quitan de la base de datos.

CHANGE_RETENTION = *período_de_retención* { **DAYS** | HOURS | MINUTES }     
Especifica el período mínimo para mantener la información del seguimiento de cambios en la base de datos. Los datos solamente se quitan cuando el valor AUTO_CLEANUP es ON.

*retention_period* es un entero que especifica el componente numérico del período de retención.

El período de retención predeterminado es de **2 días**. El período de retención mínimo es de 1 minuto. El tipo de retención predeterminado es **DAYS**.

Apagado     
Deshabilita el seguimiento de cambios para la base de datos. Deshabilite el seguimiento de cambios en todas las tablas para poder deshabilitarlo en la base de datos.

**\<cursor_option> ::=**      
Controla las opciones del cursor.

CURSOR_CLOSE_ON_COMMIT { ON | OFF }     
ACTIVAR     
Todos los cursores abiertos cuando confirma o deshace una transacción se cierran.

Apagado     
Los cursores permanecen abiertos cuando se confirma una transacción. Cuando se revierte se cierran todos los cursores, excepto los que están definidos como INSENSITIVE o STATIC.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada de la base de datos para CURSOR_CLOSE_ON_COMMIT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión que establece CURSOR_CLOSE_ON_COMMIT en OFF para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CURSOR_CLOSE_ON_COMMIT](../../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).

Para determinar el estado de esta opción, examine la columna is_cursor_close_on_commit_on de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad IsCloseCursorsOnCommitEnabled de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md). El cursor se desasigna implícitamente solamente cuando se realiza la desconexión. Para más información, consulte [DECLARE CURSOR](../../t-sql/language-elements/declare-cursor-transact-sql.md).

**\<db_encryption_option> ::=**      
Controla el estado del cifrado de la base de datos.

ENCRYPTION { ON | **OFF** }     
Establece que se cifre (ON) o no se cifre (OFF) la base de datos. Para más información sobre el cifrado de base de datos, consulte [Cifrado de datos transparente](../../relational-databases/security/encryption/transparent-data-encryption.md) y [Cifrado de datos transparente con Azure SQL Database](/azure/azure-sql/database/transparent-data-encryption-tde-overview).

Cuando el cifrado está habilitado en el nivel de la base de datos, se cifrarán todos los grupos de archivos. Todos los grupos de archivos nuevos heredarán la propiedad de cifrado. Si en la base de datos hay grupos de archivos establecidos en READ ONLY, se producirá un error en la operación de cifrado de la base de datos.

Puede ver el estado del cifrado de la base de datos mediante la vista de administración dinámica [sys.dm_database_encryption_keys](../../relational-databases/system-dynamic-management-views/sys-dm-database-encryption-keys-transact-sql.md).

**\<db_update_option> ::=**      
Controla si se permiten las actualizaciones en la base de datos.

READ_ONLY     
Los usuarios pueden leer datos de la base de datos, pero no pueden modificarlos.

> [!NOTE]
> Para mejorar el rendimiento de las consultas, actualice las estadísticas antes de establecer una base de datos en READ_ONLY. Si se necesitan estadísticas adicionales después de establecer una base de datos en READ_ONLY, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] creará las estadísticas en tempdb. Para más información sobre las estadísticas para una base de datos de solo lectura, vea [Utilizar las estadísticas para mejorar el rendimiento de las consultas](../../relational-databases/statistics/statistics.md).

READ_WRITE     
La base de datos está disponible para operaciones de lectura y escritura.

Para cambiar este estado, debe tener acceso exclusivo a la base de datos.

**\<db_user_access_option> ::=**      
Controla el acceso del usuario a la base de datos.

RESTRICTED_USER     
Solo permite a los miembros del rol fijo de base de datos `db_owner` y de los roles fijos de servidor `dbcreator` y `sysadmin` conectarse a la base de datos, pero no limita la cantidad de miembros. Todas las conexiones a la base de datos se desconectan en el margen de tiempo especificado por la cláusula de terminación de la instrucción ALTER DATABASE. Una vez que la base de datos ha cambiado al estado RESTRICTED_USER, se rechazan los intentos de conexión por parte de usuarios no cualificados. **RESTRICTED_USER** no se puede modificar con SQL Managed Instance.

MULTI_USER     
Todos los usuarios que tengan los permisos correspondientes pueden conectarse a la base de datos.

Para determinar el estado de esta opción, examine la columna user_access de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `UserAccess` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<delayed_durability_option> ::=**      
Controla si las transacciones se confirman con perdurabilidad total o diferida.

DISABLED     
Todas las transacciones tras `SET DISABLED` son totalmente perdurables. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

ALLOWED     
Todas las transacciones tras `SET ALLOWED` son totalmente perdurables o de perdurabilidad diferida, dependiendo de la opción de perdurabilidad establecida en el bloque ATOMIC o la instrucción de confirmación.

FORCED     
Todas las transacciones tras `SET FORCED` son de perdurabilidad diferida. Se omiten las opciones de perdurabilidad que se establecen en un bloque ATOMIC o en una instrucción de confirmación.

**\<PARAMETERIZATION_option> ::=**      
Controla la opción de parametrización.

PARAMETERIZATION { **SIMPLE** | FORCED }     
SIMPLE     
Las consultas incluyen parámetros en función del comportamiento predeterminado de la base de datos.

FORCED     
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye parámetros para todas las consultas de la base de datos.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_parameterization_forced` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

<a name="query-store"></a> **\<query_store_options> ::=**

ON | OFF | CLEAR [ ALL ]     
Controla si el almacén de consultas está habilitado en esta base de datos y también controla la eliminación del contenido del almacén de consultas.

ACTIVAR     
Habilita el Almacén de consultas.

Apagado     
Deshabilita el Almacén de consultas. Este es el valor predeterminado.

CLEAR     
Quita el contenido del Almacén de consultas.

OPERATION_MODE     
Describe el modo de operación del Almacén de consultas. Los valores válidos son READ_ONLY y READ_WRITE. En el modo READ_WRITE, el Almacén de consultas recopila y continúa el plan de consultas y la información de estadística del tiempo de ejecución. En el modo READ_ONLY, la información se puede leer del almacén de consultas, pero no se agrega información nueva. Si se ha agotado el espacio máximo del Almacén de consultas, el Almacén de consultas cambiará el modo de operación a READ_ONLY.

CLEANUP_POLICY     
Describe la directiva de retención de datos del Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS determina el número de días durante los que se conserva la información de una consulta en el Almacén de consultas. STALE_QUERY_THRESHOLD_DAYS es de tipo **bigint**. El valor predeterminado es 30. En la edición [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Basic, el valor predeterminado es **7** días.

DATA_FLUSH_INTERVAL_SECONDS     
Determina la frecuencia con la que los datos escritos en el Almacén de consultas se conservan en el disco. Para optimizar el rendimiento, los datos recopilados por el Almacén de consultas se escriben de manera asincrónica en el disco. La frecuencia con la que se produce esta transferencia asincrónica se configura mediante el argumento DATA_FLUSH_INTERVAL_SECONDS. DATA_FLUSH_INTERVAL_SECONDS es de tipo **bigint**. El valor predeterminado es **900** (15 minutos).

MAX_STORAGE_SIZE_MB     
Determina el espacio asignado al Almacén de consultas. MAX_STORAGE_SIZE_MB es de tipo **bigint**. El valor predeterminado es **100 MB**.

> [!NOTE]
> El límite `MAX_STORAGE_SIZE_MB` no se aplica de forma estricta. El tamaño de almacenamiento solo se comprueba cuando el almacén de consultas escribe datos en el disco. Este intervalo se establece mediante la opción `DATA_FLUSH_INTERVAL_SECONDS` o la opción **Intervalo de vaciado de datos** del cuadro de diálogo Almacén de consultas de [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. El valor predeterminado de intervalo es 900 segundos (o 15 minutos).
> Si el Almacén de consultas ha infringido el límite de `MAX_STORAGE_SIZE_MB` entre las comprobaciones de tamaño de almacenamiento, pasará al modo de solo lectura. Si `SIZE_BASED_CLEANUP_MODE` está habilitado, también se desencadena el mecanismo de limpieza para aplicar el límite de `MAX_STORAGE_SIZE_MB`.
> Una vez que se haya borrado espacio suficiente, el modo de Almacén de consultas volverá a cambiar de forma automática a lectura y escritura.

> [!IMPORTANT]
> Si cree que la captura de la carga de trabajo necesitará más de 10 GB de espacio en disco, probablemente debería replantear y optimizar la carga de trabajo para reutilizar los planes de consulta (por ejemplo, mediante la [parametrización forzada](../../relational-databases/query-processing-architecture-guide.md#ForcedParam)) o ajustar las configuraciones del Almacén de consultas.    
> A partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], puede establecer `QUERY_CAPTURE_MODE` en PERSONALIZADO para obtener un control adicional sobre la directiva de captura de consultas.

INTERVAL_LENGTH_MINUTES     
Determina el intervalo de tiempo en el que se agregan los datos de estadísticas de ejecución en tiempo de ejecución al Almacén de consultas. Para optimizar el uso del espacio, se agregan las estadísticas de ejecución en tiempo de ejecución en el almacén de estadísticas de tiempo de ejecución en una ventana de tiempo fijo. Esta ventana de tiempo fijo se configura con el argumento INTERVAL_LENGTH_MINUTES. INTERVAL_LENGTH_MINUTES es de tipo **bigint**. El valor predeterminado es **60**.

SIZE_BASED_CLEANUP_MODE     
Controla si la limpieza se activará de forma automática cuando la cantidad total de datos se acerque al tamaño máximo.

Apagado     
La limpieza según el tamaño no se activará automáticamente.

AUTO     
La limpieza según el tamaño se activará automáticamente cuando el tamaño en disco alcance el 90 % de **max_storage_size_mb**. La limpieza según el tamaño quita primero las consultas menos caras y más antiguas. Se detiene aproximadamente en el 80 % de **max_storage_size_mb**. Es el valor de configuración predeterminado.

SIZE_BASED_CLEANUP_MODE es de tipo **nvarchar**.

QUERY_CAPTURE_MODE { ALL \| AUTO \| CUSTOM \| NONE }     
Designa el modo de captura de consulta que está activo.

ALL     
Se capturan todas las consultas.

AUTO     
Captura consultas pertinentes en función del consumo de recursos y el recuento de ejecuciones. Es el valor de configuración predeterminado para [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)].

Ninguno     
Detiene la captura de nuevas consultas. El almacén de consultas seguirá recopilando estadísticas de compilación y tiempo de ejecución para las consultas que ya se han capturado. Use esta configuración con precaución ya que podría omitir la captura de consultas importantes.

QUERY_CAPTURE_MODE es de tipo **nvarchar**.

max_plans_per_query     
Entero que representa el número máximo de planes que se tienen para cada consulta. MAX_PLANS_PER_QUERY es del tipo **int**. El valor predeterminado es **200**.

WAIT_STATS_CAPTURE_MODE { **ON** | OFF }     
Controla si se capturarán estadísticas de espera por consulta.

ACTIVAR    
Se captura información de estadísticas de espera por consulta. Este es el valor de configuración predeterminado.

Apagado    
No se capturará información de estadísticas de espera por consulta.

**\<query_capture_policy_option_list> :: =**      
Controla las opciones de directiva de captura de Almacén de consultas. Excepto en STALE_CAPTURE_POLICY_THRESHOLD, estas opciones definen las condiciones OR que se tienen que dar para que las consultas se capturen en el valor definido en el umbral de la directiva de capturas obsoletas.

STALE_CAPTURE_POLICY_THRESHOLD = *number* { DAYS | HOURS }     
Define el período de intervalo de evaluación para determinar si se debe capturar una consulta. El valor predeterminado es de 1 día y puede establecerse de 1 hora a 7 días. *number* es del tipo **int**.

EXECUTION_COUNT     
Define el número de veces que se ejecuta una consulta durante el período de evaluación. El valor predeterminado es 30, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta se tendrá que ejecutar al menos 30 veces en un día para que se conserve en el almacén de consultas. EXECUTION_COUNT es del tipo **int**.

TOTAL_COMPILE_CPU_TIME_MS     
Define el tiempo total de CPU de compilación transcurrido que usa una consulta durante el período de evaluación. El valor predeterminado es 1000, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos un segundo de tiempo de CPU durante la compilación de consulta en un día para conservarse en el almacén de consultas. TOTAL_COMPILE_CPU_TIME_MS es del tipo **int**.

TOTAL_EXECUTION_CPU_TIME_MS     
Define el tiempo de ejecución total de CPU transcurrido que ha utilizado una consulta durante el período de evaluación. El valor predeterminado es 100, lo que significa que, para el valor predeterminado del umbral de la directiva de capturas obsoletas, una consulta tendrá que invertir un total de al menos 100 ms de tiempo de CPU durante la ejecución en un día para que se conserve en el almacén de consultas. TOTAL_EXECUTION_CPU_TIME_MS es del tipo **int**.

**\<snapshot_option> ::=**

Determina el nivel de aislamiento de transacción.

ALLOW_SNAPSHOT_ISOLATION { ON | OFF }     
ACTIVAR     
Habilita la opción de instantánea en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones pueden especificar el nivel de aislamiento de transacción SNAPSHOT. Si se ejecuta una transacción en el nivel de aislamiento SNAPSHOT, todas las instrucciones verán una instantánea de los datos tal como estaban al inicio de la transacción. Si una transacción ejecutada en el nivel de aislamiento SNAPSHOT tiene acceso a los datos de varias bases de datos, ALLOW_SNAPSHOT_ISOLATION debe establecerse en ON en todas las bases de datos o cada instrucción de la transacción debe utilizar sugerencias de bloqueo en cualquier referencia de una cláusula FROM a una tabla de una base de datos donde ALLOW_SNAPSHOT_ISOLATION sea OFF.

Apagado     
Desactiva la opción de instantánea en el nivel de base de datos. Las transacciones no pueden especificar el nivel de aislamiento de la transacción SNAPSHOT.

Si se establece ALLOW_SNAPSHOT_ISOLATION en un estado nuevo (de ON a OFF o de OFF a ON), ALTER DATABASE no devuelve el control al autor de la llamada hasta confirmar todas las transacciones existentes de la base de datos. Si la base de datos ya se encuentra en el estado especificado en la instrucción ALTER DATABASE, se devuelve de inmediato el control al autor de la llamada. Use [sys.dm_tran_active_snapshot_database_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-active-snapshot-database-transactions-transact-sql.md) para determinar si hay transacciones de ejecución prolongada si la instrucción ALTER DATABASE no devuelve resultados rápidamente. Si se cancela la instrucción ALTER DATABASE, la base de datos permanece en el estado en que estaba al iniciar ALTER DATABASE. La vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) indica el estado de las transacciones de aislamiento de instantáneas en la base de datos. Si **snapshot_isolation_state_desc** = IN_TRANSITION_TO_ON, ALTER DATABASE ALLOW_SNAPSHOT_ISOLATION OFF se pausará durante seis segundos y reintentará la operación.

No puede cambiar el estado de ALLOW_SNAPSHOT_ISOLATION si la base de datos está establecida en OFFLINE.

Si establece ALLOW_SNAPSHOT_ISOLATION en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece posteriormente en READ_WRITE.

Puede cambiar la configuración de ALLOW_SNAPSHOT_ISOLATION para las bases de datos maestra, model, msdb y tempdb. La configuración se mantiene cada vez que la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] se detiene y se reinicia si cambia la configuración para tempdb. Si cambia la configuración para la base de datos model, dicha configuración se convierte en la configuración predeterminada para todas las bases de datos nuevas que se crean, excepto para tempdb.

La opción es ON de forma predeterminada para las bases de datos maestra y msdb.

El valor actual de esta opción se puede determinar mediante el examen de la columna `snapshot_isolation_state` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

READ_COMMITTED_SNAPSHOT { ON | **OFF** }     
ACTIVAR     
Habilita la opción Instantánea de lectura confirmada en el nivel de base de datos. Cuando se habilita, las instrucciones DML inician la generación de versiones de fila aunque ninguna transacción utilice el aislamiento de instantánea. Una vez habilitada esta opción, las transacciones que especifican el nivel de aislamiento READ COMMITTED usan versiones de fila en lugar de bloqueos. Todas las instrucciones ven una instantánea de los datos tal y como existen al inicio de la instrucción si una transacción se ejecuta en el nivel de aislamiento READ COMMITTED.

Apagado     
Desactiva la opción de instantánea de lectura confirmada en el nivel de base de datos. Las transacciones que especifican el nivel de aislamiento READ COMMITTED utilizan el bloqueo.

Para establecer READ_COMMITTED_SNAPSHOT en ON u OFF, no puede haber ninguna conexión activa a la base de datos, excepto la que ejecuta el comando ALTER DATABASE. Sin embargo, no es necesario que la base de datos esté en modo de usuario único. No puede cambiar el estado de esta opción si la base de datos está establecida en OFFLINE.

Si establece READ_COMMITTED_SNAPSHOT en una base de datos READ_ONLY, la configuración se mantendrá si la base de datos se establece después en READ_WRITE.

READ_COMMITTED_SNAPSHOT no se puede cambiar a ON para las bases de datos maestra, tempdb o msdb. Si cambia la configuración para model, dicha configuración se convierte en predeterminada para todas las bases de datos creadas, excepto para tempdb.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_read_committed_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

> [!WARNING]
> Cuando se crea una tabla con **DURABILITY = SCHEMA_ONLY** y posteriormente se cambia **READ_COMMITTED_SNAPSHOT** mediante **ALTER DATABASE**, se pierden los datos de la tabla.

MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT { ON | **OFF** }     
ACTIVAR     
Cuando el nivel de aislamiento de transacción se establece en uno inferior a SNAPSHOT, todas las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria se ejecutan con aislamiento SNAPSHOT. Los ejemplos de los niveles de aislamiento inferiores a la instantánea son READ COMMITTED o READ UNCOMMITTED. Estas operaciones se ejecutan si el nivel de aislamiento de transacción se establece explícitamente en el nivel de sesión o el valor predeterminado se utiliza de forma implícita.

Apagado     
No eleva el nivel de aislamiento de transacción para las operaciones interpretadas de [!INCLUDE[tsql](../../includes/tsql-md.md)] en las tablas optimizadas para memoria.

No puede cambiar el estado de MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT si la base de datos está establecida en OFFLINE.

El valor predeterminado es OFF.

El valor actual de esta opción se puede determinar mediante el examen de la columna `is_memory_optimized_elevate_to_snapshot_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).

**\<sql_option> ::=**      
Controla las opciones de cumplimiento con ANSI en el nivel de base de datos.

ANSI_NULL_DEFAULT { ON | OFF }     
Determina el valor predeterminado, NULL o NOT NULL, de una columna o del [tipo definido por el usuario CLR](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) para los que no se ha definido la nulabilidad explícitamente en las instrucciones CREATE TABLE o ALTER TABLE. Las columnas para las que se hayan definido restricciones siguen las reglas de las restricciones, independientemente de esta configuración.

ACTIVAR     
El valor predeterminado es NULL.

Apagado     
El valor predeterminado es NOT NULL.

La configuración del nivel de conexión, establecida mediante la instrucción SET, invalida la configuración predeterminada del nivel de base de datos para ANSI_NULL_DEFAULT. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULL_DEFAULT en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULL_DFLT_ON](../../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).

Para la compatibilidad con ANSI, si se establece la opción de base de datos ANSI_NULL_DEFAULT en ON, el valor predeterminado cambia a NULL.

Para determinar el estado de esta opción, examine la columna `is_ansi_null_default_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullDefault` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_NULLS { ON | OFF }     
ACTIVAR     
Todas las comparaciones con un valor NULL se evalúan como UNKNOWN.

Apagado     
Las comparaciones de valores no UNICODE con un valor NULL se evalúan como TRUE si ambos valores son NULL.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_NULLS siempre estará ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

  La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_NULLS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_NULLS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_NULLS](../../t-sql/statements/set-ansi-nulls-transact-sql.md).

> [!IMPORTANT]
> El valor de SET ANSI_NULLS también debe estar en ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_ansi_nulls_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiNullsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_PADDING { ON | **OFF** }     
ACTIVAR     
Las cadenas se rellenan a la misma longitud antes de la conversión. También se rellenan hasta la misma longitud antes de la inserción en un tipo de datos **varchar** o **nvarchar**.

Apagado     
Inserta espacios en blanco finales en los valores de caracteres en las columnas **varchar** o **nvarchar**. También deja los ceros a la derecha en los valores binarios insertados en columnas **varbinary**. Los valores no se rellenan hasta completar la longitud de la columna.

Si se especifica OFF, esta opción solamente afecta a la definición de las columnas nuevas.

> [!IMPORTANT]
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ANSI_PADDING siempre estará en ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan. Se recomienda establecer siempre ANSI_PADDING en ON. ANSI_PADDING también debe estar en ON al crear o tratar índices en columnas calculadas o vistas indizadas.

**char(_n_)** y **binary(_n_)** las columnas que admiten valores NULL se rellenan hasta la longitud de la columna cuando ANSI_PADDING se establece en ON. Los ceros y los espacios en blanco finales se recortan si ANSI_PADDING es OFF. Las columnas **char(_n_)** y **binary(_n_)** que no permiten valores NULL siempre se rellenan hasta completar la longitud de la columna.

  La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada del nivel de base de datos para ANSI_PADDING. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_PADDING en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_PADDING](../../t-sql/statements/set-ansi-padding-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_padding_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiPaddingEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ANSI_WARNINGS { ON | **OFF** }     
ACTIVAR     
Se emiten errores o advertencias si se dan condiciones tales como la división por cero. También se emiten errores y advertencias cuando aparecen valores null en funciones de agregado.

Apagado     
No se genera ninguna advertencia ni se devuelven valores NULL si se producen condiciones como la división por cero.

> [!IMPORTANT]
> El valor de SET ANSI_WARNINGS debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

  La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para ANSI_WARNINGS. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión mediante el establecimiento de ANSI_WARNINGS en ON para la sesión. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET ANSI_WARNINGS](../../t-sql/statements/set-ansi-warnings-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_ansi_warnings_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAnsiWarningsEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

ARITHABORT { ON | OFF }     
ACTIVAR     
Se finaliza una consulta cuando se produce un error de desbordamiento o de división por cero durante su ejecución.

Apagado     
Aparece un mensaje de advertencia cuando se produce uno de estos errores. La consulta, el proceso por lotes o la transacción continúa procesándose como si no se hubiera producido ningún error aunque se muestre una advertencia.

> [!IMPORTANT]
> El valor de SET ARITHABORT debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.

  Para determinar el estado de esta opción, examine la columna `is_arithabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsArithmeticAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

COMPATIBILITY_LEVEL = { 150 \| 140 \| 130 \| 120 \| 110 \| 100 }     
Para más información, consulte [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md).

CONCAT_NULL_YIELDS_NULL { ON | **OFF** }     
ACTIVAR     
El resultado de una operación de concatenación es NULL si alguno de los operandos es NULL. Por ejemplo, la concatenación de la cadena de caracteres "Esto es" y NULL da como resultado el valor NULL, y no el valor "Esto es".

Apagado     
El valor NULL se trata como una cadena de caracteres vacía.

> [!IMPORTANT]
> El valor de CONCAT_NULL_YIELDS_NULL también debe ser ON al crear o realizar cambios en los índices en columnas calculadas o vistas indizadas.
>
> En una versión futura de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], CONCAT_NULL_YIELDS_NULL siempre estará en ON y cualquier aplicación que establezca explícitamente la opción en OFF producirá un error. Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.

La configuración del nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para CONCAT_NULL_YIELDS_NULL. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión y establecen CONCAT_NULL_YIELDS_NULL en ON para la sesión al realizar la conexión con una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET CONCAT_NULL_YIELDS_NULL](../../t-sql/statements/set-concat-null-yields-null-transact-sql.md).

Para determinar el estado de esta opción, examine la columna `is_concat_null_yields_null_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNullConcat` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

QUOTED_IDENTIFIER { ON | OFF }     
ACTIVAR     
Las comillas dobles se pueden usar para identificadores delimitados.

Todas las cadenas delimitadas por comillas dobles se interpretan como identificadores de objetos. Los identificadores entre comillas no tienen que adaptarse a las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para identificadores. Pueden ser palabras clave e incluir caracteres que no se permiten en los identificadores de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Si una comilla simple (') forma parte de la cadena literal, puede representarse mediante comillas dobles (").

Apagado     
Los identificadores no se pueden incluir entre comillas y deben seguir todas las reglas de [!INCLUDE[tsql](../../includes/tsql-md.md)] para los identificadores. Los literales se pueden delimitar con comillas simples o dobles.

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también permite delimitar los identificadores con corchetes ([ ]). Los identificadores entre corchetes pueden usarse siempre, independientemente del valor de QUOTED_IDENTIFIER. Para obtener más información, vea [Database Identifiers](../../relational-databases/databases/database-identifiers.md).

  Al crear una tabla, la opción QUOTED IDENTIFIER siempre se almacena como ON en los metadatos de la tabla. La opción se almacena incluso si está establecida en OFF al crear la tabla.

La configuración en el nivel de conexión establecida mediante la instrucción SET invalida la configuración predeterminada de la base de datos para QUOTED_IDENTIFIER. De forma predeterminada, los clientes ODBC y OLE DB generan una instrucción SET en el nivel de conexión estableciendo QUOTED_IDENTIFIER en ON. Los clientes ejecutan la instrucción cuando se conecta a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para más información, consulte [SET QUOTED_IDENTIFIER](../../t-sql/statements/set-quoted-identifier-transact-sql.md).

  Para determinar el estado de esta opción, examine la columna `is_quoted_identifier_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsQuotedIdentifiersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

NUMERIC_ROUNDABORT { ON | OFF }     
ACTIVAR     
Se genera un error cuando se produce una pérdida de precisión en una expresión.

Apagado     
Las pérdidas de precisión no generan un mensaje de error y el resultado se redondea con la precisión de la columna o variable que lo almacena.

> [!IMPORTANT]
> El valor de NUMERIC_ROUNDABORT debe ser OFF al crear o realizar cambios en índices de columnas calculadas o vistas indizadas.

Para determinar el estado de esta opción, examine la columna `is_numeric_roundabort_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsNumericRoundAbortEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

RECURSIVE_TRIGGERS { ON | OFF }     
ACTIVAR     
Se permite la activación recursiva de desencadenadores AFTER.

Apagado     
Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

> [!NOTE]
> La recursividad directa solo se evita cuando RECURSIVE_TRIGGERS se establece en OFF. Para deshabilitar la recursividad indirecta, también debe establecer la opción de servidor desencadenadores anidados en 0.

Para determinar el estado de esta opción, examine la columna `is_recursive_triggers_on`de la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o la propiedad `IsRecursiveTriggersEnabled` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

**\<target_recovery_time_option> ::=**      
Especifica la frecuencia de puntos de comprobación indirectos por base de datos. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], el valor predeterminado para nuevas bases de datos es de **un minuto**, lo cual indica que la base de datos usará puntos de comprobación indirectos. Para versiones anteriores, el valor predeterminado es 0, lo cual indica que la base de datos usará puntos de comprobación automáticos, cuya frecuencia depende del valor de intervalo de recuperación de la instancia de servidor. [!INCLUDE[msCoName](../../includes/msconame-md.md)] recomienda un minuto para la mayoría de los sistemas.

TARGET_RECOVERY_TIME **=** _target_recovery_time_ { SECONDS | MINUTES }     
*target_recovery_time*     
Especifica el límite máximo de tiempo para recuperar la base de datos especificada en caso de bloqueo.

SECONDS     
Indica que *target_recovery_time* se expresa como el número de segundos.

MINUTES     
Indica que *target_recovery_time* se expresa como el número de minutos.

Para más información sobre los puntos de control indirectos, consulte [Puntos de control de base de datos](../../relational-databases/logs/database-checkpoints-sql-server.md).

ROLLBACK AFTER *integer* [SECONDS] | ROLLBACK IMMEDIATE     
Especifica si la operación de reversión se ejecuta transcurrido un número de segundos determinado o de forma inmediata.

NO_WAIT     
Especifica que la solicitud producirá un error si el cambio de opción o estado de la base de datos solicitado no puede completarse inmediatamente. Completarse inmediatamente significa que no se espera a que las transacciones se confirmen o reviertan por su cuenta.

## <a name="setting-options"></a><a name="SettingOptions"></a> Configuración de opciones
Para recuperar la configuración actual de las opciones de base de datos, use la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) o [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Una vez configurada una opción de la base de datos, la nueva configuración surte efecto de inmediato.

Puede cambiar los valores predeterminados de cualquiera de las opciones de las bases de datos recién creadas. Para ello, cambie la opción adecuada de base de datos en la base de datos de modelo.

## <a name="examples"></a>Ejemplos

### <a name="a-setting-the-database-to-read_only"></a>A. Establecer la base de datos en READ_ONLY
El cambio del estado de una base de datos o un grupo de archivos a READ_ONLY o READ_WRITE requiere el acceso exclusivo a la base de datos. En el ejemplo siguiente, la base de datos se establece en el modo `RESTRICTED_USER` para restringir el acceso. A continuación, el ejemplo establece el estado de la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] en `READ_ONLY` y devuelve el acceso a la base de datos a todos los usuarios.

```sql
USE master;
GO
ALTER DATABASE [database_name]
SET RESTRICTED_USER;
GO
ALTER DATABASE [database_name]
SET READ_ONLY
GO
ALTER DATABASE [database_name]
SET MULTI_USER;
GO
```

### <a name="b-enabling-snapshot-isolation-on-a-database"></a>B. Habilitar el aislamiento de instantánea en una base de datos
En el siguiente ejemplo se habilita la opción del marco de aislamiento de instantánea para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
USE [database_name];
USE master;
GO
ALTER DATABASE [database_name]
SET ALLOW_SNAPSHOT_ISOLATION ON;
GO
-- Check the state of the snapshot_isolation_framework
-- in the database.
SELECT name, snapshot_isolation_state,
    snapshot_isolation_state_desc AS description
FROM sys.databases
WHERE name = N'[database_name]';
GO
```

El conjunto de resultados muestra que el marco de aislamiento de instantánea está habilitado.

|name |snapshot_isolation_state |description|
|-------------------- |------------------------|----------|
|[database_name] |1| ACTIVAR |

### <a name="c-enabling-modifying-and-disabling-change-tracking"></a>C. Habilitar, modificar y deshabilitar el seguimiento de cambios
En el ejemplo siguiente se habilita el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] y se establece el período de retención en `2` días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = ON
(AUTO_CLEANUP = ON, CHANGE_RETENTION = 2 DAYS);
```

En el ejemplo siguiente se muestra cómo cambiar el período de retención a `3` días.

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING (CHANGE_RETENTION = 3 DAYS);
```

En el ejemplo siguiente se muestra cómo deshabilitar el seguimiento de cambios para la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] .

```sql
ALTER DATABASE [database_name]
SET CHANGE_TRACKING = OFF;
```

### <a name="d-enabling-the-query-store"></a>D. Habilitar el Almacén de consultas
En el ejemplo siguiente se habilita el Almacén de consultas y configura los parámetros del Almacén de consultas.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      QUERY_CAPTURE_MODE = AUTO,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60
    );
```

### <a name="e-enabling-the-query-store-with-wait-statistics"></a>E. Habilitar el Almacén de consultas con las estadísticas de espera
En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
      INTERVAL_LENGTH_MINUTES = 60,
      SIZE_BASED_CLEANUP_MODE = AUTO,
      MAX_PLANS_PER_QUERY = 200,
      WAIT_STATS_CAPTURE_MODE = ON,
    );
```

### <a name="f-enabling-the-query-store-with-custom-capture-policy-options"></a>F. Habilitar el Almacén de consultas con las opciones de directiva de captura personalizadas
En el ejemplo siguiente se habilita el Almacén de consultas y configura sus parámetros.

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON
    (
      OPERATION_MODE = READ_WRITE,
      CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = 90 ),
      DATA_FLUSH_INTERVAL_SECONDS = 900,
      MAX_STORAGE_SIZE_MB = 1024,
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

## <a name="see-also"></a>Consulte también

- [Nivel de compatibilidad de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-compatibility-level.md)
- [Creación de reflejo de la base de datos de ALTER DATABASE](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)
- [estadísticas](../../relational-databases/statistics/statistics.md)
- [CREATE DATABASE](../../t-sql/statements/create-database-transact-sql.md?view=azuresqldb-mi-current&preserve-view=true)
- [Habilitar y deshabilitar el seguimiento de cambios](../../relational-databases/track-changes/enable-and-disable-change-tracking-sql-server.md)
- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [SET TRANSACTION ISOLATION LEVEL](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md)
- [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [sys.data_spaces](../../relational-databases/system-catalog-views/sys-data-spaces-transact-sql.md)
- [Procedimiento recomendado con el Almacén de consultas](../../relational-databases/performance/best-practice-with-the-query-store.md)

::: moniker-end
::: moniker range="=azure-sqldw-latest"

:::row:::
    :::column:::
        [SQL Server](alter-database-transact-sql-set-options.md?view=sql-server-ver15&preserve-view=true)
    :::column-end:::
    :::column:::
        [SQL Database](alter-database-transact-sql-set-options.md?view=azuresqldb-current&preserve-view=true)
    :::column-end:::
    :::column:::
        [Instancia administrada de SQL](alter-database-transact-sql-set-options.md?view=azuresqldb-mi-current&preserve-view=true)
    :::column-end:::
    :::column:::
        **_\* Azure Synapse<br />Analytics \*_** &nbsp;
    :::column-end:::
:::row-end:::

&nbsp;

## <a name="azure-synapse-analytics"></a>Azure Synapse Analytics

## <a name="syntax"></a>Sintaxis

```syntaxsql
ALTER DATABASE { database_name }
SET
{
    <optionspec> [ ,...n ]
}
;

<option_spec>::=
{
    <auto_option>
  | <db_encryption_option>
  | <query_store_options>
  | <result_set_caching>
  | <snapshot_option>
}
;

<auto_option> ::=
{
    AUTO_CREATE_STATISTICS { OFF | ON }
}

<db_encryption_option> ::=
{
    ENCRYPTION { ON | OFF }
}

<query_store_option> ::=
{
    QUERY_STORE { OFF |  ON }
}

<result_set_caching_option> ::=
{
    RESULT_SET_CACHING {ON | OFF}
}

<snapshot_option> ::=
{
    READ_COMMITTED_SNAPSHOT {ON | OFF }
}

```

## <a name="arguments"></a>Argumentos

*database_name*     
Es el nombre de la base de datos que se va a modificar.

**<auto_option> ::=**      
Controla las opciones automáticas.

AUTO_CREATE_STATISTICS { **ON** | OFF }     

ACTIVAR     
El optimizador de consultas crea estadísticas en columnas únicas de los predicados de consulta, según sea necesario, para mejorar los planes de consulta y el rendimiento de las consultas. Estas estadísticas de columna única se crean cuando el optimizador de consultas las compila. Las estadísticas de columna única solamente se crean en las columnas que ya no son la primera columna de un objeto de estadísticas existente.

El valor predeterminado es ON. Recomendamos utilizar la configuración predeterminada para la mayoría de las bases de datos.

Apagado     
El optimizador de consultas no crea las estadísticas en columnas únicas de los predicados de consulta cuando compila las consultas. Establecer esta opción en OFF puede producir planes de consulta poco óptimos y un rendimiento degradado de las consultas.

### <a name="remarks"></a>Observaciones
Este comando debe ejecutarse mientras se está conectado a la base de datos de usuario.

Para determinar el estado de esta opción, examine la columna i`s_auto_create_stats_on` en la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md). También puede determinar el estado si examina la propiedad `IsAutoCreateStatistics` de la función [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md).

Para más información, vea la sección "Uso de las opciones de estadísticas de toda la base de datos" de Estadísticas.

**<db_encryption_option> ::=**      
Controla el estado del cifrado de la base de datos.

ENCRYPTION { ON | OFF }     

ACTIVAR     
Establece la base de datos que se va a cifrar.

Apagado     
Establece la base de datos que no se va a cifrar.

Para más información sobre el cifrado de base de datos, vea Cifrado de datos transparente y Cifrado de datos transparente con Azure SQL Database.

Cuando el cifrado está habilitado en el nivel de la base de datos, se cifrarán todos los grupos de archivos. Todos los grupos de archivos nuevos heredarán la propiedad de cifrado. Si en la base de datos hay grupos de archivos establecidos en READ ONLY, se producirá un error en la operación de cifrado de la base de datos.

Puede ver el estado del cifrado de la base de datos, así como el estado del análisis de cifrado, mediante la vista de administración dinámica sys.dm_database_encryption_keys.

**<query_store_option> ::=**      
Controla si el almacén de consultas está habilitado en este almacenamiento de datos.

QUERY_STORE { ON |  **OFF**  }     

ACTIVAR     
Habilita el Almacén de consultas.

Apagado     
Deshabilita el Almacén de consultas. OFF Es el valor predeterminado.

> [!NOTE]
> En [!INCLUDE[ssSDW](../../includes/sssdw-md.md)], debe ejecutar `ALTER DATABASE SET QUERY_STORE` desde la base de datos de usuario. No se admite la ejecución de la instrucción desde otra instancia de almacén de datos.

**<result_set_caching_option> ::=**      
**Se aplica a**: Azure Synapse Analytics  

Controla si el resultado de la consulta se almacena en caché en la base de datos.

RESULT_SET_CACHING { ON | OFF}     

ACTIVAR     
Especifica que los conjuntos de resultados de consulta devueltos por esta base de datos se almacenarán en caché en la base de datos.

Apagado     
Especifica que los conjuntos de resultados de consulta devueltos por esta base de datos no se almacenarán en caché en la base de datos.

### <a name="remarks"></a>Observaciones

Este comando debe ejecutarse mientras se está conectado a la base de datos `master`.  Los cambios realizados a esta configuración de base de datos se aplicarán inmediatamente.  Los costos de almacenamiento se aplican mediante el almacenamiento en caché de conjuntos de resultados de consultas. Después de deshabilitar el almacenamiento en caché de resultados para una base de datos, la caché de resultados que persistía anteriormente se eliminará inmediatamente del almacenamiento de Azure Synapse.

Ejecute este comando para comprobar la configuración del almacenamiento en caché del conjunto de resultados de una base de datos.  Si el almacenamiento en caché del conjunto de resultados está activado, is_result_set_caching_on devolverá 1.

```sql
SELECT name, is_result_set_caching_on FROM sys.databases
WHERE name = <'Your_Database_Name'>
```

Ejecute este comando para comprobar si se ha ejecutado una consulta con un resultado en caché.  La columna result_cache_hit devuelve 1 para aciertos de caché, 0 para errores de caché y valores negativos para los motivos por los que no se ha usado el almacenamiento en caché del conjunto de resultados.  Para más información, consulte [sys.dm_pdw_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md?view=aps-pdw-2016-au7&preserve-view=true).  

```sql
SELECT request_id, command, result_cache_hit FROM sys.dm_pdw_exec_requests
WHERE request_id = <'Your_Query_Request_ID'>
```

> [!IMPORTANT]
> Las operaciones para crear la caché del conjunto de resultados y recuperar los datos de la caché se producen en el nodo de control de una instancia de almacenamiento de datos. Cuando se activa el almacenamiento en caché de conjuntos de resultados, la ejecución de consultas que devuelven un conjunto de resultados grande (por ejemplo, > 1 millón de filas) puede provocar un uso intensivo de la CPU en el nodo de control y ralentizar la respuesta de consulta general en la instancia. Estas consultas se suelen usar durante la exploración de datos o las operaciones ETL. Para evitar el esfuerzo del nodo de control y los problemas de rendimiento, los usuarios deben desactivar el almacenamiento en caché de los conjuntos de resultados en la base de datos antes de ejecutar esos tipos de consultas.  

Para obtener más información sobre la optimización del rendimiento con el almacenamiento en caché de conjuntos de resultados, consulte [Guía de ajuste del rendimiento](/azure/sql-data-warehouse/performance-tuning-result-set-caching).

### <a name="permissions"></a>Permisos

Para establecer la opción RESULT_SET_CACHING, un usuario necesita un inicio de sesión principal de nivel de servidor (el que crea el proceso de aprovisionamiento), o bien ser miembro del rol de la base de datos de `dbmanager`.  

**<snapshot_option> ::=**      
**Se aplica a**: Azure Synapse Analytics

Controla el nivel de aislamiento de transacción de una base de datos.

READ_COMMITTED_SNAPSHOT  { ON | **OFF** }     

ACTIVAR     
Habilita la opción READ_COMMITTED_SNAPSHOT en el nivel de base de datos.

Apagado     
Desactiva la opción READ_COMMITTED_SNAPSHOT en el nivel de base de datos.

### <a name="remarks"></a>Observaciones

Este comando debe ejecutarse mientras se está conectado a la base de datos `master`. La activación o desactivación de READ_COMMITTED_SNAPSHOT para una base de datos de usuario finalizará todas las conexiones abiertas en esta base de datos. Puede que quiera realizar este cambio durante el período de mantenimiento de la base de datos o esperar hasta que no haya ninguna conexión activa a la base de datos, excepto la que ejecuta el comando ALTER DATABSE.  La base de datos no tiene que estar en modo de usuario único. No se admite el cambio del valor READ_COMMITTED_SNAPSHOT en el nivel de sesión.  Para comprobar este valor en una base de datos, consulte la columna is_read_committed_snapshot_on en sys.databases.

En una base de datos con la opción READ_COMMITTED_SNAPSHOT habilitada, las consultas pueden experimentar un rendimiento más lento debido al examen de versiones si hay varias versiones de los datos. Las transacciones de larga duración también pueden provocar un aumento del tamaño de la base de datos. Esta incidencia se produce si estas transacciones cambian los datos que bloquean la limpieza de la versión.  

### <a name="permissions"></a>Permisos

Para establecer la opción READ_COMMITTED_SNAPSHOT, un usuario necesita el permiso ALTER en la base de datos.

## <a name="examples"></a>Ejemplos

### <a name="check-statistics-setting-for-a-database"></a>Comprobación de la configuración de estadísticas para una base de datos

```sql
SELECT name, is_auto_create_stats_on FROM sys.databases
```

### <a name="enable-query-store-for-a-database"></a>Habilitación del almacén de consultas para una base de datos

```sql
ALTER DATABASE [database_name]
SET QUERY_STORE = ON;
```

### <a name="enable-result-set-caching-for-a-database"></a>Habilitación del almacenamiento en caché de conjuntos de resultados para una base de datos

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE [database_name]
SET RESULT_SET_CACHING ON;
```

### <a name="check-result-set-caching-setting-for-a-database"></a>Comprobación de la configuración de almacenamiento en caché de conjuntos de resultados para una base de datos

```sql
SELECT name, is_result_set_caching_on
FROM sys.databases;
```

### <a name="enable-the-read_committed_snapshot-option-for-a-database"></a>Habilitación de la opción Read_Committed_Snapshot para una base de datos

```sql
-- Run this command when connecting to the MASTER database

ALTER DATABASE MyDatabase
SET READ_COMMITTED_SNAPSHOT ON
```

## <a name="see-also"></a>Consulte también

- [DATABASEPROPERTYEX](../../t-sql/functions/databasepropertyex-transact-sql.md)
- [DROP DATABASE](../../t-sql/statements/drop-database-transact-sql.md)
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)
- [Procedimientos recomendadas para Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-best-practices#maintain-statistics)
- [Diseño de tablas en Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-tables-overview#statistics)
- [Elementos de lenguaje de Azure Synapse Analytics](/azure/sql-data-warehouse/sql-data-warehouse-reference-tsql-language-elements)

::: moniker-end
