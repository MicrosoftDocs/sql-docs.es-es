---
description: Message Queue Task
title: Tarea Cola de mensajes | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.messagequeuetask.f1
- sql13.dts.designer.msgqueuetask.general.f1
- sql13.dts.designer.msgqueuetask.send.f1
- sql13.dts.designer.msgqueuetask.receive.f1
- sql13.dts.designer.selectvariables.f1
helpviewer_keywords:
- Message Queue task [Integration Services]
- receiving messages
- messages [Integration Services]
- sending messages
ms.assetid: ae1d8fad-6649-4e93-b589-14a32d07da33
author: chugugrace
ms.author: chugu
ms.openlocfilehash: e39187e32afbcf72bf1eb950f18d5f21cef38211
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348298"
---
# <a name="message-queue-task"></a>Message Queue Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Cola de mensajes le permite usar Message Queue Server (que también recibe el nombre de MSMQ) para enviar y recibir mensajes entre paquetes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], o enviar mensajes a una cola de aplicaciones procesada por una aplicación personalizada. Estos mensajes pueden adoptar la forma de texto simple, archivos o variables y sus valores.  
  
 Al utilizar la tarea Cola de mensajes, puede coordinar operaciones en toda la empresa. Los mensajes se pueden colocar en cola y enviarse más tarde si el destino no está disponible o está ocupado. Por ejemplo, la tarea puede colocar en cola mensajes para el equipo portátil sin conexión de los representantes de ventas, que reciben sus mensajes cuando se conectan a la red. Puede usar la tarea Cola de mensajes para los siguientes fines:  
  
-   Retrasar la ejecución de la tarea hasta que hayan entrado otros paquetes. Por ejemplo, en cada punto de venta, después del mantenimiento nocturno, una tarea Cola de mensajes puede enviar un mensaje al equipo corporativo. Un paquete que se ejecuta en el equipo corporativo contiene tareas Cola de mensajes, cada una de las cuales espera el mensaje de un punto de venta específico. Cuando llega un mensaje de un punto de venta, una tarea carga los datos desde ese punto de venta. Una vez han llegado todos los puntos de venta, el paquete calcula los resultados totales.  
  
-   Enviar archivos de datos al equipo que los procesa. Por ejemplo, los resultados de la caja registradora de un restaurante pueden enviarse en un mensaje de archivo de datos al sistema de nóminas corporativo, donde se extraen los datos de las propinas de cada camarero.  
  
-   Distribuir archivos en toda la empresa. Por ejemplo, un paquete puede utilizar una tarea Cola de mensajes para enviar un archivo de paquete a otro equipo. A continuación, un paquete que se ejecuta en el equipo de destino puede utilizar una tarea Cola de mensajes para recuperar y guardar localmente el paquete.  
  
 Al enviar o recibir mensajes, la tarea Cola de mensajes usa uno de estos cuatro tipos de mensajes: archivo de datos, cadena, mensaje de cadena para variable o variable. El tipo de mensaje Mensaje de cadena para variable solamente se puede usar al recibir mensajes.  
  
 La tarea usa el administrador de conexiones MSMQ para conectarse a una cola de mensajes. Para más información, vea [Administrador de conexiones MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md). Para obtener más información acerca de Message Queue Server, vea [MSDN Library](../../sql-server/index.yml).  
  
 La tarea Cola de mensajes exige que se instale el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Algunos componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que puede seleccionar para su instalación en la página **Componentes para instalar** o **Selección de características** del Asistente para la instalación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalan un subconjunto parcial de componentes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] . Estos componentes resultan útiles para tareas específicas, pero la funcionalidad de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] estará limitada. Por ejemplo, la opción [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] instala los componentes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que se necesitan para diseñar un paquete, pero no se instala el servicio [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] y, por tanto, la tarea Cola de mensajes no es funcional. Para garantizar la instalación completa de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)], debe seleccionar [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] en la página **Componentes para instalar** . Para más información sobre cómo instalar y ejecutar la tarea Cola de mensajes, vea [Instalar Integration Services](../../integration-services/install-windows/install-integration-services.md).  
  
> [!NOTE]  
>  La tarea Cola de mensajes no cumple el estándar federal de procesamiento de información (FIPS, Federal Information Processing Standard) 140-2 cuando el sistema operativo del equipo se configura en modo FIPS y la tarea usa cifrado. Si la tarea Cola de mensajes no usa cifrado, se puede ejecutar la tarea correctamente.  
  
## <a name="message-types"></a>Tipos de mensajes  
 Puede configurar los tipos de mensaje que proporciona la tarea Cola de mensajes de las siguientes maneras:  
  
-   El mensaje **Data file** especifica que un archivo contiene el mensaje. Al recibir mensajes, puede configurar la tarea para guardar el archivo, sobrescribir un archivo existente y especificar el paquete desde el cual la tarea puede recibir mensajes.  
  
-   El mensaje **String** especifica el mensaje como una cadena. Al recibir mensajes, puede configurar la tarea para comparar la cadena recibida con una cadena definida por el usuario y actuar según la comparación. La comparación de cadenas puede ser exacta, distinguir o no entre mayúsculas y minúsculas o usar una subcadena.  
  
-   **String message to variable** especifica el mensaje de origen como una cadena que se envía a una variable de destino. Puede configurar la tarea para comparar la cadena recibida con una cadena definida por el usuario aplicando una comparación exacta, que no distinga entre mayúsculas y minúsculas o de subcadena. Este tipo de mensaje está disponible solamente cuando la tarea recibe mensajes.  
  
-   **Variable** especifica que el mensaje contiene una o más variables. Puede configurar la tarea para especificar los nombres de las variables que se incluyen en el mensaje. Al recibir mensajes, puede configurar la tarea para especificar el paquete desde el cual se pueden recibir mensajes y la variable que es el destino del mensaje.  
  
## <a name="sending-messages"></a>enviar mensajes  
 Cuando configure la tarea Cola de mensajes para enviar mensajes, puede utilizar uno de los algoritmos de cifrado actualmente compatibles con tecnología de Message Queue Server, RC2 y RC4, para cifrar el mensaje. Ambos algoritmos de cifrado se consideran ahora criptográficamente menos seguros que algoritmos más recientes con los que aún no es compatible la tecnología de Message Queue Server. Por tanto, debe considerar con detenimiento sus necesidades criptográficas a la hora de enviar mensajes con la tarea Cola de mensajes.  
  
## <a name="receiving-messages"></a>recibir mensajes  
 Al recibir mensajes, la tarea Cola de mensajes se puede configurar de las siguientes maneras:  
  
-   Omitir el mensaje o eliminar el mensaje de la cola.  
  
-   Especificar un tiempo de espera.  
  
-   Generar un error cuando se agota el tiempo de espera.  
  
-   Sobrescribir un archivo existente, si el mensaje se guarda en un **Data file**.  
  
-   Guardar el archivo de mensajes con un nombre de archivo diferente, si el mensaje usa el tipo **Data file message** .  
  
## <a name="custom-logging-messages-available-on-the-message-queue-task"></a>Mensajes de registro personalizados disponibles en la tarea Cola de mensajes  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Cola de mensajes. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
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
  
## <a name="configuration-of-the-message-queue-task"></a>Configuración de la tarea Cola de mensajes  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación. Para obtener información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Página Expresiones](../../integration-services/expressions/expressions-page.md)  
  
 Para obtener información sobre cómo establecer estas propiedades mediante programación, vea la documentación de la clase **Microsoft.SqlServer.Dts.Tasks.MessageQueueTask.MessageQueueTask** en la Guía del desarrollador.  
  
## <a name="related-tasks"></a>Related Tasks  
 Para más información sobre cómo establecer estas propiedades en el Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] , vea [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md).  
  
## <a name="message-queue-task-editor-general-page"></a>Editor de la tarea Cola de mensajes (página General)
  Utilice la página **General** del cuadro de diálogo **Editor de la tarea Cola de mensajes** para asignar un nombre y describir la tarea Cola de mensajes, especificar el formato del mensaje e indicar si la tarea envía o recibe o mensajes.  
  
### <a name="options"></a>Opciones  
 **Nombre**  
 Proporcione un nombre único para la tarea Cola de mensajes. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Cola de mensajes.  
  
 **Use2000Format**  
 Indica si se va a utilizar el formato 2000 de Message Queue Server (que también recibe el nombre de MSMQ). El valor predeterminado es **False**.  
  
 **MSMQConnection**  
 Seleccione un administrador de conexiones MSMQ existente o haga clic en \<**New connection...**> para crear uno.  
  
 **Temas relacionados**: [Administrador de conexiones MSMQ](../../integration-services/connection-manager/msmq-connection-manager.md), [Editor del administrador de conexiones MSMQ](../connection-manager/msmq-connection-manager.md)  
  
 **Mensaje**  
 Especifique si la tarea Cola de mensajes envía o recibe mensajes. Si selecciona **Enviar mensaje**, la página Enviar se agrega a la lista del panel izquierdo del cuadro de diálogo. Si selecciona **Recibir mensaje**, se agrega la página Recibir. De forma predeterminada, este valor está establecido en **Enviar mensaje**.  
  
## <a name="message-queue-task-editor-send-page"></a>Editor de la tarea Cola de mensajes (página Enviar)
  Utilice la página **Enviar** del cuadro de diálogo **Editor de la tarea Cola de mensajes** para configurar una tarea Cola de mensajes para enviar mensajes desde un paquete de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
### <a name="options"></a>Opciones  
 **UseEncryption**  
 Indica si se debe cifrar el mensaje. El valor predeterminado es **False**.  
  
 **EncryptionAlgorithm**  
 Si opta por usar cifrado, especifique el nombre del algoritmo de cifrado que debe utilizarse. La tarea Cola de mensajes puede utilizar los algoritmos RC2 y RC4. El valor predeterminado es **RC2**.  
  
> [!NOTE]  
>  El algoritmo RC4 se admite únicamente por razones de compatibilidad con versiones anteriores. El material nuevo solo se puede cifrar con RC4 o RC4_128 cuando la base de datos tenga el nivel de compatibilidad 90 o 100. (No se recomienda). Use un algoritmo más reciente como uno de los algoritmos AES en su lugar. En la versión actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], el material cifrado con RC4 o RC4_128 se puede descifrar en cualquier nivel de compatibilidad.  
  
> [!IMPORTANT]  
>  Éstos son los algoritmos de cifrado que admite la tecnología de Message Queue Server (que también recibe el nombre de MSMQ). Ambos algoritmos de cifrado se consideran en estos momentos criptográficamente menos seguros que otros algoritmos más recientes con los que Message Queue Server aún no es compatible. Por tanto, debe considerar con detenimiento sus necesidades criptográficas a la hora de enviar mensajes con la tarea Cola de mensajes.  
  
 **MessageType**  
 Seleccione el tipo de mensaje. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Mensaje de archivo de datos**|El mensaje se almacena en un archivo. Al seleccionar este valor se muestra la opción dinámica **DataFileMessage**.|  
|**Mensaje de variable**|El mensaje se almacena en una variable. Al seleccionar este valor se muestra la opción dinámica **VariableMessage**.|  
|**Mensaje de cadena**|El mensaje se almacena en la tarea Cola de mensajes. Al seleccionar este valor se muestra la opción dinámica **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Opciones dinámicas de MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Mensaje de archivo de datos  
 **DataFileMessage**  
 Escriba la ruta de acceso del archivo de datos, o bien haga clic en el botón de puntos suspensivos **(…)** y busque el archivo.  
  
#### <a name="messagetype--variable-message"></a>MessageType = Mensaje de variable  
 **VariableMessage**  
 Escriba los nombres de variable, o bien haga clic en el botón de puntos suspensivos **(…)** y seleccione las variables. Las variables se separan con comas.  
  
 **Temas relacionados:** Seleccionar variables  
  
#### <a name="messagetype--string-message"></a>MessageType = Mensaje de cadena  
 **StringMessage**  
 Escriba el mensaje de cadena, o bien haga clic en el botón de puntos suspensivos **(…)** y escriba el mensaje en el cuadro de diálogo **Escribir mensaje de cadena**.  
  
## <a name="message-queue-task-editor-receive-page"></a>Editor de la tarea Cola de mensajes (página Recibir)
  Use la página **Recibir** del cuadro de diálogo **Editor de la tarea Cola de mensajes** para configurar una tarea de la cola de mensajes para recibir mensajes de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Message Queuing (MSMQ).  
  
### <a name="options"></a>Opciones  
 **RemoveFromMessageQueue**  
 Indique si desea eliminar el mensaje de la cola una vez recibido. De manera predeterminada, este valor se establece en **False**.  
  
 **ErrorIfMessageTimeOut**  
 Indique si desea mostrar un mensaje de error si se produce un error en la tarea al exceder el tiempo de espera. El valor predeterminado es **False**.  
  
 **TimeoutAfter**  
 Si elige mostrar un mensaje de error al producirse errores en la tarea, indique el número de segundos que deben transcurrir antes de mostrar el mensaje de tiempo de espera.  
  
 **MessageType**  
 Seleccione el tipo de mensaje. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Mensaje de archivo de datos**|El mensaje se almacena en un archivo. Al seleccionar este valor se muestra la opción dinámica **DataFileMessage**.|  
|**Mensaje de variable**|El mensaje se almacena en una variable. Al seleccionar este valor se muestra la opción dinámica **VariableMessage**.|  
|**Mensaje de cadena**|El mensaje se almacena en la tarea Cola de mensajes. Al seleccionar este valor se muestra la opción dinámica **StringMessage**.|  
|**Mensaje de cadena para variable**|El mensaje.<br /><br /> Al seleccionar este valor se muestra la opción dinámica **StringMessage**.|  
  
### <a name="messagetype-dynamic-options"></a>Opciones dinámicas de MessageType  
  
#### <a name="messagetype--data-file-message"></a>MessageType = Mensaje de archivo de datos  
 **SaveFileAs**  
 Escriba la ruta del archivo que quiere usar, o bien haga clic en el botón de puntos suspensivos **(…)** para buscar el archivo.  
  
 **Sobrescribir**  
 Indique si desea sobrescribir los datos de un archivo existente al guardar el contenido del mensaje de archivo de datos. El valor predeterminado es **False**.  
  
 **Filter**  
 Indique si desea aplicar un filtro al mensaje. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Sin filtro**|La tarea no filtra mensajes. Al seleccionar este valor se muestra la opción dinámica **IdentifierReadOnly**.|  
|**De paquete**|El mensaje solo recibe mensajes del paquete seleccionado. Al seleccionar este valor se muestra la opción dinámica **Identifier**.|  
  
#### <a name="filter-dynamic-options"></a>Opciones dinámicas de Filtro  
  
##### <a name="filter--no-filter"></a>Filtro = Sin filtro  
 **IdentifierReadOnly**  
 Esta opción es de solo lectura. Podría estar en blanco o contener el GUID de un paquete si se ha establecido anteriormente la propiedad Filtro.  
  
##### <a name="filter--from-package"></a>Filtro = De paquete  
 **Identificador**  
 Si decide aplicar un filtro, escriba el identificador único del paquete del que se recibirán los mensajes, o bien haga clic en el botón de puntos suspensivos **(…)** y, después, especifique el paquete.  
  
 **Temas relacionados:** [Seleccionar un paquete](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--variable-message"></a>MessageType = Mensaje de variable  
 **Filter**  
 Especifica si se desea aplicar un filtro a los mensajes. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Sin filtro**|La tarea no filtra mensajes. Al seleccionar este valor se muestra la opción dinámica **IdentifierReadOnly**.|  
|**De paquete**|El mensaje solo recibe mensajes del paquete seleccionado. Al seleccionar este valor se muestra la opción dinámica **Identifier**.|  
  
 **Variable**  
 Escriba el nombre de la variable o haga clic en \<**New variable...**> para configurar una nueva variable.  
  
 **Temas relacionados:** [Agregar variable](../integration-services-ssis-variables.md)  
  
#### <a name="filter-dynamic-options"></a>Opciones dinámicas de Filtro  
  
##### <a name="filter--no-filter"></a>Filtro = Sin filtro  
 **IdentifierReadOnly**  
 Esta opción está en blanco.  
  
##### <a name="filter--from-package"></a>Filtro = De paquete  
 **Identificador**  
 Si decide aplicar un filtro, escriba el identificador único del paquete del que se recibirán los mensajes, o bien haga clic en el botón de puntos suspensivos **(…)** y, después, especifique el paquete.  
  
 **Temas relacionados:** [Seleccionar un paquete](../../integration-services/control-flow/select-a-package.md)  
  
#### <a name="messagetype--string-message"></a>MessageType = Mensaje de cadena  
 **Comparar**  
 Especifica si se desea aplicar un filtro a los mensajes. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**None**|Los mensajes no se comparan.|  
|**Coincidencia exacta**|Los mensajes deben coincidir exactamente con la cadena de la opción **CompareString** .|  
|**Omitir mayúsculas y minúsculas**|El mensaje debe coincidir con la cadena de la opción **CompareString** , pero la comparación no distingue mayúsculas de minúsculas.|  
|**Que contenga**|El mensaje debe contener la cadena de la opción **CompareString** .|  
  
 **CompareString**  
 A menos que la opción **Comparar** se haya establecido en **Ninguno**, deberá indicar la cadena con la que se comparará el mensaje.  
  
#### <a name="messagetype--string-message-to-variable"></a>MessageType = Mensaje de cadena para variable  
 **Comparar**  
 Especifica si se desea aplicar un filtro a los mensajes. Esta propiedad presenta las opciones indicadas en la siguiente tabla.  
  
|Value|Descripción|  
|-----------|-----------------|  
|**None**|Los mensajes no se comparan.|  
|**Coincidencia exacta**|Los mensajes deben coincidir exactamente con la cadena de la opción **CompareString** .|  
|**Omitir mayúsculas y minúsculas**|El mensaje debe coincidir con la cadena de la opción **CompareString** , pero la comparación no distingue mayúsculas de minúsculas.|  
|**Que contenga**|El mensaje debe contener la cadena de la opción **CompareString** .|  
  
 **CompareString**  
 A menos que la opción **Comparar** se haya establecido en **Ninguno**, deberá indicar la cadena con la que se comparará el mensaje.  
  
 **Variable**  
 Escriba el nombre de la variable para conservar el mensaje recibido, o haga clic en \<**New variable...**> y, después, configure una nueva variable.  
  
 **Temas relacionados:** [Agregar variable](../integration-services-ssis-variables.md)  
  
## <a name="select-variables"></a>Seleccionar variables
  Utilice el cuadro de diálogo **Seleccionar variables** para especificar las variables que se van a utilizar en una operación de envío de mensaje en la tarea Cola de mensajes. La lista **Variables disponibles** incluye las variables definidas por el usuario y de sistema que se encuentran en el ámbito de la tarea Cola de mensaje o en su contenedor principal. La tarea utiliza las variables de la lista **Variables seleccionadas** .  
  
### <a name="options"></a>Opciones  
 **Variables disponibles**  
 Seleccione una o más variables.  
  
 **Variables seleccionadas**  
 Seleccione una o más variables.  
  
 **Flechas derechas**  
 Mueva las variables seleccionadas a la lista **Variables seleccionadas** .  
  
 **Flechas izquierdas**  
 Mueva las variables seleccionadas nuevamente a la lista **Variables disponibles** .  
  
 **Nueva variable**  
 Cree una nueva variable.  
  
 **Temas relacionados:** [Agregar variable](../integration-services-ssis-variables.md)  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
