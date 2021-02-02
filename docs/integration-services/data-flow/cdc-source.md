---
description: origen de CDC
title: Origen CDC | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.designer.cdcsource.f1
- sql13.ssis.designer.cdcsource.connection.f1
- sql13.ssis.designer.cdcsource.columns.f1
- sql13.ssis.designer.cdcsource.errorhandling.f1
ms.assetid: 99775608-e177-44ed-bb44-aaccb0f4f327
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9c10e3f1aa1460f63f3fcd3e95bb95bef8e63065
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049222"
---
# <a name="cdc-source"></a>origen de CDC

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  El origen CDC lee un intervalo de datos modificados de las tablas de cambios de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] y entrega los cambios de nivel inferior a otros componentes de SSIS.  
  
 El intervalo de lectura de los datos modificados por el origen CDC se denomina intervalo de procesamiento CDC y se determina con la tarea Control CDC que se ejecuta antes de que el flujo de datos actual se inicie. El intervalo de procesamiento CDC se deriva del valor de una variable de paquete que mantiene el estado del procesamiento CDC para un grupo de tablas.  
  
 El origen CDC extrae los datos de una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante una tabla de base de datos, una vista o una instrucción SQL.  
  
 El origen CDC utiliza las configuraciones siguientes:  
  
-   Administrador de conexiones ADO.NET de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para tener acceso a la base de datos CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información sobre cómo configurar la conexión de origen CDC, vea [Editor de origen de CDC &#40;página Administrador de conexiones&#41;]().  
  
-   Tabla habilitada para CDC.  
  
-   Nombre de la instancia de captura de la tabla seleccionada (si existe más de una).  
  
-   Modo del procesamiento de cambios.  
  
-   Nombre de la variable de paquete de estado CDC basado en lo que determina el intervalo de procesamiento CDC. El origen CDC no modifica esa variable.  
  
 Los datos devueltos por el origen CDC son los mismos que los devueltos por las funciones CDC de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **cdc.fn_cdc_get_all_changes_\<capture-instance-name>** o **cdc.fn_cdc_get_net_changes_\<capture-instance-name>** (si está disponible). La única adición opcional es la columna **__$initial_processing** , que indica si el intervalo de procesamiento actual puede solaparse con una carga inicial de la tabla. Para obtener más información acerca del procesamiento inicial, vea [CDC Control Task](../../integration-services/control-flow/cdc-control-task.md).  
  
 El origen de CDC tiene una salida normal y una salida de error.  
  
## <a name="error-handling"></a>Tratamiento de errores  
 El origen de CDC tiene una salida de error. La salida de error del componente incluye las columnas de salida siguientes:  
  
-   **Código de error**: el valor es siempre -1.  
  
-   **Columna de error**: columna de origen que produce el error (para los errores de conversión).  
  
-   **Columnas de fila de error**: datos de registro que ocasionan el error.  
  
 Según la configuración del comportamiento de los errores, el origen CDC permite devolver los errores (conversión de datos, truncamiento) que aparecerán durante el proceso de extracción en la salida de error.  
  
## <a name="data-type-support"></a>Compatibilidad con tipos de datos  
 El componente de origen CDC para Microsoft admite todos los tipos de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , que se asignan a los tipos de datos de SSIS correctos.  
  
## <a name="troubleshooting-the-cdc-source"></a>Solucionar problemas del origen de CDC  
 A continuación se muestra información para solucionar problemas del origen de CDC.  
  
### <a name="use-this-script-to-isolate-problems-and-reproduce-them-in-sql-server-management-studio"></a>Use este script para aislar los problemas y reproducirlos en SQL Server Management Studio  
 La operación de origen de CDC se rige por la operación de la tarea Control CDC ejecutada antes de invocar al origen de CDC. La tarea Control CDC prepara el valor de la variable de paquete de estado CDC para contener los LSN inicial y final. Realiza la función equivalente a la del script siguiente:  
  
```sql
use <cdc-enabled-database-name>  
               declare @start_lsn binary(10), @end_lsn binary(10)  
               set @start_lsn = sys.fn_cdc_increment_lsn(  
                       convert(binary(10),'0x' + '<value-from-state-cs>', 1))  
               set @end_lsn =   
                       convert(binary(10),'0x' + '<value-from-state-ce>', 1)  
               select * from cdc.fn_cdc_get_net_changes_dbo_Table1(@start_lsn,  
@end_lsn, '<mode>')  
```  
  
 donde:  
  
-   \<cdc-enabled-database-name> es el nombre de la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contiene las tablas de cambios.  
  
-   \<value-from-state-cs> es el valor que aparece en la variable de estado CDC como CS/\<value-from-state-cs>/ (CS representa el inicio del intervalo de procesamiento actual [Current-processing-range-Start]).  
  
-   \<value-from-state-ce> es el valor que aparece en la variable de estado CDC como CE/\<value-from-state-cs>/ (CE representa el final del intervalo de procesamiento actual [Current-processing-range-End]).  
  
-   \<mode> son los modos de procesamiento de CDC. Los modos de procesamiento tienen uno de los siguientes valores **Todo**, **Todo con valores antiguos**, **Neto**, **Neto con máscara de actualización**, **Neto con combinación**.  
  
 Este script ayuda a aislar los problemas reproduciéndolos en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], donde resulta sencillo reproducir e identificar los errores.  
  
#### <a name="sql-server-error-message"></a>Mensaje de error de SQL Server  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]podría devolver el mensaje siguiente:  
  
 **Se ha especificado un número insuficiente de argumentos para el procedimiento o función cdc.fn_cdc_get_net_changes_ \<..>.**  
  
 Este error indica que falta un argumento. Indica que los valores de LSN inicial o final de la variable de estado CDC no son válidos.  
  
## <a name="configuring-the-cdc-source"></a>Configurar el origen de CDC  
 Puede configurar el origen de CDC mediante programación o través del Diseñador SSIS.  
  
 Para obtener más información, vea uno de los siguientes temas:  
  
-   [Editor de origen de CDC &#40;página Administrador de conexiones&#41;]()  
  
-   [Editor de origen de CDC &#40;página Columnas&#41;]()  
  
-   [Editor de origen de CDC &#40;página Salida de error&#41;]()  
  
 El cuadro de diálogo **Editor avanzado** contiene las propiedades que se pueden establecer mediante programación.  
  
 Para abrir el cuadro de diálogo **Editor avanzado** :  
  
-   En la pantalla **Flujo de datos** del proyecto de [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] , haga clic con el botón secundario en el origen CDC y seleccione **Mostrar editor avanzado**.  
  
 Para obtener más información acerca de las propiedades que se pueden establecer en el cuadro de diálogo **Editor avanzado** , vea [CDC Source Custom Properties](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="in-this-section"></a>En esta sección  
  
-   [Propiedades personalizadas del origen de CDC](../../integration-services/data-flow/cdc-source-custom-properties.md)  
  
-   [Extraer datos de modificaciones mediante el origen de CDC](../../integration-services/data-flow/extract-change-data-using-the-cdc-source.md)  
  
## <a name="cdc-source-editor-connection-manager-page"></a>Editor de origen de CDC (página Administrador de conexiones)
  Use la página **Administrador de conexiones** del cuadro de diálogo del **Editor de origen de CDC** con el fin de seleccionar el administrador de conexiones de ADO.NET para la base de datos de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] donde el origen de CDC lee las filas de cambios (la base de datos CDC). Una vez que se haya seleccionado la base de datos CDC, debe seleccionar una tabla capturada en la base de datos.  
  
 Para obtener más información acerca del origen de CDC, vea [CDC Source](../../integration-services/data-flow/cdc-source.md).  
  
### <a name="task-list"></a>Lista de tareas  
 **Para abrir la página Administrador de conexiones del Editor de origen de CDC**  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el paquete [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] que tiene el origen de CDC.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el origen de CDC.  
  
3.  En el **Editor de origen de CDC**, haga clic en **Administrador de conexiones**.  
  
### <a name="options"></a>Opciones  
 **Administrador de conexiones de ADO.NET**  
 Seleccione un administrador de conexiones existente de la lista o haga clic en **Nueva** para crear una nueva conexión. Es preciso realizar la conexión a una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] habilitada para CDC y donde se encuentre la tabla de cambios seleccionada.  
  
 **Nuevo**  
 Haga clic en **Nueva**. Se abrirá el cuadro de diálogo **Configurar el administrador de conexiones ADO.NET** , donde puede crear un administrador de conexiones nuevo.  
  
 **Tabla CDC**  
 Seleccione la tabla de origen de CDC que contenga los cambios capturados que desee leer y distribuir para el procesamiento de los componentes SSIS de nivel inferior.  
  
 **Instancia de captura**  
 Seleccione o escriba el nombre de la instancia de captura CDC con la tabla CDC que se va a leer.  
  
 Una tabla de origen capturada puede tener una o dos instancias capturadas para controlar que la transición de una definición de tabla a través de los cambios en el esquema se realice sin problemas. Si se define más de una instancia de captura para la tabla de origen que se va a capturar, seleccione aquí la instancia de captura que desee usar. El nombre predeterminado de la instancia de captura para una tabla [schema].[table] es \<schema>_\<table>, pero los nombres de instancia de captura reales en uso podrían ser distintos. La tabla real desde la que se lee es la tabla de CDC **cdc.\<capture-instance>_CT**.  
  
 **Modo de procesamiento CDC**  
 Seleccione el modo de procesamiento que mejor controle las necesidades de procesamiento. Las opciones posibles son:  
  
-   **Todos**: devuelve los cambios en el intervalo CDC actual sin los valores de **Antes de actualización** .  
  
-   **Todos con valores antiguos**: devuelve los cambios en el intervalo de procesamiento CDC actual, incluidos los valores antiguos (**Antes de actualización**). Para cada operación de actualización habrá dos filas: una con los valores anteriores a la actualización y otra con los valores posteriores a la actualización.  
  
-   **Neto**: devuelve una sola fila de cambios por cada fila de origen modificada en el intervalo de procesamiento de CDC actual. Si una fila de origen se actualizó varias veces, se genera el cambio combinado (por ejemplo, se genera insertar+actualizar como una actualización única y se genera actualizar+eliminar como una eliminación única). Al trabajar en el modo de procesamiento de cambios Neto, es posible dividir los cambios en salidas de eliminar, insertar y actualizar y controlarlos todos en paralelo, ya que la fila de origen única aparece en más de un resultado.  
  
-   **Neto con máscara de actualización**: este modo es similar al modo neto normal, pero también agrega columnas booleanas con el patrón de nombre **__$\<column-name>\__Changed** que indican las columnas modificadas en la fila de cambio actual.  
  
-   **Neto con combinación**: este modo es similar al modo Neto normal, pero con las operaciones de inserción y actualización combinadas en una sola operación de combinación (UPSERT).  
  
> [!NOTE]  
>  Para todas las opciones de cambio Neto, la tabla de origen debe tener una clave principal o un índice único. Para las tablas sin una clave principal o índices únicos, debe usar la opción **Todos** .  
  
 **Variable que contiene el estado CDC**  
 Seleccione la variable de paquete de la cadena de SSIS que mantenga el estado CDC para el contexto CDC actual. Para obtener más información sobre la variable de estado CDC, vea [Definir una variable de estado](../../integration-services/data-flow/define-a-state-variable.md).  
  
 **Incluir una columna de indicador de reprocesamiento**  
 Active esta casilla para crear una columna especial de salida denominada **__$reprocessing**.  
  
 Esta columna tiene un valor **true** cuando el intervalo de procesamiento CDC se superpone con el intervalo de procesamiento inicial (el intervalo de LSN correspondiente al periodo de carga inicial) o cuando un intervalo de procesamiento CDC se vuelve a procesar tras un error en una ejecución anterior. Con esta columna de indicador, el desarrollador de SSIS puede controlar los errores de manera diferente a cuando se vuelven a procesar los cambios (por ejemplo, se pueden omitir acciones como la eliminación de una fila que no existe o una inserción que causó un error en una clave duplicada).  
  
 Para más información, consulte [CDC Source Custom Properties](../../integration-services/data-flow/cdc-source-custom-properties.md).  
  
## <a name="cdc-source-editor-columns-page"></a>Editor de origen de CDC (página Columnas)
  Use la página **Columnas** del cuadro de diálogo **Editor de origen de CDC** para asignar una columna de salida a cada columna externa (origen).  
  
### <a name="task-list"></a>Lista de tareas  
 **Para abrir la página Columnas del Editor de origen de CDC**  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el paquete [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] que tiene el origen de CDC.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el origen de CDC.  
  
3.  En el **Editor de origen de CDC**, haga clic en **Columnas**.  
  
### <a name="options"></a>Opciones  
 **Columnas externas disponibles**  
 Lista de las columnas externas disponibles en el origen de datos. Esta tabla no se puede usar para agregar o quitar columnas. Seleccione las columnas que se van a usar en el origen. Las columnas seleccionadas se agregan a la lista **Columna externa** en el orden en que se seleccionan.  
  
 **Columna externa**  
 Vista de las columnas externas (origen) en el orden en que se verán cuando configure componentes que usen datos del origen de CDC. Para cambiar este orden, en primer lugar borre las columnas seleccionadas en la lista **Columnas externas disponibles** y, a continuación, seleccione las columnas externas en la lista en un orden diferente. Las columnas seleccionadas se agregan a la lista **Columna externa** en el orden en que las haya seleccionado.  
  
 **Columna de salida**  
 Especifique un nombre único para cada columna de salida. El nombre predeterminado es el nombre de la columna externa (origen) seleccionada; sin embargo, puede elegir cualquier nombre único y descriptivo. El nombre especificado se muestra en el Diseñador de SSIS.  
  
## <a name="cdc-source-editor-error-output-page"></a>Editor de origen de CDC (página Salida de error)
  Use la página **Salida de error** del cuadro de diálogo **Editor de origen de CDC** para seleccionar las opciones de control de errores.  
  
### <a name="task-list"></a>Lista de tareas  
 **Para abrir la página Salida de error del Editor de origen de CDC**  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el paquete [!INCLUDE[ssISCurrent](../../includes/ssiscurrent-md.md)] que tiene el origen de CDC.  
  
2.  En la pestaña **Flujo de datos** , haga doble clic en el origen de CDC.  
  
3.  En el **Editor de origen de CDC**, haga clic en **Salida de error**.  
  
### <a name="options"></a>Opciones  
 **Entrada/salida**  
 Muestra el nombre del origen de datos.  
  
 **Columna**  
 Muestra las columnas externas (origen) que se han seleccionado en la página **Administrador de conexiones** del cuadro de diálogo **Editor de origen de CDC** .  
  
 **Error**  
 Seleccione la forma en la que el origen de CDC debe controlar errores en un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.  
  
 **Truncamiento**  
 Seleccione la forma en la que el origen de CDC debe controlar el truncamiento en un flujo: omitir el error, redirigir la fila o hacer que el componente no funcione.  
  
 **Descripción**  
 No se usa.  
  
 **Establecer este valor en las celdas seleccionadas**  
 Seleccione la forma en la que el origen de CDC controla todas las celdas seleccionadas cuando se produce un error o un truncamiento: omitir el error, redirigir la fila o hacer que el componente no funcione.  
  
 **Aplicar**  
 Aplica las opciones de control de errores a las celdas seleccionadas.  
  
### <a name="error-handling-options"></a>Opciones de control de errores  
 Use las opciones siguientes para configurar la forma en la que el origen de CDC controla errores y truncamientos.  
  
 **Error de componente**  
 La tarea Flujo de datos genera un error cuando se produce un error o truncamiento. Este es el comportamiento predeterminado.  
  
 **Omitir error**  
 El error o el truncamiento se omite y la fila de datos se dirige a la salida de origen de CDC.  
  
 **Redirigir fila**  
 La fila de datos de error o truncamiento se dirige a la salida de error del origen de CDC. En este caso, se usa el control de errores de origen de CDC. Para más información, consulte [CDC Source](../../integration-services/data-flow/cdc-source.md).  
  
## <a name="related-content"></a>Contenido relacionado  
  
-   Entrada del blog, sobre [Modos de procesamiento para el origen CDC](https://www.mattmasson.com/2012/01/processing-modes-for-the-cdc-source/), en mattmasson.com.  
  
