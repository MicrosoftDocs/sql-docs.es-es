---
description: sp_delete_jobschedule (Transact-SQL)
title: sp_delete_jobschedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_jobschedule
- sp_delete_jobschedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_jobschedule
ms.assetid: 82fbb48b-603a-4016-a7fb-1ce17fb76919
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c06030d2cc646dd269a0b3cb6bf564ce9248c9de
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195517"
---
# <a name="sp_delete_jobschedule-transact-sql"></a>sp_delete_jobschedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una programación para un trabajo.  
  
 **sp_delete_jobschedule** solo se proporciona por compatibilidad con versiones anteriores.  
  
  
## <a name="remarks"></a>Observaciones  
 Ahora, las programaciones de trabajos se pueden administrar independientemente de los trabajos. Para quitar una programación de un trabajo, use **sp_detach_schedule**. Para eliminar una programación, use **sp_delete_schedule**.  
  
> **Nota: sp_delete_jobschedule** no admite programaciones que se adjuntan a varios trabajos. Si un script existente llama **sp_delete_jobschedule** para quitar una programación que está asociada a más de un trabajo, el procedimiento devuelve un error.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Los miembros del rol **sysadmin** pueden eliminar cualquier programación de trabajo. Los usuarios que no son miembros del rol **sysadmin** solo pueden eliminar las programaciones de trabajos que les pertenecen.  
  
## <a name="see-also"></a>Consulte también  
 [sp_delete_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-schedule-transact-sql.md)   
 [sp_detach_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-detach-schedule-transact-sql.md)   
 [Ver o modificar trabajos](../../ssms/agent/view-or-modify-jobs.md)   
 [sp_add_schedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)   
 [sp_help_jobschedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-jobschedule-transact-sql.md)   
 [sp_update_jobschedule &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-jobschedule-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
