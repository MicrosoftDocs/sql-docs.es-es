---
title: Publicar un informe en una biblioteca de SharePoint | Microsoft Docs
description: Obtenga información sobre cómo publicar un informe en una biblioteca de SharePoint mediante el establecimiento de las propiedades del proyecto en Diseñador de informes.
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reports
ms.topic: conceptual
helpviewer_keywords:
- deploying [Reporting Services], reports in SharePoint integrated mode
- SharePoint integration [Reporting Services], publishing to a library
- publishing reports [Reporting Services], to a SharePoint library
ms.assetid: 3f6dfc28-50d8-4231-bd25-871b5f77cce6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 093837d7d7d2c6833b4c7dce8fabbf554d634290
ms.sourcegitcommit: ff82f3260ff79ed860a7a58f54ff7f0594851e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2020
ms.locfileid: "79510116"
---
# <a name="publish-a-report-to-a-sharepoint-library"></a>Publicar un informe en una biblioteca de SharePoint
  Para publicar un informe en un sitio de SharePoint configurado para la integración de SharePoint, debe establecer las propiedades del proyecto en el Diseñador de informes. En las propiedades de proyecto, todas las referencias a servidores, informes y orígenes de datos compartidos deben ser direcciones URL completas. En la definición de informe, todas las referencias a subinformes, informes detallados y recursos, como imágenes basadas en web, deben ser direcciones URL completas.  
  
 Debe disponer del permiso **Miembro** o **Propietario** en el sitio de SharePoint para establecer las propiedades en el proyecto. Para obtener más información, vea [Ejemplos de dirección URL para los elementos de informe publicados en un servidor de informes en modo de SharePoint &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md).  
  
### <a name="to-publish-a-report-to-a-sharepoint-site"></a>Para publicar un informe en un sitio de SharePoint  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra un nuevo proyecto de servidor de informes o uno existente.  
  
2.  En el menú **Proyecto** , haga clic en **Propiedades**. Se abre el cuadro de diálogo **Páginas de propiedades** de _\<proyecto>_.  
  
3.  En la lista **Configuración** , seleccione el nombre de una configuración de generación de soluciones para usarla en la generación y publicación del informe. La configuración actual aparece como **Active**(*\<configuración>*).  
  
4.  Si desea publicar los orígenes de datos compartidos del proyecto y sobrescribir los publicados anteriormente, establezca **OverwriteDataSources** en **True**.  
  
5.  (Opcional) Para **TargetDataSourceFolder**, escriba una dirección URL a una biblioteca de SharePoint o una carpeta de biblioteca (por ejemplo, `https://TestServer/TestSite/Documents/DataSources`).  
  
     Si no se especifica ningún valor, se usa el valor **TargetReportFolder** .  
  
6.  Para **TargetReportFolder**, escriba una dirección URL a una biblioteca o una carpeta de biblioteca (por ejemplo, `https://TestServer/TestSite/Documents/Reports`).  
  
7.  Para **TargetServerURL**, escriba una dirección URL a un sitio de nivel superior o a un subsitio de SharePoint. Si no especifica ningún sitio, se usa el sitio de primer nivel predeterminado (por ejemplo, `https://servername`, `https://servername/site`o `https://servername/site/subsite`).  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
9. En el Explorador de soluciones, haga clic con el botón derecho en el informe que quiera publicar y haga clic en **Implementar**. El informe se publicará en la ubicación especificada en **TargetReportFolder**. Los errores de implementación se mostrarán en la ventana de resultados.  
  
## <a name="see-also"></a>Consulte también  
 [Páginas de propiedades del proyecto (cuadro de diálogo)](../../reporting-services/tools/project-property-pages-dialog-box.md)   
 [Establecer propiedades de implementación &#40;Reporting Services&#41;](../../reporting-services/tools/set-deployment-properties-reporting-services.md)   
 [Publicación de informes en un servidor de informes](../../reporting-services/reports/publishing-reports-to-a-report-server.md)   
 [Ejemplos de dirección URL para los elementos de informe publicados en un servidor de informes en modo de SharePoint &#40;SSRS&#41;](../../reporting-services/tools/url-examples-for-items-on-a-report-server-sharepoint-mode.md)   
 [Usar una conexión de datos de Office &#40;.odc&#41; con informes &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-data/use-an-office-data-connection-odc-with-reports.md)  
  
  
