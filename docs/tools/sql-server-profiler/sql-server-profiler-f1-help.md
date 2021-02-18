---
title: Cuadros de diálogo de SQL Server Profiler
titleSuffix: SQL Server Profiler
description: Explore los comandos y valores disponibles en SQL Server Profiler para usarlos en seguimientos, plantillas, archivos, tablas, contadores de rendimiento y reproducciones.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: reference
ms.assetid: e57b9160-4b78-4353-abb2-bfdbdf523d7a
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 07/07/2017
ms.openlocfilehash: 06711f4975bb84163f58857759c5470f004c52e9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338634"
---
# <a name="sql-server-profiler-dialog-boxes"></a>Cuadros de diálogo de SQL Server Profiler

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] de Microsoft es una herramienta que captura eventos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de un servidor. Los eventos se guardan en un archivo de seguimiento que posteriormente se puede analizar o utilizar para reproducir una serie de pasos específicos cuando se intenta diagnosticar un problema. A continuación se muestran los comandos y la configuración disponibles en los cuadros de diálogo de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].  
## <a name="trace-properties"></a>Propiedades de seguimiento
### <a name="general-tab"></a>Pestaña General
Utilice la pestaña **General** del cuadro de diálogo **Propiedades de seguimiento** para ver o especificar las propiedades de un seguimiento.  

|Elemento|Descripción
|---|---
|**Nombre de seguimiento** |Especifica el nombre del seguimiento.  
|**Nombre del proveedor de seguimiento**|Muestra el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que va a realizarse un seguimiento. Este campo se rellena automáticamente con el nombre del servidor que haya especificado al conectarse. Para cambiar el nombre del proveedor de seguimiento, haga clic en **Cancelar** para cerrar el cuadro de diálogo e iniciar un nuevo seguimiento.  
|**Tipo de proveedor de seguimiento**|Muestra el tipo de proveedor que proporciona el seguimiento. El archivo de definición de seguimiento rellena el campo **Tipo de proveedor de seguimiento** automáticamente. No se puede modificar este campo.  
|**version**|Muestra la versión del servidor que proporciona el seguimiento. El archivo de definición de seguimiento rellena el campo **Versión** automáticamente. No se puede modificar este campo.  
|**Usar la plantilla**|Selecciona una plantilla del directorio de plantillas. El directorio se rellena con las plantillas predeterminadas y con cualquier tipo de plantilla definida por el usuario que se haya creado para el tipo de proveedor de seguimiento actual.  
|**Guardar en el archivo**|Captura los datos del seguimiento en un archivo .trc. Guardar los datos del seguimiento es útil para su posterior revisión y análisis.  
|**Establecer el tamaño máximo de archivo (MB)**|Si elige la opción de guardar los datos de seguimiento en un archivo, deberá especificar el tamaño máximo del mismo. El valor predeterminado es 5 megabytes (MB). El tamaño máximo está limitado únicamente por el sistema de archivos (NTFS, FAT) donde se guarde el archivo.  
|**Guardar como**|Después de seleccionar guardar, puede seleccionar este icono para cambiar el nombre de archivo.  
|**Habilitar sustitución incremental de archivos**|Seleccione esta opción para habilitar la creación de archivos adicionales, para que se acepten los datos del seguimiento cuando se alcance el tamaño máximo de archivo. Los nuevos nombres de archivo consisten en el nombre de archivo .trc original, numerado de forma secuencial. Por ejemplo, una vez que **NewTrace.trc** alcanza el tamaño máximo de archivo, se cierra, y se abre un nuevo archivo, **NewTrace_1.trc**, seguido de **NewTrace_2.trc** y así sucesivamente. La sustitución incremental de archivos está habilitada de forma predeterminada cuando se guarda un seguimiento en un archivo.  
|**El servidor procesa los datos de seguimiento**|Especifica que el servidor que ejecuta el seguimiento debe procesar los datos de seguimiento. El uso de esta opción reduce la sobrecarga de rendimiento en la que se incurre con el trazado. Si se selecciona esta opción, no se omite ningún evento, incluso en condiciones de gran demanda. Si esta casilla está desactivada, el Analizar de SQL Server se encarga del procesamiento y existe la posibilidad de que no se realice un seguimiento de algunos eventos de gran demanda.  
|**Guardar en la tabla**|Captura los datos del seguimiento en una tabla de base de datos. Guardar los datos del seguimiento es útil para su posterior revisión y análisis. Sin embargo, guardar los datos de un seguimiento en una tabla también puede aumentar de forma significativa la sobrecarga en el servidor en el que se guarda el seguimiento. Si es posible, no guarde la tabla de seguimiento en el mismo servidor del que se realiza un seguimiento.  
|**Tabla de destino**|Después de seleccionar que los datos de seguimiento se guarden en una tabla de base de datos, puede seleccionar este icono para cambiar el nombre de la tabla.  
| **Establecer número máximo de filas (en miles)**|Especifica el número máximo de filas en las que guardar los datos. El valor predeterminado es 1000 filas. 
|**Habilitar hora de detención de seguimiento**|Establece la fecha y la hora de finalización y de cierre del seguimiento. 

### <a name="events-selection-tab"></a>Pestaña Selección de eventos
Utilice la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades de seguimiento** para ver o especificar columnas de datos y eventos de seguimiento.  

|Elemento|Descripción
|---|---
|Columna **Eventos**|Especifique los eventos de seguimiento seleccionando o desactivando la casilla de la columna de eventos. Los **eventos** se organizan por categoría. Las clases de evento especificadas en la plantilla se seleccionan automáticamente. Para obtener más información, consulte [Referencia de las clase de eventos de SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Columnas de datos|Especifique las columnas de datos de seguimiento activando la casilla que se corresponda con el evento y la columna de datos necesarios. Todas las columnas de eventos importantes se activan de manera predeterminada en cada evento incluido en el seguimiento.  
|Filtros|Especifique los filtros haciendo clic en el encabezado de la columna de datos y especificando los criterios de filtro. Las columnas de datos filtradas se indican mediante un icono de filtro situado a la izquierda de la etiqueta de columna en el cuadro de diálogo **Editar filtro** . Para obtener más información, vea [SQL Server Profiler (Editar filtro)]().  
|**Mostrar todos los eventos**|Se muestran todos los eventos disponibles. De forma predeterminada, solo se muestran las filas de la cuadrícula **Selección de eventos** que están seleccionadas. Desactive esta casilla para ocultar todos los eventos que no estén seleccionados en la cuadrícula **Selección de eventos** .  
|**Mostrar todas las columnas**|Muestra todas las columnas de datos disponibles. De forma predeterminada, solo se muestran las columnas de datos seleccionadas. Desactive esta casilla para ocultar todas las columnas de datos que no estén seleccionadas en la cuadrícula **Selección de eventos** .  
|**Filtros de columnas**|Inicia el cuadro de diálogo **Editar filtro** . Puede utilizar este cuadro de diálogo para editar filtros de columnas de datos.  
|**Organizar columnas**|Cambia el orden de las columnas del seguimiento y agrupa los resultados en una o más columnas.  

## <a name="trace-template-properties"></a>Propiedades de la plantilla de seguimiento 
### <a name="new-general-tab"></a>Nuevo (pestaña General)
Utilice la pestaña **General** del cuadro de diálogo **Propiedades de la plantilla de seguimiento** para crear nuevas plantillas de seguimiento mediante las siguientes opciones. Para acceder a este cuadro de diálogo, en el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], coloque el cursor sobre **Plantillas** y haga clic en **Nuevo**.

|Elemento|Descripción
|---|---
|**Seleccionar tipo de servidor**|Especifique el tipo de servidor donde se utilizará esta plantilla.  
|**Nuevo nombre de plantilla**|Proporcione un nombre descriptivo para la plantilla.  
|**Basar plantilla nueva en una existente**|Utilice una plantilla de la lista como base para esta plantilla. Todos los eventos, columnas de datos y filtros seleccionados coinciden inicialmente con los de la plantilla existente y se pueden modificar según sea necesario.  
|**Usar como plantilla predeterminada para tipo de servidor seleccionado**|Utilice esta plantilla como valor predeterminado en seguimientos creados para este tipo de servidor.  

### <a name="edit-general-tab"></a>Editar (pestaña General)
 Utilice la pestaña **General** del cuadro de diálogo **Propiedades de la plantilla de seguimiento** para ver o editar las plantillas de seguimiento existentes utilizando las opciones que se muestran a continuación. Para obtener acceso a este cuadro de diálogo, en el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], seleccione **Plantillas** y, a continuación, haga clic en **Editar plantilla**.  

|Elemento|Descripción
|---|---
|**Seleccionar tipo de servidor**|Especifique el tipo de servidor donde se utilizará esta plantilla.  
|**Seleccionar nombre de plantilla**|Seleccione la plantilla que desea editar.  
|**Usar como plantilla predeterminada para tipo de servidor seleccionado**|Utilice esta plantilla como valor predeterminado en seguimientos creados para este tipo de servidor.  

### <a name="events-selection-tab"></a>Pestaña Selección de eventos
Utilice la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades de la plantilla de seguimiento** para ver, editar o especificar las clases de eventos y las columnas de datos que se van a incluir en una plantilla de seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

|Elemento|Descripción
|---|---
|Columna **Eventos**|Active o desactive la casilla de la columna de eventos para especificar los eventos de los que debe realizarse un seguimiento. Los eventos se organizan por categoría. Si ha seleccionado **Basar plantilla nueva en una existente** en la pestaña **General** , los eventos se seleccionan automáticamente de acuerdo con la plantilla especificada. Para obtener más información sobre las clases de eventos, vea [Referencia de las clase de eventos de SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Columnas de datos|Especifique las columnas de datos de las que debe realizarse un seguimiento activando el cuadro correspondiente al evento y la columna de datos que necesite. Si se activa la casilla correspondiente al evento, todas las columnas de eventos pertinentes se activan de forma predeterminada para cada evento incluido en el seguimiento. Si ha seleccionado **Basar plantilla nueva en una existente** en la pestaña **General** , las columnas de datos y los filtros se seleccionan automáticamente de acuerdo con la plantilla especificada.  
|Filtros|Especifique los filtros haciendo clic en el encabezado de la columna de datos y especificando los criterios de filtro. Las columnas de datos filtradas se indican mediante un icono de filtro situado a la izquierda de la etiqueta de columna en el cuadro de diálogo **Editar filtro** .  
|**Mostrar todos los eventos**|Se muestran todos los eventos disponibles. Esta opción está activada de forma predeterminada si crea una nueva plantilla que no se base en una plantilla existente. Desactive esta opción para ocultar todos los eventos no seleccionados en la cuadrícula **Selección de eventos** .  
|**Mostrar todas las columnas**|Muestra todas las columnas de datos disponibles. Esta opción está activada de forma predeterminada si crea una nueva plantilla que no se base en una plantilla existente. Desactive esta opción para ocultar todas las columnas de datos no seleccionadas en la cuadrícula **Selección de eventos** .  
|**Filtros de columnas**|Inicia el cuadro de diálogo **Editar filtro**, que muestra un icono de filtro a la izquierda de la etiqueta de columna de datos. Utilice el cuadro de diálogo **Editar filtro** para editar los filtros de las columnas de datos.  
|**Organizar columnas**|Cambia el orden de las columnas del seguimiento y agrupa los resultados en una o más columnas. 

## <a name="trace-file-properties"></a>Propiedades del archivo de seguimiento 
### <a name="general-tab"></a>Pestaña General
Utilice la pestaña **General** del cuadro de diálogo **Propiedades del archivo de seguimiento** para ver las propiedades de un archivo de seguimiento.  
Para ver esta ventana, abra un archivo de seguimiento. A continuación, en el menú **Archivo** , haga clic en **Propiedades**.  

|Elemento|Descripción
|---|---
|**Nombre de archivo**|Muestra la ruta de acceso y el nombre del archivo de seguimiento.  
|**Nombre del proveedor de seguimiento**|Muestra el nombre de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se realizó un seguimiento.  
|**Tipo de proveedor de seguimiento**|Muestra el tipo de proveedor que ha proporcionado el seguimiento.  
|**version**|Muestra la versión del proveedor que ha proporcionado el seguimiento.  
|**Tamaño de archivo (KB)**|Tamaño del archivo de seguimiento en kilobytes (KB).  
|**Creado**|Fecha y hora en que se creó el archivo de seguimiento.  
|**Modificado** |Fecha y hora en que se modificó el archivo de seguimiento.  

### <a name="events-selection-tab"></a>Pestaña Selección de eventos
Utilice la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades del archivo de seguimiento** para ver las propiedades de columna del seguimiento o quitar columnas de datos del seguimiento.  
Para ver esta ventana, abra un archivo de seguimiento. Después, en el menú **Archivo** , haga clic en **Propiedades** y, a continuación, haga clic en la pestaña **Selección de eventos** .  

|Elemento|Descripción
|---|---
|Columna **Eventos**|Muestra los eventos de los que se hace el seguimiento y que están organizados por categoría de eventos. Inicialmente, se seleccionan todos los eventos del seguimiento. Para seleccionar un evento, debe activarse la casilla o la columna de datos de dicho evento. Si la casilla del evento está activada, se seleccionan todas las columnas de datos disponibles para dicho evento. Si la columna de datos de un evento está activada, se activa el evento, y cualquier otra columna requerida también se activa automáticamente. Si se está viendo un archivo o una tabla de seguimiento, al desactivar las casillas de las columnas de datos o eventos, se reduce la cantidad de datos visibles en la ventana de seguimiento para simplificar el análisis. Se pueden cambiar los filtros de las columnas para reducir la cantidad de datos visibles en la ventana de seguimiento. Para obtener más información sobre las clases de eventos, vea [Referencia de las clase de eventos de SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Columnas de datos|Muestra las columnas de datos de las que se realiza un seguimiento. Todas las columnas de datos relevantes del seguimiento están activadas de forma predeterminada para cada uno de los eventos incluidos en el seguimiento.  
|Filtros|Especifique los filtros haciendo clic en el encabezado de la columna de datos y especificando los criterios de filtro. Las columnas de datos filtradas se indican mediante un icono de filtro situado a la izquierda de la etiqueta de columna en el cuadro de diálogo **Editar filtro** .  
|**Mostrar todos los eventos**|Se muestran todos los eventos disponibles. De forma predeterminada, solo se muestran las filas de la cuadrícula **Selección de eventos** que están seleccionadas. Desactive esta casilla para ocultar todos los eventos que no estén seleccionados en la cuadrícula **Selección de eventos** . Si se activa **Mostrar todos los eventos** y se está viendo un archivo o una tabla de seguimiento, todos los eventos que se grabaron en el seguimiento se mostrarán en la ventana de seguimiento.  
|**Mostrar todas las columnas**|Muestra todas las columnas de datos disponibles. De forma predeterminada, solo se muestran las columnas de datos seleccionadas. Desactive esta casilla para ocultar todas las columnas de datos que no estén seleccionadas en la cuadrícula **Selección de eventos** .  
|**Filtros de columnas**|Inicia el cuadro de diálogo **Editar filtro** , que muestra un icono de filtro a la izquierda de la etiqueta de columna para las columnas de datos filtradas. Utilice el cuadro de diálogo **Editar filtro** para editar los filtros de las columnas de datos.  
|**Organizar columnas**|Después de seleccionar la columna **Eventos** y la columna de datos de las que se va a realizar un seguimiento, haga clic en **Organizar columnas** para que la cuadrícula reordene la columna en la ventana de resultados del seguimiento.  

## <a name="trace-table-properties"></a>Propiedades de la tabla de seguimiento
### <a name="events-selection-tab"></a>Pestaña Selección de eventos
Utilice la pestaña **Selección de eventos** del cuadro de diálogo **Propiedades de la tabla de seguimiento** para ver las propiedades de columna de datos y eventos del seguimiento o para quitar eventos o columnas del seguimiento.  
Para ver esta ventana, use [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para abrir una tabla de seguimiento. Después, en el menú **Archivo** , haga clic en **Propiedades** y, a continuación, haga clic en la pestaña **Selección de eventos** .  

|Elemento|Descripción
|---|---
|Columna **Eventos**|Muestra los eventos de los que se hace el seguimiento y que están organizados por categoría de eventos. Para seleccionar un evento, debe activarse la casilla o la columna de datos de dicho evento. Si la casilla del evento está activada, se seleccionan todas las columnas de datos disponibles para dicho evento. Si la columna de datos de un evento está activada, se activa el evento, y cualquier otra columna requerida también se activa automáticamente. Si se está viendo un archivo o una tabla de seguimiento, al desactivar las casillas de las columnas de datos o eventos, se reduce la cantidad de datos visibles en la ventana de seguimiento para simplificar el análisis. Se pueden cambiar los filtros de las columnas para reducir la cantidad de datos visibles en la ventana de seguimiento. Para obtener más información sobre las clases de eventos, vea [Referencia de las clase de eventos de SQL Server](../../relational-databases/event-classes/sql-server-event-class-reference.md).  
|Otras columnas de datos|Muestra las columnas de datos de las que se realiza un seguimiento. Todas las columnas de datos relevantes del seguimiento están activadas de forma predeterminada para cada uno de los eventos incluidos en el seguimiento.  
|Filtros|Especifique los filtros haciendo clic en el encabezado de la columna de datos y especificando los criterios de filtro. Las columnas de datos filtradas se indican mediante un icono de filtro situado a la izquierda de la etiqueta de columna en el cuadro de diálogo **Editar filtro** .  
|**Mostrar todos los eventos**|Se muestran todos los eventos disponibles. De forma predeterminada, solo se muestran las filas de la cuadrícula **Selección de eventos** que están seleccionadas. Desactive esta casilla para ocultar todos los eventos que no estén seleccionados en la cuadrícula **Selección de eventos** . Si se activa **Mostrar todos los eventos** y se está viendo un archivo o una tabla de seguimiento, todos los eventos que se grabaron en el seguimiento se mostrarán en la ventana de seguimiento.  
|**Mostrar todas las columnas**|Muestra todas las columnas de datos disponibles. De forma predeterminada, solo se muestran las columnas de datos seleccionadas. Desactive esta casilla para ocultar todas las columnas de datos que no estén seleccionadas en la cuadrícula **Selección de eventos** .  
|**Filtros de columnas**|Inicia el cuadro de diálogo **Editar filtro**, en el que se muestra un icono de filtro a la izquierda de la etiqueta de la columna. Puede utilizar este cuadro de diálogo para editar los filtros de las columnas de datos.  
|**Organizar columnas** |Después de seleccionar la columna **Eventos** y la columna de datos de las que se va a realizar un seguimiento, haga clic en **Organizar columnas** para que la cuadrícula reordene la columna en la ventana de resultados del seguimiento.  

## <a name="performance-counters-limit"></a>Límite de contadores de rendimiento
Utilice el cuadro de diálogo Límite de contadores de rendimiento para limitar la información de un archivo de registro de rendimiento del Monitor de sistema cuando lo correlacione con un seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] . Puede utilizar este cuadro de diálogo para seleccionar los contadores que deben mostrarse y utilizarse para la correlación.  
El cuadro de diálogo **Límite de contadores de rendimiento** se llena con los objetos y contadores de rendimiento que contiene el archivo de registro de rendimiento.  
### <a name="to-select-performance-objects-and-counters-to-correlate-with-a-trace"></a>Para seleccionar los objetos y contadores de rendimiento que deben correlacionarse con un seguimiento  
1.  Expanda un objeto de rendimiento para ver los contadores que se incluyen en el archivo de registro de rendimiento.  
2.  Seleccione los contadores que desea correlacionar con el archivo de seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] .  

Si desea seleccionar todos los contadores para un objeto de rendimiento, active la casilla que se encuentra junto a dicho objeto. Si selecciona el nodo superior, que indica el equipo, selecciona todos los objetos y contadores de rendimiento que contiene el archivo de registro de rendimiento. 
## <a name="toolsoptions-general-options-page"></a>Herramientas/opciones (página Opciones generales)
Utilice el cuadro de diálogo **Opciones generales** para ver o especificar las siguientes opciones.  
### <a name="display-options"></a>Opciones de presentación  

|Elemento|Descripción
|---|---
|**Nombre de fuente**|Muestra el nombre de la fuente utilizada en la cuadrícula de resultados de seguimiento durante los seguimientos.  
|**Tamaño de fuente**|Muestra el tamaño de la fuente utilizada en la cuadrícula de resultados de seguimiento durante los seguimientos.  
|**Elegir fuente**|Abre un cuadro de diálogo para cambiar la configuración de fuente.  
|**Usar la configuración regional para mostrar valores de fecha y hora**|Muestra los valores de fecha y hora en la configuración regional establecida en el equipo. Si no selecciona esta opción, los valores de fecha y hora se muestran en el formato fijo que utiliza Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], que incluye milisegundos. Observe que si activa o desactiva esta casilla se cambia el formato de presentación de las columnas de hora como **Hora de inicio** y **Hora de finalización**. Pero no cambia los parámetros del valor **DateTime** dentro de los eventos de lenguaje o las llamadas a procedimientos remotos (RPC).  
|**Mostrar valores en la columna Duración en microsegundos**|Muestra los valores en microsegundos en la columna de datos **Duración** de los seguimientos. De manera predeterminada, la columna **Duración** muestra los valores en milisegundos.  

### <a name="tracing-options"></a>Opciones de seguimiento  

|Elemento|Descripción
|---|---
|**Iniciar la traza inmediatamente tras realizar la conexión**|Inicia un seguimiento con la plantilla predeterminada en cuanto se establece una conexión.  
|**Actualizar definición de seguimiento cuando cambie la versión del proveedor**|Aplica la definición de seguimiento más actual a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] cuando se actualiza el proveedor. Esta opción no está activada de manera predeterminada. Esto obliga a que el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] consulte al servidor la definición de seguimiento y vuelva a crear, si existe, el archivo en el disco.  

### <a name="file-rollover-options"></a>Opciones de sustitución incremental de archivos  

|Elemento|Descripción
|---|---
|**Cargar todos los archivos de sustitución incremental en secuencia sin preguntar**|Carga automáticamente los archivos de sustitución incremental cuando se abre un archivo de seguimiento. Si se ha creado más de un archivo durante la traza, la selección de esta opción carga automáticamente todos los archivos de sustitución incremental.  
|**Preguntar antes de cargar archivos de sustitución incremental**|Cuando se abre un archivo de seguimiento, el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] pregunta antes de agregar un archivo de sustitución incremental.  
|**No cargar nunca los archivos siguientes de sustitución incremental**|[!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] no carga archivos de sustitución incremental.  

### <a name="replay-options"></a>Opciones de reproducción  

|Elemento|Descripción
|---|---
|**Número predeterminado de subprocesos de reproducción**|Especifique el número de subprocesos de reproducción que se utilizarán simultáneamente. Un número más alto consume más recursos durante la reproducción, pero aumenta la simultaneidad.  
|**Intervalo de espera del monitor de estado predeterminado (seg.)**|Especifique el intervalo de espera de la reproducción en segundos. El valor predeterminado es 3600 segundos (1 hora). Esta configuración afecta al tiempo que puede ejecutarse un subproceso antes de que lo finalice el monitor de estado.  
|**Intervalo de sondeo del monitor de estado predeterminado (seg.)**|Especifique el intervalo de sondeo del monitor de estado durante la reproducción en segundos. El valor predeterminado es 60 segundos. Este valor permite al usuario configurar la frecuencia con que el monitor de estado sondea los candidatos para terminar.

## <a name="source-table-database-engine-tuning-advisor-select-workload-table"></a>Tabla de origen (Asistente para la optimización de motor de base de datos, Seleccionar tabla de carga de trabajo)
Microsoft SQL Server Profiler y el Asistente para la optimización utilizan este cuadro de diálogo para seleccionar tablas.  
- En Profiler, use el cuadro de diálogo **Tabla de origen** a fin de especificar una tabla de origen para una tabla de seguimiento. Esta última es una tabla desde la que se carga un seguimiento y su contenido se ve y usa para reproducir el seguimiento.  
- En el Asistente para la optimización, use el cuadro de diálogo **Seleccionar tabla de carga de trabajo** para seleccionar una tabla de base de datos que contenga información de seguimiento del generador de perfiles para usarse como carga de trabajo de optimización, o bien para obtener una vista previa del contenido de la tabla antes de iniciar el análisis de optimización.  

|Elemento|Descripción
|---|---
|**SQL Server**|Especifica la instancia de SQL Server conectada actualmente. Este campo se llena automáticamente y no puede actualizarse.  
|**Base de datos**|Especifica la base de datos en la que se ubica la tabla de seguimiento.  
|**Propietario**|Especifica el propietario de la tabla de seguimiento. Este campo se llena automáticamente como **dbo**.  
|**Table**|Especifica el nombre de la tabla de seguimiento desde la que debe leerse el seguimiento.  

## <a name="destination-table"></a>Tabla de destino
Utilice el cuadro de diálogo **Tabla de destino** para especificar una tabla donde almacenar el seguimiento.  

|Elemento|Descripción
|---|---
|**SQL Server**|Especifica la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] conectada actualmente. Este campo se llena automáticamente y no puede actualizarse. Para cambiar el servidor, haga clic en **Cancelar** y conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde desea almacenar la tabla de seguimiento.  
|**Base de datos**|Especifique la base de datos donde desea almacenar la tabla de seguimiento.  
|**Propietario**|Especifica el propietario de la tabla de seguimiento. Este campo se llena automáticamente como **dbo**.  
|**Table**|Especifique el nombre de la tabla donde desea almacenar el seguimiento.  

## <a name="replay-configuration"></a>Configuración de reproducción
### <a name="basic-replay-options"></a>Opciones básicas de reproducción
En el cuadro de diálogo **Configuración de reproducción** , utilice la página **Opciones básicas de reproducción** para especificar cómo reproducir una tabla o un archivo de seguimiento.  
Para ver esta ventana, utilice el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para abrir un seguimiento o un archivo de seguimiento que contenga los eventos adecuados para la reproducción. Para más información, consulte [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md). Cuando la tabla o el archivo de seguimiento están abiertos, en el menú **Reproducir** , haga clic en **Iniciar** y, a continuación, establezca la conexión con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] donde desea reproducir el seguimiento.  

|Elemento|Descripción
|---|---
|**Servidor de reproducción**|Muestra la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con la que se establece la conexión para la reproducción.  
|**Cambiar...**|Inicia el cuadro de diálogo **Conectar al servidor** para conectarse a otro servidor.  
|**Guardar en el archivo** |Guarda los resultados de la reproducción en un archivo. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el cuadro de diálogo de archivo estándar, en el puede especificar la ubicación en la que se guarda el archivo.  
|**Guardar en la tabla**|Guarda los resultados de la reproducción en una tabla. [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] muestra el cuadro de diálogo de selección de tabla, en el puede especificar la ubicación en la que se guarda la tabla.  
|**Número de subprocesos de reproducción**|Especifique el número de subprocesos de reproducción que se utilizarán simultáneamente. Un número mayor consume más recursos durante la reproducción, pero ésta es más rápida y simultánea.  
|**Reproducir eventos en el orden del seguimiento**|Reproduce los eventos de forma secuencial. Utilice esta opción si reproduce un seguimiento para depuración.  
|**Reproducir eventos mediante múltiples subprocesos** |Reproduce los eventos de forma simultánea. Esta opción es más rápida que la reproducción secuencial, pero deshabilita la depuración. Los eventos se ordenan dentro de sus identificadores de proceso del sistema (SPID).  
|**Mostrar los resultados de la reproducción**|Muestra el resultado de la reproducción en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. 

### <a name="advanced-replay-options"></a>Opciones avanzadas de reproducción
En el cuadro de diálogo **Configuración de reproducción** , utilice la pestaña **Opciones avanzadas de reproducción** para especificar cómo reproducir un archivo de seguimiento.  
Para ver esta ventana, utilice el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] para abrir un seguimiento o un archivo de seguimiento que contenga los eventos adecuados para la reproducción. Para más información, consulte [Replay Requirements](../../tools/sql-server-profiler/replay-requirements.md). Cuando el archivo o tabla de seguimiento esté abierto, en el menú **Reproducir** , haga clic en **Iniciar**, establezca la conexión con la sesión de SQL Server donde desea reproducir el seguimiento y haga clic en la pestaña **Opciones avanzadas de reproducción** .  

|Elemento|Descripción
|---|---
|**Reproducir los SPID del sistema**|Especifica si el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] reproduce identificadores de proceso del sistema (SPID).  
|**Reproducir solo un SPID**|Solo reproduce la actividad del archivo de seguimiento de origen relacionada con el SPID seleccionado.  
|**SPID para reproducir**|Especifique el SPID que desea reproducir.  
|**Limitar reproducción por fecha y hora**|Active esta opción para reproducir solo una parte del archivo de seguimiento de origen.  
|**Hora de inicio**|Fecha y hora en que debe comenzar la reproducción del archivo de seguimiento de origen.  
|**Hora de finalización**|Fecha y hora en que debe detenerse la reproducción del archivo de seguimiento de origen.  
|**Intervalo de espera del monitor de estado (seg.)**|Especifique el intervalo de espera de la reproducción en segundos. El valor predeterminado es 3600 segundos (1 hora). Esta configuración afecta al tiempo que puede ejecutarse un proceso antes de que lo finalice el monitor de estado.  
|**Intervalo de sondeo del monitor de estado (seg.)**|Especifique el intervalo de sondeo del monitor de estado durante la reproducción en segundos. El valor predeterminado es 60 segundos. Este valor permite al usuario configurar la frecuencia con que el monitor de estado sondea los candidatos para terminar.  
|**Habilitar monitor de procesos bloqueados de SQL Server**|Habilita un proceso que busca procesos bloqueados o de bloqueo.  
|**Intervalo de espera del monitor de procesos bloqueados (seg.)**|Configura la frecuencia con que el monitor de procesos bloqueados busca procesos bloqueados o de bloqueo.  

## <a name="find-dialog-box"></a>Buscar, cuadro de diálogo
Utilice el cuadro de diálogo **Buscar** para buscar un seguimiento para palabras o caracteres específicos. Para cancelar la búsqueda en curso, presione ESC.  
 Para abrir este cuadro de diálogo en el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], en el menú **Editar** , haga clic en **Buscar**.  

|Elemento|Descripción
|---|---
|**Buscar**|Escriba el texto que desea buscar. La búsqueda mostrará cualquier cadena que contenga la cadena especificada. Por ejemplo, la búsqueda de "Completed" devolverá "SQL:BatchCompleted". No se admiten caracteres comodín (*, ?, etc.).  
|**Buscar en columna**|Haga clic en una columna de datos para buscar, o bien haga clic en **\<All columns>** para buscar en todas las columnas de datos del seguimiento.  
|**Coincidir mayúsculas y minúsculas**|Busca texto en el que se usen las mismas mayúsculas y minúsculas que en el cuadro **Buscar** . Desactive esta casilla para buscar ejemplos del seguimiento que tengan caracteres tanto en mayúscula como en minúscula.  
|**Solo palabras completas**|Limita la búsqueda a palabras completas. Desactive la casilla **Solo palabras completas** si desea realizar la búsqueda de algunos caracteres de una palabra.  
|**Buscar siguiente**|Busca el ejemplo siguiente de los caracteres del cuadro **Buscar** .  
|**Buscar anterior**|Realiza una búsqueda hacia atrás en el seguimiento, para buscar el ejemplo anterior de los caracteres del cuadro **Buscar** .  

 ## <a name="organize-columns"></a>Organizar columnas
Utilice el cuadro de diálogo **Organizar columnas** para seleccionar columnas de datos y agrupar o agregar eventos que se muestran en un seguimiento; esto facilita la lectura y el análisis de los archivos o tablas de seguimiento grandes.  
- La agregación mueve y contrae todos los eventos del seguimiento bajo su tipo de clase de evento correspondiente. A la izquierda del nombre de clase de eventos aparece un signo más ( **+** ). Si hace clic en el signo más, expande la clase de evento y puede ver todos sus eventos.  
- La agrupación organiza todas las clases de evento de un tipo específico en una ventana de seguimiento. Sin embargo, los eventos no se contraen bajo el tipo de clase de evento.  

Cuando agrupa o agrega eventos en una ventana de seguimiento, las columnas seleccionadas para agrupar o agregar permanecen fijas en la ventana, pero puede desplazarse a la derecha o la izquierda para ver las demás columnas de datos.  
Para tener acceso a este cuadro de diálogo, abra un archivo o una tabla de seguimiento existente y haga clic en **Propiedades** en el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]. En el cuadro de diálogo **Propiedades de seguimiento** , haga clic en la pestaña **Selección de eventos** y, a continuación, haga clic en **Organizar columnas**. También puede hacer clic en **Organizar columnas** en la pestaña **Selección de eventos** cuando crea un nuevo seguimiento.  
Desplaza los nombres de columnas de datos bajo **Grupos** para agrupar o agregar clases de eventos en la ventana de seguimiento.
- Para agregar eventos, desplace una columna de datos a **Grupos**. De este modo, todos los eventos de un tipo específico se contraen bajo el nombre del tipo de clase de evento en la ventana de seguimiento. A la izquierda del nombre de clase de eventos aparece un signo más ( **+** ). Haga clic en el signo más para expandir el tipo de clase de evento y ver todos sus eventos. Puede activar y desactivar la agregación y la agrupación si hace clic en **Vista agregada** o **Vista agrupada** en el menú **Ver** .
- Para agrupar eventos, desplace varias columnas de datos a **Grupos**. De este modo, todos los eventos de un tipo específico se agrupan en la ventana de seguimiento, pero no se contraen bajo el nombre del tipo de clase de evento. Puede cambiar entre una vista agrupada y una vista no agrupada si hace clic en **Vista agrupada** en el menú Ver. Cuando se desplaza más de una columna de datos a **Grupos**, la opción para cambiar a **Vista agregada** deja de estar disponible.

|Elemento|Descripción
|---|---
|**Columnas**|Muestra las columnas de datos que se pueden desplazar a **Grupos**. Haga clic en el signo más ( **+** ) situado a la izquierda de **Columnas** para expandir la lista.  
|**Up** (Arriba)|Después de seleccionar una columna de datos, haga clic en **Subir** para subir las columnas a **Grupos**. También puede hacer clic en **Subir** para volver a organizar la visualización de las columnas en la ventana de seguimiento.  
|**Bajar**|Después de seleccionar una columna de datos, haga clic en **Bajar** para quitar las columnas de **Grupos**. También puede hacer clic en **Bajar** para volver a organizar la visualización de las columnas en la ventana de seguimiento.  

## <a name="edit-filter"></a>Editar filtro
Utilice el cuadro de diálogo **Editar filtro** para crear y modificar filtros de columna de datos en un seguimiento. Haga clic en un nombre de columna de datos de la lista; en el panel adyacente se muestran los criterios de filtro disponibles para dicha columna de datos. Escriba los criterios de filtro y haga clic en **Aceptar** para aplicarlos a la columna de datos seleccionada. Si aparece un icono de filtro a la izquierda del nombre de la columna de datos en la lista, indica que esta columna ya dispone de un filtro configurado.  
 >[!NOTE]
 >Para las columnas de tipo de datos de cadena, los criterios de filtro aparecerán como un valor de cadena LIKE o NOT LIKE.  

## <a name="select-template-name"></a>Seleccionar nombre de plantilla
Utilice el cuadro de diálogo **Seleccionar nombre de plantilla** para seleccionar una plantilla de seguimiento del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] existente para exportarla a un archivo del sistema operativo. También puede utilizar este cuadro de diálogo para seleccionar o escribir un nombre distinto para guardar una plantilla de seguimiento como cuando se edita una plantilla de seguimiento existente. Para tener acceso a este cuadro de diálogo cuando se exporta una plantilla, en el menú **Archivo** de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], seleccione **Plantillas** y, a continuación, haga clic en **Exportar plantilla**. Para tener acceso a este cuadro de diálogo cuando se cambia el nombre de una plantilla, en el menú **Archivo** , seleccione **Plantillas**, **Editar plantilla** y, a continuación, haga clic en **Guardar como**.  

|Elemento|Descripción
|---|---
|**Tipo de servidor**|Elija el tipo de servidor desde el que desea elegir una plantilla. Esta opción solo se encuentra disponible cuando se exporta una plantilla.  
|**Nombre de plantilla**|Escriba un nombre de plantilla nuevo o seleccione un nombre de plantilla de la lista. Si exporta una plantilla, solo podrá seleccionar un nombre de plantilla de la lista. 

## <a name="see-also"></a>Consulte también 
[SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)   
[Supervisión de la actividad y rendimiento del servidor](../../relational-databases/performance/server-performance-and-activity-monitoring.md)  
  
