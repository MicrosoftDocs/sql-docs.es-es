---
description: sp_update_proxy (Transact-SQL)
title: sp_update_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_update_proxy
- sp_update_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER PROXY statement
- sp_update_proxy
ms.assetid: 864fd0e6-9d61-4f07-92ef-145318d2f881
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f12d4666ed97033b19d7a0d4e39b17806e4d0953
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171722"
---
# <a name="sp_update_proxy-transact-sql"></a>sp_update_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia las propiedades de un proxy existente.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_update_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @new_name = ] 'new_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_id = ] id` Número de identificación del proxy que se va a cambiar. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy que se va a cambiar. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @credential_name = ] 'credential_name'` El nombre de la nueva credencial para el proxy. La *credential_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar *credential_name* o *credential_id* .  
  
`[ @credential_id = ] credential_id` El número de identificación de la nueva credencial para el proxy. La *credential_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar *credential_name* o *credential_id* .  
  
`[ @new_name = ] 'new_name'` Nuevo nombre del proxy. La *new_name* es de **tipo sysname y su** valor predeterminado es NULL. Cuando se proporciona, el procedimiento cambia el nombre del proxy a *new_name*. Si este argumento es NULL, el nombre del proxy no varía.  
  
`[ @enabled = ] is_enabled` Indica si el proxy está habilitado. La marca *is_enabled* es **tinyint** y su valor predeterminado es NULL. Cuando *is_enabled* es **0**, el proxy no está habilitado y no se puede usar en un paso de trabajo. Si este argumento es NULL, el estado del proxy no varía.  
  
`[ @description = ] 'description'` Nueva descripción del proxy. La *Descripción* es de tipo **nvarchar (512)** y su valor predeterminado es NULL. Si este argumento es NULL, la descripción del proxy no varía.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Se debe especificar **\@ proxy_name** o **\@ proxy_id** . Si se especifican los dos argumentos, deben hacer referencia al mismo proxy; de lo contrario, el procedimiento almacenado genera un error.  
  
 Se debe especificar **\@ credential_name** o **\@ credential_id** para cambiar la credencial para el proxy. Si se especifican los dos argumentos, deben hacer referencia a la misma credencial; de lo contrario, el procedimiento almacenado genera un error.  
  
 Este procedimiento cambia el proxy, pero no cambia el acceso al proxy. Para cambiar el acceso a un proxy, use **sp_grant_login_to_proxy** y **sp_revoke_login_from_proxy**.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de seguridad **sysadmin** pueden ejecutar este procedimiento.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se establece el valor habilitado para el `Catalog application proxy` en `0`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_update_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 0;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implementar la seguridad de Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_add_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
