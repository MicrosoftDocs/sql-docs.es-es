---
description: sp_syspolicy_update_policy_category (Transact-SQL)
title: sp_syspolicy_update_policy_category (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_update_policy_category_TSQL
- sp_syspolicy_update_policy_category
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_update_policy_category
ms.assetid: 6b6413c2-7a3b-4eff-91d9-5db2011869d6
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8825e94add7ceb74f6adcc0d52fda768d5e777bc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161282"
---
# <a name="sp_syspolicy_update_policy_category-transact-sql"></a>sp_syspolicy_update_policy_category (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Actualiza si una categoría de directiva está establecida para asignar suscripciones de base de datos. Si la suscripción está asignada, la categoría de directiva se aplica a todas las bases de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syspolicy_update_policy_category { [ @name = ] 'name' | [ @policy_category_id = ] policy_category_id }  
    , [ @mandate_database_subscriptions = ] mandate_database_subscriptions ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Es el nombre de la categoría de directiva. *Name* es de **tipo sysname** y se debe especificar si *policy_category_id* es NULL.  
  
`[ @policy_category_id = ] policy_category_id` Es el identificador de la categoría de directiva. *policy_category_id* es de **tipo int** y se debe especificar si *Name* es NULL.  
  
`[ @mandate_database_subscriptions = ] mandate_database_subscriptions` Determina si la suscripción a la base de datos se asigna a la categoría de directiva. *mandate_database_subscriptions* es un valor de **bit** y su valor predeterminado es NULL. Puede usar alguno de los valores siguientes:  
  
-   0 = no asignado  
  
-   1 = asignado  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Debe ejecutar sp_syspolicy_update_policy_category en el contexto de la base de datos del sistema msdb.  
  
 Debe especificar un valor para cualquier *nombre* o para *policy_category_id*. Ambos no pueden ser NULL. Para obtener estos valores, consulte la vista del sistema msdb.dbo.syspolicy_policy_categories.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Posible elevación de credenciales: los usuarios del rol PolicyAdministratorRole pueden crear desencadenadores del servidor y programar ejecuciones de directivas que pueden afectar al funcionamiento de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Por ejemplo, los usuarios del rol PolicyAdministratorRole pueden crear una directiva que puede evitar que la mayoría de los objetos se creen en [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Debido a esta posible elevación de credenciales, el rol PolicyAdministratorRole se debería conceder únicamente a los usuarios que sean de confianza para controlar la configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente actualiza la categoría 'Finance' para asignar las suscripciones de base de datos.  
  
```  
EXEC msdb.dbo.sp_syspolicy_update_policy_category @name = N'Finance'  
, @mandate_database_subscriptions = 1;  
  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)   
 [sp_syspolicy_add_policy_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-add-policy-category-transact-sql.md)   
 [sp_syspolicy_delete_policy_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-delete-policy-category-transact-sql.md)   
 [sp_syspolicy_rename_policy_category &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-syspolicy-rename-policy-category-transact-sql.md)  
  
  
