---
description: sys.server_file_audits (Transact-SQL)
title: sys.server_file_audits (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 04/05/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- server_file_audits_TSQL
- sys.server_file_audits_TSQL
- sys.server_file_audits
- server_file_audits
dev_langs:
- TSQL
helpviewer_keywords:
- sys.server_file_audits catalog view
ms.assetid: 553288a0-be57-4d79-ae53-b7cbd065e127
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 01cc8c02293c7e1ff5c26af7a9c3fbc579746d51
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193813"
---
# <a name="sysserver_file_audits-transact-sql"></a>sys.server_file_audits (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información extendida sobre el tipo de auditoría de archivos en una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Auditoría en una instancia de servidor. Para obtener más información, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|audit_id|**int**|Identificador de la auditoría.|  
|name|**sysname**|Nombre de la auditoría.|  
|audit_guid|**uniqueidentifier**|GUID de la auditoría.|  
|create_date|**datetime**|Fecha UTC en la que se creó la auditoría de archivos.|  
|modify_date|**datatime**|Fecha UTC en la que se modificó por última vez la auditoría de archivos.|  
|principal_id|**int**|Identificador del propietario de la auditoría tal y como se registró en el servidor.|  
|tipo|**char(2)**|Tipo de auditoría:<br /><br /> SL = registro de eventos de seguridad de NT<br /><br /> AL = registro de eventos de aplicación NT<br /><br /> FL = archivo en el sistema de archivos|  
|type_desc|**nvarchar(60)**|Descripción del tipo de auditoría.|  
|on_failure|**tinyint**|Condición en caso de error:<br /><br /> 0 = Continuar<br /><br /> 1 = Cerrar la instancia del servidor<br /><br /> 2 = Error en la operación|  
|on_failure_desc|**nvarchar(60)**|En caso de error escribir una entrada de acción:<br /><br /> CONTINUE<br /><br /> SHUTDOWN SERVER INSTANCE<br /><br /> FAIL OPERATION|  
|is_state_enabled|**tinyint**|0 = Deshabilitado<br /><br /> 1 = Habilitado|  
|queue_delay|**int**|Tiempo máximo sugerido, en milisegundos, que debe transcurrir antes de realizar la escritura en disco. Si es igual a 0, la auditoría garantizará la escritura antes de que continúe el evento.|  
|predicate|**nvarchar (8000)**|Expresión de predicado aplicada al evento.|  
|max_file_size|**bigint**|Tamaño máximo de la auditoría, expresado en megabytes:<br /><br /> 0 = Ilimitado o no aplicable al tipo de auditoría seleccionada.|  
|max_rollover_files|**int**|Número máximo de archivos que se van a utilizar con la opción de sustitución incremental.|  
|max_files|**int**|Número máximo de archivos que se van a utilizar sin la opción de sustitución incremental.|  
|reserved_disk_space|**int**|Cantidad de espacio en disco que se debe reservar para cada archivo.|  
|log_file_path|**nvarchar(260)**|Ruta de acceso a la auditoría. Para auditorías de archivos, la ruta de acceso del archivo; para auditorías de registro de la aplicación, la ruta de acceso del registro de la aplicación.|  
|log_file_name|**nvarchar(260)**|Nombre base para el archivo de registro proporcionado en la instrucción DDL CREATE AUDIT. Se agrega un número incremental al archivo base_log_name como un sufijo para crear el nombre del archivo de registro.|  
  
## <a name="permissions"></a>Permisos  
 Las entidades de seguridad con el permiso **ALTER any Server Audit** o **View any Definition** tienen acceso a esta vista de catálogo. Además, no se debe denegar el permiso **View any Definition** a la entidad de seguridad.  
  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits (Transact-SQL)](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Crear una auditoría de servidor y una especificación de auditoría de servidor](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
