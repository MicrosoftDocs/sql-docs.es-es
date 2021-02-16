---
title: Ejecución de una conmutación por error de un grupo de disponibilidad forzada manualmente
description: En este tema se describe cómo realizar una conmutación por error manual forzada de un grupo de disponibilidad Always On (con posible pérdida de datos) mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.availabilitygroup.forcefailover.f1
helpviewer_keywords:
- Availability Groups [SQL Server], failover
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 222288fe-ffc0-4567-b624-5d91485d70f0
author: cawrites
ms.author: chadam
ms.openlocfilehash: a2f5c7236bc7ab6bc7510f7209dc2987c779ae57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350795"
---
# <a name="perform-a-forced-manual-failover-of-an-always-on-availability-group-sql-server"></a>Realización de una conmutación por error manual forzada de un grupo de disponibilidad Always On (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo realizar una conmutación por error forzada (con posible pérdida de datos) en un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Una conmutación por error forzada es una forma de conmutación por error manual pensada estrictamente para la recuperación ante desastres, cuando no es posible realizar una [conmutación por error manual planeada](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md) . Si se fuerza la conmutación por error a una réplica secundaria no sincronizada, es posible que se pierdan datos. Por tanto, se recomienda encarecidamente que solo fuerce la conmutación por error si debe restaurar el servicio al grupo de disponibilidad inmediatamente y asume el riesgo de perder datos.  
  
 Después de una conmutación por error forzada, el destino de la conmutación por error al que se aplicó la conmutación por error del grupo de disponibilidad se convierte en la nueva réplica principal. Las bases de datos secundarias de las réplicas secundarias restantes se suspenden y se deben reanudar manualmente. Cuando la réplica principal anterior está disponible, realiza la transición al rol secundario, haciendo que las bases de datos principales anteriores se conviertan en bases de datos secundarias y realicen la transición al estado SUSPENDED. Antes de reanudar una base de datos secundaria determinada, quizás pueda recuperar datos perdidos de ella. Sin embargo, tenga en cuenta que el truncamiento del registro de transacciones se retrasa en una base de datos principal determinada mientras cualquiera de sus bases de datos secundarias está suspendida.  
  
> [!IMPORTANT]  
>  La sincronización de datos con la base de datos principal no se realiza hasta que no se reanuda la base de datos secundaria. Para obtener información sobre cómo reanudar una base de datos secundaria, vea [Seguimiento: tareas esenciales después de una conmutación por error forzada](#FollowUp), más adelante en este artículo.  
  
 Es necesario realizar una conmutación por error forzada en las situaciones de emergencia siguientes:  
  
-   Después de forzar el cuórum en el clúster de WSFC (*cuórum forzado*), debe forzar la conmutación por error de todos los grupos de disponibilidad (con posible pérdida de datos). Es necesario forzar la conmutación por error porque el estado real de los valores del clúster de WSFC puede haberse perdido. Sin embargo, puede evitar la pérdida de datos si puede forzar la conmutación por error en la instancia del servidor que hospedaba la réplica que era la réplica principal antes de forzar el quórum o en una réplica secundaria que se sincronizó antes de que se forzara el quórum. Para obtener más información, vea [Formas posibles de evitar la pérdida de datos después de forzar el quórum](#WaysToAvoidDataLoss), más adelante en este tema.  
  
    > [!IMPORTANT]  
    >  Si el quórum se recupera por medios naturales en lugar de forzarse, las réplicas de disponibilidad pasarán por una recuperación normal. Si la réplica principal sigue sin estar disponible después de que se recupere el quórum, puede realizar una conmutación por error manual planeada en una réplica secundaria sincronizada.  
  
     Para obtener información sobre cómo forzar el cuórum, vea [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md). Para obtener información sobre por qué es necesario forzar la conmutación por error después de forzar el cuórum, vea [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
-   Si la réplica principal deja de estar disponible cuando el clúster de WSFC tiene un quórum en buen estado, puede forzar la conmutación por error (con posible pérdida de datos) a cualquier réplica cuyo rol esté en el estado SECONDARY o RESOLVING. Si es posible, fuerce la conmutación por error a una réplica secundaria de confirmación sincrónica que se sincronizó cuando se perdió la réplica principal.  
  
    > [!TIP]  
    >  Cuando el clúster de WSFC tiene un quórum en buen estado, si emite un comando para forzar la conmutación por error en una réplica secundaria sincronizada, la réplica realiza realmente una conmutación por error manual planeada.  
  
> [!NOTE]  
>  Para más información sobre los requisitos previos y las recomendaciones para forzar la conmutación por error y consultar un escenario de ejemplo en el que se usa una conmutación por error forzada para recuperarse de un error grave, vea [Escenario de ejemplo: uso de una conmutación por error forzada para recuperarse de un error catastrófico](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md#ExampleRecoveryFromCatastrophy), más adelante en este tema.  
  
  
##  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   La única vez que no se puede realizar una conmutación por error forzada es cuando el clúster de Clústeres de conmutación por error de Windows Server (WSFC) no tiene quórum.  
  
-   La pérdida de datos es posible durante la conmutación por error forzada de un grupo de disponibilidad. Además, si la réplica principal se ejecuta cuando se inicia una conmutación por error forzada, los clientes pueden seguir estando conectados a las bases de datos principales anteriores. Por lo tanto, es absolutamente recomendable que solo fuerce la conmutación por error si la réplica principal ya no se está ejecutando y si asume el riesgo de perder datos para restaurar el acceso a las bases de datos del grupo de disponibilidad.  
  
-   Cuando una base de datos secundaria está en estado REVERTING o INITIALIZING, la acción de forzar la conmutación por error hará que la base de datos no se pueda iniciar como base de datos principal. Si la base de datos estaba en estado INITIALIZING, será necesario aplicar las entradas de registro que faltan desde una copia de seguridad de la base de datos o restaurar totalmente la base de datos desde el principio. Si la base de datos estaba en estado REVERTING será necesario restaurar totalmente la base de datos desde las copias de seguridad.  
  
-   Un comando de conmutación por error realiza la devolución en cuanto el destino de la conmutación por error acepte el comando. Sin embargo, la recuperación de la base de datos se produce de forma asincrónica cuando el grupo de disponibilidad ha terminado la conmutación por error.  
  
-   Puede que la coherencia entre las distintas bases de datos del grupo de disponibilidad no se mantenga en la conmutación por error.  
  
    > [!NOTE]  
    >  La compatibilidad con transacciones distribuidas y entre bases de datos varía según la versión de SQL Server y del sistema operativo. Para obtener más información, vea [Transacciones entre bases de datos y transacciones distribuidas para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   El clúster de WSFC tiene quórum. Si el clúster no tiene cuórum, vea [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md).  
  
-   Debe poder conectarse a una instancia de servidor que hospeda una replicación cuyo rol esté en el estado SECONDARY o RESOLVING.  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   No fuerce la conmutación por error mientras la réplica principal sigue en ejecución.  
  
-   Si es posible, fuerce la conmutación por error solo a un destino de conmutación por error cuyas bases de datos secundarias estén en estado NOT SYNCHRONIZED, SYNCHRONIZED o SYNCHRONIZING. Para obtener información sobre las implicaciones de forzar la conmutación por error cuando una base de datos secundaria está en estado INITIALIZING o REVERTING, vea [Limitaciones y restricciones](#Restrictions), anteriormente en este tema.  
  
-   Generalmente, la latencia de una base de datos secundaria determinada, relativa a la base de datos principal, debería ser similar en réplicas secundarias de confirmación asincrónica diferentes. Sin embargo, al forzar una conmutación por error, la pérdida de datos podría ser un riesgo significativo. Por tanto, dedique el tiempo necesario para determinar la latencia relativa de las copias de las bases de datos en réplicas secundarias diferentes. Para determinar qué copia de una base de datos secundaria determinada tiene la menor latencia, compare sus LSN del final del registro. Cuanto mayor es el LSN del final del registro, menor latencia.  
  
    > [!TIP]  
    >  Para comparar los LSN del final del registro, conéctese a cada réplica secundaria en línea por turnos y consulte [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) para ver el valor de **end_of_log_lsn** de cada base de datos secundaria local. Después, compare los LSN del final del registro de las diferentes copias de cada base de datos. Observe que diferentes bases de datos podrían tener sus LSN más altos en diferentes réplicas secundarias. En este caso, el destino de conmutación por error más apropiado depende de la importancia relativa que le dé a los datos de las distintas bases de datos. Es decir, ¿para cuál de estas bases de datos desearía más minimizar la posible pérdida de datos?  
  
-   Si los clientes pueden conectarse a la réplica principal original, una conmutación por error forzada comporta el riesgo de "comportamiento de división de cerebro". Antes de forzar la conmutación por error, se recomienda encarecidamente impedir el acceso de los clientes a la réplica principal original. De lo contrario, una vez forzada la conmutación por error, las bases de datos principales originales y las bases de datos principales actuales pueden actualizarse independientemente.  
  
##  <a name="potential-ways-to-avoid-data-loss-after-quorum-is-forced"></a><a name="WaysToAvoidDataLoss"></a> Formas posibles de evitar la pérdida de datos después de forzar el quórum  
 En determinadas condiciones de error después de que se pierda el quórum, puede evitar evita la pérdida de datos de la manera siguiente:  
  
-   **Si la réplica principal original pasa a estar en línea**  
  
     Si se perdió el quórum y al forzar el quórum de WSFC se restaura el nodo del clúster que hospeda la réplica principal de un grupo de disponibilidad, puede evitar la pérdida de datos para este grupo de disponibilidad. Conéctese a la réplica principal y realice una conmutación por error forzada (FAILOVER_ALLOW_DATA_LOSS). Esto hace que la réplica principal vuelva a estar en línea. Como realiza la conmutación por error forzada en la réplica principal original, no se pierde ningún dato.  
  
-   **Si una réplica secundaria sincronizada con confirmación sincrónica pasa a estar en línea**  
  
     Si se perdió el quórum y al forzar el quórum de WSFC se restaura un nodo del clúster que hospeda una réplica secundaria sincronizada para un grupo de disponibilidad, debe poder impedir la pérdida de datos para este grupo de disponibilidad. Si el nodo restaurado estaba activo en el momento en que se perdió el cuórum, puede determinar si se pudieron perder datos en una base de datos determinada consultando la columna **is_failover_ready** de la vista de administración dinámica [sys.dm_hadr_database_replica_cluster_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-cluster-states-transact-sql.md) . Por ejemplo, para una instancia de servidor denominada `sql108w2k8r22`, ejecute la consulta siguiente:  
  
    ```  
    SELECT * FROM sys.dm_hadr_database_replica_cluster_states  
       WHERE replica_id=(SELECT replica_id FROM sys.availability_replicas   
          WHERE replica_server_name ='sql108w2k8r22')  
    ```  
  
    > [!CAUTION]  
    >  Si el nodo restaurado no estaba activo en el momento en que se perdió el cuórum, puede que **is_failover_ready** no refleje el estado real del clúster en el momento en que la réplica principal pasó a estar sin conexión. Por tanto, el valor **is_failover_ready** solo es adecuado si el nodo del host estaba activo en el momento del error. Para obtener más información, vea "Por qué se requiere la conmutación por error después de forzar el cuórum" en [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
     Si **is_failover_ready** = 1, la base de datos está marcada como sincronizada en el clúster y está preparada para una conmutación por error. Si **is_failover_ready** = 1 en todas las bases de datos de una réplica secundaria determinada, puede realizar una conmutación por error forzada (FORCE_FAILOVER_ALLOW_DATA_LOSS) sin pérdida de datos en esta réplica secundaria. La réplica secundaria sincronizada se pone en línea en el rol principal; es decir, como la nueva réplica principal, con todos los datos intactos.  
  
     Si **is_failover_ready** = 0, la base de datos no está marcada como sincronizada en el clúster y *no* está preparada para una conmutación por error manual planeada. Si se fuerza la conmutación por error en la réplica secundaria del host, se perderán datos en esta base de datos.  
  
    > [!NOTE]  
    >  Cuando se fuerza la conmutación por error en una réplica secundaria, la cantidad de datos perdidos depende de hasta dónde se retrase el destino de la conmutación por error detrás de la réplica principal. Desgraciadamente, cuando el clúster de WSFC no tiene quórum o se ha forzado el quórum, no puede evaluar la cantidad de pérdida potencial de datos. No obstante, tenga en cuenta que una vez que el clúster de WSFC recupere un quórum en buen estado, puede empezar a hacer un seguimiento de la posible pérdida de datos. Para obtener más información, vea "Seguimiento de la posible pérdida de datos" en [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para forzar la conmutación por error (con posible pérdida de datos)**  
  
1.  En el Explorador de objetos, conéctese a una instancia de servidor que hospede una réplica cuyo rol esté en el estado SECONDARY o RESOLVING en el grupo de disponibilidad que necesita ser objeto de conmutación por error y expanda el árbol de servidores.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic con el botón derecho en el grupo de disponibilidad que va a ser objeto de conmutación por error y seleccione el comando **Conmutación por error** .  
  
4.  Esto inicia el Asistente para conmutación por error del grupo de disponibilidad. Para obtener más información, vea [Usar el Asistente para grupo de disponibilidad de conmutación por error &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md).  
  
5.  Después de forzar un grupo de disponibilidad a la conmutación por error, complete los pasos necesarios de seguimiento. Para más información, vea [Seguimiento: tareas esenciales después de una conmutación por error forzada](#FollowUp), más adelante en este tema.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para forzar la conmutación por error (con posible pérdida de datos)**  
  
1.  Conéctese a una instancia de servidor que hospede una réplica cuyo rol esté en el estado SECONDARY o RESOLVING en el grupo de disponibilidad que necesita ser objeto de conmutación por error.  
  
2.  Use la instrucción [ALTER AVAILABILITY GROUP](../../../t-sql/statements/alter-availability-group-transact-sql.md) del siguiente modo:  
  
     ALTER AVAILABILITY GROUP *group_name* FORCE_FAILOVER_ALLOW_DATA_LOSS  
  
     donde *group_name* es el nombre del grupo de disponibilidad.  
  
     En el ejemplo siguiente se fuerza la conmutación por error del grupo de disponibilidad `AccountsAG` a la réplica secundaria local.  
  
    ```  
    ALTER AVAILABILITY GROUP AccountsAG FORCE_FAILOVER_ALLOW_DATA_LOSS;  
    ```  
  
3.  Después de forzar un grupo de disponibilidad a la conmutación por error, complete los pasos necesarios de seguimiento. Para más información, vea [Seguimiento: tareas esenciales después de una conmutación por error forzada](#FollowUp), más adelante en este tema.  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para forzar la conmutación por error (con posible pérdida de datos)**  
  
1.  Cambie el directorio (**cd**) a una instancia de servidor que hospede una réplica cuyo rol se encuentre en el estado SECONDARY o RESOLVING en el grupo de disponibilidad para el que se debe realizar la conmutación por error.  
  
2.  Use el cmdlet **Switch-SqlAvailabilityGroup** con el parámetro **AllowDataLoss** en una de las siguientes formas:  
  
    -   **-AllowDataLoss**  
  
         De forma predeterminada, el parámetro **-AllowDataLoss** hace que **Switch-SqlAvailabilityGroup** muestre un mensaje para recordarle que forzar la conmutación por error podría provocar la pérdida de transacciones no confirmadas y para pedir confirmación. Para continuar, especifique **Y**. Para cancelar la operación, especifique **N**.  
  
         En el ejemplo siguiente se realiza una conmutación por error forzada (con posible pérdida de datos) del grupo de disponibilidad `MyAg` a la réplica secundaria en la instancia del servidor denominada `SecondaryServer\InstanceName`. Se le pedirá que confirme esta operación.  
  
        ```  
        Switch-SqlAvailabilityGroup `  
           -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg `  
           -AllowDataLoss  
        ```  
  
    -   **-AllowDataLoss-Force**  
  
         Para iniciar una conmutación por error forzada sin confirmación, especifique los parámetros **-AllowDataLoss** y **-Force** . Esto es útil si desea incluir el comando en un script y ejecutarlo sin la intervención del usuario.  Pero use la opción **-Force** con precaución, ya que una conmutación por error forzada podría provocar la pérdida de datos de las bases de datos que forman parte del grupo de disponibilidad.  
  
         En el ejemplo siguiente se realiza una conmutación por error forzada (con posible pérdida de datos) del grupo de disponibilidad `MyAg` a la instancia del servidor denominada `SecondaryServer\InstanceName`. La opción **-Force** suprime la confirmación de esta operación.  
  
        ```  
        Switch-SqlAvailabilityGroup `  
           -Path SQLSERVER:\Sql\SecondaryServer\InstanceName\AvailabilityGroups\MyAg `  
           -AllowDataLoss -Force  
        ```  
  
    > [!NOTE]  
    >  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
3.  Después de forzar un grupo de disponibilidad a la conmutación por error, complete los pasos necesarios de seguimiento. Para más información, vea [Seguimiento: tareas esenciales después de una conmutación por error forzada](#FollowUp), más adelante en este tema.  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
##  <a name="follow-up-essential-tasks-after-a-forced-failover"></a><a name="FollowUp"></a> Seguimiento: Tareas esenciales después de una conmutación por error forzada  
  
1.  Después de una conmutación por error forzada, la réplica secundaria objeto de conmutación por error se convierte en la nueva réplica principal. Sin embargo, para que la réplica de disponibilidad esté accesible a los clientes, puede que necesite volver a configurar el quórum de WSFC o ajustar la configuración de modo de disponibilidad del grupo de disponibilidad del siguiente modo:  
  
    -   **Si ha conmutado por error fuera de la [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)]:**  ajuste los votos de quórum de los nodos WSFC para reflejar la configuración del nuevo grupo de disponibilidad. Si el nodo de WSFC que hospeda la réplica secundaria de destino no tiene un voto de quórum de WSFC, puede que sea necesario forzar el quórum de WSFC.  
  
        > [!NOTE]  
        >  [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] solo existe si dos réplicas de disponibilidad (incluida la réplica principal anterior) están configuradas para el modo de confirmación sincrónica con conmutación por error automática.  
  
         **Para ajustar los votos de quórum**  
  
        -   [Ver la configuración de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
        -   [Configurar los valores de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
        -   [Forzar el inicio de un clúster WSFC sin un quórum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
    -   **Si ha conmutado por error fuera de la [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)]:** Se recomienda que considere la posibilidad de ajustar el modo de disponibilidad y el modo de conmutación por error en la nueva réplica principal y en las réplicas secundarias restantes para que se reflejen la confirmación sincrónica deseada y la configuración de conmutación por error automática.  
  
        > [!NOTE]  
        >  [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)] solo existe si la réplica principal actual se configura para el modo de confirmación sincrónica.  
  
         **Para cambiar el modo de disponibilidad y el modo de conmutación por error**  
  
        -   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
        -   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
2.  Después de una conmutación por error forzada, se suspenden todas las bases de datos secundarias. Esto incluye las bases de datos principales antiguas, después de que la réplica principal anterior vuelva a estar en línea y detecte que ahora es una réplica secundaria. Debe reanudar manualmente y de forma individual cada una de las bases de datos suspendidas en cada réplica secundaria.  
  
     Cuando se reanuda una base de datos secundaria, inicia la sincronización de datos con la base de datos principal correspondiente. La base de datos secundaria solo revierte algunos registros que nunca se confirmaron en la nueva base de datos principal. Por tanto, si le preocupa la posible pérdida de datos en las bases de datos principales posteriores a la conmutación por error, debe intentar crear una instantánea de base de datos en las bases de datos suspendidas en una de las bases de datos secundarias de confirmación sincrónica.  
  
    > [!IMPORTANT]  
    >  El truncamiento del registro de transacciones se retrasa en una base de datos principal mientras cualquiera de sus bases de datos secundarias está suspendida. Además, el estado de sincronización de una réplica secundaria de confirmación sincrónica no puede pasar a HEALTHY siempre que alguna base de datos local se suspende.  
  
     **Para crear una instantánea de base de datos**  
  
    -   [Crear una instantánea de base de datos &#40;Transact-SQL&#41;](../../../relational-databases/databases/create-a-database-snapshot-transact-sql.md)  
  
     **Para reanudar una base de datos de disponibilidad**  
  
    -   [Reanudar una base de datos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)  
  
    > [!CAUTION]  
    >  Después de reanudar todas las bases de datos secundarias, antes de intentar volver a realizar la conmutación por error del grupo, espere a que cada base de datos secundaria del destino de conmutación por error siguiente entre en el estado SYNCHRONIZING. Si una base de datos no está todavía en estado SYNCHRONIZING, se impedirá que esa base de datos entre en línea como base de datos principal, y el restablecimiento de la sincronización de datos para la base de datos podría requerir que se restaurasen los registros de transacciones, se restaurase una copia de seguridad completa de la base de datos o se realizase de nuevo la conmutación por error a la réplica principal anterior.  
  
3.  Si una réplica de disponibilidad que generó un error no se devolviera a la réplica de disponibilidad o se devolviera demasiado tarde para poder retrasar el truncamiento del registro de transacciones en la nueva base de datos principal, considere la posibilidad de quitar la réplica con problemas del grupo de disponibilidad para que los archivos de registro no se queden sin espacio en disco.  
  
     **Para quitar una réplica secundaria**  
  
    -   [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)  
  
4.  Si se realiza una conmutación por error forzada después de una o varias conmutaciones por error forzadas adicionales, realice una copia de seguridad de registros después de cada conmutación por error forzada adicional de la serie. Para obtener más información sobre la razón de ello, vea "Riesgos de forzar la conmutación por error" en la sección "Conmutación por error forzada (con posible pérdida de datos)" de [Conmutación por error y modos de conmutación por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
     **Para realizar una copia de seguridad de registros**  
  
    -   [Realizar una copia de seguridad de un registro de transacciones &#40;SQL Server&#41;](../../../relational-databases/backup-restore/back-up-a-transaction-log-sql-server.md)  
  
##  <a name="example-scenario-using-a-forced-failover-to-recover-from-a-catastrophic-failure"></a><a name="ExampleRecoveryFromCatastrophy"></a> Escenario de ejemplo: Uso de una conmutación por error forzada para recuperarse de un error catastrófico  
 En el ejemplo siguiente se realiza manualmente una conmutación por error en el grupo de disponibilidad a la réplica secundaria local que está sincronizada actualmente con la réplica principal. La idoneidad de forzar una conmutación por error depende de: (1) si espera que la réplica principal esté sin conexión más tiempo de lo que el contrato de nivel de servicio (SLA) tolera y (2) si está dispuesto a exponerse a la posible pérdida de datos para conseguir que las bases de datos principales estén disponibles de inmediato. Si decide que un grupo de disponibilidad requiere una conmutación por error forzada, la conmutación por error forzada real no es sino uno de los pasos de un proceso de varios.  
  
 Para ilustrar los pasos necesarios para usar una conmutación por error forzada para recuperarse de un error catastrófico, en este tema se presenta un posible escenario de recuperación de desastres. El escenario de ejemplo considera un grupo de disponibilidad cuya topología original consta de un centro de datos principal que hospeda tres réplicas de disponibilidad de confirmación sincrónica, incluida la réplica principal, y un centro de datos remoto que hospeda dos réplicas secundarias de confirmación asincrónica. En la ilustración siguiente se muestra la topología original de este grupo de disponibilidad de ejemplo. El grupo de disponibilidad está hospedado por un clúster WSFC de múltiples subredes con tres nodos en el centro de datos principal (**Nodo 01**, **Nodo 02** y **Nodo 03**) y dos nodos en un centro de datos remoto (**Nodo 04** y **Nodo 05**).  
  
 ![Topología original del grupo de disponibilidad](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-origtopology.gif "Topología original del grupo de disponibilidad")  
  
 El centro de datos principal se apaga inesperadamente. Sus tres réplicas de disponibilidad se ponen sin conexión y sus bases de datos dejan de estar disponibles. En la ilustración siguiente se muestra el impacto de este error en la topología original del grupo de disponibilidad.  
  
 ![Topología después del error del centro de datos principal](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-catastrophy.gif "Topología después del error del centro de datos principal")  
  
 El administrador de la base de datos (DBA) determina que la mejor respuesta posible es forzar la conmutación por error del grupo de disponibilidad a una de las réplicas secundarias de confirmación asincrónica. En este ejemplo se muestran los pasos típicos implicados cuando se fuerza la conmutación por error del grupo de disponibilidad a una réplica remota y, a la larga, devuelve el grupo de disponibilidad a su topología original.  
  
 La respuesta al error presentada aquí consta de las dos fases siguientes:  
  
-   [Responder al error catastrófico del centro de datos principal](#FailureResponse)  
  
-   [Devolver el grupo de disponibilidad a su topología original](#ReturnToOrigTopology)  
  
###  <a name="responding-to-the-catastrophic-failure-of-the-main-data-center"></a><a name="FailureResponse"></a> Responding to the Catastrophic Failure of the Main Data Center  
 En la ilustración siguiente se muestra la serie de acciones realizadas en el centro de datos remoto en respuesta a un error catastrófico en el centro de datos principal.  
  
 ![Pasos para responder al error del centro de datos principal](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-actions-part1.gif "Pasos para responder al error del centro de datos principal")  
  
 Los pasos de esta ilustración indican los pasos siguientes:  
  
|Paso|Acción|Vínculos|  
|----------|------------|-----------|  
|**1.**|El administrador de red o DBA se asegura de que el clúster WSF tiene un quórum correcto. En este ejemplo, el quórum tiene que forzarse.|[Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md)<br /><br /> [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)|  
|**2.**|El DBA se conecta a la instancia del servidor con la menor latencia (en el **Nodo 04**) y realiza una conmutación por error manual forzada. Esta conmutación por error forzada pasa esta réplica secundaria al rol principal y suspende las bases de datos secundarias en el resto de réplicas secundarias (en el **Nodo 05**).|[sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) (Consulte la columna **end_of_log_lsn** . Para obtener más información, vea [Recomendaciones](#Recommendations), anteriormente en este tema).|  
|**3.**|El DBA reanuda de modo manual cada una de las bases de datos secundarias en la réplica secundaria restante.|[Reanudar una base de datos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)|  
  
###  <a name="returning-the-availability-group-to-its-original-topology"></a><a name="ReturnToOrigTopology"></a> Devolver el grupo de disponibilidad a su topología original  
 En la ilustración siguiente se muestra la serie de acciones que devuelven el grupo de disponibilidad a su topología original una vez que el centro de datos principal se pone en línea y los nodos WSFC restablecen la comunicación con el clúster WSFC.  
  
> [!IMPORTANT]  
>  Si el quórum del clúster WSFC ha sido forzado, a medida que los nodos sin conexión se reinicien, podrían formar un nuevo quórum si se dan las dos condiciones siguientes: (a) no hay conectividad de red entre ninguno de los nodos en el conjunto de quórum forzado y (b) los nodos que se reinician son la mayoría de los nodos del clúster. Eso resultaría en una condición de "división de cerebro" en la que el grupo poseería dos réplicas principales independientes, una en cada centro de datos. Antes de forzar el cuórum para crear un conjunto de cuórum minoritario, vea [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md).  
  
 ![Pasos para devolver el grupo a su topología original](../../../database-engine/availability-groups/windows/media/aoag-failurerecovery-actions-part2.gif "Pasos para devolver el grupo a su topología original")  
  
 Los pasos de esta ilustración indican los pasos siguientes:  
  
||Paso|Vínculos|  
|-|----------|-----------|  
|**1.**|Los nodos del centro de datos principal se ponen de nuevo en línea y se vuelve a establecer la comunicación con el clúster WSFC. Sus réplicas de disponibilidad se ponen en línea como réplicas secundarias con las bases de datos suspendidas y el administrador de base de datos tendrá que reanudar manualmente cada una de estas bases de datos pronto.|[Reanudar una base de datos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/resume-an-availability-database-sql-server.md)<br /><br /> Sugerencia: Si le preocupa la posible pérdida de datos en las bases de datos principales tras la conmutación por error, debería intentar crear una instantánea de base de datos en las bases de datos suspendidas en una de las bases de datos secundarias de confirmación sincrónica. Tenga en cuenta que el truncamiento del registro de transacciones se retrasa en una base de datos principal mientras cualquiera de sus bases de datos secundarias se suspende. Además, el estado de sincronización de la réplica secundaria de confirmación sincrónica no puede pasar a HEALTHY siempre que alguna base de datos local se suspende.|  
|**2.**|Un vez que las bases de datos se reanudan, el administrador de base de datos cambia temporalmente la nueva réplica principal al modo de confirmación sincrónica. Esto conlleva dos pasos:<br /><br /> 1) Cambiar una réplica de disponibilidad sin conexión al modo de confirmación asincrónica.<br /><br /> 2) Cambiar la nueva réplica principal al modo de confirmación sincrónica. Nota: Este paso habilita las bases de datos secundarias de confirmación sincrónica para que se sincronicen.|[Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)|  
|**3.**|Una vez que la réplica secundaria de confirmación sincrónica del **Nodo 03** (la réplica principal original) entra en el estado de sincronización HEALTHY, el administrador de base de datos realiza una conmutación por error manual planeada en la réplica, para convertirla de nuevo en la principal. La réplica en el **Nodo 04** vuelve a ser una réplica secundaria.|[sys.dm_hadr_database_replica_states &#40;Transact-SQL&#41;](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md)<br /><br /> [Usar directivas de AlwaysOn para ver el estado de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md)<br /><br /> [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)|  
|**4.**|El administrador de base de datos se conecta con la nueva réplica principal y:<br /><br /> 1) Cambia la anterior réplica principal (del centro remoto) de nuevo al modo de confirmación asincrónica.<br /><br /> 2) Cambia la réplica secundaria de confirmación asincrónica en el centro de datos principal de nuevo al modo de confirmación sincrónica.|[Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)|  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para ajustar los votos de quórum**  
  
-   [Ver la configuración de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurar los valores de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Forzar el inicio de un clúster WSFC sin un quórum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
 **Conmutación por error manual planeada:**  
  
-   [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Usar el Asistente para grupo de disponibilidad de conmutación por error &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
 **Para solucionar problemas:**  
  
-   [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Solucionar problemas relativos a una operación de agregar archivos con error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   **Blogs:**  
  
     [Blogs del equipo de Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
     [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
-   **Notas del producto:**  
  
     [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Notas del producto de Microsoft para SQL Server 2012](https://social.technet.microsoft.com/wiki/contents/articles/13146.white-paper-gallery-for-sql-server.aspx#[Category]SQLServer2012)  
  
     [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)   
 [Supervisión de los grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/monitoring-of-availability-groups-sql-server.md)   
 [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
