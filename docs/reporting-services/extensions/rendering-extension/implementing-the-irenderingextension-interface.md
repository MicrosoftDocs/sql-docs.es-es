---
title: Implementar la interfaz IRenderingExtension | Microsoft Docs
description: Obtenga información sobre cómo implementar la interfaz IRenderingExtension, que transforma los datos del informe en formatos de salida para usarlos en visores, impresoras y otros destinos.
ms.date: 03/16/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- IRenderingExtension interface
- rendering extensions [Reporting Services], IRenderingExtension interface
ms.assetid: 74b2f2b7-6796-42da-ab7d-b05891ad4001
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 984775286d4f32d0c1e844bc6ba0ca62bcf198ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042185"
---
# <a name="implementing-the-irenderingextension-interface"></a>Implementar la interfaz IRenderingExtension
  La extensión de representación toma los resultados de una definición de informe que se combina con los datos reales y representa los datos resultantes en un formato que se puede usar. La transformación de los datos combinados y el formato se efectúan utilizando una clase de Common Language Runtime (CLR) que implementa <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension>. De esta forma se transforma el modelo de objetos en un formato de salida que puede usar un visor, impresora u otro destino de salida.  
  
 <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension> tiene tres métodos que deben estar codificados:  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A>: representa el informe.  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A>: representa un flujo concreto del informe.  
  
-   <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A>: obtiene la información adicional, como iconos, necesaria para el informe.  
  
 En las siguientes secciones se describen los métodos con más detalle.  
  
## <a name="render-method"></a>Método Render  
 El método <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> contiene argumentos que representan los objetos siguientes:  
  
-   El elemento *report* que quiere representar. Este objeto contiene propiedades, datos e información de presentación para el informe. El informe es la raíz del árbol del modelo de objetos de informe.  
  
-   El parámetro *ServerParameters* que contiene el objeto de diccionario de cadena, con los parámetros para el servidor de informes, si los hubiera.  
  
-   El parámetro *deviceInfo* que contiene la configuración del dispositivo. Para más información, vea [Pasar la configuración de información de dispositivo a las extensiones de representación](../../../reporting-services/report-server-web-service/net-framework/passing-device-information-settings-to-rendering-extensions.md).  
  
-   El parámetro *clientCapabilities* que contiene un objeto de diccionario <xref:System.Collections.Specialized.NameValueCollection> que tiene información sobre el cliente al que está representando.  
  
-   Parámetro *RenderProperties* que contiene información sobre el resultado de la representación.  
  
-   *createAndRegisterStream* es una función delegada a la que se va a llamar para obtener una secuencia en la que representar.  
  
### <a name="deviceinfo-parameter"></a>Parámetro de deviceInfo  
 El parámetro *deviceInfo* contiene los parámetros de representación, no los parámetros de informe. Estos parámetros de representación se pasan a la extensión de representación. El servidor de informes convierte los valores de *deviceInfo* en un objeto <xref:System.Collections.Specialized.NameValueCollection>. Los elementos del parámetro *deviceInfo* se tratan como valores que no distinguen mayúsculas y minúsculas. Si la solicitud de representación se ha producido como resultado de un acceso URL, los parámetros de dirección URL con el formato `rc:key=value` se convierten en pares de clave-valor en el objeto de diccionario *deviceInfo*. El código de detección de explorador también proporciona los elementos siguientes en el diccionario *clientCapabilities*: EcmaScriptVersion, JavaScript, MajorVersion, MinorVersion, Win32, Type y AcceptLanguage. Se omite cualquier par de nombre-valor del parámetro *deviceInfo* que la extensión de representación no entienda. La muestra de código siguiente muestra un método <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> de muestra que recupera los iconos.  
  
```csharp  
public void GetRenderingResource (CreateStream createStreamCallback, NameValueCollection deviceInfo)  
{  
    string[] iconTagValues = deviceInfo.GetValues("Icon");  
    if ((iconTagValues != null) && (iconTagValues.Length > 0) )  
    {  
        // Create a stream to output to.  
        Stream outputStream = createStreamCallback(m_iconResourceName, "gif", null, "image/gif", false);  
        // Get the GIF image for one of the buttons on the toolbar  
        Image requiredImage = (Image) m_resourcemanager.GetObject(m_iconResourceName  
        // Write the image to the output stream  
        requiredImage.Save(outputStream, requiredImage.RawFormat);  
    }  
    return;  
}  
```  
  
## <a name="renderstream-method"></a>Método RenderStream  
 El método <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.RenderStream%2A> representa un flujo determinado del informe. Todos los flujos se crean durante la llamada de <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.Render%2A> inicial, pero los flujos no se devuelven inicialmente al cliente. Este método se utiliza para flujos secundarios (similares a las imágenes en la representación HTML) o páginas adicionales de una extensión de representación de varias páginas (similar a Image/EMF).  
  
## <a name="getrenderingresource-method"></a>Método GetRenderingResource  
 El método <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A> recupera la información sin ejecutar una representación completa del informe. Hay ocasiones en que el informe requiere información que no requiere representar el propio informe. Por ejemplo, si necesita el icono asociado a la extensión de representación, use el parámetro *deviceInfo* que contiene la etiqueta única **\<Icon>** . En estos casos, puede usar el método <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension.GetRenderingResource%2A>.  
  
## <a name="see-also"></a>Consulte también  
 [Implementar una extensión de representación](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Información general de las extensiones de representación](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)  
  
  
