---
title: Información general sobre los agentes de replicación | Microsoft Docs
description: Obtenga información sobre los agentes que usa la replicación de SQL Server para llevar a cabo tareas asociadas con el seguimiento de los cambios y la distribución de los datos.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- Distribution Agent
- agents [SQL Server replication]
- Queue Reader Agent, about Queue Reader Agent
- Queue Reader Agent
- Merge Agent, about Merge Agent
- Log Reader Agent, about Log Reader Agent
- replication [SQL Server], agents and profiles
- Log Reader Agent
- Distribution Agent, about Distribution Agent
- agents [SQL Server replication], about agents
- Merge Agent
- Snapshot Agent, about Snapshot Agent
- Snapshot Agent
ms.assetid: a35ecd7d-f130-483c-87e3-ddc8927bb91b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 5181d54b5df6463405f689bc102ddf56336ae602
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467326"
---
# <a name="replication-agents-overview"></a>Información general sobre los agentes de replicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  La replicación utiliza varios programas independientes, llamados agentes, para realizar las tareas asociadas con el seguimiento de los cambios y la distribución de los datos. De forma predeterminada, los agentes de replicación se ejecutan como trabajos programados en el Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y es necesario que se esté ejecutando el Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para que puedan ejecutarse los trabajos. Los agentes de replicación también se pueden ejecutar desde la línea de comandos y en aplicaciones que utilizan Replication Management Objects (RMO). Los agentes de replicación se pueden administrar desde el Monitor de replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
## <a name="sql-server-agent"></a>Agente SQL Server  
 El Agente[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] hospeda y programa los agentes utilizados en la replicación, y proporciona una manera sencilla de ejecutar los agentes de replicación. El Agente[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] también controla y supervisa las operaciones fuera de la replicación. Para obtener más información, consulte [Configure SQL Server Agent](../../../ssms/agent/configure-sql-server-agent.md).  
  
> [!IMPORTANT]  
>  De manera predeterminada, el servicio del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] está deshabilitado cuando se instala [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , a menos que se elija explícitamente iniciar el servicio automáticamente durante la instalación. Para obtener más información sobre cómo iniciar el servicio del Agente [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vea [Start, Stop, or Pause the SQL Server Agent Service](../../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md).  
  
## <a name="snapshot-agent"></a>Agente de instantáneas  
 Por lo general, el Agente de instantáneas se utiliza con todos los tipos de replicación. Prepara esquemas y archivos de datos iniciales de tablas publicadas y otros objetos, almacena los archivos de instantáneas y registra la información acerca del estado de sincronización en la base de datos de distribución. El Agente de instantáneas se ejecuta en el distribuidor. Para más información, consulte [Replication Snapshot Agent](../../../relational-databases/replication/agents/replication-snapshot-agent.md).  
  
## <a name="log-reader-agent"></a>Agente de registro del LOG  
 El Agente de registro del LOG se utiliza en la replicación transaccional. Mueve las transacciones marcadas para replicación desde el registro de transacciones del publicador a la base de datos de distribución. Cada base de datos publicada con la replicación transaccional tiene su propio Agente de registro del LOG, que se ejecuta en el distribuidor y se conecta al publicador (el distribuidor puede estar en el mismo equipo que el publicador). Para más información, consulte [Replication Log Reader Agent](../../../relational-databases/replication/agents/replication-log-reader-agent.md).  
  
## <a name="distribution-agent"></a>Agente de distribución  
 El Agente de distribución se utiliza en la replicación de instantáneas y transaccional. Aplica la instantánea inicial al suscriptor y mueve las transacciones contenidas en la base de datos de distribución a los suscriptores. El Agente de distribución se ejecuta en el distribuidor, para las suscripciones de inserción, o en el suscriptor, para las suscripciones de extracción. Para más información, consulte [Replication Distribution Agent](../../../relational-databases/replication/agents/replication-distribution-agent.md).  
  
## <a name="merge-agent"></a>Agente de mezcla  
 El Agente de mezcla se utiliza con la replicación de mezcla. Aplica la instantánea inicial al suscriptor, y transfiere y reconcilia los cambios incrementales de datos que se producen. Cada suscripción de mezcla tiene su propio Agente de mezcla, que se conecta con el publicador y con el suscriptor, y los actualiza. El Agente de mezcla se ejecuta en el distribuidor, para las suscripciones de inserción, o en el suscriptor, para las suscripciones de extracción. De forma predeterminada, el Agente de mezcla carga los cambios del suscriptor al publicador y, a continuación, descarga los cambios del publicador al suscriptor. Para más información, consulte [Replication Merge Agent](../../../relational-databases/replication/agents/replication-merge-agent.md).  
  
## <a name="queue-reader-agent"></a>Agente de lectura de cola  
 El Agente de lectura de cola se utiliza con la replicación transaccional y la opción de actualización en cola. El agente se ejecuta en el distribuidor y transfiere los cambios realizados en el suscriptor de vuelta al publicador. A diferencia del Agente de distribución y del Agente de mezcla, solo existe una instancia del Agente de lectura de cola para todos los publicadores y las publicaciones de una determinada base de datos. Para obtener más información acerca del Agente de lectura de cola, vea [Replication Queue Reader Agent](../../../relational-databases/replication/agents/replication-queue-reader-agent.md). Para obtener más información sobre las suscripciones actualizables, vea [Updatable Subscriptions for Transactional Replication](../../../relational-databases/replication/transactional/updatable-subscriptions-for-transactional-replication.md).  
  
## <a name="replication-maintenance-jobs"></a>Trabajos de mantenimiento de replicación  
 La replicación incluye varios trabajos de mantenimiento que realizan operaciones de mantenimiento programadas y a petición. Para obtener más información, vea [Administración del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-administration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar y detener un agente de replicación &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)   
 [Ejecutar trabajos de mantenimiento de replicación &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/administration/run-replication-maintenance-jobs-sql-server-management-studio.md)   
 [Conceptos de los ejecutables del Agente de replicación](../../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)   
 [Administración del Agente de replicación](../../../relational-databases/replication/agents/replication-agent-administration.md)  
  
