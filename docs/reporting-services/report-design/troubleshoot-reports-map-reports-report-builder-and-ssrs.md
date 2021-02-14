---
title: 'Solucionar problemas de informes: Informes de mapa (Generador de informes) | Microsoft Docs'
description: Explore ideas de solución de problemas para situaciones que pueden producirse al personalizar, agregar una capa o publicar un informe con un mapa en el Generador de informes.
ms.date: 07/22/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: a690aec2-056b-40bc-8cab-c694bd2d6d62
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a4580559356869ffb52d75241219e42c8511ba3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081166"
---
# <a name="troubleshoot-reports-map-reports-report-builder-and-ssrs"></a>Solucionar problemas de informes: informes de mapa (Generador de informes y SSRS)
  Podrían producirse problemas con los mapas de un informe paginado de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] al agregar un mapa o una capa de mapa a un informe, personalizar un mapa o una capa de mapa existentes en un informe, obtener una vista previa de un mapa en un informe o publicar un informe con un mapa. Utilice este tema como ayuda para solucionar estos problemas.  
    
   ## <a name="need-more-help"></a>¿Necesita ayuda?  
   
  Pruebe lo siguiente:  
 * [SQL Server Reporting Services](https://stackoverflow.com/questions/tagged/reporting-services) en Stack Overflow  
 * Registre un problema o sugerencia en [UserVoice de Microsoft SQL Server](https://feedback.azure.com/forums/908035-sql-server).  

  
##  <a name="report-definition-size-issues"></a><a name="Embedded"></a> Problemas con el tamaño de la definición de informe  
 Utilice esta sección para ayudar a resolver problemas relacionados con el tamaño de la definición de informe.  
  
## <a name="how-do-i-reduce-the-report-definition-size"></a>¿Cómo reduzco el tamaño de la definición de informe?  
 Una capa de mapa contiene elementos de mapa que se crean con datos espaciales. En algunos casos, los elementos de mapa se incrustan en la definición de informe. Esto sucede cuando ocurre lo siguiente:  
  
-   Si el origen de datos espaciales es de un mapa de la Galería de mapas o de un archivo de forma ESRI del equipo local, los elementos de mapa se incrustan automáticamente en la definición de informe.  
  
     Si publica un informe en el servidor de informes y hay una referencia del origen de datos espacial a un archivo local, los datos espaciales no se pueden recuperar en el momento de procesar el informe. Para evitar este problema, los datos del mapa se incrustan en la definición de informe.  
  
-   En el Asistente para mapas o el Asistente para capas de mapa, si selecciona la opción para incrustar los datos espaciales, los elementos de mapa basados en los datos espaciales se incrustan en la capa de mapa en la definición de informe.  
  
-   En el panel Mapa, si hace clic con el botón derecho en la capa y, después, hace clic en una de las opciones de **Incrustar datos espaciales** , los elementos de mapa basados en los datos espaciales se incrustan en la capa de mapa en la definición de informe.  
  
-   Para quitar los datos insertados basados en un archivo de forma ESRI de una definición de informe, debe hacer lo siguiente:  
  
1.  Cargue o publique los archivos ESRI .shp y .dbf en el servidor de informes.  
  
2.  En el informe, en el panel MApa de la vista Diseño, seleccione la capa que contenga los datos insertados y abra las propiedades de **Datos de la capa** . En **Usar datos espaciales de**, seleccione **Vincular a archivo de forma ESRI** y, a continuación, vaya a la carpeta del servidor de informes que contenga los archivos de forma ESRI, selecciónela y haga clic en Aceptar.  
  
3.  Guarde el informe. Los datos insertados de la capa que cambió se han quitado de la definición de informe.  
  
 Los elementos de mapa de un informe de la Galería de mapas siempre se incrustarán en una capa de mapa.  
  
##  <a name="spatial-data-issues"></a><a name="Spatial"></a> Problemas con los datos espaciales  
 Utilice esta sección como ayuda para resolver problemas relacionados con los datos espaciales.  
  
## <a name="on-the-design-surface-i-see-sample-spatial-data"></a>En la superficie de diseño, veo el ejemplo de datos espaciales  
 En tiempo de diseño, la superficie de diseño podría mostrar el mensaje sobre los datos espaciales de ejemplo debido a las razones siguientes:  
  
-   Los datos espaciales proceden de un archivo ESRI .shp, pero el archivo .dbf correspondiente no está disponible. Los archivos de forma ESRI normalmente incluyen tanto un archivo .shp con datos espaciales como un archivo .dbf auxiliar. Compruebe que el archivo .dbf está en el mismo directorio que el archivo .shp.  
  
-   Los datos espaciales proceden de un conjunto de datos y la conexión de datos para la consulta es no válida o las credenciales actuales son no válidas.  
  
-   La capa de mapa contiene una propiedad con una expresión. Las expresiones no se evalúan hasta que el informe se ejecuta. Para ver el mapa, debe obtener una vista previa del informe.  
  
-   Los datos espaciales proceden de un conjunto de datos que tiene un ámbito concreto. Por ejemplo, cuando un mapa está anidado en una región de datos Tablix o el mapa utiliza el mismo ámbito del conjunto de datos para los datos analíticos y los datos espaciales, el ámbito de los datos no se calcula hasta que el informe se ejecuta.  
  
## <a name="when-i-set-an-offset-for-an-individual-map-element-a-cluster-of-map-elements-move"></a>Cuando establezco un desplazamiento para un elemento de mapa individual, se mueve un grupo de elementos de mapa  
 Los datos espaciales definen los elementos de mapa que se muestran en cada capa de mapa. Un elemento de un mapa puede estar basado en datos espaciales que sean un único punto, un conjunto de puntos, una única línea, un conjunto de líneas, un único polígono o un conjunto de polígonos. Cada elemento del mapa es una unidad. Si un elemento del mapa contiene varios puntos y mueve el elemento, todos los puntos de ese elemento del mapa se moverán.  
  
 El formato de los datos espaciales del origen externo determina los datos de cada elemento de mapa. Por ejemplo, cuando una consulta especifica los datos espaciales de una base de datos de SQL Server, cada fila del conjunto de resultados puede contener varios conjuntos de coordenadas de un polígono, línea o punto. Todos los elementos de mapa que se definen con una única fila en el conjunto de resultados se tratan como una unidad. Si desea variar la presentación de conjuntos concretos de coordenadas, debe elegir entre lo siguiente:  
  
-   Cambie la consulta para que devuelva los conjuntos de coordenadas como filas independientes en el conjunto de resultados.  
  
-   Seleccione los elementos de mapa para que varíen y establezca las propiedades del polígono, línea o punto incrustado correspondientes invalidando las propiedades de presentación predeterminadas del tipo de capa correspondiente.  
  
## <a name="my-layer-that-uses-spatial-data-from-an-esri-shapefile-always-has-embedded-data"></a>Una capa, que utiliza datos espaciales de un archivo de forma ESRI, siempre tiene datos insertados.  
 Para asegurarse de que los informes con mapas se pueden ejecutar en un servidor de informes, los archivos de forma ESRI deben estar disponibles como recursos en el servidor de informes. Si agrega una capa a un mapa y especifica un archivo de forma que está en el sistema de archivos local, los datos espaciales se incrustan automáticamente en el informe.  
  
 Para reemplazar los datos insertados con un vínculo al archivo de forma ESRI, debe cargar el archivo ESRI .shp y el archivo .dbf correspondiente en el servidor de informes, y después cambiar el origen de datos espaciales para la capa.  
  
## <a name="i-renamed-a-data-source-or-dataset-to-a-friendly-name-and-now-no-data-appears-in-my-map"></a>Cambié el nombre de un origen de datos o conjunto de datos por un nombre descriptivo y ahora no aparece ningún dato en el mapa.  
 La definición de informe no se actualiza automáticamente al cambiar manualmente el nombre de un elemento de informe.  
  
 Al cambiar el nombre de un conjunto de datos, cualquier región de datos o capa de mapa que se refiera a ese conjunto de datos debe actualizarse de forma manual. Para cambiar el enlace de un Tablix, gráfico o medidor a un conjunto de datos, seleccione el elemento en la superficie de diseño, abra las propiedades de la región de datos y seleccione el nombre del conjunto de datos apropiado. Para cambiar el enlace de una capa de mapa a un conjunto de datos, seleccione la capa, abra las propiedades de capa y seleccione el nombre del conjunto de datos adecuado.  
  
## <a name="my-spatial-data-contains-nulls-and-empty-strings"></a>Mis datos espaciales contienen cadenas nulas y vacías.  
 En los datos espaciales de un elemento de informe de un mapa, los valores nulos se establecen en cero (0) y las cadenas vacías se quedan en blanco ("").  
  
 En el caso de datos espaciales que proceden de una base de datos de SQL Server, para cambiar este comportamiento, debe cambiar la consulta que devuelve los datos espaciales.  
  
## <a name="my-map-exceeds-the-maximum-number-of-spatial-elements"></a>Mi mapa supera el número máximo de elementos espaciales  
 De forma predeterminada, un mapa puede tener 20.000 elementos de mapa o 1.000.000 puntos. Si un mapa supera estos límites, puede aplicar una de las soluciones siguientes:  
  
-   Quitar una capa.  
  
-   Disminuir la resolución del mapa.  
  
-   Disminuir las coordenadas de la ventanilla del mapa para ver un área menor.  
  
-   Si los datos espaciales proceden de un conjunto de datos de informe, establezca un filtro para limitar los datos del conjunto de datos. El filtro se debe establecer en un campo que no sea un tipo de datos espaciales.  
  
-   Si los datos espaciales proceden de una base de datos de SQL Server, cambie la consulta para utilizar funciones espaciales con el fin de limitar los datos a un área más pequeña.  
  
##  <a name="viewport-center-and-view-issues"></a><a name="Viewport"></a> Problemas de las vistas y del centro de las ventanillas  
 Utilice esta sección como ayuda para resolver problemas relacionados con las opciones de las ventanillas.  
  
## <a name="i-cannot-set-the-center-and-view-on-an-embedded-map-element"></a>No puedo establecer el centro y la vista en un elemento de mapa incrustado.  
 Para centrar una ventanilla en un elemento de mapa concreto, debe haber asociado los datos espaciales de una capa a los datos analíticos.  
  
## <a name="i-set-the-map-center-and-view-in-my-report-when-i-reopen-the-report-why-isnt-the-map-view-the-same"></a>Establecí el centro y la vista de un mapa en un informe. Cuando vuelvo a abrir el informe, ¿por qué la vista del mapa no es la misma?  
 Si las credenciales del usuario que se necesitan para leer los datos espaciales no están disponibles para el informe al abrirlo, se utilizan datos espaciales de marcador de posición. Según las opciones de centro y zoom establecidas para la ventanilla del mapa, la vista del mapa podría centrarse en una capa diferente.  
  
 Para volver a cargar los datos espaciales y usar el centro de la vista de mapa guardado en el informe, haga clic con el botón derecho en la ventanilla del mapa y, después, haga clic en **Volver a cargar**. Después de escribir las credenciales del origen de datos espaciales, la capa carga los datos espaciales y la vista del mapa se restaura.  
  
## <a name="the-center-and-view-for-a-map-layer-option-does-not-work"></a>El centro y la vista de una opción de las capas de mapa no funcionan.  
 Cuando la ventanilla se establece para centrar los datos espaciales de una capa concreta y el centro de la vista no parece estar en el centro de la capa, probablemente haya pequeñas islas o áreas que estén incluidas en los datos espaciales y sean demasiado pequeñas para ser vistas en la ventanilla. Por ejemplo, los datos espaciales de un país podrían incluir islas pequeñas u otros territorios pequeños como parte del territorio. La ventanilla utiliza todos los datos espaciales para calcular el centro de la capa.  
  
 Para invalidar los cálculos de la capa, puede elegir entre lo siguiente:  
  
-   Especificar un centro personalizado para la ventanilla.  
  
-   Cambiar el nivel de ampliación para la ventanilla con el fin de eliminar las ubicaciones que no desea incluir.  
  
-   Incrustar los datos espaciales en el informe y eliminar las ubicaciones que no se desea incluir.  
  
##  <a name="layer-issues"></a><a name="Layers"></a> Problemas con las capas  
 Utilice esta sección como ayuda para resolver problemas relacionados con las opciones de las capas.  
  
## <a name="i-do-not-see-one-or-more-layers-in-my-map"></a>No veo una o varias capas de un mapa.  
 El que se vea o no una capa de mapa en un informe depende de la disponibilidad de los datos espaciales, la relación entre estos y los datos analíticos, el tipo de datos espaciales y el tipo de capa correspondiente, las opciones de transparencia y visibilidad de la capa, y el orden de dibujo de la capa. Si no ve los datos de una capa, compruebe las opciones siguientes:  
  
-   **El tipo de capa y el tipo de datos espaciales.** El tipo de capa solo muestra los datos espaciales que coinciden con el tipo de capa. Por ejemplo, si el tipo de capa es Punto pero los datos espaciales son Línea, no aparecerá ningún dato.  
  
-   **Valores de los campos coincidentes.** Los valores de los campos que especifique para relacionar los datos analíticos y los datos espaciales deben identificar cada elemento de mapa de forma única. Los campos deben tener el mismo tipo de datos. Los valores de los campos deben ser idénticos. Para obtener más información, vea [Problemas con la leyenda, la escala de colores y la escala de distancia](#Legend).  
  
-   **Orden de las capas.** El orden de las capas en el panel Mapa es aquel con que se dibujan las capas en el representador de informes. Los datos espaciales de las capas que se dibujan primero podrían sobrescribirse con los de las capas que se dibujen después. Primero se dibujan las capas que aparecen en la parte superior de la lista. Al cambiar el orden de las capas en la lista, cambia el orden del dibujo de las capas.  
  
-   **Transparencia.** Puede especificar independientemente la transparencia de cada capa de mapa. Los valores predeterminados de la transparencia difieren en función de cómo se agregue una capa. Un valor de transparencia del 0% significa que la capa es opaca y que ningún dato de otra capa se mostrará a través de esta. Para permitir que otros datos se muestren a través de una capa existente, ajuste el valor a un porcentaje mayor que le proporcione el efecto que desee.  
  
-   **Visibilidad.** La visibilidad de una capa es **Visible**, **Oculta** o **ZoomBased**, según el nivel de zoom de la ventanilla del mapa. También se puede especificar el intervalo máximo y mínimo del nivel de ampliación. La visibilidad puede basarse en una expresión que se evalúa como uno de estos valores.  
  
    > [!TIP]  
    >  Puede alternar la visibilidad para cada capa en el panel Mapa. Al diseñar cada capa, desactive todas las demás para determinar si el problema es de una capa individual o de la transparencia entre las capas.  
  
## <a name="i-set-a-filter-on-the-map-layer-and-it-has-no-effect"></a>Establecí un filtro en la capa de mapa y no tiene ningún efecto.  
 Para filtrar los datos de una capa, se debe especificar el tipo de datos de la expresión de filtro. Compruebe que ha especificado el tipo de datos subyacente correcto para que la ecuación de filtro evalúe correctamente la condición especificada. Para más información, vea [Ejemplos de ecuaciones de filtro &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md).  
  
##  <a name="legend-color-scale-and-rule-issues"></a><a name="Legend"></a> Problemas con la leyenda, la escala de colores y la regla  
 Utilice esta sección como ayuda para resolver problemas relacionados con las opciones de regla, leyenda y escala de colores.  
  
## <a name="how-do-i-control-the-values-in-the-map-legend"></a>¿Cómo controlo los valores de la leyenda del mapa?  
 Los valores de la leyenda se determinan automáticamente con las reglas de los tipos de elementos de mapa que especifique para cada capa de mapa y con las reglas de distribución que especifique para la leyenda.  
  
 De forma predeterminada, todos los elementos generados por todas las reglas se muestran en la primera leyenda. Los valores de todas las reglas de polígono, línea y punto de cada capa contribuyen al intervalo de la leyenda combinado. Para mostrar los elementos en leyendas diferentes, primero debe crear varias y, a continuación, para cada regla, especificar en qué leyenda mostrar los elementos relacionados.  
  
 Para asociar una regla a una leyenda concreta, abra las propiedades de regla y, en la página Leyenda, seleccione el nombre de la leyenda que se utilizará. Para quitar los elementos de una leyenda, en las opciones de la leyenda, seleccione la línea en blanco como nombre de la misma. Si cambia el nombre de los elementos de leyenda en un informe, debe asociar manualmente cada capa al elemento de leyenda adecuado.  
  
 Para controlar el título y el contenido de cada leyenda, utilice las propiedades de Leyenda para la regla. Puede especificar cuántas divisiones crear, cambiar los cálculos que asignan valores a cada división, establecer los valores máximo y mínimo del intervalo, y cambiar el formato del texto de la leyenda.  
  
 Para obtener más información, vea [Cambiar leyendas de mapa, escala de colores y reglas asociadas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
## <a name="the-rules-that-i-set-do-not-give-the-results-that-i-expect"></a>Las reglas que establecí no proporcionan los resultados que espero.  
 Las reglas se aplican a los datos analíticos que están asociados a los elementos de mapa en una capa. Utilice la lista siguiente para ayudar a identificar los problemas de todas las reglas de colores, reglas de tamaño, reglas de ancho y reglas de tipo de marcador:  
  
-   La prioridad para aplicar un estilo a cada elemento de mapa (polígono, línea, punto) es, de menor a mayor prioridad: propiedades de capa; propiedades de elementos de capa para todos los elementos de mapa de la capa; reglas que especifique; y, a continuación, para los elementos de mapa insertados para los que seleccione la opción de invalidar, los valores que especifique. Cuando seleccione la opción de invalidar para un elemento incrustado, las reglas dejarán de aplicarse, aun cuando cambie los valores de nuevo a su configuración original.  
  
-   Problemas con los campos coincidentes. Los campos coincidentes habilitan el enlace de datos entre los elementos de mapa y los datos analíticos. Los campos de datos espaciales y de datos analíticos que se corresponden con los campos coincidentes deben tener el mismo tipo de datos y el mismo formato. Si el campo coincidente no coincide exactamente con los datos espaciales y los datos analíticos, la regla no tiene ningún efecto. Por ejemplo, si el campo coincidente para los datos espaciales tiene espacios en blanco adicionales o un signo de puntuación adicional en comparación con el campo coincidente para los datos analíticos, no se produce ninguna coincidencia.  
  
-   Para obtener más información, vea [Variar la presentación de polígonos, líneas y puntos usando reglas y datos analíticos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/vary-polygon-line-and-point-display-by-rules-and-analytical-data.md).  
  
## <a name="what-is-the-value-nan-on-the-color-scale"></a>¿Cuál es el valor NaN en la escala de colores?  
 **NaN** significa Not a number (no es un número). Se espera que los valores de las escalas de colores sean numéricos. Compruebe en la configuración de la distribución y el valor de texto de la leyenda las reglas asociadas a la escala de colores. Si creó los intervalos de distribución personalizados, compruebe que especificó el límite inferior en el primer intervalo y el límite superior en el último.  
  
## <a name="my-color-scale-does-not-appear-when-i-run-the-report"></a>Mi escala de colores no aparece cuando ejecuto el informe.  
 La escala de colores muestra información al usuario cuando una capa de mapa especifica reglas de colores para los polígonos, líneas o puntos de toda la capa o de los elementos de mapa incrustado. Si ningún elemento de mapa especifica una regla de colores o si las reglas de colores se especifican con una leyenda en lugar del mapa de colores, el mapa de colores no aparece en el informe representado.  
  
 Para mostrar la escala de colores, especifique las reglas de colores para una capa o un elemento de mapa incrustado. Para obtener más información, vea [Cambiar leyendas de mapa, escala de colores y reglas asociadas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/change-map-legends-color-scale-and-associated-rules-report-builder-and-ssrs.md).  
  
##  <a name="tile-issues"></a><a name="Tile"></a> Problemas con el mosaico  
 Utilice esta sección como ayuda para resolver problemas relacionados con las opciones del fondo de mosaico.  
  
## <a name="i-cannot-see-the-bing-maps-tile-background"></a>No puedo ver el fondo de mosaicos de Bing Maps.  
 La configuración siguiente afecta a si el fondo de mosaico de Bing Maps se muestra en la vista previa local o en un informe que se ejecuta desde el servidor de informes:  
  
-   La capa del mosaico del mapa debe existir. En el Asistente para mapas o en el Asistente para capas, seleccione **Agregar un fondo de Bing Maps a esta vista del mapa**. Así se agrega una capa de mosaico para el nivel de zoom y el centro de la ventanilla del mapa actual. También puede agregar una capa de mosaico desde la barra de herramientas del panel Mapa.  
  
-   El sistema de coordenadas del mapa para la ventanilla debe ser **Geográfico**, no **Planar**.  
  
-   La proyección del mapa debe ser **Mercator**.  
  
-   Para la vista previa local, debe tener acceso a Internet. Si el informe se ejecuta desde el servidor de informes, este debe configurarse para ser compatible con el fondo de mosaico. Para más información, consulte [Planeamiento de un informe de asignaciones](plan-a-map-report-report-builder-and-ssrs.md).  
  
 Para más información sobre cómo agregar una capa de mosaico, vea [Agregar, cambiar o eliminar un mapa o una capa de mapa &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-change-or-delete-a-map-or-map-layer-report-builder-and-ssrs.md).  
  
## <a name="how-do-i-control-the-text-on-a-tile-layer"></a>¿Cómo controlo el texto en una capa de mosaico?  
 Tanto la vista **Carretera** como **Híbrida** incluyen texto. El texto forma parte de los mosaicos que proceden de Servicios web de Bing Maps.  
  
 Para incluir una capa de mosaico sin texto, seleccione la vista **Aérea** .  
  
##  <a name="tooltip-and-label-issues"></a><a name="Tooltip"></a> Problemas con la información sobre herramientas y las etiquetas  
 Utilice esta sección como ayuda para resolver problemas relacionados con las opciones de Información sobre herramientas o de etiquetas.  
  
## <a name="i-get-an-expression-error-about-dataset-scope-when-i-set-a-label-or-tooltip-to-an-expression"></a>Aparece un error de expresión sobre el ámbito del conjunto de datos al establecer una etiqueta o Información sobre herramientas en una expresión.  
 Cuando los datos espaciales proceden de una galería de mapas o de un archivo de forma ESRI, los datos asociados no forman parte de un conjunto de datos de informe. No puede utilizar la sintaxis de las expresiones para que una referencia del campo de conjunto de datos especifique estos datos para una etiqueta o información sobre herramientas.  
  
 Para especificar los datos relacionados con los datos espaciales que no forman parte de un conjunto de datos de informe, debe utilizar el símbolo # seguido de una etiqueta que especifique el nombre de los datos.  
  
## <a name="see-also"></a>Consulte también  
 [Mapas &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/maps-report-builder-and-ssrs.md)   
 [Solucionar problemas del Generador de informes](/previous-versions/sql/2014/reporting-services/troubleshoot-report-builder?view=sql-server-2014&preserve-view=true)  
  
  
