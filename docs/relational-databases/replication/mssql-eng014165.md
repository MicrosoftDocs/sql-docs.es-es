---
description: MSSQL_ENG014165
title: MSSQL_ENG014165 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
helpviewer_keywords:
- MSSQL_ENG014165 error
ms.assetid: 7bb07672-310c-4f51-ae76-c55e7c8d51ea
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 0a48d5391244d1a130ae09edba975d72bfb64475
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191573"
---
# <a name="mssql_eng014165"></a>MSSQL_ENG014165
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>Detalles del mensaje  
  
|Atributo|Value|  
|-|-|  
|Nombre de producto|SQL Server|  
|Id. de evento|14165|  
|Origen de eventos|MSSQLSERVER|  
|Componente|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|Nombre simbólico||  
|Texto del mensaje|Se ha establecido el umbral [%s:%s] para la publicación [%s]. Asegúrese de que el agente de mezcla se está ejecutando y cumple con el requisito esperado.|  
  
## <a name="explanation"></a>Explicación  
 La replicación le permite habilitar advertencias para varias condiciones. Incluye el error al procesar un número suficiente de filas al sincronizar cambios entre un suscriptor y un publicador de mezcla. Se pueden especificar diferentes horas para las conexiones LAN y las de acceso telefónico.  
  
 Al habilitar una advertencia mediante el Monitor de replicación o [sp_replmonitorchangepublicationthreshold](../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md), especifica un umbral que determina cuándo se desencadena una advertencia. Cuando se alcanza o se supera un umbral, aparece una advertencia en el Monitor de replicación y se escribe un evento en el registro de eventos de Windows. Alcanzar un umbral también puede desencadenar una alerta del Agente SQL Server. Para obtener más información, vea [Establecer umbrales y advertencias en el Monitor de replicación](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md) y [Cómo supervisar la replicación mediante programación (programación con RMO)](../../relational-databases/replication/monitor/programmatically-monitor-replication.md).  
  
## <a name="user-action"></a>Acción del usuario  
 Si una suscripción no cumple un umbral de procesamiento de filas, debe determinar si hay algún problema de rendimiento en el sistema o si debe ajustarse el umbral. Una vez configurada la replicación, desarrolle una línea base de rendimiento que le permitirá determinar el comportamiento de la replicación con la carga de trabajo habitual de las aplicaciones y la topología. Incluya el número de filas procesadas en esta línea base para poder establecer un valor adecuado para el umbral.  
  
 Si el valor del umbral es adecuado, pero está siendo superado, debe determinar dónde se encuentra el cuello de botella en el sistema. Para obtener más información acerca de cómo supervisar y solucionar problemas relacionados con el rendimiento de replicación, vea los temas siguientes:  
  
-   [Supervisar el rendimiento con el Monitor de replicación](../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md), especialmente la sección "Ver detalles del rendimiento de la sincronización en la replicación de mezcla"  
  
## <a name="see-also"></a>Consulte también  
 [Referencia de errores y eventos &#40;replicación&#41;](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
