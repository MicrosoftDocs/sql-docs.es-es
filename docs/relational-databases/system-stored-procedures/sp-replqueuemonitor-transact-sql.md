---
description: sp_replqueuemonitor (Transact-SQL)
title: sp_replqueuemonitor (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_replqueuemonitor
- sp_replqueuemonitor_TSQL
helpviewer_keywords:
- sp_replqueuemonitor
ms.assetid: 6909a3f1-43a2-4df5-a6a5-9e6f347ac841
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 8fa6635b4f31692e5f45e848043e44496e2d702f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211668"
---
# <a name="sp_replqueuemonitor-transact-sql"></a>sp_replqueuemonitor (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Enumera los mensajes de cola de una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cola o [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queue Server para las suscripciones de actualización en cola a una publicación especificada. Si se utilizan colas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones. Si se utiliza Message Queue Server, este procedimiento almacenado se ejecuta en el distribuidor de la base de datos de distribución.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_replqueuemonitor [ @publisher = ] 'publisher'  
    [ , [ @publisherdb = ] 'publisher_db' ]  
    [ , [ @publication = ] 'publication' ]  
    [ , [ @tranid = ] 'tranid' ]  
    [ , [ @queuetype = ] 'queuetype' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es NULL. El servidor debe estar configurado para publicación. Su valor es NULL para todos los publicadores.  
  
`[ @publisherdb = ] 'publisher_db' ]` Es el nombre de la base de datos de publicación. *publisher_db* es de **tipo sysname y su** valor predeterminado es NULL. Su valor es NULL para todas las bases de datos de publicaciones.  
  
`[ @publication = ] 'publication' ]` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es NULL. Su valor es NULL para todas las publicaciones.  
  
`[ @tranid = ] 'tranid' ]` Es el identificador de la transacción. *tranid* es de **tipo sysname y su** valor predeterminado es NULL. Su valor es NULL para todas las transacciones.  
  
`[ @queuetype = ] 'queuetype' ]` Es el tipo de cola que almacena las transacciones. *queuetype* es de **tinyint** con un valor predeterminado de **0** y puede tener uno de estos valores.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Todos los tipos de colas|  
|**1**|Message Queue Server|  
|**2**|Cola de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_replqueuemonitor** se utiliza en la replicación de instantáneas o en la replicación transaccional con suscripciones de actualización en cola. No muestra los mensajes de la cola que no contienen comandos SQL o que forman parte de un comando SQL múltiple.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_replqueuemonitor**.  
  
## <a name="see-also"></a>Consulte también  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
