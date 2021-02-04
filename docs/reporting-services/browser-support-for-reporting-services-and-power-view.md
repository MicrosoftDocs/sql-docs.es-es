---
title: Compatibilidad del explorador de Reporting Services y Power View | Microsoft Docs
description: Obtenga información sobre las versiones del explorador que son compatibles para administrar y ver SQL Server Reporting Services, los controles ReportViewer y Power View.
ms.date: 01/28/2021
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
helpviewer_keywords:
- displaying reports
- scripts [Reporting Services], requirements
- viewing reports
- browsers [Reporting Services]
- Web browsers [Reporting Services], about browser support
- browsing reports [Reporting Services]
- components [Reporting Services], browsers
- Web browsers [Reporting Services]
ms.assetid: 48a75bbb-0029-4c43-891d-dc8f4fc0ebe1
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e246db4f2a7b2a94ce17f8a48acf05b16aebbdf4
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049026"
---
# <a name="browser-support-for-reporting-services-and-power-view"></a>Compatibilidad del explorador de Reporting Services y Power View

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-pbirsi](../includes/ssrs-appliesto-pbirs.md)]

Obtenga información sobre las versiones del explorador que son compatibles para administrar y ver SQL Server Reporting Services, los controles ReportViewer y Power View.

> [!NOTE]
> La integración de Reporting Services con SharePoint ya no está disponible a partir de SQL Server 2016.

## <a name="browser-requirements-for-the-web-portal"></a>Requisitos del explorador para el portal web

Esta es la lista actual de exploradores admitidos para el portal web.

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*
- Microsoft Edge (+)
- Microsoft Internet Explorer 10 u 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple iOS**  
*iPhone y iPad con iOS 9*

- Apple Safari (+)

**Google Android**  
*Teléfonos y tabletas con Android 4.4 (KitKat) o versiones posteriores*

- Google Chrome (+)

 **(+)** Última versión publicada

## <a name="browser-requirements-for-the-reportviewer-web-control-2015"></a>Requisitos del explorador para el control web ReportViewer (2015)

 Esta es la lista actual de exploradores compatibles con el control web ReportViewer (2015). El visor de informes permite ver informes desde el portal web de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] y desde bibliotecas de SharePoint.  

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 u 11
- Google Chrome (+)
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Última versión publicada

::: moniker range="=sql-server-2016"

 Si usa un producto de SharePoint integrado con [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)], vea  [Planear la compatibilidad con exploradores en SharePoint 2016](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

### <a name="authentication-requirements"></a>Requisitos de autenticación

 Los exploradores admiten esquemas de autenticación concretos que deben ser administrados por el servidor de informes para que la solicitud del cliente tenga éxito. En la tabla siguiente se identifican los tipos de autenticación predeterminados que admite cada explorador que se ejecuta en un sistema operativo Windows.

|**Tipo de explorador**|**Es compatible con**|**Configuración predeterminada del explorador**|**Configuración predeterminada del servidor**|
|----------------------|------------------|-------------------------|------------------------|
|**Microsoft Edge** (+)|Negotiate, NTLM, Basic|Negotiate|Sí. La configuración de autenticación predeterminada funciona con Edge.|
|**Microsoft Internet Explorer**|Negotiate, NTLM, Basic|Negotiate|Sí. La configuración de autenticación predeterminada funciona con Internet Explorer.|
|**Google Chrome**(+)|Negotiate, NTLM, Basic|Negotiate|Sí. La configuración de autenticación predeterminada funciona con Chrome.|
|**Mozilla Firefox**(+)|NTLM, Basic|NTLM|Sí. La configuración de autenticación predeterminada funciona con Firefox.|
|**Apple Safari**(+)|NTLM, Basic|Básico|Sí. La configuración de autenticación predeterminada funciona con Safari.|

 **(+)** Última versión publicada

### <a name="script-requirements-for-viewing-reports"></a>Requisitos de script para ver informes

 Para usar el Visor de informes, configure el explorador para que ejecute scripts.

 Si el scripting no está habilitado, verá un mensaje de error similar al siguiente al abrir un informe:

- **Su explorador no admite secuencias de comandos o está configurado para no admitir su ejecución. Haga clic aquí para ver este informe sin scripts**.

 Si decide ver el informe prescindiendo de la compatibilidad con script, se representará en formato HTML y no incorporará funcionalidad del Visor de informes, como la barra de herramientas Informes y el mapa del documento.

> [!NOTE]
> La barra de herramientas Informes forma parte del componente Visor HTML. De forma predeterminada la barra de herramientas aparece en la parte superior de todos los informes que se representan en una ventana de explorador. El visor de informes proporciona características entre las que se incluyen la posibilidad de buscar información en el informe, desplazarse a una página específica y ajustar el tamaño de página para la visualización. Para obtener más información acerca de la barra de herramientas Informes o el Visor HTML, vea [HTML Viewer and the Report Toolbar](../reporting-services/html-viewer-and-the-report-toolbar.md).

## <a name="browser-support-for-reportviewer-web-server-controls-in-visual-studio"></a>Compatibilidad del explorador con los controles de servidor web ReportViewer en Visual Studio

 El control de servidor web ReportViewer se emplea para incrustar funcionalidad de informes en una aplicación web ASP.NET. Los controles se incluyen con Visual Studio y admiten otros exploradores y versiones de explorador que de los demás componentes descritos en este tema. El tipo de explorador utilizado para ver la aplicación determina el tipo de funcionalidad de ReportViewer que puede ofrecer en la aplicación. Use la tabla que se facilita en este tema para determinar qué exploradores admitidos están sujetos a las restricciones de la funcionalidad de informes y las plataformas admitidas.  

 Use un explorador que tenga habilitada la compatibilidad con scripts. Si el explorador no puede ejecutar scripts, no podrá ver el informe.

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Edge (+)
- Microsoft Internet Explorer 10 u 11
- Google Chrome (+)
- Mozilla Firefox (+)

 **(+)** Última versión publicada

## <a name="power-view-browser-support"></a>Compatibilidad con exploradores para Power View

**Microsoft Windows**  
*Windows 7, 8.1, 10; Windows Server 2008 R2, 2012, 2012 R2*

- Microsoft Internet Explorer 10 u 11
- Mozilla Firefox (+)

**Apple OS X**  
*OS X 10.9-10.11*

- Apple Safari (+)

 **(+)** Última versión publicada

::: moniker range="=sql-server-2016"

 Para más información sobre la compatibilidad con exploradores de SharePoint 2016, vea [Planear la compatibilidad con exploradores en SharePoint 2013](https://technet.microsoft.com//library/cc263526\(v=office.16\).aspx).

::: moniker-end

## <a name="next-steps"></a>Pasos siguientes

[Búsqueda y visualización de informes en el portal web](report-builder/finding-and-viewing-reports-in-the-web-portal-report-builder-and-ssrs.md)  
[Herramientas de Reporting Services](../reporting-services/tools/reporting-services-tools.md)  
[Portal web (modo nativo de SSRS)](./web-portal-ssrs-native-mode.md)  
[Visor HTML y la barra de herramientas del informe](../reporting-services/html-viewer-and-the-report-toolbar.md)  
[Referencia de parámetros de acceso URL](../reporting-services/url-access-parameter-reference.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
