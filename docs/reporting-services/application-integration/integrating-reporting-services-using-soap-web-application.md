---
title: Uso de la API de SOAP en aplicaciones web
description: Puede acceder a la funcionalidad del servidor de informes a través de la API SOAP de Reporting Services, a la que se puede acceder para proporcionar características de informes empresariales.
ms.date: 06/26/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- SOAP [Reporting Services], Web applications
- impersonation [Reporting Services]
- user impersonation [Reporting Services]
- report servers [Reporting Services], SOAP
- Web applications [Reporting Services]
ms.assetid: e8ca4455-0dc3-4741-8872-3636114938ad
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8a8f6182e4579d30cfcb4a8102a439bb7b12c7a6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071150"
---
# <a name="integrating-reporting-services-using-soap---web-application"></a>Integración de Reporting Services con SOAP: aplicación web
  Puede tener acceso a la funcionalidad completa del servidor de informes a través de la API SOAP de Reporting Services. Dado que es un servicio web, se puede tener acceso con facilidad a esta API para proporcionar características de informes de empresa para aplicaciones empresariales personalizadas. Para tener acceso al servicio web del servidor de informes desde una aplicación web, se usa casi el mismo proceso que en el acceso a la API SOAP desde una aplicación para [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows. Con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], puede generar una clase de proxy que exponga las propiedades y los métodos del servicio web del servidor de informes y le permita usar una infraestructura y herramientas conocidas para compilar las aplicaciones empresariales en la tecnología [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 El acceso a la funcionalidad de administración de informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se realiza con tanta facilidad desde una aplicación web como desde una aplicación para Windows. Desde una aplicación web, puede agregar y quitar los elementos de la base de datos del servidor de informes, establecer la seguridad de los elementos, modificar los elementos de la base de datos del servidor de informes, administrar la programación y la entrega, etcétera.  
  
## <a name="enabling-impersonation"></a>Habilitar la suplantación  
 El primer paso para configurar una aplicación web es habilitar la suplantación desde el cliente de servicios web. Con la suplantación, las aplicaciones de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] pueden ejecutarse con la identidad del cliente en cuyo el nombre operan. [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] se basa en [!INCLUDE[msCoName](../../includes/msconame-md.md)] Internet Information Services (IIS) para autenticar al usuario y pasar un token autenticado a la aplicación de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] o, si no se puede autenticar al usuario, pasar un token sin autenticar. En cualquier caso, la aplicación de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] suplanta al token que se reciba, si está habilitada la suplantación. Puede habilitar la suplantación en el cliente modificando el archivo Web.config de la aplicación cliente como sigue:  
  
```asp.net  
<!-- Web.config file. -->  
<identity impersonate="true"/>  
```  
  
> [!NOTE]  
>  De manera predeterminada, la suplantación está deshabilitada.  
  
 Para más información sobre la suplantación de [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)], vea la documentación del SDK de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
## <a name="managing-the-report-server-using-soap-api"></a>Administrar el servidor de informes mediante la API SOAP  

::: moniker range="=sql-server-2016"

 También puede utilizar la aplicación web para administrar un servidor de informes y su contenido. El Administrador de informes, que se incluye con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], es un ejemplo de aplicación web que se genera completamente utilizando [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)] y la API SOAP de Reporting Services. Puede agregar la funcionalidad de administración de informes del Administrador de informes a sus aplicaciones web personalizadas. Por ejemplo, podría querer devolver una lista de los informes disponibles en la base de datos del servidor de informes y mostrarlos en un control [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]Listbox **de** para que los usuarios puedan elegir. El código siguiente se conecta a la base de datos del servidor de informes y devuelve una lista de los elementos de la base de datos del servidor de informes. A continuación, los informes disponibles se agregan a un control Listbox, que muestra la ruta de acceso de cada informe.  

::: moniker-end

::: moniker range=">=sql-server-2017"

 También puede utilizar la aplicación web para administrar un servidor de informes y su contenido. El portal web, incluido con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], es un ejemplo de una aplicación web que administra una mayoría de las tareas que se realizarían normalmente con Reporting Services. Puede agregar la funcionalidad de administración de informes del portal web a sus aplicaciones web personalizadas. Por ejemplo, podría querer devolver una lista de los informes disponibles en la base de datos del servidor de informes y mostrarlos en un control [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]Listbox **de** para que los usuarios puedan elegir. El código siguiente se conecta a la base de datos del servidor de informes y devuelve una lista de los elementos de la base de datos del servidor de informes. A continuación, los informes disponibles se agregan a un control Listbox, que muestra la ruta de acceso de cada informe.  

::: moniker-end
  
```vb  
Private Sub Page_Load(sender As Object, e As System.EventArgs)  
   ' Create a Web service proxy object and set credentials  
   Dim rs As New ReportingService2005()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return a list of catalog items in the report server database  
   Dim items As CatalogItem() = rs.ListChildren("/", True)  
  
   ' For each report, display the path of the report in a Listbox  
   Dim ci As CatalogItem  
   For Each ci In  items  
      If ci.Type = ItemTypeEnum.Report Then  
         catalogListBox.Items.Add(ci.Path)  
      End If  
   Next ci  
End Sub ' Page_Load   
```  
  
```csharp  
private void Page_Load(object sender, System.EventArgs e)  
{  
   // Create a Web service proxy object and set credentials  
   ReportingService2005 rs = new ReportingService2005();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return a list of catalog items in the report server database  
   CatalogItem[] items = rs.ListChildren("/", true);  
  
   // For each report, display the path of the report in a Listbox  
   foreach(CatalogItem ci in items)  
   {  
      if (ci.Type == ItemTypeEnum.Report)  
         catalogListBox.Items.Add(ci.Path);  
   }  
}  
```  
  
## <a name="see-also"></a>Consulte también  

 [Creación de aplicaciones con el servicio web y .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Integración de Reporting Services en las aplicaciones](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Usar la API de SOAP en una aplicación Windows](../../reporting-services/application-integration/integrating-reporting-services-using-soap-windows-application.md)  
  
  
