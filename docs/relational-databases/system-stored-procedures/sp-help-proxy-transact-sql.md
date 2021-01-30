---
description: sp_help_proxy (Transact-SQL)
title: sp_help_proxy (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_help_proxy
- sp_help_proxy_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_help_proxy
ms.assetid: a2fce164-2b64-40c2-8f35-6eeb7844abf1
author: markingmyname
ms.author: maghan
ms.openlocfilehash: a598d3fef67af2e2d9f4874b9e8f63770b762bb7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199614"
---
# <a name="sp_help_proxy-transact-sql"></a>sp_help_proxy (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra información de uno o varios servidores proxy.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_help_proxy   
    [ @proxy_id = ] id,  
    [ @proxy_name = ] 'proxy_name' ,  
    [ @subsystem_name = ] 'subsystem_name' ,  
    [ @name = ] 'name'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @proxy_id = ] id` Número de identificación del proxy del que se va a mostrar información. La *proxy_id* es de **tipo int** y su valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
`[ @proxy_name = ] 'proxy_name'` Nombre del proxy del que se va a mostrar información. La *proxy_name* es de **tipo sysname y su** valor predeterminado es NULL. Se puede especificar el *identificador* o el *proxy_name* .  
  
`[ @subsystem_name = ] 'subsystem_name'` Nombre del subsistema para el que se van a enumerar los servidores proxy. La *subsystem_name* es de **tipo sysname y su** valor predeterminado es NULL. Cuando se especifica *subsystem_name* , también se debe especificar *Name* .  
  
 En la tabla siguiente se muestran los valores disponibles para cada subsistema.  
  
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
|Dts|Ejecución de paquetes SSIS|  
|PowerShell|Script de PowerShell|  
  
`[ @name = ] 'name'` Nombre de un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión para el que se van a enumerar los servidores proxy. El nombre es **nvarchar (256)** y su valor predeterminado es NULL. Cuando se especifica *Name* , también se debe especificar *subsystem_name* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**proxy_id**|**int**|Número de identificación del proxy.|  
|**name**|**sysname**|Nombre del proxy.|  
|**credential_identity**|**sysname**|Nombre de dominio y nombre de usuario de Microsoft Windows para la credencial asociada con el proxy.|  
|**enabled**|**tinyint**|Si el proxy está habilitado. { **0** = no habilitado, **1** = habilitado}|  
|**description**|**nvarchar(1024)**|Descripción de este proxy.|  
|**user_sid**|**varbinary(85)**|Id. de seguridad del usuario de Windows para este proxy.|  
|**credential_id**|**int**|Identificador de la credencial asociada con este proxy.|  
|**credential_identity_exists**|**int**|Si existe credential_identity. { 0 = no existe, 1 = existe }|  
  
## <a name="remarks"></a>Observaciones  
 Cuando no se proporcionan parámetros, **sp_help_proxy** muestra información de todos los servidores proxy de la instancia.  
  
 Para determinar qué servidores proxy puede usar un inicio de sesión para un subsistema determinado, especifique *el nombre y el* *subsystem_name*. Cuando se proporcionan estos argumentos, **sp_help_proxy** muestra los servidores proxy a los que puede tener acceso el inicio de sesión especificado y que se pueden usar para el subsistema especificado.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, los miembros del rol fijo de servidor **sysadmin** pueden ejecutar este procedimiento almacenado. A otros usuarios debe concederse el rol fijo de base de datos **SQLAgentOperatorRole** en la base de datos **msdb** .  
  
 Para obtener más información sobre **SQLAgentOperatorRole**, vea [Agente SQL Server roles fijos de base de datos](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
> [!NOTE]  
>  Las columnas **credential_identity** y **user_sid** solo se devuelven en el conjunto de resultados cuando los miembros de **sysadmin** ejecutan este procedimiento almacenado.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-information-for-all-proxies"></a>A. Mostrar información de todos los servidores proxy  
 El ejemplo siguiente muestra información de todos los servidores proxy de la instancia.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy ;  
GO  
```  
  
### <a name="b-listing-information-for-a-specific-proxy"></a>B. Mostrar información de un proxy específico  
 El ejemplo siguiente muestra información del proxy denominado `Catalog application proxy`.  
  
```  
USE msdb ;  
GO  
  
EXEC dbo.sp_help_proxy  
    @proxy_name = N'Catalog application proxy' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Agente SQL Server procedimientos almacenados &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sql-server-agent-stored-procedures-transact-sql.md)   
 [sp_add_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)   
 [sp_delete_proxy &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md)  
  
  
