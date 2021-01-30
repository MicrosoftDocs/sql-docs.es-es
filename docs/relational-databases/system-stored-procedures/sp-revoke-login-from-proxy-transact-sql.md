---
description: sp_revoke_login_from_proxy (Transact-SQL)
title: sp_revoke_login_from_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_proxy_TSQL
- sp_revoke_login_from_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_login_from_proxy
ms.assetid: e4546c13-9fba-4bab-8b42-d6f18b33ec25
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9c95315de974558eaf82878d087af09d2ac8e81b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183077"
---
# <a name="sp_revoke_login_from_proxy-transact-sql"></a>sp_revoke_login_from_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita el acceso a un proxy correspondiente a una entidad de seguridad.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_revoke_login_from_proxy   
    [ @name = ] 'name' ,  
    [ @proxy_id = ] id ,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` Nombre del rol de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión, rol de servidor o base de datos **msdb** para el que se va a quitar el acceso. *Name* es de tipo **nvarchar (256)** y no tiene ningún valor predeterminado.  
  
`[ @proxy_id = ] id` Identificador del proxy al que se va a quitar el acceso. Se debe especificar el *identificador* o el *proxy_name* , pero no se pueden especificar ambos. El *identificador* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy al que se va a quitar el acceso. Se debe especificar el *identificador* o el *proxy_name* , pero no se pueden especificar ambos. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 Los trabajos propiedad del inicio de sesión que hace referencia a este proxy no podrán ejecutarse.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar este procedimiento almacenado, un usuario debe ser miembro del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
 El siguiente ejemplo revoca el acceso del inicio de sesión `terrid` al proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_login_from_proxy  
    @name = N'terrid',  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_help_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)  
  
  
