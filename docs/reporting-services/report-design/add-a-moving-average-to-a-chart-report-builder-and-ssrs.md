---
title: Adición de una media móvil a un gráfico (Generador de informes) | Microsoft Docs
description: Obtenga información sobre cómo se puede mostrar el indicador de precio de la fórmula Media móvil en un gráfico para identificar tendencias en el Generador de informes.
ms.date: 03/03/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: 166cf9c1-0750-4866-8381-542e4fbfe65a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3820bee4c008bb82a2c1ffdb8a4a6bfb00335958
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907269"
---
# <a name="add-a-moving-average-to-a-chart-report-builder-and-ssrs"></a>Agregar una media móvil a un gráfico (Generador de informes y SSRS)
Una media móvil es una media de los datos de la serie, calculada en un período de tiempo definido. En los informes paginados de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , la media móvil se puede mostrar en el gráfico para identificar tendencias significativas.  

![Captura de pantalla de un gráfico de Ventas.](../../reporting-services/media/report-builder-column-chart-tutorial.png)
  
 La fórmula de la media móvil es el indicador de precio más habitual en los análisis técnicos. También se pueden derivar de una serie del gráfico muchas otras fórmulas, como el promedio, la mediana y la desviación estándar. Al especificar una media móvil, cada fórmula puede tener uno o varios parámetros que deberá especificar.  
 
 En [Tutorial: agregar un gráfico de columnas a un informe (Generador de informes)](../tutorial-add-a-column-chart-to-your-report-report-builder.md) se explica cómo agregar una media móvil a un gráfico, en el caso de que quiera probarlo con datos de ejemplo.
  
 Cuando se agrega una fórmula de media móvil en modo de diseño, la serie de líneas que se agrega es solo un marcador de posición visual. El gráfico calculará los puntos de datos de cada fórmula durante el procesamiento del informe.  
  
 En [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], no está disponible la compatibilidad integrada para las líneas de tendencia.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="to-add-a-calculated-moving-average-to-a-series-on-the-chart"></a>Para agregar una media móvil calculada a una serie del gráfico  
  
1.  Haga clic con el botón secundario en el área **Valores** y, a continuación, haga clic en **Agregar serie calculada**. Se abre el cuadro de diálogo **Propiedades de la serie calculada** .  
  
2.  Seleccione la opción **Media móvil** en la lista desplegable **Fórmula** .  
  
3.  Especifique un valor entero para el **Período** que representa el período de la media móvil.  
  
    > [!NOTE]  
    >  El período es el número de días usado para calcular una media móvil. Si en el eje X no se especifican valores de fecha y hora, el período de tiempo lo representa el número de puntos de datos usados para calcular una media móvil. Si solo hay un punto de datos, la fórmula de la media móvil no se calcula. La media móvil se calcula a partir del segundo punto. Si especifica la opción **Empezar desde el primer punto** , el gráfico iniciará la media móvil en el primer punto. Si solo hay un punto de datos, el punto de la media móvil calculada será idéntico al primer punto de la serie original.  
  
## <a name="see-also"></a>Consulte también  
* [Tutorial: Incorporación de un gráfico de columnas a un informe (Generador de informes)](../tutorial-add-a-column-chart-to-your-report-report-builder.md)
*  [Aplicar formato a un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
*  [Gráficos &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
*  [Agregar puntos vacíos al gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md)  
  
  
