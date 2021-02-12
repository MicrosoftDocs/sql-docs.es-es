---
title: Información general de las extensiones de entrega | Microsoft Docs
description: Lea una introducción sobre las extensiones de entrega, que puede usar para entregar informes de Reporting Services de varias maneras, como por ejemplo, mediante correo electrónico o el uso compartido de archivos.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- subscriptions [Reporting Services], delivery extensions
- delivery extensions [Reporting Services], about extensions
ms.assetid: a30600a9-bbed-4519-9426-3470ff2982e7
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8eecb8ce351fd067d944daf4fdd7fc9ee811341f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076659"
---
# <a name="delivery-extensions-overview"></a>Información general de las extensiones de entrega
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permite a los usuarios crear y publicar informes que, una vez creados y publicados, se pueden entregar en varias ubicaciones. Además, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] incluye varias extensiones de entrega y una API de entrega que permite a los programadores crear extensiones de entrega adicionales para extender aún más la funcionalidad de entrega en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 En la tabla siguiente se enumeran las extensiones de entrega incluidas con [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
|Extensión de entrega|Descripción|  
|------------------------|-----------------|  
|Correo electrónico del servidor de informes|Utiliza un servidor SMTP para enviar informes a usuarios individuales o grupos por correo electrónico.|  
|Recurso compartido del servidor de informes|Se utiliza para distribuir los informes dentro de una organización a los recursos compartidos de archivos de red. Permite copiar automáticamente un informe en un recurso compartido de archivos con una programación designada.|  
  
 ![Arquitectura de extensión de entrega de Reporting Services](../../../reporting-services/extensions/delivery-extension/media/bk-reportservicedelivery.gif "Arquitectura de extensión de entrega de Reporting Services")  
Arquitectura de extensión de entrega de Reporting Services  
  
 Las extensiones de entrega se emparejan con las suscripciones. Al crear una suscripción, puede elegir una de las extensiones de entrega disponibles para determinar cómo se entrega el informe. En [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], las suscripciones se encuentran en la base de datos del servidor de informes. Cuando se produce un evento, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] hace corresponder el evento con las suscripciones contenidas en la base de datos del servidor de informes. El servidor de informes crea una notificación para cada suscripción enlazada al evento. En las suscripciones controladas por datos se crea una notificación para cada destinatario. Una vez creada una notificación, el servidor de informes invoca una extensión de entrega determinada y pasa los valores para la configuración de las extensiones especificada en la notificación. La extensión de entrega envía la notificación al usuario cuando lo especifica la extensión de entrega seleccionada.  
  
 Las extensiones de entrega implementan la API de extensiones de entrega de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)]. Al admitir la API de extensiones de entrega de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], las extensiones de entrega pueden recibir las notificaciones del servidor de informes y proporcionar el estado de la notificación.  
  
 El servidor de informes no administra los destinos de entrega para las notificaciones e informes. La recopilación de la información de destino se realiza a través de código que se escribe en la extensión de entrega.  
  
## <a name="subscriptions-and-delivery-extensions"></a>Suscripciones y extensiones de entrega  
 Las aplicaciones cliente crean suscripciones que utilizan extensiones de entrega usando dos métodos del servicio web del servidor de informes: <xref:ReportService2010.ReportingService2010.CreateSubscription%2A> y <xref:ReportService2010.ReportingService2010.CreateDataDrivenSubscription%2A>. Para modificar suscripciones que ya existen, se usan los métodos <xref:ReportService2010.ReportingService2010.SetSubscriptionProperties%2A> y <xref:ReportService2010.ReportingService2010.SetDataDrivenSubscriptionProperties%2A>. Al crear una suscripción, el usuario también selecciona una extensión de entrega para la suscripción y escribe los valores de las opciones de extensión necesarias. Cuando un usuario guarda una suscripción, se almacena en la base de datos del servidor de informes. Las suscripciones crean las notificaciones según una programación o un evento. Cuando una entrega comienza, la extensión de entrega seleccionada carga primero cualquier dato de configuración desde el archivo de configuración. Luego, se recupera la configuración de extensión para la suscripción y se establecen los valores. Finalmente, se llama al método <xref:Microsoft.ReportingServices.Interfaces.IDeliveryExtension.Deliver%2A> y se envía la notificación.  
  
## <a name="developer-requirements"></a>Requisitos para el programador  
 Para desarrollar una extensión de entrega de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] es necesario que tenga:  
  
-   Un equipo de implementación con un servidor de informes instalado.  
  
-   Un equipo de desarrollo con [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] o el Kit de desarrollo de software (SDK) de [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] instalado.  
  
-   Una comprensión detallada de las características y capacidades de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], en concreto, la suscripción y la entrega.  
  
-   Una comprensión detallada de los controles web y de [!INCLUDE[vstecasp](../../../includes/vstecasp-md.md)], si planea implementar su propia interfaz de usuario de suscripción para el Administrador de informes.  
  
-   Experiencia de desarrollo en un lenguaje de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] como [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C# o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] .NET.  
  
## <a name="see-also"></a>Consulte también  
 [Implementar una extensión de entrega](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
