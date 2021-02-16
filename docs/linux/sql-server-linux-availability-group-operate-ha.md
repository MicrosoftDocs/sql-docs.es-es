---
title: Manejo de grupos de disponibilidad de SQL Server en Linux
description: En este artículo se describe cómo realizar una actualización gradual con instancias de SQL Server en Linux con grupos de disponibilidad. Antes de actualizar, revise los procedimientos recomendados.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 00e576b6fd2fc92cca591a5ffa3d6e97556bc1ad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352566"
---
# <a name="operate-always-on-availability-groups-on-linux"></a>Manejo de grupos de disponibilidad Always On en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

## <a name="upgrade-availability-group"></a>Actualización de un grupo de disponibilidad

Antes de actualizar un grupo de disponibilidad, revise los patrones y los procedimientos de [Actualización de instancias de la réplica del grupo de disponibilidad](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).

En las secciones siguientes se explica cómo realizar una actualización gradual con instancias de SQL Server en Linux con grupos de disponibilidad. 

### <a name="upgrade-steps-on-linux"></a>Pasos de actualización en Linux

Cuando las réplicas de los grupos de disponibilidad se encuentran en instancias de SQL Server en Linux, el tipo de clúster del grupo de disponibilidad es `EXTERNAL` o `NONE`. Un grupo de disponibilidad administrado por un administrador de clústeres además del clúster de conmutación por error de Windows Server (WSFC) es `EXTERNAL`. Pacemaker con Corosync es un ejemplo de administrador de clústeres externo. Un grupo de disponibilidad sin administrador de clústeres tiene el tipo de clúster `NONE`. Los pasos de actualización que se indican aquí son específicos para los grupos de disponibilidad de tipo de clúster `EXTERNAL` o `NONE`.

El orden en el que se actualizan las instancias depende de si su rol es secundario y de si hospedan o no réplicas sincrónicas o asincrónicas. Actualice primero las instancias de SQL Server que hospedan réplicas secundarias asincrónicas. Luego, actualice las instancias que hospedan réplicas secundarias sincrónicas. 

   >[!NOTE]
   >Si un grupo de disponibilidad solo tiene réplicas asincrónicas, para evitar cualquier pérdida de datos, cambie una réplica a sincrónica y espere hasta que se sincronice. Luego, actualice esta réplica.
   
Antes de comenzar, realice una copia de seguridad de cada base de datos.

1. Detenga el recurso en el nodo que hospeda la réplica secundaria destinada a la actualización.
   
   Antes de ejecutar el comando de actualización, detenga el recurso para que el clúster no lo supervise y genere un error innecesariamente. En el ejemplo siguiente se agrega una restricción de ubicación en el nodo que va a dar lugar a la detención del recurso. Actualice `ag_cluster-master` con el nombre de recurso y `nodeName1` con el nodo que hospeda la réplica destinada a la actualización.

   ```bash
   pcs constraint location ag_cluster-master avoids nodeName1
   ```

1. Actualice SQL Server en la réplica secundaria.

   El siguiente ejemplo actualiza los paquetes de `mssql-server` y `mssql-server-ha`.

   ```bash
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
1. Quite la restricción de ubicación.

   Antes de ejecutar el comando de actualización, detenga el recurso para que el clúster no lo supervise y genere un error innecesariamente. En el ejemplo siguiente se agrega una restricción de ubicación en el nodo que va a dar lugar a la detención del recurso. Actualice `ag_cluster-master` con el nombre de recurso y `nodeName1` con el nodo que hospeda la réplica destinada a la actualización.

   ```bash
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```
   Como procedimiento recomendado, asegúrese de que el recurso se inicia (con el comando `pcs status`) y de que la réplica secundaria está conectada y en estado sincronizado tras la actualización.

1. Después de actualizar todas las réplicas secundarias, realice una conmutación por error manual a una de las réplicas secundarias sincrónicas.

   En los grupos de disponibilidad con un tipo de clúster `EXTERNAL`, use las herramientas de administración de clústeres para la conmutación por error; los grupos de disponibilidad con el tipo de clúster `NONE` deben usar Transact-SQL para la conmutación por error. 
   En el ejemplo siguiente se realiza una conmutación por error de un grupo de disponibilidad con las herramientas de administración de clústeres. Reemplace `<targetReplicaName>` por el nombre de la réplica secundaria sincrónica que se va a convertir en principal:

   ```bash
   sudo pcs resource move ag_cluster-master <targetReplicaName> --master  
   ``` 
   
   >[!IMPORTANT]
   >Los pasos siguientes solo se aplican a los grupos de disponibilidad que no tienen un administrador de clústeres.

   Si el tipo de clúster del grupo de disponibilidad es `NONE`, realice una conmutación por error manual. Realice los pasos siguientes en el orden indicado:

      a. El comando siguiente establece la réplica principal en secundaria. Reemplace `AG1` por el nombre del grupo de disponibilidad. Ejecute el comando de Transact-SQL en la instancia de SQL Server que hospeda la réplica principal.

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] SET (ROLE = SECONDARY);
      ```

      b. El comando siguiente establece una réplica secundaria sincrónica en principal. Ejecute el siguiente comando de Transact-SQL en la instancia de destino de SQL Server (la instancia que hospeda la réplica secundaria sincrónica).

      ```transact-sql
      ALTER AVAILABILITY GROUP [ag1] FAILOVER;
      ```

1. Tras la conmutación por error, actualice SQL Server en la réplica principal antigua mediante la repetición del procedimiento anterior.

   El siguiente ejemplo actualiza los paquetes de `mssql-server` y `mssql-server-ha`.

   ```bash
   # add constraint for the resource to stop on the upgraded node
   # replace 'nodename2' with the name of the cluster node targeted for upgrade
   pcs constraint location ag_cluster-master avoids nodeName2
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```
   
   ```bash
   # upgrade mssql-server and mssql-server-ha packages
   sudo yum update mssql-server
   sudo yum update mssql-server-ha
   ```

   ```bash
   # remove the constraint; make sure the resource is started and replica is connected and synchronized
   pcs constraint remove location-ag_cluster-master-rhel1--INFINITY
   ```

1. En el caso de un grupo de disponibilidad con un administrador de clústeres externo (donde el tipo de clúster es EXTERNO), limpie la restricción de ubicación causada por la conmutación por error manual. 

   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```

1. Reanude el movimiento de datos de la réplica secundaria recién actualizada: la réplica principal anterior. Este paso es necesario cuando una instancia de versión superior de SQL Server está transfiriendo bloques de registro a una instancia de versión inferior de un grupo de disponibilidad. Ejecute el siguiente comando en la nueva réplica secundaria (la réplica principal anterior).

   ```transact-sql
   ALTER DATABASE database_name SET HADR RESUME;
   ```

Después de actualizar todos los servidores, puede realizar una conmutación por recuperación. Vuelva a conmutar por error a la réplica principal original, si fuera necesario. 

## <a name="drop-an-availability-group"></a>Eliminación de un grupo de disponibilidad

Para eliminar un grupo de disponibilidad, ejecute [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Si el tipo de clúster es `EXTERNAL` o `NONE`, ejecute el comando en cada instancia de SQL Server que hospede una réplica. Por ejemplo, para quitar un grupo de disponibilidad denominado `group_name`, ejecute el siguiente comando:

   ```transact-sql
   DROP AVAILABILITY GROUP group_name
   ```
 

## <a name="next-steps"></a>Pasos siguientes

[Configuración de clúster RHEL para el grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configuración de clústeres de SLES para grupos de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurar el clúster de Ubuntu para recursos de clúster del grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
