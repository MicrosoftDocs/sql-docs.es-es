---
description: sp_helppeerresponses (Transact-SQL)
title: sp_helppeerresponses (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helppeerresponses_TSQL
- sp_helppeerresponses
helpviewer_keywords:
- sp_helppeerresponses
ms.assetid: e55789d1-43fb-4a37-9e5e-60ccef122a5d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 969c0bfc0a1c9ef0dcd12feb077e8fec6ae19790
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99171735"
---
# <a name="sp_helppeerresponses-transact-sql"></a>sp_helppeerresponses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve todas las respuestas a una solicitud de estado específica recibida de un participante en una topología de replicación punto a punto, donde la solicitud se inició ejecutando [sp_helppeerrequests](../../relational-databases/system-stored-procedures/sp-requestpeerresponse-transact-sql.md) en cualquier base de datos Publicada en la topología. Este procedimiento almacenado se ejecuta en la base de datos de publicación, en un publicador que participa en una topología de replicación punto a punto. Para obtener más información, consulte [Peer-to-Peer Transactional Replication](../../relational-databases/replication/transactional/peer-to-peer-transactional-replication.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helppeerresponses [ @request_id = ] request_id  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @request_id = ] request_id` Es el identificador de una solicitud de estado específica. *request_id* es de **tipo int** y no tiene ningún valor predeterminado.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**id_de_solicitud**|**int**|Id. de la solicitud de estado.|  
|**personas**|**sysname**|Nombre del elemento del mismo nivel que generó la respuesta.|  
|**peer_db**|**sysname**|Nombre de la base de datos del mismo nivel que generó la respuesta.|  
|**received_date**|**datetime**|Fecha y hora cuando el solicitante recibió la respuesta del elemento del mismo nivel que la envió.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helppeerresponses** se usa en la replicación transaccional punto a punto.  
  
 **sp_helppeerresponses** procedimiento se utiliza al restaurar una base de datos Publicada en una topología punto a punto.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helppeerresponses**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_deletepeerrequesthistory &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-deletepeerrequesthistory-transact-sql.md)   
 [sp_helppeerrequests &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppeerrequests-transact-sql.md)  
  
  
