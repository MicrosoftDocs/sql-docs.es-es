---
description: sp_validatemergesubscription (Transact-SQL)
title: sp_validatemergesubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_validatemergesubscription
- sp_validatemergesubscription_TSQL
helpviewer_keywords:
- sp_validatemergesubscription
ms.assetid: d73ad03c-e5b3-4606-a0ee-7d75e12762a6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 15ebc99d11b0ff009d80f769ea9fbfad60f11c76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201813"
---
# <a name="sp_validatemergesubscription-transact-sql"></a>sp_validatemergesubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Realiza una validación de la suscripción especificada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_validatemergesubscription [@publication=] 'publication'  
        , [ @subscriber = ] 'subscriber'  
        , [ @subscriber_db = ] 'subscriber_db'  
        , [ @level = ] level  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @level = ] 'level'` Es el tipo de validación que se va a realizar. *LEVEL* es de **tinyint** y no tiene ningún valor predeterminado. Puede ser uno de estos valores.  
  
|Valor de nivel|Descripción|  
|-----------------|-----------------|  
|**1**|Validación solo del recuento de filas|  
|**2**|Validación del recuento de filas y de la suma de comprobación.|  
|**3**|Validación del recuento de filas y de la suma de comprobación binaria.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_validatemergesubscription** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_validatemergesubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)   
 [Validar datos replicados](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [sp_validatemergepublication &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-validatemergepublication-transact-sql.md)  
  
  
