---
description: sp_add_proxy (Transact-SQL)
title: sp_add_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_add_proxy
- sp_add_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE PROXY statement
- sp_add_proxy
ms.assetid: cb59df37-f103-439b-bec1-2871fb669a8b
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c60c3bb1ea9f96352662b72080afbfa1e76b1dce
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192393"
---
# <a name="sp_add_proxy-transact-sql"></a>sp_add_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega el proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_add_proxy  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @enabled = ] is_enabled ,  
    [ @description = ] 'description' ,  
    [ @credential_name = ] 'credential_name' ,  
    [ @credential_id = ] credential_id ,  
    [ @proxy_id = ] id OUTPUT   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy que se va a crear. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Cuando el *proxy_name* es null o una cadena vacía, el nombre del proxy tiene como valor predeterminado el *user_name* proporcionado.  
  
`[ @enabled = ] is_enabled` Especifica si el proxy está habilitado. La marca *is_enabled* es **tinyint** y su valor predeterminado es 1. Cuando *is_enabled* es **0**, el proxy no está habilitado y no se puede usar en un paso de trabajo.  
  
`[ @description = ] 'description'` Descripción del proxy. La descripción es de tipo **nvarchar (512)** y su valor predeterminado es NULL. La descripción permite documentar el proxy. El Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no le da otros usos. Por tanto, este argumento es opcional.  
  
`[ @credential_name = ] 'credential_name'` El nombre de la credencial para el proxy. La *credential_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *credential_name* o *credential_id* .  
  
`[ @credential_id = ] credential_id` El número de identificación de la credencial para el proxy. La *credential_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *credential_name* o *credential_id* .  
  
`[ @proxy_id = ] id OUTPUT` El número de identificación del proxy asignado al proxy si se ha creado correctamente.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 None  
  
## <a name="remarks"></a>Observaciones  
 Este procedimiento almacenado se debe ejecutar en la base de datos **msdb** .  
  
 Un proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] administra la seguridad para los pasos de trabajo que afectan a los subsistemas que no sean [!INCLUDE[tsql](../../includes/tsql-md.md)]. Cada proxy se corresponde con unas credenciales de seguridad. Un proxy puede tener acceso a cualquier número de subsistemas.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de seguridad **sysadmin** pueden ejecutar este procedimiento.  
  
 Los miembros del rol fijo de seguridad **sysadmin** pueden crear pasos de trabajo que utilizan cualquier proxy. Use el procedimiento almacenado [sp_grant_login_to_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md) para conceder a otros inicios de sesión acceso al proxy.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se crea un proxy para la credencial `CatalogApplicationCredential`. El código da por supuesto que la credencial ya existe. Para obtener más información sobre las credenciales, vea [Create CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md).  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_add_proxy  
    @proxy_name = 'Catalog application proxy',  
    @enabled = 1,  
    @description = 'Maintenance tasks on catalog application.',  
    @credential_name = 'CatalogApplicationCredential' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../t-sql/statements/create-credential-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
