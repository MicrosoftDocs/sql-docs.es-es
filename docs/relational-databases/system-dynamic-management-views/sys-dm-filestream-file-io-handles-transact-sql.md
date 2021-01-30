---
description: sys.dm_filestream_file_io_handles (Transact-SQL)
title: sys.dm_filestream_file_io_handles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_filestream_file_io_handles
- sys.dm_filestream_file_io_handles
- dm_filestream_file_io_handles_TSQL
- sys.dm_filestream_file_io_handles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_filestream_file_io_handle catalog view
ms.assetid: e59632f4-3292-419f-9217-ca375749f1a5
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 869d9ffbc532e5b819a11c19a387d7b83faae56f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198419"
---
# <a name="sysdm_filestream_file_io_handles-transact-sql"></a>sys.dm_filestream_file_io_handles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra los identificadores de archivo que conoce el propietario del espacio de nombres (NSO). En esta vista se muestran los identificadores de FileStream que un cliente ha recibido mediante **OpenSqlFilestream** .  
  
|Columna|Tipo|Descripción|  
|------------|----------|-----------------|  
|**handle_context_address**|**varbinary(8**|Muestra la dirección de la estructura NSO interna asociada con el identificador del cliente. Acepta valores NULL.|  
|**creation_request_id**|**int**|Muestra un campo desde la solicitud de E/S de REQ_PRE_CREATE usada para crear este controlador. No admite valores NULL.|  
|**creation_irp_id**|**int**|Muestra un campo desde la solicitud de E/S de REQ_PRE_CREATE usada para crear este controlador. No admite valores NULL|  
|**handle_id**|**int**|Muestra el identificador único de este controlador asignado por el controlador. No admite valores NULL.|  
|**creation_client_thread_id**|**varbinary(8**|Muestra un campo desde la solicitud de E/S de REQ_PRE_CREATE usada para crear este controlador. Acepta valores NULL.|  
|**creation_client_process_id**|**varbinary(8**|Muestra un campo desde la solicitud de E/S de REQ_PRE_CREATE usada para crear este controlador. Acepta valores NULL.|  
|**filestream_transaction_id**|**varbinary(128)**|Muestra el identificador de la transacción asociado con el controlador determinado. Este es el valor devuelto por la función **get_filestream_transaction_context** . Utilice este campo para unirse a la vista **Sys.dm_filestream_file_io_requests** . Acepta valores NULL.|  
|**access_type**|**nvarchar(60)**|No admite valores NULL.|  
|**logical_path**|**nvarchar(256)**|Muestra el nombre de ruta de acceso lógico del archivo que abrió este controlador. Es el mismo que devuelve el **. Método PathName** de FileStream de tipo **varbinary**(**Max**). Acepta valores NULL.|  
|**physical_path**|**nvarchar(256)**|Muestra el nombre de ruta de acceso NTFS real del archivo. Es el mismo directorio devuelto por **. Método PhysicalPathName** de FileStream de tipo **varbinary**(**Max**). Lo habilita la marca de seguimiento 5556. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de FileStream y FileTable &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/filestream-and-filetable-dynamic-management-views-transact-sql.md)  
  
  
