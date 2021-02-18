---
title: Análisis de consultas con resultados de SHOWPLAN
titleSuffix: SQL Server Profiler
description: Obtenga información sobre cómo extraer eventos de plan de presentación de un seguimiento para que SQL Server Profiler muestre información del plan de consulta. Vea una tabla de eventos de seguimiento del plan de presentación.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: 6a2f7727-141c-4f59-8613-2e452bc78467
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 31771ddc584d72dee2b3c859b349dd5752e4d4d5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349452"
---
# <a name="analyze-queries-with-showplan-results-in-sql-server-profiler"></a>Analizar consultas con resultados de SHOWPLAN en SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Puede agregar clases de eventos Showplan a una definición de seguimiento que permitan que el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] recopile y muestre información del plan de consulta en el seguimiento. También es posible extraer los eventos Showplan de los otros eventos recopilados en el seguimiento y guardarlos en un archivo XML independiente.  
  
 La extracción de eventos Showplan del seguimiento puede hacerse de dos formas:  
  
-   Al configurar el seguimiento, mediante la pestaña **Configuración de extracción de eventos** . Tenga en cuenta que esta pestaña no aparece hasta que haya seleccionado uno de los eventos Showplan en la pestaña **Selección de eventos** .  
  
-   Mediante la opción **Extraer eventos de SQL Server** del menú **Archivo** .  
  
-   Mediante la extracción y almacenamiento de eventos individuales al hacer clic con el botón derecho en un evento concreto y seleccionar **Extraer datos de evento**.  
  
## <a name="showplan-events"></a>Showplan (eventos)  
 Los eventos de seguimiento Showplan se enumeran y describen en la siguiente tabla:  
  
|Nombre del evento|Descripción|  
|----------------|-----------------|  
|**Performance statistics**|Indica la primera vez que se guarda en caché un plan de presentación compilado, cuándo se vuelve a compilar y cuándo se quita de la caché del plan. La columna **TextData** contiene el plan de presentación en formato XML. Para obtener más información, vea [Performance Statistics (clase de eventos)](../../relational-databases/event-classes/performance-statistics-event-class.md).|  
|**Showplan All**|Muestra el plan de consulta con detalles completos de la compilación de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] ejecutada. Por ejemplo, puede mostrar listas de columnas y estimaciones de costes. Para más información, consulte [Showplan All Event Class](../../relational-databases/event-classes/showplan-all-event-class.md).|  
|**Showplan All For Query Compile**|Tiene lugar cuando se compila o se vuelve a compilar una consulta en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Es el equivalente en tiempo de compilación del evento **Showplan All** . **Showplan All** tiene lugar cuando se ejecuta una consulta. **Showplan All For Query Compile** tiene lugar cuando se compila una consulta. Para más información, consulte [Showplan All for Query Compile Event Class](../../relational-databases/event-classes/showplan-all-for-query-compile-event-class.md).|  
|**Showplan Statistics Profile**|Muestra el plan de consulta con detalles completos acerca del tiempo de ejecución de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se está ejecutando, incluido el número real de filas que pasan por cada operación. Para más información, consulte [Showplan Statistics Profile Event Class](../../relational-databases/event-classes/showplan-statistics-profile-event-class.md).|  
|**Showplan Text**|Muestra como datos binarios el árbol del plan de la consulta de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se está ejecutando. Para más información, consulte [Showplan Text Event Class](../../relational-databases/event-classes/showplan-text-event-class.md).|  
|**Showplan Text (Unencoded)**|Muestra como texto el árbol del plan de consulta de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se está ejecutando. Esta clase de evento muestra la misma información que Showplan Text, pero en formato de texto en lugar de datos binarios. Para obtener más información, vea [Showplan Text &#40;Unencoded&#41;, clase de eventos](../../relational-databases/event-classes/showplan-text-unencoded-event-class.md).|  
|**Showplan XML**|Muestra el plan de consulta con los datos completos recopilados durante la optimización de la consulta. Este evento solo se genera al optimizar un plan de consulta. Para más información, consulte [Showplan XML Event Class](../../relational-databases/event-classes/showplan-xml-event-class.md).|  
|**Showplan XML For Query Compile**|Muestra el plan de consulta al compilar la consulta. Para más información, consulte [Showplan XML for Query Compile Event Class](../../relational-databases/event-classes/showplan-xml-for-query-compile-event-class.md).|  
|**Showplan XML Statistics Profile**|Muestra el plan de consulta con detalles completos acerca del tiempo de ejecución en formato XML. Por ejemplo, esta clase de evento captura el número de filas que pasan por cada operador de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que se está ejecutando. Para más información, consulte [Showplan XML Statistics Profile Event Class](../../relational-databases/event-classes/showplan-xml-statistics-profile-event-class.md).|  
  
## <a name="see-also"></a>Consulte también  
 [Rendimiento (categoría de eventos)](../../relational-databases/event-classes/performance-event-category.md)  
  
  
