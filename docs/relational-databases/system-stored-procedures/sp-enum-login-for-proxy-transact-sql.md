---
description: sp_enum_login_for_proxy (Transact-SQL)
title: sp_enum_login_for_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_enum_login_for_proxy_TSQL
- sp_enum_login_for_proxy
dev_langs:
- TSQL
helpviewer_keywords:
- sp_enum_login_for_proxy
ms.assetid: 62a75019-248a-44c8-a5cc-c79f55ea3acf
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: b51d02a66833df04a82a970319bab4e62f462a33
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186960"
---
# <a name="sp_enum_login_for_proxy-transact-sql"></a>sp_enum_login_for_proxy (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra las asociaciones entre las entidades de seguridad y los servidores proxy.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_enum_login_for_proxy  
    [ @name = ] 'name'  
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name = ] 'name'` El nombre de una [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] entidad de seguridad, Inicio de sesión, rol de servidor o rol de base de datos **msdb** para el que se van a enumerar los servidores proxy. El nombre es **nvarchar (256)** y su valor predeterminado es NULL.  
  
`[ @proxy_id = ] id` Número de identificación del proxy del que se va a mostrar información. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy del que se va a mostrar información. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Número de identificación del proxy.|  
|**proxy_name**|**sysname**|Nombre del proxy.|  
|**name**|**sysname**|Nombre de la entidad de seguridad para la asociación.|  
|**flags**|**int**|Tipo de la entidad de seguridad.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión<br /><br /> **1** = rol fijo del sistema<br /><br /> **2** = rol de base de datos en **msdb**|  
| &nbsp; | &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Observaciones  
 Cuando no se proporcionan parámetros, **sp_enum_login_for_proxy** muestra información acerca de todos los inicios de sesión de la instancia para cada proxy.  
  
 Cuando se proporciona un identificador de proxy o un nombre de proxy, **sp_enum_login_for_proxy** muestra los inicios de sesión que tienen acceso al proxy. Cuando se proporciona un nombre de inicio de sesión, **sp_enum_login_for_proxy** muestra los servidores proxy a los que tiene acceso el inicio de sesión.  
  
 Cuando se suministra información acerca del proxy y un nombre de inicio de sesión, el conjunto de resultados devuelve una fila si el inicio de sesión especificado tiene acceso al proxy especificado.  
  
 Este procedimiento almacenado se encuentra en **msdb**.  
  
## <a name="permissions"></a>Permisos  
 Los permisos de ejecución para este procedimiento tienen como valor predeterminado los miembros del rol fijo de servidor **sysadmin** .  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-associations"></a>A. Mostrar todas las asociaciones  
 En el ejemplo siguiente se muestran todos los permisos establecidos entre los inicios de sesión y los servidores proxy de la instancia actual.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy ;  
GO  
```  
  
### <a name="b-listing-proxies-for-a-specific-login"></a>B. Mostrar los servidores proxy para un inicio de sesión específico  
 En el ejemplo siguiente se muestran los servidores proxy a los que tiene acceso el inicio de sesión `terrid`.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_enum_login_for_proxy  
    @name = 'terrid' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_help_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-help-proxy-transact-sql.md)   
 [sp_grant_login_to_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-login-to-proxy-transact-sql.md)   
 [sp_revoke_login_from_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-login-from-proxy-transact-sql.md)  
  
  
