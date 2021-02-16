---
title: Visualización de información y realización de tareas (Monitor de replicación)
description: Aprenda a ver información y a realizar varias tareas mediante el Monitor de replicación de SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing publication information
- publications [SQL Server replication], viewing information
- publications [SQL Server replication], Replication Monitor tasks
ms.assetid: 92e28a07-d6a7-461b-a0b3-bd9bc6afcbe5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 24eb21c5907e343ec433316fbdceebfc3864cedc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340753"
---
# <a name="view-information-and-perform-tasks-using-replication-monitor"></a>Visualización de información y realización de tareas mediante el Monitor de replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
El Monitor de replicación proporciona una serie de pestañas y opciones para ver información y realizar diversas tareas. En este artículo se describe todo lo que se puede ver y realizar con el Monitor de replicación. 


## <a name="for-a-publication"></a>Para una publicación

### <a name="view-information"></a>Ver información
El Monitor de replicación proporciona las siguientes pestañas con información acerca de la publicación seleccionada:  
  
-   **Todas las suscripciones**: en esta pestaña se muestra información sobre todas las suscripciones de la publicación seleccionada.  
  
-   **Agentes**: en esta pestaña se muestra información sobre todos los agentes usados por una replicación:   
    -   El Agente de instantáneas, utilizado por todas las publicaciones.   
    -   El Agente de registro del LOG, utilizado por todas las publicaciones transaccionales.   
    -   El Agente de lectura de cola, utilizado por las publicaciones transaccionales con suscripciones de actualización en cola.  
  
-   **Advertencias**: esta pestaña permite especificar advertencias y alertas para los agentes.  
  
-   **Testigos de seguimiento** (solo para la replicación transaccional): esta pestaña permite medir la latencia, es decir, el tiempo que transcurre entre la confirmación de una transacción en el publicador y la confirmación de la transacción correspondiente en el suscriptor.  
  
 Para más información sobre las opciones de cada pestaña, haga clic en la pestaña en el panel derecho y después en **Ayuda** en la barra de menús. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Realizar tareas
  
1.  Expanda un grupo de publicador en el panel de la izquierda, expanda un publicador y, después, haga clic en una publicación.   
2.  Para ver y modificar las propiedades de una publicación, haga clic con el botón derecho en la publicación y, después, seleccione **Propiedades**.    
3.  Para ver información sobre las suscripciones, haga clic en la pestaña **Todas las suscripciones**, haga clic con el botón derecho en la suscripción y, después, seleccione **Propiedades**. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada. 
4.  Para ver información sobre los agentes, haga clic en la pestaña **Agentes**. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada. 
5.  Para ver información sobre las advertencias y los umbrales del agente, haga clic en la pestaña **Advertencias**. Para más información, consulte [Set Thresholds and Warnings in Replication Monitor](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md).  
6.  Para ver información sobre los testigos de seguimiento, haga clic en la pestaña **Testigos de seguimiento**. Para obtener más información acerca de cómo utilizar los testigos de seguimiento, vea [Medir la latencia y validar las conexiones de la replicación transaccional](../../../relational-databases/replication/monitor/measure-latency-and-validate-connections-for-transactional-replication.md).  
  
## <a name="for-a-publisher"></a>Para un publicador 

### <a name="view-information"></a>Ver información
El Monitor de replicación proporciona las siguientes pestañas que muestran información sobre el publicador seleccionado:   
-   **Publicaciones**: en esta pestaña se muestra información sobre todas las publicaciones en el publicador seleccionado.   
-   **Lista de supervisión de suscripciones**: en esta pestaña se muestra información sobre las suscripciones de todas las publicaciones disponibles en el publicador seleccionado que tienen errores, advertencias o un rendimiento insuficiente. Esta pestaña no se muestra en los distribuidores que se ejecutan en versiones anteriores de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)].    
-   **Agentes**: en esta pestaña se muestra información detallada sobre los agentes y trabajos que usan todos los tipos de replicación. La pestaña también le permite iniciar y detener cada agente y trabajo. Para obtener más información acerca de las opciones de esta pestaña, haga clic en el panel derecho y , a continuación, haga clic en **Ayuda** en la barra de menús. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Realizar tareas
  
1.  Expanda un grupo de publicador en el panel izquierdo y, a continuación, haga clic en un publicador.   
2.  Para ver información sobre todas las publicaciones, haga clic en la pestaña **Publicaciones** .    
3.  Para ver información acerca de las suscripciones, haga clic en la pestaña **Lista de supervisión de suscripciones** . En esta pestaña, también puede realizar tareas y tener acceso a información más detallada:   
    -   Para ver información detallada sobre el agente asociado a una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**.    
    -   Para ver las propiedades de una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Propiedades**.    
    -   Para sincronizar una suscripción de inserción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.   
    -   Para reinicializar una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Reinicializar suscripción**.   
4.  Para ver información sobre los agentes, haga clic en la pestaña **Agentes** . En esta pestaña, también puede realizar tareas y tener acceso a información más detallada:    
    -   Para ver información detallada sobre un agente (por ejemplo, mensajes informativos y mensajes de error), haga clic con el botón secundario en el agente y, a continuación, haga clic en **Ver detalles**.  
    -   Para ver información detallada del trabajo que ejecuta el agente (como la programación, los detalles de los pasos del trabajo, etc.), haga clic con el botón secundario en el agente y, a continuación, haga clic en **Propiedades**.    
    -   Para administrar perfiles para el agente, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Perfil del agente**. Para más información, vea [Trabajar con perfiles del Agente de replicación](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).    
    -   Para iniciar un agente que no se está ejecutando, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Iniciar agente**.  
    -   Para detener un agente que se está ejecutando, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Detener agente**.  


## <a name="for-a-subscription"></a>Para una suscripción 

### <a name="view-information"></a>Ver información
  El Monitor de replicación proporciona las siguientes pestañas que incluyen información acerca de las suscripciones:    
-   **Todas las suscripciones**: se muestra información sobre todas las suscripciones de la publicación seleccionada.   
-   **Lista de supervisión de suscripciones**: se muestra información sobre las suscripciones de todas las publicaciones disponibles en el publicador seleccionado que tienen errores, advertencias o un rendimiento insuficiente. Esta pestaña no se muestra en los distribuidores que se ejecutan en versiones anteriores de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Para obtener más información acerca de las opciones de cada pestaña, haga clic en la pestaña en el panel derecho y, a continuación, haga clic en **Ayuda** en la barra de menús. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Realizar tareas
  
1.  Expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.   
2.  Para ver información acerca de las suscripciones, haga clic en la pestaña **Todas las suscripciones** . Para ver solamente aquellas suscripciones que se encuentren en un estado determinado, por ejemplo sincronizando, seleccione una opción de la lista desplegable **Mostrar** .    
3.  Para ver y modificar las propiedades de una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Propiedades**. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada. 
  
### <a name="to-view-information-and-perform-tasks-for-subscriptions-in-the-subscription-watch-list-tab"></a>Para ver información y realizar tareas para suscripciones en la pestaña Lista de supervisión de suscripciones  
  
1.  Expanda un grupo de publicador en el panel izquierdo y, a continuación, haga clic en un publicador.    
2.  Para ver información acerca de las suscripciones, haga clic en la pestaña **Lista de supervisión de suscripciones** .    
3.  Seleccione el tipo de suscripción que quiere mostrar en la lista desplegable **Mostrar suscripciones \<SubscriptionType>** . Para ver solamente aquellas suscripciones que se encuentren en un estado determinado, por ejemplo sincronizando, seleccione una opción de la lista desplegable **Mostrar** .    
4.  Para ver y modificar las propiedades de una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Propiedades**. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada. 
  
  
## <a name="for-publication-agents"></a>Para los agentes de publicación
El Monitor de replicación proporciona la pestaña **Agentes** , que incluye información sobre los agentes asociados con la publicación seleccionada. El Agente de distribución y el Agente de mezcla se asocian a las suscripciones. 
  
### <a name="view-information"></a>Ver información
 Esta pestaña muestra información de los siguientes agentes:  
  
-   El Agente de instantáneas, utilizado por todas las publicaciones.  
-   El Agente de registro del LOG, utilizado por todas las publicaciones transaccionales.  
-   El Agente de lectura de cola, utilizado por las publicaciones transaccionales con suscripciones de actualización en cola habilitadas. 
  
 Para ver más información acerca de las opciones de esta pestaña, haga clic en **Ayuda** en la barra de menús. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="to-view-information-and-perform-tasks-for-the-agents-associated-with-a-publication"></a>Para ver información y realizar tareas para los agentes asociados con una publicación  
  
1.  Expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.    
2.  Para ver información sobre los agentes, haga clic en la pestaña **Agentes** . En esta pestaña, también puede realizar tareas y tener acceso a información más detallada:    
    -   Para ver información detallada sobre un agente (por ejemplo, mensajes informativos y mensajes de error), haga clic con el botón secundario en el agente y, a continuación, haga clic en **Ver detalles**.  
    -   Para ver información detallada del trabajo que ejecuta el agente (como la programación, los detalles de los pasos del trabajo, etc.), haga clic con el botón secundario en el agente y, a continuación, haga clic en **Propiedades**.    
    -   Para administrar perfiles para el agente, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Perfil del agente**. Para más información, vea [Trabajar con perfiles del Agente de replicación](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).   
    -   Para iniciar un agente que no se está ejecutando, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Iniciar agente**.   
    -   Para detener un agente que se está ejecutando, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Detener agente**.  
  
## <a name="for-subscription-agents"></a>Para los agentes de suscripción

### <a name="view-information"></a>Ver información
-   **Todas las suscripciones**: se muestra información sobre todas las suscripciones de la publicación seleccionada.  
  
-   **Lista de supervisión de suscripciones**: está pensada para mostrar información sobre las suscripciones de todas las publicaciones disponibles en el publicador seleccionado que tienen errores, advertencias o un rendimiento insuficiente. Esta pestaña no se muestra en los distribuidores que se ejecutan en versiones anteriores de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)]. Para obtener más información acerca de las opciones de cada pestaña, haga clic en la pestaña en el panel derecho y, a continuación, haga clic en **Ayuda** en la barra de menús. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
### <a name="perform-tasks"></a>Realizar tareas
  
1.  Expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.    
2.  Haga clic en la pestaña **Todas las suscripciones** para ver información sobre todas las suscripciones. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada:   
    -   Para ver información detallada sobre el agente asociado a una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**. La información detallada incluye: historial del agente y mensajes de error, estadísticas de rendimiento de la replicación transaccional y estadísticas de sincronización de los artículos de la replicación de mezcla.  
  
         Las pestañas de la ventana de detalle que se muestra dependen del tipo de suscripción: en las suscripciones de instantánea, la pestaña es **Historial de Distribuidor a suscriptor**; en las suscripciones transaccionales, las pestañas son **Historial de Publicador a distribuidor**, **Historial de Distribuidor a suscriptor** y **Comandos sin distribuir**; en las suscripciones de mezcla, la pestaña es **Historial de sincronizaciones**.  
  
    -   Para sincronizar una suscripción de inserción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.    
    -   Para reinicializar una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Reinicializar suscripción**.    
    -   Para validar una única suscripción de mezcla, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar suscripción**. Para validar todas las suscripciones de una publicación de combinación, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar todas las suscripciones**. Para validar todas las suscripciones de una publicación transaccional, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar suscripciones**.    
    -   Para administrar perfiles para el agente, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Perfil del agente**. Para más información, vea [Trabajar con perfiles del Agente de replicación](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  
  
### <a name="subscription-watch-list-tab"></a>Pestaña Lista de supervisión de suscripciones 
  
1.  Expanda un grupo de publicador en el panel izquierdo y, a continuación, haga clic en un publicador.    
2.  Haga clic en la pestaña **Lista de supervisión de suscripciones** para ver información sobre todas las suscripciones. En esta pestaña, también puede realizar tareas y tener acceso a información más detallada:   
    -   Para ver información detallada sobre el agente asociado a una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**. La información detallada incluye: historial del agente y mensajes de error, estadísticas de rendimiento de la replicación transaccional y estadísticas de sincronización de los artículos de la replicación de mezcla.    
         Las pestañas de la ventana de detalle que se muestra dependen del tipo de suscripción: en las suscripciones de instantánea, la pestaña es **Historial de Distribuidor a suscriptor**; en las suscripciones transaccionales, las pestañas son **Historial de Publicador a distribuidor**, **Historial de Distribuidor a suscriptor** y **Rendimiento**; en las suscripciones de mezcla, la pestaña es **Historial de sincronizaciones**.  
  
    -   Para sincronizar una suscripción de inserción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Iniciar sincronización**.    
    -   Para reinicializar una suscripción, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Reinicializar suscripción**.    
    -   Para validar una única suscripción de mezcla, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar suscripción**. Para validar todas las suscripciones de una publicación de combinación, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar todas las suscripciones**. Para validar todas las suscripciones de una publicación transaccional, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Validar suscripciones**.    
    -   Para administrar perfiles para el agente, haga clic con el botón secundario en el agente y, a continuación, haga clic en **Perfil del agente**. Para más información, vea [Trabajar con perfiles del Agente de replicación](../../../relational-databases/replication/agents/work-with-replication-agent-profiles.md).  

    


## <a name="see-also"></a>Consulte también  
 [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md)   
 [Ver y modificar las propiedades de una suscripción de inserción](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md)   
 [Ver y modificar las propiedades de una suscripción de extracción](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  
 [Establecer umbrales y advertencias en el Monitor de replicación](../../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)   
 [Administración del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-administration.md)    
  
  
