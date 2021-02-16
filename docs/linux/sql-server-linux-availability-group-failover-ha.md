---
title: 'Administrar la conmutación por error del grupo de disponibilidad: SQL Server en Linux'
description: 'En este artículo se describen los tipos de conmutación por error: automática, conmutación por error manual planeada y conmutación por error manual forzada. Las conmutaciones por error automáticas o manuales planeadas mantienen todos los datos.'
author: tejasaks
ms.author: tejasaks
ms.reviewer: vanto
ms.date: 03/01/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 52fbeee33dd992f4916f33a1545b59265a8b47f9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345648"
---
# <a name="always-on-availability-group-failover-on-linux"></a>Conmutación por error del grupo de disponibilidad Always On en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Dentro del contexto de un grupo de disponibilidad, el rol principal y el rol secundario de las réplicas de disponibilidad suelen ser intercambiables en un proceso denominado conmutación por error. Hay tres formas de conmutación por error: conmutación por error automática (sin pérdida de datos), conmutación por error manual planeada (sin pérdida de datos) y conmutación por error manual forzada (con posible pérdida de datos), normalmente denominada *conmutación por error forzada*. Las conmutaciones por error automáticas o manuales planeadas conservan todos los datos. Un grupo de disponibilidad conmuta por error en el nivel de la réplica de disponibilidad. Es decir, un grupo de disponibilidad conmuta por error en una de sus réplicas secundarias (el destino de la conmutación por error actual). 

Para obtener información general sobre la conmutación por error, vea [Conmutación por error y modos de conmutación por error](../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).

## <a name="manual-failover"></a><a name="failover"></a>Conmutación por error manual

Use las herramientas de administración de clústeres para conmutar por error un grupo de disponibilidad administrado por un administrador de clústeres externo. Por ejemplo, si una solución usa Pacemaker para administrar un clúster de Linux, use `pcs` para realizar las conmutaciones por error manuales en RHEL o Ubuntu. En SLES, use `crm`. 

> [!IMPORTANT]
> En las operaciones normales, no conmute por error con las herramientas de administración de SQL Server o Transact-SQL, como SSMS o PowerShell. Cuando `CLUSTER_TYPE = EXTERNAL`, el único valor aceptable para `FAILOVER_MODE` es `EXTERNAL`. Con esta configuración, el administrador de clústeres externo ejecuta todas las acciones de conmutación por error manuales o automáticas. Para obtener instrucciones sobre cómo forzar la conmutación por error con una posible pérdida de datos, vea [Forzar la conmutación por error](#forceFailover).

### <a name="a-namemanualfailovermanual-failover-steps"></a><a name="manualFailover">Pasos de una conmutación por error manual

Para conmutar por error, la réplica secundaria que se convertirá en la réplica principal tiene que ser sincrónica. Si una réplica secundaria es asincrónica, [cambie el modo de disponibilidad](../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).

Conmute por error de forma manual en dos pasos.

   Primero, [conmute por error de forma manual al mover el recurso del grupo de disponibilidad](#manualMove) desde el nodo del clúster propietario de los recursos a un nodo nuevo.

   El clúster conmutará por error el recurso del grupo de disponibilidad y agregará una restricción de ubicación. Esta restricción configura el recurso para ejecutarse en el nuevo nodo. Quite esta restricción para conmutar por error correctamente en el futuro.

   En segundo lugar, [quite la restricción de ubicación](#removeLocConstraint).

#### <a name="step-1-manually-fail-over-by-moving-availability-group-resource"></a><a name="manualMove"></a> Paso 1. Conmutar por error de forma manual al mover el recurso de un grupo de disponibilidad

Para conmutar por error de forma manual el recurso de un grupo de disponibilidad denominado *ag_cluster* a un nodo de clúster denominado *nodeName2*, ejecute el comando adecuado para su distribución:

- **Ejemplo de RHEL/Ubuntu**

   ```bash
   sudo pcs resource move ag_cluster-master nodeName2 --master
   ```

- **Ejemplo de SLES**

   ```bash
   crm resource migrate ag_cluster nodeName2
   ```

>[!IMPORTANT]
>Después de conmutar por error de forma manual un recurso, necesita quitar una restricción de ubicación que se agregará automáticamente.

#### <a name="step-2-remove-the-location-constraint"></a><a name="removeLocConstraint"> </a> Paso 2. Quitar la restricción de ubicación

Durante una conmutación por error manual, el comando `move` de `pcs` o el comando `migrate` de `crm` agregan una restricción de ubicación para el recurso que se aplicará en el nuevo nodo de destino. Para ver la nueva restricción, ejecute el comando siguiente después de mover manualmente el recurso:

- **Ejemplo de RHEL/Ubuntu**

   ```bash
   sudo pcs constraint list --full
   ```

- **Ejemplo de SLES**

   ```bash
   crm config show
   ```

Un ejemplo de la restricción creada debido a una conmutación por error manual. 
 `Enabled on: Node1 (score:INFINITY) (role: Master) (id:cli-prefer-ag_cluster-master)`

   > [!NOTE]
   > El nombre del recurso de AG en los clústeres de Pacemaker en Red Hat Enterprise Linux 8.x y Ubuntu 18.04 puede ser similar a *ag_cluster-clon*, ya que la nomenclatura relacionada con los recursos ha evolucionado para usar *clonación que se puede promocionar*. 

- **Ejemplo de RHEL/Ubuntu**

   En el comando siguiente, `cli-prefer-ag_cluster-master` es el identificador de la restricción que se debe quitar. `sudo pcs constraint list --full` devuelve este identificador. 
   
   ```bash
   sudo pcs resource clear ag_cluster-master  
   ```
   Or
   
   ```bash
   sudo pcs constraint remove cli-prefer-ag_cluster-master  
   ```
  
   Como alternativa, puede realizar tanto el movimiento como el borrado de restricciones generadas automáticamente en una sola línea, como se indica a continuación. En el ejemplo siguiente se usa la terminología *clone* según Red Hat Enterprise Linux 8.x. 
  
   ```bash
   sudo pcs resource move ag_cluster-clone --master nodeName2 && sleep 30 && sudo pcs resource clear ag_cluster-clone

   ```
  
- **Ejemplo de SLES**

   En el comando siguiente, `cli-prefer-ms-ag_cluster` es el identificador de la restricción. `crm config show` devuelve este identificador. 
   
   ```bash
   crm configure
   delete cli-prefer-ms-ag_cluster 
   commit
   ```

>[!NOTE]
>La conmutación automática por error no agrega una restricción de ubicación, por lo que no es necesaria ninguna limpieza. 

Para obtener más información:
- [Red Hat: Managing Cluster Resources](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-manageresource-HAAR.html) (Red Hat: Administración de recursos de clúster)
- [Pacemaker - Move Resources Manually (Pacemaker: mover recursos de forma manual)](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/_move_resources_manually.html)
 [SLES Administration Guide - Resources (Guía de administración de SLES: recursos)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.resource) 
 
## <a name="force-failover"></a><a name="forceFailover"></a> Forzar la conmutación por error 

La conmutación por error forzada se usa estrictamente para la recuperación ante desastres. En este caso, no se puede conmutar por error con las herramientas de administración de clústeres porque el centro de datos principal está inactivo. Si se fuerza la conmutación por error a una réplica secundaria no sincronizada, es posible que se pierdan datos. Solo ejecute una conmutación por error forzada si necesita restaurar el servicio al grupo de disponibilidad de inmediato y está dispuesto a asumir el riesgo de que se pierdan datos.

Si no puede usar las herramientas de administración de clústeres para interactuar con el clúster (por ejemplo, si el clúster no responde debido a un evento de desastre en el centro de datos principal), puede que tenga que forzar la conmutación por error para omitir el administrador de clústeres externo. Este procedimiento no se recomienda para operaciones normales, ya que existe el riesgo de que se produzca una pérdida de datos. Úselo cuando las herramientas de administración de clústeres no puedan ejecutar la acción de conmutación por error. De manera funcional, este procedimiento es similar a [ejecutar una conmutación por error manual forzada](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md) en un grupo de disponibilidad en Windows.
 
Este proceso para forzar la conmutación por error es específico de SQL Server en Linux.

1. Asegúrese de que el clúster ya no administre el recurso del grupo de disponibilidad. 

      - Establezca el recurso como el modo no administrado en el nodo del clúster de destino. Este comando indica al agente del recurso que detenga la supervisión y administración de recursos. Por ejemplo: 
      
      ```bash
      sudo pcs resource unmanage <resourceName>
      ```

      - Si se produce un error al intentar establecer el modo no administrado en el recurso, elimine el recurso. Por ejemplo:

      ```bash
      sudo pcs resource delete <resourceName>
      ```

      >[!NOTE]
      >Al eliminar un recurso, también se eliminan todas las restricciones asociadas. 

1. En la instancia de SQL Server que hospeda la réplica secundaria, establezca la variable de contexto de sesión `external_cluster`.

   ```Transact-SQL
   EXEC sp_set_session_context @key = N'external_cluster', @value = N'yes';
   ```

1. Conmute por error el grupo de disponibilidad con Transact-SQL. En el ejemplo siguiente, reemplace `<MyAg>` por el nombre de su grupo de disponibilidad. Conéctese a la instancia de SQL Server que hospeda la réplica secundaria de destino y ejecute el comando siguiente:

   ```Transact-SQL
   ALTER AVAILABILITY GROUP <MyAg> FORCE_FAILOVER_ALLOW_DATA_LOSS;
   ```

1.  Después de una conmutación por error forzada, devuelva el grupo de disponibilidad a un estado correcto; para hacerlo, reinicie las funciones de administración y supervisión de recursos del clúster, o bien recree el recurso del grupo de disponibilidad. Vea [Tareas esenciales después de una conmutación por error forzada](../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#FollowUp).

1.  Reinicie las funciones de administración y supervisión de recursos del clúster:

   Para reiniciar las funciones de administración y supervisión de recursos del clúster, ejecute el comando siguiente:

   ```bash
   sudo pcs resource manage <resourceName>
   sudo pcs resource cleanup <resourceName>
   ```

   Si ha eliminado recursos del clúster, vuelva a crearlo. Para recrear el recurso del clúster, siga las instrucciones de [Crear un recurso de grupo de disponibilidad](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).

>[!Important]
>No siga el procedimiento anterior para la obtención de detalles de recuperación ante desastres, ya que existe el riesgo de que se produzca una pérdida de datos. En su lugar, cambie la réplica asincrónica a sincrónica y siga las instrucciones para una [conmutación por error manual normal](#manualFailover).

## <a name="database-level-monitoring-and-failover-trigger"></a>Supervisar el nivel de la base de datos y desencadenar una conmutación por error

En el caso de `CLUSTER_TYPE=EXTERNAL`, la semántica para desencadenar una conmutación por error es distinta en comparación con WSFC. Cuando del grupo de disponibilidad se encuentra en una instancia de SQL Server en WSFC, la transición fuera de un estado de `ONLINE` para la base de datos causa que el estado del grupo de disponibilidad informe de un error. En respuesta, el administrador de clústeres desencadena una acción de conmutación por error. En Linux, la instancia de SQL Server no se puede comunicar con el clúster. La supervisión del estado de la base de datos se realiza *desde fuera hacia dentro*. Si el usuario ha activado la supervisión de conmutación por error de nivel de base de datos y la conmutación por error (al establecer la opción `DB_FAILOVER=ON` al crear el grupo de disponibilidad), el clúster comprobará si el estado de la base de datos es `ONLINE` cada vez que ejecute una acción de supervisión. El clúster consulta el estado en `sys.databases`. Para cualquier estado distinto de `ONLINE`, desencadenará automáticamente una conmutación por error (si se cumplen las condiciones de la conmutación automática por error). El tiempo real de la conmutación por error depende de la frecuencia de la acción de supervisión, así como del estado de la base de datos que se actualiza en sys.databases.

La conmutación automática por error necesita como mínimo una réplica sincrónica.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de clúster RHEL para el grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configuración de clústeres de SLES para grupos de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurar el clúster de Ubuntu para recursos de clúster del grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)
