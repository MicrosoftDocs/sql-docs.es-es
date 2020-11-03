---
title: Instalar el Generador de informes | Microsoft Docs
description: Generador de informes es una aplicación independiente que el usuario o un administrador pueden instalar en el equipo.
ms.date: 01/31/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 6b2291bb-1d20-4d08-81cb-a16dd8e01faf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 98134fb195b9184bb10905b4a4f8ddec48f3cb57
ms.sourcegitcommit: d35d0901296580bfceda6e0ab2e14cf2b7e99a0f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2020
ms.locfileid: "92496981"
---
# <a name="install-report-builder"></a>instalar el Generador de informes

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)]

[!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] es una aplicación independiente que el usuario o un administrador pueden instalar en el equipo. Puede instalarlo desde el Centro de descarga de Microsoft, desde un servidor de informes de [!INCLUDE[ssRSCurrent](../../includes/ssrscurrent-md.md)] o desde un sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  

> [!NOTE]
> ¿Busca información sobre la instalación de Power BI Report Builder en su lugar? Vaya a la página [Power BI Report Builder](https://www.microsoft.com/download/details.aspx?id=58158) del Centro de descarga. 

 Un administrador normalmente instala y configura [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], concede permiso para descargar [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde el portal web y administra las carpetas y los permisos para los informes, los elementos de informe y los conjuntos de datos compartidos guardados en el servidor de informes. Para más información sobre la administración de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], vea [Servidor de informes de Reporting Services &#40;modo nativo&#41;](../../reporting-services/report-server/reporting-services-report-server-native-mode.md).  
  
## <a name="install-ssrbnoversion-from--a--web-portal-or-sharepoint-library"></a>Instalar [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde un portal web o una biblioteca de SharePoint 

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.
  
 Puede iniciar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde un portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o desde un sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Para obtener información, vea [Iniciar el Generador de informes](../../reporting-services/report-builder/start-report-builder.md).  

::: moniker range="=sql-server-2016||=sqlallproducts-allversions"
  
### <a name="sharepoint-site-integrated-with-ssrsnoversion"></a>Sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]
  
 En un sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], si en el menú **Nuevo documento** no aparece **Informe del Generador de informes** , **Modelo del Generador de informes** y **Orígenes de datos de informe** , será necesario agregar sus tipos de contenido a la biblioteca de SharePoint. Para obtener más información, vea [Add Reporting Services Content Types to a SharePoint Library (Agregar los tipos de contenido de Reporting Services a una biblioteca de SharePoint)](../../reporting-services/report-server-sharepoint/add-reporting-services-content-types-to-a-sharepoint-library.md).  

::: moniker-end
 
## <a name="install-ssrbnoversion-with-microsoft-endpoint-configuration-manager"></a>Instalación de [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] con Microsoft Endpoint Configuration Manager 
  
 Un administrador también puede usar software como Microsoft Endpoint Configuration Manager para insertar el programa en su equipo. Para obtener información sobre cómo usar software específico para instalar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], consulte la documentación del software. Para más información, consulte la [documentación de Microsoft Endpoint Configuration Manager](/configmgr/).  
  
> [!IMPORTANT]  
>  Las características de seguridad de Windows Vista y Windows 7 exigen permisos elevados para ejecutar operaciones de línea de comandos y solicitarán permiso para ejecutar la línea de comandos. La instalación no es silenciosa. Para que la instalación sea silenciosa, es necesario ejecutar la línea de comandos como administrador.  
  
## <a name="system-requirements"></a>Requisitos del sistema
  
 Consulte la sección **System Requirements** (Requisitos del sistema) en la [página de descarga del Generador de informes](https://go.microsoft.com/fwlink/?LinkID=734968) en el Centro de descarga de Microsoft.
  
##  <a name="to-install-ssrbnoversion-from-the-download-site"></a><a name="download"></a> Para instalar [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde el sitio de descarga  
  
1.  En la [página del Generador de informes del Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkID=734968) , haga clic en **Download** (Descargar).  
  
2.  Cuando haya acabado la descarga de [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], haga clic en **Ejecutar**.  
  
     De este modo se inicia el Asistente para el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] de SQL Server.  
  
3.  Acepte los términos del contrato de licencia y haga clic en **Siguiente**.  
  
4.  En la página **Servidor de destino predeterminado** , puede especificar la dirección URL al servidor de informes de destino si difiere del valor predeterminado. Haga clic en **Next**.  
  
    > [!NOTE]  
    >  Si tiene previsto trabajar con [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] cuando esté conectado a un servidor de informes, conviene especificar en este momento la dirección URL al servidor. También puede hacerlo desde el cuadro de diálogo **Opciones** del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  
  
5.  Haga clic en **Instalar** para completar la instalación del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)].  
  
## <a name="to-install-ssrbnoversion-from-a-share"></a>Para instalar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde un recurso compartido  
  
1.  Póngase en contacto con el administrador para obtener la ubicación del archivo ReportBuilder.msi que se ejecuta para instalar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] en el equipo local.  
  
2.  Busque el archivo ReportBuilder.msi, el paquete de Windows Installer (MSI) para [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], y haga clic en él.  
  
     De este modo se inicia el Asistente para el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] de SQL Server.  
  
3.  Complete el resto de los pasos de [Instalar Generador de informes desde el sitio de descarga](#download).  
  
## <a name="to-install-ssrbnoversion-from-the-command-line"></a>Para instalar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] desde la línea de comandos 

 También puede realizar una instalación de línea de comandos del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] y proporcionar argumentos para personalizar la instalación. Además de los parámetros estándar intrínsecos de MSI, puede usar los parámetros personalizados que proporciona [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]: RBINSTALLDIR y RBSERVERURL. RBINSTALLDIR especifica la carpeta de instalación raíz para el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]. RBSERVERURL especifica el servidor de informes predeterminado que usa [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] para guardar los informes en el servidor.  
  
 Si quiere realizar una instalación completamente silenciosa, sin ninguna interacción con la interfaz de usuario, especifique la opción **/quiet** . Por diseño, la marca de la opción quiet suprime los errores de instalación. Por lo tanto, es recomendable que, cuando use la opción quiet, incluya la opción **/l** que especifica el registro.   
  
1.  En la [página del Generador de informes del Centro de descarga de Microsoft](https://go.microsoft.com/fwlink/?LinkID=734968), haga clic en **Download** (Descargar).  
  
2.  Después de que haya acabado la descarga del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], haga clic en **Guardar**.  
  
3.  En el menú **Inicio** , haga clic en **Ejecutar**.  
  
4.  En el cuadro **Abrir** , escriba **cmd.**  
  
5.  En la ventana del símbolo del sistema, vaya a la carpeta en la que haya guardado ReportBuilder.msi.  
  
6.  Escriba un comando con el formato siguiente:  
  
     `msiexec/i ReportBuilder.msi OPTION=OptionValue [OPTION=OptionValue]`  
  
     Las dos opciones específicas a la instalación de [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] son: RBINSTALLDIR y RBSERVERURL. No hace falta que incluya estos argumentos en la línea de comandos. El siguiente es el comando de línea base:  
  
     `msiexec /i ReportBuilder3_x86.msi /quiet`  
  
7.  Para ejecutar el comando, pulse ENTRAR.  
  
## <a name="set-ssrbnoversion-defaults"></a>Establecer los valores predeterminados del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)]  
  
-   Después de instalar el [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)], puede establecer algunas opciones predeterminadas. Haga clic en **Archivo** > **Opciones**.  
  
     Lo más útil es establecer el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el sitio de SharePoint predeterminados. Para obtener más información, consulte [Set default options for Report Builder](../../reporting-services/report-builder/set-default-options-for-report-builder.md).  
  
-   Haga clic en **Generador de informes** .  
  
     Si no ve el servidor de informes en la lista de servidores existentes, cierre el cuadro de diálogo **Abrir informe** y haga clic en **Conectar** en la parte inferior del [!INCLUDE[ssRBnoversion](../../includes/ssrbnoversion.md)] para conectarse al servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar el Generador de informes](../../reporting-services/report-builder/start-report-builder.md)   
 [Desinstalar el Generador de informes](../../reporting-services/install-windows/uninstall-report-builder.md)  
  
