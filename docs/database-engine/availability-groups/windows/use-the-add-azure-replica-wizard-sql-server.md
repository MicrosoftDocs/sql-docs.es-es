---
title: Configuración de una máquina virtual de Azure como una réplica secundaria en un grupo de disponibilidad
description: Use el Asistente para agregar réplica de Azure a fin de crear una máquina virtual de Azure en una TI híbrida y configurarla como una réplica secundaria para un grupo de disponibilidad Always On nuevo o existente.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.addreplicawizard.azurereplica.f1
ms.assetid: b89cc41b-07b4-49f3-82cc-bc42b2e793ae
author: cawrites
ms.author: chadam
ms.openlocfilehash: 5cdd8ab097516686ffc912de06e754bd48ba6056
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344477"
---
# <a name="configure-azure-vm-as-a-secondary-replica-in-an-availability-group"></a>Configuración de una máquina virtual de Azure como una réplica secundaria en un grupo de disponibilidad
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Use el Asistente para agregar réplica de Azure para que le ayude a crear una nueva VM de Azure en una TI híbrida y a configurarla como una réplica secundaria para un grupo de disponibilidad AlwaysON nuevo o existente.  

>  [!IMPORTANT]  
>  Azure tiene dos modelos de implementación diferentes para crear y trabajar con recursos: Resource Manager y el clásico. En este artículo se trata el modelo de implementación clásico. Microsoft recomienda que las implementaciones más recientes usen el modelo de Resource Manager. Los pasos de este artículo no son aplicables si va a implementar la máquina virtual de Azure con el modelo de Resource Manager.   

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 Si nunca ha agregado una réplica de disponibilidad a un grupo de disponibilidad, vea las secciones "Instancias del servidor" y "Grupos y réplicas de disponibilidad" en [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal actual.  
  
-   Debe tener un entorno de tecnologías informáticas híbrido donde la subred local tenga una VPN de sitio a sitio con Azure. Para obtener más información, vea [Configurar una VPN de sitio a sitio en el Portal de administración](/azure/vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal).  
  
-   Es preciso que el grupo de disponibilidad cuente con réplicas locales disponibles.  
  
-   Los clientes del agente de escucha del grupo de disponibilidad deben disponer de conectividad a Internet si van a mantener la conectividad con el agente de escucha cuando el grupo de disponibilidad haya provocado una conmutación por error en una réplica de Azure.  
  
-   **Requisitos previos para usar la sincronización de datos inicial completa** Deberá especificar un recurso compartido de red para que el asistente cree copias de seguridad y pueda tener acceso a estas. Para la réplica principal, la cuenta usada para iniciar el [!INCLUDE[ssDE](../../../includes/ssde-md.md)] debe tener permisos del sistema de archivos de lectura y escritura en un recurso compartido de red. Para las réplicas secundarias, la cuenta debe tener permiso de lectura en el recurso compartido de red.  
  
     Si no puede utilizar el asistente para realizar la sincronización de datos inicial completa, debe preparar las bases de datos secundarias manualmente. Puede hacerlo antes o después de ejecutar el asistente. Para obtener más información, vea [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
 También se necesita el permiso CONTROL ON ENDPOINT si desea permitir que el asistente Agregar réplica a grupo de disponibilidad administre el extremo de creación de reflejo de la base de datos.  
  
##  <a name="using-the-add-azure-replica-wizard-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Usar el Asistente para agregar réplica de Azure (SQL Server Management Studio)  

>  [!IMPORTANT]  
>  El Asistente para agregar réplicas de Azure solo es compatible con las máquinas virtuales creadas con el modelo de implementación clásica. Las nuevas implementaciones de máquinas virtuales deben utilizar el último modelo de Resource Manager. Si usa máquinas virtuales con Resource Manager, debe agregar manualmente la réplica secundaria de Azure mediante comandos de Transact-SQL (algo que no se muestra aquí). Este Asistente no funcionará en el escenario de Resource Manager. 
>
>  El Asistente para agregar réplicas de Azure no está disponible en las versiones más recientes (versiones 18.x y 17.x) de SQL Server Management Studio.
        
 El Asistente para agregar réplica de Azure se puede iniciar desde [Especificar la página de réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md). Hay dos formas de llegar a esta opción:  
  
-   [Usar el Asistente para grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
 Una vez que haya iniciado el Asistente para agregar réplica de Azure, siga los pasos a continuación:  
  
1.  En primer lugar, descargue un certificado de administración para la suscripción de Azure. Haga clic en **Descargar** para abrir la página de inicio de sesión.  
  
2.  Inicie sesión en Microsoft Azure con la cuenta de Microsoft o la cuenta corporativa. La cuenta corporativa o de Microsoft presenta el formato de una dirección de correo electrónico, como HYPERLINK "mailto:patc@contoso.com" patc@contoso.com. Para obtener más información sobre las credenciales de Azure, vea [Preguntas frecuentes sobre Cuenta para organizaciones de Microsoft](/previous-versions/jj592903(v=msdn.10)) y [Solución de problemas de inicio de sesión con la cuenta corporativa](http://web.archive.org/web/20121016005434/http://support.microsoft.com:80/kb/2756852).  
  
3.  A continuación, haga clic en **Conectar** para conectarse a su suscripción. Cuando lo haya hecho, las listas desplegables se rellenan con los parámetros de Azure, como **Red virtual** y **Subred de la red virtual**.  
  
4.  Especifique los valores de configuración para la máquina virtual de Azure que hospedará la nueva réplica secundaria:  
  
     Imagen  
     Nombre de la imagen de SQL Server que se va a usar para la máquina virtual de Azure  
  
     Tamaño de VM  
     Tamaño de la máquina virtual de Azure  
  
     Nombre de la máquina virtual  
     Nombre DNS de la máquina virtual de Azure  
  
     Nombre de usuario de VM  
     Nombre de usuario del administrador predeterminado para la máquina virtual de Azure  
  
     Contraseña del administrador de la máquina virtual (y confirmar contraseña)  
     Contraseña del administrador predeterminado para la máquina virtual de Azure  
  
     Virtual Network  
     Red virtual en la que estará la máquina virtual de Azure  
  
     Subred de la red virtual  
     Subred de la red virtual en la que estará la máquina virtual de Azure  
  
     Domain  
     Dominio de Active Directory (AD) para combinar la máquina virtual de Azure  
  
     Nombre de usuario de dominio  
     Nombre de usuario de AD que se usa para combinar la máquina virtual de Azure con el dominio  
  
     Contraseña  
     Contraseña que se usa para combinar la máquina virtual de Azure con el dominio  
  
5.  Haga clic en **Aceptar** para confirmar los valores de configuración y salir del Asistente para agregar réplica de Azure.  
  
6.  Continúe con el resto de los pasos de configuración para [Especificar la página de réplicas](../../../database-engine/availability-groups/windows/specify-replicas-page-new-availability-group-wizard-add-replica-wizard.md) , tal como hace para las réplicas nuevas.  
  
     Una vez haya terminado con el Asistente para nuevo grupo de disponibilidad o el Asistente para agregar una réplica al grupo de disponibilidad, el proceso de configuración realizará todas las operaciones de Azure para crear una máquina virtual nueva, combinarla con el dominio de AD, agregarla al clúster de Windows, habilitar la alta disponibilidad de AlwaysOn y agregar una nueva réplica al grupo de disponibilidad.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Agregar una réplica secundaria a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/add-a-secondary-replica-to-an-availability-group-sql-server.md)  
  
