---
title: Implementar una extensión de representación | Microsoft Docs
description: Descubra cómo implementar una extensión de representación del informe. Vea qué entradas del archivo de configuración se van a agregar para que el servidor de informes y el Diseñador de informes encuentren la extensión.
ms.date: 03/20/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- deploying [Reporting Services], extensions
- rendering extensions [Reporting Services], deploying
ms.assetid: 9fb8c887-5cb2-476e-895a-7b0e2dd11398
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 56c22120fb08b538b845647f1a2300c22e17d017
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100042217"
---
# <a name="deploying-a-rendering-extension"></a>Implementar una extensión de representación
  Después de haber escrito y compilado la extensión de representación de informes de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] en una biblioteca de [!INCLUDE[dnprdnshort](../../../includes/dnprdnshort-md.md)], es necesario hacer que el servidor de informes y el Diseñador de informes la puedan reconocer. Para ello, copie la extensión en el directorio adecuado y agregue las entradas a los archivos de configuración de [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] adecuados.  
  
## <a name="configuration-file-rendering-extension-element"></a>Elemento Extension de representación de archivos de configuración  
 Una vez compilada una extensión de representación en una .DLL, agregue una entrada al archivo rsreportserver.config. De forma predeterminada, la ubicación es %Archivos de programa%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer. El elemento primario es \<Render>. Bajo el elemento Render hay un elemento Extension para cada extensión de representación. El elemento **Extension** contiene dos atributos, Name y Type.  
  
 En la tabla siguiente se describen los atributos para el elemento **Extension** correspondientes a las extensiones de representación:  
  
|Atributo|Descripción|  
|---------------|-----------------|  
|**Nombre**|Nombre único de la extensión. La longitud máxima para el atributo **Name** es de 255 caracteres. El nombre debe ser único entre todas las entradas dentro del elemento **Extensions** del archivo de configuración. Si hay un nombre duplicado, el servidor de informes devuelve un error.|  
|**Tipo**|Lista separada por comas que incluye el espacio de nombres completo junto con el nombre del ensamblado.|  
|**Visible**|El valor **false** indica que la extensión de representación no debería estar visible en las interfaces de usuario. Si el atributo no está incluido, el valor predeterminado es **true**.|  
|**LogAllExecutionRequests**|Un valor **false** indica que una entrada solo se registra para la primera ejecución de un informe en una sesión. Si el atributo no está incluido, el valor predeterminado es **true**.<br /><br /> Por ejemplo, este valor de configuración determina si se ha de registrar una entrada solo para la primera página representada en un informe (en el caso de **false**) o una entrada para cada página representada en el informe (en el caso de **true**).|  
  
 Para más información, vea [El archivo de configuración RSReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
## <a name="deploying-the-extension-to-the-report-server"></a>Implementar la extensión para el servidor de informes  
 El servidor de informes utiliza las extensiones de representación para exportar los informes a otros formatos. Debería implementar su ensamblado de extensión de representación para el servidor de informes como un ensamblado privado. También tiene que realizar una entrada en el archivo de configuración del servidor de informes, rsreportserver.config.  
  
### <a name="to-deploy-the-assembly"></a>Para implementar el ensamblado  
  
1.  Copie el ensamblado de la ubicación provisional al directorio bin del servidor de informes en el que desea utilizar la extensión de representación. La ubicación predeterminada del directorio bin del servidor de informes es %Archivos de programa%\Microsoft SQL Server\MSRS10_50.\<InstanceName>\Reporting Services\ReportServer\Bin.  
  
2.  Una vez copiado el archivo de ensamblado, abra el archivo rsreportserver.config. El archivo rsreportserver.config también se encuentra en el directorio bin del servidor de informes. Tiene que realizar una entrada en el archivo de configuración para el archivo de ensamblado de extensión. Puede abrir el archivo con [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] o con un simple editor de texto.  
  
     Para más información, vea [El archivo de configuración RSReportServer.config](../../../reporting-services/report-server/rsreportserver-config-configuration-file.md).  
  
3.  Busque el elemento **Render** en el archivo Rsreportserver.config. En la ubicación siguiente se debería realizar una extensión creada recientemente:  
  
    ```  
    <Extensions>  
       <Render>  
          <extension configuration>  
       </Render>  
    </Extensions>  
    ```  
  
4.  Agregue una entrada para la extensión de representación. La entrada debería incluir un elemento que tenga valores para **Name** y **Type**, y podría parecerse a la siguiente:  
  
    ```  
    <Extension Name="My Rendering Extension Name" Type="CompanyName.ExtensionName.MyRenderingProvider, AssemblyName" />  
    ```  
  
     El valor de **Name** es el nombre único de la extensión de representación. El valor de **Type** es una lista separada por comas que incluye una entrada para el espacio de nombres completo de la implementación de <xref:Microsoft.ReportingServices.OnDemandReportRendering.IRenderingExtension>, seguida del nombre del ensamblado (sin incluir la extensión de archivo .dll). De forma predeterminada, las extensiones de representación están visibles. Para ocultar una extensión de las interfaces de usuario, por ejemplo del Administrador de informes, agregue un atributo **Visible** al elemento **Extension** y establézcalo en **false**.  
  
## <a name="verifying-the-deployment"></a>Comprobación de la implementación  
 Puede abrir también el Administrador de informes y comprobar que la extensión está incluida en la lista de tipos de exportación disponibles para un informe.  
  
## <a name="see-also"></a>Consulte también  
 [Implementar una extensión de representación](../../../reporting-services/extensions/rendering-extension/implementing-a-rendering-extension.md)   
 [Información general de las extensiones de representación](../../../reporting-services/extensions/rendering-extension/rendering-extensions-overview.md)   
 [Implementar la interfaz IRenderingExtension](../../../reporting-services/extensions/rendering-extension/implementing-the-irenderingextension-interface.md)   
 [Consideraciones de seguridad para las extensiones](../../../reporting-services/extensions/security-considerations-for-extensions.md)  
  
  
