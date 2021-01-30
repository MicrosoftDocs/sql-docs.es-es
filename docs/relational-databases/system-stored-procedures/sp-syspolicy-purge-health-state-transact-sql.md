---
description: sp_syspolicy_purge_health_state (Transact-SQL)
title: sp_syspolicy_purge_health_state (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syspolicy_purge_health_state_TSQL
- sp_syspolicy_purge_health_state
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syspolicy_purge_health_state
ms.assetid: 4ba4aa91-4c19-41c7-b70d-5fd9d0e89a5e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: e82c254c11196a4266800525d6f906905ac2bf44
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176397"
---
# <a name="sp_syspolicy_purge_health_state-transact-sql"></a>sp_syspolicy_purge_health_state (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina los estados de mantenimiento de directivas en la administración basada en directivas. Los estados de mantenimiento de directivas son indicadores visuales (un símbolo de desplazamiento con una "X" roja) dentro del Explorador de objetos que permiten determinar qué nodos han sufrido un error en la evaluación de una directiva.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_syspolicy_purge_health_state [ @target_tree_root_with_id = ] 'target_tree_root_with_id'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @target_tree_root_with_id = ] 'target_tree_root_with_id'` Representa el nodo en Explorador de objetos donde desea borrar el estado de mantenimiento. *target_tree_root_with_id* es de tipo **nvarchar (400)** y su valor predeterminado es NULL.  
  
 Puede especificar los valores de la columna target_query_expression_with_id de la vista de sistema msdb.dbo.syspolicy_system_health_state.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Debe ejecutar sp_syspolicy_purge_health_state en el contexto de la base de datos del sistema msdb.  
  
 Si ejecuta este procedimiento almacenado sin ningún parámetro, el estado de mantenimiento del sistema se elimina para todos los nodos del Explorador de objetos.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos PolicyAdministratorRole.  
  
> [!IMPORTANT]  
>  Posible elevación de credenciales: los usuarios del rol PolicyAdministratorRole pueden crear desencadenadores del servidor y programar ejecuciones de directivas que pueden afectar al funcionamiento de la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Por ejemplo, los usuarios del rol PolicyAdministratorRole pueden crear una directiva que puede evitar que la mayoría de los objetos se creen en [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Debido a esta posible elevación de credenciales, el rol PolicyAdministratorRole se debería conceder únicamente a los usuarios que sean de confianza para controlar la configuración de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se eliminan los estados de mantenimiento de un nodo concreto del Explorador de objetos.  
  
```  
EXEC msdb.dbo.sp_syspolicy_purge_health_state @target_tree_root_with_id = 'Server/Database[@ID=7]';  
  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de administración basada en directivas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/policy-based-management-stored-procedures-transact-sql.md)  
  
  
