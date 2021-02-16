---
description: Tarea Ejecutar proceso
title: Tarea Ejecutar proceso | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executeprocesstask.f1
- sql13.dts.designer.executeprocesstask.general.f1
- sql13.dts.designer.executeprocesstask.process.f1
helpviewer_keywords:
- Execute Process task [Integration Services]
ms.assetid: aca5a0b5-34a9-45bc-a234-8e63ea51a1ee
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 98409c2a75cc76589a705579ff02f1be34c9c7b2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352111"
---
# <a name="execute-process-task"></a>Tarea Ejecutar proceso

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Ejecutar proceso ejecuta una aplicación o un archivo por lotes como parte de un flujo de trabajo de paquete de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Aunque puede utilizar la tarea Ejecutar proceso para abrir cualquier aplicación estándar, como [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] o [!INCLUDE[ofprword](../../includes/ofprword-md.md)], normalmente la utilizará para ejecutar aplicaciones empresariales o archivos por lotes que trabajen con un origen de datos. Por ejemplo, puede utilizar la tarea Ejecutar proceso para expandir un archivo de texto comprimido. Una vez hecho esto, el paquete puede usar el archivo de texto como origen de datos para el flujo de datos. Otro ejemplo sería utilizar la tarea Ejecutar proceso para ejecutar una aplicación de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] personalizada que genera un informe de ventas diario. Se puede adjuntar este informe a una tarea Enviar correo para reenviarlo a una lista de distribución.  
  
 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] incluye otras tareas que realizan operaciones de flujo de trabajo, como ejecutar paquetes. Para obtener más información, consulte [Execute Package Task](../../integration-services/control-flow/execute-package-task.md).  
  
## <a name="custom-log-entries-available-on-the-execute-process-task"></a>Entradas del registro personalizadas disponibles en la tarea Ejecutar proceso  
 La siguiente tabla contiene las entradas del registro personalizadas para la tarea Ejecutar proceso. Para obtener más información, vea [Registro de Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrada del registro|Descripción|  
|---------------|-----------------|  
|**ExecuteProcessExecutingProcess**|Proporciona información sobre el proceso que se configuró para que ejecute la tarea.<br /><br /> Se escriben dos entradas del registro. Una entrada contiene información sobre el nombre y la ubicación del ejecutable que ejecuta la tarea, y la otra entrada registra la salida del ejecutable.|  
|**ExecuteProcessVariableRouting**|Proporciona información acerca de las variables que se enrutan a la entrada y las salidas del ejecutable. Se escriben entradas del registro para stdin (la entrada), stdout (la salida) y stderr (la salida de errores).|  
  
## <a name="configuration-of-the-execute-process-task"></a>Configuración de la tarea Ejecutar proceso  
 Puede establecer propiedades a través del Diseñador de [!INCLUDE[ssIS](../../includes/ssis-md.md)] o mediante programación.  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
### <a name="property-settings"></a>Valores de propiedades  
 Cuando la tarea Ejecutar proceso ejecuta una aplicación personalizada, la tarea proporciona la entrada para la aplicación por medio de los métodos siguientes:  
  
-   Una variable que se especifica en la configuración de la propiedad **StandardInputVariable** . Para más información, vea [Variables de Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md) y [Usar variables en paquetes](../integration-services-ssis-variables.md).  
  
-   Un argumento que se especifica en la configuración de la propiedad **Arguments** . Por ejemplo, si la tarea abre un documento en Word, el argumento puede asignar un nombre al archivo .doc.  
  
 Cuando se pasan varios argumentos a una aplicación personalizada en una tarea Ejecutar proceso, se usan espacios para separarlos. Los argumentos no pueden incluir espacios; de lo contrario, la tarea no se ejecutará. Puede usar una expresión para pasar un valor de variable como argumento. En el ejemplo siguiente, la expresión pasa dos valores de variable como argumentos y usa un espacio para delimitar los argumentos:  
  
 `@variable1 + " " + @variable2`  
  
 Puede usar una expresión para establecer diversas propiedades de la tarea Ejecutar proceso.  
  
 Cuando use la propiedad **StandardInputVariable** para configurar la tarea Ejecutar proceso para proporcionar la entrada, llame al método **Console.ReadLine** desde la aplicación para leer la entrada. Para obtener más información, vea el tema [Console.ReadLine (Método)](/dotnet/api/system.console.readline), en la Biblioteca de clases de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 Cuando use la propiedad **Argumentos** para configurar la tarea Ejecutar proceso con el fin de proporcionar la entrada, realice uno de los pasos siguientes para obtener los argumentos:  
  
-   Si usa Microsoft Visual Basic para escribir la aplicación, establezca la propiedad **My.Application.CommandLineArgs** . En el ejemplo siguiente se establece la propiedad **My.Application.CommandLineArgs** para recuperar dos argumentos:  
  
    ```vb  
    Dim variable1 As String = My.Application.CommandLineArgs.Item(0)  
    Dim variable2 As String = My.Application.CommandLineArgs.Item(1)   
    ```  
  
     Para obtener más información, vea el tema [My.Application.CommandLineArgs (Propiedad)](https://go.microsoft.com/fwlink/?LinkId=129200)en la Referencia del lenguaje [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] .  
  
-   Si escribe la aplicación en Microsoft Visual C#, use el método **Main**.  
  
     Para más información, vea el tema [Argumentos de la línea de comandos (Guía de programación de C#)](/dotnet/csharp/programming-guide/main-and-command-args/command-line-arguments)en la Guía de programación de C#.  
  
 La tarea Ejecutar proceso también incluye las propiedades **StandardOutputVariable** y **StandardErrorVariable** para especificar las variables que usan la salida estándar y la salida de errores de la aplicación, respectivamente.  
  
 Además, puede configurar la tarea Ejecutar proceso para especificar un directorio de trabajo, un período de tiempo de espera o un valor que indique que el ejecutable se ejecutó correctamente. La tarea también se puede configurar para que no se complete su ejecución si el código de retorno del ejecutable no coincide con el valor que indica que se ejecutó correctamente o si el ejecutable no se encuentra en la ubicación especificada.  
  
## <a name="programmatic-configuration-of-the-execute-process-task"></a>Configuración mediante programación de la tarea Ejecutar proceso  
 Para obtener más información sobre cómo establecer estas propiedades mediante programación, haga clic en el tema siguiente:  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.ExecuteProcess.ExecuteProcess>  
  
## <a name="execute-process-task-editor-general-page"></a>Editor de la tarea Ejecutar proceso (página General)
  Use la página **General** del cuadro de diálogo **Editor de la tarea Ejecutar proceso** para asignar un nombre a la tarea Ejecutar proceso y describirla.  
  
### <a name="options"></a>Opciones  
 **Nombre**  
 Proporcione un nombre único para la tarea Ejecutar proceso. Este nombre se utiliza como etiqueta en el icono de tarea.  
  
> [!NOTE]  
>  Los nombres de tarea deben ser únicos en un paquete.  
  
 **Descripción**  
 Escriba una descripción de la tarea Ejecutar proceso.  
  
## <a name="execute-process-task-editor-process-page"></a>Editor de la tarea Ejecutar proceso (página Procesar)
  Utilice la página **Proceso** del cuadro de diálogo **Editor de la tarea Ejecutar proceso** para configurar las opciones que permitirán ejecutar el proceso. Estas opciones incluyen el ejecutable que se debe instalar, su ubicación, los argumentos de línea de comandos y las variables que proporcionan información de entrada y capturan información de salida.  
  
### <a name="options"></a>Opciones  
 **RequireFullFileName**  
 Indica si la tarea debe generar un error en caso de que no se encuentre el ejecutable en la ubicación específica.  
  
 **Executable**  
 Escriba el nombre del ejecutable que desea instalar.  
  
 **Argumentos**  
 Proporcione los argumentos de línea de comandos.  
  
 **WorkingDirectory**  
 Escriba la ruta de acceso de la carpeta que contiene el ejecutable, o bien haga clic en el botón Examinar **(…)** y busque la carpeta.  
  
 **StandardInputVariable**  
 Seleccione una variable para proporcionar la entrada al proceso o haga clic en \<**New variable...**> para crear una:  
  
 **Temas relacionados:** [Agregar variable](../integration-services-ssis-variables.md)  
  
 **StandardOutputVariable**  
 Seleccione una variable para capturar la salida del proceso o haga clic en \<**New variable...**> para crear una.  
  
 **StandardErrorVariable**  
 Seleccione una variable para capturar la salida del error del procesador o haga clic en \<**New variable...**> para crear una.  
  
 **FailTaskIfReturnCodeIsNotSuccessValue**  
 Esta opción indica si la tarea genera un error porque el código de salida del proceso es diferente del valor especificado en **SuccessValue**.  
  
 **SuccessValue**  
 Especifique el valor que devuelve el ejecutable para indicar su instalación correcta. De forma predeterminada, este valor está establecido en **0**.  
  
 **TimeOut**  
 Especifique el número de segundos que el proceso puede ejecutarse. El valor **0** indica que no se usará ningún valor de tiempo de espera y que el proceso se ejecutará hasta su conclusión o hasta que se produzca un error.  
  
 **TerminateProcessAfterTimeOut**  
 Esta opción indica si el proceso está obligado a finalizar después del período de tiempo de espera especificado con la opción **TimeOut** . Esta opción solo está disponible si el valor de **TimeOut** es distinto de **0**.  
  
 **WindowStyle**  
 Especifique el estilo de ventana en el que ejecutará el proceso.  
  
## <a name="see-also"></a>Consulte también  
 [Tareas de Integration Services](../../integration-services/control-flow/integration-services-tasks.md)   
 [Flujo de control](../../integration-services/control-flow/control-flow.md)  
  
