---
description: sp_reinitmergesubscription (Transact-SQL)
title: sp_reinitmergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_reinitmergesubscription_TSQL
- sp_reinitmergesubscription
helpviewer_keywords:
- sp_reinitmergesubscription
ms.assetid: 249a4048-e885-48e0-a92a-6577f59de751
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 5d4a16d71453d08fca6ab496cc103e43edbc160e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185670"
---
# <a name="sp_reinitmergesubscription-transact-sql"></a>sp_reinitmergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Marca una suscripción de mezcla para reinicializarla la próxima vez que se ejecute el Agente de mezcla. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_reinitmergesubscription [ [ @publication = ] 'publication'  
    [ , [ @subscriber = ] 'subscriber'  
    [ , [ @subscriber_db = ] 'subscriber_db'  
    [ , [ @upload_first = ] 'upload_first'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **All**.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname y su** valor predeterminado es **All**.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos del suscriptor. *subscriber_db* es de **tipo sysname y su** valor predeterminado es **All**.  
  
`[ @upload_first = ] 'upload_first'` Indica si se cargan los cambios en el suscriptor antes de que se reinicialice la suscripción. *upload_first* es de tipo **nvarchar (5)** y su valor predeterminado es false. Si **es true**, los cambios se cargan antes de que se reinicialice la suscripción. Si **es false**, los cambios no se cargan.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_reinitmergesubscription** se utiliza en la replicación de mezcla.  
  
 se puede llamar a **sp_reinitmergesubscription** desde el publicador para reinicializar las suscripciones de mezcla. También se recomienda volver a ejecutar el Agente de instantáneas.  
  
 Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
## <a name="examples"></a>Ejemplos  

### <a name="a-reinitialize-the-push-subscription-and-lose-pending-changes"></a>A. Reinicializar la suscripción de extracción y perder los cambios pendientes

 [!code-sql[HowTo#sp_reinitmergepushsub](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_1.sql)]  
  
### <a name="b-reinitialize-the-push-subscription-and-upload-pending-changes"></a>B. Reinicializar la suscripción de extracción y cargar los cambios pendientes
 [!code-sql[HowTo#sp_reinitmergepushsubwithupload](../../relational-databases/replication/codesnippet/tsql/sp-reinitmergesubscripti_2.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_reinitmergesubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Reinicializar suscripciones](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
