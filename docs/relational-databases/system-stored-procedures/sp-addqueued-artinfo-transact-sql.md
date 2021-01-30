---
description: sp_addqueued_artinfo (Transact-SQL)
title: sp_addqueued_artinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_addqueued_artinfo
- sp_addqueued_artinfo_TSQL
helpviewer_keywords:
- sp_addqueued_artinfo
ms.assetid: decdb6eb-3dcd-4053-a21d-fd367c3fbafb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c0899ff490b553471fc766413fdf0cca27955fa0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206662"
---
# <a name="sp_addqueued_artinfo-transact-sql"></a>sp_addqueued_artinfo (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  
  
> [!IMPORTANT]  
>  Se debe utilizar el procedimiento [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) en lugar de **sp_addqueued_artinfo**. [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) genera un script que contiene las llamadas a **sp_addqueued_artinfo** y **sp_addsynctrigger** .  
  
 Crea el [MSsubscription_articles](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md) tabla en el suscriptor que se usa para realizar un seguimiento de la información de suscripción de los artículos (actualización en cola y actualización inmediata con actualización en cola como conmutación por error). Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addqueued_artinfo [ @artid= ] 'artid'  
        , [ @article= ] 'article'  
        , [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @dest_table= ] 'dest_table'  
        , [ @owner = ] 'owner'  
        , [ @cft_table= ] 'cft_table'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @artid = ] 'artid'` Es el nombre del identificador de artículo. *artid especificado* es de **tipo int** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo que se va a incluir en el script. *article* es de **tipo sysname** y no tiene ningún valor predeterminado  
  
`[ @publisher = ] 'publisher'` Es el nombre del servidor del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación que se va a incluir en el script. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @dest_table = ] _'dest_table'` Es el nombre de la tabla de destino. *dest_table* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
 [**@owner =** ] **'**_propietario_**'**  
 Es el propietario de la suscripción. *Owner* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @cft_table = ] 'cft_table'` Nombre de la tabla de conflictos de actualización en cola de este artículo. *cft_table* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 el Agente de distribución usa **sp_addqueued_artinfo** como parte de la inicialización de la suscripción. Los usuarios no suelen ejecutar este procedimiento almacenado, pero puede resultar útil si el usuario debe configurar manualmente una suscripción.  
  
 [sp_script_synctran_commands](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md) en lugar de **sp_addqueued_artinfo**.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_addqueued_artinfo**.  
  
## <a name="see-also"></a>Consulte también  
 [Updatable Subscriptions for Transactional Replication](../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md)   
 [sp_script_synctran_commands &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-script-synctran-commands-transact-sql.md)   
 [MSsubscription_articles &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/mssubscription-articles-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
