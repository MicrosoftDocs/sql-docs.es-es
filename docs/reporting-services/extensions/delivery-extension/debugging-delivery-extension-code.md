---
title: Depurar el código de extensión de entrega | Microsoft Docs
description: Obtenga información sobre cómo usar las herramientas de depuración de Microsoft .NET Framework para analizar el código de la extensión de entrega y localizar errores en él.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- delivery extensions [Reporting Services], debugging
- debugging delivery extensions [Reporting Services]
- troubleshooting [Reporting Services], delivery extensions
ms.assetid: a7d959da-5005-4a50-aca7-2cef36aa9947
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 830ff0bf030ad81f02f915ff50078baef9d57600
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060970"
---
# <a name="debugging-delivery-extension-code"></a>Depurar el código de extensión de entrega
  [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)] proporciona varias herramientas de depuración que pueden ayudarle a analizar el código de extensión de entrega y localizar errores en él. La herramienta más conveniente dependerá de lo que intente llevar a cabo. En este ejemplo se usa [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)].  
  
#### <a name="to-debug-your-delivery-extension-code"></a>Para depurar el código de extensión de entrega  
  
1.  Inicie [!INCLUDE[vsOrcas](../../../includes/vsorcas-md.md)] y abra el proyecto de extensión de entrega.  
  
2.  Genere el proyecto e implemente el ensamblado de extensión de entrega y el archivo .pdb acompañante en el servidor de informes y el Administrador de informes. Para más información sobre cómo implementar extensiones, vea [Implementar una extensión de entrega](../../../reporting-services/extensions/delivery-extension/deploying-a-delivery-extension.md).  
  
3.  Si ha escrito una interfaz de usuario de suscripción para extender el Administrador de informes, ha abierto Internet Explorer y ha navegado al Administrador de informes mientras dejaba abierto el código de extensión de entrega en [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. Si no tiene implementada una interfaz de usuario de suscripción para el Administrador de informes, simplemente abra la aplicación cliente desde la que llama a la extensión de entrega utilizando la API SOAP.  
  
4.  Navegue a [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] y al proyecto de extensión de entrega, y establezca algunos puntos de interrupción en el código.  
  
5.  Con el proyecto de extensión de entrega aún en la ventana activa, haga clic en **Asociar al proceso** en el menú **Depurar**.  
  
     Se abre el cuadro de diálogo **Asociar al proceso**.  
  
6.  En la lista de procesos, seleccione el proceso aspnet_wp.exe (o w3wp.exe, si la aplicación se implementa en IIS 6.0) y haga clic en **Asociar**.  
  
7.  Defina una nueva suscripción mediante su extensión de entrega. Probablemente utilizará el Administrador de informes o la API SOAP. De esta forma se debería invocar el depurador y ejecutar el código correspondiente a sus puntos de interrupción.  
  
8.  Recorra el código con la tecla **F11**. Para obtener más información sobre cómo utilizar [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] para la depuración, vea la documentación de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Implementar una extensión de entrega](../../../reporting-services/extensions/delivery-extension/implementing-a-delivery-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
