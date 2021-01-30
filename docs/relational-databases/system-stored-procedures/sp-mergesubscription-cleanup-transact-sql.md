---
description: sp_mergesubscription_cleanup (Transact-SQL)
title: sp_mergesubscription_cleanup (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergesubscription_cleanup
- sp_mergesubscription_cleanup_TSQL
helpviewer_keywords:
- sp_mergesubscription_cleanup
ms.assetid: bfad414f-2bda-4bf5-9507-56a1e743dfc4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cafbf58ca892d052c396dd5638536d0fadae7106
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183596"
---
# <a name="sp_mergesubscription_cleanup-transact-sql"></a>sp_mergesubscription_cleanup (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita los metadatos, como los desencadenadores y las entradas, en **sysmergesubscriptions** y **sysmergearticles** después de quitar la suscripción de extracción de mezcla especificada en el publicador. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
> [!NOTE]  
>  En una suscripción de extracción, los metadatos se quitan cuando se ejecuta [sp_dropmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_mergesubscription_cleanup [ @publisher =] 'publisher'  
        , [ @publisher_db =] 'publisher_db'  
        , [ @publication =] 'publication'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_mergesubscription_cleanup** se utiliza en la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_mergesubscription_cleanup**.  
  
## <a name="see-also"></a>Consulte también  
 [Eliminar una suscripción de extracción](../../relational-databases/replication/delete-a-push-subscription.md)   
 [sp_expired_subscription_cleanup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-expired-subscription-cleanup-transact-sql.md)   
 [sp_subscription_cleanup &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
