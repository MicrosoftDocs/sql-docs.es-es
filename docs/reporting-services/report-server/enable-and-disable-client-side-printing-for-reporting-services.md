---
title: Habilitar y deshabilitar la impresión del lado cliente para Reporting Services | Microsoft Docs
description: Obtenga información sobre cómo habilitar o deshabilitar la impresión del lado cliente para informes de Reporting Services que se ven en un explorador. La impresión del lado cliente usa PDF y está habilitada de manera predeterminada.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- pdf
- viewer
- reportviewer
- toolbar
ms.assetid: 0e709c96-7517-4547-8ef6-5632f8118524
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 8cf4544579f4a3258d4bc785c98ada5935653dff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100075781"
---
# <a name="enable-and-disable-client-side-printing-for-reporting-services"></a>Habilitar y deshabilitar la impresión del lado cliente para Reporting Services

  El botón de impresión de la barra de herramientas del Visor de informes usa el formato Portable Document Format (PDF) en las impresiones del lado cliente de los informes de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] visualizados en un explorador. En la nueva experiencia de impresión remota se usa la extensión de representación de PDF incluida en [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]para representar el informe en formato PDF. Puede descargar un formulario en .PDF del informe o, si tiene una aplicación instalada para ver archivos .PDF, el botón de impresión abrirá un cuadro de diálogo de impresión con los elementos de configuración comunes de página, como el tamaño y la orientación de página y una vista previa del archivo .PDF. Aunque la impresión del lado cliente está habilitada de manera predeterminada, puede deshabilitar esta característica para impedir que sea utilizada.  
  
 En las versiones anteriores de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] se usaba un control ActiveX que requería la descarga en el equipo cliente desde el servidor de informes. Si actualiza el servidor de informes a SQL Server 2016 o una versión posterior, el control de impresión no se quita de los equipos cliente ni del servidor de informes.  

##  <a name="the-print-experience"></a><a name="bkmk_clientside_printexpereince"></a> La experiencia de impresión  
 Cuando se hace clic en el botón de impresión ![htmlviewer_print](../../reporting-services/report-server/media/htmlviewer-print.png "htmlviewer_print") de la barra de herramientas del Visor de informes, la experiencia variará en función de las aplicaciones de visualización de .PDF que haya instaladas en el equipo cliente y del explorador que se use.   Dependiendo del equipo cliente, puede descargar el archivo PDF o configurar las opciones de impresión en un cuadro de diálogo (o ambos).  
  
 ![Barra de herramientas de informe](../../reporting-services/media/ssrs-htmlviewer-toolbar.png "Barra de herramientas de informe")  
  
|Interacción|Interfaz de usuario|  
|-|-|  
|El primer cuadro de diálogo es el mismo para todos los exploradores y permite cambiar las propiedades de diseño básicas, como la orientación. Al hacer clic en **Imprimir**, la experiencia será ligeramente diferente dependiendo del explorador que se use.|![ssrs_pdfprint_chrome1](../../reporting-services/report-server/media/ssrs-pdfprint-chrome1.png "ssrs_pdfprint_chrome1")|  
|En Chrome, se abre un cuadro de diálogo de impresión de explorador detallado.   Puede cambiar la configuración de impresión, imprimir y abrir el cuadro de diálogo de impresión del sistema operativo.|![ssrs_pdfprint_chrome2](../../reporting-services/report-server/media/ssrs-pdfprint-chrome2.png "ssrs_pdfprint_chrome2") ![ssrs_pdfprint_chrome3.png](../../reporting-services/report-server/media/ssrs-pdfprint-chrome3-png.png "ssrs_pdfprint_chrome3.png")|  
|Si tiene instalada una aplicación de lector de PDF, el botón de impresión abrirá una ventana de vista previa del archivo PDF, que puede guardar o imprimir.||  
|Si no tiene instalada una aplicación de lector de PDF, hay dos experiencias de usuario posibles:<br /><br /> El informe se representa automáticamente y se usará el proceso de descarga del explorador para descargar el archivo PDF.   **Nota:** cuanto más complejo sea el informe, mayor será el tiempo que transcurra desde que haga clic en **Imprimir** hasta que vea la notificación de descarga del explorador. También puede forzar la descarga de nuevo haciendo clic en **Click here to view the PDF of your report**(Haga clic aquí para ver el PDF del informe).<br /><br /> Fuerce la descarga del PDF haciendo clic en **Click here to view the PDF of your report**(Haga clic aquí para ver el PDF del informe).|![ssrs_pdfprint_firefox2](../../reporting-services/report-server/media/ssrs-pdfprint-firefox2.png "ssrs_pdfprint_firefox2")|  
  
##  <a name="troubleshoot-client-side-printing"></a><a name="bkmk_troubleshoot_clientsideprinting"></a> Solucionar problemas de impresión del lado cliente  
 Si el botón de impresión de la barra de herramientas del Visor de informes está deshabilitado, compruebe lo siguiente:  
  
-   La impresión del lado cliente está deshabilitada en el servidor de informes en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Vea la sección  [Habilitar y deshabilitar la impresión del lado cliente](#bkmk_enable) de este tema.  
  
-   La extensión de representación de PDF [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] está deshabilitada. Consulte la sección `<Extension Name="PDF"` del archivo **rsreportserver.config** .  
  
-   Está viendo el informe en modo de comparación, en el que se usa el antiguo motor de representación HTML4 de [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] . La experiencia de impresión de PDF requiere el motor de representación HTML5.  Haga clic en el botón **Try Preview** (Probar vista previa) de la barra de herramientas.  
  
     ![ssrs_html5_switch2html5](../../reporting-services/report-server/media/ssrs-html5-switch2html5.png "ssrs_html5_switch2html5")  
  
##  <a name="enable-and-disable-client-side-printing"></a><a name="bkmk_enable"></a> Habilitar y deshabilitar la impresión del lado cliente  
 Los administradores de servidores de informes tienen la opción de deshabilitar la función de impresión remota; solo tienen que establecer la propiedad del sistema del servidor de informes **EnableClientPrinting** en **false**. De este modo se deshabilitará la impresión del lado cliente para todos los informes administrados por ese servidor. La propiedad **EnableClientPrinting** está establecida de manera predeterminada en **true**. Puede deshabilitar la impresión del lado cliente de las siguientes maneras:  
  
-   Para un **servidor de informes en modo nativo**:  
  
    1.  Inicie [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] con privilegios de administrador.  
  
    2.  Conéctese a una instancia del servidor de informes en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)].  
  
    3.  Haga clic con el botón derecho en el nodo del servidor de informes y, después, haga clic en **Propiedades**. Si la opción **Propiedades** está deshabilitada, compruebe que inició [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] con privilegios de administrador.  
  
    4.  Haga clic en **Avanzadas**.  
  
    5.  Seleccione **EnableClientPrinting**.  
  
    6.  Elija entre True o False y haga clic en **Aceptar**.  
  
         ![ssrs_ssmsproperties_clientprinting](../../reporting-services/report-server/media/ssrs-ssmsproperties-clientprinting.png "ssrs_ssmsproperties_clientprinting")  
  
-   Para un **servidor de informes en modo de SharePoint**:  
  
    1.  En Administración central de SharePoint, haga clic en **Administración de aplicaciones**.  
  
    2.  Haga clic en **Administrar aplicaciones de servicio**.  
  
    3.  Haga clic el nombre de la aplicación de servicio de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y, a continuación, haga clic en **Administrar** en la cinta de opciones de SharePoint.  
  
    4.  Haga clic en **Configuración del sistema**.  
  
    5.  Seleccione **Habilitar la impresión de cliente**. La opción **Habilitar la impresión de cliente** está cerca de la parte inferior de la página.  
  
    6.  Haga clic en **OK**.  
  
-   Escriba código o script para establecer la propiedad **EnableClientPrinting** del sistema del servidor de informes en **false.**  
  
 El siguiente script de ejemplo ilustra un enfoque válido para deshabilitar la impresión de lado cliente. Compile y ejecute el siguiente código de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] para establecer la propiedad **EnableClientPrinting** en **False**. Después de ejecutar el código, reinicie IIS.  
  
### <a name="sample-script"></a>Script de ejemplo  
  
```  
Imports System  
Imports System.Web.Services.Protocols  
Class Sample  
   Public Shared Sub Main()  
Dim rs As New ReportingService()  
      rs.Credentials = System.Net.CredentialCache.DefaultCredentials  
        Dim props(0) As [Property]  
        Dim setProp As New [Property]  
        setProp.Name = "EnableClientPrinting"  
        setProp.Value = "False"   
        props(0) = setProp  
        Try  
            rs.SetSystemProperties(props)  
        Catch ex As System.Web.Services.Protocols.SoapException  
            Console.Write(ex.Detail.InnerXml)  
        Catch e as Exception  
            Console.Write(e.Message)  
        End Try  
    End Sub 'Main  
End Class 'Sample  
```

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
