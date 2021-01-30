---
description: sp_help_maintenance_plan (Transact-SQL)
title: sp_help_maintenance_plan (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_maintenance_plan_TSQL
- sp_help_maintenance_plan
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_maintenance_plan
ms.assetid: e972a510-960e-41d6-93c5-c71cd581a585
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 4d996179f089b95a3270c9045778afaaf5c13cd9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200016"
---
# <a name="sp_help_maintenance_plan-transact-sql"></a>sp_help_maintenance_plan (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información relacionada con el plan de mantenimiento especificado. Si no se especifica un plan, este procedimiento almacenado devuelve información acerca de todos los planes de mantenimiento.  
  
> **Nota:** Este procedimiento almacenado se utiliza con los planes de mantenimiento de bases de datos. Esta característica se ha reemplazado por planes de mantenimiento que no utilizan este procedimiento almacenado. Utilice este procedimiento para conservar planes de mantenimiento de bases de datos en instalaciones actualizadas de una versión anterior de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_maintenance_plan [ [ @plan_id = ] 'plan_id' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @plan_id = ] 'plan\_id'` Especifica el identificador del plan de mantenimiento. *plan_id* es de tipo **uniqueidentifier**. El valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 None  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Si se especifica *plan_id* , **sp_help_maintenance_plan** devolverá tres tablas: Plan, base de datos y trabajo.  
  
### <a name="plan-table"></a>Tabla Plan  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**plan_id**|**uniqueidentifier**|Id. del plan de mantenimiento.|  
|**plan_name**|**sysname**|Nombre del plan de mantenimiento.|  
|**date_created**|**datetime**|Fecha de creación del plan de mantenimiento.|  
|**owner**|**sysname**|Propietario del plan de mantenimiento.|  
|**max_history_rows**|**int**|Número máximo de filas asignadas para registrar el historial del plan de mantenimiento de la tabla del sistema.|  
|**remote_history_server**|**int**|Nombre del servidor remoto en el que se puede escribir el informe de historial.|  
|**max_remote_history_rows**|**int**|Número máximo de filas asignadas en la tabla del sistema en un servidor remoto en el que se puede escribir el informe del historial.|  
|**user_defined_1**|**int**|El valor predeterminado es NULL.|  
|**user_defined_2**|**nvarchar(100**|El valor predeterminado es NULL.|  
|**user_defined_3**|**datetime**|El valor predeterminado es NULL.|  
|**user_defined_4**|**uniqueidentifier**|El valor predeterminado es NULL.|  
  
### <a name="database-table"></a>Tabla Database  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**database_name**|Nombre de todas las bases de datos asociadas con el plan de mantenimiento. *database_name* es **sysname**.|  
  
### <a name="job-table"></a>Tabla Job  
  
|Nombre de la columna|Descripción|  
|-----------------|-----------------|  
|**job_id**|Identificador de todos los trabajos asociados con el plan de mantenimiento. *job_id* es de tipo **uniqueidentifier**.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_help_maintenance_plan** está en la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_help_maintenance_plan**.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se proporciona información descriptiva acerca del plan de mantenimiento FAD6F2AB-3571-11D3-9D4A-00C04FB925FC.  
  
```  
EXECUTE   sp_help_maintenance_plan   
   N'FAD6F2AB-3571-11D3-9D4A-00C04FB925FC';  
```  
  
## <a name="see-also"></a>Vea también  
 [Planes de mantenimiento](../../relational-databases/maintenance-plans/maintenance-plans.md)   
 [Procedimientos almacenados del plan de mantenimiento de bases de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-maintenance-plan-stored-procedures-transact-sql.md)  
  
  
