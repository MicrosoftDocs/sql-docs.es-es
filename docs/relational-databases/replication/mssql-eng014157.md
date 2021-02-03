---
description: MSSQL_ENG014157
title: MSSQL_ENG014157 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014157 error
ms.assetid: 1a0890cf-d977-43e0-a2ba-9c5ff1a8f856
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 49be4e6ae7cbea651b5565dfd1e6c22afe4ad063
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187068"
---
# <a name="mssql_eng014157"></a>MSSQL_ENG014157
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14157|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|La suscripción creada por el suscriptor '%1!s!' a la publicación '%2!s!' expiró y ha sido eliminada.|  
  
## <a name="explanation"></a>Explicación  
 Un suscriptor debe sincronizarse con el publicador en el tiempo especificado en el período de conservación de la publicación. Si un suscriptor no se sincroniza en este período, la suscripción expira. Para más información, consulte [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="user-action"></a>Acción del usuario  
 La suscripción se debe volver a crear e inicializar para que el suscriptor pueda empezar a recibir cambios de datos de nuevo:  
  
-   Para obtener información sobre la creación de suscripciones, vea [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md).  
  
-   Para obtener información sobre la inicialización de suscripciones, vea [Initialize a Subscription](../../relational-databases/replication/initialize-a-subscription.md) (Inicializar una suscripción).  
  
 Si la base de datos de suscripciones contiene varios cambios que no se han sincronizado con el publicador, puede usar la [tablediff Utility](../../tools/tablediff-utility.md) para determinar qué filas de las bases de datos de publicaciones y suscripciones son diferentes.  
  
 Puede aumentar el período de conservación de la publicación para evitar tener suscripciones expiradas. Tenga cuidado al establecer un valor alto porque puede provocar que se almacenen más datos y metadatos, lo que afectaría al rendimiento. Para más información, consulte [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md).  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
