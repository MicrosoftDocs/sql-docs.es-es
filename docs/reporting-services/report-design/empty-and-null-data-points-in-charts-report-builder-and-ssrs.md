---
title: Puntos de datos vacíos y nulos en los gráficos (Generador de informes) | Microsoft Docs
description: Obtenga información sobre el procesamiento de valores vacíos en los gráficos, el uso de marcadores de posición y cómo se muestran los puntos de datos vacíos y nulos en el Generador de informes.
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
ms.assetid: faddd29d-4cc1-4c2c-8e29-d3d9918fe22a
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 4ed8a71adea4b57d96137cc94168ca1810956bdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073789"
---
# <a name="empty-and-null-data-points-in-charts-report-builder-and-ssrs"></a>Puntos de datos vacíos y nulos en los gráficos (Generador de informes y SSRS)

  Si muestra los campos con valores vacíos o valores NULL en el gráfico de un informe paginado de [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , es posible que el gráfico no tenga el aspecto esperado. Los gráficos procesan los valores vacíos de forma distinta según el tipo de gráfico especificado:  
  
-   Si se trata de un tipo de gráfico lineal (de barras, de columnas, de dispersión, de líneas, de áreas o de intervalos), los valores vacíos se muestran como espacios vacíos o "huecos" en el gráfico. Si desea indicar los puntos vacíos, debe agregar marcadores de posición de punto vacíos. Para más información, vea [Incorporación de datos vacíos a un gráfico &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-empty-points-to-a-chart-report-builder-and-ssrs.md).  
  
-   Si el tipo de gráfico es un tipo de gráfico contiguo y lineal (áreas, barras, columnas, líneas, dispersión), se agregan puntos de datos vacíos al gráfico para mantener la continuidad de la serie.  
  
-   Si se trata de un tipo de gráfico no lineal (polar, circular, anillos, embudo o pirámide), los valores vacíos se omiten de la presentación en el gráfico.  
  
-   En los tipos de gráfico de formas, los valores NULL se omiten.  
  
 Un ejemplo de gráfico con puntos de datos vacíos está disponible como informe de ejemplo. Para más información acerca de cómo descargar este y otros informes de ejemplo, consulte el tema sobre [informes de ejemplo del Generador de informes y el Diseñador de informes](https://go.microsoft.com/fwlink/?LinkId=198283).  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
## <a name="removing-empty-or-null-values"></a>Quitar los valores vacíos o nulos  
 Para evitar ocultar datos importantes, considere la posibilidad de quitar los valores vacíos del conjunto de datos. Para filtrar los valores NULL, puede usar la cláusula NOT IS NULL en la consulta. También puede agregar una expresión de filtro que especifique que solo desea mostrar los valores distintos de cero. Para obtener más información, vea [Agregar filtros de conjunto de datos, filtros de región de datos y filtros de grupo &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-design/add-dataset-filters-data-region-filters-and-group-filters.md).  
  
## <a name="fields-with-no-values-in-a-chart"></a>Campos sin valores en un gráfico  
 Si un campo no contiene ningún valor en el conjunto de datos devuelto, el gráfico muestra un gráfico vacío sin puntos de datos, pero se agrega el nombre de la serie (normalmente el nombre del campo) como un elemento de leyenda.  
  
 Este comportamiento difiere del caso en que el conjunto de datos devuelto contiene cero filas de datos, lo que puede ocurrir cuando el informe tiene parámetros y el valor seleccionado devuelve un conjunto de resultados vacío. Si la consulta del conjunto de datos devuelve cero filas de datos, se muestra un mensaje en tiempo de ejecución para indicar que no puede mostrarse ningún dato. Puede personalizar este mensaje si modifica el título NoDataMessage del informe en el panel **Propiedades** . Para más información, vea [Conjuntos de datos incrustados y compartidos de informe &#40;Generador de informes y SSRS&#41;](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md).  

## <a name="next-steps"></a>Pasos siguientes

[Gráficos](../../reporting-services/report-design/charts-report-builder-and-ssrs.md)   
[Dar formato a un gráfico](../../reporting-services/report-design/formatting-a-chart-report-builder-and-ssrs.md)   
[Agregar un gráfico a un informe](../../reporting-services/report-design/add-a-chart-to-a-report-report-builder-and-ssrs.md)   
[Solucionar problemas de gráficos](../../reporting-services/report-design/troubleshoot-charts-report-builder-and-ssrs.md)  

¿Tiene alguna pregunta más? [Puede plantear sus dudas en el foro de Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231).
