---
description: 'Captura de datos modificados: sys.dm_cdc_log_scan_sessions'
title: sys.dm_cdc_log_scan_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_cdc_log_scan_sessions
- dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions_TSQL
- sys.dm_cdc_log_scan_sessions
dev_langs:
- TSQL
helpviewer_keywords:
- change data capture [SQL Server], log scan reporting
- sys.dm_cdc_log_scan_sessions dynamic management view
ms.assetid: d337e9d0-78b1-4a07-8820-2027d0b9f87c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e8f69ed4f8b1ca90bc6de468b4136c6c36687cfc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99148195"
---
# <a name="change-data-capture---sysdm_cdc_log_scan_sessions"></a>Captura de datos modificados: sys.dm_cdc_log_scan_sessions
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada sesión de recorrido de registro de la base de datos actual. La última fila devuelta representa la sesión actual. Puede usar esta vista para devolver información de estado sobre la sesión del recorrido del registro actual, o bien información agregada sobre todas las sesiones desde que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inició por última vez.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Id. de la sesión.<br /><br /> 0 = los datos devueltos en esta fila son un agregado de todas las sesiones desde que la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se inició por última vez.|  
|**start_time**|**datetime**|Hora que la sesión comenzó.<br /><br /> Cuando **session_id** = 0, comienza la recopilación de datos agregados de hora.|  
|**end_time**|**datetime**|Hora a la que finalizó la sesión<br /><br /> NULL = la sesión está activa.<br /><br /> Cuando **session_id** = 0, la hora en que finalizó la última sesión.|  
|**duration**|**bigint**|Duración, en segundos, de la sesión<br /><br /> 0 = la sesión no contiene transacciones de captura de datos de cambio.<br /><br /> Cuando **session_id** = 0, la suma de la duración (en segundos) de todas las sesiones con las transacciones de captura de datos modificados.|  
|**scan_phase**|**nvarchar(200)**|La fase actual de la sesión. Estos son los valores posibles y sus descripciones:<br /><br /> 1: lectura de la configuración<br />2: primer examen, compilar tabla hash<br />3: segundo examen<br />4: segundo examen<br />5: segundo examen<br />6: control de versiones de esquema<br />7: último examen<br />8: listo<br /><br /> Cuando **session_id** = 0, este valor siempre es "Aggregate".|  
|**error_count**|**int**|Número máximo de errores detectados<br /><br /> Cuando **session_id** = 0, número total de errores en todas las sesiones.|  
|**start_lsn**|**nvarchar (23)**|Iniciar LSN para la sesión.<br /><br /> Cuando **session_id** = 0, el LSN de inicio de la última sesión.|  
|**current_lsn**|**nvarchar (23)**|LSN actual del que se realiza un recorrido.<br /><br /> Cuando **session_id** = 0, el LSN actual es 0.|  
|**end_lsn**|**nvarchar (23)**|LSN final de la sesión.<br /><br /> NULL = la sesión está activa.<br /><br /> Cuando **session_id** = 0, el LSN final de la última sesión.|  
|**tran_count**|**bigint**|Número de transacciones de captura de datos de cambio procesados. Este contador se rellena en la fase 2.<br /><br /> Cuando **session_id** = 0, el número de transacciones procesadas en todas las sesiones.|  
|**last_commit_lsn**|**nvarchar (23)**|LSN de la última entrada del registro de confirmación procesada.<br /><br /> Cuando **session_id** = 0, último LSN de registro de confirmación para cualquier sesión.|  
|**last_commit_time**|**datetime**|Hora de procesamiento de la última entrada del registro de confirmación.<br /><br /> Cuando **session_id** = 0, hora de la última entrada de registro de confirmación para cualquier sesión.|  
|**log_record_count**|**bigint**|Número de entradas de registro de las que se ha realizado un recorrido.<br /><br /> Cuando **session_id** = 0, número de registros examinados para todas las sesiones.|  
|**schema_change_count**|**int**|Número de operaciones de lenguaje de definición de datos (DDL) detectadas. Este contador se rellena en la fase 6.<br /><br /> Cuando **session_id** = 0, número de operaciones DDL procesadas en todas las sesiones.|  
|**command_count**|**bigint**|Número de comandos procesados.<br /><br /> Cuando **session_id** = 0, número de comandos procesados en todas las sesiones.|  
|**first_begin_cdc_lsn**|**nvarchar (23)**|Primer LSN que contenía las transacciones de captura de los datos de cambio.<br /><br /> Cuando **session_id** = 0, el primer LSN que contenía las transacciones de captura de datos modificados.|  
|**last_commit_cdc_lsn**|**nvarchar (23)**|LSN de la última entrada de registro de confirmación que contenía las transacciones de captura de los datos de cambio.<br /><br /> Cuando **session_id** = 0, último LSN de registro de confirmación para cualquier sesión que contenía las transacciones de captura de datos modificados|  
|**last_commit_cdc_time**|**datetime**|Hora de procesamiento de la última entrada de registro de confirmación que contenía las transacciones de captura de los datos de cambio.<br /><br /> Cuando **session_id** = 0, hora de la última entrada de registro de confirmación para cualquier sesión que contenía las transacciones de captura de datos modificados.|  
|**duplica**|**int**|La diferencia, en segundos, entre **end_time** y **last_commit_cdc_time** en la sesión. Este contador se rellena al final de la fase 7.<br /><br /> Cuando **session_id** = 0, el último valor de latencia distinto de cero registrado por una sesión.|  
|**empty_scan_count**|**int**|Número de sesiones consecutivas que no contenían ninguna transacciones de captura de los datos de cambio.|  
|**failed_sessions_count**|**int**|Número de sesiones erróneas.|  
  
## <a name="remarks"></a>Observaciones  
 Se restablecen los valores en esta vista de administración dinámica siempre que se inicia la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE para consultar la vista de administración dinámica **Sys.dm_cdc_log_scan_sessions** . Para obtener más información acerca de los permisos en las vistas de administración dinámica, vea [funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md).  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo devuelve información para la sesión más actual.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT session_id, start_time, end_time, duration, scan_phase,  
    error_count, start_lsn, current_lsn, end_lsn, tran_count,  
    last_commit_lsn, last_commit_time, log_record_count, schema_change_count,  
    command_count, first_begin_cdc_lsn, last_commit_cdc_lsn,   
    last_commit_cdc_time, latency, empty_scan_count, failed_sessions_count  
FROM sys.dm_cdc_log_scan_sessions  
WHERE session_id = (SELECT MAX(b.session_id) FROM sys.dm_cdc_log_scan_sessions AS b);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_cdc_errors &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md)  
  
  

