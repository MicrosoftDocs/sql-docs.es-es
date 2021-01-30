---
title: sp_dropsubscription (Transact-SQL) | Microsoft Docs
description: Quita las suscripciones a un artículo, publicación o suscripción en el publicador. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropsubscription
- sp_dropsubscription_TSQL
helpviewer_keywords:
- sp_dropsubscription
ms.assetid: 7551f345-5510-4684-ab53-f9057249d13a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 3097c61204e0a9ae310af87fb83901a252c50e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157948"
---
# <a name="sp_dropsubscription-transact-sql"></a>sp_dropsubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Quita las suscripciones a un artículo, publicación o conjunto de suscripciones específico del publicador. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropsubscription [ [ @publication= ] 'publication' ]  
    [ , [ @article= ] 'article' ]  
        , [ @subscriber= ] 'subscriber'  
    [ , [ @destination_db= ] 'destination_db' ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @reserved= ] 'reserved' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación asociada. *Publication* es de **tipo sysname y su** valor predeterminado es NULL. Si es **All**, se cancelan todas las suscripciones de todas las publicaciones del suscriptor especificado. la *publicación* es un parámetro necesario.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname y su** valor predeterminado es NULL. Si es **All**, se quitan las suscripciones a todos los artículos de cada publicación y suscriptor especificados. Usar **todo para las** publicaciones que permiten la actualización inmediata.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor al que se quitarán sus suscripciones. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado. Si es **All**, se quitan todas las suscripciones de todos los suscriptores.  
  
`[ @destination_db = ] 'destination_db'` Es el nombre de la base de datos de destino. *destination_db* es de **tipo sysname y su** valor predeterminado es NULL. Cuando es NULL, se quitan todas las suscripciones del suscriptor indicado.  
  
`[ @ignore_distributor = ] ignore_distributor`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
`[ @reserved = ] 'reserved'`  
 [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropsubscription** se utiliza en la replicación de instantáneas y transaccional.  
  
 Si quita la suscripción a un artículo de una publicación de sincronización inmediata, no podrá volverla a agregar, a menos que quite las suscripciones a todos los artículos de la publicación y las agregue de nuevo todas a la vez.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/sp-dropsubscription-tran_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , el rol fijo de base de datos **db_owner** o el usuario que creó la suscripción pueden ejecutar **sp_dropsubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-push-subscription.md)   
 [sp_addsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsubscription-transact-sql.md)   
 [sp_changesubstatus &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changesubstatus-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)  
  
  
