---
description: sp_copymergesnapshot (Transact-SQL)
title: sp_copymergesnapshot (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copymergesnapshot
- sp_copymergesnapshot_TSQL
helpviewer_keywords:
- sp_copymergesnapshot
ms.assetid: eaecd6e0-8486-4e5d-ace7-8ae75768c0a8
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 49418b8df94d987b1050591fb6e4cd1d88ddc82d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185793"
---
# <a name="sp_copymergesnapshot-transact-sql"></a>sp_copymergesnapshot (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Copia la carpeta de instantáneas de la publicación especificada en la carpeta indicada en el **\@ destination_folder**. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_copymergesnapshot [ @publication = ] 'publication', [ @destination_folder = ] 'destination_folder'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación cuyo contenido de instantánea se va a copiar. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @destination_folder = ] 'destination_folder'` Es el nombre de la carpeta donde se va a copiar el contenido de la instantánea de la publicación. *destination_folder* es de tipo **nvarchar (255)** y no tiene ningún valor predeterminado. El *destination_folder* puede ser una ubicación alternativa, por ejemplo, en otro servidor, en una unidad de red o en medios extraíbles (por ejemplo, CD-ROMs o discos extraíbles).  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_copymergesnapshot** se utiliza en la replicación de mezcla. Los suscriptores que ejecutan [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] la versión 7,0 y versiones anteriores no pueden usar la ubicación de instantáneas alternativa.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_copymergesnapshot**.  
  
## <a name="see-also"></a>Consulte también  
 [Ubicaciones de carpeta de instantáneas alternativas](../../relational-databases/replication/snapshot-options.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
