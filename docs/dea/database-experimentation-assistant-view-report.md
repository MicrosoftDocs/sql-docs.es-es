---
title: Ver informes de análisis para actualizaciones de SQL Server
description: Obtenga información sobre cómo ver y entender los informes de análisis para obtener información sobre el rendimiento en Asistente para experimentación con bases de datos (DEA).
ms.custom: seo-lt-2019
ms.date: 02/04/2020
ms.prod: sql
ms.prod_service: dea
ms.suite: sql
ms.technology: dea
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: pochiraju
ms.author: rajpo
ms.reviewer: mathoma
ms.openlocfilehash: d1666d4ba2279623065dd3c0d8faf5e0653de82e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100065950"
---
# <a name="view-analysis-reports-in-database-experimentation-assistant"></a>Ver informes de análisis en Asistente para experimentación con bases de datos

Después de usar Asistente para experimentación con bases de datos (DEA) para [crear un informe de análisis](database-experimentation-assistant-create-report.md), puede revisar el informe para obtener información sobre el rendimiento en función de la prueba A/B realizada.

## <a name="open-an-existing-analysis-report"></a>Abrir un informe de análisis existente

1. En DEA, seleccione el icono de lista, especifique el nombre del servidor y el tipo de autenticación, Active o desactive las casillas **cifrar conexión** y **confiar en certificado de servidor** según corresponda para su escenario y, a continuación, seleccione **conectar**.

   ![Conectarse al servidor con el informe](./media/database-experimentation-assistant-view-report/dea-connect-to-server-with-report-files.png)

2. En la pantalla **informes de análisis** , en el lado izquierdo, seleccione la entrada para el informe que desea ver.

   ![Abrir un archivo de informe existente](./media/database-experimentation-assistant-view-report/dea-select-report-to-view.png)

## <a name="view-and-understand-the-analysis-report"></a>Ver y entender el informe de análisis

Esta sección le guía a través del informe de análisis.

En la primera página del informe, aparece la información sobre la versión y la información de compilación de los servidores de destino en los que se ejecutó el experimento. El umbral le permite ajustar la sensibilidad o la tolerancia del análisis de prueba A/B. De forma predeterminada, el umbral se establece en el 5%; cualquier mejora en el rendimiento >= 5% se clasifica como ' mejorado '.  La lista desplegable permite evaluar el informe con distintos umbrales de rendimiento.

Puede exportar los datos del informe a un archivo CSV seleccionando el botón **exportar** .  En cualquier página del informe de análisis, puede seleccionar **Imprimir** para imprimir lo que está visible en la pantalla en ese momento.

### <a name="query-distribution"></a>Distribución de consultas

- Seleccione distintos sectores de los gráficos circulares para mostrar solo las consultas que pertenecen a esa categoría.

   ![Categorías de informe como segmentos del gráfico circular](./media/database-experimentation-assistant-view-report/dea-view-report-pie-slices.png)

  - **Degradado**: consultas que han realizado peor en el destino 2 que en el destino 1.
  - **Errores**: consultas que mostraron errores al menos una vez en al menos uno de los destinos.
  - **Mejorado**: consultas que se realizaron mejor en el destino 2 que en el destino 1.
  - **No se puede evaluar**: las consultas que tenían un tamaño de muestra demasiado pequeño para el análisis estadístico. Para el análisis de pruebas A/B, DEA requiere que las mismas consultas tengan al menos 15 ejecuciones en cada destino.
  - Lo **mismo**: consultas que no tienen ninguna diferencia estadística entre el destino 1 y el destino 2.

  Las consultas de error, si las hay, se muestran en gráficos independientes; un gráfico de barras que clasifica los errores por tipo y un gráfico circular que clasifica los errores por identificador de error.

   ![Gráficos de consultas de error](./media/database-experimentation-assistant-view-report/dea-error-query-charts.png)

  Hay cuatro tipos posibles de errores:

  - **Errores existentes**: errores que existen tanto en el destino 1 como en el destino 2.
  - **Nuevos errores**: errores que son nuevos en el destino 2.
  - **Errores resueltos**: errores que existen en el destino 1, pero se resuelven en el destino 2.
  - **Actualizar bloqueadores**: errores que bloquean la actualización al servidor de destino.

  Al hacer clic en cualquier barra o sección circular de los gráficos se profundiza en la categoría y se muestran las métricas de rendimiento, incluso para la categoría **no se puede evaluar** .

  Además, el panel muestra las cinco mejores consultas mejoradas y degradadas para proporcionar una introducción rápida al rendimiento.

### <a name="individual-query-drill-down"></a>Obtención de detalles de consulta individual

Puede seleccionar vínculos de plantilla de consulta para obtener información más detallada sobre consultas específicas.

![Explorar en profundidad una consulta específica](./media/database-experimentation-assistant-view-report/dea-query-drill-down-report.png)

- Seleccione una consulta específica para abrir el Resumen de la comparación relacionada.

   ![Resumen de comparación](./media/database-experimentation-assistant-view-report/dea-view-report-comparison-summary.png)

   Puede encontrar estadísticas de resumen para esa consulta, como el número de ejecuciones, la duración media, la CPU media, las lecturas y escrituras medias y el recuento de errores.  Si la consulta es una consulta de error, en la pestaña **información de error** se muestran más detalles sobre el error.  En la pestaña **información del plan de consulta** , puede encontrar información sobre los planes de consulta usados para la consulta en el destino 1 y el destino 2.

   > [!NOTE]
   > Si está analizando el evento extendido (. XEL), la información del plan de consulta no se recopila para limitar la presión de memoria en el equipo del usuario.

## <a name="see-also"></a>Consulte también

- Para obtener información sobre cómo generar un informe de análisis en un símbolo del sistema, vea [ejecutar en el símbolo del sistema](database-experimentation-assistant-run-command-prompt.md).
