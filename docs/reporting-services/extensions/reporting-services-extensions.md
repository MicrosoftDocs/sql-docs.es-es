---
title: Extensiones de Reporting Services | Microsoft Docs
description: Obtenga información sobre cómo extender las funciones de procesamiento de datos en Reporting Services mediante la personalización de datos de informe, mecanismos de notificación y sistemas de seguridad.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- SQL Server Reporting Services, extending
- extensions [Reporting Services], about extensions
- SSIS, extending
- Reporting Services, extending
- extensions [Reporting Services]
ms.assetid: 2bf17ae4-2292-4a58-a1f0-56e99abd9b69
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b5572aca5e477c8738e46a628ce37f7690239173
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074896"
---
# <a name="reporting-services-extensions"></a>Extensiones de Reporting Services
  La arquitectura modular de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se ha diseñado para permitir ampliaciones. Hay una API de código administrado que permite desarrollar, instalar y administrar con facilidad las extensiones que usan numerosos componentes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] . Puede crear ensamblados privados o compartidos mediante [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] y agregar una nuevas funciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a fin de satisfacer sus necesidades empresariales en constante evolución.  
  
 La arquitectura de extensibilidad única de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] permite a los programadores extender características concretas del producto y sus componentes. Actualmente, hay numerosas funciones que permiten extender las capacidades de procesamiento de datos de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. La API de procesamiento de datos incluye construcciones conocidas del proveedor de datos de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] y convenciones con las que los programadores pueden integrar el procesamiento de datos adicional en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Estas extensiones de procesamiento de datos agregan funcionalidad al servidor de informes y al Diseñador de informes, lo que habilita la integración sin problemas de los datos personalizados en los informes.  
  
 Otra extensión admitida es la de entrega. La API de entrega se integra totalmente con la arquitectura de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], lo que permite usar una gran variedad de mecanismos de entrega al enviar notificaciones de informes a los usuarios. Puede extender el servidor de informes para proporcionar una entrega personalizada a los usuarios y extender las páginas de administración de suscripción del Administrador de informes para habilitar las suscripciones que usan extensiones de entrega personalizadas.  
  
 Otra extensión del servidor de informes, Extensión de personalización de definición de informe (RDCE, Report Definition Customization Extension), puede personalizar dinámicamente una definición de informe antes de pasarse al motor del procesamiento. Podría personalizar los informes según factores como los usuarios o los idiomas. Por ejemplo, podría desear implementar vistas diferentes para varios usuarios, por ejemplo, para los administradores o los miembros de un departamento, o personalizar un informe para que tuviera un diseño diferente cuando se representara en francés o en árabe.  
  
## <a name="in-this-section"></a>En esta sección  
 [Consideraciones de seguridad para las extensiones](../../reporting-services/extensions/security-considerations-for-extensions.md)  
 Describe problemas de seguridad relacionados con el desarrollo e implementación de extensiones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 [Implementación de una extensión de procesamiento de datos](../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)  
 Describe los requisitos y los pasos para implementar una extensión de procesamiento de datos para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 [Ejecución de una extensión de entrega](../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)  
 Describe los requisitos y los pasos para implementar una extensión de entrega para [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 [Ejecución de una extensión de representación](../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)  
 Contiene una introducción para desarrollar extensiones de representación.  
  
 [Implementación de una extensión de seguridad](../../reporting-services/extensions/security-extension/implementing-a-security-extension.md)  
 Describe los requisitos y los pasos para implementar una extensión de seguridad de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 [Biblioteca de extensiones de Reporting Services](../../reporting-services/extensions/reporting-services-extension-library.md)  
 Contiene la referencia de programación de la biblioteca de la API de extensiones para las características de extensibilidad de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
  
