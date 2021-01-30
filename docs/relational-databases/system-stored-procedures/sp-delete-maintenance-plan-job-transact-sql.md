---
description: sp_delete_maintenance_plan_job (Transact-SQL)
title: sp_delete_maintenance_plan_job (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_delete_maintenance_plan_job
- sp_delete_maintenance_plan_job_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_delete_maintenance_plan_job
ms.assetid: 1c2148c3-2928-4d9b-b1c8-3512cfbd6a63
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 090fc42c20612f737bd0ed7b0be910d59dd1eb12
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183411"
---
# <a name="sp_delete_maintenance_plan_job-transact-sql"></a>sp_delete_maintenance_plan_job (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita la asociación del plan de mantenimiento especificado con el trabajo especificado.  
  
> [!NOTE]  
>  Este procedimiento almacenado se utiliza con planes de mantenimiento de bases de datos. Esta característica se ha reemplazado por planes de mantenimiento que no utilizan este procedimiento almacenado. Utilice este procedimiento para conservar planes de mantenimiento de bases de datos en instalaciones actualizadas de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_delete_maintenance_plan_job [ @plan_id = ] 'plan_id' ,   
   [ @job_id = ] 'job_id'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @plan_id = ] 'plan\_id'` Especifica el identificador del plan de mantenimiento. *plan_id* es de tipo **uniqueidentifier** y debe ser un ID. válido.  
  
`[ @job_id = ] 'job\_id'` Especifica el identificador del trabajo al que está asociado el plan de mantenimiento. *job_id* es de tipo **uniqueidentifier** y debe ser un ID. válido.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_delete_maintenance_plan_job** se debe ejecutar desde la base de datos **msdb** .  
  
 Cuando se han quitado todos los trabajos del plan de mantenimiento, se recomienda que los usuarios ejecuten **sp_delete_maintenance_plan_db** para quitar las bases de datos restantes del plan.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_delete_maintenance_plan_job**.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se elimina el trabajo "B8FCECB1-E22C-11D2-AA64-00C04F688EAE" del plan de mantenimiento.  
  
```  
EXECUTE   sp_delete_maintenance_plan_job N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC', N'B8FCECB1-E22C-11D2-AA64-00C04F688EAE';  
```  
  
## <a name="see-also"></a>Vea también  
 [Planes de mantenimiento](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Procedimientos almacenados del plan de mantenimiento de bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
