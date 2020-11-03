---
title: Especificación de un intervalo de eje (Generador de informes) | Microsoft Docs
description: Obtenga información sobre cómo cambiar el número de etiquetas y marcas de graduación en el eje de categorías (X) de un gráfico mediante el establecimiento del intervalo del eje en el Generador de informes.
ms.date: 09/02/2016
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: ae46712d-a5bf-44c0-9929-e30ccc1e7e33
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 6b756d7db3a41787da2a9a1a30f1c82b9a72317c
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907183"
---
# <a name="specify-an-axis-interval-report-builder-and-ssrs"></a>Especificar un intervalo de eje (Generador de informes y SSRS)
Aprenda a cambiar el número de etiquetas y las marcas de graduación en el eje de categorías (X) de un gráfico; para ello, establezca el intervalo de eje de un informe paginado de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)] .
 
En el eje de valores (normalmente el eje Y), los intervalos de eje proporcionan una medida coherente de los puntos de datos representados en el gráfico. 

Pero en el eje de categorías (normalmente el eje X), a veces un intervalo de eje automático genera categorías sin etiquetas de eje. Puede especificar el número de intervalos que quiere en la propiedad Intervalo del eje. [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] calcula el número de intervalos en tiempo de ejecución, según los datos del conjunto de resultados. Para más información sobre cómo se calculan los intervalos de eje, consulte [Aplicar formato a las etiquetas de los ejes de un gráfico](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  

Para probar la configuración del intervalo de eje con datos de ejemplo, consulte [Tutorial: Incorporación de un gráfico de columnas a un informe (Generador de informes)](../tutorial-add-a-column-chart-to-your-report-report-builder.md).
  
> [!NOTE]  
>  El eje de categorías normalmente es el eje horizontal o eje X. Sin embargo, en el caso de los gráficos de barras, el eje de categorías es el eje vertical o eje Y.  
>
> Este tema no se aplica a lo siguiente:
>-   Valores de fecha u hora en el eje de categorías. De forma predeterminada, los valores de tipo **DateTime** aparecen como días. Puede especificar un intervalo de fecha u hora distinto, como un intervalo mensual o de hora. Para más información, consulte [Aplicar formato de fecha o de a las etiquetas de los ejes](../../reporting-services/report-design/format-axis-labels-as-dates-or-currencies-report-builder-and-ssrs.md).  
>-  Gráficos circulares, de anillo, de embudo ni piramidales, ya que no tienen ejes. 
  
## <a name="to-show-all-the-category-labels-on-the-x-axis"></a>Para mostrar todas las etiquetas de categoría en el eje X  

En este gráfico de columnas, el intervalo de etiqueta horizontal está definido en automático.

![Captura de pantalla de la vista previa de un gráfico de columnas del generador de informes con el intervalo del eje X definido en automático.](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-auto.png)
  
1.  Haga clic con el botón derecho en el eje de categorías y haga clic en **Propiedades del eje horizontal**.   

    ![Captura de pantalla de un gráfico de columnas del generador de informes que muestra cómo definir las etiquetas del eje X.](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-labels.png)
  
2.  En el cuadro de diálogo **Propiedades del eje horizontal** > pestaña **Opciones del eje** , establezca **Intervalo** en **1** para mostrar cada etiqueta de grupo de categorías. Para mostrar cualquier otra etiqueta de grupo de categorías en el eje X, escriba **2**. 

     ![Captura de pantalla de un gráfico de columnas del generador de informes que muestra cómo definir el intervalo del eje X en uno.](../../reporting-services/report-design/media/report-builder-column-chart-x-axis-interval-one.png)
  
3. [!INCLUDE[clickOK](../../includes/clickok-md.md)]
     
     El gráfico de columnas ahora muestra todas sus etiquetas del eje horizontal.
     
     ![Captura de pantalla de la vista previa de un gráfico de columnas del generador de informes que muestra las etiquetas del eje X.](../../reporting-services/report-design/media/report-builder-column-chart-preview-x-axis-interval-one.png)
     
     > [!NOTE]  
     >  Cuando establece un intervalo de eje, se deshabilita todo el etiquetado automático. Si especifica un valor para el intervalo de eje, puede ver un comportamiento imprevisible del etiquetado en función del número de categorías existentes en el eje de categorías.  

## <a name="change-the-label-interval-in-properties-pane"></a>Cambiar el intervalo de etiqueta en el panel Propiedades

También puede establecer el intervalo de etiqueta en el panel Propiedades.

1.  En la vista Diseño del informe, haga clic en el gráfico y, luego, seleccione las etiquetas del eje horizontal.

3. En el panel Propiedades, establezca LabelInterval en **1**.

    ![Captura de pantalla del gráfico de columnas del generador de informes que muestra cómo definir el intervalo de etiqueta.](../../reporting-services/media/report-builder-column-chart-set-label-interval.png)

    El gráfico tiene el mismo aspecto en la vista Diseño. 
    
5.  Haga clic en **Ejecutar** para obtener la vista previa del informe.

    ![Captura de pantalla de la vista previa de un gráfico de columnas del generador de informes que muestra el intervalo de etiqueta en uno.](../../reporting-services/media/report-builder-column-chart-label-interval-one-preview.png)
    
    Ahora el gráfico muestra todas sus etiquetas.
  
## <a name="to-enable-a-variable-interval-calculation-on-an-axis"></a>Para habilitar el cálculo de un intervalo variable en un eje  

De manera predeterminada, [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] establece el intervalo de eje en automático. Este procedimiento explica cómo puede volver a establecerlo en el valor predeterminado. 
  
1.  Haga clic con el botón derecho en el eje del gráfico que quiera cambiar y, después, haga clic en **Propiedades del eje**. 
  
2.  En el cuadro de diálogo **Propiedades del eje horizontal** > pestaña **Opciones del eje** , establezca **Intervalo** en **Automático**. El gráfico mostrará el número óptimo de etiquetas de categoría que quepan a lo largo del eje.  
  
3.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Aplicar formato a un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
 [Aplicar formato a los puntos de datos de un gráfico (Generador de informes y SSRS)](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
 [Ordenar datos en una región de datos (Generador de informes y SSRS)](../../reporting-services/report-design/sort-data-in-a-data-region-report-builder-and-ssrs.md)   
 [Cuadro de diálogo Propiedades del eje, Opciones del eje &#40;Generador de informes y SSRS&#41;](/previous-versions/sql/)   
 [Especificar una escala logarítmica &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/specify-a-logarithmic-scale-report-builder-and-ssrs.md)   
 [Trazar datos en un eje secundario &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/plot-data-on-a-secondary-axis-report-builder-and-ssrs.md)  
  
