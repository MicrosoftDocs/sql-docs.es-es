---
description: Registro de Integration Services (SSIS)
title: Registro de Integration Services (SSIS) | Microsoft Docs
ms.custom: supportability
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.configuredtslogs.containers.f1
- sql13.dts.designer.configuredtslogs.loggingdetails.f1
- sql13.dts.designer.configuredtslogs.providersandlogs.f1
- SQL13.SSIS.SSMS.ISMANAGECUSTOMIZEDLOGGINGLEVEL.F1
helpviewer_keywords:
- containers [Integration Services], logs
- Windows Event log provider [Integration Services]
- XML File log provider [Integration Services]
- SQL Server Profiler, log provider
- SQL Server Integration Services packages, logs
- SSIS packages, logs
- tasks [Integration Services], logs
- logs [Integration Services], log providers
- Integration Services packages, logs
- log providers [Integration Services]
- packages [Integration Services], logs
- Text File log provider
- SQL Server log provider
ms.assetid: 65e17889-371f-4951-9a7e-9932b2d0dcde
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8ac0f913188b23b85548f87a4f458d1c2b754d15
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347457"
---
# <a name="integration-services-ssis-logging"></a>Registro de Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye proveedores de registro que se pueden usar para implementar registros en paquetes, contenedores y tareas. Con los registros, se puede capturar información de tiempo de ejecución sobre un paquete, lo que le ayuda a auditar y solucionar los problemas de un paquete cada vez que se ejecuta. Por ejemplo, un registro puede capturar el nombre del operador que ejecutó el paquete y la hora en que el paquete empezó y terminó.  
  
 Puede configurar el ámbito del registro que se realiza durante la ejecución de un paquete en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Para más información, vea [Habilitar el registro para la ejecución de paquetes en el servidor SSIS](#server_logging).  
  
 También puede incluir el registro al ejecutar un paquete con la utilidad del símbolo del sistema **dtexec**. Para obtener información acerca de los argumentos del símbolo del sistema que admiten registro, vea [dtexec Utility](../../integration-services/packages/dtexec-utility.md).  
  
## <a name="configure-logging-in-sql-server-data-tools"></a>Configurar el registro en SQL Server Data Tools  
 Los registros se asocian a paquetes y se configuran en el nivel de paquete. Cada tarea o contenedor de un paquete puede registrar información en cualquier registro del paquete. Es posible habilitar las tareas y contenedores de un paquete para registro aunque el paquete no lo esté. Por ejemplo, puede habilitar el registro en una tarea Ejecutar SQL sin habilitar el registro en el paquete primario. Un paquete, un contenedor o una tarea pueden escribir en varios registros. Puede habilitar el registro solamente en el paquete, o en cualquier tarea o contenedor individual que incluya el paquete.  
  
 Cuando se agrega el registro a un paquete, se elige el proveedor de registro y la ubicación del registro. El proveedor de registro especifica el formato para los datos del registro: por ejemplo, una base de datos o un archivo de texto de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye los siguientes proveedores de registros:  
  
-   El proveedor de registro de archivo de texto, que escribe entradas de registro en archivos de texto ASCII con un formato de valores separados por comas (CSV). La extensión predeterminada de los nombres de archivo de este proveedor es .log.  
  
-   El proveedor de registro del [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] , que escribe seguimientos que se pueden ver con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Profiler. La extensión predeterminada de los nombres de archivo de este proveedor es .trc.  
  
    > [!NOTE]  
    >  No se puede usar el proveedor de registro de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] en un paquete que se ejecute en modo de 64 bits.  
  
-   El proveedor de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , que escribe entradas de registro en la tabla **sysssislog** en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   El proveedor de registro de eventos de Windows, que escribe entradas en el registro de aplicación del registro de eventos de Windows en el equipo local.  
  
-   El proveedor de registro de archivos XML, que escribe archivos de registro en un archivo XML. La extensión predeterminada de los nombres de archivo de este proveedor es .xml.  
  
 Si agrega un proveedor de registro a un paquete o configura los registros mediante programación, puede usar un ProgID o ClassID para identificar el proveedor de registro, en lugar de usar los nombres que el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] muestra en el cuadro de diálogo **Configurar registros de SSIS** .  
  
 La siguiente tabla enumera los ProgID y ClassID para los proveedores de registro que incluye [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y la ubicación de los registros en los que los proveedores de registro escriben.  
  
|Proveedor de registro|ProgID|ClassID|Location|  
|------------------|------------|-------------|--------------|  
|Archivo de texto|DTS.LogProviderTextFile|{0A039101-ACC1-4E06-943F-279948323883}|El administrador de conexiones de archivos que utiliza el proveedor de registro especifica la ruta de acceso al archivo de texto.|  
|SQL Server Profiler|DTS.LogProviderSQLProfiler|{E93F6300-AE0C-4916-A7BF-A8D0CE12C77A}|El administrador de conexiones de archivos que utiliza el proveedor de registro especifica la ruta de acceso al archivo usado por [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)].|  
|SQL Server|DTS.LogProviderSQLServer|{94150B25-6AEB-4C0D-996D-D37D1C4FDEDA}|El administrador de conexiones OLE DB que usa el proveedor de registro especifica la base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que contiene la tabla sysssislog con las entradas de registro.|  
|Registro de eventos de Windows|DTS.LogProviderEventLog|{071CC8EB-C343-4CFF-8D58-564B92FCA3CF}|El registro de aplicación del Visor de eventos de Windows contiene la información de registro de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] .|  
|Archivo XML|DTS.LogProviderXMLFile|{440945A4-2A22-4F19-B577-EAF5FDDC5F7A}|El administrador de conexiones de archivos que utiliza el proveedor de registro especifica la ruta de acceso al archivo XML.|  
  
 También puede crear proveedores de registro personalizados. Para más información, vea [Creating a Custom Log Provider](../../integration-services/extending-packages-custom-objects/log-provider/creating-a-custom-log-provider.md).  
  
 Los proveedores de registro de un paquete son miembros de la colección de proveedores de registro del paquete. Cuando crea un paquete e implementa el registro mediante el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , puede ver una lista de los miembros de la colección en las carpetas **Proveedor de registro** en la pestaña **Explorador de paquetes** del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Puede configurar un proveedor de registro proporcionando un nombre y descripción para el proveedor de registro y especificando el administrador de conexiones que usa el proveedor de registro. El proveedor de registro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza un administrador de conexiones OLE DB. Todos los proveedores de registro de archivos de texto, de [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)]y de archivos XML utilizan administradores de conexión de archivos. El proveedor de registro de eventos de Windows no usa un administrador de conexiones, dado que escribe directamente en el registro de eventos de Windows. Para obtener más información, consulte [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) y [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
### <a name="logging-customization"></a>Personalización del registro  
 Para personalizar el registro de un evento o mensaje personalizado, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona un esquema de la información registrada habitualmente que se puede incluir en entradas del registro. El esquema de registro [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] define la información que puede registrar. Puede seleccionar elementos del esquema de registro para cada entrada del registro.  
  
 Un paquete y sus contenedores y tareas no tienen que registrar la misma información; además, las tareas y contenedores de un mismo paquete pueden registrar diferente información. Por ejemplo, un paquete puede registrar información del operador al iniciarse, una tarea puede registrar el origen del error de la tarea y otra tarea puede registrar la información de cuándo se producen errores. Si un paquete y sus contenedores y tareas utilizan varios registros, se escribirá la misma información en todos los registros.  
  
 Puede seleccionar un nivel de registro adaptado a sus necesidades, especificando los eventos que desea registrar y la información que desea registrar para cada evento. Algunos eventos pueden proporcionarle más información útil que otros. Por ejemplo, puede registrar únicamente el nombre del equipo y del operador para el evento **PreExecute** , y toda la información disponible para el evento **Error** .  
  
 Para evitar que los archivos de registro utilicen una gran cantidad de espacio en disco o evitar el registro excesivo, que podría reducir el rendimiento, puede limitar el registro, seleccionando los eventos y elementos de información específicos que desee registrar. Por ejemplo, puede configurar un registro para capturar únicamente la fecha y el nombre del equipo para cada error.  
  
 En el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , puede definir las opciones de registro en el cuadro de diálogo **Configurar registros de SSIS** .  
  
#### <a name="log-schema"></a>Esquema de registro  
 En la tabla siguiente se describen los elementos del esquema de registro.  
  
|Elemento|Descripción|  
|-------------|-----------------|  
|Computer|Nombre del equipo en el que ocurrió el evento del registro.|  
|Operator|La identidad del usuario que inició el paquete.|  
|SourceName|Nombre del contenedor o la tarea en los que ocurrió el evento del registro.|  
|SourceID|Identificador único del paquete; el contenedor de bucles For, bucles Foreach o secuencias, o bien la tarea en la que ocurrió el evento del registro.|  
|ExecutionID|GUID de la instancia de ejecución del paquete.<br /><br /> Nota: Al ejecutar un paquete único, se podrían crear entradas de registro con valores diferentes para el elemento ExecutionID. Por ejemplo, al ejecutar un paquete en [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], la fase de validación podría crear entradas de registro con un elemento ExecutionID que corresponda a [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)]. Sin embargo, la fase de ejecución podría crear entradas de registro con un elemento ExecutionID que corresponda a dtshost.exe. Otro ejemplo de esta situación es cuando se ejecuta un paquete que contiene tareas Ejecutar paquete, cada una de las cuales ejecuta un paquete secundario. Estos paquetes secundarios podrían crear entradas de registro que tengan un elemento ExecutionID diferente al de las entradas de registro que crea el paquete principal.|  
|MessageText|Mensaje asociado a la entrada del registro.|  
|DataBytes|Matriz de bytes específica de la entrada del registro. El significado de este campo varía para cada entrada del registro.|  
  
 En la tabla siguiente se describen tres elementos adicionales del esquema del registro que no están disponibles en la pestaña **Detalles** del cuadro de diálogo **Configurar registros de SSIS** .  
  
|Elemento|Descripción|  
|-------------|-----------------|  
|StartTime|Hora en que el contenedor o la tarea empieza a ejecutarse.|  
|EndTime|Hora en que el contenedor o la tarea deja de ejecutarse.|  
|DataCode|Valor entero opcional que suele contener un valor de la enumeración <xref:Microsoft.SqlServer.Dts.Runtime.DTSExecResult> , que indica el resultado de la ejecución del contenedor o tarea:<br /><br /> 0 (correcto)<br /><br /> 1 (error)<br /><br /> 2 (completado)<br /><br /> 3 (cancelado)|  
  
#### <a name="log-entries"></a>Entradas del registro  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] admite entradas del registro en eventos predefinidos y proporciona entradas del registro personalizadas para muchos objetos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . El cuadro de diálogo **Configurar registros de SSIS** del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] enumera estos eventos y entradas del registro personalizadas.  
  
 En la tabla siguiente se describen los eventos predefinidos que se pueden habilitar para escribir entradas del registro cuando se producen eventos en tiempo de ejecución. Estas entradas del registro se aplican a los ejecutables, al paquete y a las tareas y contenedores que incluye el paquete. El nombre de la entrada del registro es el nombre del evento que se produjo en tiempo de ejecución y que originó la entrada del registro.  
  
|Eventos|Descripción|  
|------------|-----------------|  
|**OnError**|Escribe una entrada del registro cuando se produce un error.|  
|**OnExecStatusChanged**|Escribe una entrada del registro cuando una tarea (no un contenedor) se suspende o se reanuda durante la depuración.|  
|**OnInformation**|Escribe una entrada del registro durante la validación y ejecución de un ejecutable, para proporcionar información.|  
|**OnPostExecute**|Escribe una entrada del registro inmediatamente después de que termine de ejecutarse el ejecutable.|  
|**OnPostValidate**|Escribe una entrada del registro cuando finaliza la validación del ejecutable.|  
|**OnPreExecute**|Escribe una entrada del registro inmediatamente antes de que se ejecute el ejecutable.|  
|**OnPreValidate**|Escribe una entrada del registro cuando se inicia la validación del ejecutable.|  
|**OnProgress**|Escribe una entrada del registro cuando el ejecutable realiza un progreso que se puede medir.|  
|**OnQueryCancel**|Escribe una entrada del registro en cualquier momento del procesamiento de la tarea en el que es posible cancelar la ejecución.|  
|**OnTaskFailed**|Escribe una entrada del registro cuando una tarea genera un error.|  
|**OnVariableValueChanged**|Escribe una entrada del registro cuando cambia el valor de una variable.|  
|**OnWarning**|Escribe una entrada del registro cuando se produce una advertencia.|  
|**PipelineComponentTime**|Por cada componente de flujo de datos, escribe una entrada de registro de cada fase de validación y ejecución. La entrada del registro especifica el tiempo de procesamiento para cada fase.|  
|**Diagnostic**<br /><br /> **DiagnosticEx**|Escribe una entrada de registro que proporciona información de diagnóstico.<br /><br /> Por ejemplo, puede registrar un mensaje antes y después de cada llamada a un proveedor de datos externo. Para más información, vea [Herramientas para solucionar problemas con la ejecución de paquetes](../../integration-services/troubleshooting/troubleshooting-tools-for-package-execution.md).<br /><br /> Registre el evento **DiagnosticEx** cuando desee buscar los nombres de columna para las columnas del flujo de datos que tienen errores. Este evento escribe un mapa de linaje de flujo de datos en el registro. Luego, puede buscar el nombre de columna en este mapa de linaje mediante el identificador de columna que captura una salida de error. Para obtener más información, vea [Error Handling in Data](../../integration-services/data-flow/error-handling-in-data.md) (Control de errores en los datos).<br /><br /> Tenga en cuenta que el evento **DiagnosticEx** no conserva el espacio en blanco en la salida XML para reducir el tamaño del registro. Para mejorar la legibilidad, copie el registro en un editor XML (en Visual Studio, por ejemplo) que admita el formato XML y el resaltado de sintaxis.<br /><br /> Nota: Si registra el evento **DiagnosticEx** con el proveedor de registro de SQL Server, la salida puede aparecer truncada. El campo **message** del proveedor de registro de SQL Server es del tipo nvarchar(2048). Para evitar el truncamiento, utilice un proveedor de registro diferente al registrar el evento **DiagnosticEx** .|  
  
 El paquete y muchas tareas poseen entradas del registro personalizadas que se pueden habilitar para el registro. Por ejemplo, la tarea Enviar correo proporciona la entrada de registro personalizada **SendMailTaskBegin** , que registra información cuando se inicia la ejecución de la tarea Enviar correo, pero antes de que la tarea envíe un mensaje de correo. Para más información, vea [Custom Messages for Logging](#custom_messages).  
  
### <a name="differentiating-package-copies"></a>Diferenciar copias de paquetes  
 Los datos del registro incluyen el nombre y GUID del paquete al que pertenecen las entradas del registro. Si crea un nuevo paquete mediante la copia de un paquete existente, también se copian el nombre y GUID del paquete existente. Como resultado, puede tener dos paquetes con el mismo nombre y GUID, lo que dificulta la diferenciación entre los paquetes en los datos del registro.  
  
 Para eliminar esta ambigüedad, deberá actualizar el nombre y GUID de los nuevos paquetes. En [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)], puede volver a generar el GUID en la propiedad **ID** y actualizar el valor de la propiedad **Name** en la ventana Propiedades. También puede cambiar el nombre y GUID mediante programación o con el símbolo del sistema de **dtutil** . Para más información, vea [Establecer las propiedades de paquetes](../../integration-services/set-package-properties.md) y [dtutil (utilidad)](../../integration-services/dtutil-utility.md).  
  
### <a name="parent-logging-options"></a>Opciones del registro primario  
 Con frecuencia, las opciones de registro de tareas y de los contenedores de bucles For y Foreach y de secuencias coinciden con las del paquete o el contenedor primario. En ese caso, puede configurarlas para que hereden las opciones de registro del contenedor primario. Por ejemplo, en un contenedor de bucles For que incluya una tarea Ejecutar SQL, esta tarea puede utilizar las opciones de registro configuradas para el contenedor de bucles For. Para usar las opciones de registro primario, establezca la propiedad LoggingMode del contenedor en **UseParentSetting**. Puede establecer esta propiedad en la ventana **Propiedades** de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] o mediante el cuadro de diálogo **Configurar registros de SSIS** del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
### <a name="logging-templates"></a>Plantillas de registro  
 En el cuadro de diálogo **Configurar registros de SSIS** , también puede crear y guardar como plantillas las configuraciones del registro que utiliza con frecuencia y después, utilizar las plantillas en distintos paquetes. Esto permite aplicar más fácilmente una estrategia de registro coherente entre varios paquetes y modificar las configuraciones del registro en los paquetes actualizando las plantillas y aplicándolas después. Las plantillas se almacenan en archivos XML.  
  
 **Para configurar el registro con el cuadro de diálogo Configurar registros de SSIS**  
  
1.  Habilite el paquete y sus tareas para el registro. El registro se puede producir en el nivel de paquete, de contenedor y de tarea. Puede especificar diferentes registros para paquetes, contenedores y tareas.  
  
2.  Seleccione un proveedor de registro y agregue un registro para el paquete. Solo se pueden crear registros en el nivel de paquete y una tarea o un contenedor deben utilizar uno de los registros creados para el paquete. Cada registro está asociado a uno de los proveedores de registro siguientes: archivo de texto, [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], Registro de eventos de Windows o archivo XML. Para más información, vea [Habilitar el registro de paquetes en SQL Server Data Tools](#ssdt).  
  
3.  Seleccione los eventos y la información del esquema de registro de cada evento que desee capturar en el registro. Para más información, vea [Configurar el registro usando un archivo de configuración guardado](#saved_config).  
  
### <a name="configuration-of-log-provider"></a>Configuración de un proveedor de registro  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Un proveedor de registro se crea y configura como un paso en la implementación de registros en un paquete.  
  
 Después de crear un proveedor de registro, puede ver y modificar sus propiedades en la ventana Propiedades de [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 Para obtener información sobre cómo establecer estas propiedades mediante programación, vea la documentación de la clase <xref:Microsoft.SqlServer.Dts.Runtime.LogProvider> .  
  
### <a name="logging-for-data-flow-tasks"></a>Registrar tareas Flujo de datos  
 La tarea Flujo de datos proporciona varias entradas del registro personalizadas que pueden utilizarse para supervisar y ajustar el rendimiento. Por ejemplo, puede supervisar componentes que puedan causar pérdidas de memoria o realizar un seguimiento del tiempo que lleva ejecutar un componente determinado. Para obtener una lista de las entradas del registro personalizadas y ejemplos de la salida del registro, vea [Data Flow Task](../../integration-services/control-flow/data-flow-task.md).  
  
#### <a name="capture-the-names-of-columns-in-which-errors-occur"></a>Captura de nombres de columnas en que hay errores  
 Al configurar una salida de error en el flujo de datos, de forma predeterminada, la salida de error proporciona solo el identificador numérico de la columna en la que se produjo el error. Para obtener más información, vea [Error Handling in Data](../../integration-services/data-flow/error-handling-in-data.md) (Control de errores en los datos).  
  
 Puede encontrar los nombres de columna habilitando el registro y seleccionando el evento **DiagnosticEx** . Este evento escribe un mapa de linaje de flujo de datos en el registro. A continuación, puede buscar el nombre de columna por su identificador en este mapa de linaje. Tenga en cuenta que el evento **DiagnosticEx** no conserva el espacio en blanco en la salida XML para reducir el tamaño del registro. Para mejorar la legibilidad, copie el registro en un editor XML (en Visual Studio, por ejemplo) que admita el formato XML y el resaltado de sintaxis.  
  
#### <a name="use-the-pipelinecomponenttime-event"></a>Usar el evento PipelineComponentTime  
 Quizás la entrada de registro personalizada más útil es el evento PipelineComponentTime. Esta entrada de registro notifica el número de milisegundos que cada componente del flujo de datos emplea en cada uno de los cinco pasos de procesamiento principales. En la tabla siguiente se describen estos pasos de procesamiento. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] Los desarrolladores reconocerán estos pasos como los métodos principales de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>.  
  
|Paso|Descripción|  
|----------|-----------------|  
|Validación|El componente comprueba los valores de configuración y los valores de propiedad válidos.|  
|PreExecute|El componente realiza el procesamiento único antes de empezar a procesar filas de datos.|  
|PostExecute|El componente realiza el procesamiento único después de haber procesado todas las filas de datos.|  
|ProcessInput|El componente de transformación o de destino procesa las filas de datos entrantes que un origen o una transformación de nivel superior le han pasado.|  
|PrimeOutput|El componente de origen o de transformación llena los búferes con los datos que se van a pasar a un componente transformación o de destino de nivel inferior.|  
  
 Cuando habilita el evento PipelineComponentTime, [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] registra un mensaje para cada paso de procesamiento realizado por cada componente. Las entradas de registro siguientes muestran un subconjunto de los mensajes que el ejemplo de paquete CalculatedColumns de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] registra:  
  
 `The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`  
  
 `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`  
  
 `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`  
  
 `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`  
  
 `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`  
  
 `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`  
  
 `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`  
  
 Estas entradas de registro muestran que la tarea de flujo de datos empleó la mayor parte del tiempo en los pasos siguientes, presentados en orden descendente:  
  
-   El origen de OLE DB, que se denomina "Extract Data", empleó 688 ms en cargar los datos.  
  
-   La transformación de la columna derivada, denominada "Calculate LineItemTotalCost" empleó 356 ms en realizar los cálculos de las filas entrantes.  
  
-   La transformación Agregado denominada “Sum Quantity and LineItemTotalCost” ha usado un tiempo combinado de 220 ms (141 ms en PrimeOutput y 79 ms en ProcessInput) en las operaciones de realizar cálculos y pasar los datos a la transformación siguiente.  

## <a name="enable-package-logging-in-sql-server-data-tools"></a><a name="ssdt"></a> Habilitar el registro de paquetes en SQL Server Data Tools
  Este procedimiento describe cómo agregar registros a un paquete, configurar el registro en el nivel de paquete y guardar la configuración de registro en un archivo XML. Solo puede agregar registros en el nivel de paquete, pero el paquete no tiene que realizar registros para habilitar el registro en los contenedores que incluye.  
  
> [!IMPORTANT]  
>  Si implementa el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en el servidor [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] , el nivel de registro que establezca para la ejecución de paquetes invalidará el registro de paquetes que se configura mediante [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)].  
  
 De manera predeterminada, los contenedores del paquete utilizan la misma configuración de registro que el contenedor principal. Para obtener información sobre la configuración de las opciones de registro para contenedores concretos, vea [Configurar el registro utilizando un archivo de configuración guardado](#saved_config).  
  
### <a name="to-enable-logging-in-a-package"></a>Para habilitar el registro en un paquete  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el menú **SSIS** , haga clic en **Registro**.  
  
3.  Seleccione un proveedor de registro en la lista **Tipo de proveedor** y a continuación, haga clic en **Agregar**.  
  
4.  En la columna **Configuración**, seleccione un administrador de conexiones o haga clic en **\<New connection>** para crear un nuevo administrador de conexiones del tipo apropiado para el proveedor de registro. En función del proveedor seleccionado, utilice uno de los siguientes administradores de conexión:  
  
    -   Para archivos de texto, utilice un administrador de conexiones de archivos. Para más información, vea [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
    -   Para el [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)], utilice un administrador de conexiones de archivos.  
  
    -   Para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], utilice un administrador de conexiones OLE DB. Para más información, consulte [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md).  
  
    -   Para el registro de eventos de Windows no haga nada. [!INCLUDE[ssIS](../../includes/ssis-md.md)] crea automáticamente el registro.  
  
    -   Para archivos XML, utilice un administrador de conexiones de archivos.  
  
5.  Repita los pasos 3 y 4 para cada registro que desee utilizar en el paquete.  
  
    > [!NOTE]  
    >  Un paquete puede utilizar más de un registro de cada tipo.  
  
6.  Opcionalmente, active la casilla de nivel de paquete, seleccione los registros que quiera usar para el registro de nivel de paquete y, después, haga clic en la pestaña **Detalles** .  
  
7.  En la pestaña **Detalles** , seleccione **Eventos** para registrar todas las entradas del registro o bien, borre **Eventos** para seleccionar eventos concretos.  
  
8.  Opcionalmente, haga clic en **Avanzada** para especificar la información que desee registrar.  
  
    > [!NOTE]  
    >  De manera predeterminada, se registra toda la información.  
  
9. En la pestaña **Detalles** , haga clic en **Guardar**. Aparece el cuadro de diálogo **Guardar como** . Localice la carpeta en la que desee guardar la configuración de registro, escriba un nombre de archivo para la nueva configuración de registro y haga clic en **Guardar**.  
  
10. Haga clic en **OK**.  
  
11. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  

## <a name="configure-ssis-logs-dialog-box"></a><a name="configure_logs"></a> Configurar el cuadro de diálogo Registros de SSIS
  Use el cuadro de diálogo **Configurar registros de SSIS** para definir las opciones de registro de un paquete.  
  
 **¿Qué desea hacer?**  
  
1.  [Abrir el cuadro de diálogo Configurar registros de SSIS](#open_dialog)  
  
2.  [Configurar las opciones en el panel Contenedores](#container)  
  
3.  [Configurar las opciones en la pestaña Proveedores y registros](#provider)  
  
4.  [Configurar las opciones en la pestaña Detalles](#detail)  
  
###  <a name="open-the-configure-ssis-logs-dialog-box"></a><a name="open_dialog"></a> Abrir el cuadro de diálogo Configurar registros de SSIS  
 **Para abrir el cuadro de diálogo Configurar registros de SSIS**  
  
-   En el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en **Registro** en el menú **SSIS** .  
  
###  <a name="configure-the-options-in-the-containers-pane"></a><a name="container"></a> Configurar las opciones en el panel Contenedores  
 Utilice el panel **Contenedores** del cuadro de diálogo **Configurar registros de SSIS** para habilitar el paquete y sus contenedores para registro.  
  
#### <a name="options"></a>Opciones  
 **Contenedores**  
 Active las casillas en la vista jerárquica para habilitar el paquete y sus contenedores para registro:  
  
-   Si no se activan, el contenedor no está habilitado para el registro. Seleccione esta opción para habilitar el registro.  
  
-   Si esta casilla aparece atenuada, el contenedor utiliza las opciones de registro de su elemento primario. Esta opción no está disponible para el paquete.  
  
-   Si se activa, el contenedor define sus propias opciones de registro.  
  
 Si un contenedor aparece atenuado y desea establecer opciones de registro en el contenedor, haga clic dos veces en la casilla. El primer clic desactiva la casilla y el segundo la activa para permitirle que elija los proveedores de registro que desea utilizar y para seleccionar la información que desea registrar.  
  
###  <a name="configure-the-options-on-the-providers-and-logs-tab"></a><a name="provider"></a> Configurar las opciones en la pestaña Proveedores y registros  
 Use la pestaña **Proveedores y registros** del cuadro de diálogo **Configurar registros de SSIS** con el fin de crear y configurar registros para la captura de eventos en tiempo de ejecución.  
  
#### <a name="options"></a>Opciones  
 **Tipo de proveedor**  
 Seleccione un tipo de proveedor de registro de la lista.  
  
 **Add (Agregar)**  
 Agregue un registro del tipo especificado a la colección de proveedores de registro del paquete.  
  
 **Nombre**  
 Use las casillas para habilitar o deshabilitar registros para contenedores o tareas que se han seleccionado en el panel **Contenedores** del cuadro de diálogo **Configurar registros de SSIS** . El campo del nombre se puede modificar. Utilice el nombre predeterminado para el proveedor o escriba un nombre descriptivo único.  
  
 **Descripción**  
 El campo de la descripción se puede modificar. Haga clic en la descripción predeterminada del registro y, a continuación, modifíquela.  
  
 **Configuración**  
 Seleccione un administrador de conexiones existente de la lista o haga clic en \<**New connection...**> para crear uno nuevo. En función del tipo de proveedor de registro, puede configurar un administrador de conexiones OLE DB o un administrador de conexiones de archivos. El proveedor de registro para Registro de eventos de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows no requiere conexión.  
  
 Temas relacionados: [OLE DB Connection Manager](../../integration-services/connection-manager/ole-db-connection-manager.md) , [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md)  
  
 **Eliminar**  
 Seleccione un proveedor de registro y haga clic en **Eliminar**.  
  
###  <a name="configure-the-options-on-the-details-tab"></a><a name="detail"></a> Configurar las opciones en la pestaña Detalles  
 Utilice la pestaña **Detalles** del cuadro de diálogo **Configurar registros de SSIS** para especificar los eventos que se van a habilitar para el registro y los detalles de información que se van a registrar. La información que selecciona se aplica a todos los proveedores de registro del paquete. Por ejemplo, no puede escribir cierta información en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] e información diferente en un archivo de texto.  
  
#### <a name="options"></a>Opciones  
 **Eventos**  
 Habilite o deshabilite eventos para el registro.  
  
 **Descripción**  
 Vea una descripción del evento.  
  
 **Avanzadas**  
 Seleccione o borre eventos para el registro, y seleccione o borre información que se va a registrar para cada evento. Haga clic en **Básicas** para ocultar todos los detalles de registro a excepción de la lista de eventos. La información siguiente está disponible para el registro:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Equipo**|Nombre del equipo en el que ha tenido lugar el evento registrado.|  
|**Operador**|El nombre de usuario de la persona que ha iniciado el paquete.|  
|**SourceName**|El nombre del paquete, contenedor o tarea en la que ha tenido lugar el evento registrado.|  
|**SourceID**|El nombre del identificador único global (GUID) del paquete, la tarea o el contenedor en el que ha tenido lugar el evento registrado.|  
|**ExecutionID**|El identificador único global de la instancia de ejecución del paquete.|  
|**MessageText**|Mensaje asociado a la entrada del registro.|  
|**DataBytes**|Reservado para uso futuro.|  
  
 **Basic**  
 Seleccione o borre los eventos de registro. Esta opción oculta los detalles de registro a excepción de la lista de eventos. Si selecciona un evento, se seleccionan todos los detalles de registro para el evento de forma predeterminada. Haga clic en **Avanzadas** para mostrar los detalles de registro.  
  
 **Cargar**  
 Especifique un archivo XML existente para utilizarlo como una plantilla para establecer las opciones de registro.  
  
 **Guardar**  
 Guarde los detalles de configuración como una plantilla para un archivo XML.  

## <a name="configure-logging-by-using-a-saved-configuration-file"></a><a name="saved_config"></a> Configurar el registro usando un archivo de configuración guardado
  Este procedimiento describe cómo configurar el registro para los contenedores nuevos de un paquete, cargando un archivo de configuración de registro previamente guardado.  
  
 De manera predeterminada, todos los contenedores de un paquete utilizan la misma configuración de registro que el contenedor principal. Por ejemplo, las tareas de un bucle ForEach utilizan la misma configuración de registro que el bucle ForEach.  
  
### <a name="to-configure-logging-for-a-container"></a>Para configurar el registro para un contenedor  
  
1.  En [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)], abra el proyecto de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que contiene el paquete que desea.  
  
2.  En el menú **SSIS** , haga clic en **Registro**.  
  
3.  Expanda la vista de árbol del paquete y seleccione el contenedor que desee configurar.  
  
4.  En la pestaña **Proveedores y registros** , seleccione los registros que desee utilizar para el contenedor.  
  
    > [!NOTE]  
    >  Solo puede crear registros en el nivel de paquete. Para más información, vea [Habilitar el registro de paquetes en SQL Server Data Tools](#ssdt).  
  
5.  Haga clic en la pestaña **Detalles** y, a continuación, en **Cargar**.  
  
6.  Localice el archivo de configuración del registro que desee utilizar y haga clic en **Abrir**.  
  
7.  También puede seleccionar una entrada diferente del registro al activar la casilla correspondiente en la columna **Eventos** . Haga clic en **Avanzadas** para seleccionar el tipo de información que se debe registrar para esta entrada.  
  
    > [!NOTE]  
    >  El nuevo contenedor puede incluir entradas de registro adicionales que no estén disponibles para el contenedor utilizado originalmente para crear la configuración del registro. Es necesario seleccionar manualmente estas entradas adicionales para que se registren.  
  
8.  Para guardar la versión actualizada de la configuración de registro, haga clic en **Guardar**.  
  
9. Para guardar el paquete actualizado, haga clic en **Guardar los elementos seleccionados**, en el menú **Archivo**.  

## <a name="enable-logging-for-package-execution-on-the-ssis-server"></a><a name="server_logging"></a> Habilitar el registro de ejecución del paquete en el servidor SSIS
  En este tema se describe cómo establecer o cambiar el nivel de registro para un paquete cuando se ejecuta un paquete implementado en el servidor de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. El nivel de registro que se establece al ejecutar el paquete invalida el registro de paquete configurado en tiempo de diseño en [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Vea [Habilitar el registro de paquetes en SQL Server Data Tools](#ssdt) para obtener más información.  
  
 En **Propiedades del servidor** de SQL Server, en la propiedad **Server logging level** (Nivel de registro de servidor), puede seleccionar un nivel de registro predeterminado de todo el servidor. Puede elegir entre uno de los niveles de registro integrados que se describen en este tema o puede elegir un nivel de registro personalizado existente. El nivel de registro seleccionado se aplica de manera predeterminada a todos los paquetes que se implementen en el catálogo de SSIS. También se aplica de forma predeterminada a un paso de trabajo del Agente SQL que ejecuta un paquete SSIS.  
  
 También puede especificar el nivel de registro de un paquete individual mediante uno de los métodos siguientes. En este tema se trata el primer método.  
  
-   Configurar una instancia de una ejecución de paquetes mediante el cuadro de diálogo Ejecutar paquete  
  
-   Configurar parámetros para una instancia de una ejecución mediante [catalog.set_execution_parameter_value &#40;base de datos de SSISDB&#41;](../../integration-services/system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md)  
  
-   Configurar un trabajo del Agente SQL Server para una ejecución de paquetes mediante el cuadro de diálogo Nuevo paso de trabajo.  
  
### <a name="set-the-logging-level-for-a-package-by-using-the-execute-package-dialog-box"></a>Establecer el nivel de registro de un paquete mediante el cuadro de diálogo Ejecutar paquete  
  
1.  En [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], navegue hasta el paquete en el Explorador de objetos.  
  
2.  Haga clic con el botón derecho en el paquete y seleccione **Ejecutar**.  
  
3.  Seleccione la pestaña **Avanzadas** en el cuadro de diálogo **Ejecutar paquete** .  
  
4.  En **Nivel de registro**, seleccione el nivel de registro. Este tema contiene una descripción de los valores disponibles.  
  
5.  Complete otras configuraciones de paquetes que desee y haga clic en **Aceptar** para ejecutar el paquete.  
  
### <a name="select-a-logging-level"></a>Seleccionar un nivel de registro  
 Están disponibles los siguientes niveles de registro integrados. También puede seleccionar un nivel de registro personalizado existente. Este tema contiene una descripción de los niveles de registro personalizados.  
  
|Nivel de registro|Descripción|  
|-------------------|-----------------|  
|None|El registro está desactivado. Solo se registra el estado de ejecución del paquete.|  
|Básica|Se registran todos los eventos, excepto los eventos personalizados y de diagnóstico. Este es el valor predeterminado.|  
|RuntimeLineage|Recopila los datos necesarios para realizar un seguimiento de la información de linaje en el flujo de datos. Puede analizar esta información de linaje para asignar la relación de linaje entre las tareas. Los ISV y los desarrolladores pueden generar herramientas de asignación de linaje personalizadas con esta información.|  
|Rendimiento|Solo se registran las estadísticas de rendimiento, y los eventos OnError y OnWarning.<br /><br /> El informe **Rendimiento de la ejecución** muestra el Tiempo activo y el TIempo total para los componentes de flujo de datos del paquete. Esta información está disponible cuando el nivel de registro de la última ejecución del paquete se estableció en **Performance** (Rendimiento) o **Verbose**(Detallado). Para obtener más información, consulte [Reports for the Integration Services Server](../../integration-services/performance/monitor-running-packages-and-other-operations.md#reports).<br /><br /> La vista [catalog.execution_component_phases](../../integration-services/system-views/catalog-execution-component-phases.md) muestra las horas de inicio y de finalización de los componentes de flujo de datos, para cada fase de una ejecución. Esta vista muestra esta información para estos componentes solo cuando el nivel de registro de la ejecución del paquete se estableció en **Rendimiento** o en **Detallado**.|  
|Verbose|Se registran todos los eventos, incluidos los eventos personalizados y de diagnóstico.<br /><br /> Los eventos personalizados incluyen los eventos registrados por las tareas de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Para obtener más información sobre los eventos personalizados, vea [Custom Messages for Logging](#custom_messages).<br /><br /> Un ejemplo de un evento de diagnóstico es el evento **DiagnosticEx** . Cada vez que una tarea Ejecutar paquete ejecuta un paquete secundario, este evento captura los valores de parámetro que se pasan a los paquetes secundarios.<br /><br /> El evento **DiagnosticEx** también le ayuda a obtener los nombres de las columnas en las que se producen errores de nivel de fila. Este evento escribe un mapa de linaje de flujo de datos en el registro. Luego, puede buscar el nombre de columna en este mapa de linaje mediante el identificador de columna que captura una salida de error.  Para obtener más información, vea [Error Handling in Data](../../integration-services/data-flow/error-handling-in-data.md) (Control de errores en los datos).<br /><br /> El valor de la columna de mensaje para **DiagnosticEx** es texto XML. Para ver el texto del mensaje de una ejecución de paquete, ejecute una consulta en la vista [catalog.operation_messages &#40;base de datos de SSISDB&#41;](../../integration-services/system-views/catalog-operation-messages-ssisdb-database.md). Tenga en cuenta que el evento **DiagnosticEx** no conserva el espacio en blanco en la salida XML para reducir el tamaño del registro. Para mejorar la legibilidad, copie el registro en un editor XML (en Visual Studio, por ejemplo) que admita el formato XML y el resaltado de sintaxis.<br /><br /> La vista [catalog.execution_data_statistics](../../integration-services/system-views/catalog-execution-data-statistics.md) muestra una fila cada vez que un componente de flujo de datos envía datos a un componente de nivel inferior para una ejecución del paquete. El nivel de registro se debe establecer en **Detallado** para capturar esta información en la vista.|  
  
### <a name="create-and-manage-customized-logging-levels-by-using-the-customized-logging-level-management-dialog-box"></a>Crear y administrar niveles de registro personalizados mediante el cuadro de diálogo de administración de niveles de registro personalizados  
 Puede crear niveles de registro personalizados que solo recopilen las estadísticas y los eventos que quiera. También puede capturar el contexto de los eventos, que incluye valores de variables, cadenas de conexión y propiedades de componentes. Cuando ejecuta un paquete, puede seleccionar un nivel de registro personalizado en el que puede seleccionar un nivel de registro integrado.  
  
> [!TIP]  
>  Para capturar los valores de las variables de paquete, la propiedad **IncludeInDebugDump** de las variables debe estar establecida en **True**.  
  
1.  Para crear y administrar niveles de registro personalizados, en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], haga clic con el botón derecho en la base de datos SSISDB y seleccione **Customized Logging Level** (Nivel de registro personalizado) para abrir el cuadro de diálogo **Administración del nivel de registro personalizado** . La lista **Customized Logging Levels** (Niveles de registro personalizados) contiene todos los niveles de registro personalizados existentes.  
  
2.  Para **crear** un nuevo registro personalizado, haga clic en **Create**(Crear) y proporcione un nombre y una descripción. En las pestañas **Statistics** (Estadísticas) y **Events** (Eventos), seleccione las estadísticas y los eventos que quiera recopilar. En la pestaña **Events** (Eventos), seleccione opcionalmente **Include Context** (Incluir contexto) para los eventos individuales. A continuación, haga clic en **Save**(Guardar).  
  
3.  Para **actualizar** un nivel de registro personalizado existente, selecciónelo en la lista, vuelva a configurarlo y haga clic en **Save**(Guardar).  
  
4.  Para **eliminar** un nivel de registro personalizado existente, selecciónelo en la lista y haga clic en **Delete**(Eliminar).  
  
 **Permisos para los niveles de registro personalizados.**  
  
-   Todos los usuarios de la base de datos SSISDB pueden ver los niveles de registro personalizados y seleccionar uno al ejecutar paquetes.  
  
-   Solo los usuarios del rol ssis_admin o sysadmin pueden crear, actualizar o eliminar niveles de registro personalizados.  

## <a name="custom-messages-for-logging"></a><a name="custom_messages"></a> Custom Messages for Logging
SQL Server Integration Services proporciona un amplio conjunto de eventos personalizados para escribir entradas del registro para paquetes y muchas tareas. Puede utilizar estas entradas para guardar información detallada sobre el progreso, resultados y problemas de ejecución al registrar eventos predefinidos o mensajes definidos por el usuario para su análisis posterior. Por ejemplo, puede registrar cuando se inicia y finaliza la inserción masiva para identificar los problemas de rendimiento en la ejecución del paquete.  
  
 Las entradas del registro personalizadas pertenecen a un conjunto de entradas diferente de los eventos estándar de registro que están disponibles para los paquetes y todos los contenedores y tareas. Las entradas del registro personalizadas se han adaptado para capturar información de utilidad sobre una tarea específica de un paquete. Por ejemplo, una de las entradas de registro personalizadas para la tarea Ejecutar SQL registra la instrucción SQL que ejecuta la tarea en el registro.  
  
 Todas las entradas del registro incluyen información de fecha y hora, incluidas las entradas del registro que se escriben automáticamente cuando se inicia o finaliza un paquete. Muchos de los eventos de registro escriben varias entradas en el registro. Esto ocurre generalmente cuando los eventos tienen diferentes fases. Por ejemplo, el evento de registro **ExecuteSQLExecutingQuery** escribe tres entradas: una entrada después de que la tarea adquiere una conexión con la base de datos, otra después de que la tarea comienza a preparar la instrucción SQL y otra más una vez que se completa la ejecución de la instrucción SQL.  
  
 Los siguientes objetos [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] poseen entradas del registro personalizadas:  
  
 [Package](#Package)  
  
 [Tarea Inserción masiva](#BulkInsert)  
  
 [Tarea Flujo de datos](#DataFlow)  
  
 [Tarea Ejecutar DTS 2000](#ExecuteDTS200)  
  
 [Tarea Ejecutar proceso](#ExecuteProcess)  
  
 [Tarea Ejecutar SQL](#ExecuteSQL)  
  
 [Tarea Sistema de archivos](#FileSystem)  
  
 [Tarea FTP](#FTP)  
  
 [Tarea Cola de mensajes](#MessageQueue)  
  
 [Tarea Script](#Script)  
  
 [Tarea Enviar correo](#SendMail)  
  
 [Tarea Transferir bases de datos](#TransferDatabase)  
  
 [Tarea Transferir mensajes de error](#TransferErrorMessages)  
  
 [Tarea Transferir trabajos](#TransferJobs)  
  
 [Tarea Transferir inicios de sesión](#TransferLogins)  
  
 [Tarea Transferir procedimientos almacenados principales](#TransferMasterStoredProcedures)  
  
 [Tarea Transferir objetos de SQL Server](#TransferSQLServerObjects)  
  
 [Tarea Servicios web](#WebServices)  
  
 [Tarea Lector de datos WMI](#WMIDataReader)  
  
 [Tarea Monitor de eventos WMI](#WMIEventWatcher)  
  
 [Tarea XML](#XML)  
  
### <a name="log-entries"></a>Entradas del registro  
  
####  <a name="package"></a><a name="Package"></a> Paquete  
 La siguiente tabla contiene las entradas del registro personalizadas para paquetes.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**PackageStart**|Indica que se inició la ejecución del paquete. Este entrada del registro se escribe automáticamente en el registro. No se puede excluir.|  
|**PackageEnd**|Indica que finalizó la ejecución del paquete. Este entrada del registro se escribe automáticamente en el registro. No se puede excluir.|  
|**Diagnostic**|Proporciona información sobre la configuración del sistema que afecta a la ejecución de paquetes, como el número de ejecutables que se pueden ejecutar simultáneamente.<br /><br /> La entrada del registro **Diagnostic** también incluye entradas por delante y por detrás relativas a las llamadas a proveedores de datos externos.|  
  
####  <a name="bulk-insert-task"></a><a name="BulkInsert"></a> Tarea Inserción masiva  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Inserción masiva.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**DTSBulkInsertTaskBegin**|Indica que se inició la inserción masiva.|  
|**DTSBulkInsertTaskEnd**|Indica que finalizó la inserción masiva.|  
|**DTSBulkInsertTaskInfos**|Proporciona información descriptiva sobre la tarea.|  
  
####  <a name="data-flow-task"></a><a name="DataFlow"></a> Data Flow Task  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Flujo de datos.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**BufferSizeTuning**|Indica que la tarea Flujo de datos cambió el tamaño del búfer. En la entrada del registro se describen las razones del cambio de tamaño y se indica el nuevo tamaño temporal del búfer.|  
|**OnPipelinePostEndOfRowset**|Indica que se ha dado la señal de fin del conjunto de filas a un componente, la cual se establece a través de la última llamada del método **ProcessInput** . Se escribe una entrada por cada componente del flujo de datos que procesa la entrada de datos. La entrada incluye el nombre del componente.|  
|**OnPipelinePostPrimeOutput**|Indica que el componente ha completado su última llamada al método **PrimeOutput** . En función del flujo de datos, es posible que se escriban varias entradas. Si el componente es un origen, esto significa que el componente ha terminado de procesar filas.|  
|**OnPipelinePreEndOfRowset**|Indica que un componente está a punto de recibir la señal de fin del conjunto de filas, la cual se establece a través de la última llamada del método **ProcessInput** . Se escribe una entrada por cada componente del flujo de datos que procesa la entrada de datos. La entrada incluye el nombre del componente.|  
|**OnPipelinePrePrimeOutput**|Indica que el componente está a punto de recibir su última llamada del método **PrimeOutput** . En función del flujo de datos, es posible que se escriban varias entradas.|  
|**OnPipelineRowsSent**|Informa del número de filas que se proporciona a una entrada de componentes a través de una llamada al método **ProcessInput** . La entrada del registro incluye el nombre del componente.|  
|**PipelineBufferLeak**|Proporciona información sobre cualquier componente que mantuvo la conexión de los búferes después de que desapareciera el administrador de búferes. Esto significa que no se liberaron los recursos de los búferes y podría ocasionar pérdidas de memoria. La entrada del registro proporciona el nombre del componente y el Id. del búfer.|  
|**PipelineExecutionPlan**|Informa del plan de ejecución del flujo de datos. Proporciona información sobre cómo se van a enviar los búferes a los componentes. Esta información, junto con la entrada PipelineExecutionTrees, explica lo que ocurre en la tarea.|  
|**PipelineExecutionTrees**|Informa sobre los árboles de ejecución del diseño del flujo de datos. El programador del motor de flujo de datos utiliza los árboles para generar el plan de ejecución del flujo de datos.|  
|**PipelineInitialization**|Proporciona información de inicialización sobre la tarea. Esta información incluye los directorios que se utilizan para el almacenamiento temporal de datos BLOB, el tamaño predeterminado del búfer y la cantidad de filas de un búfer. En función de la configuración de la tarea Flujo de datos, es posible que se escriban varias entradas.|  
  
####  <a name="execute-dts-2000-task"></a><a name="ExecuteDTS200"></a> Tarea Ejecutar DTS 2000  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Ejecutar DTS 2000.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**ExecuteDTS80PackageTaskBegin**|Indica que la tarea inició la ejecución del paquete DTS 2000.|  
|**ExecuteDTS80PackageTaskEnd**|Indica que finalizó la tarea.<br /><br /> Nota: Es posible que el paquete DTS 2000 continúe ejecutándose una vez finalizada la tarea.|  
|**ExecuteDTS80PackageTaskTaskInfo**|Proporciona información descriptiva sobre la tarea.|  
|**ExecuteDTS80PackageTaskTaskResult**|Informa del resultado de la ejecución del paquete DTS 2000 que ejecutó la tarea.|  
  
####  <a name="execute-process-task"></a><a name="ExecuteProcess"></a> Tarea Ejecutar proceso  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Ejecutar proceso.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Proporciona información sobre el proceso de ejecución del ejecutable que se configuró para que ejecute la tarea.<br /><br /> Se escriben dos entradas del registro. Una entrada contiene información sobre el nombre y la ubicación del ejecutable que ejecuta la tarea y la otra entrada registra la salida del ejecutable.|  
|**ExecuteProcessVariableRouting**|Proporciona información acerca de las variables que se enrutan a la entrada y las salidas del ejecutable. Se escriben entradas del registro para stdin (la entrada), stdout (la salida) y stderr (la salida de errores).|  
  
####  <a name="execute-sql-task"></a><a name="ExecuteSQL"></a> Tarea Ejecutar SQL  
 La siguiente tabla contiene la entrada del registro personalizada para la tarea Ejecutar SQL.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**ExecuteSQLExecutingQuery**|Proporciona información sobre las fases de ejecución de la instrucción SQL. Las entradas de registro se escriben cuando la tarea adquiere una conexión con la base de datos, cuando la tarea comienza a preparar la instrucción SQL y una vez que se completa la ejecución de la instrucción SQL. La entrada del registro para la fase de preparación incluye la instrucción SQL que utiliza la tarea.|  
  
####  <a name="file-system-task"></a><a name="FileSystem"></a> Tarea Sistema de archivos  
 La siguiente tabla contiene las entradas de registro personalizadas para la tarea Sistema de archivos.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**FileSystemOperation**|Informa sobre la operación que realiza la tarea. La entrada del registro se escribe cuando se inicia la operación del sistema de archivos e incluye información sobre el origen y el destino.|  
  
####  <a name="ftp-task"></a><a name="FTP"></a> Tarea FTP  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea FTP.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**FTPConnectingToServer**|Indica que la tarea inició una conexión con el servidor FTP.|  
|**FTPOperation**|Informa del comienzo y del tipo de operación de FTP que realiza la tarea.|  
  
####  <a name="message-queue-task"></a><a name="MessageQueue"></a> Tarea Cola de mensajes  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Cola de mensajes.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**MSMQAfterOpen**|Indica que la tarea finalizó la apertura de la cola de mensajes.|  
|**MSMQBeforeOpen**|Indica que la tarea inició la apertura de la cola de mensajes.|  
|**MSMQBeginReceive**|Indica que la tarea comenzó a recibir un mensaje.|  
|**MSMQBeginSend**|Indica que la tarea comenzó a enviar un mensaje.|  
|**MSMQEndReceive**|Indica que la tarea finalizó la recepción de un mensaje.|  
|**MSMQEndSend**|Indica que la tarea finalizó el envío de un mensaje.|  
|**MSMQTaskInfo**|Proporciona información descriptiva sobre la tarea.|  
|**MSMQTaskTimeOut**|Indica que se superó el tiempo de espera de la tarea.|  
  
####  <a name="script-task"></a><a name="Script"></a> Tarea Script  
 La siguiente tabla contiene la entrada personalizada de registro para la tarea Script.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**ScriptTaskLogEntry**|Informa sobre los resultados de la implementación del registro en el script. Se escribe una entrada de registro para cada llamada al método **Log** del objeto **Dts** . La entrada se escribe cuando se ejecuta el código. Para más información, consulte [Logging in the Script Task](../../integration-services/extending-packages-scripting/task/logging-in-the-script-task.md).|  
  
####  <a name="send-mail-task"></a><a name="SendMail"></a> Tarea Enviar correo  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Enviar correo.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**SendMailTaskBegin**|Indica que la tarea comenzó a enviar un mensaje de correo electrónico.|  
|**SendMailTaskEnd**|Indica que la tarea finalizó el envío de un mensaje de correo electrónico.|  
|**SendMailTaskInfo**|Proporciona información descriptiva sobre la tarea.|  
  
####  <a name="transfer-database-task"></a><a name="TransferDatabase"></a> Tarea Transferir bases de datos  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir bases de datos.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**SourceDB**|Especifica la base de datos que copió la tarea.|  
|**SourceSQLServer**|Especifica el equipo desde el que se copió la base de datos.|  
  
####  <a name="transfer-error-messages-task"></a><a name="TransferErrorMessages"></a> Tarea Transferir mensajes de error  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir mensajes de error.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**TransferErrorMessagesTaskFinishedTransferringObjects**|Indica que la tarea finalizó la transferencia de los mensajes de error.|  
|**TransferErrorMessagesTaskStartTransferringObjects**|Indica que la tarea inició la transferencia de los mensajes de error.|  
  
####  <a name="transfer-jobs-task"></a><a name="TransferJobs"></a> Tarea Transferir trabajos  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir trabajos.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**TransferJobsTaskFinishedTransferringObjects**|Indica que la tarea finalizó la transferencia de los trabajos del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TransferJobsTaskStartTransferringObjects**|Indica que la tarea inició la transferencia de los trabajos del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
####  <a name="transfer-logins-task"></a><a name="TransferLogins"></a> Tarea Transferir inicios de sesión  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir inicios de sesión.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**TransferLoginsTaskFinishedTransferringObjects**|Indica que la tarea finalizó la transferencia de inicios de sesión.|  
|**TransferLoginsTaskStartTransferringObjects**|Indica que la tarea inició la transferencia de los inicios de sesión.|  
  
####  <a name="transfer-master-stored-procedures-task"></a><a name="TransferMasterStoredProcedures"></a> Tarea Transferir procedimientos almacenados principales  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir procedimientos almacenados principales.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**TransferStoredProceduresTaskFinishedTransferringObjects**|Indica que la tarea finalizó la transferencia de los procedimientos almacenados definidos por el usuario que están almacenados en la base de datos **maestra** .|  
|**TransferStoredProceduresTaskStartTransferringObjects**|Indica que la tarea inició la transferencia de los procedimientos almacenados definidos por el usuario que están almacenados en la base de datos **maestra** .|  
  
####  <a name="transfer-sql-server-objects-task"></a><a name="TransferSQLServerObjects"></a> Tarea Transferir objetos de SQL Server  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Transferir objetos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**TransferSqlServerObjectsTaskFinishedTransferringObjects**|Indica que la tarea finalizó la transferencia de los objetos de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**TransferSqlServerObjectsTaskStartTransferringObjects**|Indica que la tarea inició la transferencia de los objetos de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
  
####  <a name="web-services-task"></a><a name="WebServices"></a> Tarea Servicios web  
 La siguiente tabla contiene las entradas del registro personalizadas que puede habilitar para la tarea Servicios web.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**WSTaskBegin**|La tarea inició el acceso a un servicio web.|  
|**WSTaskEnd**|La tarea completó un método de servicio web.|  
|**WSTaskInfo**|Información descriptiva acerca de la tarea.|  
  
####  <a name="wmi-data-reader-task"></a><a name="WMIDataReader"></a> Tarea Lector de datos WMI  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Lector de datos WMI.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**WMIDataReaderGettingWMIData**|Indica que la tarea inició la lectura de datos WMI.|  
|**WMIDataReaderOperation**|Informa de la consulta WQL que ejecutó la tarea.|  
  
####  <a name="wmi-event-watcher-task"></a><a name="WMIEventWatcher"></a> Tarea Monitor de eventos WMI  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Monitor de eventos WMI.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**WMIEventWatcherEventOccurred**|Indica que ocurrió el evento que supervisaba la tarea.|  
|**WMIEventWatcherTimedout**|Indica que se superó el tiempo de espera de la tarea.|  
|**WMIEventWatcherWatchingForWMIEvents**|Indica que la tarea inició la ejecución de la consulta WQL. La entrada incluye la consulta.|  
  
####  <a name="xml-task"></a><a name="XML"></a> Tarea XML  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea XML.  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**XMLOperation**|Proporciona información sobre la operación que la tarea realiza.|  

## <a name="related-tasks"></a>Related Tasks  
 La lista siguiente contiene vínculos a temas que muestran cómo realizar tareas relacionadas con la característica de registro.  
  
-   [Eventos registrados por un paquete de Integration Services](../../integration-services/performance/events-logged-by-an-integration-services-package.md)  
  
## <a name="related-content"></a>Contenido relacionado  
 [Herramienta DTLoggedExec para el registro completo y detallado (proyecto CodePlex)](https://go.microsoft.com/fwlink/?LinkId=150579)  
