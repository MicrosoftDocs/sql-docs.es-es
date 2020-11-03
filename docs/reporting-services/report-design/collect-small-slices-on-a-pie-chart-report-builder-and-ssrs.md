---
title: Recopilación de segmentos pequeños en un gráfico circular (Generador de informes) | Microsoft Docs
description: Aprenda a recopilar muchos segmentos pequeños de un gráfico circular en un único segmento en los informes paginados del Generador de informes.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 21c2b8cb-b9ca-4bc0-bf49-50ba432562f6
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4e51e1a12f28ae18ff6ba833ace19a1a97ba72dd
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907244"
---
# <a name="collect-small-slices-on-a-pie-chart-report-builder-and-ssrs"></a>Recopilar segmentos pequeños en un gráfico circular (Generador de informes y SSRS)
Los gráficos circulares con demasiados segmentos pueden parecer abarrotados. Aprenda a combinar muchos segmentos pequeños de un gráfico circular en un único segmento en informes paginados de [!INCLUDE[ssRSnoversion_md](../../includes/ssrsnoversion-md.md)].
 
 Para recopilar sectores pequeños en un solo sector, en primer lugar debe decidir si el umbral de recopilación de sectores pequeños será un porcentaje del gráfico circular o un valor fijo. 
 
 El [Tutorial: Agregar un gráfico circular a un informe (Generador de informes)](../tutorial-add-a-pie-chart-to-your-report-report-builder.md) le guía a lo largo del proceso de combinación de segmentos pequeños en uno solo, si primero quiere probar esto con datos de ejemplo.
 
 ![Captura de pantalla del gráfico circular de un generador de informes que muestra el otro segmento.](../../reporting-services/report-design/media/report-builder-pie-chart-other-slice.png)
  
 También puede recopilar sectores pequeños en un segundo gráfico circular al que se llamará desde un sector recopilado del primer gráfico circular. El segundo gráfico circular se dibuja a la derecha del gráfico circular original.  
  
 En los gráficos de embudo y en los piramidales, no es posible combinar varios sectores en uno solo.  
  
 
## <a name="to-collect-small-slices-into-a-single-slice-on-a-pie-chart"></a>Para recopilar sectores pequeños en un solo sector de un gráfico circular  
  
1.  Abra el panel de propiedades.  
  
2.  En la superficie de diseño, haga clic en cualquier sector del gráfico circular. Las propiedades de la serie se muestran en el panel de propiedades.  
  
3.  En la sección **General** , expanda el nodo **CustomAttributes** .  
  
4.  Establezca la propiedad CollectedStyle en **SingleSlice**.  

    ![Captura de pantalla del gráfico circular de un generador de informes que muestra cómo configurar una propiedad de segmento individual.](../../reporting-services/media/report-builder-pie-chart-single-slice-property.png)
  
5.  Establezca el valor del umbral de recopilación y el tipo de umbral. En los ejemplos siguientes se muestran formas habituales de establecer umbrales de recopilación.  
  
    -   **Por porcentaje.** Por ejemplo, para recopilar en un solo sector cualquier sector del gráfico circular que esté por debajo del 10%:  
  
         Establezca la propiedad CollectedThresholdUsePercent en **True**.  
  
         Establezca la propiedad CollectedThreshold en **10**.  
  
        > [!NOTE]  
        >  Si establece CollectedStyle en **SingleSlice** , CollectedThreshold en un valor mayor que **100** y CollectedThresholdUsePercent en **True** , el gráfico inicia una excepción, porque no puede calcular un porcentaje. Para resolver este problema, establezca CollectedThreshold en un valor menor que **100**.  
  
    -   **Por valor de datos.** Por ejemplo, para recopilar en un solo sector cualquier sector del gráfico circular que esté por debajo de 5000:  
  
         Establezca la propiedad CollectedThresholdUsePercent en **False**.  
  
         Establezca la propiedad CollectedThreshold en **5000**.  
  
6.  Establezca la propiedad CollectedLabel en una cadena que represente la etiqueta de texto que se mostrará en el sector recopilado.  
  
7.  (Opcional) Establezca las propiedades CollectedSliceExploded, CollectedColor, CollectedLegendText y CollectedToolTip. Estas propiedades cambian el aspecto, el color, el texto de la etiqueta, el texto de la leyenda y la información sobre herramientas del sector simple.  
  
## <a name="to-collect-small-slices-into-a-secondary-callout-pie-chart"></a>Para recopilar sectores pequeños en un gráfico circular secundario con llamada  
  
1.  Siga los pasos 1 a 3 descritos anteriormente.  
  
2.  Establezca la propiedad CollectedStyle en **CollectedPie**.  
  
3.  Establezca la propiedad CollectedThreshold en un valor que represente el umbral de recopilación de los sectores pequeños en un solo sector. Cuando la propiedad CollectedStyle se establece en **CollectedPie** , la propiedad CollectedThresholdUsePercent siempre se establece en **True** y el umbral de recopilación siempre se mide como porcentaje.  
  
4.  (Opcional) Establezca las propiedades CollectedColor, CollectedLabel, CollectedLegendText y CollectedToolTip. El resto de las propiedades denominadas "Collected" no se aplican al gráfico circular recopilado.  
  
> [!NOTE]  
>  El gráfico circular secundario se calcula en función de los sectores pequeños existentes en los datos, por lo que solo se ve en Vista previa. No aparece en la superficie de diseño.  
  
> [!NOTE]  
>  No puede dar formato al gráfico circular secundario. Por esta razón, a la hora de recopilar sectores del gráfico circular se recomienda el uso del primer enfoque.  
  
## <a name="see-also"></a>Consulte también  
* [Tutorial: Agregar un gráfico circular a un informe (generador de informes)](../tutorial-add-a-pie-chart-to-your-report-report-builder.md)
*  [Gráficos circulares &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/pie-charts-report-builder-and-ssrs.md)   
*  [Aplicar formato a los puntos de datos de un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-data-points-on-a-chart-report-builder-and-ssrs.md)   
*  [Mostrar las etiquetas de los puntos de datos fuera de un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-data-point-labels-outside-a-pie-chart-report-builder-and-ssrs.md)   
*  [Mostrar valores de porcentaje en un gráfico circular &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/display-percentage-values-on-a-pie-chart-report-builder-and-ssrs.md)     
  
