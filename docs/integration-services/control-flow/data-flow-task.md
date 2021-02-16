---
description: tarea Flujo de datos
title: Tarea Flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.dataflowtask.f1
helpviewer_keywords:
- data flow task [Integration Services]
- performance [Integration Services]
- data flow [Integration Services], performance
- data flow [Integration Services], Data Flow task
- Integration Services, performance
ms.assetid: c27555c4-208c-43c8-b511-a4de2a8a3344
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 00270115679b886a0f4d657487a91e845c56da3f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352124"
---
# <a name="data-flow-task"></a>tarea Flujo de datos

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Flujo de datos encapsula el motor de flujo de datos que mueve datos entre orígenes y destinos, y permite al usuario transformar, limpiar y modificar datos a medida que se mueven. Agregar una tarea Flujo de datos a un flujo de control de paquetes permite que el paquete extraiga, transforme y cargue datos.  
  
 Un flujo de datos se compone de por lo menos un componente de flujo de datos, pero normalmente es un conjunto de componentes de flujo de datos conectados: orígenes que extraen datos, transformaciones que modifican, enrutan o resumen datos, y destinos que cargan datos.  
  
 En el tiempo de ejecución, la tarea Flujo de datos genera un plan de ejecución a partir del flujo de datos y el motor de flujo de datos ejecuta el plan. Se puede crear una tarea Flujo de datos que no tenga flujo de datos, pero la tarea solo se ejecuta si incluye por lo menos un flujo de datos.  
  
 Para realizar una inserción masiva de datos de archivos de texto en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puede usar la tarea Inserción masiva en lugar de la tarea Flujo de datos y un flujo de datos. Sin embargo, la tarea Inserción masiva no transforma datos. Para más información, consulte [Bulk Insert Task](../../integration-services/control-flow/bulk-insert-task.md).  
  
## <a name="multiple-flows"></a>Varios flujos  
 Una tarea Flujo de datos puede incluir varios flujos de datos. Si una tarea copia varios conjuntos de datos, y si el orden en que los datos se copian no es significativo, puede ser más conveniente incluir varios flujos de datos en la tarea Flujo de datos. Por ejemplo, puede crear cinco flujos de datos, y cada uno de ellos copiaría datos de un archivo plano en una tabla de dimensiones diferente en un esquema de estrella de almacenamiento de datos.  
  
 Sin embargo, el motor de flujo de datos determina el orden de ejecución cuando existen varios flujos de datos dentro de una tarea de flujo de datos. Por tanto, cuando el orden es importante, el paquete debe usar varias tareas Flujo de datos, cada una de las cuales contiene un flujo de datos. En ese caso, puede aplicar restricciones de precedencia para controlar el orden de ejecución de las tareas.  
  
 El siguiente diagrama muestra una tarea Flujo de datos con varios flujos de datos.  
  
 ![Flujos de datos](../../integration-services/control-flow/media/mw-dts-09.gif "Flujos de datos")  
  
## <a name="log-entries"></a>Entradas del registro  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] proporciona un conjunto de eventos de registro que están disponibles para todas las tareas. [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] también proporciona entradas del registro personalizadas para numerosas tareas. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md). La tarea Flujo de datos incluye las siguientes entradas de registro personalizadas:  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**BufferSizeTuning**|Indica que la tarea Flujo de datos cambió el tamaño del búfer. En la entrada del registro se describen las razones del cambio de tamaño y se indica el nuevo tamaño temporal del búfer.|  
|**OnPipelinePostEndOfRowset**|Indica que se ha dado la señal de fin del conjunto de filas a un componente, la cual se establece a través de la última llamada del método **ProcessInput** . Se escribe una entrada por cada componente del flujo de datos que procesa la entrada de datos. La entrada incluye el nombre del componente.|  
|**OnPipelinePostPrimeOutput**|Indica que el componente ha completado su última llamada al método **PrimeOutput** . En función del flujo de datos, es posible que se escriban varias entradas. Si el componente es un origen, esta entrada del registro significa que el componente ha terminado de procesar filas.|  
|**OnPipelinePreEndOfRowset**|Indica que un componente está a punto de recibir la señal de fin del conjunto de filas, la cual se establece a través de la última llamada del método **ProcessInput** . Se escribe una entrada por cada componente del flujo de datos que procesa la entrada de datos. La entrada incluye el nombre del componente.|  
|**OnPipelinePrePrimeOutput**|Indica que el componente está a punto de recibir su última llamada del método **PrimeOutput** . En función del flujo de datos, es posible que se escriban varias entradas.|  
|**OnPipelineRowsSent**|Informa del número de filas que se proporciona a una entrada de componentes a través de una llamada al método **ProcessInput** . La entrada del registro incluye el nombre del componente.|  
|**PipelineBufferLeak**|Proporciona información sobre cualquier componente que mantuvo la conexión de los búferes después de que desapareciera el administrador de búferes. Si se mantiene la conexión de un búfer, no se liberan los recursos de los búferes y podrían ocasionarse pérdidas de memoria. La entrada del registro proporciona el nombre del componente y el Id. del búfer.|  
|**PipelineComponentTime**|Indica la cantidad de tiempo (en milisegundos) que el componente ha usado en cada uno de los cinco pasos de procesamiento principales: Validate, PreExecute, PostExecute, ProcessInput y ProcessOutput.|  
|**PipelineExecutionPlan**|Informa del plan de ejecución del flujo de datos. El plan de ejecución proporciona información sobre cómo se van a enviar los búferes a los componentes. Esta información, junto con la entrada del registro PipelineExecutionTrees, describe lo que ocurre en la tarea Flujo de datos.|  
|**PipelineExecutionTrees**|Informa sobre los árboles de ejecución del diseño del flujo de datos. El programador del motor de flujo de datos utiliza los árboles para generar el plan de ejecución del flujo de datos.|  
|**PipelineInitialization**|Proporciona información de inicialización sobre la tarea. Esta información incluye los directorios que se utilizan para el almacenamiento temporal de datos BLOB, el tamaño predeterminado del búfer y la cantidad de filas de un búfer. En función de la configuración de la tarea Flujo de datos, es posible que se escriban varias entradas.|  
  
 Estas entradas de registro proporcionan gran cantidad de información acerca de la ejecución de la tarea Flujo de datos cada vez que ejecuta un paquete. Conforme ejecuta los paquetes repetidamente, puede recopilar información que, con el tiempo, proporciona datos históricos importantes acerca del procesamiento que realiza la tarea, problemas que pueden afectar al rendimiento y el volumen de datos que controla la tarea.  
  
 Para obtener más información sobre cómo utilizar estas entradas de registro para supervisar y mejorar el rendimiento del flujo de datos, vea uno de los temas siguientes:  
  
-   [Contadores de rendimiento](../../integration-services/performance/performance-counters.md)  
  
-   [Características de rendimiento del flujo de datos](../../integration-services/data-flow/data-flow-performance-features.md)  
  
### <a name="sample-messages-from-a-data-flow-task"></a>Mensajes de ejemplo de una tarea Flujo de trabajo  
 En la tabla siguiente se muestran mensajes de ejemplo de entradas de registro de un paquete muy sencillo. El paquete utiliza un origen OLE DB para extraer datos de una tabla, una transformación Ordenar para ordenar los datos y un destino OLE DB para escribir los datos en una tabla diferente.  
  
|Entrada del registro|error de Hadoop|  
|---------------|--------------|  
|**BufferSizeTuning**|`Rows in buffer type 0 would cause a buffer size greater than the configured maximum. There will be only 9637 rows in buffers of this type.`<br /><br /> `Rows in buffer type 2 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`<br /><br /> `Rows in buffer type 3 would cause a buffer size greater than the configured maximum. There will be only 9497 rows in buffers of this type.`|  
|**OnPipelinePostEndOfRowset**|`A component will be given the end of rowset signal. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component will be given the end of rowset signal. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePostPrimeOutput**|`A component has returned from its PrimeOutput call. : 1180 : Sort`<br /><br /> `A component has returned from its PrimeOutput call. : 1 : OLE DB Source`|  
|**OnPipelinePreEndOfRowset**|`A component has finished processing all of its rows. : 1180 : Sort : 1181 : Sort Input`<br /><br /> `A component has finished processing all of its rows. : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input`|  
|**OnPipelinePrePrimeOutput**|`PrimeOutput will be called on a component. : 1180 : Sort`<br /><br /> `PrimeOutput will be called on a component. : 1 : OLE DB Source`|  
|**OnPipelineRowsSent**|`Rows were provided to a data flow component as input. :  : 1185 : OLE DB Source Output : 1180 : Sort : 1181 : Sort Input : 76`<br /><br /> `Rows were provided to a data flow component as input. :  : 1308 : Sort Output : 1291 : OLE DB Destination : 1304 : OLE DB Destination Input : 76`|  
|**PipelineComponentTime**|`The component "Calculate LineItemTotalCost" (3522) spent 356 milliseconds in ProcessInput.`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 79 milliseconds in ProcessInput.`<br /><br /> `The component "Calculate Average Cost" (3662) spent 16 milliseconds in ProcessInput.`<br /><br /> `The component "Sort by ProductID" (3717) spent 125 milliseconds in ProcessInput.`<br /><br /> `The component "Load Data" (3773) spent 0 milliseconds in ProcessInput.`<br /><br /> `The component "Extract Data" (3869) spent 688 milliseconds in PrimeOutput filling buffers on output "OLE DB Source Output" (3879).`<br /><br /> `The component "Sum Quantity and LineItemTotalCost" (3619) spent 141 milliseconds in PrimeOutput filling buffers on output "Aggregate Output 1" (3621).`<br /><br /> `The component "Sort by ProductID" (3717) spent 16 milliseconds in PrimeOutput filling buffers on output "Sort Output" (3719).`|  
|**PipelineExecutionPlan**|`SourceThread0`<br /><br /> `Drives: 1`<br /><br /> `Influences: 1180 1291`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 1 for output ID 11.`<br /><br /> `SetBufferListener: "WorkThread0" for input ID 1181`<br /><br /> `CreatePrimeBuffer of type 3 for output ID 12.`<br /><br /> `CallPrimeOutput on component "OLE DB Source" (1)`<br /><br /> `End Output Work List`<br /><br /> `End SourceThread0`<br /><br /> `WorkThread0`<br /><br /> `Drives: 1180`<br /><br /> `Influences: 1180 1291`<br /><br /> `Input Work list, input ID 1181 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1181 on component "Sort" (1180) for view type 2`<br /><br /> `End Input Work list for input 1181`<br /><br /> `Output Work List`<br /><br /> `CreatePrimeBuffer of type 4 for output ID 1182.`<br /><br /> `SetBufferListener: "WorkThread1" for input ID 1304`<br /><br /> `CallPrimeOutput on component "Sort" (1180)`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread0`<br /><br /> `WorkThread1`<br /><br /> `Drives: 1291`<br /><br /> `Influences: 1291`<br /><br /> `Input Work list, input ID 1304 (1 EORs Expected)`<br /><br /> `CallProcessInput on input ID 1304 on component "OLE DB Destination" (1291) for view type 5`<br /><br /> `End Input Work list for input 1304`<br /><br /> `Output Work List`<br /><br /> `End Output Work List`<br /><br /> `End WorkThread1`|  
|**PipelineExecutionTrees**|`begin execution tree 0`<br /><br /> `output "OLE DB Source Output" (11)`<br /><br /> `input "Sort Input" (1181)`<br /><br /> `end execution tree 0`<br /><br /> `begin execution tree 1`<br /><br /> `output "OLE DB Source Error Output" (12)`<br /><br /> `end execution tree 1`<br /><br /> `begin execution tree 2`<br /><br /> `output "Sort Output" (1182)`<br /><br /> `input "OLE DB Destination Input" (1304)`<br /><br /> `output "OLE DB Destination Error Output" (1305)`<br /><br /> `end execution tree 2`|  
|**PipelineInitialization**|`No temporary BLOB data storage locations were provided. The buffer manager will consider the directories in the TEMP and TMP environment variables.`<br /><br /> `The default buffer size is 10485760 bytes.`<br /><br /> `Buffers will have 10000 rows by default`<br /><br /> `The data flow will not remove unused components because its RunInOptimizedMode property is set to false.`|  
  
 Muchos eventos de registro escriben varias entradas, y los mensajes de un gran número de entradas del registro contienen datos complejos. Para facilitar la comprensión y comunicar el contenido de mensajes complejos, puede analizar el texto del mensaje. En función de la ubicación de los registros, puede usar instrucciones Transact-SQL o un componente de script para separar el texto complejo en columnas u otros formatos que considere más útiles.  
  
 Por ejemplo, la tabla siguiente contiene el mensaje "Se proporcionaron filas como entrada de un componente de flujo de datos. :  : 1185 : Salida de origen de OLE DB : 1180 : Ordenar : 1181 : Entrada de ordenación : 76", analizado en columnas. El evento **OnPipelineRowsSent** escribió el mensaje cuando se enviaron filas del origen de OLE DB a la transformación Ordenar.  
  
|Columna|Descripción|Value|  
|------------|-----------------|-----------|  
|**PathID**|Valor de la propiedad **ID** de la ruta entre el origen de OLE DB y la transformación Ordenar.|1185|  
|**PathName**|Valor de la propiedad **Name** de la ruta.|Salida de origen de OLE DB|  
|**ComponentID**|Valor de la propiedad **ID** de la transformación Ordenar.|1180|  
|**ComponentName**|Valor de la propiedad **Name** de la transformación Ordenar.|Sort|  
|**InputID**|Valor de la propiedad **ID** de la entrada de la transformación Ordenar.|1181|  
|**InputName**|Valor de la propiedad **Name** de la entrada de la transformación Ordenar.|Entrada de ordenación|  
|**RowsSent**|Número de filas enviadas a la entrada de la transformación Ordenar.|76|  
  
## <a name="configuration-of-the-data-flow-task"></a>Configuración de la tarea Flujo de datos  
 Puede establecer propiedades en la ventana **Propiedades** o mediante programación.  
  
 Para obtener más información sobre cómo establecer estas propiedades en la ventana **Propiedades** , haga clic en el tema siguiente:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-data-flow-task"></a>Configuración mediante programación de la tarea Flujo de datos  
 Para obtener más información sobre cómo agregar una tarea Flujo de datos a un paquete y establecer las propiedades del flujo de datos mediante programación, haga clic en el tema siguiente:  
  
-   [Agregar la tarea de flujo de datos mediante programación](../../integration-services/building-packages-programmatically/adding-the-data-flow-task-programmatically.md)  
  
## <a name="related-tasks"></a>Related Tasks  
 [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="related-content"></a>Contenido relacionado  
 Vídeo, [Balanced Data Distributor](/previous-versions/dn912438(v=msdn.10)), en technet.microsoft.com  
  
