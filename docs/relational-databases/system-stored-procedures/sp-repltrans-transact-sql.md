---
description: sp_repltrans (Transact-SQL)
title: sp_repltrans (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_repltrans_TSQL
- sp_repltrans
helpviewer_keywords:
- sp_repltrans
ms.assetid: 738e2322-335b-44fa-820e-f31c02743978
author: markingmyname
ms.author: maghan
ms.openlocfilehash: c408bc1563046bb300e7dd862074b87b59edbee2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211623"
---
# <a name="sp_repltrans-transact-sql"></a>sp_repltrans (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve un conjunto de resultados con todas las transacciones del registro de transacciones de la base de datos de publicaciones que están marcadas para replicación pero que no se han marcado como distribuidas. Este procedimiento almacenado se ejecuta en el publicador de una base de datos de publicaciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_repltrans  
```  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_repltrans** devuelve información acerca de la base de datos de publicación desde la que se ejecuta, lo que le permite ver las transacciones que no se distribuyen actualmente (aquellas transacciones que permanecen en el registro de transacciones que no se han enviado al distribuidor). El conjunto de resultados muestra los números de secuencia del registro relativos al primer y último registro de cada transacción. **sp_repltrans** es similar a [sp_replcmds &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) pero no devuelve los comandos de las transacciones.  
  
## <a name="remarks"></a>Observaciones  
 **sp_repltrans** se utiliza en la replicación transaccional.  
  
 **sp_repltrans** no se admite para publicadores que no son de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_repltrans**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_repldone &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-repldone-transact-sql.md)   
 [sp_replflush &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
