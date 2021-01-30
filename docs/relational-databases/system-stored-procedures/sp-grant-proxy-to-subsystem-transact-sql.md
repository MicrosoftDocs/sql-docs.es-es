---
description: sp_grant_proxy_to_subsystem (Transact-SQL)
title: sp_grant_proxy_to_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_grant_login_to_subsystem_TSQL
- sp_grant_login_to_subsystem
dev_langs:
- TSQL
helpviewer_keywords:
- sp_grant_proxy_to_subsystem
ms.assetid: 866aaa27-a1e0-453a-9b1b-af39431ad9c2
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 41cbd629572229b4a21c17aca989bc3ab25d5413
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171748"
---
# <a name="sp_grant_proxy_to_subsystem-transact-sql"></a>sp_grant_proxy_to_subsystem (Transact-SQL)

[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede acceso al proxy a un subsistema.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sp_grant_proxy_to_subsystem  
     { [ @proxy_id = ] proxy_id | [ @proxy_name = ] 'proxy_name' },  
     { [ @subsystem_id = ] subsystem_id | [ @subsystem_name = ] 'subsystem_name' }  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_id = ] id` Número de identificación del proxy al que se va a conceder acceso. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *proxy_id* o *proxy_name* , pero no se pueden especificar ambos.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy al que se va a conceder acceso. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *proxy_id* o *proxy_name* , pero no se pueden especificar ambos.  
  
`[ @subsystem_id = ] id` Número de ID. del subsistema al que se va a conceder acceso. La *subsystem_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *subsystem_id* o *subsystem_name* , pero no se pueden especificar ambos. En la tabla siguiente se muestran los valores disponibles para cada subsistema.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**2**|Script[!INCLUDE[msCoName](../../includes/msconame-md.md)] ActiveX<br /><br /> Importante el subsistema de scripts ActiveX se quitará del agente en una versión futura de **\* . \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.|  
|**3**|Sistema operativo (**CmdExec**)|  
|**4**|Agente de instantáneas de replicación|  
|**5**|Agente de registro del LOG de replicación|  
|**6**|Agente de distribución de replicación|  
|**7**|Replication Merge Agent|  
|**8**|Agente de lectura de cola de replicación|  
|**9**|Consulta de Analysis Services|  
|**10**|Comando de Analysis Services|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] ejecución de paquetes|  
|**12**|Script de PowerShell|  
| &nbsp; | &nbsp; |
  
`[ @subsystem_name = ] 'subsystem_name'` Nombre del subsistema al que se va a conceder acceso. La **subsystem_name** es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *subsystem_id* o *subsystem_name* , pero no se pueden especificar ambos. En la tabla siguiente se muestran los valores disponibles para cada subsistema.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**ActiveScripting**|Script ActiveX|  
|**CmdExec**|Sistema operativo (**CmdExec**)|  
|**Instantánea**|Agente de instantáneas de replicación|  
|**LogReader**|Agente de registro del LOG de replicación|  
|**Distribución**|Agente de distribución de replicación|  
|**Combinar**|Replication Merge Agent|  
|**QueueReader**|Agente de lectura de cola de replicación|  
|**ANALYSISQUERY**|Consulta de Analysis Services|  
|**ANALYSISCOMMAND**|Comando de Analysis Services|  
|**Dts**|Ejecución de paquetes SSIS|  
|**PowerShell**|Script de PowerShell|  
| &nbsp; | &nbsp; |
  
## <a name="remarks"></a>Observaciones  
 La concesión de acceso al proxy a un subsistema no cambia los permisos para la entidad de seguridad especificada en el proxy.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_grant_proxy_to_subsystem**.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-granting-access-to-a-subsystem-by-id"></a>A. Conceder acceso a un subsistema mediante el identificador  
 En el siguiente ejemplo se concede acceso al proxy `Catalog application proxy` al subsistema de scripts ActiveX.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_proxy_to_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_id = 2;  
GO  
```  
  
### <a name="b-granting-access-to-a-subsystem-by-name"></a>B. Conceder acceso a un subsistema mediante el nombre.  
 En el siguiente ejemplo se concede acceso al proxy `Catalog application proxy` al subsistema de ejecución de paquetes SSIS.  
  
```sql
USE msdb ;  
GO  
  
EXEC dbo.sp_grant_proxy_to_subsystem  
    @proxy_name = N'Catalog application proxy',  
    @subsystem_name = N'Dts' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Implementar la seguridad de Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_revoke_proxy_from_subsystem &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-revoke-proxy-from-subsystem-transact-sql.md)   
 [sp_add_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)   
 [sp_update_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-update-proxy-transact-sql.md)  
  
  
