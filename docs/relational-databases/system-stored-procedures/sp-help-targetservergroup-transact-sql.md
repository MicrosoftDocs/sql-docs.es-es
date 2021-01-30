---
description: sp_help_targetservergroup (Transact-SQL)
title: sp_help_targetservergroup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_targetservergroup_TSQL
- sp_help_targetservergroup
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_targetservergroup
ms.assetid: ec3a4a68-b591-431c-9518-053ede522d0c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 200b6b0a8d6a6700f7aa98db0cde45cfc278dc53
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186706"
---
# <a name="sp_help_targetservergroup-transact-sql"></a>sp_help_targetservergroup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra en una lista todos los servidores de destino del grupo especificado. Si no se especifica ningún grupo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] devuelve información acerca de todos los grupos de servidores de destino.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_targetservergroup  
    [ [ @name = ] 'name' ]  
```  
  
## <a name="argument"></a>Argumento  
`[ @name = ] 'name'` Es el nombre del grupo de servidores de destino para el que se va a devolver información. *Name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**servergroup_id**|**int**|Número de identificación del grupo de servidores.|  
|**name**|**sysname**|Nombre del grupo de servidores|  
  
## <a name="permissions"></a>Permisos  
 Los permisos para ejecutar este procedimiento tienen como valor predeterminado el rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-information-for-all-target-server-groups"></a>A. Presentar información de todos los grupos de servidores de destino  
 En este ejemplo se presenta información de todos los grupos de servidores de destino.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-target-server-group"></a>B. Presentar información de un grupo de servidores de destino específico  
 En este ejemplo se presenta información del grupo de servidores de destino `Servers Maintaining Customer Information`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_targetservergroup   
    N'Servers Maintaining Customer Information' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_add_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-targetservergroup-transact-sql.md)   
 [sp_delete_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-targetservergroup-transact-sql.md)   
 [sp_update_targetservergroup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-targetservergroup-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
