---
title: Cómo implementar una extensión de procesamiento de datos en el Diseñador de informes | Microsoft Docs
description: Obtenga información sobre cómo implementar una extensión de procesamiento de datos en el Diseñador de informes y aprenda qué entradas agregar a los archivos de configuración.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- data processing extensions [Reporting Services], deploying
- assemblies [Reporting Services], data processing extension deployments
ms.assetid: 3614e601-004e-4a16-8388-836ffd67e9dd
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: ee0f4c58af897bacf929f0447141256a30682990
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065405"
---
# <a name="deploying-a-data-processing-extension-to-report-designer"></a>Implementar una extensión de procesamiento de datos en el Diseñador de informes
  El Diseñador de informes utiliza las extensiones de procesamiento de datos para recuperar y procesar los datos mientras se diseñan los informes. Debería implementar el ensamblado de extensión de procesamiento de datos para el Diseñador de informes como un ensamblado privado. También tiene que realizar una entrada en el archivo de configuración del Diseñador de informes, RSReportDesigner.config.  
  
#### <a name="to-deploy-a-data-processing-extension-assembly"></a>Para implementar un ensamblado de extensión de procesamiento de datos  
  
1.  Copie el ensamblado desde la ubicación de almacenamiento provisional en el directorio del Diseñador de informes. La ubicación predeterminada del directorio del Diseñador de informes es C:\Archivos de programa\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies.  
  
2.  Una vez copiado el archivo de ensamblado, abra el archivo RSReportDesigner.config. El archivo RSReportDesigner.config también se encuentra en el directorio del Diseñador de informes. Tiene que realizar una entrada en el archivo de configuración para el archivo de ensamblado de extensión de procesamiento de datos. Puede abrir el archivo de configuración con [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] o con un editor de texto sencillo, como el Bloc de notas.  
  
3.  Busque el elemento **Data** en el archivo RSReportDesigner.config. En la ubicación siguiente se debería realizar una entrada para la extensión de procesamiento de datos creada recientemente:  
  
    ```  
    <Extensions>  
       <Data>  
          <Your extension configuration information goes here>  
       </Data>  
    </Extensions>  
    ```  
  
4.  Agregue una entrada para la extensión de procesamiento de datos que incluya un elemento **Extension** con valores para los atributos **Name**, **Type** y **Visible**. La entrada podría tener la apariencia siguiente:  
  
    ```  
    <Extension Name="ExtensionName" Type="CompanyName.ExtensionName.MyConnectionClass, AssemblyName" />  
    ```  
  
     El valor de **Name** es el nombre único de la extensión de procesamiento de datos. El valor de **Type** es una lista separada por comas que incluye una entrada para el espacio de nombres completo de la clase que implementa las interfaces <xref:Microsoft.ReportingServices.Interfaces.IExtension> y <xref:Microsoft.ReportingServices.DataProcessing.IDbConnection>, seguido del nombre del ensamblado (sin incluir la extensión de archivo .dll). De forma predeterminada, las extensiones de procesamiento de datos están visibles. Para ocultar una extensión de las interfaces de usuario, como por ejemplo el Diseñador de informes, agregue un atributo **Visible** al elemento **Extension** y establézcalo en **false**.  
  
5.  Finalmente, agregue un grupo de código para el ensamblado personalizado que conceda el permiso **FullTrust** para la extensión. Para ello, se agrega el grupo de código al archivo rspreviewpolicy.config, que se encuentra de forma predeterminada en la carpeta C:\Archivos de programa\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies. El grupo de código podría tener la apariencia siguiente:  
  
    ```  
    <CodeGroup class="UnionCodeGroup"  
       version="1"  
       PermissionSetName="FullTrust"  
       Name="MyExtensionCodeGroup"  
       Description="Code group for my data processing extension">  
          <IMembershipCondition class="UrlMembershipCondition"  
             version="1"  
             Url="C:\Program Files\Microsoft Visual Studio 9.0\Common7\IDE\PrivateAssemblies\MyExtensionAssembly.dll"  
           />  
    </CodeGroup>  
    ```  
  
 La pertenencia de dirección URL es solo una de las muchas condiciones de pertenencia que podría elegir para la extensión de procesamiento de datos. Para más información sobre la seguridad de acceso del código de [!INCLUDE[ssRSversion2005](../../../includes/ssrsversion2005-md.md)], vea [Desarrollo seguro &#40;Reporting Services&#41;](../../../reporting-services/extensions/secure-development/secure-development-reporting-services.md).  
  
## <a name="generic-query-designer"></a>Diseñador de consultas genérico  
 El Diseñador de consultas proporciona un diseñador de consultas genérico que puede utilizar con extensiones de procesamiento de datos personalizadas. Este diseñador está compuesto de dos paneles: uno de consulta y otro de resultados. Puede utilizar el diseñador genérico para escribir las consultas que la interfaz gráfica no admita. A diferencia del diseñador gráfico de consultas, el diseñador genérico no comprueba la sintaxis de la consulta ni procede a reestructurarla.  
  
#### <a name="to-enable-the-generic-query-designer-for-a-custom-extension"></a>Para habilitar el diseñador de consultas genérico para una extensión personalizada  
  
-   Agregue la entrada siguiente al archivo RSReportDesigner.config bajo el elemento **Designer**, reemplazando el atributo **Name** por el nombre que proporcionó en las entradas anteriores.  
  
    ```  
    <Extension Name="ExtensionName" Type="Microsoft.ReportingServices.QueryDesigners.GenericQueryDesigner,Microsoft.ReportingServices.QueryDesigners"/>  
    ```  
  
## <a name="verifying-the-deployment"></a>Comprobación de la implementación  
 Para poder comprobar la implementación, debe cerrar todas las instancias de [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] en el equipo local. Una vez que haya finalizado todas las sesiones actuales, puede comprobar si la extensión de procesamiento de datos se implementó correctamente para el Diseñador de informes creando un proyecto de informe nuevo en [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)]. La extensión se debe incluir en la lista de tipos de orígenes de datos disponibles al crear un conjunto de datos nuevo para el informe.  
  
## <a name="see-also"></a>Consulte también  
 [Implementación de una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/deploying-a-data-processing-extension.md)   
 [Extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implementar una extensión de procesamiento de datos](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Biblioteca de extensiones de Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
