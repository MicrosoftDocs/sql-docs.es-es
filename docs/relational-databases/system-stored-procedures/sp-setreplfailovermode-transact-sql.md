---
description: sp_setreplfailovermode (Transact-SQL)
title: sp_setreplfailovermode (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_setreplfailovermode
- sp_setreplfailovermode_TSQL
helpviewer_keywords:
- sp_setreplfailovermode
ms.assetid: ca98a4c3-bea4-4130-88d7-79e0fd1e85f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 216f76723d0c7d00461b5fd299bee01c87e99388
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176413"
---
# <a name="sp_setreplfailovermode-transact-sql"></a>sp_setreplfailovermode (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Permite definir el modo de funcionamiento de conmutación por error para las suscripciones habilitadas para actualización inmediata con actualización en cola como conmutación por error. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones. Para obtener más información sobre los modos de conmutación por error, consulte [suscripciones actualizables para la replicación transaccional](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_setreplfailovermode [ @publisher= ] 'publisher'  
    [ , [ @publisher_db = ] 'publisher_db' ]  
    [ , [ @publication= ] 'publication' ]  
    [ , [ @failover_mode= ] 'failover_mode' ]  
    [ , [ @override = ] override ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado. La publicación debe existir.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @failover_mode = ] 'failover_mode'` Es el modo de conmutación por error de la suscripción. *failover_mode* es **nvarchar (10)** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**inmediato** o **sincronizar**|A medida que se vayan modificando los datos en el suscriptor, se realiza una copia masiva de las modificaciones en el publicador.|  
|**en cola**|Las modificaciones de datos se almacenan en una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cola.|  
  
> [!NOTE]  
>  [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server ha quedado desusado y ya no se admite.  
  
`[ @override = ] override` Solo para uso interno.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_setreplfailovermode** se utiliza en la replicación de instantáneas o en la replicación transaccional para la que se habilitan las suscripciones, ya sea para la actualización en cola con conmutación por error a actualización inmediata o para una actualización inmediata con conmutación por error a la actualización en cola.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_setreplfailovermode**.  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar entre modos de actualización para una suscripción transaccional actualizable](../../relational-databases/replication/administration/switch-between-update-modes-for-an-updatable-transactional-subscription.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
