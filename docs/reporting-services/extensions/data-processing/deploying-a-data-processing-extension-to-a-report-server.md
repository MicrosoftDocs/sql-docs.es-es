---
title: Cómo implementar una extensión de procesamiento de datos en un servidor de informes | Microsoft Docs
description: Obtenga información sobre cómo implementar una extensión de procesamiento de datos en un servidor de informes y aprenda qué entradas agregar a los archivos de configuración.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: e00dface-70f8-434b-9763-8ebee18737d2
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1aadbda9a8842777b395612c59d753d3afb875ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060960"
---
# <a name="deploying-a-data-processing-extension-to-a-report-server"></a>Implementar una extensión de procesamiento de datos en un servidor de informes
  Los servidores de informes utilizan las extensiones de procesamiento de datos para recuperar y procesar los datos en informes representados. Debería implementar el ensamblado de extensión de procesamiento de datos en un servidor de informes como un ensamblado privado. También tiene que realizar una entrada en el archivo de configuración del servidor de informes, RSReportServer.config.  
  
## <a name="procedures"></a>Procedimientos  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>Para implementar un ensamblado de extensión de procesamiento de datos  
  
1.  Copie el ensamblado de la ubicación provisional al directorio bin del servidor de informes en el que desea utilizar la extensión de procesamiento de datos. La ubicación predeterminada del directorio bin del servidor de informes es %Archivos de programa%\Microsoft SQL Server\MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer\bin.  
  
    > [!NOTE]  
    >  Este paso evitará una actualización a una instancia más nueva de SQL Server. Para obtener más información, vea [Upgrade and Migrate Reporting Services](../../../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md).  
  
2.  Una vez copiado el archivo de ensamblado, abra el archivo RSReportServer.config. El archivo RSReportServer.config se encuentra en el directorio ReportServer. Tiene que realizar una entrada en el archivo de configuración para el archivo de ensamblado de extensión de procesamiento de datos. Puede abrir el archivo de configuración con Visual Studio o con un procesador de texto sencillo como el Bloc de notas.  
  
3.  Busque el elemento **Data** en el archivo RSReportServer.config. En la ubicación siguiente se debería realizar una entrada para la extensión de procesamiento de datos creada recientemente:  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  Agregue una entrada para su extensión de procesamiento de datos. La entrada debería incluir un elemento **Extension** con valores para **Name** y **Type**, y podría parecerse a la siguiente:  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, MyExtensionAssembly" />  
    ```  
  
     El valor de **Name** es el nombre único de la extensión de procesamiento de datos. El valor de **Type** es una lista separada por comas que incluye una entrada para el espacio de nombres completo de la clase que implementa las interfaces <xref:Microsoft.ReportingServices.Interfaces.IExtension> y <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>, seguido del nombre del ensamblado (sin incluir la extensión de archivo .dll). De forma predeterminada, las extensiones de procesamiento de datos están visibles. Para ocultar una extensión de las interfaces de usuario, por ejemplo del Administrador de informes, agregue un atributo **Visible** al elemento **Extension** y establézcalo en **false**.  
  
5.  Agregue un grupo de código para el ensamblado personalizado que conceda el permiso **FullTrust** a la extensión. Para ello, agregue el grupo de código al archivo rssrvpolicy.config que se encuentra, de forma predeterminada, en %Archivos de programa%\Microsoft SQL Server\\<MSRS10_50.\<*Instance Name*>\Reporting Services\ReportServer. El grupo de código podría tener la apariencia siguiente:  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft SQL Server\MSRS10_50.<Instance Name>\Reporting Services\ReportServer\bin\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 La pertenencia de dirección URL es solo una de las muchas condiciones de pertenencia que podría elegir para la extensión de procesamiento de datos. Para más información sobre la seguridad de acceso del código en [!INCLUDE[ssCurrentUI](../../../includes/sscurrentui-md.md)] [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vea [Desarrollo seguro &#40;Reporting Services&#41](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md);.  
  
## <a name="verifying-the-deployment"></a>Comprobación de la implementación  
 Puede comprobar si la extensión de procesamiento de datos se implementó correctamente en el servidor de informes utilizando el método <xref:ReportService2010.ReportingService2010.ListExtensions%2A> del servicio web. Puede abrir también Administrador de informes y comprobar que su extensión está incluida en la lista de orígenes de datos disponibles. Para más información sobre el Administrador de informes y los orígenes de datos, vea [Crear, modificar y eliminar orígenes de datos compartidos &#40;SSRS&#41;](../../../reporting-services/report-data/create-modify-and-delete-shared-data-sources-ssrs.md).  
  
## <a name="see-also"></a>Consulte también  
 [Implementación de una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementar una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
