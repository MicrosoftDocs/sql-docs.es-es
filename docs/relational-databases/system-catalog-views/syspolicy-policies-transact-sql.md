---
description: syspolicy_policies (Transact-SQL)
title: syspolicy_policies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policies_TSQL
- syspolicy_policies
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policies view
ms.assetid: aecf35bb-187e-4f80-870f-48081b88974e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8547fa15c966e6835c0b100e628217bc0281991e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192988"
---
# <a name="syspolicy_policies-transact-sql"></a>syspolicy_policies (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una fila para cada directiva de administración basada en directivas en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . syspolicy_policies pertenece al esquema DBO en la base de datos msdb. En la tabla siguiente se describen las columnas de la vista syspolicy_policies.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|policy_id|**int**|Identificador de la directiva.|  
|name|**sysname**|Nombre de la directiva.|  
|condition_id|**int**|Identificador de la condición exigida o probada por esta directiva.|  
|root_condition_id|**int**|Solo para uso interno.|  
|date_created|**datetime**|Fecha y hora cuando se creó la directiva.|  
|execution_mode|**int**|Modo de evaluación para la directiva. Los valores posibles son:<br /><br /> 0 = A petición<br /><br /> Este modo evalúa la directiva cuando lo especifica el usuario directamente.<br /><br /> 1 = Al cambiar: impedir<br /><br /> Este modo automatizado utiliza desencadenadores DDL para evitar infracciones de las directivas.<br /><br /> 2 = Al cambiar: solo registrar<br /><br /> Este modo automatizado utiliza la notificación de eventos para evaluar una directiva cuando se produce un cambio relevante y registra las infracciones de la directiva.<br /><br /> 4 = Al programar<br /><br /> Este modo automatizado utiliza un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para evaluar una directiva periódicamente. El modo registra las infracciones de la directiva.<br /><br /> Nota: el valor 3 no es un valor posible.|  
|policy_category|**int**|Identificador de la categoría de directivas de administración basada en directivas al que esta directiva pertenece. Es NULL si es el grupo de directivas predeterminado.|  
|schedule_uid|**uniqueidentifier**|Cuando execution_mode es Al programar, contiene el identificador de la programación; de lo contrario, es NULL.|  
|description|**nvarchar(max)**|Descripción de la directiva. La columna de descripción es opcional y puede ser NULL.|  
|help_text|**nvarchar(4000)**|Texto del hipervínculo que pertenece a help_link.|  
|help_link|**nvarchar (2083)**|Hipervínculo de ayuda adicional que el creador de la directiva asigna a la misma.|  
|object_set_id|**int**|Identificador del conjunto de objetos que la directiva evalúa.|  
|is_enabled|**bit**|Indica si la directiva está habilitada (1) o deshabilitada (0) actualmente.|  
|job_id|**uniqueidentifier**|Cuando execution_mode es Al programar, contiene el identificador del trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que ejecuta la directiva.|  
|created_by|**sysname**|Inicio de sesión que creó la Directiva.|  
|modified_by|**sysname**|Inicio de sesión que modificó la directiva por última vez. Es NULL si nunca se produjo una modificación.|  
|date_modified|**datetime**|Fecha y hora cuando se creó la directiva. Es NULL si nunca se produjo una modificación.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando solucione problemas de la administración basada en directivas, consulte la vista [syspolicy_conditions](../../relational-databases/system-catalog-views/syspolicy-conditions-transact-sql.md) para determinar si la Directiva está habilitada. Esta vista también muestra quién creó la directiva o la cambió en último lugar.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vistas de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
