---
description: MSSQL_ENG014005
title: MSSQL_ENG014005 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014005 error
ms.assetid: f168f0d6-cb11-45d4-9781-c374d7f388ee
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 741ba12abae8aefc58e5dc9f8a8c3c11320f0891
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198940"
---
# <a name="mssql_eng014005"></a>MSSQL_ENG014005
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14005|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|No se pudo quitar la publicación. Hay una suscripción para ella.|  
  
## <a name="explanation"></a>Explicación  
 Ha intentado quitar una publicación que tiene asociadas una o varias suscripciones. Una publicación se puede quitar únicamente si no hay suscripciones asociadas.  
  
## <a name="user-action"></a>Acción del usuario  
 Quite las suscripciones antes de quitar la publicación. Si utiliza [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para quitar la publicación, se le ofrecerá la opción de quitar automáticamente todas las suscripciones asociadas antes de quitar la publicación. Si utiliza procedimientos almacenados, primero debe quitar explícitamente las suscripciones. Para obtener más información, consulte [Delete a Push Subscription](../../relational-databases/replication/delete-a-push-subscription.md) y [Delete a Pull Subscription](../../relational-databases/replication/delete-a-pull-subscription.md).  
  
 Si parece no existir ninguna suscripción para la publicación o si ve este error cuando crea la publicación, es posible que tenga una suscripción anterior que no se limpiara completamente cuando se quitó. Ejecute [sp_removedbreplication &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) en la base de datos para eliminar todos los objetos y la configuración completa relacionados con la replicación.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
