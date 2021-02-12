---
title: Servicio web del servidor de informes | Microsoft Docs
description: Reporting Services proporciona la funcionalidad del servidor de informes con el servicio web de dicho servidor, un punto de conexión de servicio SOAP que sirve para ejecutar y administrar informes.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-web-service
ms.topic: reference
helpviewer_keywords:
- SSIS, Web service
- Web service [Reporting Services]
- Reporting Services, extending
- SQL Server Reporting Services, Web service
- Reporting Services, Web service
- XML Web service [Reporting Services]
- Report Server Web service
ms.assetid: 16c21dec-6b46-4497-9a0c-1b0f2b6ab8fc
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 55160717791f4d14a7f008725710f725069cd6d4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100022129"
---
# <a name="report-server-web-service"></a>servicio web del servidor de informes
  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona acceso a la funcionalidad completa del servidor de informes a través del servicio web de dicho servidor. El servicio web del servidor de informes es un servicio web XML con una API SOAP. Utiliza SOAP sobre HTTP y actúa como interfaz de comunicaciones entre los programas clientes y el servidor de informes. El servicio web proporciona dos extremos, uno para la ejecución y otro para la administración de informes, con métodos que exponen la funcionalidad del servidor de informes y le permiten crear herramientas personalizadas para cualquier parte del ciclo de vida del informe.  
  
 Hay tres modos principales para desarrollar aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] basadas en el servicio web. Puede:  
  
-   Desarrollar aplicaciones con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] y el SDK de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Para más información sobre cómo usar [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] para generar las aplicaciones del servicio web, vea [Creación de aplicaciones con el servicio Web y .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md).  
  
-   Desarrollar aplicaciones con la utilidad **rs** (RS.exe), el entorno de script de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Con los scripts de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] puede ejecutar cualquiera de las operaciones del servicio web del servidor de informes. Para más información sobre cómo crear scripts en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Script con la utilidad rs.exe y el servicio web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md).  
  
-   Desarrollar aplicaciones con cualquier conjunto habilitado para SOAP de herramientas de desarrollo. Para más información, vea [El rol de SOAP en Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md).  
  
## <a name="programming-diagram"></a>Diagrama de programación  
 ![Opciones de desarrollo del servicio web del servidor de informes](../../reporting-services/report-server-web-service/media/reportserviceswebserviceprog-01.gif "Opciones de desarrollo del servicio web del servidor de informes")  
Opciones de desarrollo de servicio web disponibles en Reporting Services  
  
## <a name="in-this-section"></a>En esta sección  
 [Métodos de servicio web del servidor de informes](../../reporting-services/report-server-web-service/methods/report-server-web-service-methods.md)  
 Describe las características y métodos de cada servicio web del servidor de informes.  
  
 [El rol de SOAP en Reporting Services](../../reporting-services/report-server-web-service/the-role-of-soap-in-reporting-services.md)  
 Proporciona información general de SOAP y de cómo se utiliza en los servicios web del servidor de informes.  
  
 [Acceso a la API SOAP](../../reporting-services/report-server-web-service/accessing-the-soap-api.md)  
 Describe el lenguaje de descripción de servicios web (WSDL) y proporciona direcciones URL para tener acceso a un archivo WSDL de Reporting Services.  
  
 [Creación de aplicaciones con el servicio web y .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)  
 Contiene información sobre cómo desarrollar aplicaciones y servicios web que llaman a la API SOAP de Reporting Services.  
  
 [Script con la utilidad rs.exe y el servicio web](../../reporting-services/tools/script-with-the-rs-exe-utility-and-the-web-service.md)  
 Proporciona información general del entorno de scripting de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 [Referencia técnica &#40;SSRS&#41;](../../reporting-services/technical-reference-ssrs.md)  
 Contiene material de referencia específico de los métodos de servicios web del servidor de informes y los tipos complejos correspondientes.  
  
## <a name="user-requirements-for-web-service-development"></a>Requisitos del usuario para el desarrollo de servicios web  
 Para desarrollar aplicaciones con el servicio web del servidor de informes, necesita:  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Explorer 5.5 o posteriores instalados en un equipo con una conexión a Internet a y acceso al servidor de informes.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] o el SDK de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] instalados en un equipo si quiere desarrollar e implementar aplicaciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] mediante [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
-   Una comprensión detallada de las características y capacidades de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
-   Conocimientos sólidos de SOAP y [!INCLUDE[vstecwebservices](../../includes/vstecwebservices-md.md)].  
  
-   Experiencia en desarrollo con un lenguaje compatible con [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] como [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../includes/csprcs-md.md)] o [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], si piensa usar [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] como plataforma de desarrollo.  
  
## <a name="see-also"></a>Consulte también  
 [Servicio web del servidor de informes](../../reporting-services/report-server-web-service/report-server-web-service.md)  
  
  
