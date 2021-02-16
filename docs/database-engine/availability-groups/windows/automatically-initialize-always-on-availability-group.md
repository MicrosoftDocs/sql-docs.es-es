---
title: Inicialización de un grupo de disponibilidad mediante la propagación automática
description: Use la propagación automática para crear automáticamente réplicas secundarias para todas las bases de datos de un grupo de disponibilidad AlwaysOn sin tener que realizar una copia de seguridad y restaurarla manualmente.
ms.custom: seo-lt-2019
ms.date: 03/26/2018
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
ms.assetid: 67c6a601-677a-402b-b3d1-8c65494e9e96
author: cawrites
ms.author: chadam
ms.openlocfilehash: 13af5b044bfc7f3242e5e69e297b1fe0a9371ac1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340923"
---
# <a name="use-automatic-seeding-to-initialize-an-always-on-availability-group"></a>Uso de la propagación automática para inicializar un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Con SQL Server 2016, se introduce la propagación automática de grupos de disponibilidad. Cuando se crea un grupo de disponibilidad con propagación automática, SQL Server crea automáticamente las réplicas secundarias para cada base de datos del grupo. Ya no es necesario realizar copias de seguridad de las réplicas secundarias ni restaurarlas de forma manual. Para habilitar la propagación automática, cree el grupo de disponibilidad con T-SQL o use la versión más reciente de SQL Server Management Studio.

Para obtener información general, consulte [Propagación automática de réplicas secundarias](automatic-seeding-secondary-replicas.md).
 
## <a name="prerequisites"></a>Prerequisites

En SQL Server 2016, la propagación automática exige que la ruta de acceso del archivo de datos y de registro sea la misma en cada instancia de SQL Server que participe en el grupo de disponibilidad. En SQL Server 2017, puede usar rutas de acceso diferentes. En cambio, Microsoft recomienda usar las mismas rutas cuando todas las réplicas se hospeden en la misma plataforma (por ejemplo, Windows o Linux). Los grupos de disponibilidad multiplataforma usan rutas de acceso diferentes para las réplicas. Para obtener más información, consulte [Diseño de disco](automatic-seeding-secondary-replicas.md#disklayout).

La propagación del grupo de disponibilidad se comunica a través del punto de conexión de creación de reflejo de la base de datos. Abra las reglas de firewall de entrada al puerto del punto de conexión de creación de reflejo en cada servidor.

Las bases de datos de un grupo de disponibilidad deben estar en modelo de recuperación completa. La base de datos debe tener una copia de seguridad completa actual y una copia de seguridad de registro de transacciones. Estos archivos de copia de seguridad no se usan para la propagación automática, pero son necesarios para incluir la base de datos en un grupo de disponibilidad. 
 
## <a name="create-availability-group-with-automatic-seeding"></a>Crear grupos de disponibilidad con propagación automática

Para crear un grupo de disponibilidad con propagación automática, establezca `SEEDING_MODE=AUTOMATIC`. 

En el ejemplo siguiente se crea un grupo de disponibilidad en un clúster de conmutación por error de dos nodos de Windows Server. Antes de ejecutar los scripts, actualice los valores del entorno.

1. Cree los puntos de conexión. Cada servidor necesita un punto de conexión. El script siguiente crea un punto de conexión que usa el puerto TCP 5022 del agente de escucha. Establezca `<endpoint_name>` y `LISTENER_PORT` de modo que coincidan con el entorno y ejecute el script en ambos servidores:

    ```sql
    CREATE ENDPOINT [<endpoint_name>] 
        STATE=STARTED
        AS TCP (LISTENER_PORT = 5022, LISTENER_IP = ALL)
        FOR DATA_MIRRORING (
            ROLE = ALL, 
            AUTHENTICATION = WINDOWS NEGOTIATE, 
            ENCRYPTION = REQUIRED ALGORITHM AES
            )
    GO
    ```

1. Cree el grupo de disponibilidad. El siguiente script crea el grupo de disponibilidad. Actualice los valores en corchetes angulares `<>` del nombre del grupo, los nombres de servidores y los nombres de dominios y ejecútelos en la instancia principal de SQL Server.  

    ```sql
    CREATE AVAILABILITY GROUP [<availability_group_name>]
        FOR DATABASE db1
        REPLICA ON'<*primary_server*>'
        WITH (ENDPOINT_URL = N'TCP://<primary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC),
        N'<secondary_server>' WITH (ENDPOINT_URL = N'TCP://<secondary_server>.<fully_qualified_domain_name>:5022', 
            FAILOVER_MODE = AUTOMATIC, 
            AVAILABILITY_MODE = SYNCHRONOUS_COMMIT, 
            BACKUP_PRIORITY = 50, 
            SECONDARY_ROLE(ALLOW_CONNECTIONS = NO), 
            SEEDING_MODE = AUTOMATIC);
    GO
    ``` 

1. Una la instancia del servidor secundario al grupo de disponibilidad y conceda permiso al grupo de disponibilidad para crear bases de datos. Actualice el script siguiente; sustituya los valores en corchetes angulares `<>` de su entorno y ejecútelo en la instancia de réplica de SQL Server: 
 
    ```sql
    ALTER AVAILABILITY GROUP [<availability_group_name>] JOIN
    GO  
    ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
    GO
    ```

SQL Server creará automáticamente la réplica de base de datos en el servidor secundario. Si la base de datos es grande, completar la sincronización de la base de datos puede llevar algún tiempo. Si una base de datos está en un grupo de disponibilidad configurado para la propagación automática, puede consultar la vista del sistema `sys.dm_hadr_automatic_seeding` para supervisar el progreso de la propagación. La siguiente consulta devuelve una fila por cada base de datos que esté en un grupo de disponibilidad configurado para la propagación automática.

```sql 
SELECT start_time,
    ag.name,
    db.database_name,
    current_state,
    performed_seeding,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding autos 
    JOIN sys.availability_databases_cluster db 
        ON autos.ag_db_id = db.group_database_id
    JOIN sys.availability_groups ag 
        ON autos.ag_id = ag.group_id
```

## <a name="prevent-automatic-seeding-after-an-availability-group"></a>Evitar la propagación automática después de un grupo de disponibilidad

Para evitar temporalmente que la réplica principal propague más bases de datos a la réplica secundaria, puede denegar el permiso para crear bases de datos al grupo de disponibilidad. Ejecute la consulta siguiente en la instancia que hospeda la réplica secundaria para denegar al grupo de disponibilidad el permiso para crear bases de datos de réplica.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    DENY CREATE ANY DATABASE
GO
```


## <a name="enable-automatic-seeding-on-an-existing-availability-group"></a>Habilitar la propagación automática en un grupo de disponibilidad existente

Puede establecer la propagación automática en una base de datos existente. El siguiente comando modificará un grupo de disponibilidad para usar la propagación automática. Ejecute el comando siguiente en la réplica principal.

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>' 
    WITH (SEEDING_MODE = AUTOMATIC)
GO
```

El comando anterior obliga a una base de datos a reiniciar la propagación si es necesario. Por ejemplo, si se produce un error de propagación debido a una insuficiencia de espacio en disco en la réplica secundaria, puede ejecutar `ALTER AVAILABILITY GROUP ... WITH (SEEDING_MODE=AUTOMATIC)` para reiniciar la propagación después de agregar espacio libre.

## <a name="stop-automatic-seeding"></a>Detener la propagación automática

Para detener la propagación automática de un grupo de disponibilidad, ejecute el script siguiente en la réplica principal:

```sql
ALTER AVAILABILITY GROUP [<availability_group_name>] 
    MODIFY REPLICA ON '<secondary_node>'   
    WITH (SEEDING_MODE = MANUAL)
GO
```

Con el script anterior, se cancelarán las réplicas que se estén propagando en ese momento y se evitará que SQL Server inicialice automáticamente cualquier réplica en este grupo de disponibilidad. No se detendrá la sincronización de las réplicas que ya se hayan inicializado. 


## <a name="monitor-automatic-seeding-availability-group"></a>Supervisar grupos de disponibilidad de propagación automática

### <a name="use-system-dynamic-management-views-to-monitor-seeding"></a>Usar vistas de administración dinámica del sistema para supervisar la propagación

Las siguientes vistas del sistema muestran el estado de la propagación automática de SQL Server.

**sys.dm_hadr_automatic_seeding** 

En la réplica principal, consulte `sys.dm_hadr_automatic_seeding` para comprobar el estado del proceso de propagación automática. La vista devuelve una fila por cada proceso de propagación. Por ejemplo:

```sql
SELECT start_time, 
    completion_time
    is_source,
    current_state,
    failure_state,
    failure_state_desc
FROM sys.dm_hadr_automatic_seeding
```
 
**sys.dm_hadr_physical_seeding_stats** 

En la réplica principal, consulte la vista de administración dinámica `sys.dm_hadr_physical_seeding_stats` para ver las estadísticas físicas de cada proceso de propagación que se esté ejecutando en ese momento. La siguiente consulta devuelve filas cuando se está ejecutando la propagación:

```sql
SELECT * FROM sys.dm_hadr_physical_seeding_stats;
```

Las columnas *total_disk_io_wait_time_ms* y *total_network_wait_time_ms* se pueden usar para determinar el cuello de botella del rendimiento en el proceso Propagación automática. Las dos columnas también están presentes en el evento extendido *hadr_physical_seeding_progress*.

**total_disk_io_wait_time_ms** representa el tiempo empleado por el subproceso de copia de seguridad/restauración durante la espera en el disco. Este valor es acumulativo desde el inicio de la operación de propagación. Si los discos no están listos para leer o escribir la secuencia de copia de seguridad, el subproceso de copia de seguridad/restauración pasará a un estado de suspensión y se reactivará cada segundo para comprobar si el disco está listo.
        
**total_network_wait_time_ms** se interpreta de otra manera para las réplicas principal y secundaria. En la réplica principal, este contador representa el tiempo de control del flujo de red. En la réplica secundaria, representa el tiempo que espera el subproceso de restauración hasta que haya un mensaje disponible para escribirlo en el disco.

### <a name="diagnose-database-initialization-using-automatic-seeding-in-the-error-log"></a>Diagnosticar la inicialización de la base de datos mediante la propagación automática en el registro de errores

Al agregar una base de datos a un grupo de disponibilidad configurado para la propagación automática, SQL Server realiza una copia de seguridad VDI a través del punto de conexión del grupo de disponibilidad. Revise el registro de errores de SQL Server para obtener información sobre cuándo se completó la copia de seguridad y cuándo se sincronizó la secundaria.

### <a name="diagnose-database-level-health-with-extended-events"></a>Diagnosticar el estado de nivel de base de datos con eventos extendidos

La propagación automática tiene nuevos eventos extendidos para realizar el seguimiento de las modificaciones de estado, los errores y las estadísticas de rendimiento durante la inicialización. 

Por ejemplo, este script crea una sesión de eventos extendidos que capture eventos relacionados con la propagación automática: 

```sql
CREATE EVENT SESSION [AlwaysOn_autoseed] ON SERVER 
    ADD EVENT sqlserver.hadr_automatic_seeding_state_transition,
    ADD EVENT sqlserver.hadr_automatic_seeding_timeout,
    ADD EVENT sqlserver.hadr_db_manager_seeding_request_msg,
    ADD EVENT sqlserver.hadr_physical_seeding_backup_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_failure,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_forwarder_target_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_progress,
    ADD EVENT sqlserver.hadr_physical_seeding_restore_state_change,
    ADD EVENT sqlserver.hadr_physical_seeding_submit_callback
    ADD TARGET package0.event_file(
        SET filename=N'autoseed.xel',
            max_file_size=(5),
            max_rollover_files=(4)
        )
WITH (
    MAX_MEMORY=4096 KB,
    EVENT_RETENTION_MODE=ALLOW_SINGLE_EVENT_LOSS,
    MAX_DISPATCH_LATENCY=30 SECONDS,
    MAX_EVENT_SIZE=0 KB,
    MEMORY_PARTITION_MODE=NONE,
    TRACK_CAUSALITY=OFF,
    STARTUP_STATE=ON
    )
GO 

ALTER EVENT SESSION AlwaysOn_autoseed ON SERVER STATE=START
GO 
```


La tabla siguiente enumera los eventos extendidos relacionados con la propagación automática: 

| Nombre | Descripción|
|------------ |---------------| 
|hadr_db_manager_seeding_request_msg |  Mensaje de solicitud propagación.
|hadr_physical_seeding_backup_state_change |    Cambio de estado del lado de copia de seguridad de propagación física.
|hadr_physical_seeding_restore_state_change |Cambio de estado del lado de restauración de propagación física.
|hadr_physical_seeding_forwarder_state_change | Cambio de estado del lado de reenviador de propagación física.
|hadr_physical_seeding_forwarder_target_state_change |  Cambio de estado del lado de destino de reenviador de propagación física.
|hadr_physical_seeding_submit_callback  |Evento de devolución de llamada del envío de propagación física.
|hadr_physical_seeding_failure  |Evento de error de propagación física.
|hadr_physical_seeding_progress |   Evento de progreso de propagación física.
|hadr_physical_seeding_schedule_long_task_failure   |Evento de error de tarea larga de programación de propagación física.
|hadr_automatic_seeding_start   |Se produce cuando se envía una operación de propagación automática.
|hadr_automatic_seeding_state_transition    |Se produce cuando cambia el estado de una operación de propagación automática.
|hadr_automatic_seeding_success |Se produce cuando se realiza correctamente una operación de propagación automática.
|hadr_automatic_seeding_failure |Se produce cuando se realiza una operación de propagación automática incorrectamente.
|hadr_automatic_seeding_timeout |Se produce cuando se agota el tiempo de espera de una operación de propagación automática.

### <a name="other-troubleshooting-considerations"></a>Otras consideraciones de solución de problemas

**Supervisar la propagación automática**

Consulte `sys.dm_hadr_physical_seeding_stats` para ver los procesos de propagación automática en ejecución. La vista devuelve una fila por cada base de datos. Por ejemplo:

```sql
SELECT local_database_name, 
    role_desc, 
    internal_state_desc, 
    transfer_rate_bytes_per_second, 
    transferred_size_bytes, 
    database_size_bytes, 
    start_time_utc, 
    end_time_utc, estimate_time_complete_utc, 
    total_disk_io_wait_time_ms, 
    total_network_wait_time_ms, 
    is_compression_enabled 
FROM sys.dm_hadr_physical_seeding_stats
```

**Solucionar el problema de que una base de datos no aparezca en un grupo de disponibilidad configurado para la propagación automática**


Cuando una base de datos no aparece como parte de un grupo de disponibilidad con la propagación automática habilitada, es probable que hubiera un error en la propagación automática. Esto evita la adición de la base de datos al grupo de disponibilidad en la réplica principal y secundaria. Consulte `sys.dm_hadr_automatic_seeding` en las réplicas principal y secundaria. Por ejemplo, ejecute la consulta siguiente para identificar el estado de error de propagación automática.

```sql
SELECT start_time, 
    completion_time, 
    is_source, 
    current_state, 
    failure_state, 
    failure_state_desc, 
    error_code 
FROM sys.dm_hadr_automatic_seeding
```

## <a name="automatic-seeding-and-performance-considerations"></a>Propagación automática y consideraciones de rendimiento

SQL Server usa un número fijo de subprocesos para la propagación automática. En la instancia principal, SQL Server usa un subproceso por LUN para leer los cambios. En la instancia secundaria, SQL Server usa un subproceso por LUN para inicializar la base de datos.

Establezca la marca de seguimiento 9567 en la réplica principal para habilitar la compresión del flujo de datos durante la propagación automática. Esto puede reducir considerablemente el tiempo de transferencia de propagación automática, aunque también aumentará el uso de CPU. Para obtener más información, consulte [Tune compression for availability group (Optimizar la compresión para el grupo de disponibilidad)](../../../database-engine/availability-groups/windows/tune-compression-for-availability-group.md). 


## <a name="when-not-to-use-automatic-seeding"></a>Cuándo no usar la propagación automática

En algunos casos, la propagación automática puede no ser óptima para inicializar una réplica secundaria. Durante la propagación automática, SQL Server realiza una copia de seguridad a través de la red para la inicialización. Este proceso puede ser lento si las bases de datos son muy grandes o si la réplica secundaria es remota. No se puede truncar el registro de transacciones de estas bases de datos durante el proceso de copia de seguridad, por lo que un proceso de inicialización prolongado en una base de datos ocupada puede dar lugar a un crecimiento considerable del registro de transacciones.
Antes de agregar una base de datos a un grupo de disponibilidad con propagación automática, evalúe el tamaño de la base de datos, la carga y la distancia al sitio entre réplicas.

## <a name="resources"></a>Recursos

[CREATE AVAILABILITY GROUP (Transact-SQL)](../../../t-sql/statements/create-availability-group-transact-sql.md)

[AlwaysOn Availability Groups Troubleshooting and Monitoring Guide](/previous-versions/sql/sql-server-guides/dn135328(v=sql.110)) (Guía de solución de problemas y supervisión de grupos de disponibilidad AlwaysOn)