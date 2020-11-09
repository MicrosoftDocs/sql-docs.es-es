---
title: 'Tutorial: Agregar un gráfico circular a un informe (Generador de informes) | Microsoft Docs'
description: Aprenda a crear un gráfico circular en un informe paginado de Reporting Services, agregar porcentajes y combinar sectores pequeños en uno solo.
ms.date: 06/15/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: reporting-services
ms.topic: conceptual
ms.assetid: eaadf7bf-c312-428a-b214-0a1fbf959c3f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: a924e8e6ed7a19904ddc9c8cded38683b849edeb
ms.sourcegitcommit: 9e2c682929ee64c051dc62f8917d147861f7c635
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93043739"
---
# <a name="tutorial-add-a-pie-chart-to-your-report-report-builder"></a>Tutorial: Incorporación de un gráfico circular a un informe (Generador de informes)
En este tutorial, creará un gráfico circular en un informe paginado de Reporting Services. Agregue porcentajes y combine segmentos pequeños en un único segmento.

Los gráficos circulares y de anillos muestran los datos como una proporción del total. No tienen ejes. Al agregar un campo numérico en un gráfico circular, el gráfico calcula el porcentaje de cada valor en relación con el total.  

En la siguiente ilustración se muestra el gráfico circular que creará. 
 
![Captura de pantalla del gráfico circular de Report Builder.](../reporting-services/media/report-builder-pie-chart-final.png)
  
Si hay demasiados puntos de datos en un gráfico circular, es posible que las etiquetas de los puntos de datos estén demasiado amontonadas y no puedan leerse. En ese caso, considere la posibilidad de combinar varios segmentos pequeños en un segmento mayor. Los gráficos circulares son más legibles al agregar los datos en algunos puntos de datos.  
 
> [!NOTE]  
> En este tutorial, los pasos del asistente se encuentran reunidos en dos procedimientos. Para obtener instrucciones paso a paso sobre cómo ir hasta un servidor de informes, agregar un origen de datos y agregar un conjunto de datos, vea el primer tutorial de esta serie: [Tutorial: Crear un informe de tabla básico &#40;Generador de informes&#41;](../reporting-services/tutorial-creating-a-basic-table-report-report-builder.md).  
  
Tiempo estimado para completar este tutorial: 10 minutos  
  
## <a name="requirements"></a>Requisitos  
Para obtener información sobre los requisitos, vea [Requisitos previos para los tutoriales &#40;Generador de informes&#41;](../reporting-services/prerequisites-for-tutorials-report-builder.md).  
  
## <a name="1-create-a-pie-chart-from-the-chart-wizard"></a><a name="Chart"></a>1. Crear un gráfico circular a partir del Asistente para gráficos  
En esta sección, usará el Asistente para gráficos con el fin de crear un conjunto de datos incrustado, elegir un origen de datos compartido y crear un gráfico circular.  

  
1.  [Inicie el Generador de informes](../reporting-services/report-builder/start-report-builder.md) desde el equipo, el portal web de [!INCLUDE[ssRSnoversion_md](../includes/ssrsnoversion-md.md)] o el modo integrado de SharePoint.  
  
    Se abre el cuadro de diálogo **Nuevo informe o conjunto de datos** .  
  
    Si no ve el cuadro de diálogo **Nuevo informe o conjunto de datos** , vaya al menú **Archivo** > **Nuevo**.  
  
2.  En el panel de la izquierda, compruebe que está seleccionada la opción **Nuevo informe** .  
  
3.  En el panel derecho, haga clic en **Asistente para gráficos**.  
  
4.  En la página **Elegir un conjunto de datos** , haga clic en **Crear un conjunto de datos** y, después, haga clic en **Siguiente**.  
  
5.  En la página **Elegir una conexión a un origen de datos** , seleccione un origen de datos existente o vaya al servidor de informes y seleccione un origen de datos, y después haga clic en **Siguiente**. Puede que necesite escribir un nombre de usuario y contraseña.  
  
    > [!NOTE]  
    > El origen de datos que elija no importa, con tal de que tenga los permisos adecuados. No está recibiendo datos del origen de datos. Para obtener más información, consulte [Maneras alternativas de obtener una conexión de datos &#40;Generador de informes&#41;](../reporting-services/alternative-ways-to-get-a-data-connection-report-builder.md).  
  
6.  En la página **Diseñar una consulta** , haga clic en **Editar como texto**.  
  
7.  Pegue la siguiente consulta en el panel de consulta:  

    > [!NOTE]  
    > En este tutorial, la consulta contiene los valores de datos, de forma que no necesita un origen de datos externo. Esto hace que la consulta requiera tiempo. En un entorno empresarial, la consulta no contendría los datos. Esto es solo con fines de aprendizaje.  
  
    ```  
    SELECT 'Advanced Digital Camera' AS Product, CAST(254995.21 AS money) AS Sales  
    UNION SELECT 'Slim Digital Camera' AS Product, CAST(164499.04 AS money) AS Sales  
    UNION SELECT 'SLR Digital Camera' AS Product, CAST(782176.79 AS money) AS Sales  
    UNION SELECT 'Lens Adapter' AS Product, CAST(36333.08 AS money) AS Sales  
    UNION SELECT 'Macro Zoom Lens' AS Product, CAST(40199.3 AS money) AS Sales  
    UNION SELECT 'USB Cable' AS Product, CAST(53245.5 AS money) AS Sales  
    UNION SELECT 'Independent Filmmaker Camcorder' AS Product, CAST(452288.0 AS money) AS Sales  
    UNION SELECT 'Full Frame Digital Camera' AS Product, CAST(247250.85 AS money) AS Sales  
    ```  
  
8.  (Opcional) Haga clic en el botón Ejecutar ( **!** ) para ver los datos en los que se basará su gráfico.  
  
9. Haga clic en **Next**.  
  
## <a name="2-choose-the-chart-type"></a><a name="ChartType"></a>2. Elegir el tipo de gráfico  
Podrá elegir entre varios tipos de gráfico predefinidos.  

  
1.  En la página **Elegir un tipo de gráfico** , haga clic en **Circular** y, después, haga clic en **Siguiente**. Se abrirá la página **Organizar campos del gráfico** .  
  
    En la página **Organizar campos del gráfico** , arrastre el campo Product hasta el panel **Categorías** . Las categorías definen el número de segmentos del gráfico circular. En este ejemplo, habrá ocho segmentos, uno para cada producto.  
  
2.  Arrastre el campo Sales hasta el panel **Valores** . Sales representa la cantidad de ventas para la subcategoría. El panel **Valores** muestra `[Sum(Sales)]` porque el gráfico muestra al agregado para cada producto.  
  
3.  Haga clic en **Siguiente** para obtener una vista previa.  
  
5.  Haga clic en **Finalizar**  
  
    El gráfico se agrega a la superficie de diseño. No ve los valores reales del gráfico circular, ve Product 1, Product 2, etc., para darle una idea del aspecto que tendrá el gráfico.  
    
    ![Captura de pantalla del gráfico circular de Report Builder en la vista de diseño.](../reporting-services/media/report-builder-pie-chart-first-design.png)
  
6.  Haga clic en el gráfico para mostrar las asas del gráfico. Arrastre la esquina inferior derecha del gráfico para hacerlo más grande. Observe que la superficie de diseño del informe también aumenta para adaptarse al tamaño del gráfico.  
  
7.  Haga clic en **Ejecutar** para obtener la vista previa del informe.  
  
El informe muestra el gráfico circular con ocho segmentos, uno para cada producto. Ahora ve los productos reales y el tamaño de cada segmento representa las ventas de ese producto. Tres de los sectores son bastante finos.  

![Captura de pantalla en la que se muestra una vista previa del gráfico circular de Report Builder.](../reporting-services/media/report-builder-pie-chart-first-preview.png)
  
## <a name="3-display-percentages-in-each-slice"></a><a name="Percentages"></a>3. Mostrar porcentajes en cada sector  
En cada sector del gráfico circular, puede mostrar un porcentaje de este sector respecto al círculo entero.  

  
1.  Cambie a la vista de diseño del informe.  
  
2.  Haga clic con el botón derecho en el gráfico circular y haga clic en **Mostrar etiquetas de datos**. Las etiquetas de datos aparecen en el gráfico.  
  
3.  Haga clic con el botón derecho en una etiqueta y, después, haga clic en **Propiedades de la etiqueta de la serie**.  
  
4.  En el cuadro **Datos de etiqueta** , seleccione **#PERCENT**.  
    
5.  (Opcional) Para especificar el número de posiciones decimales que se deben mostrar en la etiqueta, en el cuadro **Datos de etiqueta** tras **#PERCENT** , escriba **{Pn}** , donde *n* es el número de posiciones decimales que se deben mostrar. Por ejemplo, para no mostrar ninguna posición decimal, escriba **#PERCENT{P0}** .  

6.  Para mostrar los valores como porcentajes, la propiedad UseValueAsLabel debe ser falsa. Si se le pide que establezca este valor en el cuadro de diálogo **Confirmar acción** , haga clic en **Sí**.  
  
    > [!NOTE]  
    > La opción **Formato de número** del cuadro de diálogo **Propiedades de la etiqueta de la serie** no tiene ningún efecto al dar formato a los porcentajes. Esto aplica formato de porcentaje a las etiquetas, pero no calcula el porcentaje del gráfico circular que cada sector representa.  
  
6.  [!INCLUDE[clickOK](../includes/clickok-md.md)]  
  
7.  Haga clic en **Ejecutar** para obtener la vista previa del informe.  
  
El informe muestra el porcentaje de la totalidad para cada sector del gráfico circular.  

![Captura de pantalla en la que se muestra una vista previa del gráfico circular de Report Builder con porcentajes en cada porción.](../reporting-services/media/report-builder-pie-chart-preview-percents.png)
  
## <a name="4-combine-small-slices-into-one-slice"></a><a name="CombineSlices"></a>4. Unir los sectores pequeños en un solo sector  
Tres de los sectores del gráfico son bastante pequeños. Puede combinar varios segmentos pequeños en un segmento mayor "Otros" que represente los tres.  

1.  Cambie a la vista de diseño del informe.  
  
2.  Si no ve el panel Propiedades, en la pestaña **Vista** > en el grupo **Mostrar u ocultar** , seleccione **Propiedades**.  
  
3.  En la superficie de diseño, haga clic en cualquier sector del gráfico circular. Las propiedades de la serie se muestran en el panel de propiedades.  
  
4.  En la sección **General** , expanda el nodo **CustomAttributes** .  
  
5.  Establezca la propiedad **CollectedStyle** en **SingleSlice**.  

    ![Captura de pantalla en la que se muestra cómo establecer una propiedad de una única porción en el gráfico circular de Report Builder.](../reporting-services/media/report-builder-pie-chart-single-slice-property.png)
 
6.  Compruebe que la propiedad **CollectedThreshold** esté establecida en 5.  
  
7.  Compruebe que la propiedad **CollectedThresholdUsePercent** esté establecida en **True**.  
  
8.  En la pestaña **Inicio** , haga clic en **Ejecutar** para obtener una vista previa del informe.  
  
En la leyenda, ahora ve la categoría "Otros". El nuevo sector del gráfico circular combina todos los sectores que estaban por debajo del 5% en un sector que es el 6% de todo el gráfico circular.  

![Captura de pantalla en la que se muestra que el gráfico circular de Report Builder empieza en los 90 grados desde la parte superior del gráfico.](../reporting-services/media/report-builder-pie-chart-start-at-90.png)
 
## <a name="5-start-pie-chart-values-at-the-top"></a><a name="DrawingEffect"></a>5. Iniciar los valores del gráfico circular desde la parte superior 

De forma predeterminada en los gráficos circulares, el primer valor del conjunto de datos se inicia en 90 grados desde la parte superior del círculo. Lo verá en el gráfico circular en las secciones anteriores.

En esta sección, haremos que el primer valor se inicie en la parte superior.

1.  Cambie a la vista de diseño del informe.  

2. Seleccione el gráfico.

3. En el panel Propiedades, en **Atributos personalizados** , cambie PieStartAngle de **0** a **270**.

4. Haga clic en **Ejecutar** para obtener una vista previa del informe.

Ahora, los segmentos del gráfico circular están en orden alfabético, empezando en la parte superior y finalizando con el segmento "Otros".

![Captura de pantalla en la que se muestra que el gráfico circular de Report Builder empieza en la parte superior.](../reporting-services/media/report-builder-pie-chart-start-at-top.png)
  
## <a name="6-add-a-report-title"></a><a name="Title"></a>6. Agregar un título de informe  
  
Dado que el gráfico circular es la única visualización del informe, el gráfico no necesita su propio título. Con el título del informe llega.
  
1.  En el gráfico, seleccione el cuadro Título del gráfico y pulse SUPRIMIR.

2. En la superficie de diseño, haga clic en **Haga clic para agregar un título**.  
  
2.  Escriba **Ventas de cámaras y cámaras de vídeo** , pulse ENTRAR y, después, escriba **Como porcentaje de ventas totales** , de modo que tenga este aspecto:  
  
    **Ventas de cámaras y cámaras de vídeo**  
  
    **Como porcentaje de ventas totales**  
  
3.  Seleccione **Ventas de cámaras y cámaras de vídeo** y en la pestaña **Inicio** > en la sección **Fuente** > haga clic en **Negrita**.  
  
4.  Seleccione **Como porcentaje de ventas totales** y en la pestaña **Inicio** > en la sección **Fuente** > establezca el tamaño de fuente en **10**.  
  
5.  (Opcional) Es posible que necesite hacer más alto el cuadro de texto Título para que quepan las dos líneas de texto.  
  
    Este título aparecerá en la parte superior del informe. Cuando no hay ningún encabezado de página definido, los elementos de la parte superior del cuerpo del informe son equivalentes a un encabezado de informe.  
  
6.  Haga clic en **Ejecutar** para obtener la vista previa del informe.  
  
## <a name="7-save-the-report"></a><a name="Save"></a>7. Guardar el informe  
  
### <a name="to-save-the-report"></a>Para guardar el informe  
  
1.  Cambie a la vista de diseño del informe.  
  
2.  En el menú **Archivo** , haga clic en **Guardar**.  
  
3.  En **Nombre** , escriba **Gráfico circular de ventas**.  
  
4.  Haga clic en **Save** (Guardar).  
  
El informe se guardará en el servidor de informes.  
  
## <a name="next-steps"></a>Pasos siguientes  
Ha completado correctamente el tutorial Agregar un gráfico circular al informe. Para obtener más información sobre los gráficos, vea [Gráficos &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/charts-report-builder-and-ssrs.md) y [Minigráficos y barras de datos &#40;Generador de informes y SSRS&#41;](../reporting-services/report-design/sparklines-and-data-bars-report-builder-and-ssrs.md).  
  
## <a name="see-also"></a>Consulte también  
[Tutoriales del Generador de informes](../reporting-services/report-builder-tutorials.md)  
[Generador de informes en SQL Server](../reporting-services/report-builder/report-builder-in-sql-server-2016.md)  
  

