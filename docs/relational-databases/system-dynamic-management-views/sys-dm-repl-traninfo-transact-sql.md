---
description: sys.dm_repl_traninfo (Transact-SQL)
title: sys.dm_repl_traninfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_repl_traninfo
- dm_repl_traninfo
- sys.dm_repl_traninfo_TSQL
- dm_repl_traninfo_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_repl_traninfo dynamic management view
ms.assetid: 5abe2605-0506-46ec-82b5-6ec08428ba13
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e3c062d6a8aafffc9621a0932e38d9c190877b72
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203391"
---
# <a name="sysdm_repl_traninfo-transact-sql"></a>sys.dm_repl_traninfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de cada transacción de la captura de datos modificados o replicada.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**fp2p_pub_exists**|**tinyint**|Si la transacción está en una base de datos publicada mediante la replicación transaccional del mismo nivel. Si es true, el valor es 1; en caso contrario, es 0.|  
|**db_ver**|**int**|Versión de la base de datos.|  
|**comp_range_address**|**varbinary(8**|Define un intervalo de reversiones parciales que deben omitirse.|  
|**textinfo_address**|**varbinary(8**|Dirección de memoria de la estructura de información de texto en caché.|  
|**fsinfo_address**|**varbinary(8**|Dirección de memoria de la estructura de información de la secuencia de archivo en caché.|  
|**begin_lsn**|**nvarchar (64)**|Número de flujo de registro (LSN) de la entrada de registro de inicio para la transacción.|  
|**commit_lsn**|**nvarchar (64)**|LSN de la entrada de registro de confirmación para la transacción.|  
|**DBID**|**smallint**|Id. de la base de datos.|  
|**rows**|**int**|Id. del comando replicado en la transacción.|  
|**elemento xdesid**|**nvarchar (64)**|Id. de la transacción.|  
|**artcache_table_address**|**varbinary(8**|Dirección de memoria de la última estructura de la tabla de artículos en caché utilizada para esta transacción.|  
|**servidor**|**nvarchar (514)**|Nombre de servidor.|  
|**server_len_in_bytes**|**smallint**|Longitud de caracteres, en bytes, del nombre del servidor.|  
|**database**|**nvarchar (514)**|nombre de base de datos.|  
|**db_len_in_bytes**|**smallint**|Longitud de caracteres, en bytes, del nombre de la base de datos.|  
|**llegar**|**nvarchar (514)**|Nombre del servidor donde se originó la transacción.|  
|**originator_len_in_bytes**|**smallint**|Longitud de caracteres, en bytes, del servidor donde se originó la transacción.|  
|**orig_db**|**nvarchar (514)**|Nombre de la base de datos donde se originó la transacción.|  
|**orig_db_len_in_bytes**|**smallint**|Longitud de caracteres, en bytes, de la base de datos donde se originó la transacción.|  
|**cmds_in_tran**|**int**|Número de comandos replicados en la transacción actual, que se utiliza para determinar cuándo debe confirmarse una transacción lógica.|  
|**is_boundedupdate_singleton**|**tinyint**|Especifica si una actualización de columna única solo afecta a una sola fila.|  
|**begin_update_lsn**|**nvarchar (64)**|LSN usado en una actualización de columna única.|  
|**delete_lsn**|**nvarchar (64)**|LSN que se va a eliminar como parte de una actualización.|  
|**last_end_lsn**|**nvarchar (64)**|Último LSN en una transacción lógica.|  
|**fcomplete**|**tinyint**|Especifica si el comando es una actualización parcial.|  
|**fcompensated**|**tinyint**|Especifica si la transacción está implicada en una reversión parcial.|  
|**fprocessingtext**|**tinyint**|Especifica si la transacción incluye una columna de tipo de datos binarios grandes.|  
|**max_cmds_in_tran**|**int**|Número máximo de comandos en una transacción lógica, como lo especifica el Agente de registro del LOG.|  
|**begin_time**|**datetime**|Hora de inicio de la transacción.|  
|**commit_time**|**datetime**|Hora de confirmación de la transacción.|  
|**session_id**|**int**|Id. de la sesión de examen del registro de captura de datos modificados. Esta columna se asigna a la columna **session_id** en [Sys.dm_cdc_logscan_sessions](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-log-scan-sessions.md).|  
|**session_phase**|**int**|Número que indica la fase en la que se encontraba la sesión en el momento de producirse el error. Esta columna se asigna a la columna **phase_number** en [Sys.dm_cdc_errors](../../relational-databases/system-dynamic-management-views/change-data-capture-sys-dm-cdc-errors.md).|  
|**is_known_cdc_tran**|**bit**|Indica que se realiza el seguimiento de la transacción mediante la captura de datos modificados.<br /><br /> 0 = Transacción replicada.<br /><br /> 1 = Transacción de la captura de datos modificados|  
|**error_count**|**int**|Número máximo de errores detectados|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DATABASE STATE en la base de datos de publicación o en la base de datos habilitada para la captura de datos modificados.  
  
## <a name="remarks"></a>Observaciones  
 Solo se devuelve la información para objetos de base de datos replicados o tablas habilitadas para la captura de datos modificados que está cargada actualmente en la caché del artículos.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas de administración dinámica relacionadas con la replicación &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/replication-related-dynamic-management-views-transact-sql.md)   
 [Vistas de administración dinámica relacionadas con la captura de datos modificados &#40;Transact-SQL&#41;](./system-dynamic-management-views.md)  
  
