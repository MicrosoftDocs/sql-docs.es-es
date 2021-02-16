---
title: Reporting Services con grupos de disponibilidad
description: Aprenda a configurar Reporting Services para trabajar con Grupos de disponibilidad AlwaysOn en SQL Server. La funcionalidad admitida difiere en los distintos escenarios.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Reporting Services, AlwaysOn Availability Groups
- Availability Groups [SQL Server], interoperability
ms.assetid: edeb5c75-fb13-467e-873a-ab3aad88ab72
author: cawrites
ms.author: chadam
manager: erikre
ms.openlocfilehash: d80b30323dfc38d31e385c88bd79e24c3d1c7184
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344542"
---
# <a name="reporting-services-with-always-on-availability-groups-sql-server"></a>Reporting Services con Grupos de disponibilidad AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Este tema contiene información acerca de la configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] para que funcione con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] (AG) en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Los tres escenarios para usar [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] son las bases de datos para orígenes de datos de informes, las bases de datos del servidor de informes y el diseñador de informes. La funcionalidad admitida y la configuración requerida son diferentes para los tres escenarios.  
  
 Una ventaja clave del uso de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con los orígenes de datos [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] es aprovechar las réplicas secundarias legibles como un origen de datos de informes, al mismo tiempo que las réplicas secundarias proporcionan una base de datos principal.  
  
 Para obtener información general sobre [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea las [preguntas más frecuentes sobre Always On en SQL Server 2012 (../../../sql-server/index.yml)](../../../sql-server/index.yml).  

##  <a name="requirements-for-using-reporting-services-and-always-on-availability-groups"></a><a name="bkmk_requirements"></a> Requisitos para usar Reporting Services y Grupos de disponibilidad AlwaysOn  
 [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y Power BI Report Server usan .NET Framework 4.0 y admiten las propiedades de cadena de conexión de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para su uso con orígenes de datos.  
  
 Para usar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con  [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] 2014 y anteriores, deberá descargar e instalar una revisión para .NET 3.5 SP1. La revisión agrega compatibilidad con las características de SQL Client para AG y con las propiedades de cadenas de conexión **ApplicationIntent** y **MultiSubnetFailover**. Si la revisión no se instala en cada equipo que hospeda un servidor de informes, los usuarios que intenten obtener la vista previa de los informes verán un mensaje de error similar al siguiente y el mensaje de error se escribirá en el registro de seguimiento del servidor de informes:  
  
> **Mensaje de error:** "La palabra clave no admite "applicationintent""  
  
 El mensaje tiene lugar cuando incluye una de las propiedades [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en la cadena de conexión [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , pero el servidor no reconoce la propiedad. El mensaje de error indicado se verá cuando haga clic en el botón "Probar conexión" en las interfaces de usuario de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y cuando obtenga la vista previa del informe si los errores remotos se habilitan en los servidores de informes.  
  
 Para obtener más información sobre la revisión necesaria, vea el artículo de KB 2654347A sobre una [revisión que incorpora compatibilidad con las características de AlwaysOn de SQL Server 2012 en .NET Framework 3.5 SP1](https://go.microsoft.com/fwlink/?LinkId=242896).  
  
 Para obtener más información sobre otros requisitos de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
> [!NOTE]  
>  Los archivos de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] como **RSreportserver.config** no se admiten como parte de la funcionalidad de los [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Si tiene que realizar cambios manualmente en un archivo de configuración de uno de los servidores de informes, tendrá que actualizar manualmente las réplicas.  
  
##  <a name="report-data-sources-and-availability-groups"></a><a name="bkmk_reportdatasources"></a> Orígenes de datos de informes y grupos de disponibilidad  
 El comportamiento de los orígenes de datos [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] basados en [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] puede variar en función del modo en que el administrador haya configurado el entorno AG.  
  
 Para utilizar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] para notificar los orígenes de datos, tiene que configurar la cadena de conexión del origen de datos de informe para usar el grupo de disponibilidad *nombre DNS del agente de escucha*. Los orígenes de datos admitidos son los siguientes:  
  
-   El origen de datos ODBC con SQL Native Client.  
  
-   SQL Client, con la revisión .Net aplicada al servidor de informes.  
  
 La cadena de conexión también puede contener nuevas propiedades de conexión AlwaysOn que configuren las solicitudes de consulta de informes para usar la réplica secundaria para los informes de solo lectura. El uso de réplicas secundarias para las solicitudes de notificación reducirá la carga en una réplica principal de lectura-escritura. La ilustración siguiente es un ejemplo de una configuración AG de tres réplicas en la que las cadenas de conexión del origen de datos [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] se han configurado con ApplicationIntent=ReadOnly. En este ejemplo las solicitudes de consulta de informe se envían a una réplica secundaria y no a la réplica principal.  
  
 
  
 El siguiente es un ejemplo de cadena de conexión en el que [AvailabilityGroupListenerName] es el **nombre DNS del agente de escucha** que se configuró cuando las réplicas se crearon:  
  
 `Data Source=[AvailabilityGroupListenerName];Initial Catalog = AdventureWorks2016; ApplicationIntent=ReadOnly`  
  
 El botón **Probar conexión** en las interfaces de usuario de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] validarán si una conexión se puede establecer pero no validará la configuración de AG. Por ejemplo, si incluye ApplicationIntent en una cadena de conexión a un servidor que no es parte de AG, el parámetro adicional se pasa por alto y el botón **Probar conexión** solo validará si una conexión se puede establecer en el servidor especificado.  
  
 El modo en que se creen los informes y se publiquen determinará dónde modificará la cadena de conexión:  
  
-   **Modo nativo:** use el [!INCLUDE[ssRSWebPortal-Non-Markdown](../../../includes/ssrswebportal-non-markdown-md.md)] para los informes y los orígenes de datos compartidos ya publicados en un servidor de informes en modo nativo.  
  
-   **Modo de SharePoint:** use las páginas de configuración de SharePoint dentro de las bibliotecas de documentos para los informes que ya se han publicado en un servidor SharePoint.  
  
-   **Diseño de informes**: [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] o [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] cuando se crean informes nuevos. Vea la sección "Diseño de informes" en este tema o en la información adicional.  
  
 **Recursos adicionales:**  
  
-   [Administrar orígenes de datos de informe](../../../reporting-services/report-data/manage-report-data-sources.md)  
  
-   Para obtener más información acerca de las propiedades de cadenas de conexión disponibles, vea [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md).  
  
-   Para obtener información esencial sobre las escuchas de grupo de disponibilidad, vea [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Consideraciones:** las réplicas secundarias normalmente experimentarán una demora al recibir los cambios de datos de la réplica principal. Los factores siguientes pueden afectar a la latencia entre las réplicas principales y secundarias:  
  
-   El número de réplicas secundarias. La demora aumenta con cada réplica secundaria que se agrega a la configuración.  
  
-   La ubicación geográfica y la distancia entre las réplicas principales y secundarias. Por ejemplo, la demora suele ser mayor si las réplicas secundarias están en un centro de datos diferente o si están en el mismo edificio que la réplica principal.  
  
-   Configuración del modo de disponibilidad para cada réplica. El modo de disponibilidad determina si la réplica principal espera la confirmación de transacciones en una base de datos hasta que una réplica secundaria haya escrito las entradas del registro de transacciones en el disco. Para obtener más información, vea la sección "Modos de disponibilidad" de [Información general de los grupos de disponibilidad Always On &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
 Cuando se usa una réplica secundaria de solo lectura como origen de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , es importante asegurarse de que la latencia de actualización de datos cumple las necesidades de los usuarios del informe.  
  
##  <a name="report-design-and-availability-groups"></a><a name="bkmk_reportdesign"></a> Diseñador de informes y grupos de disponibilidad  
 Al diseñar los informes en [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] o un proyecto de informe en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)], un usuario puede configurar una cadena de conexión del origen de datos de informe para que contenga las nuevas propiedades de conexión proporcionadas por [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. La compatibilidad con las nuevas propiedades de conexión depende de si un usuario obtiene la vista previa del informe.  
  
-   **Vista previa local**: [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)] y [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] usan .NET Framework 4.0 y admiten [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
-   **Vista previa en modo servidor o remoto:** si tras publicar informes en el servidor de informes o usar la vista previa en [!INCLUDE[ssRBnoversion](../../../includes/ssrbnoversion.md)], ve un error similar al siguiente, es una indicación de que está obteniendo la vista previa de los informes con el servidor de informes y la revisión .Net Framework 3.5 SP1 para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] no se ha instalado en el servidor de informes.  
  
> **Mensaje de error:** "La palabra clave no admite "applicationintent""  
  
##  <a name="report-server-databases-and-availability-groups"></a><a name="bkmk_reportserverdatabases"></a> Bases de datos del servidor de informes y grupos de disponibilidad  
 Reporting Services y Power BI Report Server ofrece compatibilidad limitada para usar [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] con bases de datos del servidor de informes. Las bases de datos del servidor de informes se pueden configurar en AG para ser parte de una réplica; sin embargo, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] no usará automáticamente una réplica diferente para las bases de datos del servidor de informes cuando se produce una conmutación por error. No se admite el uso de MultiSubnetFailover con las bases de datos del servidor de informes.  
  
 Los scripts de automatización personalizada o acciones manuales tienen que usarse para realizar la conmutación por error y la recuperación. Hasta que estas acciones se completen, algunas características del servidor de informes pueden no funcionar de forma correcta tras la conmutación por error de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Al planear la recuperación de desastres y la conmutación por error para las bases de datos del servidor de informes, se aconseja que siempre haga una copia de seguridad de la clave de cifrado del servidor de informes.  
  
###  <a name="differences-between-sharepoint-native-mode"></a><a name="bkmk_differences_in_server_mode"></a> Diferencias entre el modo nativo de SharePoint  
 En esta sección se resumen las diferencias entre el modo en que los servidores de informes del nodo nativo y el modo de SharePoint interactúan con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Un servidor de informes de SharePoint crea las bases de datos de **3** para cada aplicación de servicios de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] que cree. La conexión a las bases de datos del servidor de informes en modo de SharePoint se configura en Administración central de SharePoint al crear una nueva aplicación de servicio. Los nombres predeterminados de las bases de datos incluyen un GUID que está asociado a la aplicación de servicio. Estos son los nombres de base de datos de ejemplo, para un servidor de informes en modo de SharePoint:  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6TempDB  
  
-   ReportingService_85c08ac3c8e64d3cb400ad06ed5da5d6_Alerting  
  
 Loas servidores de informes de modo nativo usan **2** bases de datos. Estos son los nombres de base de datos de ejemplo, para un servidor de informes en modo nativo:  
  
-   ReportServer  
  
-   ReportServerTempDB  
  
 El modo nativo no admite ni usa las bases de datos de alerta ni las características relacionadas. Los servidores de informes en modo nativo se configuran en el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . En el modo SharePoint, configure el nombre de base de datos de la aplicación de servicio para que sea el nombre del "punto de acceso de cliente" que creó como parte de la configuración de SharePoint. Para obtener más información sobre cómo configurar SharePoint con [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], vea [Configuración y administración de grupos de disponibilidad de SQL Server para SharePoint Server (/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14))](/previous-versions/office/sharepoint-server-2010/hh913923(v=office.14)).  
  
> [!NOTE]
>  Los servidores de informes de modo de SharePoint usan un proceso de sincronización entre las bases de datos de aplicación de servicio [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y las bases de datos de contenido de SharePoint. Es importante mantener juntas las bases de datos del servidor de informes y las bases de datos de contenido. Debe considerar configurarlas en los mismos grupos de disponibilidad para que conmuten por error y se recuperen como un conjunto. Considere el caso siguiente:  
> 
>  -   Restaura o conmuta por error a una copia de la base de datos de contenido que no ha recibido las mismas actualizaciones que la base de datos del servidor de informes.  
> -   El proceso de sincronización de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] detectará las diferencias entre la lista de elementos de la base de datos de contenido y las bases de datos del servidor de informes.  
> -   El proceso de sincronización eliminará o actualizará los elementos de la base de datos de contenido.  
  
###  <a name="prepare-report-server-databases-for-availability-groups"></a><a name="bkmk_prepare_databases"></a> Preparar las bases de datos del servidor de informes para grupos de disponibilidad  
 A continuación se indican los pasos básicos para preparar y agregar las bases de datos del servidor de informes a un [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]:  
  
-   Cree su grupo de disponibilidad y configure un *nombre DNS del agente de escucha*.  
  
-   **Réplica principal:** configure las bases de datos del servidor de informes para que formen parte de un solo grupo de disponibilidad y cree una réplica principal que incluya todas las bases de datos del servidor de informes.  
  
-   **Réplicas secundarias:** cree una o varias réplicas secundarias. El enfoque habitual para copiar las bases de datos de la réplica principal a la secundaria consiste en restaurar las bases de datos en cada réplica secundaria mediante "RESTORE WITH NORECOVERY". Para obtener más información sobre cómo crear réplicas secundarias y comprobar que la sincronización de datos funciona, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
-   **Credenciales del servidor de informes:** tiene que crear las credenciales del servidor de informes apropiadas en las réplicas secundarias que creó en la principal. Los pasos exactos dependen del tipo de autenticación que usa en su entorno [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]; la cuenta de servicio de Windows [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], la cuenta de usuario de Windows o la autenticación de SQL Server. Para obtener más información, vea [Configurar una conexión a la base de datos del servidor de informes &#40;Administrador de configuración de SSRS&#41;](../../../reporting-services/install-windows/configure-a-report-server-database-connection-ssrs-configuration-manager.md).  
  
-   Actualice la conexión a la base de datos para utilizar el nombre DNS del cliente de escucha. Para los servidores de informes en modo nativo, cambie el **Nombre de base de datos del servidor de informes** en el administrador de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] . Para el modo de SharePoint, cambie el **nombre del servidor de base de datos** de las aplicaciones de servicio de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
###  <a name="steps-to-complete-disaster-recovery-of-report-server-databases"></a><a name="bkmk_steps_to_complete_failover"></a> Pasos para completar la recuperación de desastres de bases de datos del servidor de informes  
 Es necesario completar los pasos siguientes tras una conmutación por error de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] a una réplica secundaria:  
  
1.  Detenga la instancia del servicio Agente SQL que usaba el motor de base de datos principal que hospeda las bases de datos de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
2.  Inicie el servicio Agente SQL en el equipo que sea la nueva réplica principal.  
  
3.  Detenga el servicio del servidor de informes.  
  
     Si el servidor de informes está en modo nativo, detenga el servidor Windows del servidor de informes con el administrador de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] .  
  
     Si el servidor de informes se configura para el modo de SharePoint, detenga el servicio compartido de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en Administración central de SharePoint.  
  
4.  Inicie el servicio del servidor de informes o el servicio [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] SharePoint.  
  
5.  Compruebe que los informes se puedan ejecutar con la nueva réplica principal.  
  
###  <a name="report-server-behavior-when-a-failover-occurs"></a><a name="bkmk_failover_behavior"></a> Comportamiento del servidor de informes cuando se produce una conmutación por error  
 Cuando las bases de datos del servidor conmutan por error y ha actualizado el entorno del servidor de informes para que use la nueva réplica principal, hay algunos problemas operativos que se derivan del proceso de conmutación por error y recuperación. La repercusión de estos problemas variará según la carga de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en el momento de la conmutación por error, así como lo que [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] tarda en conmutar por error a una réplica secundaria y el administrador del servidor de informes en actualizar el entorno de informes para usar la nueva réplica principal.  
  
-   La ejecución del procesamiento en segundo plano puede ocurrir más de una vez debido a la lógica de reintento y a la incapacidad del servidor de informes de marcar el trabajo programado a medida que se completa durante el período de conmutación por error.  
  
-   La ejecución del proceso en segundo plano que normalmente se habría desencadenado durante el período de la conmutación por error no se producirá porque el Agente SQL Server no podrá escribir los datos en la base de datos del servidor de informes y estos datos no se sincronizarán con la nueva réplica principal.  
  
-   Una vez completada la conmutación por error de la base de datos y tras reiniciarse el servicio del servidor de informes, los trabajos del Agente SQL Server se volverán a crear de forma automática. Hasta que los trabajos del agente SQL se vuelvan a crear, cualquier ejecución en segundo plano asociada a los trabajos del Agente SQL Server no se procesarán. Esta versión también incluye suscripciones de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] , programaciones e instantáneas.  
  
## <a name="see-also"></a>Consulte también  
 [Compatibilidad de SQL Server Native Client para la alta disponibilidad con recuperación de desastres](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Introducción a los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/getting-started-with-always-on-availability-groups-sql-server.md)   
 [Using Connection String Keywords with SQL Server Native Client](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  (Usar palabras clave de cadena de conexión con SQL Server Native Client)  
 [Compatibilidad de SQL Server Native Client para la alta disponibilidad con recuperación de desastres](../../../relational-databases/native-client/features/sql-server-native-client-support-for-high-availability-disaster-recovery.md)   
 [Acerca del acceso de conexión de cliente a réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/about-client-connection-access-to-availability-replicas-sql-server.md)  
  
