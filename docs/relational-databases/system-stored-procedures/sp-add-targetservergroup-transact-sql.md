---
description: sp_add_targetservergroup (Transact-SQL)
title: sp_add_targetservergroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_targetservergroup
- sp_add_targetservergroup_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_add_targetservergroup
ms.assetid: acb69343-d766-46ff-b771-0c7655c5231a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f070ce20e9efc67f8d40b5597a7118aeecddfd32
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192385"
---
# <a name="sp_add_targetservergroup-transact-sql"></a>sp_add_targetservergroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega el grupo de servidores especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_targetservergroup [ @name = ] 'name'   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Nombre del grupo de servidores que se va a crear. *Name* es de **tipo sysname** y no tiene ningún valor predeterminado. *el nombre* no puede contener comas.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Los grupos de servidores de destino proporcionan una forma sencilla para asignar un trabajo a un grupo de servidores. Para obtener más información, vea [sp_apply_job_to_targets](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se crea el grupo de servidores de destino denominado `Servers Processing Customer Orders`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_targetservergroup  
    'Servers Processing Customer Orders' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_apply_job_to_targets &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-apply-job-to-targets-transact-sql.md)   
 [sp_delete_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_help_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
