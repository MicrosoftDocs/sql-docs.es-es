---
description: sp_dropsubscriber (Transact-SQL)
title: sp_dropsubscriber (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscriber_TSQL
- sp_dropsubscriber
helpviewer_keywords:
- sp_dropsubscriber
ms.assetid: 8c6eb282-81b5-4ec4-b691-aa061d9267dc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 96ba954cf4674c370b33e783c8f71dd4da69277b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187001"
---
# <a name="sp_dropsubscriber-transact-sql"></a>sp_dropsubscriber (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita la designación de suscriptor de un servidor registrado. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
> [!IMPORTANT]  
>  Este procedimiento almacenado ha quedado desusado. Ya no es necesario registrar de forma explícita un suscriptor en el publicador.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropsubscriber [ @subscriber= ] 'subscriber'   
    [ , [ @reserved= ] 'reserved' ]   
    [ , [ @ignore_distributor = ] ignore_distributor ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor que se va a quitar. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @reserved = ] 'reserved'` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @ignore_distributor = ] ignore_distributor` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropsubscriber** se utiliza en todos los tipos de replicación.  
  
 Este procedimiento almacenado quita la opción Server **Sub** y quita la asignación de inicio de sesión remota del administrador del sistema a **repl_subscriber**.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_dropsubscriber**.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-push-subscription.md)   
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addsubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscriber-transact-sql.md)   
 [sp_changesubscriber &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md)   
 [sp_helpdistributor &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpdistributor-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_helpsubscriberinfo &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
