---
description: sp_droppullsubscription (Transact-SQL)
title: sp_droppullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_droppullsubscription
- sp_droppullsubscription_TSQL
helpviewer_keywords:
- sp_droppullsubscription
ms.assetid: 7352d94a-f8f2-42ea-aaf1-d08c3b5a0e76
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 699d23141e060ee26f96043b44864621a84217c8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158103"
---
# <a name="sp_droppullsubscription-transact-sql"></a>sp_droppullsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita una suscripción de la base de datos actual del suscriptor. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones de extracción.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_droppullsubscription [ @publisher= ] 'publisher'  
        , [ @publisher_db= ] 'publisher_db'  
        , [ @publication= ] 'publication'  
    [ , [ @reserved= ] reserved ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del servidor remoto. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado. Si es **All**, la suscripción se quita de todos los publicadores.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado. **All** significa todas las bases de datos del publicador.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. Si es **All**, la suscripción se coloca en todas las publicaciones.  
  
`[ @reserved = ] reserved` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_droppullsubscription** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 **sp_droppullsubscription** elimina la fila correspondiente en el [MSreplication_subscriptions &#40;tabla de&#41;de Transact-SQL](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md) y el agente de distribuidor correspondiente en el suscriptor. Si no se deja ninguna fila en [MSreplication_subscriptions &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/msreplication-subscriptions-transact-sql.md), se quita la tabla.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_droptranpullsubscription](../../relational-databases/replication/codesnippet/tsql/sp-droppullsubscription-_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o el usuario que creó la suscripción de extracción pueden ejecutar **sp_droppullsubscription**. El rol fijo de base de datos **db_owner** solo se puede ejecutar **sp_droppullsubscription** si el usuario que creó la suscripción de extracción pertenece a este rol.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addpullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_change_subscription_properties &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_dropsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md)  
  
  
