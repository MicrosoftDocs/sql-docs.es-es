---
title: Generación de fuentes de distribución de datos a partir de informes (Generador de informes) | Microsoft Docs
description: La extensión de representación de Atom de Reporting Services genera un documento de servicio de las fuentes de distribución de datos en un informe paginado y las fuentes de las regiones de datos que contiene.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-builder
ms.topic: conceptual
ms.assetid: 4e00789f-6967-42e5-b2b4-03181fdb1e2c
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: e0b1b313671b93fd62fb61d6a244590cf6a62086
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100014845"
---
# <a name="generating-data-feeds-from-reports-report-builder-and-ssrs"></a>Generar fuentes de distribución de datos a partir de informes (Generador de informes y SSRS)

  La extensión de representación de Atom de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] genera un documento de servicio de Atom que enumera las fuentes de distribución de datos disponibles en un informe paginado y las fuentes de distribución de datos de las regiones de datos de un informe. Esta extensión se usa para generar las fuentes de distribución de datos compatibles con Atom que son legibles y se pueden intercambiar con las aplicaciones que pueden usar las fuentes de distribución de datos generadas en los informes. Por ejemplo, puede utilizar la extensión de representación Atom para las fuentes de distribución de datos generadas que después puede utilizar en Power Pivot o en Power BI.  
  
 El documento de servicio Atom enumera al menos una fuente de distribución de datos de cada región de datos de un informe. Según el tipo de región de datos y los datos que esta muestra, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] podría generar varias fuentes de distribución de datos a partir de una región de datos. Por ejemplo, una matriz o gráfico pueden proporcionar varias fuentes de distribución de datos. Cuando la extensión de representación Atom crea el documento de servicio Atom, se crea un identificador único para cada fuente de distribución de datos y el identificador se usa en la dirección URL para tener acceso al contenido de la fuente de distribución de datos.  
  
 La extensión de representación Atom genera los datos de una fuente de distribución de datos de forma similar al modo en que la extensión de representación de valores separados por comas (CSV) representa los datos en un archivo CSV. Al igual que un archivo CSV, la fuente de distribución de datos es una representación plana de los datos del informe. Por ejemplo, una tabla con un grupo de filas que sume las ventas dentro de un grupo repite la suma en cada fila de datos y no hay ninguna fila independiente que solo contenga la suma.  
  
 Puede generar documentos y fuentes de distribución de datos de servicio Atom con el portal web de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , el servidor de informes, o bien con un sitio de SharePoint integrado con [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)].  
  
 Atom se aplica a un par de estándares relacionados. El documento de servicio de Atom cumple la especificación de protocolo de publicación Atom de RFC 5023 y las fuentes de distribución de datos cumplen la especificación de protocolo del formato para redifusión web Atom de RFC 4287.  
  
 En las secciones siguientes se proporciona información adicional sobre cómo utilizar la extensión de representación de Atom:  
  
 [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="reports-as-data-feeds"></a><a name="ReportDataAsDataFeeds"></a> Informes como fuentes de distribución de datos  
 Puede exportar un informe de producción como una fuente de distribución de datos o puede crear un informe cuyo propósito principal sea proporcionar datos, en forma de fuentes de distribución de datos, a las aplicaciones. Utilizar los informes como una fuente de distribución de datos ofrece una manera adicional de proporcionar datos a las aplicaciones cuando estos no son fáciles de usar a través de los proveedores de datos del cliente, o cuando se prefiera ocultar la complejidad del origen de datos y facilitar la utilización de los datos. Otra ventaja de usar los datos de informe como una fuente de distribución de datos es que se pueden usar características de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] (como la seguridad, la programación y las instantáneas de informe) para administrar los informes que proporcionan fuentes de distribución de datos.  
  
 Para sacar el máximo partido de la extensión de representación de Atom, debe saber cómo se representa el informe en las fuentes de distribución de datos. Si usa los informes existentes, poder predecir qué fuentes de distribución de datos generarán los informes resulta útil; si va a crear el informe específicamente para usarlo como fuente de distribución de datos, ser capaz de incluir los datos y ajustar el diseño del informe para sacar el máximo provecho de las fuentes de distribución de datos es también de gran valor.  
  
 Para más información, vea [Generar fuentes de distribución de datos a partir de un informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-builder/generate-data-feeds-from-a-report-report-builder-and-ssrs.md).  
  
  
##  <a name="atom-service-document-atomsvc-file"></a><a name="AtomServiceDocument"></a> Documento de servicio de Atom (archivo .atomsvc)  
 En un documento de servicio de Atom se especifica una conexión a una o varias fuentes de distribución de datos. Como mínimo, la conexión es una dirección URL simple al servicio de datos que genera la fuente.  
  
 Al representar los datos del informe utilizando la extensión de representación de Atom, el documento de servicio de Atom enumera las fuentes de distribución de datos disponibles para un informe. El documento enumera al menos una fuente de distribución de datos para cada región de datos del informe. Las tablas y medidores generan solo una fuente de distribución de datos cada uno, pero las matrices, listas y gráficos podían generar varias en función de los datos que muestren.  
  
 El diagrama siguiente muestra un informe que utiliza dos tablas y un gráfico.  
  
 ![RS_Atom_TableAndChartDataFeeds](../../reporting-services/report-builder/media/rs-atom-tableandchartdatafeeds.gif "RS_Atom_TableAndChartDataFeeds")  
  
 El documento de servicio de Atom generado con este informe incluye tres fuentes de distribución de datos, una para cada tabla y otra para el gráfico.  
  
 Las regiones de datos de matriz podrían tener más de una fuente de distribución de datos, según la estructura de la matriz. El diagrama siguiente muestra un informe que utiliza una matriz que genera dos fuentes de distribución de datos.  
  
 ![RS_Atom_PeerDynamicColumns](../../reporting-services/report-builder/media/rs-atom-peerdynamiccolumns.gif "RS_Atom_PeerDynamicColumns")  
  
 En el documento de servicio de Atom generado con este informe se incluyen dos fuentes de distribución de datos, una para cada una de las columnas dinámicas del mismo nivel: Territory y Year. El diagrama siguiente muestra el contenido de cada fuente de distribución de datos.  
  
 ![RS_Atom_PeerDynamicDataFeeds](../../reporting-services/report-builder/media/rs-atom-peerdynamicdatafeeds.gif "RS_Atom_PeerDynamicDataFeeds")  
  
  
##  <a name="data-feeds"></a><a name="DataFeeds"></a> Fuentes de distribución de datos  
 La fuente de distribución de datos es un archivo XML que tiene un formato tabular coherente que no cambia con el tiempo y datos variables que pueden ser diferentes cada vez que se ejecuta el informe. Las fuentes de distribución de datos que genera [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] están en el mismo formato que las generadas por ADO.NET Data Services.  
  
 Una fuente de distribución de datos contiene dos secciones: encabezado y datos. La especificación de Atom define los elementos en cada sección. El encabezado incluye información como el esquema de codificación de caracteres para utilizar con las fuentes de distribución de datos.  
  
### <a name="header-section"></a>Sección de encabezado  
 El siguiente código XML muestra la sección de encabezado de una fuente de distribución de datos.  
  
 `<?xml version="1.0" encoding="utf-8" standalone="yes"?><feed xmlns:d="https://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="https://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">`  
  
 `<title type="text"></title>`  
  
 `<id>uuid:1795992c-a6f3-40ec-9243-fbfd0b1a5be3;id=166321</id>`  
  
 `<updated>2009-05-08T23:09:58Z</updated>`  
  
### <a name="data-section"></a>Sección de datos  
 La sección de datos de las fuentes de distribución de datos contiene un elemento \<**entry**> para cada fila del conjunto de filas planas generado por la extensión de representación de Atom.  
  
 El diagrama siguiente muestra un informe que utiliza grupos y totales.  
  
 ![RS_Atom_ProductSalesSummaryCircledValues](../../reporting-services/report-builder/media/rs-atom-productsalessummarycircledvalues.gif "RS_Atom_ProductSalesSummaryCircledValues")  
  
 En el código XML siguiente se muestra un elemento \<**entry**> de ese informe en una fuente de distribución de datos. Como puede ver, en el elemento \<**entry**> se incluyen los totales de ventas y pedidos del grupo, y los totales de ventas y pedidos de todos los grupos. En el elemento \<**entry**> se incluyen todos los valores del informe.  
  
 `<entry><id>uuid:1795992c-a6f3-40ec-9243-fbfd0b1a5be3;id=166322</id><title type="text"></title><updated>2009-05-08T23:09:58Z</updated><author /><content type="application/xml"><m:properties>`  
  
 `<d:ProductCategory_Value>Accessories</d:ProductCategory_Value>`  
  
 `<d:OrderYear_Value m:type="Edm.Int32">2001</d:OrderYear_Value>`  
  
 `<d:SumLineTotal_Value m:type="Edm.Decimal">20235.364608</d:SumLineTotal_Value>`  
  
 `<d:SumOrderQty_Value m:type="Edm.Int32">1003</d:SumOrderQty_Value>`  
  
 `<d:SumLineTotal_Total_2_1 m:type="Edm.Decimal">1272072.883926</d:SumLineTotal_Total_2_1>`  
  
 `<d:SumOrderQty_Total_2_1 m:type="Edm.Double">61932</d:SumOrderQty_Total_2_1>`  
  
 `<d:SumLineTotal_Total_2_2 m:type="Edm.Decimal">109846381.399888</d:SumLineTotal_Total_2_2>`  
  
 `<d:SumOrderQty_Total_2_2 m:type="Edm.Double">274914</d:SumOrderQty_Total_2_2></m:properties></content>`  
  
 `</entry>`  
  
### <a name="working-with-data-feeds"></a>Trabajar con fuentes de distribución de datos  
 Todas las fuentes de distribución de datos generadas incluyen los elementos de informe que están en el ámbito del elemento primario de la región de datos que las genera. . Imagine un informe con varias tablas y un gráfico. Los cuadros de texto del cuerpo del informe proporcionan texto descriptivo de cada región de datos. Cada entrada de cada fuentes de distribución de datos que el informe genera incluye el valor del cuadro de texto. Por ejemplo, si el texto es "El gráfico muestra el promedio de ventas mensuales por región de ventas", las tres fuentes de distribución de datos incluirían este texto en cada fila.  
  
 Si el diseño del informe incluye relaciones de datos jerárquicas, como regiones de datos anidadas, esas relaciones se incluyen en el conjunto de filas planas de datos del informe.  
  
 Las filas de datos de las regiones de datos anidadas suelen ser anchas, sobre todo si las tablas anidadas y las matrices incluyen grupos y totales. Podría encontrar de utilidad exportar el informe a una fuente de distribución de datos y ver esta para comprobar que los datos generados son lo que esperaba.  
  
 Cuando la extensión de representación Atom crea el documento de servicio Atom, se crea un identificador único para la fuente de distribución de datos y el identificador se usa en la dirección URL para ver el contenido de la misma. El documento de servicio de Atom de ejemplo, mostrado anteriormente, incluye la dirección URL `https://ServerName/ReportServer?%2fProduct+Sales+Summary&rs%3aCommand=Render&rs%3aFormat=ATOM&rc%3aDataFeed=xAx0x1`. La dirección URL identifica el informe (resumen de ventas de producto), el formato de representación de Atom (ATOM) y el nombre de la fuente de distribución de datos (xAx0x1).  
  
 Los nombres de los elementos de informe tienen como valor predeterminado los nombres de los elementos de lenguaje RDL (Report Definition Language) de los elementos de informe y, a menudo, no son intuitivos ni fáciles recordar. Por ejemplo, el nombre predeterminado de la primera matriz colocada en un informe es Tablix 1. Las fuentes de distribución de datos usan estos nombres.  
  
 Para que le resulte más fácil trabajar con la fuente de distribución de datos, puede usar la propiedad DataElementName de la región de datos para proporcionar nombres descriptivos. Si proporciona un valor para DataElementName, el elemento de la fuente de distribución de datos \<**d**> lo usará en lugar del nombre de región de datos predeterminado. Por ejemplo, si el nombre predeterminado de una región de datos es Tablix1 y DataElementName establece SalesByTerritoryYear, el elemento \<**d**> de la fuente de distribución de datos usa SalesByTerritoryYear. Si las regiones de datos tienen dos fuentes de distribución de datos como el informe de matriz descrito anteriormente, los nombres utilizados en las fuentes de distribución de datos son SalesByTerritoryYear _Territory y SalesByTerritoryYear _Year.  
  
 Si compara los datos mostrados en el informe y los de las fuentes de distribución de datos, podría observar algunas diferencias. Los informes suelen mostrar datos de fecha y hora, y datos con formato numérico, mientras que la fuente de distribución de datos contiene datos sin formato.  
  
 Una fuente de distribución de datos se guarda con la extensión de nombre de archivo .atom. Puede utilizar un editor XML o de texto, como el Bloc de notas, o el Editor XML para ver la estructura de archivos y el contenido.  
  
  
##  <a name="flattening-report-data"></a><a name="FlatteningReportData"></a> Quitar información de estructura jerárquica a los datos del informe  
 El representador de Atom proporciona los datos del informe en forma de conjuntos de filas planas en un formato XML. Las reglas para quitar información de estructura jerárquica de las tablas de datos son las mismas que las del procesador de CSV con pocas excepciones:  
  
-   La información de estructura jerárquica de los elementos del ámbito se quitan hasta el nivel de detalle. A diferencia del representador de CSV, los cuadros de texto en el nivel superior aparecen en cada entrada escrita en la fuente de distribución de datos.  
  
-   Los valores de parámetro de informe se representan en cada fila de la salida.  
  
 Para que los datos jerárquicos y los datos agrupados puedan representarse en el formato compatible con Atom, es necesario quitar la información de estructura jerárquica. La extensión de representación quita información de estructura jerárquica en el informe y lo convierte en una estructura de árbol que representa los grupos anidados dentro de la región de datos. Para quitar información de estructura jerárquica en el informe:  
  
-   Se quita información de estructura jerárquica de las jerarquías de fila antes que de las jerarquías de columna.  
  
-   Los miembros de la jerarquía de filas se representan en la fuente de distribución de datos antes que los miembros de la jerarquía de columnas.  
  
-   Las columnas se ordenan de la manera siguiente: los cuadros de texto del cuerpo se ordenan de izquierda a derecha y de arriba abajo seguidos por las regiones de datos, que se ordenan de izquierda a derecha y de arriba abajo.  
  
-   Dentro de una región de datos, las columnas se ordenan de la manera siguiente: los miembros de las esquinas, los miembros de la jerarquía de fila, los miembros de la jerarquía de columna y, a continuación, las celdas.  
  
-   Las regiones de datos del mismo nivel son regiones de datos o grupos dinámicos que comparten una región de datos común o un antecesor dinámico. Los datos del mismo nivel se identifican creando una bifurcación del árbol sin información de estructura jerárquica.  
  
 Para más información, vea [Tablas, matrices y listas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/tables-matrices-and-lists-report-builder-and-ssrs.md).  
  
  
##  <a name="atom-rendering-rules"></a><a name="AtomRendering"></a> Reglas de representación de Atom  
 La extensión de representación de Atom omite la información siguiente al representar una fuente de distribución de datos:  
  
-   Formato y diseño  
  
-   Encabezado de página  
  
-   Pie de página  
  
-   Elementos de informes personalizados  
  
-   Rectángulos  
  
-   Líneas  
  
-   Imágenes  
  
-   Subtotales automáticos  
  
 El resto de los elementos del informe se ordenan, de arriba a abajo y de izquierda a derecha. Cada elemento se representa en una columna. Si el informe contiene elementos de datos anidados, como listas o tablas, los elementos primarios se repiten en cada fila.  
  
 En la tabla siguiente se indica el aspecto de los elementos de informe cuando se representan:  
  
|Elemento|Comportamiento de la representación|  
|----------|------------------------|  
|Tabla|Realiza la representación mediante la expansión de la tabla y la creación de una fila y una columna para cada fila y columna del nivel máximo de detalle. Las filas y columnas de subtotal no tienen encabezados de columna ni de fila. No se admiten informes detallados.|  
|Matrix|Realiza la representación mediante la expansión de la matriz y la creación de una fila y una columna para cada fila y columna del nivel máximo de detalle. Las filas y columnas de subtotal no tienen encabezados de columna ni de fila.|  
|List|Representa un registro para cada instancia o fila de detalle de la lista.|  
|Subinforme|El elemento primario se repite en todas las instancias del contenido.|  
|Gráfico|Representa un registro con todas las etiquetas de gráfico de cada valor de gráfico. Las etiquetas de las series y las categorías de las jerarquías se quitan y se incluyen en la fila de un valor de gráfico.|  
|Barra de datos|Se representa como un gráfico. Normalmente, una barra de datos no incluye jerarquías ni etiquetas.|  
|Minigráfico|Se representa como un gráfico. Normalmente, un minigráfico no incluye jerarquías ni etiquetas.|  
|Indicador|Se representa como un único registro con los valores máximo y mínimo de la escala lineal, los valores inicial y final del intervalo, y el valor del puntero.|  
|Indicador|Se representa como un único registro con el nombre del estado activo, los estados disponibles y el valor de los datos.|  
|Map|Genera una fuente de distribución de datos para cada región de datos del mapa. Si varias capas de mapa utilizan la misma región de datos, las fuentes de distribución de datos los incluyen a todos. Las fuentes de distribución de datos incluyen un registro con las etiquetas y valores para cada miembro de mapa de la capa de mapa.|  
  
  
##  <a name="device-information-settings"></a><a name="DeviceInfo"></a> Configuración de la información del dispositivo  
 Puede cambiar parte de la configuración predeterminada de este representador, incluido el esquema de codificación que se desea utilizar. Para obtener más información, consulte [ATOM Device Information Settings](../../reporting-services/atom-device-information-settings.md).  

## <a name="next-steps"></a>Pasos siguientes

[Exportación a un archivo CSV](../../reporting-services/report-builder/exporting-to-a-csv-file-report-builder-and-ssrs.md)   
[Exportación de informes](../../reporting-services/report-builder/export-reports-report-builder-and-ssrs.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
