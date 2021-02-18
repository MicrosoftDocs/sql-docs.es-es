---
title: 'Grupo de disponibilidad: Requisitos previos, restricciones y recomendaciones'
description: Descripción de los requisitos previos, las restricciones y las recomendaciones para implementar un grupo de disponibilidad AlwaysOn en SQL Server.
ms.custom: seo-lt-2019
ms.date: 07/22/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], server instance
- Availability Groups [SQL Server], deploying
- Availability Groups [SQL Server], WSFC clusters
- Availability Groups [SQL Server], about
- Availability Groups [SQL Server], prerequisites and restrictions
- Availability Groups [SQL Server], Failover Cluster Instances
- Availability Groups [SQL Server], databases
- Availability Groups [SQL Server]
ms.assetid: edbab896-42bb-4d17-8d75-e92ca11f7abb
author: cawrites
ms.author: chadam
ms.openlocfilehash: 571cf683c8d905cfb499624cbd05dc40efe94b86
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100344645"
---
# <a name="prerequisites-restrictions-and-recommendations-for-always-on-availability-groups"></a>Requisitos previos, restricciones y recomendaciones para grupos de disponibilidad Always On
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  En este artículo se describen las consideraciones necesarias para implementar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], incluidos requisitos previos, restricciones y recomendaciones relativos a los equipos host, los clústeres de conmutación por error de Windows Server (WSFC), las instancias del servidor y los grupos de disponibilidad. Para cada uno de estos componentes, se indican los criterios de seguridad y permisos necesarios, si existen.  
  
> [!IMPORTANT]  
>  Antes de implementar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], le recomendamos encarecidamente que lea todas las secciones de este tema.  
    
##  <a name="net-hotfixes-that-support-availability-groups"></a><a name="DotNetHotfixes"></a> Revisiones de .NET que admiten Grupos de disponibilidad  
 Según los componentes y características de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] que use con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], puede que tenga que instalar las revisiones de .NET adicionales identificadas en la tabla siguiente. Las revisiones pueden instalarse en cualquier orden.  
  
|Característica dependiente|Revisión|Vínculo|  
|-----------------------|------------|----------|  
|[!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]|La revisión para .NET 3.5 SP1 incorpora compatibilidad con las características cliente SQL para AlwaysOn de intención de lectura, de solo lectura y de conmutación por error de múltiples subredes. La revisión tiene que instalarse en cada servidor de informes [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .|KB 2654347: [Revisión para .NET 3.5 SP1 para agregar compatibilidad con las características de Always On](https://go.microsoft.com/fwlink/?LinkId=242896)|  
  

###  <a name="checklist-requirements-windows-system"></a><a name="SystemRequirements"></a> Lista de comprobación: Requisitos (sistema de Windows)  
 Para admitir la característica de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , asegúrese de que cada equipo que vaya a participar en uno o varios grupos de disponibilidad cumpla los requisitos básicos siguientes:  
  
|Requisito|Vínculo|  
|-----------------|----------|  
|Asegúrese de que el sistema no es un controlador de dominio.|No se admiten grupos de disponibilidad en los controladores de dominio.|  
|Procure que cada equipo ejecute Windows Server 2012 o una versión posterior.|[Requisitos de hardware y software para instalar SQL Server 2016](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)|  
|Asegúrese de que cada equipo es un nodo en un WSFC.|[Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)|  
|Asegúrese de que el WSFC contiene los nodos suficientes para admitir las configuraciones de grupo de disponibilidad.|Un nodo de clúster puede hospedar una réplica para un grupo de disponibilidad. El mismo nodo no puede hospedar dos réplicas del mismo grupo de disponibilidad. El nodo del clúster puede participar en varios grupos de disponibilidad, con una réplica de cada grupo. <br /><br /> Pregunte a los administradores de bases de datos cuántos nodos de clúster se requieren para admitir las réplicas de disponibilidad de los grupos de disponibilidad planeados.<br /><br /> [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).|  

  
> [!IMPORTANT]  
>  Asegúrese también de que el entorno esté configurado correctamente para conectarse a un grupo de disponibilidad. Para obtener más información, vea [Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="recommendations-for-computers-that-host-availability-replicas-windows-system"></a><a name="ComputerRecommendations"></a> Recomendaciones para equipos que hospedan réplicas de disponibilidad (sistema de Windows)  
  
-   **Sistemas comparables:**  En el caso de un grupo de disponibilidad determinado, todas las réplicas de disponibilidad se deben ejecutar en sistemas comparables que puedan controlar cargas de trabajo idénticas.  
  
-   **Adaptadores de red dedicados:**  Para obtener el mejor rendimiento, utilice un adaptador de red (tarjeta de interfaz de red) dedicado para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Espacio suficiente en disco:**  todos los equipos en los que una instancia del servidor hospede una réplica de disponibilidad deben tener suficiente espacio en disco para todas las bases de datos del grupo de disponibilidad. Tenga en cuenta que según crecen las bases de datos principales, las correspondientes bases de datos secundarias aumentan la misma cantidad.  
  
###  <a name="permissions-windows-system"></a><a name="PermissionsWindows"></a> Permisos (sistema de Windows)  
 Para administrar un WSFC, el usuario debe ser administrador del sistema en cada nodo de clúster.  
  
 Para más información sobre la cuenta para administrar el clúster, vea [Apéndice A: Requisitos de clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197454(v=ws.10)).  
  
###  <a name="related-tasks-windows-system"></a><a name="RelatedTasksWindows"></a> Tareas relacionadas (sistema de Windows)  
  
|Tarea|Vínculo|  
|----------|----------|  
|Establecer el valor de HostRecordTTL.|[Cambiar el valor de HostRecordTTL (con Windows PowerShell)](#ChangeHostRecordTTLps)|  
  
####  <a name="change-the-hostrecordttl-using-windows-powershell"></a><a name="ChangeHostRecordTTLps"></a> Cambiar el valor de HostRecordTTL (con Windows PowerShell)  
  
1.  Abra la ventana de PowerShell mediante **Ejecutar como administrador**.  
  
2.  Importe el módulo FailoverClusters.  
  
3.  Use el cmdlet **Get-ClusterResource** para encontrar el recurso Nombre de red y, después, use el cmdlet **Set-ClusterParameter** para establecer el valor **HostRecordTTL** de la siguiente manera:  
  
     Get-ClusterResource " *\<NetworkResourceName>* " | Set-ClusterParameter HostRecordTTL *\<TimeInSeconds>*  
  
     En el siguiente ejemplo de PowerShell se establece el HostRecordTTL en 300 segundos en un recurso de nombre de red denominado `SQL Network Name (SQL35)`.  
  
    ```powershell
    Import-Module FailoverClusters  
  
    $nameResource = "SQL Network Name (SQL35)"  
    Get-ClusterResource $nameResource | Set-ClusterParameter ClusterParameter HostRecordTTL 300  
    ```  
  
    > [!TIP]  
    >  Cada vez que abre una nueva ventana de PowerShell, necesita importar el módulo **FailoverClusters** .  
  
##### <a name="related-content-powershell"></a>Contenido relacionado (PowerShell)  
  
-   [Clustering and High-Availability (Clústeres y alta disponibilidad)](https://techcommunity.microsoft.com/t5/failover-clustering/bg-p/FailoverClustering) (blog del equipo de Agrupacion de clústeres de conmutación por error y equilibrio de carga de red)  
  
-   [Introducción a Windows PowerShell en un clúster de conmutación por error](https://technet.microsoft.com/library/ee619762\(WS.10\).aspx)  
  
-   [Comandos de recursos de clúster y cmdlets equivalentes de Windows PowerShell](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee619744(v=ws.10)#BKMK_resource)  
  
###  <a name="related-content-windows-system"></a><a name="RelatedContentWS"></a> Contenido relacionado (sistema Windows)  
  
-   [Configurar los valores de DNS en un clúster de conmutación por error de varios sitios](https://technet.microsoft.com/library/dd197562\(WS.10\).aspx)  
  
-   [Registro DNS con el recurso de nombre de red](https://techcommunity.microsoft.com/t5/failover-clustering/dns-registration-with-the-network-name-resource/ba-p/371482)  
  

##  <a name="sql-server-instance-prerequisites-and-restrictions"></a><a name="ServerInstance"></a> Requisitos previos y restricciones de las instancias de SQL Server  
 Cada grupo de disponibilidad requiere un conjunto de asociados de conmutación por error, conocido como *réplicas de disponibilidad*, que se hospedan en instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Una instancia del servidor determinada puede ser una *instancia independiente* o una [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]*instancia de clúster de conmutación por error* (FCI).  
  
 **En esta sección:**  
  
-   [Lista de comprobación: Requisitos previos](#PrerequisitesSI)  
  
-   [Uso de subprocesos por parte de los grupos de disponibilidad](#ThreadUsage)  
  
-   [Permisos](#PermissionsSI)  
  
-   [Tareas relacionadas](#RelatedTasksSI)  
  
-   [Contenido relacionado](#RelatedContentSI)  
  
###  <a name="checklist-prerequisites-server-instance"></a><a name="PrerequisitesSI"></a> Lista de comprobación: Requisitos previos (instancia de servidor)  
  
|Requisito previo|Vínculos|  
|------------------|-----------|  
|Este equipo host debe ser un nodo de WSFC. Las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospedan réplicas de disponibilidad de un determinado grupo de disponibilidad residen en nodos independientes del clúster. Un grupo de disponibilidad puede ocupar temporalmente dos clústeres mientras se migra a otro clúster. SQL Server 2016 incorpora la novedad de los grupos de disponibilidad distribuidos. En un grupo de disponibilidad distribuido, dos grupos de disponibilidad residen en clústeres distintos.|[Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)<br /><br /> [Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)<br/> <br/> [Grupos de disponibilidad distribuidos (Grupos de disponibilidad AlwaysOn)](./distributed-availability-groups.md)|  
|Si desea que un grupo de disponibilidad trabaje con Kerberos:<br /><br /> Todas las instancias de servidor que hospedan una réplica de disponibilidad para el grupo de disponibilidad deben usar la misma cuenta de servicio de SQL Server.<br /><br /> El administrador de dominio debe registrar manualmente un nombre principal de servicio (SPN) con Active Directory en la cuenta del servicio SQL Server para el nombre de red virtual (VNN) del agente de escucha del grupo de disponibilidad. Si el SPN se registra en una cuenta distinta de la cuenta del servicio SQL Server, la autenticación no se realizará correctamente.<br /><br /> <br /><br /> <b>\*\* Importante \*\*</b> Si cambia la cuenta del servicio SQL Server, el administrador de dominio deberá volver a registrar manualmente el SPN.|[Registrar un nombre principal de servicio para las conexiones con Kerberos](../../../database-engine/configure-windows/register-a-service-principal-name-for-kerberos-connections.md)<br /><br /> **Breve explicación:**<br /><br /> Kerberos y los SPN exigen la autenticación mutua. El SPN se asigna a la cuenta de Windows que inicia los servicios SQL Server. Si el SPN no se registra correctamente o provoca un error, el nivel de seguridad de Windows no podrá determinar la cuenta asociada al SPN y no podrá utilizarse la autenticación Kerberos.<br /><br /> <br /><br /> Nota: NTLM no tiene este requisito.|  
|Si va a usar una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para hospedar una réplica de disponibilidad, asegúrese de que comprende las restricciones de FCI y de que se cumplen los requisitos de FCI.|[Requisitos previos y requisitos para usar una instancia de clúster de conmutación por error (FCI) de SQL Server para hospedar una réplica de disponibilidad](#FciArLimitations) (más adelante en este artículo)|  
|Cada instancia del servidor debe ejecutar la misma versión de SQL Server para participar en un grupo de disponibilidad Always On.|Ediciones y características admitidas para [SQL 2014](/previous-versions/sql/2014/getting-started/features-supported-by-the-editions-of-sql-server-2014?view=sql-server-2014&preserve-view=true), [SQL 2016](../../../sql-server/editions-and-components-of-sql-server-2016.md?view=sql-server-2016&preserve-view=true), [SQL 2017](../../../sql-server/editions-and-components-of-sql-server-2017.md?view=sql-server-2017&preserve-view=true).|  
|Todas las instancias del servidor que hospedan réplicas de disponibilidad para un grupo de disponibilidad deben usar la misma intercalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Configurar o cambiar la intercalación del servidor](../../../relational-databases/collations/set-or-change-the-server-collation.md)|  
|Habilite la característica de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en cada instancia de servidor que hospeda una réplica de disponibilidad para cualquier grupo de disponibilidad. En un equipo determinado, puede habilitar tantas instancias de servidor de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] como admita la instalación de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Habilitar y deshabilitar grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)<br /><br /> <br /><br /> <b>\*\* Importante \*\*</b> Si elimina y vuelve a crear un WSFC, debe deshabilitar y volver a habilitar la característica [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en cada instancia del servidor habilitada para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en el clúster original.|  
|Cada instancia del servidor requiere un extremo de creación de reflejo de la base de datos. Observe que todas las réplicas de disponibilidad, asociados de creación de reflejo de la base de datos y testigos de la instancia del servidor comparten este extremo.<br /><br /> Si una instancia de servidor seleccionada para hospedar una réplica de disponibilidad se ejecuta bajo una cuenta de usuario de dominio y no tiene todavía un punto de conexión de creación de reflejo de la base de datos, el [Asistente para nuevo grupo de disponibilidad](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md) (o el [asistente Agregar réplica a grupo de disponibilidad](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)) puede crear el punto de conexión y conceder el permiso CONNECT a la cuenta de servicio de la instancia de servidor. Sin embargo, si el servicio [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se ejecuta como una cuenta integrada (como sistema local, servicio local o servicio de red), o una cuenta que no es de dominio, debe usar certificados para la autenticación de extremos y el asistente no podrá crear un extremo de creación de reflejo de la base de datos en la instancia de servidor. En este caso, se recomienda crear los extremos de creación de reflejo de la base de datos manualmente antes de iniciar el asistente.<br /><br /> <br /><br /> <b>\*\* Nota de seguridad \*\*</b> La seguridad de transporte para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] es la misma que para la creación de reflejo de la base de datos.|[El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)<br /><br /> [Seguridad de transporte para la creación de reflejo de la base de datos y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/database-mirroring/transport-security-database-mirroring-always-on-availability.md)|  
|Si va a agregar cualquier base de datos que usa FILESTREAM a un grupo de disponibilidad, asegúrese de que FILESTREAM está habilitado en cada instancia de servidor que hospedará una réplica de disponibilidad para el grupo de disponibilidad.|[Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Si va a agregar cualquier base de datos independiente a un grupo de disponibilidad, asegúrese de que la opción de servidor **contained database authentication** esté establecida en **1** en cada instancia del servidor que hospedará una réplica de disponibilidad para el grupo de disponibilidad.|[contained database authentication (opción de configuración del servidor)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Opciones de configuración de servidor &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
###  <a name="thread-usage-by-availability-groups"></a><a name="ThreadUsage"></a> Uso de subprocesos por parte de los grupos de disponibilidad  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] tiene los siguientes requisitos para los subprocesos de trabajo:  
  
-   En una instancia inactiva de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] usa 0 subprocesos.  
  
-   El número máximo de subprocesos usados por los grupos de disponibilidad es el valor configurado para el número máximo de subprocesos de servidor ('**max worker threads**') menos 40.  
  
-   Las réplicas de disponibilidad hospedadas en una instancia de servidor dada comparten un único grupo de subprocesos.  
  
     Los subprocesos se comparten a petición, de la manera siguiente:  
  
    -   Normalmente, hay entre 3 y 10 subprocesos compartidos, pero este número puede aumentar según la carga de réplica principal.  
  
    -   Si un subproceso determinado está inactivo durante un tiempo, se libera de nuevo en el grupo de subprocesos general de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Normalmente, un subproceso inactivo se libera después de unos 15 segundos de inactividad. Sin embargo, dependiendo de la última actividad, un subproceso inactivo se puede conservar más tiempo.  

    -   Una instancia de SQL Server utiliza hasta 100 subprocesos para puesta al día en paralelo para las réplicas secundarias. Cada base de datos utiliza hasta la mitad del número total de núcleos de CPU, pero no más de 16 subprocesos por base de datos. Si el número total de subprocesos necesarios para una única instancia supera los 100, SQL Server utiliza un solo subproceso de puesta al día para cada base de datos restante. Los subprocesos de rehacer en serie se liberan después de unos 15 segundos de inactividad. 
     
-   Además, los grupos de disponibilidad usan subprocesos no compartidos, de la manera siguiente:  
  
    -   Cada réplica principal usa 1 subproceso de captura de registro para cada base de datos principal. Además, usa 1 subproceso de envío de registro para cada base de datos secundaria. Los subprocesos de envío de registro se liberan después de unos 15 segundos de inactividad.    
  
    -   Una copia de seguridad en una réplica secundaria contiene un subproceso en la réplica principal mientras dura la operación de copia de seguridad. 

-  SQL Server 2019 presentó una puesta al día en paralelo de las bases de datos de grupo de disponibilidad optimizadas para memoria. En SQL Server 2016 y 2017, las tablas basadas en disco no usan la puesta al día en paralelo si una base de datos de un grupo de disponibilidad también está optimizada para memoria. 
  
 Para más información, vea [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases) (Series de aprendizaje de Always ON - HADRON: Uso del grupo de trabajo para las bases de datos compatibles con HADRON) (Blog de ingenieros de CSS [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]).  
  
###  <a name="permissions-server-instance"></a><a name="PermissionsSI"></a> Permisos (instancia del servidor)  
  
|Tarea|Permisos necesarios|  
|----------|--------------------------|  
|Crear el extremo de creación de reflejo de la base de datos|Requiere permiso CREATE ENDPOINT o pertenecer al rol fijo de servidor **sysadmin** .  También requiere el permiso CONTROL ON ENDPOINT. Para obtener más información, vea [GRANT &#40;permisos de punto de conexión de Transact-SQL&#41;](../../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).|  
|Habilitar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]|Requiere la pertenencia al grupo **Administrador** en el equipo local y control total del WSFC.|  
  
###  <a name="related-tasks-server-instance"></a><a name="RelatedTasksSI"></a> Tareas relacionadas (instancia del servidor)  
  
|Tarea|Artículo|  
|----------|-----------|  
|Determinar si existe un extremo de creación de reflejo de la base de datos|[sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)|  
|Crear el extremo de creación de reflejo de la base de datos (si aún no existe)|[Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)<br /><br /> [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)<br /><br /> [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)|  
|Habilitar grupos de disponibilidad|[Habilitar y deshabilitar grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)|  
  
###  <a name="related-content-server-instance"></a><a name="RelatedContentSI"></a> Contenido relacionado (instancia del servidor)  
  
-   [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases) (Series de aprendizaje de Always ON - HADRON: uso del grupo de trabajo para las bases de datos compatibles con HADRON)  
  
##  <a name="network-connectivity-recommendations"></a><a name="NetworkConnect"></a> Recomendaciones de conectividad de red  
 Se recomienda usar los mismos vínculos de red para las comunicaciones entre los nodos de WSFC y las comunicaciones entre las réplicas de disponibilidad.  El uso de vínculos de red independientes puede provocar comportamientos inesperados si alguno de los vínculos da error (incluso de forma intermitente).  
  
 Por ejemplo, para que un grupo de disponibilidad admita la conmutación por error automática, la réplica secundaria que sea el asociado de conmutación por error automática debe tener el estado SYNCHRONIZED. Si el vínculo de red a esta réplica secundaria da error (incluso de forma intermitente), la réplica entra en el estado UNSYNCHRONIZED y no puede iniciarse para volver a sincronizar hasta que se restaure el vínculo. Si el WSFC solicita una conmutación por error automática mientras la réplica secundaria no está sincronizada, la conmutación por error automática no aparecerá.  
  
##  <a name="client-connectivity-support"></a><a name="ClientConnSupport"></a> Compatibilidad con conectividad de cliente  
 Para obtener más información sobre la compatibilidad de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para la conectividad de cliente, vea [Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md).  
  
##  <a name="prerequisites-and-restrictions-for-using-a-sql-server-failover-cluster-instance-fci-to-host-an-availability-replica"></a><a name="FciArLimitations"></a> Requisitos previos y restricciones para usar una instancia de clúster de conmutación por error (FCI) de SQL Server para hospedar una réplica de disponibilidad  
 **En esta sección:**  
  
-   [Restricciones](#RestrictionsFCI)  
  
-   [Lista de comprobación: Requisitos previos](#PrerequisitesFCI)  
  
-   [Tareas relacionadas](#RelatedTasksFCIs)  
  
-   [Contenido relacionado](#RelatedContentFCIs)  
  
###  <a name="restrictions-fcis"></a><a name="RestrictionsFCI"></a> Restricciones (FCI)  
  
> [!NOTE]  
> Las instancias del clúster de conmutación por error admiten volúmenes compartidos en clúster (CSV). Para obtener más información sobre CSV, vea [Descripción de Volúmenes compartidos de clúster en un clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)).  
  
-   **Los nodos de clúster de una FCI pueden hospedar una única réplica para un grupo de disponibilidad determinado:**  si agrega una réplica de disponibilidad en una FCI, los nodos del clúster de WSFC que sean posibles propietarios de FCI no pueden hospedar otra réplica para el mismo grupo de disponibilidad.  Para evitar posibles conflictos, se recomienda configurar los posibles propietarios de la instancia de clúster de conmutación por error. Con esto se evitará que un único WSFC intente hospedar dos réplicas de disponibilidad para el mismo grupo de disponibilidad.
  
     Además, el resto de réplicas debe hospedarse en una instancia de SQL Server 2016 que resida en otro nodo de clúster del mismo clúster de conmutación por error de Windows Server. La única excepción es que mientras se migra a otro clúster, un grupo de disponibilidad puede ocupar temporalmente dos clústeres. 

  >[!WARNING]
  > Si usa el Administrador de clústeres de conmutación por error para mover una *instancia de clúster de conmutación por error* que hospeda un grupo de disponibilidad a un nodo que *ya* hospeda una réplica del mismo grupo de disponibilidad, podría provocar la pérdida de la réplica del grupo de disponibilidad, impidiendo que se ponga en línea en el nodo de destino. Un único nodo de un clúster de conmutación por error no puede hospedar más de una réplica para el mismo grupo de disponibilidad. Para más información sobre cómo se produce esto y cómo recuperar, eche un vistazo a la entrada de blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group) (Réplica dejada inesperadamente en grupo de disponibilidad). 

  
-   **Las FCI no admiten la conmutación automática por error por grupos de disponibilidad:**  las FCI no admiten la conmutación automática por error de grupos de disponibilidad; por tanto, todas las réplicas de disponibilidad hospedadas por una FCI solo se pueden configurar para la conmutación por error manual.  
  
-   **Cambiar el nombre de red de FCI:**  si necesita cambiar el nombre de red de una FCI que hospede una réplica de disponibilidad, tendrá que quitar la réplica del grupo de disponibilidad y, después, volver a agregar la réplica al grupo de disponibilidad. No puede quitar la réplica principal, de modo que si cambia el nombre de una FCI que hospeda la réplica principal, debe conmutar por error a una réplica secundaria, después quitar la réplica principal anterior y volver a agregarla. Observe que cambiar el nombre de una FCI puede modificar la dirección URL del extremo de creación de reflejo de la base de datos. Al agregar la réplica asegúrese de especificar la dirección URL del extremo actual.  
  
###  <a name="checklist-prerequisites-fcis"></a><a name="PrerequisitesFCI"></a> Lista de comprobación: Requisitos previos (FCI)  
  
|Requisito previo|Vínculo|  
|------------------|----------|  
|Asegúrese de que cada instancia de clúster de conmutación por error (FCI) de SQL Server posee el almacenamiento compartido necesario según la instalación estándar de la instancia de clúster de conmutación por error de SQL Server.||  
  
###  <a name="related-tasks-fcis"></a><a name="RelatedTasksFCIs"></a> Tareas relacionadas (FCI)  
  
|Tarea|Artículo|  
|----------|-----------|  
|Instalar un clúster de conmutación por error de SQL Server|[Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Actualización en contexto del clúster de conmutación por error existente de SQL Server|[Actualizar una instancia de clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md)|  
|Mantener el clúster de conmutación por error existente de SQL Server|[Agregar o quitar nodos en un clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md)|  
  
###  <a name="related-content-fcis"></a><a name="RelatedContentFCIs"></a> Contenido relacionado (FCI)  
  
-   [Clústeres de conmutación por error y grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)  
  
-   [Guía de arquitectura de Always On: generación de una solución de alta disponibilidad y recuperación ante desastres mediante instancias de clúster de conmutación por error y grupos de disponibilidad](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
##  <a name="availability-group-prerequisites-and-restrictions"></a><a name="PrerequisitesForAGs"></a> Requisitos previos y restricciones de los grupos de disponibilidad  
 **En esta sección:**  
  
-   [Restricciones](#RestrictionsAG)  
  
-   [Requisitos](#RequirementsAG)  
  
-   [Seguridad](#SecurityAG)  
  
-   [Tareas relacionadas](#RelatedTasksAGs)  
  
###  <a name="restrictions-availability-groups"></a><a name="RestrictionsAG"></a> Restricciones (grupos de disponibilidad)  
  
-   **Las réplicas de disponibilidad se deben hospedar en distintos nodos de un WSFC:**  para un grupo de disponibilidad determinado, las réplicas de disponibilidad se deben hospedar en las instancias del servidor que se ejecutan en nodos diferentes del mismo WSFC. La única excepción es que mientras se migra a otro clúster, un grupo de disponibilidad puede ocupar temporalmente dos clústeres.  
  
    > [!NOTE]  
    >  Cada una de las diferentes máquinas virtuales de un mismo equipo físico puede hospedar una réplica de disponibilidad para el mismo grupo de disponibilidad, porque cada máquina virtual actúa como un equipo independiente.  
  
-   **Nombre único del grupo de disponibilidad:**  cada nombre de grupo de disponibilidad debe ser único en el WSFC. La longitud máxima del nombre de un grupo de disponibilidad es 128 caracteres.  
  
-   **Réplicas de disponibilidad:**  Cada grupo de disponibilidad admite una réplica principal y hasta ocho réplicas secundarias. Todas las réplicas pueden ejecutarse en el modo de confirmación asincrónica, o bien hasta cinco de ellas pueden ejecutarse en el modo de confirmación sincrónica (una réplica principal con dos réplicas secundarias sincrónicas).  
  
-   **Número máximo de grupos de disponibilidad y bases de datos de disponibilidad por equipo:** El número real de bases de datos y grupos de disponibilidad que puede colocar en un equipo (equipo físico o máquina virtual) depende del hardware y de la carga de trabajo, pero no hay ningún límite impuesto. Microsoft ha probado hasta 10 grupos de disponibilidad y 100 bases de datos por máquina física, pero este no es un límite vinculante. Según la especificación de hardware en el servidor y la carga de trabajo, puede poner un mayor número de bases de datos y grupos de disponibilidad en una instancia de SQL Server. Los signos de sistemas sobrecargados pueden incluir, pero no limitarse a los siguientes: agotamiento de subprocesos de trabajo, tiempos de respuesta lentos para vistas del sistema del grupo de disponibilidad y DMV de AlwaysOn o volcados del sistema del distribuidor detenidos. Asegúrese de comprobar minuciosamente el entorno con una carga de trabajo similar a la que usará en producción para asegurarse de que puede controlar la capacidad de carga de trabajo máxima con sus contratos de nivel de servicio de aplicación. Cuando evalúe los contratos de nivel de servicio, no olvide tener en cuenta la carga en condiciones de error además de los tiempos de respuesta esperados.  
  
-   **No use el Administrador de clústeres de conmutación por error para manipular grupos de disponibilidad**. El estado de una Instancia de clúster de conmutación por error (FCI) de SQL Server se comparte entre SQL Server y el Clúster de conmutación por error de Windows (WSFC), y SQL Server mantiene información de estado sobre las instancias más detallada que de la que se ocupa el clúster. El modelo de administración supone que SQL Server debe impulsar las transacciones y se encarga de mantener la vista de estado del clúster sincronizada con la vista de estado de SQL Server. Si el estado del clúster cambia fuera de SQL Server, es posible que el estado no esté sincronizado entre WSFC y SQL Server, lo que puede provocar un comportamiento impredecible.
  
     Por ejemplo:  
  
    -   No cambie ninguna propiedad del grupo de disponibilidad, como los posibles propietarios.  
  
    -   No use el Administrador de clústeres de conmutación por error para conmutar grupos de disponibilidad. Debe usar [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
###  <a name="prerequisites-availability-groups"></a><a name="RequirementsAG"></a> Requisitos previos (grupos de disponibilidad)  
 Al crear o establecer de nuevo una configuración de grupo de disponibilidad, asegúrese de que se adhiere a los siguientes requisitos.  
  
|Requisito previo|Descripción|  
|------------------|-----------------|  
|Si va a usar una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para hospedar una réplica de disponibilidad, asegúrese de que comprende las restricciones de FCI y de que se cumplen los requisitos de FCI.|[Requisitos previos y restricciones del uso de una instancia de clúster de conmutación por error (FCI) de SQL Server para hospedar una réplica de disponibilidad](#FciArLimitations) (más adelante en este artículo)|  
  
###  <a name="security-availability-groups"></a><a name="SecurityAG"></a> Seguridad (grupos de disponibilidad)  
  
-   La seguridad se hereda del WSFC. El clúster de conmutación por error de Windows Server proporciona dos niveles de seguridad del usuario con una granularidad de clúster completo:  
  
    -   Acceso de solo lectura  
  
    -   Control total  
  
         [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] necesita el control total, y habilitar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proporciona control total del clúster (a través del SID de servicio).  
  
         No puede agregar o quitar directamente la seguridad de una instancia del servidor en el Administrador de clústeres. Para administrar las sesiones de seguridad del clúster, use el administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o el equivalente de WMI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
-   Cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe tener permisos de acceso al Registro, al clúster, y así sucesivamente.  
  
-   Se recomienda utilizar cifrado para las conexiones entre las instancias del servidor que hospedan las réplicas de disponibilidad de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
#### <a name="permissions-availability-groups"></a>Permisos (grupos de disponibilidad)  
  
|Tarea|Permisos necesarios|  
|----------|--------------------------|  
|Crear un grupo de disponibilidad|Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.|  
|Modificar un grupo de disponibilidad|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.<br /><br /> Además, combinar una base de datos con un grupo de disponibilidad requiere ser miembro del rol fijo de base de datos **db_owner** .|  
|Quitar o eliminar un grupo de disponibilidad|Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER. Para quitar un grupo de disponibilidad que no se encuentre hospedado en la ubicación de réplica local, se necesita el permiso CONTROL SERVER o el permiso CONTROL en ese grupo de disponibilidad.|  
  
###  <a name="related-tasks-availability-groups"></a><a name="RelatedTasksAGs"></a> Tareas relacionadas (grupos de disponibilidad)  
  
|Tarea|Artículo|  
|----------|-----------|  
|Crear un grupo de disponibilidad|[Usar el grupo de disponibilidad (Asistente para nuevo grupo de disponibilidad)](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)<br /><br /> [Crear un grupo de disponibilidad (Transact-SQL)](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)<br /><br /> [Crear un grupo de disponibilidad (SQL Server PowerShell)](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)<br /><br /> [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)|  
|Modificar el número de réplicas de disponibilidad|[Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)<br /><br /> [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md)|  
|Crear un agente de escucha del grupo de disponibilidad|[Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)|  
|Quitar un grupo de disponibilidad|[Quitar un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-an-availability-group-sql-server.md)|  
  
##  <a name="availability-database-prerequisites-and-restrictions"></a><a name="PrerequisitesForDbs"></a> Requisitos previos y restricciones de las bases de datos de disponibilidad  
 Para poder agregar una base de datos a un grupo de disponibilidad, la base de datos debe cumplir los requisitos previos y restricciones siguientes.  
  
 **En esta sección:**  
  
-   [Requisitos](#RequirementsDb)  
  
-   [Restricciones](#RestrictionsDb)  
  
-   [Recomendaciones para equipos que hospedan réplicas de disponibilidad (sistema de Windows)](#TDEdbs)  
  
-   [Permisos](#PermissionsDbs)  
  
-   [Tareas relacionadas](#RelatedTasksADb)  
  
###  <a name="checklist-requirements-availability-databases"></a><a name="RequirementsDb"></a> Lista de comprobación: Requisitos (bases de datos de disponibilidad)  
 Para poder agregarse a un grupo de disponibilidad, una base de datos debe:  
  
|Requisitos|Vínculo|  
|------------------|----------|  
|Ser una base de datos de usuario. Las bases de datos del sistema no pueden pertenecer a un grupo de disponibilidad.||  
|Residir en la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] donde esté creando el grupo de disponibilidad y ser accesible a la instancia del servidor.||  
|Ser una base de datos de lectura y escritura. Las bases de datos de solo lectura no se pueden agregar a un grupo de disponibilidad.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_read_only** = 0)|  
|Ser una base de datos multiusuario.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**user_access** = 0)|  
|No utilizar AUTO_CLOSE.|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**is_auto_close_on** = 0)|  
|Utilice el modelo de recuperación completa (también conocido como modo de recuperación completa).|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**recovery_model** = 1)|  
|Posea al menos una copia de seguridad completa de la base de datos.<br /><br /> Nota: Después de establecer una base de datos en modo de recuperación completa, se requiere una copia de seguridad completa para iniciar la cadena de registros de recuperación completa.|[Crear una copia de seguridad completa de base de datos &#40;SQL Server&#41;](../../../relational-databases/backup-restore/create-a-full-database-backup-sql-server.md)|  
|No pertenecer a ningún grupo de disponibilidad existente|[sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) (**group_database_id** = NULL)|  
|No estar configurada para la creación de reflejo de la base de datos.|[sys.database_mirroring](../../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md) (si la base de datos no participa en la creación de reflejo, todas las columnas con el prefijo “mirroring_” son NULL).|  
|Para poder agregar una base de datos que usa FILESTREAM a un grupo de disponibilidad, asegúrese de que FILESTREAM está habilitado en cada instancia del servidor que hospeda o que hospedará una réplica de disponibilidad para el grupo de disponibilidad.|[Enable and Configure FILESTREAM](../../../relational-databases/blob/enable-and-configure-filestream.md)|  
|Antes de agregar una base de datos independiente a un grupo de disponibilidad, asegúrese de que la opción de servidor **contained database authentication** está establecida en **1** en cada instancia del servidor que hospeda o que hospedará una réplica de disponibilidad para el grupo de disponibilidad.|[contained database authentication (opción de configuración del servidor)](../../../database-engine/configure-windows/contained-database-authentication-server-configuration-option.md)<br /><br /> [Opciones de configuración de servidor &#40;SQL Server&#41;](../../../database-engine/configure-windows/server-configuration-options-sql-server.md)|  
  
> [!NOTE]  
>  [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] funciona con cualquier nivel de compatibilidad con bases de datos.  
  
###  <a name="restrictions-availability-databases"></a><a name="RestrictionsDb"></a> Restricciones (bases de datos de disponibilidad)  
  
-   Si la ruta de acceso de archivo (incluida la letra de unidad) de una base de datos secundaria es diferente de la ruta de acceso de la base de datos principal correspondiente, se aplican las restricciones siguientes:  
  
    -   **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]/[!INCLUDE[ssAoAddDbWiz](../../../includes/ssaoadddbwiz-md.md)]:**  La opción **Completa** no es compatible (en la [página Seleccionar sincronización de datos iniciales](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md)),  
  
    -   **RESTORE WITH MOVE:**  para crear las bases de datos secundarias, se debe aplicar RESTORE WITH MOVE a todas las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeden una réplica secundaria.  
  
    -   **Impacto en las operaciones de agregar archivos:**  una operación de agregar archivos posterior en la réplica principal podría producir un error en las bases de datos secundarias. Este error puede causar la suspensión de las bases de datos secundarias. Esto, a su vez, hace que las réplicas secundarias entren en el estado NOT SYNCHRONIZING.  
  
        > [!NOTE]  
        >  Para obtener más información sobre cómo responder a una operación de adición de archivos errónea, vea [Solucionar problemas relativos a una operación de agregar archivos con error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md).  
  
-   No se puede quitar una base de datos que pertenezca actualmente a un grupo de disponibilidad.  
  
###  <a name="follow-up-for-tde-protected-databases"></a><a name="TDEdbs"></a> Seguimiento de las bases de datos protegidas por TDE  
 Si usa cifrado de datos transparente (TDE), la clave de certificado o asimétrica para crear y descifrar otras claves debe ser la misma en todas las instancias de servidor que hospedan una réplica de disponibilidad para el grupo de disponibilidad. Para obtener más información, vea [Mover una base de datos protegida por TDE a otra instancia de SQL Server](../../../relational-databases/security/encryption/move-a-tde-protected-database-to-another-sql-server.md).  
  
###  <a name="permissions-availability-databases"></a><a name="PermissionsDbs"></a> Permisos (bases de datos de disponibilidad)  
 Requiere el permiso ALTER en la base de datos.  
  
###  <a name="related-tasks-availability-databases"></a><a name="RelatedTasksADb"></a> Tareas relacionadas (bases de datos de disponibilidad)  
  
|Tarea|Artículo|  
|----------|-----------|  
|Preparar una base de datos secundaria (manualmente)|[Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)|  
|Combinar una base de datos secundaria con un grupo de disponibilidad (manualmente)|[Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)|  
|Modificar el número de bases de datos de disponibilidad|[Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)<br /><br /> [Quitar una base de datos secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server.md)<br /><br /> [Quitar una base de datos principal de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-primary-database-from-an-availability-group-sql-server.md)|  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Blog del equipo Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
-   [Always On - HADRON Learning Series: Worker Pool Usage for HADRON Enabled Databases](/archive/blogs/psssql/alwayson-hadron-learning-series-worker-pool-usage-for-hadron-enabled-databases) (Series de aprendizaje de Always ON - HADRON: uso del grupo de trabajo para las bases de datos compatibles con HADRON)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)   
 [Conectividad de cliente de AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-client-connectivity-sql-server.md)  
  
    
  
--------------------------------------------------
