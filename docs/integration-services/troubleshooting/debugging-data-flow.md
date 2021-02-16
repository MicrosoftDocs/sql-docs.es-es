---
description: Depurar el flujo de datos
title: Depurar el flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- progress reporting [Integration Services]
- data viewers [Integration Services]
- data flow [Integration Services], debugging
- debugging [Integration Services], data flow
- counting rows
ms.assetid: 1c574f1b-54f7-4c05-8e42-8620e2c1df0f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: c69bed5fbec76fab380f91515827630fa60938a5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342166"
---
# <a name="debugging-data-flow"></a>Depurar el flujo de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] incluyen características y herramientas que puede usar para solucionar los problemas de los flujos de datos en un paquete de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona visores de datos.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] y las transformaciones de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporcionan recuentos de filas.  
  
-   [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona informes de progreso en tiempo de ejecución.  
  
## <a name="data-viewers"></a>Visores de datos  
 Los visores de datos muestran datos entre dos componentes en un flujo de datos. Los visores de datos pueden mostrar datos cuando los datos se extraen de un origen de datos y entran por primera vez en un flujo de datos, antes y después de que una transformación actualice los datos, y antes de que los datos se carguen en su destino.  
  
 Para ver los datos, se adjuntan visores de datos a la ruta que conecta dos componentes de flujo de datos. La capacidad para ver datos entre componentes de flujo de datos hace que sea más fácil identificar valores de datos inesperados, ver la forma en que una transformación cambia los valores de columna y detectar el motivo por el cual una transformación genera errores. Por ejemplo, puede ocurrir que una búsqueda en una tabla de referencia genere un error, y para corregir esto puede ser necesario agregar una transformación que proporcione datos predeterminados para columnas en blanco.  
  
 Un visor de datos puede mostrar los datos en una cuadrícula. Con una cuadrícula, se seleccionan las columnas que se muestran. Los valores de las columnas seleccionadas se muestran en formato tabular.  
  
 También puede incluir varios visores de datos en una ruta. Puede mostrar los mismos datos en distintos formatos (por ejemplo, crear una vista de gráfico y una vista de cuadrícula de los datos), o bien puede crear varios visores de datos para diferentes columnas de datos.  
  
 Cuando se agrega un visor de datos a una ruta, el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] agrega un icono de visor de datos a la superficie de diseño de la pestaña **Flujo de datos** , junto a la ruta. Las transformaciones que pueden tener varias salidas, como la transformación División condicional, pueden incluir un visor de datos en cada ruta.  
  
 En el tiempo de ejecución, se abre una ventana de **Visor de datos** , que muestra la información especificada por el formato de visor de datos. Por ejemplo, un visor de datos que usa el formato de cuadrícula muestra datos para las columnas seleccionadas, la cantidad de filas de salida que se pasan al componente de flujo de datos y la cantidad de filas que se muestran. La información muestra cada búfer individualmente y, según el ancho de las filas en el flujo de datos, un búfer puede contener más o menos filas.  
  
 En el cuadro de diálogo **Visor de datos** , puede copiar los datos en el Portapapeles, eliminar todos los datos de la tabla, reconfigurar el visor de datos, reanudar el flujo de datos y separar o adjuntar el visor de datos.  
  
#### <a name="to-add-a-data-viewer"></a>Para agregar un visor de datos  
  
-   [Agregar un visor de datos a un flujo de datos](#add_viewer)  
  
## <a name="row-counts"></a>Recuentos de filas  
 La cantidad de filas que han pasado por una ruta aparece en la superficie de diseño de la pestaña **Flujo de datos** en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] junto a la ruta. La cantidad se actualiza periódicamente mientras los datos pasan por la ruta.  
  
 También puede agregar una transformación Recuento de filas al flujo de datos para capturar el recuento de filas final en una variable. Para más información, consulte [Row Count Transformation](../../integration-services/data-flow/transformations/row-count-transformation.md).  
  
## <a name="progress-reporting"></a>Informes de progreso  
 Al ejecutar un paquete, el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] muestra el progreso en la superficie de diseño en la pestaña **Flujo de datos** , mostrando cada componente de flujo de datos con un color que indica el estado. Cuando cada componente empieza a ejecutar su trabajo, cambia de incoloro a amarillo, y cuando termina correctamente, se cambia a verde. El color rojo indica un error del componente.  
  
 En la tabla siguiente se describen los códigos de colores.  
  
|Color|Descripción|  
|-----------|-----------------|  
|Sin color|Espera la llamada del motor de flujo de datos.|  
|Amarillo|Ejecución de una transformación, extracción de datos o carga de datos.|  
|Verde|Ejecución correcta.|  
|rojo|Ejecución con errores.|  

## <a name="analysis-of-data-flow"></a>Análisis de flujo de datos
  Puede usar la vista de base de datos [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) **SSISDB** para analizar el flujo de datos de los paquetes. Esta vista muestra una fila cada vez que un componente de flujo de datos envía datos a un componente de nivel inferior. La información se puede utilizar para obtener una descripción más profunda de las filas que se envían a cada componente.  
  
> [!NOTE]  
>  El nivel de registro se debe establecer en **Verbose** para capturar información en la vista catalog.execution_data_statistics.  
  
 El ejemplo siguiente muestra el número de filas enviadas entre los componentes de un paquete.  
  
```sql
use SSISDB  
select package_name, task_name, source_component_name, destination_component_name, rows_sent  
from catalog.execution_data_statistics  
where execution_id = 132  
order by source_component_name, destination_component_name   
```  
  
 El ejemplo siguiente se calcula el número de filas por milisegundo enviadas por cada componente de una ejecución concreta. Los valores calculados son:  
  
-   **total_rows** : suma de todas las filas enviadas por el componente.  
  
-   **wall_clock_time_ms**: tiempo de ejecución total transcurrido, en milisegundos, para cada componente.  
  
-   **num_rows_per_millisecond**: número de filas por milisegundo enviadas por cada componente.  
  
 La cláusula **HAVING** se usa para evitar un error de división por cero en los cálculos.  
  
```sql  
use SSISDB  
select source_component_name, destination_component_name,  
    sum(rows_sent) as total_rows,  
    DATEDIFF(ms,min(created_time),max(created_time)) as wall_clock_time_ms,  
    ((0.0+sum(rows_sent)) / (datediff(ms,min(created_time),max(created_time)))) as [num_rows_per_millisecond]  
from [catalog].[execution_data_statistics]  
where execution_id = 132  
group by source_component_name, destination_component_name  
having (datediff(ms,min(created_time),max(created_time))) > 0  
order by source_component_name desc  
```  

## <a name="configure-an-error-output-in-a-data-flow-component"></a>Configurar una salida de error en un componente de flujo de datos
  Un gran número de componentes de flujo de datos admiten salidas de errores y, dependiendo del componente, el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] proporciona diferentes maneras de configurar una salida de error. Además de configurar una salida de error, también puede configurar sus columnas correspondientes. Esto incluye configurar las columnas **ErrorCode** y **ErrorColumn** agregadas por el componente.  
  
### <a name="configuring-an-error-output"></a>Configurar una salida de error  
 Para configurar una salida de error, tiene dos opciones:  
  
-   Utilice el cuadro de diálogo **Configurar la salida de errores** . Puede usar este cuadro de diálogo para configurar una salida de error en cualquier componente de flujo de datos que la admita.  
  
-   Utilice el cuadro de diálogo del editor para el componente. Algunos componentes permiten configurar directamente salidas de errores en el cuadro de diálogo de su editor. Sin embargo, no se pueden configurar salidas de errores en el cuadro de diálogo del editor para el origen de ADO NET, la transformación Importar columna, la transformación Comando de OLE DB o el destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact.  
  
 Los procedimientos siguientes describen cómo utilizar estos cuadros de diálogo para configurar salidas de errores.  
  
#### <a name="to-configure-an-error-output-using-the-configure-error-output-dialog-box"></a>Para configurar una salida de error mediante el cuadro de diálogo Configurar la salida de errores  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  En el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en la pestaña **Flujo de datos** .  
  
4.  Arrastre la salida de error, representada por una flecha roja, del componente de origen de los errores a otro componente de flujo de datos.  
  
5.  En el cuadro de diálogo **Configurar la salida de errores** , seleccione una acción de las columnas **Error** y **Truncamiento** para cada columna de la entrada de componentes.  
  
6.  Para guardar el paquete actualizado, en el menú **Archivo** , haga clic en **Guardar los elementos seleccionados**.  
  
#### <a name="to-add-an-error-output-using-the-editor-dialog-box-for-the-component"></a>Para agregar una salida de error mediante el cuadro de diálogo del editor para el componente  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  En el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en la pestaña **Flujo de datos** .  
  
4.  Haga doble clic en los componentes de flujo de datos en los que desee configurar una salida de error y, en función del componente, realice una de las siguientes acciones:  
  
    -   Haga clic en **Configurar la salida de errores**.  
  
    -   Haga clic en **Salida de error**.  
  
5.  Establezca la opción **Error** para cada columna.  
  
6.  Establezca la opción **Truncamiento** para cada columna.  
  
7.  Haga clic en **OK**.  
  
8.  Para guardar el paquete actualizado, en el menú **Archivo** , haga clic en **Guardar los elementos seleccionados**.  
  
### <a name="configuring-error-output-columns"></a>Configurar columnas de salida de error  
 Para configurar columnas de salida de error, tiene que utilizar la pestaña **Propiedades de entrada y salida** del cuadro de diálogo **Editor avanzado** .  
  
#### <a name="to-configure-error-output-columns"></a>Para configurar columnas de salida de error  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  En el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en la pestaña **Flujo de datos** .  
  
4.  Haga clic con el botón derecho en el componente cuyas columnas de salida de error quiere configurar y haga clic en **Mostrar editor avanzado**.  
  
5.  Haga clic en la pestaña **Propiedades de entrada y salida** y expanda **Salida de error de \<component name>** y, después, expanda **Columnas de salida**.  
  
6.  Haga clic en una columna y actualice sus propiedades.  
  
    > [!NOTE]  
    >  La lista de columnas incluye las columnas de la entrada de componentes, las columnas **ErrorCode** y **ErrorColumn** agregadas por salidas de errores previas, y las columnas **ErrorCode** y **ErrorColumn** agregadas por este componente.  
  
7.  Haga clic en **Aceptar**.  
  
8.  Para guardar el paquete actualizado, en el menú **Archivo** , haga clic en **Guardar los elementos seleccionados**.  

## <a name="add-a-data-viewer-to-a-data-flow"></a><a name="add_viewer"></a> agregar un visor de datos a un flujo de datos
  En este tema se describe cómo agregar y configurar un visor de datos en un flujo de datos. Un visor de datos muestra los datos que se mueven entre dos componentes de flujo de datos. Por ejemplo, un visor de datos puede mostrar los datos que se extraen de un origen de datos antes de que una transformación en el flujo de datos los modifique.  
  
 Una ruta de acceso conecta componentes de un flujo de datos conectando la salida de un componente de flujo de datos con la entrada de otro componente.  
  
 Para poder agregar visores de datos a un paquete, éste debe incluir una tarea Flujo de datos y al menos dos componentes de flujo de datos conectados.  
  
 Agregar un visor de datos a una salida de error para que se muestre la descripción del error y el nombre de la columna en la que se produjo. De forma predeterminada, la salida de error incluye solo identificadores numéricos para el error y la columna.  
  
### <a name="to-add-a-data-viewer-to-a-data-flow"></a>Para agregar un visor de datos a un flujo de datos  
  
1.  En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el Explorador de soluciones, haga doble clic en el paquete para abrirlo.  
  
3.  Haga clic en la pestaña **Flujo de control** , si aún no está activo.  
  
4.  Haga clic en la tarea Flujo de datos que contiene el flujo de datos al que desee adjuntar un visor de datos y, a continuación, haga clic en la pestaña **Flujo de datos** .  
  
5.  Haga clic con el botón derecho en una ruta entre dos componentes de flujo de datos y, después, haga clic en **Editar**.  
  
6.  En la página **General** puede ver y editar propiedades de ruta. Por ejemplo, en la lista desplegable **PathAnnotation** puede seleccionar la anotación que aparece junto a la ruta de acceso.  
  
7.  En la página **Metadatos** puede ver los metadatos de columna y copiar los metadatos al Portapapeles.  
  
8.  En la página **Visor de datos** , haga clic en **Habilitar visor de datos**.  
  
9. En el área Columnas que se mostrarán, seleccione las columnas que desee mostrar en el visor de datos. De forma predeterminada, todas las columnas disponibles aparecen seleccionadas y en la lista **Columnas mostradas** . Mueva las columnas que no desee utilizar a la lista **Columnas sin usar** , seleccionándolas y haciendo clic en la flecha hacia la izquierda.  
  
    > [!NOTE]  
    >  En la cuadrícula, los valores que representan los tipos de datos DT_DATE, DT_DBTIME2, DT_FILETIME, DT_DBTIMESTAMP, DT_DBTIMESTAMP2 y DT_DBTIMESTAMPOFFSET aparecen como cadenas con formato ISO 8601 y un espacio separador reemplaza el separador **T** . Los valores que representan los tipos de datos DT_DATE y DT_FILETIME incluyen siete dígitos para las fracciones de segundo. Dado que el tipo de datos DT_FILETIME almacena solamente tres dígitos de fracciones de segundo, la cuadrícula muestra ceros para los cuatro dígitos restantes. Los valores que representan el tipo de datos DT_DBTIMESTAMP incluyen tres dígitos para las fracciones de segundo. Para los valores que representan los tipos de datos DT_DBTIME2, DT_DBTIMESTAMP2 y DT_DBTIMESTAMPOFFSET, el número de dígitos de las fracciones de segundo corresponde a la escala especificada para el tipo de datos de la columna. Para obtener más información acerca de los formatos ISO 8601, vea [Date and Time Formats](../data-flow/parsing-data.md). Para obtener más información acerca de los tipos de datos, vea [Integration Services Data Types](../../integration-services/data-flow/integration-services-data-types.md).  
  
10. Haga clic en **OK**.  

## <a name="data-flow-taps"></a>Derivaciones de flujo de datos
 Puede agregar una derivación de datos en una ruta de flujo de datos de un paquete en tiempo de ejecución y dirigir el resultado de la derivación de datos a un archivo externo. Para usar esta característica, debe implementar el proyecto de SSIS con el modelo de implementación de proyectos en un servidor de SSIS. Después de implementar el paquete en el servidor, debe ejecutar scripts T-SQL en la base de datos SSISDB para agregar derivaciones de datos antes de ejecutar el paquete. Este es un escenario de ejemplo:  
  
1.  Cree una instancia de ejecución de un paquete con el procedimiento almacenado [catalog.create_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-create-execution-ssisdb-database.md).  
  
2.  Para agregar una derivación de datos, use el procedimiento almacenado [catalog.add_data_tap](../../integration-services/system-stored-procedures/catalog-add-data-tap.md) o [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md) .  
  
3.  Inicie la instancia de ejecución del paquete con [catalog.start_execution &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-start-execution-ssisdb-database.md).  
  
 Este es un script SQL de ejemplo que realiza los pasos descritos en el escenario anterior:  
  
```sql
Declare @execid bigint  
EXEC [SSISDB].[catalog].[create_execution] @folder_name=N'ETL Folder', @project_name=N'ETL Project', @package_name=N'Package.dtsx', @execution_id=@execid OUTPUT  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt'  
EXEC [SSISDB].[catalog].[start_execution] @execid  
```  
  
 Los parámetros de nombre de carpeta, nombre de proyecto y nombre de paquete del procedimiento almacenado create_execution corresponden a los nombres de carpeta, proyecto y paquete del catálogo de Integration Services. Puede obtener los nombres de carpeta, proyecto y paquete que debe usar en la llamada a create_execution desde SQL Server Management Studio, como se muestra en la ilustración siguiente. Si no ve el proyecto de SSIS aquí, quizás no haya implementado todavía el proyecto en el servidor de SSIS. Haga clic con el botón secundario en el proyecto de SSIS en Visual Studio y, a continuación, haga clic en Implementar para implementar el proyecto en el servidor de SSIS esperado.  
  
 En lugar de escribir instrucciones SQL, puede generar el script de ejecución de paquetes mediante los pasos siguientes:  
  
1.  Haga clic con el botón derecho en **Package.dtsx** y, después, haga clic en **Ejecutar**.  
  
2.  Haga clic en el botón **Script** de la barra de herramientas para generar el script.  
  
3.  Ahora, agregue la instrucción add_data_tap antes de la llamada a start_execution.  
  
 El parámetro task_package_path del procedimiento almacenado add_data_tap corresponde a la propiedad PackagePath de la tarea de flujo de datos en Visual Studio. En Visual Studio, haga clic con el botón derecho en **Tarea Flujo de datos** y, después, haga clic en **Propiedades** para abrir la ventana Propiedades.  Anote el valor de la propiedad **PackagePath** para usarlo como valor para el parámetro task_package_path en la llamada al procedimiento almacenado add_data_tap.  
  
 El parámetro dataflow_path_id_string del procedimiento almacenado add_data_tap corresponde a la propiedad IdentificationString de la ruta de flujo de datos a la que desea agregar una derivación de datos. Para obtener dataflow_path_id_string, haga clic en la ruta de flujo de datos (flecha entre las tareas del flujo de datos) y anote el valor de la propiedad **IdentificationString** de la ventana Propiedades.  
  
 Cuando se ejecuta el script, el archivo de salida se almacena en \<Program Files>\Microsoft SQL Server\110\DTS\DataDumps. Si ya existe un archivo con ese nombre, se crea un archivo nuevo con un sufijo (por ejemplo: output[1].txt).  
  
 Como se ha indicado anteriormente, también puede usar el procedimiento almacenado [catalog.add_data_tap_by_guid](../../integration-services/system-stored-procedures/catalog-add-data-tap-by-guid.md), en lugar de usar el procedimiento almacenado add_data_tap. Este procedimiento almacenado toma como parámetro el identificador de la tarea Flujo de datos en lugar de task_package_path. Puede obtener el identificador de la tarea Flujo de datos de la ventana Propiedades en Visual Studio.  
  
### <a name="removing-a-data-tap"></a>Quitar una derivación de datos  
 Puede quitar una derivación de datos antes de iniciar la ejecución con el procedimiento almacenado [catalog.remove_data_tap](../../integration-services/system-stored-procedures/catalog-remove-data-tap.md) . Este procedimiento almacenado toma como parámetro el identificador de la derivación de datos, que puede obtener como resultado del procedimiento almacenado add_data_tap.  
  
```sql
DECLARE @tap_id bigint  
EXEC [SSISDB].[catalog].add_data_tap @execution_id = @execid, @task_package_path = '\Package\Data Flow Task', @dataflow_path_id_string = 'Paths[Flat File Source.Flat File Source Output]', @data_filename = 'output.txt' @data_tap_id=@tap_id OUTPUT  
EXEC [SSISDB].[catalog].remove_data_tap @tap_id  
```  
  
### <a name="listing-all-data-taps"></a>Mostrar todas las derivaciones de datos  
 También puede enumerar todas las derivaciones de datos mediante la vista catalog.execution_data_taps. En el ejemplo siguiente se extraen derivaciones de datos para una instancia de ejecución de especificación (Id.: 54).  
  
```sql 
select * from [SSISDB].[catalog].execution_data_taps where execution_id=@execid  
```  
  
### <a name="performance-consideration"></a>Consideraciones sobre el rendimiento  
 Al habilitar el nivel de registro detallado y agregar derivaciones de datos aumentan las operaciones de E/S que realiza la solución de integración de datos. Por tanto, se recomienda agregar derivaciones de datos solo para solucionar problemas.  
  
### <a name="video"></a>Vídeo  
 En este [vídeo de TechNet](../../sql-server/index.yml) se muestra cómo agregar y usar derivaciones de datos en el catálogo de SSISDB de SQL Server 2012, que permiten depurar paquetes mediante programación y capturar los resultados parciales en tiempo de ejecución. También explica cómo enumerar o quitar estas derivaciones de datos y las prácticas recomendadas para usar derivaciones de datos en paquetes de SSIS.  
 
## <a name="see-also"></a>Consulte también  
 [Control de errores en los datos](../../integration-services/data-flow/error-handling-in-data.md)  
  
