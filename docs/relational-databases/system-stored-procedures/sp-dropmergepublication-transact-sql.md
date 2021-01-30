---
description: sp_dropmergepublication (Transact-SQL)
title: sp_dropmergepublication (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_dropmergepublication
- sp_dropmergepublication_TSQL
helpviewer_keywords:
- sp_dropmergepublication
ms.assetid: 9e1cb96e-5889-4f97-88cd-f60cf313ce68
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 35024724255eea714de42f8fb3a974188730fcef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208214"
---
# <a name="sp_dropmergepublication-transact-sql"></a>sp_dropmergepublication (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita una publicación de combinación y su Agente de instantáneas asociado. Antes de quitar una publicación de combinación, es necesario quitar todas las suscripciones. Los artículos de la publicación se quitan automáticamente. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropmergepublication [ @publication= ] 'publication'   
    [ , [ @ignore_distributor = ] ignore_distributor ]   
    [ , [ @reserved = ] reserved ]  
    [ , [ @ignore_merge_metadata = ] ignore_merge_metadata ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que se va a quitar. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. Si es **All**, se quitan todas las publicaciones de combinación existentes, así como el agente de instantáneas trabajo asociado a ellas. Si especifica un valor concreto para la *publicación*, solo se quitarán esa publicación y su trabajo de agente de instantáneas asociado.  
  
`[ @ignore_distributor = ] ignore_distributor` Se utiliza para quitar una publicación sin realizar tareas de limpieza en el distribuidor. *ignore_distributor* es de **bit** y su valor predeterminado es **0**. Este parámetro también se utiliza al volver a instalar el distribuidor.  
  
`[ @reserved = ] reserved` Está reservado para uso futuro. *Reserved* es de **bit** y su valor predeterminado es **0**.  
  
`[ @ignore_merge_metadata = ] ignore_merge_metadata` Solo para uso interno.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropmergepublication** se utiliza en la replicación de mezcla.  
  
 **sp_dropmergepublication** quita recursivamente todos los artículos asociados a una publicación y, a continuación, quita la propia publicación. No se puede quitar una publicación si tiene una o más suscripciones. Para obtener información acerca de cómo quitar suscripciones, consulte [eliminar una suscripción de inserción](../../relational-databases/replication/delete-a-push-subscription.md) y [eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 La ejecución de **sp_dropmergepublication** para quitar una publicación no quita los objetos publicados de la base de datos de publicación ni los objetos correspondientes de la base de datos de suscripciones. Utilice DROP \<object> para quitar estos objetos manualmente si es necesario.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_dropmergepublication](../../relational-databases/replication/codesnippet/tsql/sp-dropmergepublication-_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_dropmergepublication**.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una publicación](../../relational-databases/replication/publish/delete-a-publication.md)   
 [sp_addmergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md)   
 [sp_changemergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md)   
 [sp_helpmergepublication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
