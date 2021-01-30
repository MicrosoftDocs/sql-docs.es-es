---
description: sp_grant_login_to_proxy (Transact-SQL)
title: sp_grant_login_to_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_proxy
- sp_grant_login_to_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_login_to_proxy
ms.assetid: 90e1a6d5-a692-4462-a163-4b0709d83150
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: c59f8b19e63f491757386ba5562ea9e0106e92a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159807"
---
# <a name="sp_grant_login_to_proxy-transact-sql"></a>sp_grant_login_to_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede acceso a una entidad de seguridad al proxy.  

  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_grant_login_to_proxy   
     { [ @login_name = ] 'login_name'   
     | [ @fixed_server_role = ] 'fixed_server_role'   
     | [ @msdb_role = ] 'msdb_role' } ,   
     { [ @proxy_id = ] id | [ @proxy_name = ] 'proxy_name' }  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @login_name = ] 'login_name'` Nombre de inicio de sesión al que se va a conceder acceso. El *login_name* es **nvarchar (256)** y su valor predeterminado es NULL. Se debe especificar uno de **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** , o bien se produce un error en el procedimiento almacenado.  
  
`[ @fixed_server_role = ] 'fixed_server_role'` Rol fijo de servidor al que se va a conceder acceso. El *fixed_server_role* es **nvarchar (256)** y su valor predeterminado es NULL. Se debe especificar uno de **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** , o bien se produce un error en el procedimiento almacenado.  
  
`[ @msdb_role = ] 'msdb_role'` Rol de base de datos de la base de datos **msdb** al que se va a conceder acceso. El *msdb_role* es **nvarchar (256)** y su valor predeterminado es NULL. Se debe especificar uno de **\@ login_name**, **\@ fixed_server_role** o **\@ msdb_role** , o bien se produce un error en el procedimiento almacenado.  
  
`[ @proxy_id = ] id` Identificador del proxy al que se va a conceder acceso. El *identificador* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar uno de **\@ proxy_id** o **\@ proxy_name** , o bien se produce un error en el procedimiento almacenado.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy al que se va a conceder acceso. El *proxy_name* es **nvarchar (256)** y su valor predeterminado es NULL. Se debe especificar uno de **\@ proxy_id** o **\@ proxy_name** , o bien se produce un error en el procedimiento almacenado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_grant_login_to_proxy** se debe ejecutar desde la base de datos **msdb** .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_grant_login_to_proxy**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se permite que el inicio de sesión `adventure-works\terrid` use el proxy `Catalog application proxy` .  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_login_to_proxy  
    @login_name = N'adventure-works\terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [sp_add_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
