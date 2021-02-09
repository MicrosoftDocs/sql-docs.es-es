---
description: Características de rendimiento del flujo de datos
title: Características de rendimiento del flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- Aggregate transformation [Integration Services]
- Integration Services packages, performance
- performance [Integration Services]
- data flow [Integration Services], troubleshooting
- SQL Server Integration Services packages, performance
- loading data
- control flow [Integration Services], troubleshooting
- SSIS packages, performance
- packages [Integration Services], performance
- queries [Integration Services], troubleshooting
- sorting data [Integration Services]
- aggregations [Integration Services]
ms.assetid: c4bbefa6-172b-4547-99a1-a0b38e3e2b05
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 600ab1bce27f4fa53c2e25ae7562a870f2ccf6a6
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536362"
---
# <a name="data-flow-performance-features"></a>Características de rendimiento del flujo de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  En este tema se proporcionan sugerencias sobre cómo diseñar los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] para evitar problemas de rendimiento comunes. También proporciona información sobre las características y las herramientas que puede utilizar para solucionar problemas relacionados con el rendimiento de los paquetes.  
  
## <a name="configuring-the-data-flow"></a>Configurar el flujo de datos  
 Para configurar la tarea Flujo de datos con objeto de mejorar su rendimiento, puede configurar las propiedades de la tarea, ajustar el tamaño de los búferes y configurar el paquete para la ejecución en paralelo.  
  
### <a name="configure-the-properties-of-the-data-flow-task"></a>Configurar las propiedades de la tarea Flujo de datos  
  
> [!NOTE]  
>  Las propiedades que se analizan en esta sección deben establecerse por separado para cada tarea Flujo de datos de cada paquete.  
  
 Puede configurar las siguientes propiedades de la tarea Flujo de datos; todas ellas influyen en el rendimiento:  
  
-   Especifique las ubicaciones para el almacenamiento temporal de datos del búfer (propiedad BufferTempStoragePath) y de columnas que contengan datos de objetos binarios grandes (BLOB) (propiedad BLOBTempStoragePath). De forma predeterminada, estas propiedades contienen los valores de las variables de entorno TMP y TEMP. Es posible que desee especificar otras carpetas para colocar los archivos temporales en otra unidad de disco duro o en una más rápida, o para distribuirlos entre varias unidades. Puede especificar varios directorios delimitando los nombres de los directorios con punto y coma.  
  
-   Defina el tamaño predeterminado del búfer que usa la tarea asignando un valor a la propiedad DefaultBufferSize y establezca la cantidad máxima de filas de cada búfer asignando un valor a la propiedad DefaultBufferMaxRows. Establezca la propiedad AutoAdjustBufferSize para indicar si el tamaño predeterminado del búfer se calcula automáticamente a partir del valor de la propiedad DefaultBufferMaxRows. El tamaño de búfer predeterminado es 10 megabytes, con un tamaño de búfer máximo de 2^31-1 bytes. La cantidad máxima de filas es 10.000.  
  
-   Establezca el número de subprocesos que la tarea puede usar durante la ejecución asignando un valor a la propiedad EngineThreads. Esta propiedad le sugiere al motor de flujo de datos cuántos subprocesos utilizar. El valor predeterminado es 10, con un valor mínimo de 3; sin embargo, el motor solamente utilizará los subprocesos necesarios, independientemente del valor asignado a esta propiedad. También puede suceder que el motor utilice más subprocesos que los especificados en esta propiedad si así fuera necesario para evitar problemas de simultaneidad.  
  
-   Indique si la tarea Flujo de datos se ejecuta en el modo optimizado (propiedad RunInOptimizedMode). El modo optimizado mejora el rendimiento quitando de flujo de datos las columnas, salidas y componentes que no se utilizan.  
  
    > [!NOTE]  
    >  Una propiedad con el mismo nombre, RunInOptimizedMode, se puede establecer en el nivel de proyecto en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] para indicar que la tarea Flujo de datos se ejecuta en el modo optimizado durante la depuración. Esta propiedad del proyecto reemplaza la propiedad RunInOptimizedMode de las tareas Flujo de datos en tiempo de diseño.  
  
### <a name="adjust-the-sizing-of-buffers"></a>Ajustar el cálculo del tamaño de los búferes  
 El motor de flujo de datos comienza la tarea de ajustar el tamaño de sus búferes calculando el tamaño estimado de una fila de datos. Luego multiplica el tamaño estimado de una fila por el valor de DefaultBufferMaxRows para obtener un valor de trabajo preliminar para el tamaño de los búferes.  
  
-   Si AutoAdjustBufferSize se establece en true, el motor de flujo de datos usa el valor calculado como el tamaño del búfer y el valor de DefaultBufferSize se omite.  
  
-   Si AutoAdjustBufferSize se establece en false, el motor de flujo de datos usa las siguientes reglas para determinar el tamaño del búfer.  
  
    -   Si el resultado es superior al valor de DefaultBufferSize, el motor reduce la cantidad de filas.  
  
    -   Si el resultado es inferior al tamaño de búfer mínimo calculado internamente, el motor aumenta el número de filas.  
  
    -   Si el resultado se encuentra entre el tamaño de búfer mínimo y el valor de DefaultBufferSize, el motor ajusta el tamaño del búfer con la mayor proximidad posible al tamaño estimado de una fila multiplicado por el valor de DefaultBufferMaxRows.  
  
 Al comenzar a probar el rendimiento de las tareas de flujo de datos, use los valores predeterminados de DefaultBufferSize y DefaultBufferMaxRows. Habilite el registro de la tarea de flujo de datos y seleccione el evento BufferSizeTuning para ver cuántas filas contiene cada búfer.  
  
 Antes de empezar a ajustar el tamaño de los búferes, la mejora más importante que se puede hacer consiste en reducir el tamaño de cada fila quitando las columnas innecesarias y configurando los tipos de datos de manera adecuada.  
  
 Para determinar la cantidad óptima de búferes y sus tamaños, haga pruebas con los valores de DefaultBufferSize y DefaultBufferMaxRows mientras supervisa el rendimiento y la información que proporciona el evento BufferSizeTuning.  
  
 No aumente el tamaño de los búferes hasta un punto en el que se produzca la paginación del disco. La paginación del disco afecta al rendimiento más que al hecho de no optimizar el tamaño de los búferes. Para saber si se está produciendo la paginación, supervise el contador de rendimiento "Búferes puestos en cola" en el complemento Rendimiento de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console (MMC).  
  
### <a name="configure-the-package-for-parallel-execution"></a>Configurar el paquete para la ejecución en paralelo  
 La ejecución en paralelo mejora el rendimiento en los equipos que tienen varios procesadores físicos o lógicos. Para admitir la ejecución en paralelo de tareas diferentes del paquete, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] usa dos propiedades: **MaxConcurrentExecutables** y **EngineThreads**.  
  
#### <a name="the-maxconcurrentexcecutables-property"></a>La propiedad MaxConcurrentExcecutables  
 La propiedad **MaxConcurrentExecutables** es una propiedad del propio paquete. Esta propiedad define cuántas tareas se pueden ejecutar simultáneamente. El valor predeterminado es -1, que significa el número de procesadores físicos o lógicos más 2.  
  
 Para entender cómo funciona esta propiedad, considere un paquete de ejemplo que tiene tres tareas Flujo de datos. Si establece **MaxConcurrentExecutables** en 3, las tres tareas Flujo de datos se pueden ejecutar simultáneamente. Sin embargo, suponga que cada tarea Flujo de datos tiene 10 árboles de ejecución de origen a destino. El hecho de establecer **MaxConcurrentExecutables** en 3 no garantiza que los árboles de ejecución de cada tarea Flujo de datos se ejecuten en paralelo.  
  
#### <a name="the-enginethreads-property"></a>La propiedad EngineThreads  
 La propiedad **EngineThreads** es una propiedad de cada tarea Flujo de datos. Esta propiedad define cuántos subprocesos puede crear y ejecutar en paralelo el motor de flujo de datos. La propiedad **EngineThreads** se aplica por igual tanto a los subprocesos de origen que crea el motor de flujo de datos para los orígenes como a los subprocesos de trabajo que crea el motor para las transformaciones y los destinos. Por consiguiente, establecer **EngineThreads** en 10 significa que el motor puede crear hasta diez subprocesos de origen y hasta diez subprocesos de trabajo.  
  
 Para entender cómo funciona esta propiedad, considere el paquete de ejemplo, que tiene tres tareas Flujo de datos. Cada una de las tareas Flujo de datos contiene diez árboles de ejecución de origen a destino. Si establece EngineThreads en 10 en cada tarea Flujo de datos, es posible que los 30 árboles de ejecución se ejecuten simultáneamente.  
  
> [!NOTE]  
>  Una discusión sobre los subprocesos queda fuera del ámbito de este tema. Sin embargo, la regla general consiste en no ejecutar en paralelo un número de subprocesos superior al número de procesadores disponibles. Ejecutar en paralelo un número de subprocesos superior al número de procesadores disponibles puede afectar al rendimiento debido a los continuos cambios de contexto entre los subprocesos.  
  
## <a name="configuring-individual-data-flow-components"></a>Configurar cada uno de los componentes de flujo de datos  
 Hay algunas directrices generales que permiten configurar cada uno de los componentes de flujo de datos con objeto de mejorar el rendimiento. También hay instrucciones específicas relativas a cada tipo de componente de flujo de datos: origen, transformación y destino.  
  
### <a name="general-guidelines"></a>Directrices generales  
 Hay dos directrices generales que no dependen del componente de flujo de datos y que debería seguir para mejorar el rendimiento: optimizar las consultas y evitar las cadenas innecesarias.  
  
#### <a name="optimize-queries"></a>Optimizar las consultas  
 Varios componentes de flujo de datos utilizan consultas, ya sea al extraer datos de los orígenes o en operaciones de búsqueda para crear tablas de referencia. La consulta predeterminada usa la sintaxis SELECT * FROM \<tableName>. Este tipo de consulta devuelve todas las columnas de la tabla de origen. Disponer de todas las columnas en tiempo de diseño permite elegir cualquier columna como columna de búsqueda, de paso a través o de origen. Sin embargo, después de seleccionar las columnas que se deben utilizar, debe revisar la consulta para que incluya únicamente las columnas seleccionadas. El hecho de quitar las columnas superfluas aumenta la eficacia de flujo de datos de un paquete, ya que al haber menos columnas se crea una fila más pequeña. Una fila más pequeña significa que caben más filas en un búfer y que cuesta menos trabajo procesar todas las filas del conjunto de datos.  
  
 Para crear una consulta, puede escribirla o utilizar el Generador de consultas.  
  
> [!NOTE]  
>  Al ejecutar un paquete en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], la pestaña Progreso del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] muestra una lista de las advertencias. Entre estas advertencias se incluye la identificación de cualquier columna de datos que un origen pone a disposición de flujo de datos, pero que no utilizan posteriormente los componentes de flujo de datos de nivel inferior. Se puede utilizar la propiedad **RunInOptimizedMode** para eliminar esas columnas automáticamente.  
  
#### <a name="avoid-unnecessary-sorting"></a>Evitar ordenaciones innecesarias  
 La ordenación es una operación inherentemente lenta, y evitar la ordenación innecesaria puede mejorar el rendimiento de flujo de datos del paquete.  
  
 A veces, los datos de origen se ordenan antes de que los utilice un componente de nivel inferior. Tal ordenación puede producirse cuando la consulta SELECT utiliza una cláusula ORDER BY o cuando los datos se insertan ordenados en el origen. Para tales datos de origen preordenados, puede proporcionar una sugerencia indicando que los datos están ordenados, con lo que evitará el uso de una transformación Ordenar para satisfacer los requisitos de ordenación de ciertas transformaciones de nivel inferior. Por ejemplo, las transformaciones Mezclar y Combinación de mezcla requieren entradas ordenadas. Para proporcionar una sugerencia indicando que los datos están ordenados, deberá realizar las tareas siguientes:  
  
-   Establecer la propiedad **IsSorted** en la salida de un componente de flujo de datos de nivel superior en **True**.  
  
-   Especificar las columnas de criterio de ordenación en las que se basa el orden de los datos.  
  
 Para obtener más información, vea [Ordenar datos para las transformaciones Mezclar y Combinación de mezcla](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md).  
  
 Si es necesario ordenar los datos en el flujo de datos, puede mejorar el rendimiento diseñando el flujo de datos de forma que utilice el menor número posible de operaciones de ordenación. Por ejemplo, si el flujo de datos utiliza una transformación Multidifusión para copiar el conjunto de datos, ordene el conjunto de datos una vez antes de que se ejecute la transformación Multidifusión, en lugar de ordenar varias salidas después de la transformación.  
  
 Para obtener más información, vea [Sort Transformation](../../integration-services/data-flow/transformations/sort-transformation.md), [Merge Transformation](../../integration-services/data-flow/transformations/merge-transformation.md), [Merge Join Transformation](../../integration-services/data-flow/transformations/merge-join-transformation.md)y [Multicast Transformation](../../integration-services/data-flow/transformations/multicast-transformation.md).  
  
### <a name="sources"></a>Orígenes  
  
#### <a name="ole-db-source"></a>Origen de OLE DB  
 Cuando utilice un origen de OLE DB para recuperar datos de una vista, seleccione "comando SQL" como modo de acceso a los datos y escriba una instrucción SELECT. El hecho de tener acceso a los datos mediante una instrucción SELECT presenta un rendimiento mejor que seleccionar "Tabla o vista" como modo de acceso a los datos.  
  
### <a name="transformations"></a>Transformaciones  
 Utilice las sugerencias de esta sección para mejorar el rendimiento de las transformaciones Agregado, Búsqueda aproximada, Agrupación aproximada, Búsqueda, Combinación de mezcla y Dimensión de variación lenta.  
  
#### <a name="aggregate-transformation"></a>Transformación Agregado  
 La transformación Agregado incluye las propiedades **Keys**, **KeysScale**, **CountDistinctKeys** y **CountDistinctScale** . Estas propiedades mejoran el rendimiento habilitando la transformación para asignar previamente la cantidad de memoria que necesita la transformación para los datos que almacena en caché. Si conoce el número exacto o aproximado de grupos que se esperan como resultado de una operación **Agrupar por** , debe establecer las propiedades **Keys** y **KeysScale** , respectivamente. Si conoce el número exacto o aproximado de valores distintos que se esperan como resultado de una operación **Recuento distinto** , debe establecer las propiedades **CountDistinctKeys** y **CountDistinctScale** , respectivamente.  
  
 Si tiene que crear varias agregaciones en un flujo de datos, considere la posibilidad de crear varias agregaciones que utilicen una sola transformación Agregado, en lugar de crear varias transformaciones. Esto mejora el rendimiento cuando una agregación es un subconjunto de otra agregación, ya que la transformación puede optimizar el almacenamiento interno y examinar los datos entrantes una sola vez. Por ejemplo, si una agregación utiliza una cláusula GROUP BY y una agregación AVG, puede mejorar el rendimiento combinándolas en una transformación. No obstante, al realizar varias agregaciones dentro de una transformación Agregado se serializan las operaciones de agregación y, por consiguiente, el rendimiento podría no mejorar cuando haya que calcular varias agregaciones por separado.  
  
#### <a name="fuzzy-lookup-and-fuzzy-grouping-transformations"></a>Transformaciones Búsqueda aproximada y Agrupación aproximada  
 Para obtener información sobre cómo optimizar el rendimiento de las transformaciones Agrupación aproximada y Búsqueda aproximada, vea las notas del producto [Fuzzy Lookup and Fuzzy Grouping in SQL Server Integration Services 2005](/previous-versions/sql/sql-server-2005/administrator/ms345128(v=sql.90)).  
  
#### <a name="lookup-transformation"></a>Transformación de búsqueda  
 Puede minimizar el tamaño de los datos de referencia en memoria si escribe una instrucción SELECT que busque solo las columnas necesarias. Esta opción presenta un rendimiento mejor que seleccionar una tabla o una vista completa, lo que devuelve una gran cantidad de datos innecesarios.  
  
#### <a name="merge-join-transformation"></a>Combinación de mezcla, transformación  
 Ya no tiene que configurar el valor de la propiedad **MaxBuffersPerInput** porque Microsoft ha realizado modificaciones que reducen el riesgo de que la transformación Combinación de mezcla utilice demasiada memoria. Este problema se producía a veces cuando varias entradas de la Combinación de mezcla generaban datos a velocidades desiguales.  
  
#### <a name="slowly-changing-dimension-transformation"></a>Dimensión de variación lenta, transformación  
 El Asistente para dimensiones variables y la transformación Dimensión de variación lenta son herramientas de uso general que satisfacen las necesidades de la mayoría de los usuarios. Sin embargo, el flujo de datos que genera el asistente no está optimizado en cuanto a rendimiento.  
  
 Normalmente, los componentes más lentos de la transformación Dimensión de variación lenta son las transformaciones Comando de OLE DB que ejecutan cláusulas UPDATE sobre las filas de una en una. Por consiguiente, la manera más efectiva de mejorar el rendimiento de la transformación Dimensión de variación lenta consiste en reemplazar las transformaciones Comando de OLE DB. Puede reemplazar estas transformaciones por componentes de destino que guarden todas las filas que hay que actualizar en una tabla de ensayo. A continuación, puede agregar una tarea Ejecutar SQL que ejecute una cláusula Transact-SQL UPDATE basada en un solo conjunto sobre todas las filas al mismo tiempo.  
  
 Los usuarios avanzados pueden diseñar un flujo de datos personalizado para el procesamiento de dimensiones de variación lenta que esté optimizado para las dimensiones de gran tamaño. Para obtener una descripción y un ejemplo de este método, consulte la sección "Unique dimension scenario" en las notas del producto [Project REAL: Business Intelligence ETL Design Practices](https://www.microsoft.com/download/details.aspx?id=14582).  
  
### <a name="destinations"></a>Destinations  
 Para lograr un mejor rendimiento con los destinos, considere la posibilidad de utilizar un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y de probar el rendimiento del destino.  
  
#### <a name="sql-server-destination"></a>SQL Server, destino  
 Cuando un paquete cargue datos en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el mismo equipo, utilice un destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Este destino está optimizado para cargas masivas de alta velocidad.  
  
#### <a name="testing-the-performance-of-destinations"></a>Probar el rendimiento de los destinos  
 Es posible que guardar datos en los destinos lleve más tiempo del esperado. Para identificar si esto se debe a que el destino no es capaz de procesar los datos con suficiente rapidez, puede sustituir el destino por una transformación Recuento de filas temporalmente. Si el rendimiento mejora de forma significativa, es probable que el destino que carga los datos sea la causa de la tardanza.  
  
### <a name="review-the-information-on-the-progress-tab"></a>Revisar la información de la pestaña Progreso  
 [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona información sobre el flujo de control y sobre el flujo de datos al ejecutar un paquete en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. En la pestaña **Progreso** se muestran las tareas y los contenedores en orden de ejecución; incluye las horas de inicio y finalización, las advertencias y los mensajes de error de cada tarea y contenedor, incluido el paquete en sí. También se muestran los componentes de flujo de datos en el orden de ejecución, y se incluye información sobre su progreso, mostrado como porcentaje finalizado, y el número de filas procesadas.  
  
 Para habilitar o deshabilitar la presentación de mensajes en la pestaña **Progreso** , active o desactive la opción **Informe de progreso de depuración** del menú **SSIS** . La deshabilitación de los informes de progreso puede ayudar a mejorar el rendimiento al ejecutar un paquete complejo en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)].  
  
## <a name="related-tasks"></a>Related Tasks  
  
-   [Ordenación de datos para las transformaciones Mezclar y Combinación de mezcla](../../integration-services/data-flow/transformations/sort-data-for-the-merge-and-merge-join-transformations.md)  
  
## <a name="related-content"></a>Contenido relacionado  
 **Artículos y publicaciones de blogs**  
  
-   Artículo técnico con una [estrategia para el rendimiento en SQL Server 2005 Integration Services](/previous-versions/sql/sql-server-2005/administrator/cc966530(v=technet.10)), en technet.microsoft.com  
  
-   Artículo técnico con [técnicas de ajuste del rendimiento en SQL Server 2005 Integration Services](/previous-versions/sql/sql-server-2005/administrator/cc966529(v=technet.10)), en technet.microsoft.com  
  
-   Artículo técnico sobre cómo [incrementar el rendimiento en las canalizaciones dividiendo las transformaciones sincrónicas en varias tareas](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/SQLCAT's%20Guide%20to%20BI%20and%20Analytics.pdf), en el manual _SQLCAT's Guide to BI and Analytics_
  
-   Artículo técnico, [The Data Loading Performance Guide](/previous-versions/sql/sql-server-2008/dd425070(v=sql.100)), en msdn.microsoft.com.  
  
-   Artículo técnico, [We Loaded 1TB in 30 Minutes with SSIS, and So Can You](/previous-versions/sql/sql-server-2008/dd537533(v=sql.100)), en msdn.microsoft.com.  
  
-   Artículo técnico, [Top 10 SQL Server Integration Services Best Practices](https://techcommunity.microsoft.com/t5/datacat/top-10-sql-server-integration-services-best-practices/ba-p/305163), en sqlcat.com.  
  
-   Artículo técnico y ejemplo, [The "Balanced Data Distributor" for SSIS](https://www.sqlshack.com/ssis-balanced-data-distributor-overview/), en sqlcat.com.  
  
-   Publicación de blog acerca sobre [la solución de problemas de rendimiento de los paquetes SSIS](https://techcommunity.microsoft.com/t5/sql-server-integration-services/api-sample-oledb-source-and-oledb-destination/ba-p/387553), en blogs.msdn.com  
  
 **Vídeos**  
  
-   Serie de vídeos sobre [el diseño y el ajuste del rendimiento de los paquetes de SSIS en la empresa (serie de vídeos de SQL)](https://go.microsoft.com/fwlink/?LinkId=400878)  
  
-   Vídeo sobre [el ajuste del flujo de datos de paquetes de SSIS en la empresa (vídeo de SQL Server)](/previous-versions/ff686901(v=msdn.10)), en technet.microsoft.com  
  
-   Vídeo de [descripción de los búferes de flujo de datos de SSIS (vídeo de SQL Server)](/previous-versions/ff686905(v=msdn.10)), en technet.microsoft.com  
  
-   Vídeo sobre [los patrones de diseño de rendimiento de Microsoft SQL Server Integration Services](https://go.microsoft.com/fwlink/?LinkID=233698&clcid=0x409), en channel9.msdn.com.  
  
-   Presentación acerca de [cómo Microsoft TI aprovecha las mejoras en el motor de flujo de datos de SQL Server 2008 SSIS](https://channel9.msdn.com/Shows/TechNet+Radio/TechNet-Radio-How-Microsoft-IT-Leverages-SQL-Server-2008-SSIS-Dataflow-Engine-Enhancements), en sqlcat.com.  
  
-   Vídeo, [Balanced Data Distributor](/previous-versions/dn912438(v=msdn.10)), en technet.microsoft.com.  
  
## <a name="see-also"></a>Consulte también  
 [Herramientas para solucionar problemas con el desarrollo de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-development.md)   
 [Herramientas para solucionar problemas de la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md)  
  
