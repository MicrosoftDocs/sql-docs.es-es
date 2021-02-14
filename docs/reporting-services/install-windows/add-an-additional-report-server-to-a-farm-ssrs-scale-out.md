---
description: Agregar un servidor de informes adicional a una granja de servidores (escalado horizontal de SSRS)
title: Agregar un servidor de informes adicional a una granja de servidores (escalabilidad horizontal de SSRS) | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint
ms.topic: conceptual
ms.assetid: c1a6b683-15cf-44ae-ac60-ceee63a60aaf
author: maggiesMSFT
ms.author: maggies
monikerRange: '>=sql-server-2016 <=sql-server-2016'
ms.openlocfilehash: f30aabd3ca217465ce29ca9a4f73a2fb030e155a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100067950"
---
# <a name="add-an-additional-report-server-to-a-farm-ssrs-scale-out"></a>Agregar un servidor de informes adicional a una granja de servidores (escalado horizontal de SSRS)

  Agregar un segundo servidor de informes en modo de SharePoint o varios a una granja de servidores de SharePoint puede mejorar el rendimiento y el tiempo de respuesta de procesamiento del servidor de informes. Si ha detectado una disminución del rendimiento al agregar más usuarios, informes y otras aplicaciones al servidor de informes, agregar servidores adicionales puede mejorar el rendimiento. También se recomienda agregar un segundo servidor de informes para aumentar la disponibilidad de los servidores de informes cuando hay problemas de hardware o cuando se realiza el mantenimiento general en servidores individuales del entorno. A partir de la versión [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , los pasos para escalar horizontalmente un entorno de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en modo de SharePoint siguen la implementación estándar de la granja de servidores de SharePoint y aprovechan las características de equilibrio de carga de SharePoint.  
  
> [!IMPORTANT]  
>  El escalado horizontal de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] no se admite en ninguna de las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea la sección [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] de [Características compatibles con las ediciones de SQL Server](~/sql-server/editions-and-components-of-sql-server-2017.md#SSRS).  
  
> [!TIP]  
>  A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] , no se usa el Administrador de configuración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para agregar servidores y escalar horizontalmente los servidores de informes. Los productos de SharePoint administran el escalado horizontal de servicios de informes a medida que los servidores SharePoint con el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se agregan a la granja.  
  
 Para aprender a escalar horizontalmente los servidores de informes en modo nativo, vea [Configurar una implementación escalada horizontalmente del servidor de informes en modo nativo &#40;Administrador de configuración del servidor de informes&#41;](../../reporting-services/install-windows/configure-a-native-mode-report-server-scale-out-deployment.md).  
  
##  <a name="load-balancing"></a><a name="bkmk_loadbalancing"></a> Equilibrio de carga  
 SharePoint administra automáticamente el equilibrio de carga de las aplicaciones de servicios de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a menos que el entorno tenga una solución personalizada o de terceros para el equilibrio de carga. El comportamiento predeterminado de equilibrio de carga de SharePoint consiste en que cada aplicación de servicios de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se equilibrará en todos los servidores de aplicaciones que ha iniciado el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Para comprobar si el servicio [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] está instalado y se ha iniciado, haga clic en **Administrar servicios en el servidor** en Administración central de SharePoint.  
  
##  <a name="prerequisites"></a><a name="bkmk_prerequisites"></a> Requisitos previos  
  
-   Debe ser administrador local para ejecutar el programa de instalación de SQL Server.  
  
-   El equipo debe estar unido a un dominio.  
  
-   Debe saber el nombre del servidor de bases de datos existente que hospeda la configuración de SharePoint y las bases de datos de contenido.  
  
-   El servidor de bases de datos debe estar configurado para permitir las conexiones de base de datos remota.  Si no lo está, no podrá combinar el nuevo servidor con la granja de servidores porque el nuevo servidor no podrá establecer una conexión a las bases de datos de configuración de SharePoint.  
  
-   El nuevo servidor deberá tener instalada la misma versión de SharePoint que están ejecutando los servidores actuales de la granja. Por ejemplo, si la granja de servidores ya tiene instalado SharePoint 2013 Service Pack 1 (SP1), deberá instalar también SP1 en el nuevo servidor para poder combinar la granja de servidores.  
  
##  <a name="steps"></a><a name="bkmk_steps"></a> Pasos  
 En los pasos de este tema se supone que el administrador de una granja de servidores de SharePoint va a instalar y configurar el servidor. En el diagrama se muestra un entorno típico de tres niveles. Los elementos numerados en el diagrama se describen en la siguiente lista:  
  
-   (1) Varios servidores front-end web (WFE). Los servidores WFE requieren el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] para SharePoint 2016.  
  
-   (2) Un único servidor de aplicaciones que ejecuta [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y los sitios web, por ejemplo, Administración central. Los pasos siguientes agregan un segundo servidor de aplicaciones en este nivel.  
  
-   (3) Dos servidores de base de datos de SQL Server.  
  
-   (4) Representa una solución de equilibrio de carga de red (NLB) de software o hardware  
  
 ![Agregar un servidor de aplicaciones de Reporting Services](../../reporting-services/install-windows/media/rs-sharepointscale.gif "Agregar un servidor de aplicaciones de Reporting Services")  
  
 En los siguientes pasos se supone que un administrador va a instalar y configurar el servidor. El servidor se configurará como un nuevo servidor de aplicaciones de la granja de servidores y no se usa como front-end web (WFE).  
  
|Paso|Descripción y vínculo|  
|----------|--------------------------|  
|Agregar un servidor de SharePoint a una granja de servidores.|Es necesario instalar SharePoint para implementar otra aplicación de Reporting Services.<br/><br/>Para SharePoint 2013, vea [Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2013](/SharePoint/install/add-web-or-application-server-to-the-farm).<br/><br/>Para SharePoint 2016, vea [Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm).|  
|Instalar y configurar el modo de SharePoint de Reporting Services.|Ejecute la instalación de SQL Server. Para obtener más información sobre la instalación del modo de SharePoint de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Instalación del primer servidor de informes en modo de SharePoint](install-the-first-report-server-in-sharepoint-mode.md).<br /><br /> Si el servidor únicamente se va a usar como servidor de aplicaciones y no como WFE, no es necesario seleccionar **Complemento de Reporting Services para productos de SharePoint**.<br /><br /> 1) En la página **Rol de instalación** , seleccione **Instalación de características de SQL Server**.<br /><br /> 2) En la página **Selección de características** , seleccione **Reporting Services - SharePoint**.<br /><br /> 3) En la página **Configuración de Reporting Services**  , compruebe que la opción **Solo instalar** está seleccionada para **Modo de SharePoint de Reporting Services**.|  
|Comprobar que Reporting Services está operativo.|1) En Administración central de SharePoint, haga clic en **Administrar servidores en esta granja de servidores** en el grupo **Configuración del sistema** .<br /><br /> 2) Compruebe el servicio **SQL Server Reporting Services**.<br /><br />Para obtener más información, vea [Verify a Reporting Services Installation](../../reporting-services/install-windows/verify-a-reporting-services-installation.md).|  
  
##  <a name="additional-configuration"></a><a name="bkmk_additional"></a> Configuración adicional  
 Puede optimizar servidores individuales de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en una implementación escalada horizontalmente para realizar el procesamiento en segundo plano únicamente ya que no compiten por los recursos con la ejecución de informes interactiva. El procesamiento en segundo plano incluye programaciones, suscripciones y alertas de datos.  
  
 Para cambiar el comportamiento de servidores de informes individuales, establezca **\<IsWebServiceEnable>** en false en el archivo de configuración **RSreportServer.config**.  
  
 De forma predeterminada, los servidores de informes están configurados con \<IsWebServiceEnable> establecido en TRUE. Cuando todos los servidores están configurados en TRUE, los interactivos y los de reserva tendrán equilibrio de carga en todos los nodos de la granja.  
  
 Si configura todos los servidores de informes con \<IsWebServiceEnable> establecido en False, verá un mensaje de error similar al siguiente al intentar usar las características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]:  
  
```output
The Reporting Services Web Service is not enabled. Configure at least one instance of the Reporting Services SharePoint Service to have <IsWebServiceEnable> set to true.
```
 
 Para obtener más información, vea [Modificar un archivo de configuración de Reporting Services &#40;RSreportserver.config&#41;](../../reporting-services/report-server/modify-a-reporting-services-configuration-file-rsreportserver-config.md).  

## <a name="next-steps"></a>Pasos siguientes

[Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2016](/SharePoint/install/add-a-server-to-a-sharepoint-server-2016-farm)  
[Agregar un servidor web o de aplicaciones a la granja de servidores en SharePoint 2013](/SharePoint/install/add-web-or-application-server-to-the-farm)

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).