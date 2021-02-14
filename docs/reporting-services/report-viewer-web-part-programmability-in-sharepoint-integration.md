---
title: Capacidad de programación del elemento web Visor de informes en la integración de SharePoint | Microsoft Docs
description: Obtenga información sobre cómo crear elementos web personalizados que proporcionan la ruta de acceso del informe y los parámetros al elemento web Visor de informes mediante conexiones de elementos web.
ms.date: 03/04/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: reference
ms.assetid: 714017b7-1bd6-4950-a3c6-d0df8450a877
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d7054e2af081bd76505f3adebbae4cc7541f8a44
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100023279"
---
# <a name="report-viewer-web-part-programmability-in-sharepoint-integration"></a>Capacidad de programación del elemento web Visor de informes en la integración de SharePoint
  El elemento web Visor de informes es un control de servidor que contiene un conjunto de interfaces de programación de aplicaciones (API) públicas que permite a los desarrolladores crear aplicaciones de SharePoint personalizadas. Puede crear elementos web personalizados que proporcionan la ruta de acceso del informe y los parámetros al elemento web Visor de informes usando conexiones de elementos web. También puede incrustar el elemento web en una página personalizada de elemento web de SharePoint y personalizarlo usando la API pública.  
  
## <a name="connecting-to-report-viewer-web-part-with-custom-web-parts"></a>Conectar con el elemento web Visor de informes con elementos webs personalizados  
 El elemento web Visor de informes es un consumidor de conexión con elementos web de SharePoint que implementan <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> o T:Microsoft.SharePoint.WebPartPages.IFilterValues. Un elemento web <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>, como **Documents**, puede proporcionar una ruta de informe a un elemento web Visor de informes cuando se coloca en la misma página de elementos web que el elemento web Visor de informes. Del mismo modo, un elemento web T:Microsoft.SharePoint.WebPartPages.IFilterValues, como **Filtro de texto** o **Filtro de opciones**, puede proporcionar un parámetro de informe a un elemento web Visor de informes cuando se coloca en la misma página de elementos web que el elemento web Visor de informes.  
  
### <a name="implementing-a-report-path-provider-with-iwebpartrow"></a>Implementar un proveedor de ruta de acceso de informe con IWebPartRow  
 Para proporcionar una ruta de acceso del informe al elemento web Visor de informes a través de las conexiones del elemento, haga lo siguiente:  
  
1.  Cree un elemento web que implemente la interfaz de <xref:System.Web.UI.WebControls.WebParts.IWebPartRow>.  
  
2.  Agregue un elemento web a la misma página del elemento web Visor de informes.  
  
3.  Conecte el elemento web con el elemento web Visor de informes en la interfaz del usuario cuyo diseño de elemento web está basado en la Web.  
  
    > [!NOTE]  
    >  Solo puede conectar un elemento web <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> con el elemento web Visor de informes cada vez y no puede conectar al mismo tiempo un elemento web <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> y un elemento web T:Microsoft.SharePoint.WebPartPages.IFilterValues al elemento web Visor de informes.  
  
 Para que el elemento web <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> funcione correctamente con T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart, debe hacer lo siguiente en el método <xref:System.Web.UI.WebControls.WebParts.IWebPartRow.GetRowData%2A>:  
  
-   Invoque el método de devolución de llamada con un objeto <xref:System.Data.DataRowView> como parámetro de entrada.  
  
-   Asegurarse de que el objeto <xref:System.Data.DataRowView> contiene una columna llamada "DocUrl" que contiene la ruta de acceso al informe.  
  
    > [!NOTE]  
    >  El elemento web Visor de informes del complemento para [!INCLUDE[offSPServ](../includes/offspserv-md.md)] 2010 también permite recibir los datos de ruta de acceso de informe mediante la columna "FileRef".  
  
### <a name="implementing-a-report-parameter-provider-with-ifiltervalues"></a>Implementar un proveedor de parámetros de informe con IFilterValues  
 Un elemento web que implementa T:Microsoft.SharePoint.WebPartPages.IFilterValues puede proporcionar un valor de parámetro al elemento web Visor de informes. El valor de parámetro enviado al elemento web Visor de informes está sujeto a las mismas restricciones del parámetro de informe cuando se especificó la definición de informe, como tipo de datos, valores válidos, etc.  
  
 Para proporcionar un parámetro de informe al elemento web Visor de informes, haga lo siguiente:  
  
1.  Cree un elemento Web que implemente la interfaz T:Microsoft.SharePoint.WebPartPages.IFilterValues.  
  
2.  Agregue el elemento web a la misma página que T:Microsoft.ReportingServices.SharePoint.UI.WebParts.ReportViewerWebPart.  
  
3.  Conecte el elemento web T:Microsoft.SharePoint.WebPartPages.IFilterValues con el elemento web Visor de informes en la interfaz de usuario de diseño de elementos web basada en web.  
  
    > [!NOTE]  
    >  Puede conectar varios elementos web T:Microsoft.SharePoint.WebPartPages.IFilterValues con el elemento web Visor de informes a la vez. Pero no puede conectar un elemento web <xref:System.Web.UI.WebControls.WebParts.IWebPartRow> y un elemento web T:Microsoft.SharePoint.WebPartPages.IFilterValues con el elemento web Visor de informes a la vez.  
  
  
