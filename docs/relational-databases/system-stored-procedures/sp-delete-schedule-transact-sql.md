---
description: sp_delete_schedule (Transact-SQL)
title: sp_delete_schedule (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_schedule
- sp_delete_schedule_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_schedule
ms.assetid: 18b2c985-47b8-49c8-82d1-8a4af3d7d33a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b152260dca3ab9791124b9f7f1ac13e311f6e514
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199157"
---
# <a name="sp_delete_schedule-transact-sql"></a>sp_delete_schedule (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina una programación.  
 
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_schedule { [ @schedule_id = ] schedule_id | [ @schedule_name = ] 'schedule_name' } ,  
     [ @force_delete = ] force_delete  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @schedule_id = ] schedule_id` El número de identificación de la programación que se va a eliminar. *schedule_id* es de **tipo int** y su valor predeterminado es NULL.  
  
> **Nota:** Se debe especificar *schedule_id* o *schedule_name* , pero no se pueden especificar ambos.  
  
`[ @schedule_name = ] 'schedule_name'` Nombre de la programación que se va a eliminar. *schedule_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> **Nota:** Se debe especificar *schedule_id* o *schedule_name* , pero no se pueden especificar ambos.  
  
`[ @force_delete = ] force_delete` Especifica si el procedimiento debe generar un error si la programación está adjunta a un trabajo. *Force_delete* es de bit y su valor predeterminado es **0**. Cuando *force_delete* es **0**, el procedimiento almacenado genera un error si la programación está adjunta a un trabajo. Cuando *force_delete* es **1**, la programación se elimina independientemente de si la programación está adjunta a un trabajo.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 De manera predeterminada, una programación no se puede eliminar si está adjunta a un trabajo. Para eliminar una programación adjunta a un trabajo, especifique un valor de **1** para *force_delete*. La eliminación de una programación no detiene los trabajos actualmente en ejecución.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. Al resto de usuarios se les debe conceder uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** :  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
 Observe que el propietario del trabajo puede adjuntar un trabajo a una programación y separar un trabajo de una programación sin tener que ser asimismo el propietario de la programación. Sin embargo, no se puede eliminar una programación si la desasociación la dejaría sin trabajos, a menos que el autor de la llamada sea el propietario de la programación.  
  
 Para detalles sobre los permisos de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
 Solo los miembros del rol **sysadmin** pueden eliminar una programación de trabajo que sea propiedad de otro usuario.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-deleting-a-schedule"></a>A. Eliminar una programación  
 En el ejemplo siguiente se elimina la programación `NightlyJobs`. Si la programación está adjunta a algún trabajo, en el ejemplo no se elimina la programación.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = N'NightlyJobs' ;  
GO  
```  
  
### <a name="b-deleting-a-schedule-attached-to-a-job"></a>B. Eliminar una programación adjunta a un trabajo  
 En el ejemplo siguiente se elimina la programación `RunOnce`, independientemente de si está adjunta a un trabajo.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_delete_schedule  
    @schedule_name = 'RunOnce',  
    @force_delete = 1;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Implementar trabajos](../../ssms/agent/implement-jobs.md)   
 [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
  
