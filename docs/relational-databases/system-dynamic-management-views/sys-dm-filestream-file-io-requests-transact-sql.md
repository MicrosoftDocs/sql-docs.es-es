---
description: sys.dm_filestream_file_io_requests (Transact-SQL)
title: sys.dm_filestream_file_io_requests (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_filestream_file_io_requests
- dm_filestream_file_io_requests
- sys.dm_filestream_file_io_requests_TSQL
- dm_filestream_file_io_requests_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_requests catalog view
ms.assetid: d41e39a5-14d5-4f3d-a2e3-a822b454c1ed
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e4ee0854a59ae465f8eff35dadb90ac33a499651
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186094"
---
# <a name="sysdm_filestream_file_io_requests-transact-sql"></a>sys.dm_filestream_file_io_requests (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una lista de solicitudes de E/S que el propietario del espacio de nombres (NSO) está procesando en el momento dado.  
  
|Columna|Tipo|Descripción|  
|------------|----------|-----------------|  
|**request_context_address**|**varbinary(8**|Muestra la dirección interna del bloque de memoria del NSO que contiene la solicitud de E/S desde el controlador. No admite valores NULL.|  
|**current_spid**|**smallint**|Muestra el ID. de proceso del sistema (SPID) de la conexión del SQL Server actual. No admite valores NULL.|  
|**request_type**|**nvarchar(60)**|Muestra el tipo de paquete de solicitud de E/S (IRP). Los tipos de solicitud posibles son REQ_PRE_CREATE, REQ_POST_CREATE, REQ_RESOLVE_VOLUME, REQ_GET_VOLUME_INFO, REQ_GET_LOGICAL_NAME, REQ_GET_PHYSICAL_NAME, REQ_PRE_CLEANUP, REQ_POST_CLEANUP, REQ_CLOSE, REQ_FSCTL, REQ_QUERY_INFO, REQ_SET_INFO, REQ_ENUM_DIRECTORY, REQ_QUERY_SECURITY y REQ_SET_SECURITY. No admite valores NULL|  
|**request_state**|**nvarchar(60)**|Muestra el estado de la solicitud de E/S en el NSO. Los valores posibles son REQ_STATE_RECEIVED, REQ_STATE_INITIALIZED, REQ_STATE_ENQUEUED, REQ_STATE_PROCESSING, REQ_STATE_FORMATTING_RESPONSE, REQ_STATE_SENDING_RESPONSE, REQ_STATE_COMPLETING y REQ_STATE_COMPLETED. No admite valores NULL.|  
|**id_de_solicitud**|**int**|Muestra el identificador de solicitud único asignado por el controlador a esta solicitud. No admite valores NULL.|  
|**irp_id**|**int**|Muestra el identificador de IRP único. Esto es útil para identificar todas las solicitudes de E/S relacionadas con el IRP determinado. No admite valores NULL.|  
|**handle_id**|**int**|Indica el id. del identificador del espacio de nombres. Se trata del identificador específico del NSO y es único en una instancia. No admite valores NULL.|  
|**client_thread_id**|**varbinary(8**|Muestra el identificador de subproceso de la aplicación cliente que origina la solicitud.<br /><br /> ADVERTENCIA esto solo es significativo si la aplicación cliente se ejecuta en el mismo equipo que SQL Server. **\* \* \* \*** Cuando la aplicación cliente se ejecuta de forma remota, en el **client_thread_id** se muestra el identificador de subproceso del proceso del sistema que funciona en nombre del cliente remoto.<br /><br /> Acepta valores NULL.|  
|**client_process_id**|**varbinary(8**|Muestra el identificador de proceso de la aplicación cliente si esta se ejecuta en la misma máquina que SQL Server. Para un cliente remoto, muestra el identificador de proceso del sistema que funciona en nombre de la aplicación cliente. Acepta valores NULL.|  
|**handle_context_address**|**varbinary(8**|Muestra la dirección de la estructura NSO interna asociada con el identificador del cliente. Acepta valores NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Muestra el identificador de la transacción asociada con el identificador determinado y todas las solicitudes asociadas con este identificador. Es el valor devuelto por la función **get_filestream_transaction_context** . Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de FileStream y FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
