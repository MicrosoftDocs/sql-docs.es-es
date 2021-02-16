---
title: Forzar la conmutación por error para grupos de disponibilidad de SQL Server
description: Forzar la conmutación por error para grupos de disponibilidad con el tipo de clúster NONE
services: ''
author: MikeRayMSFT
ms.topic: include
ms.date: 02/05/2018
ms.author: mikeray
ms.custom: include file
ms.openlocfilehash: 6e4eb6f9cf7b58a18f42d0b99531084d7fb2cd59
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343358"
---
Cada grupo de disponibilidad tiene solo una réplica principal. La réplica principal permite lecturas y escrituras. Para cambiar la réplica principal, puede efectuar una conmutación por error. En un grupo de disponibilidad habitual, el administrador de clústeres automatiza el proceso de conmutación por error. En un grupo de disponibilidad con el tipo de clúster NONE, el proceso de conmutación por error es manual.

Hay dos maneras de efectuar una conmutación por error de la réplica principal en un grupo de disponibilidad de tipo de clúster NONE:

- Conmutación por error manual sin pérdida de datos
- Conmutación por error manual forzada con pérdida de datos


### <a name="manual-failover-without-data-loss"></a>Conmutación por error manual sin pérdida de datos

Use este método si la réplica principal está disponible, pero necesita modificar temporal o permanentemente la instancia que hospeda dicha réplica principal.
Antes de emitir la conmutación por error manual, asegúrese de que la réplica secundaria de destino está actualizada para evitar una posible pérdida de datos.

Para realizar la conmutación por error manual sin pérdida de datos:

1. Establezca las réplicas de destino principal y secundaria actuales en `SYNCHRONOUS_COMMIT`.

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        MODIFY REPLICA ON N'<node2>' 
        WITH (AVAILABILITY_MODE = SYNCHRONOUS_COMMIT);
   ```

1. Ejecute la consulta siguiente para identificar que las transacciones activas se confirman en la réplica principal y en al menos una réplica secundaria sincrónica:

   ```SQL
   SELECT ag.name, 
      drs.database_id, 
      drs.group_id, 
      drs.replica_id, 
      drs.synchronization_state_desc, 
      ag.sequence_number
   FROM sys.dm_hadr_database_replica_states drs, sys.availability_groups ag
   WHERE drs.group_id = ag.group_id; 
   ```

   La réplica secundaria se sincroniza si `synchronization_state_desc` es `SYNCHRONIZED`.

1. Actualice `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` a 1.

   El siguiente script establece `REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT` en 1 en un grupo de disponibilidad denominado `ag1`. Antes de ejecutar el siguiente script, reemplace `ag1` por el nombre del grupo de disponibilidad:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (REQUIRED_SYNCHRONIZED_SECONDARIES_TO_COMMIT = 1);
   ```

   Este valor garantiza que todas las transacciones activas se confirman en la réplica principal y en, al menos, una réplica secundaria sincrónica.
   >[!NOTE]
   >Esta opción no es específica de la conmutación por error y se debe establecer en función de los requisitos del entorno.

1. Establezca la réplica principal sin conexión para preparar el cambio de roles: 

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```

1. Ascienda la réplica secundaria de destino a principal.

   ```SQL
   ALTER AVAILABILITY GROUP AGRScale FORCE_FAILOVER_ALLOW_DATA_LOSS; 
   ```

1. Actualice el rol de la réplica principal antigua a `SECONDARY`, ejecute el comando siguiente en la instancia de SQL Server en la que se hospeda la réplica principal anterior:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] 
        SET (ROLE = SECONDARY); 
   ```

   > [!NOTE]
   > Para eliminar un grupo de disponibilidad, use [DROP AVAILABILITY GROUP](../t-sql/statements/drop-availability-group-transact-sql.md). Para un grupo de disponibilidad creado con el tipo de clúster NONE o EXTERNAL, ejecute el comando en todas las réplicas que forman parte del grupo de disponibilidad.

1. Reanude el movimiento de datos, ejecute el siguiente comando para cada base de datos del grupo de disponibilidad en la instancia de SQL Server que hospeda la réplica principal:

   ```SQL
   ALTER DATABASE [db1]
        SET HADR RESUME
   ```

1. Vuelva a crear cualquier cliente de escucha que haya creado para fines de escalado de lectura y que no esté administrado por un administrador de clústeres. Si el cliente de escucha original apunta a la réplica principal anterior, suéltela y vuelva a crearla para que apunte a la nueva réplica principal.

### <a name="forced-manual-failover-with-data-loss"></a>Conmutación por error manual forzada con pérdida de datos

Si la réplica principal no está disponible y no se puede recuperar justo en ese momento, deberá forzar una conmutación por error en la secundaria con pérdida de datos. Sin embargo, si la réplica principal original se recupera tras la conmutación por error, pasará a ostentar el rol principal. Para evitar discrepancias en los estados de las réplicas, quite la principal original del grupo de disponibilidad tras haber forzado la conmutación por error con pérdida de datos. Una vez que la principal original vuelva a estar en línea, quite el grupo de disponibilidad al completo. 

Para forzar una conmutación por error manual con pérdida de datos de la réplica principal N1 a la secundaria N2, siga estos pasos: 

1. En la réplica secundaria (N2), inicie una conmutación por error forzada: 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale] FORCE_FAILOVER_ALLOW_DATA_LOSS;
    ```
    
1. En la nueva réplica principal (N2), quite la principal original (N1): 

    ```SQL
    ALTER AVAILABILITY GROUP [AGRScale]
    REMOVE REPLICA ON N'N1';
    ```
    
1. Valide que el tráfico de todas las aplicaciones apunte al cliente de escucha o la nueva réplica principal. 
1. Si la principal original (N1) está en línea, desconecte el grupo de disponibilidad AGRScale de la principal original (N1) de inmediato:

   ```SQL
   ALTER AVAILABILITY GROUP [AGRScale] OFFLINE
   ```
1. Si hay datos o cambios sin sincronizar, conserve dichos datos por medio de copias de seguridad u otras opciones de replicación de datos, en consonancia con los requisitos de su empresa.     
1. Luego, quite el grupo de disponibilidad de la principal original (N1):

    ```SQL
    DROP AVAILABILITY GROUP [AGRScale];
    ```
1. Anule la base de datos del grupo de disponibilidad de la réplica principal original (N1): 

    ```SQL
    USE [master]
    GO
    DROP DATABASE [AGDBRScale]
    GO
    ```
    
 1. (Opcional) Si quiere, ahora puede volver a agregar N1 como nueva réplica secundaria al grupo de disponibilidad AGRScale.
