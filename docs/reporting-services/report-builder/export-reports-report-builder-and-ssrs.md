---
title: Exportación de informes (Generador de informes) | Microsoft Docs
description: Puede exportar un informe de Reporting Services a otro formato de archivo, como PowerPoint o PDF, mediante Generador de informes, Diseñador de informes o el servidor de informes.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d82c6f45d6cd8e4e35735291433d3e054afe5481
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024117"
---
# <a name="export-reports-report-builder-and-ssrs"></a>Exportación de informes (Generador de informes y SSRS)

  Puede exportar un informe de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] a otro formato de archivo, como PowerPoint, Image, PDF, [!INCLUDE[ofprword](../../includes/ofprword-md.md)]o [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] , o bien exportarlo generando un documento de servicio de Atom, enumerando las fuentes de distribución de datos compatibles con Atom que están disponibles en el informe. Puede exportar el informe desde el Generador de informes, el Diseñador de informes ([!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]) o el servidor de informes.  
  
 Exporte un informe para hacer lo siguiente:  
  
-   **Trabajar con los datos del informe en otra aplicación.** Por ejemplo, puede exportar el informe a Excel y, a continuación, continuar trabajando con los datos en Excel.  
  
-   **Imprimir el informe en un formato diferente.** Por ejemplo, puede exportar el informe al formato de archivo PDF y, a continuación, imprimirlo.  
  
-   **Guardar una copia del informe como otro tipo de archivo.** Por ejemplo, puede exportar un informe a Word y guardarlo, creando una copia del informe.  
  
-   **Usar los datos del informe como fuentes de distribución de datos de las aplicaciones.** Por ejemplo, puede generar fuentes de distribución de datos compatibles con Atom que puedan usar Power Pivot o Power BI y luego trabajar con los datos en Power Pivot o Power BI. Para más información, vea [Generar fuentes de distribución de datos a partir de un informe](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
-   Representar el informe en el servidor de informes resulta útil si se configuran suscripciones o se entregan informes a través del correo electrónico o si se desea guardar un informe que está disponible en el servidor de informes. Para obtener más información, vea [Suscripciones y entrega &#40;Reporting Services&#41;](../../reporting-services/subscriptions/subscriptions-and-delivery-reporting-services.md).  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona muchas extensiones de presentación, admitiendo exportaciones de informes a formatos de archivo comunes. Las extensiones de presentación admiten formatos de archivo con saltos de página automáticos (por ejemplo, Word o Excel), saltos de página duros (por ejemplo, PDF o TIFF) o solo datos (por ejemplo, CSV o XML que admita Atom).  
  
 Exportar un informe a un formato diferente podría afectar a su paginación. Al obtener una vista previa de un informe, lo ve tal como es presentado por la extensión de presentación HTML, que sigue las reglas de salto de página automático. Al exportar un informe a un formato de archivo diferente, como Adobe Acrobat (PDF), la paginación está basada en el tamaño de página físico, que sigue las reglas de salto de página forzado. Las páginas también pueden separarse mediante saltos de página lógicos que se agregan al informe, pero la longitud real de una página varía en función del tipo de representador que se utiliza. Para cambiar la paginación de su informe, debe entender el comportamiento de la paginación de la extensión de presentación que elija. Podría necesitar ajustar el diseño de su informe para esta extensión de presentación. Para más información, vea [Representación y diseño de páginas](../../reporting-services/report-design/page-layout-and-rendering-report-builder-and-ssrs.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-export-a-report-from-report-builder"></a><a name="bkmk_export_from_rb"></a> Para exportar un informe del Generador de informes

1.  Ejecute el informe u obtenga una vista previa.  
  
2.  Haga clic en **Exportar** en la cinta.  
  
     ![Ayuda de Report Builder](../../reporting-services/report-builder/media/ssrb-export.png "Ayuda de Report Builder")  
  
3.  Seleccione el formato que desea usar.  
  
     Se abre el cuadro de diálogo **Guardar como** . De forma predeterminada, aparece el nombre de archivo del informe que exportó. Si lo desea, puede cambiar el nombre de archivo.  
  
##  <a name="to-export-a-report-from-the-ssrsnoversion-web-portal"></a><a name="bkmk_export_from_rm"></a> Para exportar un informe desde el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]  
  
1.  En la página [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] Inicio **del portal web de** , vaya al informe que quiere exportar.  
  
2.  Haga clic en el informe para representarlo y obtener una vista previa.  
  
3.  En la barra de herramientas del Visor de informes, haga clic en la flecha desplegable **Exportar** .  
  
     ![Exportación del portal web de Reporting Services](../../reporting-services/report-builder/media/ssrsportal-export.png "Exportación del portal web de Reporting Services")  
  
4.  Seleccione el formato que desea usar.  
  
5.  Haga clic en **Exportar**. Aparece un cuadro de diálogo en el que se pregunta si desea abrir o guardar el archivo.  
  
6.  Para ver el informe en el formato de exportación seleccionado, haga clic en **Abrir**.  
  
     \- O bien  
  
     Para guardar de manera inmediata el informe en el formato de exportación seleccionado, haga clic en **Guardar**.  
  
     El informe se muestra o se guarda mediante la aplicación asociada al formato elegido. Si hace clic en **Guardar**, se le solicitará una ubicación para guardar el informe.  
  
##  <a name="to-export-a-report-from-a-sharepoint-library"></a><a name="bkmk_export_from_sharepoint"></a> Para exportar un informe desde una biblioteca de SharePoint  
  
1.  Obtenga una vista previa del informe.  
  
2.  En la barra de herramientas, haga clic en **Acciones**, seleccione **Exportar** y, a continuación, seleccione el formato que desea utilizar.  
  
     Se abre el cuadro de diálogo **Descarga de archivos** .  
  
3.  Para ver el informe en el formato de exportación seleccionado, haga clic en **Abrir**.  
  
     \- O bien  
  
     Para guardar de manera inmediata el informe en el formato de exportación seleccionado, haga clic en **Guardar**.  
  
     El informe se muestra o se guarda mediante la aplicación asociada al formato elegido. Si hace clic en **Guardar**, se le solicitará una ubicación para guardar el informe.  
  
     Opcionalmente, cambie el nombre de archivo del informe exportado.  
  
     **Nota** : si el programa no puede abrir el informe en el formato que ha elegido porque no tiene un programa asociado a este tipo de archivo, se le pedirá que guarde el informe exportado o que busque un programa en línea para abrir el informe.  
  
##  <a name="rendering-extension-types"></a><a name="RendererTypes"></a> Tipos de extensión de presentación  
 Hay tres tipos de extensiones de presentación de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] :  
  
-   **Extensiones de presentador de datos:** las extensiones de presentación de datos quitan del informe toda la información de formato y muestran solo los datos. El archivo resultante se puede usar para importar los datos del informe sin formato en otro tipo de archivo, como Excel, en otra base de datos, en un mensaje de datos XML o en una aplicación personalizada. Los presentadores de datos no admiten los saltos de página.  
  
     Las extensiones de presentación de datos admitidas son las siguientes: CSV, XML y Atom.  
  
-   **Extensiones del representador de saltos de página automáticos:** las extensiones del representador de saltos de página automáticos mantienen el diseño y el formato del informe. El archivo resultante se optimiza para la visualización y la entrega basadas en pantalla, como en una página web o en los controles **ReportViewer** .  
  
     Las extensiones de presentación de saltos de página admitidas son las siguientes: [!INCLUDE[msCoName](../../includes/msconame-md.md)] Excel, [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word y archivos de web (MHTML).  
  
-   **Extensiones del representador de saltos de página manuales:** las extensiones del representador de saltos de página manuales mantienen el diseño y el formato del informe. El archivo resultante se optimiza para que su aspecto no varíe al imprimirlo o para ver el informe en pantalla con formato de libro.  
  
     Las extensiones de presentación de saltos de página forzados admitidas son las siguientes: TIFF y PDF.  
  
##  <a name="formats-you-can-export-while-viewing-reports"></a><a name="ExportFormats"></a> Formatos que puede exportar durante la visualización de informes  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona extensiones de presentación que presentan los informes en formatos diferentes. Debería optimizar el diseño de informe para el formato de archivo elegido.  En la tabla siguiente se enumeran los formatos que puede exportar desde la interfaz de usuario.  Existen formatos adicionales que puede utilizar con suscripciones de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o si va a exportar desde el acceso de la dirección URL.  Vea la sección [Otras maneras de exportar Informes](#OtherWaysExportingReports)de este tema.  
  
|Formato|Tipo de extensión de presentación|Descripción|  
|------------|------------------------------|-----------------|  
|Archivo PDF de Acrobat|Salto de página duro|La extensión de representación en PDF representa un informe en archivos que se pueden abrir en Adobe Acrobat y en visores de PDF de otros fabricantes que admiten PDF 1.3. Aunque PDF 1.3 es compatible con Adobe Acrobat 4.0 y versiones posteriores, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] admite Adobe Acrobat 6 o posterior. La extensión de representación no requiere el software Adobe para representar el informe. Sin embargo, se necesitan visores de PDF, como Adobe Acrobat, para ver o imprimir un informe en formato PDF.<br /><br /> Para más información, vea [Exportar a un archivo PDF](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md).|  
|Atom|data|La extensión de presentación Atom genera fuentes de distribución de datos compatibles con Atom desde los informes. Las fuentes de distribución de datos son legibles y se pueden intercambiar con aplicaciones como Power Pivot o Power BI que pueden usar fuentes de distribución de datos compatibles con Atom.<br /><br /> El resultado es un documento de servicio de Atom que enumera las fuentes de los datos disponibles de un informe. Se crea al menos una fuente de datos para cada región de datos de un informe. Según el tipo de región de datos y los datos que esta muestra, podrían generarse varias fuentes de distribución de datos.<br /><br /> Para más información, vea [Generar fuentes de distribución de datos a partir de informes](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md).|  
|CSV|data|La extensión de representación de valores separados por comas (CSV) representa los informes como una representación sin estructura jerárquica de los datos a partir de un informe estándar y sin formato para que resulten fáciles de leer e intercambiar con muchas aplicaciones.<br /><br /> Para más información, vea [Exportar a un archivo CSV](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md).|  
|EXCELOPENXML|Salto de página automático|Se muestra como "Excel" en los menús de exportación al revisar informes. La extensión de representación en Excel representa un informe como un documento de Excel (.xlsx) compatible con [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] 2013.  Para más información, vea [Exportar a Microsoft Excel](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md).|  
|PowerPoint|Salto de página duro|La extensión de representación de PowerPoint representa un informe como un documento de PowerPoint (.pptx) que es compatible con PowerPoint 2013.|  
|Archivo TIFF|Salto de página duro|La extensión de presentación en imágenes presenta un informe en un mapa de bits o metarchivo. De manera predeterminada, una extensión de representación en imágenes genera un archivo TIFF del informe, que se puede ver en varias páginas. Cuando el cliente recibe la imagen, se puede mostrar en un visor de imágenes y se puede imprimir.<br /><br /> La extensión de presentación de imágenes puede generar archivos en cualquiera de los formatos compatibles con [!INCLUDE[ndptecgdiplus](../../includes/ndptecgdiplus-md.md)]: BMP, EMF, EMFPlus, GIF, JPEG, PNG y TIFF.<br /><br /> Para más información, vea [Exportar a un archivo de imagen](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md).|  
|Archivo web|Salto de página automático|La extensión de representación en HTML representa un informe en formato HTML. La extensión de representación también puede generar páginas HTML completas o fragmentos de HTML para incrustarlos en otras páginas HTML. Todos los HTML se generan con la codificación UTF-8.<br /><br /> La extensión de presentación en HTML es la predeterminada para los informes que se visualizan en el Generador de informes y en un explorador, lo que incluye también cuando se ejecutan en el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .<br /><br /> Para más información, vea [Representar en HTML](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md).|  
|WORDOPENXML|Salto de página automático|Se muestra como "Word" en el menú de exportación al visualizar informes. La extensión de representación de Word representa un informe como un documento de Word (.docx) compatible con [!INCLUDE[ofprword](../../includes/ofprword-md.md)] 2013.  Para más información, vea [Exportar a Microsoft Word](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md).|  
|XML|data|La extensión de presentación en XML devuelve un informe en formato XML. El esquema XML del informe es específico de éste y solamente contiene datos. La extensión de representación en XML no representa la información de diseño ni mantiene la paginación. El XML que genera esta extensión se puede importar a una base de datos, se puede usar como mensaje de datos XML o se puede enviar a una aplicación personalizada.<br/><br/> Para más información, vea [Exportar a XML](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md).|  
  
##  <a name="generating-data-feeds-from-a-report"></a><a name="GeneratingDataFeedsFromReport"></a> Generar fuentes de distribución de datos desde un informe  
 Para generar las fuentes de distribución de datos desde un informe, ejecute el informe en el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] y, después, haga clic en el icono **Generar fuente de distribución de datos** en la barra de herramientas del portal web. Se le solicita que decida si guardar o abrir el archivo. Si elige **Abrir**, el documento de servicio de Atom se abre en la aplicación que está asociada a la extensión de archivo .atomsvc. Si elige **Guardar**, el documento se guarda como un archivo .atomsvc. De forma predeterminada, el nombre del archivo es el nombre del informe. Puede cambiar el nombre a uno que sea más significativo.  
  
 Guarde el documento de servicio de Atom en su equipo. Después puede cargarlo a un servidor de informes o a otro servidor para que esté disponible para que otros lo utilicen. Para más información, vea [Generar fuentes de distribución de datos a partir de informes](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md) y [Generar fuentes de distribución de datos a partir de un informe](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
##  <a name="troubleshooting-exported-reports"></a><a name="Troubleshooting"></a> Solucionar problemas de informes exportados  
 A veces, los informes parecen diferentes o no funcionan como desea tras exportarlos a un formato diferente. Esto se debe a que podrían aplicarse ciertas reglas y limitaciones al representador. Puede solucionar muchas limitaciones teniéndolas en cuenta al crear el informe. Podría necesitar usar un diseño ligeramente diferente en el informe, alinear cuidadosamente los elementos dentro del mismo, restringir los pies de página del informe a una sola línea de texto, etc.  
  
 Si un informe contiene texto Unicode y números arábigos, o contiene fechas arábigas, las fechas y los números no se representan correctamente cuando el informe se imprime o cuando se exporta cualquiera de los formatos siguientes.  
  
-   PDF  
  
-   Word  
  
-   Excel  
  
-   Imagen/TIFF  
  
 Si se exporta el informe a HTML, las fechas y los números se representan correctamente.  
  
 Los temas acerca de los representadores concretos describen cómo se representan los elementos de informe y las regiones de datos, así como las limitaciones y soluciones de cada procesador.  
  
-   [Exportar a un archivo CSV &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)  
  
-   [Exportar a Microsoft Excel &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-excel-report-builder-and-ssrs.md)  
  
-   [Exportar a Microsoft Word &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-microsoft-word-report-builder-and-ssrs.md)  
  
-   [Representar en HTML &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/rendering-to-html-report-builder-and-ssrs.md)  
  
-   [Exportar a un archivo PDF &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-a-pdf-file-report-builder-and-ssrs.md)  
  
-   [Exportar a un archivo de imagen &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-an-image-file-report-builder-and-ssrs.md)  
  
-   [Exportar a XML &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/exporting-to-xml-report-builder-and-ssrs.md)  
  
-   [Generar fuentes de distribución de datos a partir de informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/generating-data-feeds-from-reports-report-builder-and-ssrs.md)  
  
 [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona características adicionales para ayudarle a crear informes que funcionen bien en otros formatos. Los saltos de página en las regiones de datos Tablix (tabla, matriz y lista), los grupos y los rectángulos proporcionan un mayor control sobre la paginación del informe. Las páginas del informe, delimitadas mediante saltos de página, pueden tener nombres de página diferentes y restablecer la numeración de las páginas. Mediante el uso de expresiones, los nombres de página y los números de página se pueden actualizar dinámicamente cuando se ejecuta el informe. Para más información, vea [Paginación en Reporting Services](../../reporting-services/report-design/pagination-in-reporting-services-report-builder-and-ssrs.md).  
  
 Además, puede usar la función RenderFormat integrada para aplicar condicionalmente diseños de informe diferentes para representadores distintos. Para más información, vea [Referencias a campos globales y de usuario integrados](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md).

##  <a name="other-ways-of-exporting-reports"></a><a name="OtherWaysExportingReports"></a> Otras maneras de exportar Informes  
 La exportación de un informe es una tarea a petición que se realiza cuando el informe está abierto en el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] o el Generador de informes. Si desea automatizar una operación de exportación (por ejemplo, exportar periódicamente un informe a una carpeta compartida como un tipo de archivo específico), cree una suscripción que entregue el informe a una carpeta compartida. Para obtener más información, vea [File Share Delivery in Reporting Services](../../reporting-services/subscriptions/file-share-delivery-in-reporting-services.md).  
  
 De forma predeterminada, los informes cuya vista previa se obtiene en las herramientas de elaboración de informes o que se abren en una aplicación de explorador, como el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , siempre se presentan primero en HTML. No es posible especificar una extensión de representación diferente como opción predeterminada para la vista. Sin embargo, puede crear una suscripción que genere un informe en el formato de representación que desee para que se envíe posteriormente a una bandeja de entrada de correo electrónico o una carpeta compartida. Para obtener más información, vea [Crear y administrar suscripciones para servidores de informes en modo nativo](../../reporting-services/subscriptions/create-and-manage-subscriptions-for-native-mode-report-servers.md) y [Crear, modificar y eliminar una suscripción controlada por datos](../../reporting-services/subscriptions/create-modify-and-delete-data-driven-subscriptions.md).  
  
 También puede tener acceso a un informe a través de una dirección URL que especifique una extensión de presentación como un parámetro URL y presente directamente el informe en el formato especificado sin presentarlo primero en HTML. En el ejemplo siguiente se presenta un informe en formato Excel:  
  
```  
https://<Report Server Name>/reportserver?/Sales/YearlySalesSummary&rs:Format=Excel&rs:Command=Render  
```  
  
 Lo siguiente representa un informe de PowerPoint desde una instancia con nombre:  
  
```  
https://<Report Server Name/ReportServer_THESQLINSTANCE/Pages/ReportViewer.aspx?%2freportfolder%2freport+name+with+spaces&rs:Format=pptx  
```  
  
 Para obtener más información, consulte [Export a Report Using URL Access](../../reporting-services/export-a-report-using-url-access.md).  

## <a name="next-steps"></a>Pasos siguientes

[Controlar saltos de página, encabezados, columnas y filas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/controlling-page-breaks-headings-columns-and-rows-report-builder-and-ssrs.md)   
[Buscar, ver y administrar informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)   
[Imprimir informes &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/print-reports-report-builder-and-ssrs.md)   
[Guardar informes &#40;Generador de informes&#41;](../../reporting-services/report-builder/saving-reports-report-builder.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
