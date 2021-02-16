---
title: Cuadro de diálogo Nuevo grupo de disponibilidad en SSMS
description: Obtenga información sobre cómo usar el cuadro de diálogo Nuevo grupo de disponibilidad de SQL Server Management Studio para configurar un grupo de disponibilidad Always On.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], creating
ms.assetid: 1b0a6421-fbd4-4bb4-87ca-657f4782c433
author: cawrites
ms.author: chadam
ms.openlocfilehash: ed9c9ec058062dd378bf29f78e1c60c49a654669
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355122"
---
# <a name="use-the-new-availability-group-dialog-box-sql-server-management-studio"></a>Usar el cuadro de diálogo Nuevo grupo de disponibilidad (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Este tema contiene información sobre cómo usar el cuadro de diálogo **Nuevo grupo de disponibilidad** de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] para crear un grupo de disponibilidad AlwaysOn en las instancias de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] habilitadas para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. Un *grupo de disponibilidad* define un conjunto de bases de datos de usuario que realizarán la conmutación por error como una sola unidad y un conjunto de asociados de conmutación por error, conocido como *réplicas de disponibilidad*, que admiten la conmutación por error.  
  
> [!NOTE]  
>  Para obtener una introducción a los grupos de disponibilidad, vea [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
   
> [!NOTE]  
>  Para obtener información sobre las formas alternativas de crear un grupo de disponibilidad, vea [Tareas relacionadas](#RelatedTasks), más adelante en este tema.  
  
  
##  <a name="prerequisites"></a><a name="PrerequisitesRestrictions"></a> Requisitos previos  
  
-   Antes de crear un grupo de disponibilidad, compruebe que las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospedan réplicas de disponibilidad residen en otro nodo (WSFC) de clúster de conmutación por error de Windows Server en el mismo clúster de conmutación por error de WSFC. Además, compruebe que cada una de las instancias de servidor está habilitada para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] y cumple todos los requisitos previos de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] . Para obtener más información, recomendamos encarecidamente que lea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
-   Antes de crear un grupo de disponibilidad, asegúrese de que cada instancia de servidor que hospedará una réplica de disponibilidad tenga un extremo de creación de reflejo de la base de datos que funcione. Para obtener más información, vea [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md).  
  
-   Para usar el cuadro de diálogo **Nuevo grupo de disponibilidad** , debe conocer los nombres de las instancias de servidor que hospedarán las réplicas de disponibilidad. Además, debe conocer los nombres de las bases de datos que se proponga agregar al nuevo grupo de disponibilidad, y debe asegurarse de que estas bases de datos cumplen los requisitos previos y las restricciones de la base de datos de disponibilidad que se describen en [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md). Si escribe valores no válidos, el nuevo grupo de disponibilidad no funcionará.  
  
## <a name="limitations"></a><a name="Limitations"></a> Limitaciones  
 El cuadro de diálogo **Nuevo grupo de disponibilidad** no:  
  
-   Crea un agente de escucha del grupo de disponibilidad.    
-   Une las réplicas secundarias al grupo de disponibilidad.    
-   Realiza la sincronización de datos inicial.  
  
 Para obtener más información sobre estas tareas de configuración, vea [Seguimiento: después de crear un agente de escucha de grupo de disponibilidad](#FollowUp) más adelante en este tema.  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere la pertenencia al rol fijo de servidor **sysadmin** y el permiso de servidor CREATE AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="using-the-new-availability-group-dialog-box-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Usar el cuadro de diálogo Nuevo grupo de disponibilidad (SQL Server Management Studio)  
 **Para crear un grupo de disponibilidad**  
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal y haga clic en el nombre del servidor.  
  
2.  Expanda el nodo **Alta disponibilidad de AlwaysOn** .  
  
3.  Haga clic con el botón derecho en el nodo **Grupos de disponibilidad** y seleccione el comando **Nuevo grupo de disponibilidad** .  
  
4.  Este comando abre el cuadro de diálogo **Nuevo grupo de disponibilidad** .  
  
5.  En la página **General** , use el campo **Nombre de grupo de disponibilidad** para escribir el nombre del nuevo grupo de disponibilidad. Este nombre debe ser un identificador de SQL Server válido y debe ser único en todos los grupos de disponibilidad del clúster de WSFC. La longitud máxima del nombre de un grupo de disponibilidad es 128 caracteres.  
  
6.  En la cuadrícula **Bases de datos de disponibilidad** , haga clic en **Agregar** y especifique el nombre de una base de datos local que desee que pertenezca a este grupo de disponibilidad. Repita esto para cada base de datos que desee agregar. Al hacer clic en **Aceptar**, el diálogo comprobará si la base de datos especificada cumple los requisitos previos para pertenecer a un grupo de disponibilidad. Para obtener más información sobre estos requisitos previos, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
7.  En la cuadrícula **Bases de datos de disponibilidad** , haga clic en **Agregar** y especifique el nombre de una instancia de servidor para hospedar una réplica secundaria. El diálogo no intentará conectarse a estas instancias. Si especifica un nombre de servidor incorrecto, se agregará una réplica secundaria pero no podrá conectarse a ella.  
  
    > [!TIP]  
    >  Si ha agregado una réplica y no puede conectarse a la instancia de servidor de host, puede quitar la réplica y agregar una nueva. Para obtener más información, vea [Quitar una réplica secundaria de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/remove-a-secondary-replica-from-an-availability-group-sql-server.md) y [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
8.  En el panel **Seleccionar una página** del cuadro de diálogo, haga clic en **Preferencias de copia de seguridad**. A continuación, en la página **Preferencias de copia de seguridad** , especifique dónde deben producirse las copias de seguridad según un rol de réplica y asigne prioridades de copia de seguridad a cada instancia de servidor que hospedará una réplica de disponibilidad para este grupo de disponibilidad. Para obtener más información, vea [Propiedades de grupo de disponibilidad: Nuevo grupo de disponibilidad (página Preferencias de copia de seguridad)](../../../database-engine/availability-groups/windows/availability-group-properties-new-availability-group-backup-preferences-page.md).  
  
9. Para crear el grupo de disponibilidad, haga clic en **Aceptar**. Esto hace que el diálogo compruebe si las bases de datos especificadas cumplen los requisitos previos.  
  
     Para salir del cuadro de diálogo sin crear el grupo de disponibilidad, haga clic en **Cancelar**.  
  
##  <a name="follow-up-after-using-the-new-availability-group-dialog-box-to-create-an-availability-group"></a><a name="FollowUp"></a> Seguimiento: Después de usar el cuadro de diálogo Nuevo grupo de disponibilidad para crear un grupo de disponibilidad  
  
-   Deberá conectarse, a su vez, a cada instancia de servidor que hospeda una réplica secundaria del grupo de disponibilidad y completar los pasos siguientes:  
  
    1.  Una la réplica secundaria al grupo de disponibilidad. Para obtener más información, vea [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md).  
  
    2.  Restaure las copias de seguridad actuales de cada base de datos principal y del registro de transacciones (mediante RESTORE WITH NORECOVERY). Para obtener más información, vea [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
    3.  Inmediatamente, una cada base de datos secundaria preparada al grupo de disponibilidad. Para obtener más información, vea [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md).  
  
-   Se recomienda crear una escucha de grupo de disponibilidad para el nuevo grupo de disponibilidad. Esto requiere que esté conectado a la instancia del servidor que hospeda la réplica principal actual. Para obtener más información, vea [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar el grupo de disponibilidad y las propiedades de réplica**  
  
-   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Configurar la directiva de conmutación por error flexible para controlar las condiciones de la conmutación automática por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
-   [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
-   [Configurar la copia de seguridad en réplicas de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-backup-on-availability-replicas-sql-server.md)  
  
-   [Configurar el acceso de solo lectura en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server.md)  
  
-   [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md)  
  
-   [Cambiar el tiempo de espera de la sesión en una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-session-timeout-period-for-an-availability-replica-sql-server.md)  
  
 **Para completar la configuración del grupo de disponibilidad**  
  
-   [Combinar una réplica secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
-   [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
 **Maneras alternativas de crear un grupo de disponibilidad**  
  
-   [Usar el Asistente para grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Crear un grupo de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-transact-sql.md)  
  
-   [Crear un grupo de disponibilidad &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/create-an-availability-group-sql-server-powershell.md)  
  
 **Para habilitar los grupos de disponibilidad AlwaysOn**  
  
-   [Habilitar y deshabilitar grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)  
  
 **Para configurar un extremo de creación del reflejo de la base de datos**  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para la autenticación de Windows &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql.md)  
  
-   [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Para solucionar problemas de configuración de grupos de disponibilidad AlwaysOn**  
  
-   [Solucionar problemas de configuración de grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/troubleshoot-always-on-availability-groups-configuration-sql-server.md)  
  
-   [Solucionar problemas relativos a una operación de agregar archivos con error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/troubleshoot-a-failed-add-file-operation-always-on-availability-groups.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [El punto de conexión de creación de reflejo de la base de datos &#40;SQL Server&#41;](../../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)   
 [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
