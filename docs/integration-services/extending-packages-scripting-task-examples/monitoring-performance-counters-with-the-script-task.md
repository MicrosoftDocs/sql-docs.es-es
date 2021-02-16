---
description: Supervisar los contadores de rendimiento con la tarea Script
title: Supervisar los contadores de rendimiento con la tarea Script | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- performance counters [Integration Services]
- SSIS Script task, performance counters
- custom performance counters [Integration Services]
- Script task [Integration Services], examples
- Script task [Integration Services], performance counters
- counters [Integration Services]
ms.assetid: 86609bf1-cae6-435e-a58d-41bdfc521e94
author: chugugrace
ms.author: chugu
ms.openlocfilehash: b05cfaa612a9e08fb503ee9d4448f871d9cd7904
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344847"
---
# <a name="monitoring-performance-counters-with-the-script-task"></a>Supervisar los contadores de rendimiento con la tarea Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los administradores quizá tengan que supervisar el rendimiento de los paquetes de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] que realizan transformaciones complejas en grandes volúmenes de datos. El espacio de nombres **System.Diagnostics** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] proporciona las clases para utilizar los contadores de rendimiento existentes y para crear sus propios contadores de rendimiento.  
  
 Los contadores de rendimiento almacenan información sobre el rendimiento de la aplicación que puede utilizar para analizar el rendimiento del software a lo largo del tiempo. Los contadores de rendimiento se pueden supervisar de forma local o remota mediante la herramienta **Monitor de rendimiento**. Puede almacenar los valores de los contadores de rendimiento en variables para la bifurcación del flujo de control posterior en el paquete.  
  
 Como alternativa a utilizar los contadores de rendimiento, puede provocar el evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> a través de la propiedad <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A> del objeto **Dts**. El evento <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireProgress%2A> devuelve el progreso incremental e información del porcentaje completado al módulo ejecutable de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
> [!NOTE]  
>  Si desea crear una tarea que pueda reutilizar más fácilmente en varios paquetes, considere la posibilidad de utilizar el código de este ejemplo de tarea Script como punto inicial de una tarea personalizada. Para más información, vea [Desarrollar una tarea personalizada](../../integration-services/extending-packages-custom-objects/task/developing-a-custom-task.md).  
  
## <a name="description"></a>Descripción  
 En el ejemplo siguiente se crea un contador de rendimiento personalizado y se incrementa el contador. Primero, el ejemplo determina si el contador de rendimiento ya existe. Si no se ha creado el contador de rendimiento, el script llama al método **Create** del objeto **PerformanceCounterCategory** para crearlo. Una vez creado el contador de rendimiento, el script incrementa el contador. Finalmente, el ejemplo sigue el procedimiento recomendado de llamar al método **Close** en el contador de rendimiento cuando ya no se necesita.  
  
> [!NOTE]  
>  Crear una nueva categoría de contador de rendimiento y el contador de rendimiento requiere derechos administrativos. Además, la nueva categoría y el contador se conservan en el equipo después de la creación.  
  
#### <a name="to-configure-this-script-task-example"></a>Para configurar este ejemplo de tarea Script  
  
-   Use una instrucción **Imports** en el código para importar el espacio de nombres **System.Diagnostics**.  
  
### <a name="example-code"></a>Código de ejemplo  
  
```vb  
Public Sub Main()  
  
    Dim myCounter As PerformanceCounter  
  
    Try  
        'Create the performance counter if it does not already exist.  
        If Not _  
        PerformanceCounterCategory.Exists("TaskExample") Then  
            PerformanceCounterCategory.Create("TaskExample", _  
                "Task Performance Counter Example", "Iterations", _  
                "Number of times this task has been called.")  
        End If  
  
        'Initialize the performance counter.  
        myCounter = New PerformanceCounter("TaskExample", _  
            "Iterations", String.Empty, False)  
  
        'Increment the performance counter.  
        myCounter.Increment()  
  
         myCounter.Close()  
        Dts.TaskResult = ScriptResults.Success  
    Catch ex As Exception  
        Dts.Events.FireError(0, _  
            "Task Performance Counter Example", _  
            ex.Message & ControlChars.CrLf & ex.StackTrace, _  
            String.Empty, 0)  
        Dts.TaskResult = ScriptResults.Failure  
    End Try  
  
End Sub  
```  
  
```csharp  
  
public class ScriptMain  
{  
  
public void Main()  
        {  
  
            PerformanceCounter myCounter;  
  
            try  
            {  
                //Create the performance counter if it does not already exist.  
                if (!PerformanceCounterCategory.Exists("TaskExample"))  
                {  
                    PerformanceCounterCategory.Create("TaskExample", "Task Performance Counter Example", "Iterations", "Number of times this task has been called.");  
                }  
  
                //Initialize the performance counter.  
                myCounter = new PerformanceCounter("TaskExample", "Iterations", String.Empty, false);  
  
                //Increment the performance counter.  
                myCounter.Increment();  
  
                myCounter.Close();  
                Dts.TaskResult = (int)ScriptResults.Success;  
            }  
            catch (Exception ex)  
            {  
                Dts.Events.FireError(0, "Task Performance Counter Example", ex.Message + "\r" + ex.StackTrace, String.Empty, 0);  
                Dts.TaskResult = (int)ScriptResults.Failure;  
            }  
  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
  
```  
