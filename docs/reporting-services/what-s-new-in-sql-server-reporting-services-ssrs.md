---
title: Novedades de Reporting Services | Microsoft Docs
description: Obtenga información sobre las novedades de las distintas versiones de SQL Server Reporting Services, incluidos los cambios en las áreas de características principales.
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 12/05/2019
ms.openlocfilehash: fd8f612727e860f9797a5af46eb77154a19ff5aa
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237163"
---
# <a name="whats-new-in-sql-server-reporting-services-ssrs"></a>Novedades de SQL Server Reporting Services (SSRS)

[!INCLUDE[ssrs-appliesto](../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../includes/ssrs-appliesto-2016-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../includes/ssrs-appliesto-not-pbirs.md)]

Obtenga información sobre las novedades de las distintas versiones de SQL Server [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Este artículo abarca las principales áreas de características y se actualiza a medida que se lanzan nuevos elementos.

Para obtener información sobre Power BI Report Server, vea [¿Qué es Power BI Report Server?](/power-bi/report-server/get-started).

::: moniker range=">=sql-server-ver15"

## <a name="sql-server-2019-reporting-services"></a>SQL Server 2019 Reporting Services

**Descargar** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

[SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) está disponible para descarga en el Centro de descarga de Microsoft.

### <a name="azure-sql-managed-instance-support"></a>Compatibilidad con la Instancia administrada de Azure SQL

Ahora puede hospedar un catálogo de base de datos usado para SQL Server Reporting Services (SSRS) en una Instancia administrada (MI) de Azure SQL hospedada en una máquina virtual o en su centro de datos. La compatibilidad se limita al uso de credenciales de base de datos para la conexión a MI de SQL.

### <a name="power-bi-premium-dataset-support"></a>Compatibilidad con el conjunto de datos de Power BI Premium

Puede conectarse a los conjuntos de datos de Power BI mediante el Generador de informes de Microsoft o SQL Server Data Tools (SSDT). Después, puede publicar esos informes en SSRS 2019 con la conectividad de SQL Server Analysis Services. Para permitir este escenario, los usuarios deben usar un nombre de usuario y una contraseña de Windows almacenados.

### <a name="alttext-alternative-text-support-for-report-elements"></a>Compatibilidad con AltText (texto alternativo) en los elementos del informe

Al crear informes, puede usar las informaciones sobre herramientas para especificar el texto para cada elemento del informe. La tecnología de lector de pantalla identifica correctamente esta información sobre herramientas.

### <a name="azure-active-directory-application-proxy-support"></a>Compatibilidad con Azure Active Directory Application Proxy

Con Azure Active Directory Application Proxy, ya no necesita administrar su propio proxy de aplicación web para permitir el acceso seguro a través de las aplicaciones web o móviles.

### <a name="custom-headers"></a>Encabezados personalizados

Establece los valores de encabezado de todas las direcciones URL que coinciden con el patrón regex especificado. Los usuarios pueden actualizar el valor de encabezado personalizado con XML válido para establecer los valores de encabezado de las direcciones URL de solicitud seleccionadas. Los administradores pueden agregar cualquier número de encabezados en el XML. Para detalles, consulte [Encabezados personalizados](tools/server-properties-advanced-page-reporting-services.md#customheaders) el artículo **Página Opciones avanzadas de las propiedades del servidor**.

### <a name="transparent-database-encryption"></a>Cifrado de base de datos transparente

SQL Server 2019 ahora admite el Cifrado de base de datos transparente para la base de datos del catálogo de SSRS de las ediciones Enterprise y Standard. 

### <a name="microsoft-report-builder-update"></a>Actualización de Generador de informes de Microsoft

La versión recién publicada del Generador de informes es totalmente compatible con las versiones 2016, 2017 y 2019 de Reporting Services. También es compatible con todas las versiones publicadas y admitidas de Power BI Report Server.

::: moniker-end

::: moniker range=">=sql-server-2017"

## <a name="sql-server-2017-reporting-services"></a>SQL Server 2017 Reporting Services

**Descargar** :::image type="icon" source="https://docs.microsoft.com/analysis-services/analysis-services/media/download.png":::

Para descargar SQL Server 2017 Reporting Services, vaya al **[Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=55252)** .

### <a name="comments-on-reports"></a>Comentarios en los informes

Ahora hay comentarios disponibles para los informes a fin de agregar perspectivas y colaborar con otros usuarios. También puede incluir archivos adjuntos con los comentarios.

![Comentarios dentro de un servidor de informes](media/what-s-new-in-sql-server-reporting-services-ssrs/report-server-comments.png)

Para más información, vea [Agregar comentarios a un informe en un servidor de informes](https://powerbi.microsoft.com/documentation/reportserver-add-comments/).

### <a name="dax-queries-in-reporting-tools"></a>Consultas DAX en herramientas de informes

En las versiones más recientes del Generador de informes y SQL Server Data Tools, se pueden crear consultas DAX nativas sobre los modelos de datos tabulares de SQL Server Analysis Services. Puede arrastrar y colocar campos en los diseñadores de consultas. Consulte el [blog de Reporting Services](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools).

### <a name="rest-api-support"></a>Compatibilidad con la API de REST

Para habilitar el desarrollo y personalización de aplicaciones modernas, SQL Server Reporting Services admite ahora una API de RESTful totalmente compatible con OpenAPI. La especificación y la documentación completas de la API se pueden encontrar en [SwaggerHub](https://app.swaggerhub.com/apis/microsoft-rs/SSRS/2.0).

### <a name="query-designer-support-for-dax-now-in-report-builder-and-sql-server-data-tools"></a>Compatibilidad con el diseñador de consultas para DAX ahora en el Generador de informes y SQL Server Data Tools

En el Generador de informes y SQL Server Data Tools, ahora se pueden crear consultas DAX nativas sobre los modelos de datos tabulares de SQL Server Analysis Services admitidos. Puede usar el diseñador de consultas en ambas herramientas para arrastrar y colocar los campos que desee. La consulta DAX se genera entonces automáticamente.

Lea más en el [blog de Reporting Services](/archive/blogs/sqlrsteamblog/query-designer-support-for-dax-now-available-in-report-builder-and-sql-server-data-tools).

* Descargue [Generador de informes de SQL Server](https://go.microsoft.com/fwlink/?LinkId=734968).
* Descargue [SQL Server Data Tools (versión candidata para lanzamiento)](../ssdt/download-sql-server-data-tools-ssdt.md).

> [!NOTE]
> Solo se puede usar el diseñador de consultas de DAX con orígenes de datos tabulares de SSAS integrados en SQL Server 2016+.
::: moniker-end

## <a name="ssrs-2016"></a>SSRS 2016

### <a name="reporting-services-ssrswebportal-non-markdown"></a>Reporting Services [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)]  

Un nuevo [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] está disponible. El portal web actualizado incluye
- KPI
- Mobile Reports (Informes móviles)
- Paginated Reports (Informes paginados)
- archivos de Excel
- Archivos de Power BI Desktop

El [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)] reemplaza al Administrador de informes de versiones anteriores.

Para crear informes móviles, necesita [!INCLUDE[SS_MobileReptPub_Short](../includes/ss-mobilereptpub-short.md)].

Para más información sobre el [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)], consulte [Portal web (modo nativo de SSRS)](../reporting-services/web-portal-ssrs-native-mode.md).  

![Captura de pantalla en la que se muestra el portal de SQL Server Reporting Services.](../reporting-services/media/ssrsportal.png "ssRSPortal")  

#### <a name="custom-branding-for-the-ssrswebportal-non-markdown"></a>Personalización de marca para el [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

Puede personalizar el [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] con el logotipo y los colores de su organización con un paquete de personalización de marca.  

Para más información sobre la personalización de marca, consulte [Personalización de marca del portal web](./branding-the-web-portal.md).

#### <a name="key-performance-indicators-kpi-in-the-ssrswebportal-non-markdown"></a>Indicadores de rendimiento clave (KPI) en el [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] 

Cree KPI directamente en el [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)] que sean contextuales a la carpeta actual. Al crear los KPI, puede elegir campos de conjunto de datos y resumir sus valores. También puede seleccionar contenido relacionado para profundizar y exponer más detalles.

![Captura de pantalla en la que se muestran los KPI en el portal de SQL Server Reporting Services.](../reporting-services/media/ssrs-webportal-kpi.png)

Para obtener más información, vea [Uso de los KPI en Reporting Services](./working-with-kpis-in-reporting-services.md)

### <a name="mobile-reports"></a>Mobile Reports (Informes móviles)

Los informes móviles de Reporting Services son informes dedicados optimizados para una amplia variedad de factores de forma y para ofrecer una experiencia óptima para usuarios que accedan a informes en dispositivos móviles. Los informes móviles cuentan con una gran variedad de visualizaciones, desde gráficos de tiempo, categoría y comparación, hasta gráficos de rectángulos y mapas personalizados. Puede conectar sus informes móviles a una gran variedad de orígenes de datos, como datos tabulares y multidimensionales de SQL Server Analysis Services local. Puede colocar campos para informes móviles en una superficie de diseño con el ajuste de las columnas y filas de la cuadrícula. Los elementos flexibles de los informes móviles se escalan automáticamente para ajustarse a cualquier tamaño de pantalla. Guarde estos informes móviles en un servidor de Reporting Services, y puede visualizar e interactuar con estos en un explorador o en la aplicación móvil de Power BI. Entre los dispositivos compatibles se encuentran los siguientes:

- iPad
- iPhone
- Teléfonos Android
- o cualquier dispositivo Windows 10

#### <a name="mobile-report-publisher"></a>Publicador de informes móviles  

El [!INCLUDE[SS_MobileReptPub_Long](../includes/ss-mobilereptpub-long.md)] le permite crear y publicar informes móviles de SQL Server en [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] [!INCLUDE[ssRSWebPortal-Non-Markdown](../includes/ssrswebportal-non-markdown-md.md)].  

![SS_MRP_LayoutTabSmall](../reporting-services/media/ss-mrp-layouttabsm.png "SS_MRP_LayoutTabSmall")  

Para más información, vea [Creación y publicación de informes móviles con el Publicador de informes móviles de SQL Server](../reporting-services/mobile-reports/create-mobile-reports-with-sql-server-mobile-report-publisher.md).  

#### <a name="sql-server-mobile-reports-hosted-in-reporting-services-available-in-power-bi-mobile-app"></a>Informes móviles de SQL Server hospedados en Reporting Services disponibles en la aplicación móvil de Power BI  

La aplicación móvil de Power BI para iOS en iPad y iPhone puede mostrar ahora informes móviles de SQL Server hospedados en el servidor de informes local.  

![SS_MRP_iPad_HomeSm](../reporting-services/media/ss-mrp-ipad-homesm.png "SS_MRP_iPad_HomeSm")  

No puede conectarse de forma predeterminada sin realizar algunos cambios en la configuración. Para obtener más información sobre cómo permitir que la aplicación móvil de Power BI se conecte a su servidor de informes, consulte [Habilitar un servidor de informes para el acceso mediante móvil a Power BI](../reporting-services/report-server/enable-a-report-server-for-power-bi-mobile-access.md).

### <a name="support-of-sharepoint-mode-and-sharepoint-2016"></a>Compatibilidad del modo de SharePoint y SharePoint 2016  

[!INCLUDE[sssql15-md](../includes/sssql16-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] admite la integración con SharePoint 2013 y SharePoint 2016.

Para más información, consulte:  

- [Combinaciones admitidas del servidor y el complemento de SharePoint y Reporting Services &#40;SQL Server 2016&#41;](../reporting-services/install-windows/supported-combinations-of-sharepoint-and-reporting-services-server.md)  

- [Dónde encontrar el complemento Reporting Services para Productos de SharePoint](../reporting-services/install-windows/where-to-find-the-reporting-services-add-in-for-sharepoint-products.md)  

- [Instalar el modo de SharePoint de Reporting Services](../reporting-services/install-windows/install-reporting-services-sharepoint-mode.md)  

### <a name="microsoft-net-framework-4-support"></a>Compatibilidad con Microsoft .NET Framework 4  

[!INCLUDE[ssRSCurrent-md](../includes/ssrscurrent-md.md)] admite las versiones actuales de Microsoft .NET Framework 4, incluidas las versiones 4.0 y 4.5.1. Si no hay instalada ninguna versión de .NET Framework 4.x, la configuración de [!INCLUDE[ssNoVersion-md](../includes/ssnoversion-md.md)] instalará .NET 4.0 durante el paso de instalación de las características.  

### <a name="report-improvements"></a>Mejoras del informe

**Motor de representación HTML 5:** un nuevo motor de representación HTML5 destinado al modo moderno de estándares web "completos" y a los exploradores modernos.  El nuevo motor de representación ya no cuenta con el modo de interpretación usado por algunos exploradores antiguos.

Para más información sobre la compatibilidad de exploradores, vea [Compatibilidad del explorador de Reporting Services y Power View](../reporting-services/browser-support-for-reporting-services-and-power-view.md).  

**Informes paginados modernos:** diseñe informes paginados modernos y muy atractivos con nuevos estilos modernos para gráficos, medidores, mapas y otras visualizaciones de datos.

**Gráficos de rectángulos y de proyección solar:** mejore sus informes con gráficos de rectángulos ![ssrs_treemap_icon](../reporting-services/media/ssrs-treemap-icon.png "ssrs_treemap_icon") y proyección solar ![ssrs_sunburst_icon](../reporting-services/media/ssrs-sunburst-icon.png "ssrs_sunburst_icon"), formas estupendas para mostrar datos jerárquicos. Para obtener más información, consulte [Tree Map and Sunburst Charts in Reporting Services](../reporting-services/report-design/tree-map-and-sunburst-charts-in-reporting-services.md).  

**Inserción de informes:** ahora puede insertar informes paginados o móviles en otras páginas web y aplicaciones; para ello, puede usar un iframe junto con parámetros de dirección URL.  

**Anclar elementos de informe a un panel de Power BI:** mientras se visualiza un informe en el [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)], puede seleccionar elementos de informe y anclarlos a un panel de [!INCLUDE[sspowerbi](../includes/sspowerbi-md.md)] .   Puede anclar elementos como gráficos, paneles de medidores, mapas e imágenes. Puede:

1. Seleccione el grupo que contiene el panel al que desea anclar.
2. Seleccione el panel al que quiere anclar el elemento.
3. Seleccione con qué frecuencia desea que se actualice el icono en el panel.

![nota](/analysis-services/analysis-services/instances/install-windows/media/ssrs-fyi-note.png "nota") Las suscripciones de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] administran la actualización y, después de anclar el elemento, se pueden modificar para configurar una programación de actualización distinta.

![Captura de pantalla en la que se muestra el cuadro de diálogo Anclar a panel de Power BI.](../reporting-services/media/ssrs-pin-to-powerbi.png) 

Para más información, vea [Integración de Power BI Report Server &#40;Configuration Manager&#41;](../reporting-services/install-windows/power-bi-report-server-integration-configuration-manager.md) y [Anclado de elementos de Reporting Services en paneles de Power BI](../reporting-services/pin-reporting-services-items-to-power-bi-dashboards.md).  

**Representación y exportación de PowerPoint:** el formato de Microsoft PowerPoint (PPTX) es una extensión de representación de [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)] nueva. Puede exportar informes en formato PPTX desde las aplicaciones habituales, como el Generador de informes, el Diseñador de informes (en SSDT) y el [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. A modo de ejemplo, en la imagen siguiente se muestra el menú Exportar en [!INCLUDE[ssRSWebPortal](../includes/ssrswebportal.md)]. 

![Captura de pantalla en la que se muestra la lista desplegable Exportar con la opción de PowerPoint destacada.](../reporting-services/media/ssrs-export-powerpoint.png) 

También puede seleccionar el formato PPTX para la salida de la suscripción y usar el acceso de dirección URL del servidor de informes para representar y exportar un informe. Por ejemplo, el siguiente comando de dirección URL de su explorador exporta un informe desde una instancia con nombre del servidor de informes.  

```https
https://servername/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  

Para obtener más información, consulte [Export a Report Using URL Access](../reporting-services/export-a-report-using-url-access.md).

**PDF reemplaza a ActiveX para la impresión remota:** La barra de herramientas del visor de informes se imprime ahora a través de PDF en lugar de los controles ActiveX. El nuevo visor de informes es compatible con los exploradores más modernos, incluido Microsoft Edge. No necesita descargar más controles de ActiveX. Según el explorador que use y las aplicaciones y servicios de visualización de PDF que haya instalado, [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] abre un diálogo de impresión para imprimir el informe o le pedirá que descargue un archivo .PDF. Como administrador, puede deshabilitar la impresión del lado cliente desde [!INCLUDE[ssManStudio](../includes/ssmanstudio-md.md)].

Para más información, vea [Habilitar y deshabilitar la impresión del lado cliente para Reporting Services](../reporting-services/report-server/enable-and-disable-client-side-printing-for-reporting-services.md).

![Captura de pantalla del cuadro de diálogo Imprimir.](../reporting-services/media/ssrs-pdf-printing.png)

### <a name="subscription-improvements"></a>Mejoras en la suscripción  

|Característica|Modo de servidor admitido|  
|-------------|---------------------------|  
|**Habilitar y deshabilitar suscripciones**. Hay opciones nuevas de interfaz de usuario para habilitar y deshabilitar rápidamente las suscripciones. Las suscripciones deshabilitadas mantienen sus otras propiedades de configuración, como la programación, y pueden habilitarse fácilmente.<br /><br /> ![Captura de pantalla en la que se muestran las opciones Habilitar, Deshabilitar y Eliminar.](../reporting-services/media/ssrs-enable-disable-subscriptions.png)<br /><br /> Para obtener más información, consulte [Disable or Pause Report and Subscription Processing](../reporting-services/subscriptions/disable-or-pause-report-and-subscription-processing.md).|en modo nativo|  
|**Descripción de la suscripción**. Cuando crea una nueva suscripción, ahora puede incluir una descripción del informe como parte de las propiedades de la suscripción. La descripción se incluye en la página de resumen de la suscripción.|Modo nativo y SharePoint|  
|**Cambiar el propietario de la suscripción**. Se ha mejorado la interfaz de usuario para poder cambiar rápidamente el propietario de una suscripción. Las versiones anteriores de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] permiten a los administradores cambiar los propietarios de la suscripción con un script. A partir de la versión de [!INCLUDE[sssql15-md](../includes/sssql16-md.md)] , puede cambiar los propietarios de la suscripción con la interfaz de usuario o con un script. El cambio del propietario de una suscripción es una tarea administrativa común cuando los usuarios dejan la organización o cuando cambian sus roles.|Modo nativo y SharePoint|  
|**Credenciales compartidas para las suscripciones de recurso compartido de archivos**. Ahora existen dos flujos de trabajo con las suscripciones de recurso compartido de archivos de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] :<br /><br /> Como novedad de esta versión, el administrador de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] puede configurar una única cuenta de recurso compartido de archivos, que se puede usar para varias suscripciones. La cuenta del recurso compartido de archivos está configurada en el administrador de configuración de modo nativo de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]**Especificar una cuenta de recurso compartido de archivos**. En la página de configuración de la suscripción, los usuarios seleccionan **Usar la cuenta de recurso compartido de archivos**.<br /><br /> Configure las suscripciones individuales con credenciales específicas para el recurso compartido de archivos de destino.<br /><br /> También puede combinar los dos enfoques y definir que algunas suscripciones de recurso compartido de archivos usen la cuenta central de recurso compartido de archivos mientras otras suscripciones usen credenciales específicas.|en modo nativo|

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)

La nueva versión de SSDT incluye plantillas de proyecto para [!INCLUDE[ssRSCurrent](../includes/ssrscurrent-md.md)]: Asistente de proyectos de servidor de informes y Proyecto de servidor de informes. Para obtener más información acerca de la descarga de SSDT, consulte [SQL Server Data Tools para Visual Studio 2015](../ssdt/download-sql-server-data-tools-ssdt.md).  

### <a name="report-builder-improvements"></a>Mejoras del generador de informes

**Nueva interfaz de usuario del Generador de informes:** la interfaz de usuario principal de [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] ahora tiene un aspecto moderno, con elementos de interfaz de usuario mejorados.  

|Nuevo|Previous|  
|-|-|  
|![ssrs_rbfacelift_new](../reporting-services/media/ssrs-rbfacelift-new.png "ssrs_rbfacelift_new")|![ssrs_rbfacelift_old](../reporting-services/media/ssrs-rbfacelift-old.png "ssrs_rbfacelift_old")|  

**Panel de parámetros personalizado:** ahora puede personalizar el panel de parámetros. Al usar la superficie de diseño en el Generador de informes, puede arrastrar un parámetro a una columna y una fila específica en el panel de parámetros. Puede agregar y quitar columnas para cambiar el diseño del panel. Para más información, vea [Customize the Parameters Pane in a Report &#40;Report Builder&#41; (Personalizar el panel de parámetros en un informe [Generador de informes])](../reporting-services/report-design/customize-the-parameters-pane-in-a-report-report-builder.md).  

![Lista de parámetros en el panel Datos de informe y en el panel Parámetros](../reporting-services/media/ssrs-customizeparameter-parameterlist-reportdatapane.png "Lista de parámetros en el panel Datos de informe y en el panel Parámetros")  

**Compatibilidad con configuración elevada de ppp:** [!INCLUDE[ssRBnoversion](../includes/ssrbnoversion.md)] es compatible con el escalado y los dispositivos con valores altos de PPP (puntos por pulgada).  Para obtener más información sobre los valores altos de PPP, consulte los artículos siguientes:  

- [Windows 8.1 DPI Scaling Enhancements (Mejoras en el ajuste de escala de PPP en Windows 8.1)](https://blogs.windows.com/windowsexperience/2013/07/15/windows-8-1-dpi-scaling-enhancements/)  

- [Valores altos de PPP y Windows 8.1](/previous-versions/windows/it-pro/windows-8.1-and-8/dn528848(v=win.10))  

## <a name="next-steps"></a>Pasos siguientes

[Novedades de Analysis Services](/analysis-services/what-s-new-in-sql-server-analysis-services?viewFallbackFrom=sql-server-ver15)  
[Compatibilidad con versiones anteriores](reporting-services-backward-compatibility.md)  
[Características de Reporting Services compatibles con las ediciones de SQL Server](../reporting-services/reporting-services-features-supported-by-the-editions-of-sql-server-2016.md)  
[Actualizar y migrar Reporting Services](../reporting-services/install-windows/upgrade-and-migrate-reporting-services.md)  
[Reporting Services](../reporting-services/create-deploy-and-manage-mobile-and-paginated-reports.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).