---
title: sys.dm_db_xtp_checkpoint_stats (Transact-SQL) | Microsoft Docs
description: Devuelve estadísticas sobre las operaciones de punto de comprobación de OLTP en memoria de la base de datos actual. Obtenga información acerca de cómo esta vista difiere en las versiones de SQL Server.
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: reference
f1_keywords:
- dm_db_xtp_checkpoint_stats
- dm_db_xtp_checkpoint_stats_TSQL
- sys.dm_db_xtp_checkpoint_stats
- sys.dm_db_xtp_checkpoint_stats_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_checkpoint_stats dynamic management view
ms.assetid: 8d0b18ca-db4d-4376-9905-3e4457727c46
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: b6211155c51a2751e3f4625a4c838bec29695905
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210893"
---
# <a name="sysdm_db_xtp_checkpoint_stats-transact-sql"></a>sys.dm_db_xtp_checkpoint_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Devuelve estadísticas sobre las operaciones de punto de comprobación de OLTP en memoria de la base de datos actual. Si la base de datos no tiene objetos de OLTP en memoria, devuelve un conjunto de resultados vacío.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
```  
USE In_Memory_db_name
SELECT * FROM sys.dm_db_xtp_checkpoint_stats;  
```  
  
**[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] es sustancialmente diferente de las versiones más recientes y se describe más abajo en el tema en [SQL Server 2014](#bkmk_2014).**
  
## <a name="sssql15-and-later"></a>[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores   
 En la tabla siguiente se describen las columnas de `sys.dm_db_xtp_checkpoint_stats` , a partir de **[!INCLUDE[ssSQL15](../../includes/sssql16-md.md)]** .  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|last_lsn_processed|**bigint**|Último LSN detectado por el controlador.|  
|end_of_log_lsn|**Numeric (38)**|LSN del final del registro.|  
|bytes_to_end_of_log|**bigint**|Bytes de registro no procesados por el controlador, correspondientes a los bytes entre `last_lsn_processed` y `end_of_log_lsn` .|  
|log_consumption_rate|**bigint**|Velocidad del consumo del registro de transacciones por el controlador (en KB/s).|  
|active_scan_time_in_ms|**bigint**|Tiempo empleado por el controlador en el análisis activo del registro de transacciones.|  
|total_wait_time_in_ms|**bigint**|Tiempo de espera acumulado para el controlador mientras no se examina el registro.|  
|waits_for_io|**bigint**|Número de esperas de e/s de registro en las que incurre el subproceso del controlador.|  
|io_wait_time_in_ms|**bigint**|Tiempo acumulado empleado en esperar la e/s del registro por parte del subproceso del controlador.|  
|waits_for_new_log_count|**bigint**|Número de esperas producidas por el subproceso del controlador para que se genere un nuevo registro.|  
|new_log_wait_time_in_ms|**bigint**|Tiempo acumulado invertido en esperar a un nuevo registro por parte del subproceso del controlador.|  
|idle_attempts_count|**bigint**|Número de veces que el controlador ha pasado a un estado de inactividad.|  
|tx_segments_dispatched|**bigint**|Número de segmentos que el controlador ha detectado y enviado a los serializadores. Segment es una parte contigua del registro que constituye una unidad de serialización. Tiene un tamaño de 1 MB, pero puede cambiar en el futuro.|  
|segment_bytes_dispatched|**bigint**|Recuento total de bytes de bytes enviados por el controlador a los serializadores, desde el reinicio de la base de datos.|  
|bytes_serialized|**bigint**|Recuento total de bytes serializados desde el reinicio de la base de datos.|  
|serializer_user_time_in_ms|**bigint**|Tiempo empleado por los serializadores en modo de usuario.|  
|serializer_kernel_time_in_ms|**bigint**|Tiempo empleado por los serializadores en modo kernel.|  
|xtp_log_bytes_consumed|**bigint**|Recuento total de bytes de registro consumidos desde el reinicio de la base de datos.|  
|checkpoints_closed|**bigint**|Recuento de puntos de control cerrados desde el reinicio de la base de datos.|  
|last_closed_checkpoint_ts|**bigint**|Marca de tiempo del último punto de control cerrado.|  
|hardened_recovery_lsn|**Numeric (38)**|La recuperación se iniciará desde este LSN.|  
|hardened_root_file_guid|**uniqueidentifier**|GUID del archivo raíz que protegió como resultado el último punto de control completado.|  
|hardened_root_file_watermark|**bigint**|**Solo** para uso interno. Hasta ahora es válido leer el archivo raíz hasta (es un tipo solo relevante internamente, denominado BSN).|  
|hardened_truncation_lsn|**Numeric (38)**|LSN del punto de truncamiento.|  
|log_bytes_since_last_close|**bigint**|Bytes desde el último cierre hasta el final actual del registro.|  
|time_since_last_close_in_ms|**bigint**|Tiempo desde el último cierre del punto de control.|  
|current_checkpoint_id|**bigint**|Actualmente se asignan segmentos nuevos a este punto de control. El sistema Checkpoint es una canalización. El punto de control actual es aquel al que se asignan los segmentos del registro. Una vez que se alcanza un límite, el controlador libera el punto de control y se crea uno nuevo como actual.|  
|current_checkpoint_segment_count|**bigint**|Recuento de segmentos en el punto de control actual.|  
|recovery_lsn_candidate|**bigint**|**Solo internamente**. Candidato que se va a seleccionar como recoverylsn al cerrar current_checkpoint_id.|  
|outstanding_checkpoint_count|**bigint**|Número de puntos de control de la canalización en espera de cierre.|  
|closing_checkpoint_id|**bigint**|IDENTIFICADOR del punto de control de cierre.<br /><br /> Los serializadores funcionan en paralelo, por lo que una vez finalizados, el punto de control es un candidato para cerrarse con el subproceso de cierre. Pero el subproceso de cierre solo puede cerrarse de una en una y debe estar en orden, por lo que el punto de control de cierre es aquel en el que está trabajando el subproceso de cierre.|  
|recovery_checkpoint_id|**bigint**|IDENTIFICADOR del punto de control que se va a usar en la recuperación.|  
|recovery_checkpoint_ts|**bigint**|Marca de tiempo del punto de comprobación de recuperación.|  
|bootstrap_recovery_lsn|**Numeric (38)**|LSN de recuperación del bootstrap.|  
|bootstrap_root_file_guid|**uniqueidentifier**|GUID del archivo raíz del bootstrap.|  
|internal_error_code|**bigint**|Error detectado por cualquiera de los subprocesos Controller, Serializer, Close y Merge.|
|bytes_of_large_data_serialized|**bigint**|La cantidad de datos que se serializaron. |  
  
##  <a name="sssql14"></a><a name="bkmk_2014"></a> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]  
 En la tabla siguiente se describen las columnas de `sys.dm_db_xtp_checkpoint_stats` , para **[!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]** .  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|log_to_process_in_bytes|**bigint**|El número de bytes de registro entre el número de secuencia de registro (LSN) actual del subproceso y el fin del registro.|  
|total_log_blocks_processed|**bigint**|Número total de bloques de registro procesados desde el inicio del servidor.|  
|total_log_records_processed|**bigint**|Número total de entradas del registro procesadas desde el inicio del servidor.|  
|xtp_log_records_processed|**bigint**|Número total de entradas del registro de OLTP en memoria procesadas desde el inicio del servidor.|  
|total_wait_time_in_ms|**bigint**|Tiempo de espera acumulativo en ms.|  
|waits_for_io|**bigint**|Número de esperas del registro E/S.|  
|io_wait_time_in_ms|**bigint**|Tiempo acumulado empleado en esperar la E/S de registro.|  
|waits_for_new_log|**bigint**|Número de esperas para el nuevo registro que se va a generar.|  
|new_log_wait_time_in_ms|**bigint**|Tiempo acumulado empleado en esperar el nuevo registro.|  
|log_generated_since_last_checkpoint_in_bytes|**bigint**|Cantidad de registro generado desde el último punto de comprobación de OLTP en memoria.|  
|ms_since_last_checkpoint|**bigint**|Cantidad de tiempo en milisegundos desde el último punto de comprobación de OLTP en memoria.|  
|checkpoint_lsn|**Numeric (38)**|Número de secuencia de registro (LSN) de recuperación asociada al punto de comprobación OLTP en memoria completado.|  
|current_lsn|**Numeric (38)**|El LSN de la entrada de registro que se está procesando actualmente.|  
|end_of_log_lsn|**Numeric (38)**|El LSN del final del registro.|  
|task_address|**varbinary(8**|Dirección de SOS_Task. Combinación con sys.dm_os_tasks para buscar información adicional.|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW DATABASE STATE` en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
