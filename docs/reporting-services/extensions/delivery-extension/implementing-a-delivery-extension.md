---
title: Implementar una extensión de entrega | Microsoft Docs
description: Lea una introducción sobre cómo ampliar la funcionalidad de entrega en Reporting Services mediante la implementación de una extensión de entrega personalizada.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery [Reporting Services]
- custom delivery extensions [Reporting Services]
- extensions [Reporting Services], delivery
- delivery extensions [Reporting Services]
ms.assetid: 600cd229-efcd-480e-8c95-3c3c39ff4e7a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 902d5d19e4073cfcaf41f62c702da9ce59a191ff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064490"
---
# <a name="implementing-a-delivery-extension"></a>Implementar una extensión de entrega
  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] permite a los usuarios crear y publicar informes que, una vez creados y publicados, se pueden entregar en varias ubicaciones. Además, [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] incluye varias extensiones de entrega y una API de entrega que permite a los programadores crear extensiones de entrega adicionales para extender aún más la funcionalidad de entrega en [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 Para obtener una implementación de ejemplo de una extensión de entrega, vea [Ejemplos del producto SQL Server Reporting Services](https://go.microsoft.com/fwlink/?LinkId=177889).  
  
## <a name="in-this-section"></a>En esta sección  
 [Información general de las extensiones de entrega](../../../reporting-services/extensions/delivery-extension/delivery-extensions-overview.md)  
 Introduce cómo escribir una extensión de entrega personalizada para [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)].  
  
 [Preparación de la ejecución de una extensión de entrega](../../../reporting-services/extensions/delivery-extension/preparing-to-implement-a-delivery-extension.md)  
 Describe las interfaces y clases disponibles al implementar una extensión de entrega de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], así como cuestiones que hay que considerar antes de la implementación.  
  
 [Creación de una biblioteca de extensiones de entrega](../../../reporting-services/extensions/delivery-extension/creating-a-delivery-extension-library.md)  
 Describe cómo asignar un espacio de nombres para su extensión de entrega de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] y cómo compilarla en una biblioteca DLL.  
  
 [Ejecución de la interfaz IDeliveryExtension para una extensión de entrega](../../../reporting-services/extensions/delivery-extension/implementing-the-ideliveryextension-interface-for-a-delivery-extension.md)  
 Describe los atributos de una extensión de entrega y cómo implementar su propia clase de extensión de entrega.  
  
 [Uso de una clase Notification para una extensión de entrega](../../../reporting-services/extensions/delivery-extension/using-a-notification-class-for-a-delivery-extension.md)  
 Describe los atributos de una clase **Notification** y cómo usarla en la implementación de la extensión de entrega.  
  
 [Uso de la clase Setting para una extensión de entrega](../../../reporting-services/extensions/delivery-extension/using-the-setting-class-for-a-delivery-extension.md)  
 Describe los atributos de una clase **Setting** y cómo usarla en la implementación de la extensión de entrega.  
  
 [Uso de la clase Report para una extensión de entrega](../../../reporting-services/extensions/delivery-extension/using-the-report-class-for-a-delivery-extension.md)  
 Describe los atributos de una clase **Report** y cómo usarla en la implementación de la extensión de entrega.  
  
 [Uso de la clase RenderedOutputFile para una extensión de entrega](../../../reporting-services/extensions/delivery-extension/using-the-renderedoutputfile-class-for-a-delivery-extension.md)  
 Describe los atributos de una clase **RenderedOutputFile** y cómo usarla en la implementación de la extensión de entrega.  
  
 [Implementación de una extensión de entrega](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md)  
 Describe cómo implementar una extensión de entrega.  
  
 [Depuración del código de extensiones de entrega](../../../reporting-services/extensions/delivery-extension/debugging-delivery-extension-code.md)  
 Describe cómo depurar el código en una extensión de entrega.  
  
 [Eliminación de una extensión de entrega](../../../reporting-services/extensions/delivery-extension/removing-a-delivery-extension.md)  
 Describe cómo quitar una extensión de entrega de un servidor de informes.  
  
## <a name="see-also"></a>Consulte también  
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
