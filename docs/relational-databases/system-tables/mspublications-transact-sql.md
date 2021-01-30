---
description: MSpublications (Transact-SQL)
title: MSpublications (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSpublications
- MSpublications_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSpublications system table
ms.assetid: 7a0b3457-7265-4f24-a255-7f055d908f20
author: cawrites
ms.author: chadam
ms.openlocfilehash: 9f4f4713dcf9266c8ba3713f0c1dd29a890f3d13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210437"
---
# <a name="mspublications-transact-sql"></a>MSpublications (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSpublications** contiene una fila por cada publicación replicada por un publicador. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher_id**|**smallint**|IDENTIFICADOR del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publication_id**|**int**|Id. de la publicación.|  
|**publication_type**|**int**|Tipo de publicación:<br /><br /> **0** = transaccional.<br /><br /> **1** = instantánea.<br /><br /> **2** = fusionar mediante combinación.|  
|**thirdparty_flag**|**bit**|Indica si una publicación es una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de datos:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br /> **1** = origen de datos distinto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**independent_agent**|**bit**|Indica si hay un agente de distribución independiente para esta publicación.|  
|**immediate_sync**|**bit**|Indica si los archivos de sincronización se crean o se vuelven a crear cada vez que se ejecuta el Agente de instantáneas.|  
|**allow_push**|**bit**|Indica si es posible crear suscripciones de inserción para la publicación indicada.|  
|**allow_pull**|**bit**|Indica si es posible crear suscripciones de extracción para la publicación indicada.|  
|**allow_anonymous**|**bit**|Indica si es posible crear suscripciones anónimas para la publicación indicada.|  
|**description**|**nvarchar(255)**|Descripción de la publicación.|  
|**vendor_name**|**nvarchar(100**|Nombre del proveedor si el publicador no es una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**políticas**|**int**|Período de retención de la publicación, expresado en horas.|  
|**sync_method**|**int**|Método de sincronización:<br /><br /> **0** = nativo (genera una salida de copia masiva en modo nativo de todas las tablas).<br /><br /> **1** = carácter (genera una salida de copia masiva en modo de carácter de todas las tablas).<br /><br /> **3** = simultáneo (genera una salida de copia masiva en modo nativo de todas las tablas, pero no bloquea la tabla durante la instantánea).<br /><br /> **4** = Concurrent_c (genera una salida de copia masiva en modo de carácter de todas las tablas, pero no bloquea la tabla durante la instantánea)<br /><br /> Los valores **3** y **4** están disponibles para la replicación transaccional y la replicación de mezcla, pero no para la replicación de instantáneas.|  
|**allow_subscription_copy**|**bit**|Habilita o deshabilita la funcionalidad de copia de las bases de datos de suscripciones suscritas a esta publicación. **0** significa que la copia está deshabilitada y **1** significa que está habilitada.|  
|**thirdparty_options**|**int**|Especifica si se suprime la presentación de una publicación en la carpeta de replicación en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] :<br /><br /> **0** = muestra una publicación heterogénea en la carpeta de replicación en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .<br /><br /> **1** = suprime la visualización de una publicación heterogénea en la carpeta de replicación en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**allow_queued_tran**|**bit**|Especifica si la publicación admite la actualización en cola:<br /><br /> **0 =** La publicación no está en cola.<br /><br /> **1** = la publicación está en cola.|  
|**options**|**int**|No hay información disponible para esta versión.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
