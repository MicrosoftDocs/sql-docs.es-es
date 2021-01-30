---
description: sp_revoke_proxy_from_subsystem (Transact-SQL)
title: sp_revoke_proxy_from_subsystem (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revoke_login_from_subsystem
- sp_revoke_login_from_subsystem_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revoke_proxy_from_subsystem
ms.assetid: b87bc8ba-3ea8-4aed-b54b-32c3d82d9d2a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 9457a1d9dd0d9fd01a7418cd56e772902d593b6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211862"
---
# <a name="sp_revoke_proxy_from_subsystem-transact-sql"></a>sp_revoke_proxy_from_subsystem (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Revoca el acceso a un subsistema desde un proxy.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_revoke_proxy_from_subsystem   
    [ @proxy_id = ] proxy_id,  
    [ @proxy_name = ] 'proxy_name',  
    [ @subsystem_id = ] subsystem_id,  
    [ @subsystem_name = ] 'subsystem_name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_id = ] id` Número de identificación del proxy del que se va a revocar el acceso. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *proxy_id* o *proxy_name* , pero no se pueden especificar ambos.  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy del que se va a revocar el acceso. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *proxy_id* o *proxy_name* , pero no se pueden especificar ambos.  
  
`[ @subsystem_id = ] id` Número de ID. del subsistema al que se va a revocar el acceso. La *subsystem_id* es de **tipo int** y su valor predeterminado es NULL. Se debe especificar *subsystem_id* o *subsystem_name* , pero no se pueden especificar ambos. En la tabla siguiente se muestran los valores disponibles para cada subsistema.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**2**|Script ActiveX<br /><br /> Importante el subsistema de scripts ActiveX se quitará del agente en una versión futura de **\* . \* \* \*** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Evite utilizar esta característica en nuevos trabajos de desarrollo y tenga previsto modificar las aplicaciones que actualmente la utilizan.|  
|**3**|Sistema operativo (CmdExec)|  
|**4**|Agente de instantáneas de replicación|  
|**5**|Agente de registro del LOG de replicación|  
|**6**|Agente de distribución de replicación|  
|**7**|Replication Merge Agent|  
|**8**|Agente de lectura de cola de replicación|  
|**9**|Comando de Analysis Services|  
|**10**|Consulta de Analysis Services|  
|**11**|[!INCLUDE[ssIS](../../includes/ssis-md.md)] ejecución de paquetes|  
|**12**|Script de PowerShell|  
  
`[ @subsystem_name = ] 'subsystem_name'` Nombre del subsistema al que se va a revocar el acceso. La *subsystem_name* es de **tipo sysname y su** valor predeterminado es NULL. Se debe especificar *subsystem_id* o *subsystem_name* , pero no se pueden especificar ambos. En la tabla siguiente se muestran los valores disponibles para cada subsistema.  
  
|Value|Descripción|  
|-----------|-----------------|  
|ActiveScripting|Script ActiveX|  
|CmdExec|Sistema operativo (CmdExec)|  
|Instantánea|Agente de instantáneas de replicación|  
|LogReader|Agente de registro del LOG de replicación|  
|Distribución|Agente de distribución de replicación|  
|Merge|Replication Merge Agent|  
|QueueReader|Agente de lectura de cola de replicación|  
|ANALYSISQUERY|Comando de Analysis Services|  
|ANALYSISCOMMAND|Consulta de Analysis Services|  
|Dts|[!INCLUDE[ssIS](../../includes/ssis-md.md)] ejecución de paquetes|  
|PowerShell|Script de PowerShell|  
  
## <a name="remarks"></a>Observaciones  
 Al revocar el acceso a un subsistema no se cambian los permisos de la entidad de seguridad especificada en el proxy.  
  
> [!NOTE]  
>  Para determinar qué pasos de trabajo hacen referencia a un proxy, haga clic con el botón secundario en el nodo **servidores proxy** en **Agente SQL Server** de Microsoft [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y, a continuación, haga clic en **propiedades**. En el cuadro de diálogo **propiedades de cuenta de proxy** , seleccione la página **referencias** para ver todos los pasos de trabajo que hacen referencia a este proxy.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_revoke_proxy_from_subsystem**.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se revoca el acceso al subsistema de [!INCLUDE[ssIS](../../includes/ssis-md.md)] para el proxy `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_revoke_proxy_from_subsystem  
    @proxy_name = 'Catalog application proxy',  
    @subsystem_name = N'Dts';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [Implementar la seguridad de Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)   
 [sp_grant_proxy_to_subsystem &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
  
  
