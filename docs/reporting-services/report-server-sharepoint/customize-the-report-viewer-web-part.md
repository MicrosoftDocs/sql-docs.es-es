---
title: Personalizar el elemento web Visor de informes | Microsoft Docs
description: Puede usar el elemento web Visor de informes para ver informes que se ejecutan en un servidor de SQL Server Reporting Services configurado para la integración con SharePoint.
ms.date: 11/26/2018
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server-sharepoint
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 2ff03267639a2ea62000c077330b2db547128be7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074816"
---
# <a name="customize-the-report-viewer-web-part"></a>Personalizar el elemento web Visor de informes

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2016-and-later](../../includes/ssrs-appliesto-2016-and-later.md)]  [!INCLUDE[ssrs-appliesto-pbirsi](../../includes/ssrs-appliesto-pbirs.md)] [!INCLUDE[ssrs-appliesto-sharepoint-2013-and-later](../../includes/ssrs-appliesto-sharepoint-2013-and-later.md)] [!INCLUDE[ssrs-appliesto-not-sharepoint-online](../../includes/ssrs-appliesto-not-sharepoint-online.md)]

[!INCLUDE [ssrs-previous-versions](../../includes/ssrs-previous-versions.md)]

Puede usar el elemento web Visor de informes para ver informes que se ejecutan en un servidor de informes configurado para la integración de SharePoint. Entre los informes que puede mostrar se incluyen archivos de definición de informe (.rdl) e informes del Generador de informes. Los informes se abren de forma automática en el elemento web Visor de informes en una página nueva. También puede agregar un elemento web Visor de informes a una página web o sitio existente si quiere que un informe concreto siempre sea visible en la página.

> [!NOTE]
> Aunque tienen nombres idénticos, el elemento web Visor de informes instalado con el complemento [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] difiere del incluido en el archivo RSWebParts.cab. Las instrucciones de este tema son específicas para el elemento web Visor de informes que se instala con el complemento de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].

 Puede personalizar el elemento web Visor de informes de los siguientes modos:  
  
-   Configure las propiedades para cambiar la apariencia del elemento web.  
  
-   Elija las características de informe interactivas disponibles en la barra de herramientas de informe.  
  
-   Especifique las áreas de visualización disponibles. El elemento web Visor de informes incluye un área de visualización de informes, un área de parámetros y un área de credenciales.  
  
 No puede ampliar el elemento web Visor de informes para admitir distintos tipos de archivos y no puede reemplazar la barra de herramientas de informe por una barra de herramientas personalizada o agregar funcionalidad nueva a la barra de herramientas existente. Si necesita personalizar las características estándar, debe crear un elemento web personalizado.  

## <a name="setting-web-part-properties"></a>Configurar las propiedades del elemento web

 El elemento web tiene propiedades personalizadas que se usan para configurar la funcionalidad específica. Además, el elemento web tiene propiedades comunes estándar para todos los elementos web.  
  
### <a name="change-default-properties"></a>Cambiar las propiedades predeterminadas

 El elemento web Visor de informes tiene propiedades predeterminadas que se adaptan perfectamente para abrir informes a petición desde una biblioteca o una carpeta. De forma predeterminada, todos los controles disponibles se muestran en la barra de herramientas. Se establecen el alto y el ancho para usar todo el espacio disponible en la página web. Si quiere modificar las propiedades predeterminadas, puede personalizar el elemento web mediante **Configuración del sitio**.  
  
1.  En el menú **Acciones del sitio** , haga clic en **Configuración del sitio**.  
  
2.  En Galerías, haga clic en **Elementos web**.  
  
3.  Haga clic en **ReportViewer.dwp**.  
  
4.  Abra el panel de herramientas y establezca las propiedades que desee usar.  
  
### <a name="customize-an-embedded-report-viewer-in-a-web-page"></a>Personalizar un Visor de informes incrustado en una página web

 Puede establecer las propiedades para adaptar el Visor de informes a una página web. El Visor de informes puede usar el mismo estilo y los mismos colores que la página que lo contiene. Puede ocultar de forma total o parcial la barra de herramientas, el mapa del documento y el área de parámetros para maximizar el área de visualización de informes en el espacio asignado. El informe siempre usa los estilos definidos para él cuando se creó. No se puede personalizar el aspecto del informe después de publicarlo en una biblioteca de SharePoint.  
  
 Si va a incrustar el elemento web Visor de informes en una página web, debe establecer la propiedad **Dirección URL de informe** para un informe específico. En caso contrario, el Visor de informes muestra instrucciones para crear un vínculo con un informe. No puede personalizar o quitar estas instrucciones.  
  
### <a name="custom-properties-of-the-report-viewer-web-part"></a>Propiedades personalizadas del elemento web Visor de informes

 Al establecer las propiedades personalizadas, debe tener en cuenta que algunas solo se usan si el elemento web Visor de informes está incrustado en una página. Algunos ejemplos son Título, Alto, Ancho, Tipo de cromo y Zona. Otras propiedades, como la configuración de la barra de herramientas y los parámetros, se usan independientemente de si el Visor de informes aparece dentro de una página o si un informe se abre en modo de página completa.  
  
 A continuación se enumeran las propiedades personalizadas del elemento web Visor de informes.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|Informe|Ruta de acceso completa a un informe que esté en el sitio de SharePoint actual o en un sitio dentro de la misma aplicación o conjunto de servidores web. Para obtener los mejores resultados al establecer propiedades adicionales, haga clic en Aplicar después de especificar la dirección URL del informe.|  
|Destino de hipervínculo|HTML estándar que especifica el marco de destino para mostrar el contenido vinculado en el documento actual. Para informes que incluyen hipervínculos a sitios web externos, puede especificar si un documento de destino reemplaza el informe existente en la ventana actual o si se abre en una nueva ventana del explorador. Los valores válidos son **_Top**, **_Blank** y **_Self**. **_Top** usa la ventana actual, **_Blank** carga el documento en una nueva ventana del explorador y **_Self** abre el documento en el marco actual. Aunque **_Parent** es un valor válido para el atributo de destino en HTML, no se recomienda usarlo para un elemento web del Visor de informes que se incrusta en una página.|  
|Generar automáticamente el título del elemento web|Un título generado que incluye el nombre del elemento web Visor de informes más el nombre del informe, separados por un guion. Si el informe no tiene ningún título, se usa el nombre del archivo de informe. El título es visible cuando se agrega un elemento web a una página. Si esta casilla está activada, el título se generará cada vez que se actualice la página.|  
|Generar automáticamente el vínculo de detalles del elemento web|Un hipervínculo generado que aparece encima del elemento web. Puede hacer clic en el vínculo para abrir el informe en una nueva página, en modo de página completa.|  
|Mostrar elemento del menú del generador de informes|Muestra u oculta la opción del menú **Acciones** para abrir el Generador de informes.|  
|Mostrar elemento del menú de suscripción|Muestra u oculta la opción del menú **Acciones** para crear una suscripción para el informe.|  
|Buscar elemento del menú de impresión|Muestra u oculta la opción del menú **Acciones** para imprimir el informe.|  
|Buscar elemento del menú de exportación|Muestra u oculta la opción del menú **Acciones** para exportar el informe.|  
|Mostrar botón Actualizar|Muestra u oculta el botón de actualización en la barra de herramientas.|  
|Mostrar controles de navegación de página|Muestra u oculta los botones de navegación del informe en la barra de herramientas. Esta opción cambia la visibilidad de todos los controles de navegación.|  
|Mostrar botón Atrás|Muestra u oculta el botón Atrás en la barra de herramientas.|  
|Mostrar controles de búsqueda|Muestra u oculta los controles de búsqueda en la barra de herramientas. Los controles de búsqueda permiten que los usuarios busquen el texto en el informe representado. Esta opción cambia la visibilidad de todos los controles de búsqueda.|  
|Mostrar control de zoom|Muestra u oculta el zoom en la barra de herramientas.|  
|Mostrar botón de fuente ATOM|Muestra u oculta el botón de fuente ATOM en la barra de herramientas.<br /><br /> ![htmlviewer_datafeed](../../reporting-services/media/htmlviewer-datafeed.gif "htmlviewer_datafeed")|  
|Ubicación de la barra de herramientas|Determina la ubicación de la barra de herramientas dentro del visor de informes. Los valores válidos son **Top** y **Bottom**.|  
|Área de mensajes|Los valores válidos son **Displayed**, **Collapsed** y **Hidden**. **Displayed** muestra el área de parámetros para los informes que incluyen valores con parámetros y que requieren una entrada de usuario antes de ejecutarse. Use **Hidden** si se han especificado todos los parámetros del informe y no desea que los usuarios puedan ver el área de parámetros.|  
|Ancho del área de parámetros|Puede elegir la medida y el valor. El valor predeterminado es 200 píxeles. El único requisito de esta propiedad es que sea mayor que cero.|  
|Mapa del documento|Control de navegación en informes definido en el informe y usado para proporcionar acceso mediante un clic a secciones específicas de un informe. Está disponible en informes HTML. El mapa del documento se muestra en un área contraíble situada junto al área de visualización de informes. Los valores válidos son **Displayed**, **Collapsed** y **Hidden**. Si se define un mapa del documento para un informe, el área se expande de manera predeterminada a menos que se marque como oculta o contraída en las propiedades del elemento web. Si el mapa del documento está contraído, puede hacer clic en la flecha para expandirlo.|  
|Ancho del área de mapa del documento|Puede elegir la medida y el valor. El valor predeterminado es 200 píxeles. El único requisito de esta propiedad es que sea mayor que cero.|  
|Cargar parámetros|Recupere las propiedades de parámetros para el informe. No todos los informes tienen parámetros. Si el informe no tiene parámetros, no se devolverá ningún valor. Si está estableciendo las propiedades de un informe que acaba de cargar, puede aparecer un error en el que se indica que la conexión con el origen de datos se ha eliminado. Si esto ocurre, restablezca la conexión y termine de establecer las propiedades de los parámetros después de especificar la conexión. Para más información sobre cómo establecer la conexión, vea [Crear y administrar orígenes de datos compartidos &#40;Reporting Services en el modo integrado de SharePoint&#41;](/previous-versions/sql/).<br /><br /> Para obtener los mejores resultados, haga clic en **Aplicar** antes de hacer clic en Cargar parámetros.<br /><br /> Después de cargar las propiedades de los parámetros, puede establecerlas igual que lo haría en las páginas de propiedades de los parámetros en el informe. Para más información sobre cómo establecer los parámetros, vea [Establecer parámetros en un informe publicado &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).|  

## <a name="customizing-the-toolbar"></a>Personalizar la barra de herramientas

 La barra de herramientas aparece debajo del título y se extiende a lo largo de la parte superior del informe. La barra de herramientas proporciona un menú **Acciones** , la navegación en páginas para informes paginados, la actualización y el zoom. Incluye un control de mapa del documento para informes que tienen un mapa del documento. El menú **Acciones** incluye comandos para exportar el informe, buscar texto o números dentro de un informe, imprimir el informe y, para los informes basados en modelos, abrir el informe en el Generador de informes.  
  
 No puede agregar comandos nuevos al menú  **Acciones** pero puede personalizarlo cambiando las opciones que están visibles para los usuarios. Para cambiar la visibilidad de los botones y los controles de la barra de herramientas, cambie las opciones en la sección **Visibilidad de los elementos de la barra de herramientas** del elemento web. Además, puede quitar el comando **Imprimir** o determinados formatos de exportación al hacer que estas características no estén disponibles en el servidor de informes. Hay controles de navegación en páginas disponibles para informes que tengan saltos de página; de lo contrario, el informe tiene una sola página de longitud variable. **Actualizar** vuelve a procesar el informe con los parámetros actuales del informe. Para mostrar todos los controles en una línea, establezca el ancho global del elemento web en 400 píxeles como mínimo.  

## <a name="customizing-the-viewing-area"></a>Personalizar el área de visualización

 El área de visualización se usa para mostrar los informes. El área de visualización del informe se comparte con las áreas de parámetros y credenciales, en caso de usarse. Si son necesarias las credenciales, el área de credenciales aparece junto a un área de visualización del informe vacía. El área de credenciales se cierra cuando el usuario proporciona las credenciales y ejecuta el informe. Para personalizar el texto que solicita a los usuarios que especifiquen las credenciales, modifique las propiedades de conexión del origen de datos. Para más información, vea [Crear y administrar orígenes de datos compartidos &#40;Reporting Services en el modo integrado de SharePoint&#41;](/previous-versions/sql/).  
  
 En el área de parámetros, se proporcionan campos para especificar valores antes de ejecutar el informe. Solo se usa cuando la definición de un informe incluye parámetros. Cuando aparecen el área de parámetros o la de credenciales, la vista del informe se ajusta para usar el ancho restante del elemento web. Puede establecer las propiedades del elemento web para personalizar el ancho de Parámetros. También puede definir las etiquetas que aparecen junto a cada parámetro de la página. Para más información sobre cómo modificar etiquetas de parámetros, vea [Establecer parámetros en un informe publicado &#40;Reporting Services en el modo integrado de SharePoint&#41;](../../reporting-services/report-design/set-parameters-on-a-published-report-sharepoint-integrated-mode.md).  
  
## <a name="see-also"></a>Consulte también

 [Elemento web Visor de informes en un sitio de SharePoint](./report-viewer-web-part-sharepoint-site.md)   
 [Agregar el elemento web Visor de informes a una página web](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).