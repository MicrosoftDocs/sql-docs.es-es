---
title: Integración de aplicaciones
description: Reporting Services es una plataforma de informe abierta y extensible diseñada para proporcionar un conjunto completo de API a los programadores para desarrollar soluciones.
ms.custom: seo-lt-2019
ms.date: 05/14/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.topic: reference
author: maggiesMSFT
ms.author: maggies
monikerRange: = sql-server-2016
ms.openlocfilehash: 32f366bcce7ce8fd40890cd2a383a41518b78758
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97466676"
---
# <a name="integrating-reporting-services-into-applications"></a>Integrar Reporting Services en las aplicaciones

[!INCLUDE [ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016](../../includes/ssrs-appliesto-2016.md)] [!INCLUDE[ssrs-appliesto-not-2017](../../includes/ssrs-appliesto-not-2017.md)] [!INCLUDE [ssrs-appliesto-not-pbirs](../../includes/ssrs-appliesto-not-pbirs.md)]

  [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] es una plataforma de informe abierta y extensible diseñada para proporcionar un conjunto completo de API a los programadores para desarrollar soluciones.

> [!NOTE]
> A partir de SQL Server 2017 Reporting Services, el acceso de API de REST está disponible para el desarrollo de soluciones. El acceso de API de SOAP está en desuso. Para más información, vea [Desarrollar con las API de REST para Reporting Services](../developer/rest-api.md).
  
 Existen tres opciones para integrar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en aplicaciones personalizadas: el servicio web del servidor de informes, que también se denomina API de SOAP de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], los controles ReportViewer para [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] y el acceso URL. Cada opción proporciona un enfoque diferente para integrar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en las aplicaciones.
  
## <a name="report-server-web-service"></a>Servicio web del servidor de informes

 El servicio web del servidor de informes es la interfaz principal para el desarrollo de soluciones con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Si está desarrollando código para administrar un catálogo de informe o representar informes en un formato admitido, el servicio web expone todos los métodos necesarios para integrar [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en las aplicaciones. Un ejemplo de tal aplicación es el portal web, que se incluye con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]; usa el servicio web para administrar la base de datos del servidor de informes.  
  
## <a name="report-viewer-controls-for-visual-studio"></a>Controles ReportViewer para Visual Studio

 Los controles ReportViewer disponibles para [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] se usan para integrar la vista del informe en las aplicaciones. Hay dos controles: uno para las aplicaciones basadas en formularios Windows Forms y otro para las aplicaciones de formularios Web Forms. Cada control permite ver los informes implementados en un servidor de informes así como la capacidad de representar los informes que existen en un entorno donde no se haya instalado un servidor de informes.  
  
## <a name="url-access"></a>acceso URL  
 El acceso URL es otra opción para integrar la vista del informe en las aplicaciones si los controles ReportViewer no son factibles. Además, el acceso URL es útil para enviar vínculos a informes a los usuarios a través de correo electrónico.  
  
## <a name="in-this-section"></a>En esta sección

 [Integración de Reporting Services con SOAP](../../reporting-services/application-integration/integrating-reporting-services-using-soap.md)  
 Describe cómo integrar la navegación en informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y la administración en las aplicaciones empresariales existentes utilizando el servicio web del servidor de informes.  
  
 [Integración de Reporting Services con los controles del Visor de informes](../../reporting-services/application-integration/integrating-reporting-services-using-reportviewer-controls.md)  
 Describe cómo integrar la vista del informe en las aplicaciones existentes utilizando controles ReportViewer.  
  
 [Integración de Reporting Services con el acceso URL](../../reporting-services/application-integration/integrating-reporting-services-using-url-access.md)  
 Describe cómo integrar la navegación en informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en las aplicaciones empresariales existentes utilizando el acceso URL.  
  
## <a name="next-steps"></a>Pasos siguientes

Para decidir sobre el uso de acceso URL o API de SOAP, vea [Elegir entre acceso URL y SOAP en Reporting Services](choosing-between-url-access-and-soap.md).

Para más información sobre la API de REST de SQL Server 2017 Reporting Services, vea [Desarrollo con las API de REST para Reporting Services](../developer/rest-api.md).

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
