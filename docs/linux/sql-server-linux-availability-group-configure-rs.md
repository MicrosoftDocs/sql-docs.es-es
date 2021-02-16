---
title: Configuración de un grupo de disponibilidad de escalado de lectura (SQL Server en Linux)
description: Aprenda a configurar un grupo de disponibilidad AlwaysOn de SQL Server para las cargas de trabajo de escalado de lectura en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/09/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 5cad1c63817bf55231c065123b2456590dbfebb2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352740"
---
# <a name="configure-a-sql-server-availability-group-for-read-scale-on-linux"></a>Configuración de un grupo de disponibilidad de SQL Server para escalado de lectura en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Se puede configurar un grupo de disponibilidad AlwaysOn de SQL Server para las cargas de trabajo de escalado de lectura en Linux. Hay dos tipos de arquitecturas para los grupos de disponibilidad. En una arquitectura de alta disponibilidad se usa un administrador de clústeres para proporcionar una continuidad empresarial mejorada. Esta arquitectura también puede incluir réplicas de escalado de lectura. Para crear la arquitectura de alta disponibilidad, vea [Configuración de un grupo de disponibilidad AlwaysOn de SQL Server para alta disponibilidad en Linux](sql-server-linux-availability-group-configure-ha.md). La otra arquitectura solo admite cargas de trabajo de escalado de lectura. En este artículo se explica cómo crear un AG sin un administrador de clústeres para las cargas de trabajo de escalado de lectura. Esta arquitectura solo proporciona escalado de lectura. No proporciona alta disponibilidad.

> [!NOTE]
> Un grupo de disponibilidad con `CLUSTER_TYPE = NONE` puede incluir réplicas hospedadas en otras plataformas de sistema operativo. No puede admitir la alta disponibilidad. 

[!INCLUDE [Create prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Crear el grupo de disponibilidad

Cree el grupo de disponibilidad (AG). Establezca `CLUSTER_TYPE = NONE`. Además, establezca cada réplica con `FAILOVER_MODE = MANUAL`. Las aplicaciones cliente que ejecutan cargas de trabajo de informes o análisis se pueden conectar directamente a las bases de datos secundarias. También se puede crear una lista de enrutamiento de solo lectura. Las conexiones a la réplica principal reenvían las solicitudes de conexión de lectura a todas las réplicas secundarias de la lista de enrutamiento en modo Round Robin.

El script de Transact-SQL siguiente crea un grupo de disponibilidad denominado `ag1`. El script configura las réplicas de grupo de disponibilidad con `SEEDING_MODE = AUTOMATIC`. Esta configuración hace que SQL Server cree de manera automática la base de datos en todos los servidores secundarios después de que se agreguen al grupo de disponibilidad. Actualice el script siguiente para su entorno. Reemplace los valores `<node1>` y `<node2>` con los nombres de las instancias de SQL Server que hospedan las réplicas. Reemplace el valor `<5022>` con el puerto que haya definido para el punto de conexión. Ejecute el script de Transact-SQL siguiente en la réplica principal de SQL Server:

```SQL
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = NONE)
    FOR REPLICA ON
        N'<node1>' WITH (
            ENDPOINT_URL = N'tcp://<node1>:<5022>',
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
                    SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            ),
        N'<node2>' WITH ( 
            ENDPOINT_URL = N'tcp://<node2>:<5022>', 
            AVAILABILITY_MODE = ASYNCHRONOUS_COMMIT,
            FAILOVER_MODE = MANUAL,
            SEEDING_MODE = AUTOMATIC,
            SECONDARY_ROLE (ALLOW_CONNECTIONS = ALL)
            );
        
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-secondary-sql-servers-to-the-ag"></a>Unir servidores SQL Server secundarios al grupo de disponibilidad

El script de Transact-SQL siguiente une un servidor a un grupo de disponibilidad denominado `ag1`. Actualice el script para su entorno. En cada réplica secundaria de SQL Server, ejecute el script siguiente de Transact-SQL para unirla al grupo de disponibilidad:

```SQL
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../includes/ss-linux-cluster-availability-group-create-post.md)]

Este grupo de disponibilidad no es una configuración de alta disponibilidad. Si necesita alta disponibilidad, siga las instrucciones de [Configuración de un grupo de disponibilidad AlwaysOn de SQL Server en Linux](sql-server-linux-availability-group-configure-ha.md). En concreto, cree el grupo de disponibilidad con `CLUSTER_TYPE=WSFC` (en Windows) o `CLUSTER_TYPE=EXTERNAL` (en Linux). Luego, integre con un administrador de clústeres mediante clústeres de conmutación por error de Windows Server en Windows o Pacemarker en Linux.

## <a name="connect-to-read-only-secondary-replicas"></a>Conectar con réplicas secundarias de solo lectura

Hay dos maneras de conectarse a réplicas secundarias de solo lectura. Las aplicaciones se pueden conectar directamente a la instancia de SQL Server que hospeda la réplica secundaria y consultar las bases de datos. También pueden usar el enrutamiento de solo lectura, lo que requiere un agente de escucha.

* [Réplicas secundarias legibles](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md)
* [Enrutamiento de solo lectura](../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md#ConnectToSecondary)

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Conmutación por error de la réplica principal en un grupo de disponibilidad de escalado de lectura

[!INCLUDE[Force failover](../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Grupos de disponibilidad distribuidos](../database-engine/availability-groups/windows/distributed-availability-groups.md)
* [Información general de los grupos de disponibilidad AlwaysOn (SQL Server)](../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)
* [Realizar una conmutación manual por error forzada](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)