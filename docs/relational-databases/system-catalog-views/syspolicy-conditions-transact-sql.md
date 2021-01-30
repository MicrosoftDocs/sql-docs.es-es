---
description: syspolicy_conditions (Transact-SQL)
title: syspolicy_conditions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_conditions
- syspolicy_conditions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_conditions view
ms.assetid: af97d26c-4bd5-4b08-be51-8419e3b2832c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: e51c0d002d697898f4ac5d47c32cfded8235db35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199676"
---
# <a name="syspolicy_conditions-transact-sql"></a>syspolicy_conditions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una fila para cada condición de administración basada en directivas en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . syspolicy_conditions pertenece al esquema DBO en la base de datos msdb. En la tabla siguiente se describen las columnas de la vista de syspolicy_conditions.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|condition_id|**int**|Identificador de esta condición. Cada condición representa una recopilación de una o varias expresiones de condición.|  
|name|**sysname**|Nombre de la condición.|  
|date_created|**datetime**|Fecha y hora en que se creó la condición.|  
|description|**nvarchar(max)**|Descripción de la condición. La columna de descripción es opcional y puede ser NULL.|  
|created_by|**sysname**|Inicio de sesión que creó la condición.|  
|modified_by|**sysname**|Inicio de sesión que modificó más recientemente la condición. Es NULL si nunca se produjo una modificación.|  
|date_modified|**datetime**|Fecha y hora en que se creó la condición. Es NULL si nunca se produjo una modificación.|  
|is_name_condition|**smallint**|Especifica si la condición es de denominación.<br /><br /> 0 = la expresión de condición no contiene la variable @Name.<br /><br /> 1 = la expresión de condición contiene la variable @Name.|  
|facet|**nvarchar(max)**|Nombre de la faceta en que está basada la condición.|  
|Expression|**nvarchar(max)**|Expresión de los estados de faceta.|  
|obj_name|**sysname**|Nombre de objeto asignado a @Name si la expresión de condición contiene esta variable.|  
  
## <a name="remarks"></a>Observaciones  
 Cuando esté solucionando problemas de la administración basada en directivas, consulte la vista de syspolicy_conditions para determinar quién creó o cambió por última vez la condición.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vistas de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
