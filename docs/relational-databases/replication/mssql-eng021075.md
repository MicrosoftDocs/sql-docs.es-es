---
description: MSSQL_ENG021075
title: MSSQL_ENG021075 | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG021075 error
ms.assetid: c8c29543-d1f6-49d5-b6c8-e8c3aa373090
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 046c21ab2758ec1e9147a04f839ddd0e2b4aaa1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187058"
---
# <a name="mssql_eng021075"></a>MSSQL_ENG021075
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|21075|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|La instantánea inicial de la publicación '%1!' aún no está disponible.|  
  
## <a name="explanation"></a>Explicación  
 El error MSSQL_ENG021075 aparece cuando se inicia el Agente de distribución o de mezcla antes de que el Agente de instantáneas termine de generar la instantánea.  
  
## <a name="user-action"></a>Acción del usuario  
 Si no se ha iniciado el Agente de instantáneas para la publicación desde que se creó la suscripción o desde la última vez que eligió reinicializar la suscripción, inícielo y espere a que termine antes de iniciar el Agente de distribución o de mezcla. Para obtener más información, vea [Crear y aplicar una instantánea](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
 Si no finaliza el Agente de instantáneas, revise el historial del Agente de instantáneas para detectar posibles errores y soluciónelos. Para obtener información sobre la visualización del estado y los errores en Monitor de replicación, vea [Visualización de información y realización de tareas mediante el Monitor de replicación](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).  
  
 Si el error persiste, aumente el registro del agente y especifique un archivo de salida para el registro. Dependiendo del contexto del error, esto puede proporcionar los pasos que conducen al error o a mensajes de error adicionales.  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
