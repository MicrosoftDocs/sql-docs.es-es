---
title: Destinos para eventos extendidos en SQL Server
description: En este artículo se explican los destinos package0 para eventos extendidos en SQL Server. Obtenga información sobre las funciones de destino en la recopilación y generación de informes de datos y parámetros de destino.
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: conceptual
ms.assetid: 47c64144-4432-4778-93b5-00496749665b
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ee107ed1d1e9fe7ec7aa8b40c87c71750b5cdf99
ms.sourcegitcommit: 3e2421ae45a8e9fa57fb590a5d1a5566721ea74a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98921393"
---
# <a name="targets-for-extended-events-in-sql-server"></a>Destinos para eventos extendidos en SQL Server

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]


En este artículo se explica cuándo y cómo usar los destinos package0 para eventos extendidos en SQL Server. Para cada destino, en el presente artículo se explican:

- Sus capacidades de recopilar y comunicar los datos enviados por eventos.
- Sus parámetros, excepto cuando el parámetro se explica por sí mismo.


#### <a name="xquery-example"></a>Ejemplo de XQuery


En la [sección ring_buffer](#h2_target_ring_buffer) se incluye un ejemplo de cómo usar [XQuery en Transact-SQL](../../xquery/xquery-language-reference-sql-server.md) para copiar una cadena de XML en un conjunto de filas relacional.


### <a name="prerequisites"></a>Prerrequisitos


- Es necesario estar familiarizado de forma general con los aspectos básicos de los eventos extendidos, como se describe en [Inicio rápido: eventos extendidos en SQL Server](../../relational-databases/extended-events/quick-start-extended-events-in-sql-server.md).


- Tener instalada una versión reciente de la utilidad SQL Server Management Studio (SSMS.exe) actualizada con frecuencia. Para obtener detalles, vea:
    - [Descarga de SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md)


- En SSMS.exe, aprenda a usar el **Explorador de objetos** y haga clic con el botón derecho en el nodo de destino en la sesión de eventos, a fin de [facilitar la visualización de los datos de salida](../../relational-databases/extended-events/advanced-viewing-of-target-data-from-extended-events-in-sql-server.md).
    - Los datos del evento se capturan como cadena XML. Sin embargo, en este artículo, los datos se muestran en filas relacionales. SSMS se usó para ver los datos y, a continuación, se copió y pegó en este artículo.
    - La técnica de T-SQL alternativa para generar conjuntos de filas desde XML se explica en la [sección ring_buffer](#h2_target_ring_buffer). Implica XQuery.



## <a name="parameters-actions-and-fields"></a>Parámetros, acciones y campos


En Transact-SQL, la instrucción [CREATE EVENT SESSION](~/t-sql/statements/create-event-session-transact-sql.md) es fundamental para los eventos extendidos. Para escribir la instrucción, a menudo necesita una lista y una descripción de lo siguiente:

- Los campos asociados al evento elegido.
- Los parámetros asociados al destino elegido.

Las instrucciones SELECT que devuelven dichas listas de las vistas del sistema están disponibles para copiarse desde el siguiente artículo, en su sección C:

- [Instrucciones SELECT y JOIN en vistas del sistema para eventos extendidos en SQL Server](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md)
    - [C.4](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) Campos SELECT para un evento.
    - [C.6](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_6_parameters_targets) Parámetros SELECT para un destino.
    - [C.3](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) Acciones SELECT.


Puede ver los parámetros, campos y acciones que se usan en el contexto de una instrucción CREATE EVENT SESSION real, en [este vínculo](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_B_2_TSQL_perspective).



<a name="h2_target_etw_classic_sync_target"></a>

## <a name="etw_classic_sync_target-target"></a>Destino etw_classic_sync_target


Los eventos extendidos de SQL Server pueden interoperar con Seguimiento de eventos para Windows (ETW) a fin de supervisar la actividad del sistema. Para más información, consulte:

- [Seguimiento de eventos para Windows como destino](../../relational-databases/extended-events/event-tracing-for-windows-target.md)
- [Supervisar la actividad del sistema mediante eventos extendidos](../../relational-databases/extended-events/monitor-system-activity-using-extended-events.md)


Este destino ETW procesa *de forma sincrónica* los datos que recibe, mientras que la mayoría de los destinos los procesan *de forma asincrónica*.

> [!NOTE]
> Azure SQL Database no admite `etw_classic_sync_target target`.

<!-- After OPS Versioning is live, the above !NOTE could be converted into a "3colon ZONE".  GeneMi = MightyPen. -->

<a name="h2_target_event_counter"></a>

## <a name="event_counter-target"></a>Destino event_counter


El destino event_counter simplemente cuenta cuántas veces se produce cada evento especificado.


A diferencia de la mayoría de los demás destinos:

- El destino event_counter no tiene parámetros.


- A diferencia de la mayoría de los destinos, el destino event_counter procesa *de forma sincrónica* los datos que recibe.
    - Sincrónico es aceptable para el destino event_counter sencillo, pues event_counter implica un procesamiento muy pequeño.
    - El motor de base de datos se desconectará de cualquier destino que sea demasiado lento y que, por tanto, amenace con ralentizar el rendimiento del motor de base de datos. Esta es una de las razones por las que la mayoría de los destinos procesan los datos *de forma asincrónica*.


#### <a name="example-output-captured-by-event_counter"></a>Salida de ejemplo capturada por event_counter


```
package_name   event_name         count
------------   ----------         -----
sqlserver      checkpoint_begin   4
```


A continuación se muestra la instrucción CREATE EVENT SESSION que dio lugar a los resultados anteriores. Para esta prueba, en la cláusula EVENT...WHERE, el campo **package0.counter** se ha usado para detener el recuento después de llegar a 4.


```sql
CREATE EVENT SESSION [event_counter_1]
    ON SERVER 
    ADD EVENT sqlserver.checkpoint_begin   -- Test by issuing CHECKPOINT; statements.
    (
        WHERE ([package0].[counter] <= (4))   -- A predicate filter.
    )
    ADD TARGET package0.event_counter
    WITH
    (
        MAX_MEMORY = 4096 KB,
        MAX_DISPATCH_LATENCY = 3 SECONDS
    );
```



<a name="h2_target_event_file"></a>

## <a name="event_file-target"></a>Destino event_file


El destino **event_file** escribe el resultado de la sesión de eventos desde el búfer en un archivo de disco:


- Especifique el parámetro *filename=* en la cláusula ADD TARGET.
    - **.xel** debe ser la extensión del archivo.


- El sistema usa el nombre de archivo que elige como prefijo al que se anexa un entero largo basado en fecha y hora, seguido de la extensión .xel.

::: moniker range="= azuresqldb-current || = azuresqldb-mi-current "

> [!NOTE]
> Azure SQL Database solo admite el almacenamiento de archivos `xel` en Azure Blob Storage. 
>
> Para ver un ejemplo de código de **event_file** particular de SQL Database (y de SQL Managed Instance), vea [Código de destino del archivo de evento para eventos extendidos en SQL Database](/azure/sql-database/sql-database-xevent-code-event-file).

::: moniker-end


#### <a name="create-event-session-with-event_file-target"></a>CREATE EVENT SESSION con destino **event_file**


A continuación se muestra la instrucción CREATE EVENT SESSION con la que solíamos probar. Una de las cláusulas ADD TARGET especifica un destino event_file.


```sql
CREATE EVENT SESSION [locks_acq_rel_eventfile_22]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION (sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name=(1),
            collect_resource_description=(1)

        ACTION(sqlserver.sql_text,sqlserver.transaction_id)

        WHERE
        (
            [database_name]=N'InMemTest2'
            AND
            [object_id]=(370100359)
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.event_file
    (
        SET     filename=N'C:\Junk\locks_acq_rel_eventfile_22-.xel'
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=10 SECONDS
    );
```


#### <a name="sysfn_xe_file_target_read_file-function"></a>sys.fn_xe_file_target_read_file, función


El destino event_file almacena los datos que recibe en un formato binario que no es legible por el usuario. Transact-SQL puede informar sobre el contenido del archivo .xel realizando una selección a partir de la función [**sys.fn_xe_file_target_read_file**](../../relational-databases/system-functions/sys-fn-xe-file-target-read-file-transact-sql.md) .


En el caso de SQL Server **2016** y versiones posteriores, la siguiente instrucción SELECT de T-SQL informó sobre los datos. El sufijo *.xel en 


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            null, null, null)  AS f;
```


En el caso de SQL Server **2014**, una instrucción SELECT similar a la siguiente informaría sobre los datos. Después de SQL Server 2014, ya no se usan los archivos .xem.


```
SELECT f.*
        --,CAST(f.event_data AS XML)  AS [Event-Data-Cast-To-XML]  -- Optional
    FROM
        sys.fn_xe_file_target_read_file(
            'C:\junk\locks_acq_rel_eventfile_22-*.xel',
            'C:\junk\metafile.xem',
            null, null)  AS f;
```


Por supuesto, también puede usar manualmente la interfaz de usuario de SSMS para ver los datos .xel:


#### <a name="data-stored-in-the-event_file-target"></a>Datos almacenados en el destino event_file


A continuación se muestra el informe resultante de la selección de **sys.fn_xe_file_target_read_file**, en SQL Server 2016.


```
module_guid                            package_guid                           object_name     event_data                                                                                                                                                                                                                                                                                          file_name                                                      file_offset
-----------                            ------------                           -----------     ----------                                                                                                                                                                                                                                                                                          ---------                                                      -----------
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_acquired   <event name="lock_acquired" package="sqlserver" timestamp="2016-08-07T20:13:35.827Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
D5149520-6282-11DE-8A39-0800200C9A66   03FDA7D0-91BA-45F8-9875-8B6DD0B8E9F2   lock_released   <event name="lock_released" package="sqlserver" timestamp="2016-08-07T20:13:35.832Z"><action name="transaction_id" package="sqlserver"><value>39194</value></action><action name="sql_text" package="sqlserver"><value><![CDATA[  select top 1 * from dbo.T_Target;  ]]></value></action></event>   C:\junk\locks_acq_rel_eventfile_22-_0_131150744126230000.xel   11776
```



<a name="h2_target_histogram"></a>

## <a name="histogram-target"></a>Destino histogram


El destino **histogram** es más sofisticado que el destino event_counter. El destino histogram puede hacer lo siguiente:

- Contar repeticiones de varios elementos por separado.
- Contar repeticiones de diversos tipos de elementos:
    - Campos de evento.
    - Acciones.


El parámetro **source_type** es la clave para controlar el destino histogram:

- **source_type=0** : implica recopilar datos para *campos de evento*).
- **source_type=1** : implica recopilar datos para *acciones*.
    - 1 es el valor predeterminado.


El valor predeterminado del parámetro "slots" es 256. Si asigna otro valor, este se redondea a la siguiente potencia de 2.

- Por ejemplo, ranuras=59 se redondearía a =64.


### <a name="action-example-for-histogram"></a>Ejemplo de *acción* para histogram


En su cláusula TARGET...SET, la siguiente instrucción CREATE EVENT SESSION de Transact-SQL especifica la asignación de parámetro de destino de **source_type=1**. El 1 significa que el destino histogram realiza un seguimiento de una acción.

En el presente ejemplo, la oferta de la cláusula EVENT...ACTION ofrece solo una acción para que el destino la elija, concretamente **sqlos.system_thread_id**. En la cláusula TARGET...SET, vemos la asignación de **source=N'sqlos.system_thread_id'** .

- Para realizar un seguimiento de más de una acción de origen, puede agregar un segundo destino histogram a la instrucción CREATE EVENT SESSION.


```sql
CREATE EVENT SESSION [histogram_lockacquired]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
        (
        ACTION
            (
            sqlos.system_thread_id
            )
        )
    ADD TARGET package0.histogram
        (
        SET
            filtering_event_name=N'sqlserver.lock_acquired',
            slots=(16),
            source=N'sqlos.system_thread_id',
            source_type=1
        )
    WITH
        (
        <.... (For brevity, numerous parameter assignments generated by SSMS.exe are not shown here.) ....>
        );
```


 Se capturaron los siguientes datos. Los valores de la columna **valor** eran valores system_thread_id. Por ejemplo, se realizaron un total de 236 bloqueos en el subproceso 6540.


```
value   count
-----   -----
 6540     236
 9308      91
 9668      74
10144      49
 5244      44
 2396      28
```


#### <a name="select-to-discover-available-actions"></a>SELECT para descubrir las acciones disponibles


La instrucción SELECT [(C.3)](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_3_select_all_available_objects) puede encontrar las acciones que el sistema tiene a su disposición para que las especifique en la instrucción CREATE EVENT SESSION. En la cláusula WHERE, primero editaría el filtro **o.name LIKE** para hacerlo coincidir con las acciones que le interesan.


A continuación se muestra un conjunto de filas de ejemplo devuelto por la instrucción SELECT (C.3). La acción **system_thread_id** está presente en la segunda fila.


```
Package-Name   Action-Name                 Action-Description
------------   -----------                 ------------------
package0       collect_current_thread_id   Collect the current Windows thread ID
sqlos          system_thread_id            Collect current system thread ID
sqlserver      create_dump_all_threads     Create mini dump including all threads
sqlserver      create_dump_single_thread   Create mini dump for the current thread
```


### <a name="event-field-example-for-histogram"></a>Ejemplo de *campo* de evento para histogram


En el siguiente ejemplo se establece **source_type=0**. El valor asignado a **source=** es un campo de evento (no una acción).



```sql
CREATE EVENT SESSION [histogram_checkpoint_dbid]
    ON SERVER 
    ADD EVENT  sqlserver.checkpoint_begin
    ADD TARGET package0.histogram
    (
    SET
        filtering_event_name = N'sqlserver.checkpoint_begin',
        source               = N'database_id',
        source_type          = (0)
    )
    WITH
    ( <....> );
```


El destino histogram capturó los siguientes datos. Los datos muestran que la base de datos ID=5 experimentó siete eventos checkpoint_begin.


```
value   count
-----   -----
5       7
7       4
6       3
```


#### <a name="select-to-discover-available-fields-on-your-chosen-event"></a>SELECT para descubrir los campos disponibles en el evento elegido


La instrucción SELECT [(C.4)](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields) muestra los campos de evento de entre los que puede elegir. Primero editaría el filtro **o.name LIKE** para hacerlo coincidir con el nombre de evento elegido.


La instrucción SELECT devolvió el siguiente conjunto de filas (C.4). El conjunto de filas muestra que database_id es el único campo del evento checkpoint_begin que puede proporcionar valores para el destino histogram.


```
Package-Name   Event-Name         Field-Name   Field-Description
------------   ----------         ----------   -----------------
sqlserver      checkpoint_begin   database_id  NULL
sqlserver      checkpoint_end     database_id  NULL
```


<a name="h2_target_pair_matching"></a>

## <a name="pair_matching-target"></a>Destino pair_matching


El destino pair_matching permite detectar eventos de inicio que se producen sin un evento de finalización correspondiente. Por ejemplo, podría suponer un problema el hecho de que se produjera un evento lock_acquired, pero no le siguiera ningún evento lock_released coincidente de forma puntual.


El sistema no hace coincidir automáticamente los eventos de inicio y finalización. En su lugar, explica la coincidencia al sistema en la instrucción CREATE EVENT SESSION. Al coincidir un evento de inicio y finalización, el par se descarta para que todo el mundo pueda centrarse en los eventos de inicio no coincidentes.


#### <a name="finding-matchable-fields-for-the-start-and-end-event-pair"></a>Búsqueda de campos coincidentes para el par de eventos de inicio y finalización


Mediante el uso de la instrucción [SELECT (C.4)](../../relational-databases/extended-events/selects-and-joins-from-system-views-for-extended-events-in-sql-server.md#section_C_4_data_fields), vemos en el siguiente conjunto de filas que hay unos 16 campos para el evento lock_acquired. El conjunto de filas aquí mostrado se ha dividido manualmente para hacer visibles los campos en los que coincidía nuestro ejemplo. Sería absurdo intentar que coincidieran algunos campos, por ejemplo, en la **duración** de ambos eventos.


```
Package-Name   Event-Name   Field-Name               Field-Description
------------   ----------   ----------               -----------------
sqlserver   lock_acquired   database_name            NULL
sqlserver   lock_acquired   mode                     NULL
sqlserver   lock_acquired   resource_0               The ID of the locked object, when lock_resource_type is OBJECT.
sqlserver   lock_acquired   resource_1               NULL
sqlserver   lock_acquired   resource_2               The ID of the lock partition, when lock_resource_type is OBJECT, and resource_1 is 0.
sqlserver   lock_acquired   transaction_id           NULL

sqlserver   lock_acquired   associated_object_id     The ID of the object that requested the lock that was acquired.
sqlserver   lock_acquired   database_id              NULL
sqlserver   lock_acquired   duration                 The time (in microseconds) between when the lock was requested and when it was canceled.
sqlserver   lock_acquired   lockspace_nest_id        NULL
sqlserver   lock_acquired   lockspace_sub_id         NULL
sqlserver   lock_acquired   lockspace_workspace_id   NULL
sqlserver   lock_acquired   object_id                The ID of the locked object, when lock_resource_type is OBJECT. For other lock resource types it will be 0
sqlserver   lock_acquired   owner_type               NULL
sqlserver   lock_acquired   resource_description     The description of the lock resource. The description depends on the type of lock. This is the same value as the resource_description column in the sys.dm_tran_locks view.
sqlserver   lock_acquired   resource_type            NULL
```


### <a name="example-of-pair_matching"></a>Ejemplo de pair_matching


La siguiente instrucción CREATE EVENT SESSION especifica dos eventos y dos destinos. El destino pair_matching especifica dos conjuntos de campos para que coincidan con los eventos en pares. La secuencia de campos delimitados por comas asignada a **begin_matching_columns=** y **end_matching_columns =** debe ser la misma. No se permiten pestañas ni nuevas líneas entre los campos mencionados en el valor delimitado por comas, aunque los espacios son correctos.

Para restringir los resultados, primero realizamos una selección a partir de sys.objects para buscar el valor object_id de nuestra tabla de prueba. Hemos agregado un filtro para ese identificador a la cláusula EVENT...WHERE.


```sql
CREATE EVENT SESSION [pair_matching_lock_a_r_33]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    ),
    ADD EVENT sqlserver.lock_released
    (
        SET
            collect_database_name = (1),
            collect_resource_description = (1)

        ACTION (sqlserver.transaction_id)

        WHERE
        (
            [database_name] = 'InMemTest2'
            AND
            [object_id] = 370100359
        )
    )
    ADD TARGET package0.event_counter,
    ADD TARGET package0.pair_matching
    (
        SET
            begin_event = N'sqlserver.lock_acquired',
            begin_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            end_event = N'sqlserver.lock_released',
            end_matching_columns =
                N'resource_0, resource_1, resource_2, transaction_id, database_id',

            respond_to_memory_pressure = (1)
    )
    WITH
    (
        MAX_MEMORY = 8192 KB,
        MAX_DISPATCH_LATENCY = 15 SECONDS
    );
```


Para probar la sesión de eventos, impedimos expresamente la liberación de bloqueos adquiridos. Lo hicimos con los siguientes pasos de T-SQL:

1. BEGIN TRANSACTION.
2. UPDATE MyTable....
3. No emita expresamente una instrucción COMMIT TRANSACTION hasta que hayamos examinado los destinos.
4. Más adelante, después de las pruebas, emitimos una instrucción COMMIT TRANSACTION.

El destino **event_counter** sencillo proporcionó las siguientes filas de salida. Como 52-50=2, el resultado nos indica que deberíamos ver dos eventos lock_acquired desemparejados al examinarse este desde el destino de coincidencia de pares.


```
package_name   event_name      count
------------   ----------      -----
sqlserver      lock_acquired   52
sqlserver      lock_released   50
```


El destino **pair_matching** proporcionó el siguiente resultado. Tal como sugiere el resultado de event_counter, vemos realmente dos filas lock_acquired. El hecho de que veamos estas filas significa que estos eventos lock_acquired están desemparejados.


```
package_name   event_name      timestamp                     database_name   duration   mode   object_id   owner_type   resource_0   resource_1   resource_2   resource_description   resource_type   transaction_id
------------   ----------      ---------                     -------------   --------   ----   ---------   ----------   ----------   ----------   ----------   --------------------   -------------   --------------
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          S      370100359   Transaction  370100359    3            0            [INDEX_OPERATION]      OBJECT          34126
sqlserver      lock_acquired   2016-08-05 12:45:47.9980000   InMemTest2      0          IX     370100359   Transaction  370100359    0            0                                   OBJECT          34126
```


Las filas para los eventos lock_acquired desemparejados podrían incluir el texto T-SQL o **sqlserver.sql_text**, que tomaron los bloqueos. Pero no queremos que la presentación se inunde.


<a name="h2_target_ring_buffer"></a>

## <a name="ring_buffer-target"></a>Destino ring_buffer


El destino ring_buffer resulta útil para probar eventos de forma rápida y sencilla. Cuando detiene la sesión de eventos, se descarta el resultado almacenado.

En esta sección ring_buffer también mostramos cómo puede usar la implementación de Transact-SQL de XQuery para copiar el contenido XML del destino ring_buffer en un conjunto de filas relacional más legible.


#### <a name="create-event-session-with-ring_buffer"></a>CREATE EVENT SESSION con ring_buffer


Esta instrucción CREATE EVENT SESSION, que usa el destino ring_buffer, no tiene nada de extraordinario.


```sql
CREATE EVENT SESSION [ring_buffer_lock_acquired_4]
    ON SERVER 
    ADD EVENT sqlserver.lock_acquired
    (
        SET collect_resource_description=(1)

        ACTION(sqlserver.database_name)

        WHERE
        (
            [object_id]=(370100359)  -- ID of MyTable
            AND
            sqlserver.database_name='InMemTest2'
        )
    )
    ADD TARGET package0.ring_buffer
    (
        SET max_events_limit=(98)
    )
    WITH
    (
        MAX_MEMORY=4096 KB,
        MAX_DISPATCH_LATENCY=3 SECONDS
    );
```


### <a name="xml-output-received-for-lock_acquired-by-ring_buffer"></a>Resultado XML recibido para lock_acquired de ring_buffer


Al recuperar el contenido una instrucción SELECT, este tiene la forma de una cadena de XML. A continuación, se muestra la cadena XML que el destino ring_buffer almacenó en nuestras pruebas. Sin embargo, en aras de la brevedad de la siguiente presentación XML, se han borrado todos los elementos &#x3c;event&#x3e; salvo dos. Además, dentro de cada &#x3c;event&#x3e;, se ha eliminado una serie de elementos &#x3c;data&#x3e; externos.


```xml
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="6" eventCount="6" droppedCount="0" memoryUsed="1032">
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:53.987Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111030</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
  <event name="lock_acquired" package="sqlserver" timestamp="2016-08-05T23:59:56.012Z">
    <data name="mode">
      <type name="lock_mode" package="sqlserver"></type>
      <value>1</value>
      <text><![CDATA[SCH_S]]></text>
    </data>
    <data name="transaction_id">
      <type name="int64" package="package0"></type>
      <value>111039</value>
    </data>
    <data name="database_id">
      <type name="uint32" package="package0"></type>
      <value>5</value>
    </data>
    <data name="resource_0">
      <type name="uint32" package="package0"></type>
      <value>370100359</value>
    </data>
    <data name="resource_1">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="resource_2">
      <type name="uint32" package="package0"></type>
      <value>0</value>
    </data>
    <data name="database_name">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[]]></value>
    </data>
    <action name="database_name" package="sqlserver">
      <type name="unicode_string" package="package0"></type>
      <value><![CDATA[InMemTest2]]></value>
    </action>
  </event>
</RingBufferTarget>
```


Para ver el código XML anterior, puede emitir la instrucción SELECT siguiente mientras la sesión de eventos está activa. Los datos XML activos se recuperan de la vista del sistema **sys.dm_xe_session_targets**.


```sql
SELECT
        CAST(LocksAcquired.TargetXml AS XML)  AS RBufXml
    INTO
        #XmlAsTable
    FROM
        (
        SELECT
                CAST(t.target_data AS XML)  AS TargetXml
            FROM
                     sys.dm_xe_session_targets  AS t
                JOIN sys.dm_xe_sessions         AS s

                    ON s.address = t.event_session_address
            WHERE
                t.target_name = 'ring_buffer'
                AND
                s.name        = 'ring_buffer_lock_acquired_4'
        )
            AS LocksAcquired;


SELECT * FROM #XmlAsTable;
```


### <a name="xquery-to-see-the-xml-as-a-rowset"></a>XQuery para ver el código XML como un conjunto de filas


Para ver el código XML anterior como un conjunto de filas relacional, continúe desde la instrucción SELECT anterior mediante la emisión de la siguiente extensión T-SQL. Las líneas comentadas explican cada uso de XQuery.


```sql
SELECT
         -- (A)
         ObjectLocks.value('(@timestamp)[1]',
            'datetime'     )  AS [OccurredDtTm]

        -- (B)
        ,ObjectLocks.value('(data[@name="mode"]/text)[1]',
            'nvarchar(32)' )  AS [Mode]

        -- (C)
        ,ObjectLocks.value('(data[@name="transaction_id"]/value)[1]',
            'bigint' )  AS [TxnId]

        -- (D)
        ,ObjectLocks.value('(action[@name="database_name" and @package="sqlserver"]/value)[1]',
            'nvarchar(128)')  AS [DatabaseName]
    FROM
        #TableXmlCell
    CROSS APPLY
        -- (E)
        TargetDateAsXml.nodes('/RingBufferTarget/event[@name="lock_acquired"]')  AS T(ObjectLocks);
```


#### <a name="xquery-notes-from-preceding-select"></a>Notas de XQuery de la instrucción SELECT anterior


(A)
- Valor del atributo timestamp=, en el elemento &#x3c;event&#x3e;.
- La construcción '(...)[1]' garantiza solo la devolución de un valor por iteración, al tratarse de una limitación requerida del método XQuery .value() de la variable y las columnas de tipo de datos XML.


(B)
- Valor interno del elemento &#x3c;text&#x3e;, dentro de un elemento &#x3c;data&#x3e; cuyo atributo name= equivale a "mode".


(C)
- Valor interno del elemento &#x3c;value&#x3e;, dentro de un elemento &#x3c;data&#x3e; cuyo atributo name= equivale a "transaction_id".


(D)
- &#x3c;event&#x3e; contiene &#x3c;action&#x3e;.
- Teniendo &#x3c;action&#x3e; un atributo name= equivalente a "database_name" y un atributo package= equivalente a "sqlserver" (no a "package0"), obtenga el valor interno del elemento &#x3c;value&#x3e;.


(E)
- C.A. hace que el procesamiento se repita para cada elemento &#x3c;event&#x3e; individual cuyo atributo name= equivale a "lock_acquired".
- Esto se aplica al código XML devuelto por la cláusula FROM anterior.


#### <a name="output-from-xquery-select"></a>Resultados de la instrucción SELECT de XQuery


A continuación se muestra el conjunto de filas generado por la extensión T-SQL anterior que incluye XQuery.


```
OccurredDtTm              Mode    DatabaseName
------------              ----    ------------
2016-08-05 23:59:53.987   SCH_S   InMemTest2
2016-08-05 23:59:56.013   SCH_S   InMemTest2
```



## <a name="xevent-net-namespaces-and-cx23"></a>Espacios de nombres de .NET de XEvent y C&#x23;


Package0 tiene dos destinos más, pero no pueden usarse en Transact-SQL:

- compressed_history
- event_stream


Una de las razones por las que sabemos que esos dos destinos no pueden usarse en T-SQL es que sus valores, que no son NULL, de la columna *sys.dm_xe_objects.capabilities* no incluyen el bit 0x1.


El destino event_stream puede usarse en programas de .NET escritos en lenguajes como C#. C# y otros desarrolladores de .NET pueden tener acceso a un flujo de eventos a través de una clase de .NET Framework, como, por ejemplo, en el espacio de nombres Microsoft.SqlServer.XEvents.Linq.

En caso de aparecer, el error **25726** significa que el flujo de eventos se llenó de datos más rápido de lo que el cliente tardaría en consumirlos. Esto hizo que el motor de base de datos de desconectara del flujo de eventos para evitar mostrar el rendimiento del servidor.


### <a name="xevent-namespaces"></a>Espacios de nombres de XEvent


- [Espacio de nombres Microsoft.SqlServer.Management.XEvent](/dotnet/api/microsoft.sqlserver.management.xevent)

- [Espacio de nombres Microsoft.SqlServer.XEvent.Linq](/dotnet/api/microsoft.sqlserver.xevent.linq)
