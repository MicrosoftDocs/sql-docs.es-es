---
title: Configuración de copias de seguridad en las réplicas secundarias de un grupo de disponibilidad
description: Se describe cómo configurar copias de seguridad en réplicas secundarias de un grupo de disponibilidad Always On mediante Transact-SQL (T-SQL), PowerShell o SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
helpviewer_keywords:
- backup priority
- backup on secondary replicas
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], backup on secondary replicas
- active secondary replicas [SQL Server], backup on
- automated backup preference
- Availability Groups [SQL Server], active secondary replicas
ms.assetid: 74bc40bb-9f57-44e4-8988-1d69c0585eb6
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7258eff52e3614b30eeb001a5f570bafd4c7f5cc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348997"
---
# <a name="configure-backups-on-secondary-replicas-of-an-always-on-availability-group"></a>Configuración de copias de seguridad en las réplicas secundarias de un grupo de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar la copia de seguridad en réplicas secundarias para un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para obtener una introducción a la copia de seguridad en réplicas secundarias, vea [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
 Debe estar conectado a la instancia del servidor que hospeda la réplica principal.  
 
   > [!NOTE]
   > No es necesario que la réplica secundaria sea legible para descargar las copias de seguridad en esta. Las copias de seguridad seguirán realizándose correctamente en la réplica secundaria aunque `Readable Secondary` se establezca en `no`. 
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
  
|Tarea|Permisos|  
|----------|-----------------|  
|Para configurar la copia de seguridad en las réplicas secundarias al crear un grupo de disponibilidad|Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
|Para modificar un grupo de disponibilidad o una réplica de disponibilidad|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para configurar la copia de seguridad en las réplicas secundarias**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y haga clic en el nombre del servidor para expandir el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic en el grupo de disponibilidad cuyas propiedades de copia de seguridad desea configurar y seleccione el comando **Propiedades** .  
  
4.  En el cuadro de diálogo **Propiedades de grupo de disponibilidad** , seleccione la página **Preferencias de copia de seguridad** .  
  
5.  En el panel **¿Dónde se realizarán las copias de seguridad?** , seleccione la preferencia de la copia de seguridad automatizada del grupo de disponibilidad, una de las siguientes:  
  
     **Preferir secundaria**  
     Especifica que las copias de seguridad se deben realizar en una réplica secundaria a menos que la réplica principal sea la única réplica en línea. En ese caso, la copia de seguridad se debe realizar en la réplica principal. Ésta es la opción predeterminada.  
  
     **Solo secundaria**  
     Especifica que las copias de seguridad no deben realizarse nunca en la réplica principal. Si la réplica principal es la única réplica en línea, no se debe realizar la copia de seguridad.  
  
     **Principal**  
     Especifica que las copias de seguridad deben realizarse siempre en la réplica principal. Esta opción es útil si necesita usar características de copia de seguridad, como crear copias de seguridad diferenciales, que no se admiten cuando la copia de seguridad se ejecuta en una réplica secundaria.  
  
    > [!IMPORTANT]  
    >  Si piensa usar el trasvase de registros para preparar cualquier base de datos secundaria de un grupo de disponibilidad, establezca la preferencia de copia de seguridad automatizada en **Principal** hasta que todas las bases de datos secundarias se hayan preparado y combinado con el grupo de disponibilidad.  
  
     **Cualquier réplica**  
     Especifica que, de acuerdo con sus preferencias, los trabajos de copia de seguridad omitan el rol de las réplicas de disponibilidad cuando la réplica realiza copias de seguridad. Tenga en cuenta que los trabajos de copia de seguridad pueden evaluar otros factores, como la prioridad de copia de seguridad de cada réplica de disponibilidad junto con su estado operativo y de conexión.  
  
    > [!IMPORTANT]  
    >  No se aplica la configuración de preferencia de copia de seguridad automatizada. La interpretación de esta preferencia depende de la lógica, si existe, del script de los trabajos de copia de seguridad para las bases de datos de un grupo de disponibilidad dado. La configuración de preferencia de copia de seguridad automatizada no tiene ningún efecto sobre las copias de seguridad ad hoc. Para obtener más información, vea [Seguimiento: después de configurar la copia de seguridad en las réplicas secundarias](#FollowUp) más adelante en este tema.  
  
6.  Use la cuadrícula **Prioridades de copia de seguridad de réplica** para cambiar la prioridad de copia de seguridad de las réplicas de disponibilidad. Esta cuadrícula muestra la prioridad de copia de seguridad actual de cada instancia de servidor que hospeda una réplica para el grupo de disponibilidad. Las columnas de la cuadrícula son las siguientes:  
  
     **Instancia del servidor**  
     El nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica de disponibilidad.  
  
     **Prioridad de copia de seguridad (Mínima=1, Máxima=100)**  
     Especifica la prioridad para realizar copias de seguridad en esta réplica en relación con las otras réplicas del mismo grupo de disponibilidad. El valor es un número entero en el intervalo de 0..100. 1 indica la prioridad mínima y 100 indica la prioridad máxima. Si **Prioridad de copia de seguridad** = 1, la réplica de disponibilidad se elegiría para realizar copias de seguridad solamente si no hay réplicas de disponibilidad con mayor prioridad disponibles actualmente.  
  
     **Excluir réplica**  
     Seleccione esta opción si no desea que nunca se elija esta réplica de disponibilidad para realizar copias de seguridad. Esto es útil, por ejemplo, para una réplica de disponibilidad remota en la que no desee nunca realizar la conmutación por error para las copias de seguridad.  
  
7.  Para confirmar sus cambios, haga clic en **Aceptar**.  
  
 **Maneras alternativas de obtener acceso a la página Preferencias de copia de seguridad**  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usar el cuadro de diálogo Nuevo grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-new-availability-group-dialog-box-sql-server-management-studio.md)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 **Para configurar la copia de seguridad en las réplicas secundarias**  
  
1.  Conéctese a la instancia del servidor que hospeda la réplica principal.  
  
2.  Para un nuevo grupo de disponibilidad, use la instrucción [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md). Si va a modificar un grupo de disponibilidad existente, use la instrucción [ALTER AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-availability-group-transact-sql.md).  
  
##  <a name="using-powershell"></a><a name="PowerShellProcedure"></a> Usar PowerShell  
 **Para configurar la copia de seguridad en las réplicas secundarias**  
  
1.  Establezca el valor predeterminado (**cd**) en la instancia del servidor que hospeda la réplica principal.  
  
2.  Opcionalmente, configure la prioridad de copia de seguridad de cada réplica de disponibilidad que está agregando o modificando. La instancia del servidor que hospeda la réplica principal usa esta prioridad para decidir qué réplica debe atender una solicitud de copia de seguridad automatizada en una base de datos del grupo de disponibilidad (se elige la réplica con mayor prioridad). Esta prioridad puede ser cualquier número comprendido entre 0 y 100, ambos incluidos. Si la prioridad es 0, indica que la réplica no debe considerarse candidata para atender solicitudes de copia de seguridad.  El valor predeterminado es 50.  
  
     Para agregar una réplica de disponibilidad a un grupo de disponibilidad, use el cmdlet **New-SqlAvailabilityReplica** . Para modificar una réplica de disponibilidad existente, use el cmdlet **Set-SqlAvailabilityReplica** . En cualquier caso, especifique el parámetro **BackupPriority**_n_ , donde *n* es un valor entre 0 y 100.  
  
     Por ejemplo, el comando siguiente establece la prioridad de copia de seguridad de la réplica de disponibilidad `MyReplica` en **60**.  
  
    ```  
    Set-SqlAvailabilityReplica -BackupPriority 60 `  
    -Path SQLSERVER:\Sql\Computer\Instance\AvailabilityGroups\MyAg\AvailabilityReplicas\MyReplica  
    ```  
  
3.  Opcionalmente, configure la preferencia de la copia de seguridad automatizada del grupo de disponibilidad que está creando o modificando. Esta preferencia indica cómo un trabajo de copia de seguridad debe evaluar la réplica principal cuando elige dónde realizar las copias de seguridad. El valor predeterminado es preferir las réplicas secundarias.  
  
     Para crear un grupo de disponibilidad, use el cmdlet **New-SqlAvailabilityGroup** . Para modificar un grupo de disponibilidad existente, use el cmdlet **Set-SqlAvailabilityGroup** . En cualquier caso, especifique el parámetro **AutomatedBackupPreference** .  
  
     donde,  
  
     **Principal**  
     Especifica que las copias de seguridad deben realizarse siempre en la réplica principal. Esta opción es útil si necesita usar características de copia de seguridad, como crear copias de seguridad diferenciales, que no se admiten cuando la copia de seguridad se ejecuta en una réplica secundaria.  
  
    > [!IMPORTANT]  
    >  Si piensa usar el trasvase de registros para preparar cualquier base de datos secundaria de un grupo de disponibilidad, establezca la preferencia de copia de seguridad automatizada en **Principal** hasta que todas las bases de datos secundarias se hayan preparado y combinado con el grupo de disponibilidad.  
  
     **SecondaryOnly**  
     Especifica que las copias de seguridad no deben realizarse nunca en la réplica principal. Si la réplica principal es la única réplica en línea, no se debe realizar la copia de seguridad.  
  
     **Secundario**  
     Especifica que las copias de seguridad se deben realizar en una réplica secundaria a menos que la réplica principal sea la única réplica en línea. En ese caso, la copia de seguridad se debe realizar en la réplica principal. Este es el comportamiento predeterminado.  
  
     **None**  
     Especifica que, de acuerdo con sus preferencias, los trabajos de copia de seguridad omitan el rol de las réplicas de disponibilidad cuando la réplica realiza copias de seguridad. Tenga en cuenta que los trabajos de copia de seguridad pueden considerar otros factores, como la prioridad de copia de seguridad de cada réplica de disponibilidad junto con su estado operativo y de conexión.  
  
    > [!IMPORTANT]  
    >  No se aplica **AutomatedBackupPreference**. La interpretación de esta preferencia depende de la lógica, si existe, del script de los trabajos de copia de seguridad para las bases de datos de un grupo de disponibilidad dado. La configuración de preferencia de copia de seguridad automatizada no tiene ningún efecto sobre las copias de seguridad ad hoc. Para obtener más información, vea [Seguimiento: después de configurar la copia de seguridad en las réplicas secundarias](#FollowUp) más adelante en este tema.  
  
     Por ejemplo, el siguiente comando establece la propiedad **AutomatedBackupPreference** del grupo de disponibilidad `MyAg` en **SecondaryOnly**. Las copias de seguridad automatizadas de bases de datos en este grupo de disponibilidad nunca se producirán en la réplica principal, sino que se redirigirán a la réplica secundaria con la configuración de la prioridad de copia de seguridad más alta.  
  
    ```  
    Set-SqlAvailabilityGroup `  
    -Path SQLSERVER:\Sql\PrimaryServer\InstanceName\AvailabilityGroups\MyAg `  
    -AutomatedBackupPreference SecondaryOnly  
    ```  
  
> [!NOTE]  
>  Para ver la sintaxis de un cmdlet, use el cmdlet **Get-Help** en el entorno de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] PowerShell. Para más información, consulte [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md).  
  
 **Para configurar y usar el proveedor de SQL Server PowerShell**  
  
-   [Proveedor de SQL Server PowerShell Provider](../../../powershell/sql-server-powershell-provider.md)  
  
-   [Get Help SQL Server PowerShell](../../../powershell/sql-server-powershell.md)  
  
##  <a name="follow-up-after-configuring-backup-on-secondary-replicas"></a><a name="FollowUp"></a> Seguimiento: después de configurar la copia de seguridad en las réplicas secundarias  
 Para tener en cuenta la preferencia de la copia de seguridad automatizada para un grupo de disponibilidad dado, en cada instancia de servidor que hospede una réplica de disponibilidad cuya prioridad de copia de seguridad sea mayor que cero (>0), se necesitan los scripts de trabajos de copia de seguridad para las bases de datos del grupo de disponibilidad. Para determinar si la réplica actual es la réplica de copia de seguridad preferida, use la función [sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md) en el script de copia de seguridad. Si la réplica de disponibilidad que está hospedada en la instancia de servidor actual es la réplica preferida para las copias de seguridad, esta función devuelve 1. De lo contrario, la función devuelve 0. Mediante la ejecución de un script simple en cada réplica de disponibilidad que consulta esta función, puede determinar qué réplica debe ejecutar un trabajo de copia de seguridad determinado. Por ejemplo, un fragmento de código típico de un script de trabajo de copia de seguridad sería:  
  
```sql  
IF (NOT sys.fn_hadr_backup_is_preferred_replica(@DBNAME))  
BEGIN  
      Select 'This is not the preferred replica, exiting with success';  
      RETURN 0 -- This is a normal, expected condition, so the script returns success  
END  
BACKUP DATABASE @DBNAME TO DISK=<disk>  
   WITH COPY_ONLY;  
```  
  
 La generación de un script para un trabajo de copia de seguridad con esta lógica permite programar que el trabajo se ejecute en cada réplica de disponibilidad en la misma programación. Cada uno de estos trabajos examina los mismos datos para determinar qué trabajo debe ejecutarse, por lo que solamente el trabajo programado pasa a la etapa de copia de seguridad.  En el caso de una conmutación por error, no es necesario modificar ninguno de los scripts o de los trabajos. Además, si vuelve a configurar un grupo de disponibilidad para agregar una réplica de disponibilidad, la administración del trabajo de copia de seguridad solo requerirá copiar o programar el trabajo de copia de seguridad. Si quita una réplica de disponibilidad, solo tiene que eliminar el trabajo de copia de seguridad de la instancia de servidor que hospedaba esa réplica.  
  
> [!TIP]  
>  Si usa el [Asistente para planes de mantenimiento](../../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md)con el fin de crear un trabajo de copia de seguridad determinado, el trabajo incluirá automáticamente la lógica de scripting que llama a la función **sys.fn_hadr_backup_is_preferred_replica** y la comprueba. Pero el trabajo de copia de seguridad no devolverá el mensaje "Esta no es la réplica preferida…". Asegúrese de crear trabajos para cada base de datos de disponibilidad de cada instancia del servidor que hospeda una réplica de disponibilidad para el grupo de disponibilidad.  
  
##  <a name="to-obtain-information-about-backup-preference-settings"></a><a name="ForInfoAboutBuPref"></a> Para obtener información acerca de los valores de preferencia de copia de seguridad  
 Los siguientes apartados son útiles para obtener la información que es importante para la copia de seguridad en réplicas secundarias.  
  
|Ver|Information|Columnas relevantes|  
|----------|-----------------|----------------------|  
|[sys.fn_hadr_backup_is_preferred_replica](../../../relational-databases/system-functions/sys-fn-hadr-backup-is-preferred-replica-transact-sql.md)|¿Es la réplica actual la réplica de copia de seguridad preferida?|No aplicable.|  
|[sys.availability_groups](../../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)|preferencia de copia de seguridad automatizada|**automated_backup_preference**<br /><br /> **automated_backup_preference_desc**|  
|[sys.availability_replicas](../../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)|Prioridad de copia de seguridad de una réplica de disponibilidad determinada|**backup_priority**|  
|[sys.dm_hadr_availability_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md)|¿Está la réplica local en la instancia del servidor?<br /><br /> Rol actual<br /><br /> Estado operativo<br /><br /> Estado de conexión<br /><br /> Estado de sincronización de una réplica de disponibilidad|**is_local**<br /><br /> **role**, **role_desc**<br /><br /> **operational_state**, **operational_state_desc**<br /><br /> **connected_state**, **connected_state_desc**<br /><br /> **synchronization_health**, **synchronization_health_desc**|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Blog del equipo de AlwaysOn de SQL Server: blog oficial del equipo de AlwaysOn de SQL Server](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md)  
  
