---
description: sys.event_log (Azure SQL Database)
title: sys.event_log (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 01/28/2019
ms.service: sql-database
ms.prod_service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- event_log
- sys.event_log_TSQL
- event_log_TSQL
- sys.event_log
dev_langs:
- TSQL
helpviewer_keywords:
- event_log
- sys.event_log
ms.assetid: ad5496b5-e5c7-4a18-b5a0-3f985d7c4758
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: = azuresqldb-current
ms.openlocfilehash: ae823cddc60a82c343bc949515493f9405a51a9a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203862"
---
# <a name="sysevent_log-azure-sql-database"></a>sys.event_log (Azure SQL Database)

[!INCLUDE[Azure SQL Database](../../includes/applies-to-version/asdb.md)]

  Devuelve [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] las conexiones de base de datos, los errores de conexión y los interbloqueos correctos. Puede utilizar esta información para realizar el seguimiento de la actividad de la base de datos o solucionar problemas relacionados con esta mediante [!INCLUDE[ssSDS](../../includes/sssds-md.md)].  
  
> [!CAUTION]  
> En el caso de las instalaciones que tienen un gran número de bases de datos o un gran número de inicios de sesión, la actividad en sys.event_log puede producir limitaciones en el rendimiento, un uso elevado de la CPU y, posiblemente, errores de inicio de sesión. Las consultas de sys.event_log pueden contribuir al problema. Microsoft está trabajando para resolver este problema. Mientras tanto, para reducir el impacto de este problema, limite las consultas de sys.event_log. Los usuarios del complemento NewRelic SQL Server deben visitar [Microsoft Azure SQL Database ajuste del complemento & los ajustes de rendimiento](https://discuss.newrelic.com/t/microsoft-azure-sql-database-plugin-tuning-performance-tweaks/30729) de la información de configuración adicional.  
  
 La vista `sys.event_log` contiene las siguientes columnas.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_name**|**sysname**|Nombre de la base de datos. Si la conexión no se realiza correctamente y el usuario no especificó un nombre de base de datos, esta columna está en blanco.|  
|**start_time**|**datetime2**|Fecha y hora UTC del inicio del intervalo de agregación. Para los eventos de agregado, la hora es siempre un múltiplo de 5 minutos. Por ejemplo:<br /><br /> '2011-09-28 16:00:00'<br />'2011-09-28 16:05:00'<br />'2011-09-28 16:10:00'|  
|**end_time**|**datetime2**|Fecha y hora UTC del final del intervalo de agregación. En el caso de los eventos agregados, **End_time** siempre es exactamente 5 minutos después de la **start_time** correspondiente de la misma fila. En el caso de los eventos que no se agregan, **start_time** y **end_time** igual a la fecha y hora UTC reales del evento.|  
|**event_category**|**nvarchar (64)**|Componente de nivel superior que generó este evento.<br /><br /> Vea [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) para obtener una lista de valores posibles.|  
|**event_type**|**nvarchar (64)**|Tipo del evento.<br /><br /> Vea [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) para obtener una lista de valores posibles.|  
|**event_subtype**|**int**|Subtipo del evento que se está produciendo.<br /><br /> Vea [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) para obtener una lista de valores posibles.|  
|**event_subtype_desc**|**nvarchar (64)**|Descripción del subtipo de evento.<br /><br /> Vea [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) para obtener una lista de valores posibles.|  
|**severity**|**int**|Gravedad del error. Los valores posibles son:<br /><br /> 0 = Información<br />1 = Advertencia<br />2 = Error|  
|**event_count**|**int**|Número de veces que se ha producido este evento en la base de datos especificada dentro del intervalo de tiempo especificado (**start_time** y **end_time**).|  
|**description**|**nvarchar(max)**|Descripción detallada del evento.<br /><br /> Vea [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) para obtener una lista de valores posibles.|  
|**additional_data**|**XML**|*Nota: este valor siempre es NULL para Azure SQL Database V12. Vea la sección [ejemplos](#Deadlock) para obtener más sobre cómo recuperar eventos de interbloqueo para V12.*<br /><br /> En el caso de eventos de **interbloqueo** , esta columna contiene el grafo de interbloqueo. Esta columna es NULL para otros tipos de eventos. |  
  
##  <a name="event-types"></a><a name="EventTypes"></a> Tipos de evento

 Los eventos registrados en cada fila de esta vista se identifican mediante una categoría (**event_category**), un tipo de evento (**event_type**) y un subtipo (**event_subtype**). En la tabla siguiente se muestra una lista de los tipos de eventos que se recopilan en esta vista:  
  
 En el caso de los eventos de la categoría **Conectividad** , la información de resumen está disponible en la vista sys.database_connection_stats.  
  
> [!NOTE]  
> Esta vista no incluye todos los posibles errores de base de datos de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] que pueden producirse, solo los mostrados aquí. Es posible que en futuras versiones de [!INCLUDE[ssSDS](../../includes/sssds-md.md)] se agreguen categorías, tipos de evento y subtipos adicionales.  
  
|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**description**|  
|-------------------------|---------------------|------------------------|------------------------------|------------------|---------------------|  
|**Conectividad**|**connection_successful**|0|**connection_successful**|0|Conectado correctamente a la base de datos.|  
|**Conectividad**|**connection_failed**|0|**invalid_login_name**|2|El nombre de inicio de sesión no es válido en esta versión de SQL Server.|  
|**Conectividad**|**connection_failed**|1|**windows_auth_not_supported**|2|Los inicios de sesión de Windows no se admiten en esta versión de SQL Server.|  
|**Conectividad**|**connection_failed**|2|**attach_db_not_supported**|2|El usuario solicitó adjuntar un archivo de base de datos que no se admite.|  
|**Conectividad**|**connection_failed**|3|**change_password_not_supported**|2|El usuario solicitó cambiar la contraseña del usuario que inicia sesión, lo que no se admite.|  
|**Conectividad**|**connection_failed**|4|**login_failed_for_user**|2|Error de inicio de sesión del usuario.|  
|**Conectividad**|**connection_failed**|5|**login_disabled**|2|El inicio de sesión se deshabilitó.|  
|**Conectividad**|**connection_failed**|6|**failed_to_open_db**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> No se pudo abrir la base de datos. Puede ser debido a que la base de datos no existe o a una falta de autenticación para abrirla.|  
|**Conectividad**|**connection_failed**|7|**blocked_by_firewall**|2|No se permite que la dirección IP del cliente tenga acceso al servidor.|  
|**Conectividad**|**connection_failed**|8|**client_close**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> Es posible que el cliente haya agotado el tiempo de espera al establecer la conexión. Intente aumentar el tiempo de espera de la conexión.|  
|**Conectividad**|**connection_failed**|9|**reconfiguración**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> Error de conexión debido a que la base de datos se estaba reconfigurando en ese momento.|  
|**Conectividad**|**connection_terminated**|0|**idle_connection_timeout**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La conexión ha estado inactiva durante más tiempo que el umbral definido por el sistema.|  
|**Conectividad**|**connection_terminated**|1|**reconfiguración**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión se ha terminado debido a una reconfiguración de la base de datos.|  
|**Conectividad**|**limitación**|*\<reason code>*|**reason_code**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> Solicitud limitada.  Código de motivo de la limitación: *\<reason code>* . Para obtener más información, consulte [limitación del motor](/previous-versions/azure/dn338079(v=azure.100)).|  
|**Conectividad**|**throttling_long_transaction**|40549|**long_transaction**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión terminó porque tiene una transacción de larga duración. Intente reducir la transacción. Para obtener más información, consulte [límites de recursos](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Conectividad**|**throttling_long_transaction**|40550|**excessive_lock_usage**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión ha terminado porque ha adquirido demasiados bloqueos. Intente leer o modificar menos filas en una sola transacción. Para obtener más información, consulte [límites de recursos](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Conectividad**|**throttling_long_transaction**|40551|**excessive_tempdb_usage**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión ha terminado debido al uso excesivo de TEMPDB. Intente modificar la consulta para reducir el uso de espacio de la tabla temporal. Para obtener más información, consulte [límites de recursos](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Conectividad**|**throttling_long_transaction**|40552|**excessive_log_space_usage**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión ha terminado debido al excesivo uso de espacio del registro de transacciones. Intente modificar menos filas en una sola transacción. Para obtener más información, consulte [límites de recursos](/previous-versions/azure/dn338081(v=azure.100)).|  
|**Conectividad**|**throttling_long_transaction**|40553|**excessive_memory_usage**|2|*Nota: solo se aplica a Azure SQL Database v11.*<br /><br /> La sesión ha terminado debido al uso excesivo de la memoria. Intente modificar la consulta para procesar menos filas. Para obtener más información, consulte [límites de recursos](/previous-versions/azure/dn338081(v=azure.100)).|  
|**engine**|**deadlock**|0|**deadlock**|2|Se ha producido un interbloqueo.|  
  
## <a name="permissions"></a>Permisos

 Los usuarios con permiso para tener acceso a la base de datos **maestra** tienen acceso de solo lectura a esta vista.  
  
## <a name="remarks"></a>Observaciones  
  
### <a name="event-aggregation"></a>Agregación de eventos

 La información de eventos de esta vista se recopila y se agrega a intervalos de 5 minutos. La columna **event_count** representa el número de veces que se ha producido un **event_type** determinado y **event_subtype** para una base de datos concreta dentro de un intervalo de tiempo determinado.  
  
> [!NOTE]  
> Algunos eventos, como los interbloqueos, no se agregan. Para estos eventos, **event_count** será 1 y **start_time** y **end_time** serán iguales a la fecha y hora UTC reales en que se produjo el evento.  
  
 Por ejemplo, si debido a que el nombre de inicio de sesión no es válido, un usuario intenta conectarse a la base de datos Database1 siete veces entre las 11:00 y las 11:05 el 5/2/2012 (UTC) y no lo consigue, esta información está disponible en una sola fila de esta vista:  
  
|**database_name**|**start_time**|**end_time**|**event_category**|**event_type**|**event_subtype**|**event_subtype_desc**|**severity**|**event_count**|**description**|**additional_data**|  
|------------------------|---------------------|-------------------|-------------------------|---------------------|------------------------|------------------------------|------------------|----------------------|---------------------|--------------------------|  
|`Database1`|`2012-02-05 11:00:00`|`2012-02-05 11:05:00`|`connectivity`|`connection_failed`|`4`|`login_failed_for_user`|`2`|`7`|`Login failed for user.`|`NULL`|  
  
### <a name="interval-start_time-and-end_time"></a>Start_time y end_time de intervalo  
 Se incluye un evento en un intervalo de agregación cuando el evento se produce *en* o _después_ de **start_time** y _antes_ de **end_time** para ese intervalo. Por ejemplo, un evento que ocurra exactamente el `2012-10-30 19:25:00.0000000` solo se incluiría en el segundo intervalo que se muestra a continuación:  
  
```
start_time                    end_time  
2012-10-30 19:20:00.0000000   2012-10-30 19:25:00.0000000  
2012-10-30 19:25:00.0000000   2012-10-30 19:30:00.0000000  
```

### <a name="data-updates"></a>Actualizaciones de datos

 Los datos de esta vista se acumulan a lo largo del tiempo. Normalmente, los datos se acumulan en la hora siguiente al inicio del intervalo de agregación, pero puede llevar hasta 24 horas que todos los datos aparezcan en la vista. Durante ese tiempo, la información de una sola fila puede actualizarse de forma periódica.  
  
### <a name="data-retention"></a>Retención de datos

 Los datos de esta vista se conservan durante un máximo de 30 días, o posiblemente menos, según el número de bases de datos y el número de eventos únicos que genera cada base de datos. Para conservar esta información durante más tiempo, copie los datos en una base de datos independiente. Una vez realizada una copia inicial de la vista, las filas de esta pueden actualizarse a medida que se acumulan datos. Para mantener actualizada su copia de los datos, realice periódicamente una exploración de las filas de la tabla para ver si se ha producido un aumento del número de eventos de las filas existentes y para identificar nuevas filas (se pueden identificar filas únicas usando las horas de inicio y fin), después actualice su copia de los datos con esos cambios.  
  
### <a name="errors-not-included"></a>Errores no incluidos

 Esta vista puede no incluir toda la información de conexión y de error:  
  
- Esta vista no incluye todos los [!INCLUDE[ssSDS](../../includes/sssds-md.md)] errores de base de datos que podrían producirse, solo los especificados en los [tipos de evento](../../relational-databases/system-catalog-views/sys-event-log-azure-sql-database.md#EventTypes) de este tema.  
- Si se produce un error del equipo en el [!INCLUDE[ssSDS](../../includes/sssds-md.md)] centro de datos, es posible que falte una pequeña cantidad de datos en la tabla de eventos.  
- Si se ha bloqueado una dirección IP a través de DoSGuard, los eventos de intento de conexión de esa dirección IP no pueden recopilarse y no aparecerán en esta vista.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="simple-examples"></a>Ejemplos sencillos

 La siguiente consulta devuelve todos los eventos que se produjeron entre el mediodía del 25/9/2011 y el mediodía del 28/9/2011 (UTC). De forma predeterminada, los resultados de la consulta se ordenan por **start_time** (orden ascendente).  

```sql
SELECT * FROM sys.event_log
WHERE start_time >= '2011-09-25 12:00:00'
    AND end_time <= '2011-09-28 12:00:00';  
```

La siguiente consulta devuelve todos los eventos de interbloqueo para la base de datos Database1 (se aplica solo a Azure SQL Database V11).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'deadlock'
    AND database_name = 'Database1';  
```

<a name="Deadlock"></a> La siguiente consulta devuelve todos los eventos de interbloqueo para la base de datos Database1 (se aplica solo a Azure SQL Database V12).  

```sql
WITH CTE AS (  
       SELECT CAST(event_data AS XML)  AS [target_data_XML]  
   FROM sys.fn_xe_telemetry_blob_target_read_file('dl', null, null, null)  
)  
SELECT target_data_XML.value('(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
target_data_XML.query('/event/data[@name=''xml_report'']/value/deadlock') AS deadlock_xml,  
target_data_XML.query('/event/data[@name=''database_name'']/value').value('(/value)[1]', 'nvarchar(100)') AS db_name  
FROM CTE  
```

La siguiente consulta devuelve las limitaciones estrictas sobre los eventos de subprocesos de trabajo de SQL que se produjeron entre las 10:00 y las 11:00 del 25/9/2011 (UTC).  

```sql
SELECT * FROM sys.event_log
WHERE event_type = 'throttling'
    AND event_subtype = 4194307
    AND start_time >= '2011-09-25 10:00:00'
    AND end_time <= '2011-09-25 11:00:00';  
```

### <a name="db-scoped-extended-event"></a>Evento extendido DB-Scoped

 Use el siguiente código de ejemplo para configurar la sesión de eventos extendidos de ámbito de base de BD (XEvent):  

```sql
IF EXISTS  
    (SELECT * from sys.database_event_sessions  
        WHERE name = 'azure_monitor_deadlock_session')  
BEGIN  
    ALTER EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE  
        DROP TARGET package0.ring_buffer;  
  
    DROP EVENT SESSION azure_monitor_deadlock_session  
        ON DATABASE;  
END  
  
CREATE EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    ADD EVENT sqlserver.database_xml_deadlock_report  
    ADD TARGET package0.ring_buffer  
    (  
        SET max_memory = 2048, max_events_limit = 10  
    )  
    WITH (STARTUP_STATE = ON,  
          EVENT_RETENTION_MODE = ALLOW_SINGLE_EVENT_LOSS);  
  
ALTER EVENT SESSION azure_monitor_deadlock_session  
    ON DATABASE  
    STATE = START;  
```

### <a name="check-for-deadlock"></a>Comprobar el interbloqueo

Utilice la siguiente consulta para comprobar si hay un interbloqueo.  

```sql
WITH CTE AS (  
    SELECT CAST(xet.target_data AS XML)  AS [target_data_XML]  
        FROM            sys.dm_xe_database_session_targets AS xet  
             INNER JOIN sys.dm_xe_database_sessions        AS xe  
                 ON (xe.address = xet.event_session_address)  
        WHERE xe.name = 'azure_monitor_deadlock_session'  
)  
, CTE2 AS (  
    SELECT  
            T2.EventData.query('.').value(  
                '(/event/@timestamp)[1]', 'DateTime2') AS Timestamp,  
            T2.EventData.query('.').query(  
                '(/event/data/value/deadlock)[1]')     AS deadlock_xml  
        FROM CTE  
            CROSS Apply [target_data_XML].nodes(  
                '/RingBufferTarget/event') AS T2(EventData)  
)  
SELECT * FROM CTE2;  
```

## <a name="see-also"></a>Consulte también

 [Eventos extendidos en Base de datos SQL de Azure](/azure/azure-sql/database/xevent-db-diff-from-svr)  
