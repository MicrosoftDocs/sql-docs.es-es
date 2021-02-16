---
title: Comparación de los planes de ejecución | Microsoft Docs
description: Obtenga información sobre cómo comparar las similitudes y las diferencias entre los planes de ejecución gráfica reales usando la función Comparación de planes de SQL Server Management Studio.
ms.custom: ''
ms.date: 11/21/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- comparing execution plans
- compare execution plans
- plan comparison
- execution plans [SQL Server], comparing
- Query Store, comparing plans
ms.assetid: 9e583a18-5f4a-4054-bfe1-4b2a76630db6
author: pmasl
ms.author: pelopes
manager: amitban
ms.openlocfilehash: 03b6ec22a3eadf7281f0513997cfcc28d40eb1b6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344730"
---
# <a name="compare-execution-plans"></a>Comparación de los planes de ejecución
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
En este tema se describe cómo comparar las similitudes y diferencias entre los planes de ejecución gráficos reales usando la función Comparación de planes de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Esta característica está disponible a partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] v16.
  
> [!NOTE]
> Los planes de ejecución reales se generan tras ejecutar las consultas o los lotes del [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por este motivo, un plan de ejecución real contiene información de tiempo de ejecución, como el número de filas real, las métricas de uso real de recursos o las advertencias en tiempo de ejecución (si las hubiera). Para obtener más información, vea [Mostrar un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md).
  
La capacidad de comparar los planes es algo que los profesionales de bases de datos pueden tener que hacer para solucionar problemas:
-   Busque por qué una consulta o lote se ralentizó de repente.
-   Comprenda el impacto de reescribir una consulta.
-   Observe cómo un cambio de mejora de rendimiento específico introducido en el diseño de esquema (como un nuevo índice) ha cambiado de forma eficaz el plan de ejecución.  
 
La opción de menú **Comparación de planes** permite la comparación en paralelo de dos planes de ejecución diferentes, para facilitar la identificación de las similitudes y los cambios que explican los comportamientos diferentes para todas las razones indicadas anteriormente. Esta opción puede comparar entre:
- Dos archivos de plan de ejecución guardados anteriormente (extensión *.sqlplan*).
- Un plan de ejecución de una consulta guardada anteriormente y un plan de ejecución activo.
- Dos planes de consulta seleccionados en [Almacén de datos de consultas](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md).

> [!TIP]
> Comparación de planes funciona con cualquier archivo *.sqlplan*, incluso desde versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Además, esta opción permite una comparación sin conexión, por lo que no es necesario tener conexión a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

Cuando se comparan dos planes de ejecución, las regiones del plan que **hacen básicamente lo mismo** se resaltan en el mismo color y patrón. Al hacer clic en una región de color de un plan, se centrará el otro plan en el nodo coincidente de ese plan. Todavía puede comparar operadores no coincidentes y nodos de los planes de ejecución, pero en ese caso debe seleccionar manualmente los operadores para comparar.

> [!IMPORTANT]
> Para buscar similitudes, solo se usan los nodos que se considera que cambian la forma del plan. Por lo tanto, puede haber un nodo que no se colorea en medio de dos nodos que están en la misma subsección del plan. En este caso, la falta de color implica que los nodos no se tuvieron en cuenta al comprobar si las secciones son iguales.
  
## <a name="to-compare-execution-plans"></a>Para comparar los planes de ejecución
  
1.  Abra un archivo de plan de ejecución de consulta guardado previamente (.sqlplan) desde el menú **Archivo** y haga clic en **Abrir archivo**, o arrastre un archivo de plan a la ventana [!INCLUDE[ssManStudio](../../includes/ssManStudio-md.md)]. De forma alternativa, si acaba de ejecutar una consulta y elige mostrar su plan de ejecución, vaya a la pestaña **Plan de ejecución** en el panel de resultados. 

2.  Haga clic en un área en blanco del plan de ejecución y haga clic en **Comparar plan de presentación**. 

    ![Hacer clic con el botón secundario en Comparar plan de presentación](../../relational-databases/performance/media/plancomparisonmenuoption.png "Hacer clic con el botón secundario en Comparar plan de presentación")   

3.  Elija el segundo archivo de plan de consulta con el que le gustaría comparar. El segundo archivo se abrirá para que pueda comparar los planes.

4.  Los planes comparados abrirán una nueva ventana de forma predeterminada, con uno en la parte superior y otro en la parte inferior. La selección predeterminada será la primera aparición de un operador o un nodo que es común en los planes comparados, pero que muestra diferencias entre los planes. Todos los nodos y los operadores resaltados existen en ambos planes comparados. Seleccionar un operador resaltado en los planes superiores o izquierdos selecciona automáticamente el operador correspondiente en los planes inferiores o derechos. Seleccionar el operador de nodo raíz en cualquiera de los planes comparados (el nodo SELECT en la siguiente imagen), también selecciona el operador de nodo raíz correspondiente en el otro plan comparado.

    ![Planeación de la comparación de dos archivos de plan guardados](../../relational-databases/performance/media/plancomparison-plans.png "Planeación de la comparación de dos archivos de plan guardados")  

     > [!TIP]
     > Para alternar la presentación de la comparación de planes de ejecución en paralelo, haga clic con el botón derecho en un área en blanco del plan de ejecución y seleccione **Cambiar orientación del divisor**.

     > [!TIP]
     > Todas las opciones de zoom y desplazamiento disponibles para los planes de ejecución funcionan en modo de comparación de plan. Para obtener más información, vea [Mostrar un plan de ejecución real](../../relational-databases/performance/display-an-actual-execution-plan.md).

5.  También se abre una ventana de propiedades dual en el lado derecho, en el ámbito de la selección predeterminada. Las propiedades que existen en ambos operadores comparados pero tienen diferencias estarán precedidas por el signo *no igual* (&ne;) para facilitar la identificación.

    ![Ventana de propiedades doble](../../relational-databases/performance/media/plancomparison-properties.png "Ventana de propiedades doble")  

6.  La ventana de navegación de comparación **Análisis del plan de presentación** también se abre en la parte inferior. Hay tres pestañas disponibles:

    1.  En la pestaña **Opciones de la instrucción**, la selección predeterminada es *Resaltar operaciones similares* y el mismo nodo u operador resaltado en los planes comparados comparte el mismo patrón de color y línea. Navegue entre las áreas similares en planes comparados haciendo clic en un patrón lima. También puede resaltar las diferencias en los planes en lugar de las similitudes si selecciona *Highlight operations not matching similar segments* (Resaltar las operaciones que no coinciden con segmentos similares). 
    
       > [!NOTE]
       > De forma predeterminada, se omiten los nombres de base de datos al comparar los planes para permitir la comparación de planes que se capturan para las bases de datos que tienen nombres diferentes pero comparten el mismo esquema. Por ejemplo al comparar los planes de las bases de datos *ProdDB* y *TestDB*. Este comportamiento puede cambiarse con la opción *Ignorar el nombre de la base de datos al comparar operadores*.

       ![Ventana de análisis del plan de presentación](../../relational-databases/performance/media/plancomparison-analysis.png "Ventana de análisis del plan de presentación") 

    2.  La pestaña **Instrucción múltiple** es útil para comparar los planes con varias instrucciones, permitiendo que se compare el par de instrucciones correcto.

        ![Varias instrucciones en un plan comparado](../../relational-databases/performance/media/plancomparison-multiple.png "Varias instrucciones en un plan comparado")  

    3.  En la pestaña **Escenarios** puede encontrar un análisis automatizado de algunos de los aspectos más importantes que debe tener en cuenta relacionados con las diferencias de [Cardinality Estimation](../../relational-databases/performance/cardinality-estimation-sql-server.md) (Estimación de cardinalidad) en los planes comparados. Para cada operador enumerado en el panel izquierdo, el panel derecho muestra detalles sobre el escenario en el vínculo *Para más información sobre este escenario, haga clic aquí* y se muestran los motivos posibles para explicar ese escenario. 

        ![Diferentes filas calculadas](../../relational-databases/performance/media/plancomparison-scenarios.png "Diferentes filas calculadas")  

    Si esta ventana está cerrada, haga doble clic en un área en blanco de un plan comparado y seleccione **Opciones de comparación de planes de presentación** para volver a abrirla.

    ![Opciones de comparación de planes](../../relational-databases/performance/media/plancomparison-options.png "Opciones de comparación de planes")  

## <a name="to-compare-execution-plans-in-query-store"></a>Para comparar los planes de ejecución en el Almacén de consultas

1.  En el Almacén de consultas, identifique una consulta que tenga más de un plan de ejecución. Para obtener más información sobre escenarios del Almacén de consultas, vea [Escenarios de uso del Almacén de consultas](../../relational-databases/performance/query-store-usage-scenarios.md#identify-and-tune-top-resource-consuming-queries).

2.  Combine el uso de la tecla MAYÚS y del mouse para seleccionar dos planes para la misma consulta. 

    ![Selección de dos planes en Almacén de consultas](../../relational-databases/performance/media/plancomparison-querystore.png "Selección de dos planes en Almacén de consultas")   

3.  Use el botón **Compare the plans for the select query in a separate window** (Comparar los planes de la consulta seleccionada en una ventana independiente) para iniciar la comparación de planes. Después, se aplican los pasos 4 al 6 de *Comparar los planes de ejecución*. 

    ![Comparación del plan de presentación en Almacén de consultas](../../relational-databases/performance/media/plancomparison-querystoreoption.png "Comparación del plan de presentación en Almacén de consultas") 
