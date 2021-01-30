---
description: syspolicy_policy_categories (Transact-SQL)
title: syspolicy_policy_categories (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syspolicy_policy_categories
- syspolicy_policy_categories_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syspolicy_policy_groups view
ms.assetid: 65f080c7-771f-4cf6-a7a0-88882c637f8d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8cf37ae7776251f1d13c9671a65458a5a31cb285
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180790"
---
# <a name="syspolicy_policy_categories-transact-sql"></a>syspolicy_policy_categories (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una fila para cada categoría de directiva de administración basada en directivas en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Las categorías de directivas ayudan a organizar las directivas cuando se tienen muchas. En la tabla siguiente se describen las columnas de la vista syspolicy_policy_groups.  
 
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|policy_category_id|**int**|Identificador de la categoría de directiva.|  
|name|**sysname**|Nombre de la categoría de directiva.|  
|mandate_database_subscriptions|**bit**|Indica si la categoría de directiva se aplica a todas las bases de datos en una instancia sin una suscripción explícita (1) o si se debe aplicar a una base de datos utilizando una suscripción explícita (0).|  
  
## <a name="remarks"></a>Observaciones  
 Muestra una lista de grupos de directivas de administración basada en directivas.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol PolicyAdministratorRole en la base de datos msdb.  
  
## <a name="see-also"></a>Consulte también  
 [Administrar servidores mediante administración basada en directivas](../../relational-databases/policy-based-management/administer-servers-by-using-policy-based-management.md)   
 [Vistas de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/policy-based-management-views-transact-sql.md)  
  
  
