---
title: Configuración de escalado de lectura para un grupo de disponibilidad
description: Obtenga información sobre cómo configurar un grupo de disponibilidad Always On de SQL Server para las cargas de trabajo de escalado de lectura en Windows.
ms.custom: seodec18
author: cawrites
ms.author: chadam
ms.reviewer: ''
ms.date: 05/24/2018
ms.topic: how-to
ms.prod: sql
ms.technology: availability-groups
ms.openlocfilehash: 995d8d2238ef9ded767544423093c8352e02bb1c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343373"
---
# <a name="configure-read-scale-for-an-always-on-availability-group"></a>Configuración de escalado de lectura para un grupo de disponibilidad Always On

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Se puede configurar un grupo de disponibilidad AlwaysOn de SQL Server para las cargas de trabajo de escalado de lectura en Windows. Hay dos tipos de arquitectura para los grupos de disponibilidad:
* Una arquitectura de alta disponibilidad en la que se usa un administrador de clústeres para proporcionar una continuidad empresarial mejorada y que puede incluir réplicas secundarias legibles. Para crear esta arquitectura de alta disponibilidad, vea [Creación y configuración de grupos de disponibilidad (SQL Server)](creation-and-configuration-of-availability-groups-sql-server.md). 
* Una arquitectura en la que solo se admiten cargas de trabajo de escalado de lectura. 

En este artículo se explica cómo crear un grupo de disponibilidad sin un administrador de clústeres para las cargas de trabajo de escalado de lectura. Esta arquitectura solo proporciona escalado de lectura. No proporciona alta disponibilidad.

>[!NOTE]
>Un grupo de disponibilidad con `CLUSTER_TYPE = NONE` puede incluir réplicas hospedadas en varias plataformas de sistema operativo. No puede admitir la alta disponibilidad. Para el sistema operativo Linux vea [Configurar un grupo de disponibilidad de SQL Server para la escala de lectura en Linux](../../../linux/sql-server-linux-availability-group-configure-rs.md).

[!INCLUDE [Create prerequisites](../../../includes/ss-availability-group-rs-prereq.md)]

## <a name="create-an-availability-group"></a>Creación de un grupo de disponibilidad

Cree un grupo de disponibilidad. Establezca `CLUSTER_TYPE = NONE`. Además, establezca cada réplica con `FAILOVER_MODE = NONE`. Las aplicaciones cliente que ejecutan cargas de trabajo de informes o análisis se pueden conectar directamente a las bases de datos secundarias. También se puede crear una lista de enrutamiento de solo lectura. Las conexiones a la réplica principal reenvían las solicitudes de conexión de lectura a todas las réplicas secundarias de la lista de enrutamiento en modo Round Robin.

En el siguiente script de Transact-SQL se crea un grupo de disponibilidad denominado `ag1`. El script configura las réplicas de grupo de disponibilidad con `SEEDING_MODE = AUTOMATIC`. Esta configuración hace que SQL Server cree de manera automática la base de datos en todos los servidores secundarios después de que se agreguen al grupo de disponibilidad. 

Actualice el script siguiente para su entorno. Reemplace los valores `<node1>` y `<node2>` con los nombres de las instancias de SQL Server que hospedan las réplicas. Reemplace el valor `<5022>` con el puerto que haya definido para el punto de conexión. Ejecute el script de Transact-SQL siguiente en la réplica principal de SQL Server:

```sql
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

### <a name="join-secondary-sql-server-instances-to-the-availability-group"></a>Una las instancias de SQL Server secundarias al grupo de disponibilidad.

El script de Transact-SQL siguiente une un servidor a un grupo de disponibilidad denominado `ag1`. Actualice el script para su entorno. Para unir al grupo de disponibilidad, ejecute el script de Transact-SQL siguiente en cada réplica secundaria de SQL Server:

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = NONE);

ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create post](../../../includes/ss-availability-group-rs-postactivity.md)]

Este grupo de disponibilidad no es una configuración de alta disponibilidad. Si necesita alta disponibilidad, siga las instrucciones descritas en [Configure an Always On Availability Group for SQL Server on Linux](../../../linux/sql-server-linux-availability-group-configure-ha.md) (Configuración de un grupo de disponibilidad AlwaysOn para SQL Server en Linux) o [Creación y configuración de grupos de disponibilidad en Windows](creation-and-configuration-of-availability-groups-sql-server.md).

## <a name="connect-to-read-only-secondary-replicas"></a>Conectar con réplicas secundarias de solo lectura

Hay dos maneras de conectarse a réplicas secundarias de solo lectura:
* Las aplicaciones se pueden conectar directamente a la instancia de SQL Server que hospeda la réplica secundaria y consultar las bases de datos. Para obtener más información, vea [Réplicas secundarias legibles](active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).
* Las aplicaciones también pueden usar el enrutamiento de solo lectura, lo que requiere un agente de escucha. Si va a implementar un escenario de escalado de lectura sin un administrador de clústeres, puede crear un cliente de escucha que apunte a la dirección IP de la réplica principal actual y al puerto, distinto al que escucha SQL Server. Tendrá que volver a crear el cliente de escucha para que apunte a la nueva dirección IP principal después de una conmutación por error. Para obtener más información, vea [Enrutamiento de solo lectura](listeners-client-connectivity-application-failover.md#ConnectToSecondary).

## <a name="fail-over-the-primary-replica-on-a-read-scale-availability-group"></a>Conmutación por error de la réplica principal en un grupo de disponibilidad de escalado de lectura

[!INCLUDE[Force failover](../../../includes/ss-force-failover-read-scale-out.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Grupos de disponibilidad distribuidos](./distributed-availability-groups.md)
* [Información general de los grupos de disponibilidad AlwaysOn (SQL Server)](overview-of-always-on-availability-groups-sql-server.md)
* [Realizar una conmutación manual por error forzada](perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)