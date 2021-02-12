---
title: Gráficos de cotizaciones (Generador de informes) | Microsoft Docs
description: Muestre datos financieros o científicos con hasta cuatro valores por punto de datos mediante marcadores como líneas o triángulos en el Generador de informes.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: f75ca11e-b7f5-4ac0-ba17-fe6f82742dad
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: b1d7dfad5e0684dc69b725fdff2bd13d59b5da5f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076486"
---
# <a name="stock-charts-report-builder-and-ssrs"></a>Gráficos de cotizaciones (Generador de informes y SSRS)

  Los gráficos de cotizaciones están diseñados específicamente para datos financieros o científicos que usen hasta cuatro valores por punto de datos. Estos valores se corresponden con los valores máximo, mínimo, de apertura y de cierre que se usan para trazar datos de acciones financieras. Este tipo de gráfico muestra los valores de apertura y de cierre mediante marcadores, que son normalmente líneas o triángulos. En el ejemplo siguiente, los marcadores de la izquierda muestran los valores de apertura y los marcadores de la derecha muestran los valores de cierre.  
  
 ![Gráfico de cotizaciones](../../reporting-services/report-design/media/rs-stockchart.gif "Gráfico de cotizaciones")  
  
 Hay un ejemplo de gráfico de cotizaciones disponible como informe de ejemplo del Generador de informes. Para más información acerca de cómo descargar este y otros informes de ejemplo, consulte el tema sobre [informes de ejemplo del Generador de informes y el Diseñador de informes](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="variations"></a>Variaciones  
  
-   **Vela japonesa**. El gráfico de vela japonesa es una variante especializada del gráfico de cotizaciones, en el que se usan cuadros para mostrar el intervalo existente entre los valores de apertura y de cierre. Al igual que el gráfico de cotizaciones, el gráfico de vela japonesa puede mostrar hasta cuatro valores por punto de datos.  
  
## <a name="data-considerations-for-stock-charts"></a>Consideraciones sobre los datos para los gráficos de cotizaciones  
  
-   Cuando se presentan muchos puntos de datos de acciones, como la tendencia anual del precio de las acciones, resulta difícil distinguir los valores máximo, mínimo, de apertura y de cierre de cada punto de datos. En este escenario, puede resultar más factible usar un gráfico de líneas en lugar de uno de cotizaciones.  
  
-   Cuando se generan las etiquetas del eje, normalmente se empieza por el cero.  En general, los precios de las acciones no fluctúan tanto como otros conjuntos de datos. Por esta razón, es posible que desee que las etiquetas del eje no empiecen por el cero con el fin de obtener una mejor perspectiva de los datos. Para ello, establezca **IncludeZero** en **false** en el cuadro de diálogo **Propiedades del eje** o en la ventana Propiedades. Para más información sobre cómo genera el gráfico etiquetas de eje, vea [Aplicar formato a las etiquetas de los ejes de un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-axis-labels-on-a-chart-report-builder-and-ssrs.md).  
  
-   [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] proporciona muchas fórmulas calculadas para su uso con los gráficos de cotizaciones, incluyendo indicadores de precio, indicadores de fuerza relativa, convergencia de media móvil, etc.  

## <a name="next-steps"></a>Pasos siguientes

[Gráficos de intervalos](../../reporting-services/report-design/range-charts-report-builder-and-ssrs.md)   
[Gráficos](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Dar formato a un gráfico](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Cuadro de diálogo Propiedades del eje, Opciones del eje](/previous-versions/sql/)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).