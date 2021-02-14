---
title: Uso de la API de SOAP en aplicaciones Windows
description: Puede acceder a la funcionalidad del servidor de informes a través de la API SOAP de Reporting Services. Para acceder al servicio web en una aplicación Windows, realice llamadas al servicio.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: application-integration
ms.custom: seo-lt-2019
ms.topic: reference
helpviewer_keywords:
- rendered reports [Reporting Services]
- Windows applications [Reporting Services]
- Windows Forms [Reporting Services]
- SOAP [Reporting Services], Windows applications
ms.assetid: e4804792-20cd-4df2-9257-fb958ff447b4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a91f7cd3a95b722db1c595767ed71900bc55af55
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082566"
---
# <a name="integrating-reporting-services-using-soap---windows-application"></a>Integrar Reporting Services con SOAP: aplicación Windows
  Puede tener acceso a la funcionalidad completa del servidor de informes a través de la API SOAP de Reporting Services. Se trata de un servicio web y, como tal, se puede tener acceso con facilidad al mismo con el fin de proporcionar características de informes de empresa para aplicaciones empresariales personalizadas. Puede tener acceso al servicio web en una aplicación Windows simplemente escribiendo código que realice llamadas al servicio. Con [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], puede generar una clase de proxy que exponga las propiedades y los métodos del servicio web y le permita usar una infraestructura y herramientas conocidas para compilar aplicaciones empresariales integradas en la tecnología [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
## <a name="integrating-report-management-functionality-using-windows-forms"></a>Integrar la funcionalidad de administración de informes usando formularios Windows Forms  
 A diferencia del acceso URL, la API SOAP expone el conjunto completo de funciones de administración que están disponibles a través del servidor de informes. Esto significa que toda la funcionalidad administrativa del Administrador de informes está disponible para los programadores a través de SOAP. Como tal, puede desarrollar una completa herramienta de administración utilizando formularios Windows Forms. Por ejemplo, en una aplicación Windows podría desear permitir que los usuarios recuperaran el contenido del espacio de nombres del servidor de informes. Para ello, podría usar el método <xref:ReportService2010.ReportingService2010.ListChildren%2A> del servicio web para mostrar todos los elementos de la base de datos del servidor de informes y, a continuación, usar un control Listview, Treeview o Combobox para mostrar esos elementos a los usuarios. El código del servicio web siguiente se podría utilizar para recuperar la lista actual de informes disponibles en la carpeta Mis informes de un usuario cuando este haga clic en un botón de un formulario:  
  
```vb  
' Button click event that retrieves a list of reports from  
' the My Reports folder and displays them in a combo box  
Private Sub listReportsButton_Click(sender As Object, e As System.EventArgs)  
   ' Create a new Web service object and set credentials  
   ' to Windows Authentication  
   Dim rs As New ReportingService2010()  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
  
   ' Return the list of items in My Reports  
   Dim items As CatalogItem() = rs.ListChildren("/Adventureworks 2008 Sample Reports", False)  
  
   Dim ci As CatalogItem  
   For Each ci In  items  
      ' If the item is a report, add it to   
      ' a combo box  
      If ci.TypeName = "Report" Then  
         catalogComboBox.Items.Add(ci.Name)  
      End If  
   Next ci  
End Sub 'listReportsButton_Click  
```  
  
```csharp  
// Button click event that retrieves a list of reports from  
// the My Reports folder and displays them in a combo box  
private void listReportsButton_Click(object sender, System.EventArgs e)  
{  
   // Create a new Web service object and set credentials  
   // to Windows Authentication  
   ReportingService2010 rs = new ReportingService2010();  
   rs.Credentials = System.Net.CredentialCache.DefaultCredentials;  
  
   // Return the list of items in My Reports  
   CatalogItem[] items = rs.ListChildren("/Adventureworks 2008 Sample Reports", false);  
  
   foreach (CatalogItem ci in items)  
   {  
      // If the item is a report, add it to   
      // a combo box  
      if (ci.TypeName == "Report")  
         catalogComboBox.Items.Add(ci.Name);  
   }  
}  
```  
  
 Desde allí, podría permitir que los usuarios seleccionen el informe en el cuadro combinado y obtener una vista previa del mismo en el formulario bien con un control de explorador web o con un control de imagen.  
  
## <a name="enabling-report-viewing-and-navigation-using-windows-forms"></a>Habilitar la visualización de informes y la navegación con formularios Windows Forms  
 Hay dos métodos disponibles para integrar los informes en aplicaciones de formularios Windows Forms.  
  
 Puede utilizar la API SOAP para representar los informes en cualquiera de los formatos de representación admitidos utilizando el método <xref:ReportExecution2005.ReportExecutionService.Render%2A>. Habilitar la visualización de los informes y la navegación a través de SOAP presenta ligeras desventajas:  
  
-   No puede aprovecharse de la funcionalidad integrada de la barra de herramientas de informe que se incluye con el Visor HTML a través del acceso URL.  
  
-   Si usa HTML para la representación, debe representar por separado las imágenes o recursos como flujos adicionales utilizando el método <xref:ReportExecution2005.ReportExecutionService.RenderStream%2A>.  
  
-   Representar los informes mediante acceso URL supone una ligera ventaja de rendimiento sobre el uso de la API SOAP.  
  
 Sin embargo, el método <xref:ReportExecution2005.ReportExecutionService.Render%2A> de la API SOAP se puede utilizar para representar los informes y guardarlos en varios formatos de salida mediante programación. Esto es una ventaja sobre el acceso URL, que requiere la interacción con el usuario. Al representar un informe usando el método <xref:ReportExecution2005.ReportExecutionService.Render%2A> de la API SOAP, puede usar cualquiera de los formatos de salida admitidos.  
  
 También puede usar los controles del Visor de informes que se distribuyen de forma gratuita y están incluidos en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsOrcas](../../includes/vsorcas-md.md)]. Los controles del Visor de informes permiten insertar fácilmente la característica de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en aplicaciones personalizadas. Los controles del Visor de informes se han concebido para los programadores que quieren proporcionar informes prediseñados, completamente creados, como parte del conjunto de características de una aplicación (por ejemplo, una aplicación de administración de un sitio web debería incluir informes que mostrasen análisis de flujos de clic de los sitios web de la compañía). Incrustar los controles en una aplicación es una alternativa simplificada frente a incluir los componentes de servidor de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] en la implementación de la aplicación. Los controles proporcionan funcionalidad de informes, pero sin las características de compatibilidad adicionales de creación, publicación, distribución y entrega de informes que se incluyen en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Hay dos versiones de los controles del Visor de informes, una para aplicaciones cliente de Windows completas y otra para las aplicaciones [!INCLUDE[vstecasp](../../includes/vstecasp-md.md)]. Los controles admiten tanto el modo de procesamiento local como el modo de procesamiento remoto. En el modo de procesamiento local, la aplicación proporciona la definición y los conjuntos de datos de los informes y desencadena el procesamiento de los informes. En el modo de procesamiento remoto, la recuperación de datos y el procesamiento de informes tienen lugar en el servidor de informes y el control se utiliza para la visualización y navegación en informes. Este modelo le permite crear aplicaciones completas que pueden distribuirse del escritorio a la empresa.  
  
 Los controles del Visor de informes se describen en la Ayuda en pantalla de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Para obtener más información, vea la documentación del producto de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)].  
  
## <a name="see-also"></a>Consulte también  
 [Creación de aplicaciones con el servicio web y .NET Framework](../../reporting-services/report-server-web-service/net-framework/building-applications-using-the-web-service-and-the-net-framework.md)   
 [Integración de Reporting Services en las aplicaciones](../../reporting-services/application-integration/integrating-reporting-services-into-applications.md)   
 [Usar la API de SOAP en una aplicación web](../../reporting-services/application-integration/integrating-reporting-services-using-soap-web-application.md)  
  
  
