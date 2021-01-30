---
description: sp_dropmergesubscription (Transact-SQL)
title: sp_dropmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergesubscription_TSQL
- sp_dropmergesubscription
helpviewer_keywords:
- sp_dropmergesubscription
ms.assetid: 34244ae6-bd98-4a6a-bbd3-85f50edfcdc0
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 29cf6541b12f49194c1d1a6562d3fb80975a5591
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204794"
---
# <a name="sp_dropmergesubscription-transact-sql"></a>sp_dropmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una suscripción a una publicación de combinación y su Agente de mezcla asociado. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropmergesubscription [ [ @publication= ] 'publication' ]   
    [ , [ @subscriber= ] 'subscriber'    
    [ , [ @subscriber_db= ] 'subscriber_db' ]   
    [ , [ @subscription_type= ] 'subscription_type' ]   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es NULL. La publicación debe existir y debe cumplir las normas de los identificadores.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscription_database* es de **tipo sysname y su** valor predeterminado es NULL.  
  
`[ @subscription_type = ] 'subscription_type'` Es el tipo de suscripción. *subscription_type* es **nvarchar (15)** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**todos**|Suscripciones de inserción, de extracción y anónimas.|  
|**Anonymous**|Suscripción anónima.|  
|**push**|Suscripción de inserción.|  
|**obtener**|Suscripción de extracción.|  
|**both** (valor predeterminado)|Suscripción de inserción y de extracción.|  
  
`[ @ignore_distributor = ] ignore_distributor` Indica si este procedimiento almacenado se ejecuta sin conectarse al distribuidor. *ignore_distributor* es de **bit** y su valor predeterminado es **0**. Este parámetro se puede utilizar para quitar una suscripción sin tener que realizar tareas de limpieza en el distribuidor. También es útil cuando ha sido necesario volver a instalar el distribuidor.  
  
`[ @reserved = ] reserved` Está reservado para uso futuro. *Reserved* es de **bit** y su valor predeterminado es **0**.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropmergesubscription** se utiliza en la replicación de mezcla.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropmergesubscription_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_dropmergesubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-push-subscription.md)   
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-pull-subscription.md)   
 [sp_addmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md)   
 [sp_changemergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md)   
 [sp_helpmergesubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md)  
  
  
