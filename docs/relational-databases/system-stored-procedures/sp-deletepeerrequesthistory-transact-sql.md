---
description: sp_deletepeerrequesthistory (Transact-SQL)
title: sp_deletepeerrequesthistory (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_deletepeerrequesthistory
- sp_deletepeerrequesthistory_TSQL
helpviewer_keywords:
- sp_deletepeerrequesthistory
ms.assetid: 63a4ec6e-ce79-4bf1-9d37-5ac88f8d6beb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 56dad6791177093750585122eb939ba5a4ca5763
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189616"
---
# <a name="sp_deletepeerrequesthistory-transact-sql"></a>sp_deletepeerrequesthistory (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Elimina el historial relacionado con una solicitud de estado de publicación, que incluye el historial de solicitudes ([MSpeer_request &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/mspeer-request-transact-sql.md)), así como el historial de respuestas ([MSpeer_response &#40;transact-SQL&#41;](../../relational-databases/system-tables/mspeer-response-transact-sql.md)). Este procedimiento almacenado se ejecuta en la base de datos de publicación en un publicador que participa en una topología de replicación punto a punto. Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_deletepeerrequesthistory [ @publication = ] 'publication'  
    [ , [ @request_id = ] request_id ]  
    [ , [ @cutoff_date = ] cutoff_date ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Nombre de la publicación para la que se realizó la solicitud de estado. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @request_id = ] request_id` Especifica una solicitud de estado individual para que se eliminen todas las respuestas a esta solicitud. *request_id* es de **tipo int** y su valor predeterminado es NULL.  
  
`[ @cutoff_date = ] cutoff_date` Especifica una fecha límite, antes de la cual se eliminan todos los registros de respuesta anteriores. *cutoff_date* es de **tipo DateTime** y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_deletepeerrequesthistory** se usa en una topología de replicación transaccional punto a punto. Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 Al ejecutar **sp_deletepeerrequesthistory**, se debe especificar *request_id* o *cutoff_date* .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_deletepeerrequesthistory**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_helppeerrequests &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppeerrequests-transact-sql.md)   
 [sp_helppeerresponses &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppeerresponses-transact-sql.md)   
 [sp_requestpeerresponse &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md)  
  
  
