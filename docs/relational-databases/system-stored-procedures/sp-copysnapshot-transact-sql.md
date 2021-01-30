---
description: sp_copysnapshot (Transact-SQL)
title: sp_copysnapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysnapshot
- sp_copysnapshot_TSQL
helpviewer_keywords:
- sp_copysnapshot
ms.assetid: a012a32f-6f26-45bf-8046-b51cd7fec455
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 76e8f1bead2aaf10289f724b300a79eacd3ca11f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185772"
---
# <a name="sp_copysnapshot-transact-sql"></a>sp_copysnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Copia la carpeta de instantáneas de la publicación especificada en la carpeta indicada en el **\@ destination_folder**. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación. Este procedimiento almacenado es muy útil para copiar una instantánea en medios extraíbles, como un CD-ROM.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_copysnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder' ]  
    [ , [ @subscriber = ] 'subscriber' ]  
    [ , [ @subscriber_db = ] 'subscriber_db' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación cuyo contenido de instantánea se va a copiar. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @destination_folder = ] 'destination_folder'` Es el nombre de la carpeta donde se va a copiar el contenido de la instantánea de la publicación. *destination_folder* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado. El *destination_folder* puede ser una ubicación alternativa, por ejemplo, en otro servidor, en una unidad de red o en medios extraíbles (por ejemplo, CD-ROMs o discos extraíbles).  
  
`[ @subscriber = ] 'subscriber'` Es el nombre del suscriptor. *Subscriber* es de tipo sysname y su valor predeterminado es NULL.  
  
`[ @subscriber_db = ] 'subscriber_db'` Es el nombre de la base de datos de suscripciones. *subscriber_db* es de tipo sysname y su valor predeterminado es NULL.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_copysnapshot** se utiliza en todos los tipos de replicación. Los suscriptores que ejecutan [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la versión 7,0 y versiones anteriores no pueden usar la ubicación de instantáneas alternativa.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_copysnapshot**.  
  
## <a name="see-also"></a>Consulte también  
 [Ubicaciones de carpeta de instantáneas alternativas](../../relational-databases/replication/snapshot-options.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
