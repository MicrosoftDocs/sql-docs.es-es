---
title: Suscriptores de replicación y grupos de disponibilidad (SQL Server)
description: Obtenga información sobre lo que sucede si un grupo de disponibilidad Always On que contiene una base de datos que es un suscriptor de replicación realiza la conmutación por error en SQL Server.
ms.custom: seo-lt-2019
ms.date: 08/08/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- failover subscribers with AlwaysOn
- Availability Groups [SQL Server], interoperability
- replication [SQL Server], AlwaysOn Availability Groups
ms.assetid: 0995f269-0580-43ed-b8bf-02b9ad2d7ee6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 310438f8a5f8bec6fa0b2b3ef5b17eb1b097f423
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344588"
---
# <a name="replication-subscribers-and-always-on-availability-groups-sql-server"></a>Suscriptores de replicación y grupos de disponibilidad AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Cuando el grupo de disponibilidad AlwaysOn que contiene una base de datos que es un suscriptor de replicación realiza una conmutación por error, se puede producir un error en la suscripción de replicación. Para los suscriptores de inserción de replicación transaccional, el agente de distribución se seguirá replicando automáticamente después de una conmutación por error si la suscripción se creó mediante el nombre de la escucha de grupo de disponibilidad. Para los suscriptores de extracción de replicación transaccional, el agente de distribución se seguirá replicando automáticamente después de una conmutación por error si la suscripción se creó mediante el nombre de la escucha de grupo de disponibilidad y el servidor del suscriptor original está activo y en ejecución. El motivo es que los trabajos del agente de distribución solo se crean en el suscriptor original (réplica principal del grupo de disponibilidad). Para los suscriptores de mezcla, un administrador de replicación debe volver a configurar manualmente el suscriptor volviendo a crear la suscripción.  
  
## <a name="what-is-supported"></a>Qué se admite  
 La replicación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] admite la conmutación por error automática del publicador y la conmutación por error automática de los suscriptores transaccionales. Los suscriptores de mezcla pueden ser parte de un grupo de disponibilidad, pero se requieren acciones manuales para configurar el suscriptor nuevo después de una conmutación por error. Los grupos de disponibilidad no se pueden combinar con escenarios de Websync y SQL Server Compact.  
  
## <a name="how-to-create-transactional-subscription-in-an-always-on-environment"></a>Cómo crear una suscripción transaccional en un entorno de AlwaysOn  
 Para la replicación transaccional, utilice los pasos siguientes para configurar y conmutar por error un grupo de disponibilidad de suscriptor:  
  
1.  Antes de crear la suscripción, agregue la base de datos del suscriptor al grupo de disponibilidad AlwaysOn adecuado.  
  
2.  Agregue el agente de escucha del grupo de disponibilidad del suscriptor como un servidor vinculado a todos los nodos del grupo de disponibilidad. Este paso garantiza que todos los posibles asociados de conmutación por error sean conscientes del agente de escucha y se puedan conectar al mismo.  
  
3.  Con el script de la sección **Crear una suscripción de inserción para una replicación transaccional** de más abajo, cree la suscripción y use el nombre del agente de escucha del grupo de disponibilidad del suscriptor. Después de una conmutación por error, el nombre del agente de escucha siempre sigue siendo válido, mientras que el nombre de servidor real del suscriptor dependerá del nodo real que se convirtió en el nuevo primario.  
  
    > [!NOTE]  
    >  Es preciso que la suscripción se cree mediante un script de [!INCLUDE[tsql](../../../includes/tsql-md.md)] y no se puede crear con [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
4.  Para crear una suscripción de extracción:  
  
    1.  Con el script de ejemplo de la sección **Crear una suscripción de extracción para una replicación transaccional** que aparece más adelante, cree la suscripción y use el nombre del agente de escucha del grupo de disponibilidad del suscriptor. 
   
    2.  Después de una conmutación por error, cree el trabajo del agente de distribución en la réplica principal con el procedimiento almacenado **sp_addpullsubscription_agent**. 
  
 Al crear una suscripción de extracción, con la base de datos de suscripciones en un grupo de disponibilidad, después de cada conmutación por error, se recomienda deshabilitar el trabajo del agente de distribución en la réplica principal anterior y habilitar el trabajo en la nueva réplica principal.  
  
## <a name="creating-a-transactional-replication-push-subscription"></a>Crear una suscripción de inserción para una replicación transaccional  
  
```  
-- commands to execute at the publisher, in the publisher database:  
use [<publisher database name>]  
EXEC sp_addsubscription @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @destination_db = N'<subscriber database name>',   
       @subscription_type = N'Push',   
       @sync_type = N'automatic', @article = N'all', @update_mode = N'read only', @subscriber_type = 0;  
GO  
  
EXEC sp_addpushsubscription_agent @publication = N'<publication name>',   
       @subscriber = N'<availability group listener name>',   
       @subscriber_db = N'<subscriber database name>',   
       @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO  
```  

## <a name="creating-a-transactional-replication-pull-subscription"></a>Crear una suscripción de extracción para una replicación transaccional  
  
```  
-- commands to execute at the subscriber, in the subscriber database:  
use [<subscriber database name>]  
EXEC sp_addpullsubscription @publisher= N'<publisher name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>',
        @subscription_type = N'pull';
Go

EXEC sp_addpullsubscription_agent 
        @publisher =  N'<publisher name>', 
        @subscriber = N'<availability group listener name>',
        @publisher_db= N'<publisher database name>',
        @publication= N'<publication name>' ;
        @job_login = null, @job_password = null, @subscriber_security_mode = 1;  
GO
```  
  
## <a name="to-resume-the-merge-agents-after-the-availability-group-of-the-subscriber-fails-over"></a>Para reanudar los agentes de mezcla después de la conmutación por error del grupo de disponibilidad del suscriptor  
 Para la replicación de mezcla, un administrador de replicación debe volver a configurar manualmente el suscriptor con los pasos siguientes:  
  
1.  Ejecute **sp_subscription_cleanup** para quitar la antigua suscripción para el suscriptor. Realice esta acción en la nueva réplica principal (que era antes la réplica secundaria).  
  
2.  Vuelva a crear la suscripción, para ello cree una nueva suscripción, comenzando por una nueva instantánea.  
  
> [!NOTE]  
>  El proceso actual no es apto para los suscriptores de replicación de mezcla, aunque el escenario principal para la replicación de mezcla son los usuarios desconectados (escritorio, equipos portátiles, dispositivos de auricular) que no usarán los grupos de disponibilidad AlwaysOn en el suscriptor.  
  
  
