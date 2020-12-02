---
description: Administrar y supervisar la captura de datos modificados (SQL Server)
title: Administrar y supervisar la captura de datos modificados
ms.date: 01/02/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- change data capture [SQL Server], monitoring
- change data capture [SQL Server], administering
- change data capture [SQL Server], jobs
ms.assetid: 23bda497-67b2-4e7b-8e4d-f1f9a2236685
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: dfc040c4157cfd44a27a0b05a3bef42a1591aa2e
ms.sourcegitcommit: 192f6a99e19e66f0f817fdb1977f564b2aaa133b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/25/2020
ms.locfileid: "96128645"
---
# <a name="administer-and-monitor-change-data-capture-sql-server"></a>Administrar y supervisar la captura de datos modificados (SQL Server)

[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo administrar y supervisar la captura de datos modificados.  
  
## <a name="capture-job"></a><a name="Capture"></a> Trabajo de captura

El trabajo de captura se inicia ejecutando el procedimiento almacenado sin parámetros `sp_MScdc_capture_job`. Este procedimiento almacenado empieza extrayendo los valores configurados para `maxtrans`, `maxscans`, `continuous` y `pollinginterval` para el trabajo de captura desde msdb.dbo.cdc_jobs. A continuación, estos valores configurados se pasan como parámetros al procedimiento almacenado `sp_cdc_scan`. Esto se utiliza para llamar a `sp_replcmds` con el fin de realizar el examen del registro.  
  
### <a name="capture-job-parameters"></a>Parámetros de trabajo de captura  

Para entender el comportamiento del trabajo de captura, debe saber cómo `sp_cdc_scan` utiliza los parámetros configurables.
  
#### <a name="maxtrans-parameter"></a>Parámetro maxtrans

El parámetro `maxtrans` especifica el número máximo de transacciones que se pueden procesar en un único ciclo de examen del registro. Si, durante el examen, el número de transacciones que se van a procesar alcanza este límite, no se incluye ninguna transacción adicional en el examen actual. Cuando finaliza un ciclo de examen, el número de transacciones que se procesaron siempre será menor o igual que `maxtrans`.

#### <a name="maxscans-parameter"></a>Parámetro maxscans

El parámetro `maxscans` especifica el número máximo de ciclos de examen que se intentan para agotar el registro antes de devolver (continuous = 0) o ejecutar un waitfor (continuous = 1).

#### <a name="continuous-parameter"></a>continuous (parámetro)

El parámetro `continuous` controla si `sp_cdc_scan` abandona el control después de agotar el registro o ejecutar el número máximo de ciclos de examen (modo de una instantánea). También controla si `sp_cdc_scan` se sigue ejecutando hasta detenerse explícitamente (modo continuo).  
  
##### <a name="one-shot-mode"></a>Modo monoestable

En el modo monoestable, el trabajo de captura solicita que `sp_cdc_scan` realice un máximo de `maxtrans` exámenes para intentar agotar el registro y volver. Cualquier transacción adicional a `maxtrans` que se encuentre en el registro se procesará en exámenes posteriores.  
  
 El modo monoestable se usa en pruebas controladas, en las que se conoce el volumen de las transacciones que se van a procesar, y hay ventajas en el hecho de que el trabajo se cierra automáticamente en cuanto finaliza. No se recomienda usar el modo monoestable en producción. Esto se debe a que se basa en la programación de trabajos para administrar la frecuencia con la que se ejecuta el ciclo de examen.  
  
 La ejecución en el modo monoestable permite calcular un límite superior en el rendimiento esperado del trabajo de captura, expresado en transacciones por segundo, mediante el cálculo siguiente:  
  
 `(maxtrans * maxscans) / number of seconds between scans`  
  
 Incluso si el tiempo que se necesitase para examinar el registro y rellenar las tablas de cambios no fuese significativamente diferente de 0, el rendimiento medio del trabajo podría no superar el valor obtenido dividiendo el resultado de multiplicar el número máximo de transacciones permitidas para un solo examen por el número máximo permitido de exámenes entre el número de segundos que separan el procesamiento de registros.  
  
 Si se usara el modo monoestable para regular el examen de registros, la programación del trabajo tendría que regir el número de segundos entre el procesamiento de registros. Cuando se desea este tipo de comportamiento, ejecutar el trabajo de captura en el modo continuo es el método mejor para administrar la reprogramación del examen de registros.  
  
##### <a name="continuous-mode-and-the-polling-interval"></a>Modo continuo e intervalo de sondeo

En el modo continuo, el trabajo de captura solicita que `sp_cdc_scan` se ejecute continuamente. Esto permite que el procedimiento almacenado administre su propio bucle de espera proporcionando no solo los valores de los parámetros maxtrans y maxscans sino también un valor para el número de segundos entre el procesamiento de registros (el intervalo de sondeo). Al ejecutarse en este modo, el trabajo de captura sigue estando activo, ejecutando un `WAITFOR` entre el examen de registros.  
  
> [!NOTE]  
> Cuando el valor del intervalo de sondeo es mayor que 0, el mismo límite superior para el rendimiento del trabajo monoestable repetido se aplica también al funcionamiento del trabajo en el modo continuo. Es decir, (`maxtrans` \* `maxscans`) dividido por un intervalo de sondeo distinto de cero impone un límite superior en el número medio de transacciones que puede procesar el trabajo de captura.  
  
### <a name="capture-job-customization"></a>Personalización del trabajo de captura

En el trabajo de captura se puede aplicar una lógica adicional para determinar si un examen nuevo comienza inmediatamente o si se impone una suspensión antes de iniciarse en lugar de basarse en un intervalo de sondeo fijo. La opción podría basarse simplemente en la hora del día, exigiendo quizás suspensiones muy largas durante las horas de actividad máxima, e incluso el paso a un intervalo de sondeo igual a 0 al final del día, cuando es importante completar el procesamiento de los días y preparar las ejecuciones nocturnas. El progreso del proceso de captura también se puede supervisar para determinar el momento en que todas las transacciones confirmadas a media noche han sido examinadas y depositadas en las tablas de cambios. Esto permite que el trabajo de captura finalice y se reinicie de la forma programada diariamente. Al reemplazar el paso de trabajo entregado llamando a `sp_cdc_scan` con una llamada a un contenedor escrito por el usuario para `sp_cdc_scan`, se puede obtener un comportamiento muy personalizado con poco esfuerzo adicional.  

## <a name="cleanup-job"></a><a name="Cleanup"></a> Trabajo de limpieza

En esta sección se proporciona información sobre cómo funciona el trabajo de limpieza de la captura de datos modificados.  
  
### <a name="structure-of-the-cleanup-job"></a>Estructura del trabajo de limpieza

La captura de datos modificados usa una estrategia de limpieza basada en la retención para administrar el tamaño de la tabla de cambios. El mecanismo de limpieza consta de un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)][!INCLUDE[tsql](../../includes/tsql-md.md)] que se crea cuando se habilita la primera tabla de la base de datos. Un único trabajo de limpieza controla la limpieza para todas las tablas de cambios de base de datos y aplica el mismo valor de retención a todas las instancias de captura definidas.
  
El trabajo de limpieza se inicia ejecutando el procedimiento almacenado sin parámetros `sp_MScdc_cleanup_job`. Este procedimiento almacenado comienza extrayendo los valores configurados para la retención y el umbral del trabajo de limpieza de `msdb.dbo.cdc_jobs`. El valor de retención se utiliza para calcular una nueva marca de límite inferior para las tablas de cambios. El número especificado de minutos se resta del valor de `tran_end_time` máximo en la tabla `cdc.lsn_time_mapping` para obtener la nueva marca de límite inferior expresada como un valor datetime. A continuación, la tabla CDC.lsn_time_mapping se utiliza para convertir este valor datetime en un valor de `lsn` correspondiente. Si varias entradas comparten el mismo tiempo de confirmación en la tabla, el `lsn` que corresponde a la entrada que tiene el valor de `lsn` más bajo se elige como nueva marca de límite inferior. Este valor de `lsn` se pasa al procedimiento `sp_cdc_cleanup_change_tables` para quitar las entradas de las tablas de cambios de base de datos.  
  
> [!NOTE]  
> La ventaja de utilizar el tiempo de confirmación de la transacción reciente como base para calcular la nueva marca de límite inferior es que permite que los cambios permanezcan en las tablas de cambios durante el tiempo especificado. Esto sucede incluso cuando el proceso de captura se ejecuta en segundo plano. Todas las entradas que tienen el mismo tiempo de confirmación que la marca de límite inferior actual continúan siendo representadas dentro de las tablas de cambios si se elige el valor de `lsn` más bajo que tenga el tiempo de confirmación compartido para la marca de límite inferior real.  

Cuando se realiza una limpieza, la marca de límite inferior para todas las instancias de captura se actualiza inicialmente en una única transacción. A continuación, intenta quitar las entradas obsoletas de las tablas de cambios y de la tabla cdc.lsn_time_mapping. El valor de umbral configurable limita el número de entradas que se eliminan en una única instrucción. El que no se pueda realizar la eliminación en alguna tabla individual no impide que la operación se intente en las tablas restantes.  
  
### <a name="cleanup-job-customization"></a>Personalización del trabajo de limpieza

 La posibilidad de personalización de los trabajos de limpieza radica en la estrategia que se usa para determinar qué entradas de la tabla de cambios se van a descartar. La única estrategia admitida en el trabajo de limpieza entregado es la que se basa en el tiempo. En esa situación, la nueva marca de límite inferior se calcula restando el período de retención permitido del tiempo de confirmación de la última transacción procesada. Como los procedimientos de limpieza subyacentes se basan en `lsn` en lugar de en el tiempo, se puede usar cualquier cantidad de estrategias para determinar el valor `lsn` más bajo que se debe conservar en las tablas de cambios. Solo algunas se basan estrictamente en el tiempo. Por ejemplo, el conocimiento sobre los clientes se puede utilizar para proporcionar un seguro si los procesos de niveles inferiores que requieren acceso a las tablas de cambios no se pueden ejecutar. Además, aunque la estrategia predeterminada aplica el mismo `lsn` para limpiar las tablas de cambios de todas las bases de datos, también se puede llamar al procedimiento de limpieza subyacente para limpiar en el nivel de la instancia de captura.  

## <a name="monitor-the-change-data-capture-process"></a><a name="Monitor"></a> Supervisar el proceso de captura de datos modificados

Supervisar el proceso de captura de datos modificados permite determinar si los cambios se están escribiendo correctamente y con una latencia razonable en las tablas de cambios. La supervisión también puede ayudar a identificar los errores que puedan producirse. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] incluye dos vistas de administración dinámica para ayudar a supervisar la captura de datos modificados: [sys.dm_cdc_log_scan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md) y [sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md).  
  
### <a name="identify-sessions-with-empty-result-sets"></a>Identificar las sesiones con conjuntos de resultados vacíos

Cada fila de sys.dm_cdc_log_scan_sessions representa una sesión de examen del registro (excepto la fila con el identificador 0). Una sesión de examen del registro es equivalente a una ejecución de [sp_cdc_scan](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md). Durante una sesión, el examen puede devolver los cambios o un resultado vacío. Si el conjunto de resultados está vacío, la columna empty_scan_count de sys.dm_cdc_log_scan_sessions se establece en 1. Si hay conjuntos de resultados vacíos consecutivos, por ejemplo si el trabajo de captura se está ejecutando continuamente, el valor empty_scan_count de la última fila existente se incrementa. Por ejemplo, si sys.dm_cdc_log_scan_sessions ya contiene 10 filas para los exámenes que devolvieron los cambios y hay seguidos cinco resultados vacíos, la vista contiene 11 filas. La última fila tiene el valor 5 en la columna empty_scan_count. Para determinar las sesiones que tenían un examen vacío, ejecute la consulta siguiente:  

```sql
SELECT * from sys.dm_cdc_log_scan_sessions where empty_scan_count <> 0
```

### <a name="determine-latency"></a>Determinar la latencia

La vista de administración `sys.dm_cdc_log_scan_sessions` incluye una columna que registra la latencia de cada sesión de captura. La latencia se define como el tiempo transcurrido entre que una transacción se confirma en una tabla de origen y la confirmación en la tabla de cambios de la última transacción capturada. La columna de latencia solo se rellena para las sesiones activas. Para las sesiones con un valor mayor que 0 en la columna empty_scan_count, la columna de latencia se establece en 0. La consulta siguiente devuelve la latencia promedio para las sesiones más recientes:  

```sql
SELECT latency FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0
```

Puede utilizar los datos de la latencia para determinar la rapidez o lentitud con que el proceso de captura procesa las transacciones. Estos datos son muy útiles cuando el proceso de captura se está ejecutando continuamente. Si el proceso de captura se ejecuta según una programación, la latencia puede ser alta debido a la diferencia temporal entre las transacciones que se confirman en la tabla de origen y el proceso de captura que se ejecuta en su momento programado.  
  
 Otra medida importante de la eficacia del proceso de captura es el rendimiento. Se trata del número promedio de comandos por segundo que se procesan durante cada sesión. Para determinar el rendimiento de una sesión, divida el valor de la columna command_count por el valor de la columna de duración. La consulta siguiente devuelve el rendimiento promedio para las sesiones más recientes:  

```sql
SELECT command_count/duration AS [Throughput] FROM sys.dm_cdc_log_scan_sessions WHERE session_id = 0
```

### <a name="use-data-collector-to-collect-sampling-data"></a>Usar el recopilador de datos para recopilar datos de muestreo

El recopilador de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite reunir instantáneas de datos de cualquier tabla o vista de administración dinámica y generar un almacenamiento de datos de rendimiento. Cuando la captura de datos modificados está habilitada en una base de datos, es útil tomar instantáneas de las vistas sys.dm_cdc_log_scan_sessions y sys.dm_cdc_errors a intervalos regulares para analizarlas posteriormente. El procedimiento siguiente configura un recopilador de datos para reunir datos de muestra de la vista de administración sys.dm_cdc_log_scan_sessions.

#### <a name="configuring-data-collection"></a>Configurar la recopilación de datos

1. Habilite el recopilador de datos y configure un almacén de administración de datos. Para obtener más información, vea [Administrar la recopilación de datos](../../relational-databases/data-collection/manage-data-collection.md).  
  
2. Ejecute el código siguiente para crear un recopilador personalizado para la captura de datos modificados.  

    ```sql  
    USE msdb;  
  
    DECLARE @schedule_uid uniqueidentifier;  

    -- Collect and upload data every 5 minutes  
    SELECT @schedule_uid = (  
    SELECT schedule_uid from sysschedules_localserver_view
    WHERE name = N'CollectorSchedule_Every_5min')  
  
    DECLARE @collection_set_id int;  
  
    EXEC dbo.sp_syscollector_create_collection_set  
    @name = N' CDC Performance Data Collector',  
    @schedule_uid = @schedule_uid,
    @collection_mode = 0,
    @days_until_expiration = 30,
    @description = N'This collection set collects CDC metadata',  
    @collection_set_id = @collection_set_id output;  
  
    -- Create a collection item using statistics from
    -- the change data capture dynamic management view.  
    DECLARE @parameters xml;  
    DECLARE @collection_item_id int;  
  
    SELECT @parameters = CONVERT(xml,
        N'<TSQLQueryCollector>  
            <Query>  
              <Value>SELECT * FROM sys.dm_cdc_log_scan_sessions</Value>  
              <OutputTable>cdc_log_scan_data</OutputTable>  
            </Query>  
          </TSQLQueryCollector>');  
  
    EXEC dbo.sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = N'302E93D1-3424-4BE7-AA8E-84813ECF2419',  
    @name = ' CDC Performance Data Collector',  
    @frequency = 5,
    @parameters = @parameters,  
    @collection_item_id = @collection_item_id output;
  
    GO  
    ```  
  
3. En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], expanda **Administración** y, a continuación, expanda **Recopilación de datos**. Haga clic con el botón derecho en **Recopilador de datos de rendimiento de CDC** y, después, haga clic en **Iniciar conjunto de recopilación de datos**.  
  
4. En el almacenamiento de datos que configuró en el paso 1, busque la tabla custom_snapshots.cdc_log_scan_data. En esta tabla se proporciona una instantánea histórica de los datos de las sesiones de examen del registro. Estos datos se pueden utilizar para analizar la latencia, el rendimiento y otras medidas de rendimiento a lo largo del tiempo.  

## <a name="script-upgrade-mode"></a><a name="ScriptUpgrade"></a> Modo de actualización de script

Cuando se aplican Service Pack o actualizaciones acumulativas a una instancia, durante el reinicio, la instancia puede entrar en modo de actualización de script. En este modo, SQL Server puede ejecutar un paso para analizar y actualizar las tablas de CDC internas, lo cual puede dar lugar a que se vuelvan a crear objetos, como los índices de las tablas de captura. Según la cantidad de datos implicados, este paso puede ser lento o generar un uso elevado del registro de transacciones para las bases de datos CDC habilitadas.


## <a name="see-also"></a>Vea también

- [Seguimiento de cambios de datos &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)
- [Acerca de la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)
- [Habilitar y deshabilitar la captura de datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/enable-and-disable-change-data-capture-sql-server.md)
- [Trabajar con datos modificados &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)  
