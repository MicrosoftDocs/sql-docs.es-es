---
title: Visualización del estado de publicación y suscripción (Monitor de replicación)
description: Aprenda a visualizar el estado de publicación y suscripción mediante el Monitor de replicación de SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Log Reader Agent, monitoring
- Merge Agent, monitoring
- Queue Reader Agent, monitoring
- publications [SQL Server replication], viewing information
- Snapshot Agent, monitoring
- Distribution Agent, monitoring
- monitoring performance [SQL Server replication], publication status
- monitoring performance [SQL Server replication], subscription status
- subscriptions [SQL Server replication], viewing status
- Replication Monitor, publication and subscription status
ms.assetid: 16590771-9867-463e-a973-36a5c145ac16
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b716a4cd679e0cb252fb5805c0d3a8e74c6fb60a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460248"
---
# <a name="view-publication-and-subscription-status-in-replication-monitor"></a>Ver el estado de la suscripción y la publicación en el Monitor de replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  El Monitor de replicación de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] muestra información de publicaciones y suscripciones:  
  
-   El estado de una publicación está determinado por el estado de prioridad más alto de sus suscripciones. Por ejemplo, si una suscripción a una publicación tiene un error y otra tiene un problema de rendimiento se muestra un estado de error para la publicación.  
  
-   El estado de una suscripción se determina por el estado de los agentes que dan servicio a la suscripción. En la replicación de mezcla, es el Agente de mezcla. En la replicación transaccional, es el Agente de registro del LOG o el Agente de distribución (se muestra el estado de prioridad más alto; el estado también se puede determinar por el Agente de lectura de cola si se utilizan suscripciones de actualización en cola). En la replicación de instantáneas, es el Agente de instantáneas o el Agente de distribución (se muestra el estado de prioridad más alto).  
  
 En las tablas de las siguientes secciones se enumeran los posibles valores de estado de publicaciones y suscripciones. Tres de los valores de estado solamente se muestran si se cumple o supera un umbral:  
  
-   Suscripción expirada  
  
     Este valor de estado se aplica a todos los tipos de replicación. Para más información, consulte [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
  
-   Rendimiento crítico  
  
     Este valor de estado se aplica a la replicación transaccional y a la replicación de mezcla. Para obtener más información, consulte [Monitor Performance with Replication Monitor](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md) (Supervisar el rendimiento con el Monitor de replicación).  
  
-   Mezcla de ejecución prolongada  
  
     Este valor de estado se aplica a la replicación de mezcla. Para obtener más información, consulte [Monitor Performance with Replication Monitor](../../../relational-databases/replication/monitor/monitor-performance-with-replication-monitor.md) (Supervisar el rendimiento con el Monitor de replicación).  
  
 Además de los estados de suscripción y publicación, la replicación de mezcla proporciona estadísticas de artículos, que ofrecen información detallada sobre cuánto tiempo tarda en completarse la fase de mezcla, cuanto tiempo se invierte en procesar un artículo determinado, el tipo de conexión de conexión que utiliza un suscriptor y demás información importante. Las estadísticas se muestran en la ventana del Agente de mezcla en el Monitor de replicación. La replicación de instantáneas y transaccional proporciona información detallada sobre el proceso del Agente de distribución.  
  
 **Para ver el estado de la publicación y la suscripción**  
  
-   Monitor de replicación: [Visualización de información y realización de tareas mediante el Monitor de replicación](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md) 
  
 **Para ver información detallada de los agentes**  
  
-   Monitor de replicación: [Visualización de información y realización de tareas mediante el Monitor de replicación](../../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md).
  
## <a name="publication-status-values"></a>Valores del estado de la publicación  
 En la siguiente tabla se muestran los valores de estado de la publicación y sus iconos correspondientes en orden de prioridad.  
  
|Status|Icono|  
|------------|----------|  
|Error|![Icono de la interfaz de usuario: error](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icono de la interfaz de usuario: error")|  
|Rendimiento crítico|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Reintentando comando con errores|![Icono de la interfaz de usuario: reintento del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icono de la interfaz de usuario: reintento del agente de replicación")|  
|Aceptar|None|  
  
## <a name="subscription-status-values"></a>Valores de estado de la suscripción  
 En las siguientes tablas se muestran los valores de estado de la suscripción y sus iconos correspondientes en orden de prioridad. Es posible que una suscripción tenga dos estados simultáneamente, por ejemplo **Con expiración en breve/Expirada** y **Reintentando comando con errores**, en cuyo caso se muestra el estado de prioridad más alta.  
  
 Los valores de estado **Rendimiento crítico**, **Con expiración en breve/Expirada** y **No inicializada** son advertencias. Cuando se muestra una advertencia, el Monitor de replicación también muestra si se está ejecutando un agente. Por ejemplo, el estado podría ser **En ejecución, Rendimiento crítico**.  
  
### <a name="transactional-subscriptions"></a>Suscripciones transaccionales  
  
|Status|Icono|  
|------------|----------|  
|Error|![Icono de la interfaz de usuario: error](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icono de la interfaz de usuario: error")|  
|Rendimiento crítico|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Con expiración en breve/Expirada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Suscripción no inicializada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Reintentando comando con errores|![Icono de la interfaz de usuario: reintento del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icono de la interfaz de usuario: reintento del agente de replicación")|  
|No está en ejecución|![Icono de la interfaz de usuario: detención del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icono de la interfaz de usuario: detención del agente de replicación")|  
|En ejecución|![Icono de la interfaz de usuario: ejecución del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icono de la interfaz de usuario: ejecución del agente de replicación")|  
  
### <a name="merge-subscriptions"></a>Suscripciones de mezcla  
  
|Status|Icono|  
|------------|----------|  
|Error|![Icono de la interfaz de usuario: error](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icono de la interfaz de usuario: error")|  
|Rendimiento crítico|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Mezcla de ejecución prolongada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Con expiración en breve/Expirada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Suscripción no inicializada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Reintentando comando con errores|![Icono de la interfaz de usuario: reintento del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icono de la interfaz de usuario: reintento del agente de replicación")|  
|Sincronizando|![Icono de la interfaz de usuario: ejecución del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icono de la interfaz de usuario: ejecución del agente de replicación")|  
|No se están sincronizando|![Icono de la interfaz de usuario: detención del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icono de la interfaz de usuario: detención del agente de replicación")|  
  
### <a name="snapshot-subscriptions"></a>Suscripciones de instantáneas  
  
|Status|Icono|  
|------------|----------|  
|Error|![Icono de la interfaz de usuario: error](../../../database-engine/availability-groups/windows/media/repl-icon-error.gif "Icono de la interfaz de usuario: error")|  
|Con expiración en breve/Expirada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Suscripción no inicializada|![Icono de la interfaz de usuario: advertencia](../../../database-engine/availability-groups/windows/media/repl-icon-warn.gif "Icono de la interfaz de usuario: advertencia")|  
|Reintentando comando con errores|![Icono de la interfaz de usuario: reintento del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-retry.gif "Icono de la interfaz de usuario: reintento del agente de replicación")|  
|Sincronizando|![Icono de la interfaz de usuario: ejecución del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-running.gif "Icono de la interfaz de usuario: ejecución del agente de replicación")|  
|No se están sincronizando|![Icono de la interfaz de usuario: detención del agente de replicación](../../../relational-databases/replication/monitor/media/repl-icon-stopped.gif "Icono de la interfaz de usuario: detención del agente de replicación")|  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación](../../../relational-databases/replication/monitor/monitoring-replication.md)  
  
  
