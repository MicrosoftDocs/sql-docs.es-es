---
title: Agrupar datos por columnas o por filas en un informe para dispositivos móviles | Reporting Services | Microsoft Docs
description: En el Publicador de informes móviles, puede organizar los datos por columnas o por filas en muchos tipos de gráficos. En este artículo se muestran los datos estructurados por columnas o por filas.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: mobile-reports
ms.topic: conceptual
ms.assetid: b9ebd36c-a337-47ae-83e5-6c2f2144eb52
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: d775b0346ce2838abeec4bebce55762afd3b0adc
ms.sourcegitcommit: ea0bf89617e11afe85ad85309e0ec731ed265583
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/28/2020
ms.locfileid: "92907333"
---
# <a name="group-data-by-columns-or-rows-in-a-mobile-report--reporting-services"></a>Agrupar datos por columnas o por filas en un informe para dispositivos móviles | Reporting Services
Los datos se pueden organizar por columnas o por filas en muchos tipos de gráficos del [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)]. Siga el procedimiento detallado de este tema.

En los gráficos de tiempo, de valores totales, circulares y de embudo, los datos se pueden organizar por filas o por columnas. 
* La organización por columnas es útil si una tabla tiene varias columnas de datos que quiere comparar. 
* La organización por filas funciona mejor si una columna de la tabla contiene los nombres de las distintas categorías. 

En los siguientes pasos se usa una tabla de comparación de valores totales con los datos simulados en el [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] para ilustrar las diferencias cuando los datos se estructuran por filas o por columnas en un gráfico.  

1. Arrastre un **gráfico de totales de comparación** desde la pestaña **Diseño** a la superficie de diseño y agrándelo.

2. Seleccione la pestaña **Datos** . Verá que la tabla SimulatedTable contiene una serie de columnas, de **Metric1** a **Metric5** y de **Comparison1** a **Comparison5**. 

   ![Captura de pantalla de columnas con grupos de datos del informe móvil.](../../reporting-services/mobile-reports/media/mobile-report-data-group-column.png)

3. En el panel **Propiedades de los datos** , **Serie principal** es **SimulatedTable**. Seleccione la flecha del cuadro junto a **Serie principal** y verá que las opciones de **Metric1** a **Metric5** están seleccionadas.

   ![Captura de pantalla de las opciones junto a la Serie principal.](../../reporting-services/mobile-reports/media/mobile-report-properties-columns.png)

   Del mismo modo, en **Serie de comparaciones** -- **Comparison1** a **Comparison5** están seleccionadas.
   
4. Seleccione **Vista previa**.

   ![Captura de pantalla de la vista previa del gráfico de los valores totales de comparaciones.](../../reporting-services/mobile-reports/media/mobile-report-chart-by-columns.png)

   Cada barra del gráfico representa una columna de la tabla. Las barras más gruesas son las columnas de las métricas y las más delgadas, las columnas de las comparaciones.

5. Seleccione la flecha Atrás situada en la esquina superior izquierda para salir del modo de vista previa.

6. En la pestaña **Diseño** , en el panel **Propiedades de los elementos visuales** , cambie **Estructura de los datos** de **Por columnas** a **Por filas**.  

7. Seleccione la pestaña **Datos** . Ahora la tabla SimulatedTable tiene una columna **Categoría** junto con las columnas **Métrica** y **Comparación** con las categorías de la A a la E. 

   ![Captura de pantalla de filas con grupos de datos del informe móvil.](../../reporting-services/mobile-reports/media/mobile-report-data-group-rows.png)

8.  En el panel **Propiedades de los datos** , ahora hay un cuadro de columna de categoría que muestra la columna Categoría de SimulatedTable. En Serie principal, puede elegir la columna que se usará para los valores. De forma predeterminada, el [!INCLUDE[SS_MobileReptPub_Short](../../includes/ss-mobilereptpub-short.md)] selecciona de Metric1 a Metric5 como series principales, y de Comparison1 a Comparison5 como series de comparación. 

    ![Captura de pantalla de las opciones junto a la Serie de comparaciones.](../../reporting-services/mobile-reports/media/mobile-report-properties-rows.png)

9. Seleccione **Vista previa**.

   ![Captura de pantalla de la vista previa del gráfico de los valores totales de comparaciones actualizados.](../../reporting-services/mobile-reports/media/mobile-report-chart-by-rows.png)

   Ahora, cada barra del gráfico representa los valores de cada categoría en la columna Categoría.

### <a name="see-also"></a>Consulte también
* [Visualizaciones de informes para dispositivos móviles de Reporting Services](../../reporting-services/mobile-reports/add-visualizations-to-reporting-services-mobile-reports.md)
