---
description: Codificar y depurar la tarea Script
title: Programar y depurar la tarea Script | Microsoft Docs
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
- Script task [Integration Services], debugging
- SSIS Script task, development environment
- SSIS Script task, debugging
- Script task [Integration Services], development environment
- Script task [Integration Services], coding
- debugging [Integration Services], scripts
- VSTA
- SSIS Script task, coding
ms.assetid: 687c262f-fcab-42e8-92ae-e956f3d92d69
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 6f5cc9132e7a8f67d2c4636eeaeaa0cb7db5ca0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351587"
---
# <a name="coding-and-debugging-the-script-task"></a>Codificar y depurar la tarea Script

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Después de configurar la tarea Script en el **Editor de la tarea Script**, puede escribir el código personalizado en el entorno de desarrollo de la tarea Script.  
  
## <a name="script-task-development-environment"></a>Entorno de desarrollo de la tarea Script  
 La tarea Script usa [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] Tools for Applications (VSTA) como el entorno de desarrollo del propio script.  
  
 El código de script se escribe en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic o [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual C#. Puede especificar el lenguaje de script estableciendo la propiedad **ScriptLanguage** en el **Editor de la tarea Script**. Si prefiere utilizar otro lenguaje de programación, puede desarrollar un ensamblado personalizado en el lenguaje seleccionado y utilizar su funcionalidad en el código de la tarea Script.  
  
 El script que crea en la tarea Script se almacena en la definición de paquete. No hay un archivo de script independiente. Por consiguiente, el uso de la tarea Script no afecta a la implementación del paquete.  
  
> [!NOTE]  
>  Al diseñar el paquete y depurar el script, el código de script se escribe temporalmente en un archivo de proyecto. Dado que almacenar información confidencial en un archivo supone un riesgo potencial para la seguridad, recomendamos que no incluya información confidencial como contraseñas en el código de script.  
  
 De forma predeterminada, la instrucción **Option Strict** está deshabilitada en el IDE.  
  
## <a name="script-task-project-structure"></a>Estructura del proyecto de la tarea Script  
 Al crear o modificar el script contenido en una tarea Script, VSTA abre un proyecto nuevo vacío o vuelve a abrir el proyecto existente. La creación de este proyecto VSTA no afecta a la implementación del paquete, porque el proyecto está guardado en el archivo empaquetado; la tarea Script no crea archivos adicionales.  
  
### <a name="project-items-and-classes-in-the-script-task-project"></a>Elementos y clases de proyecto en el proyecto de la tarea Script  
 De forma predeterminada, el proyecto de la tarea Script mostrado en la ventana Explorador de proyectos de VSTA contiene un elemento único, **ScriptMain**. El elemento **ScriptMain**, a su vez, contiene una clase única, también denominada **ScriptMain**. Los elementos de código de la clase varían, dependiendo del lenguaje de programación seleccionado para la tarea Script:  
  
-   Cuando la tarea Script se configura para el lenguaje de programación [!INCLUDE[vb_orcas_long](../../../includes/vb-orcas-long-md.md)], la clase **ScriptMain** tiene una subrutina pública, **Main**. La subrutina **ScriptMain.Main** es el método al que el entorno en tiempo de ejecución llama cuando ejecuta la tarea Script.  
  
     De forma predeterminada, el único código de la subrutina **Main** de un script nuevo es la línea `Dts.TaskResult = ScriptResults.Success`. Esta línea informa al módulo ejecutable que la tarea se realizó correctamente en su operación. La propiedad **Dts.TaskResult** se describe en [Devolver los resultados de la tarea Script](../../../integration-services/extending-packages-scripting/task/returning-results-from-the-script-task.md).  
  
-   Cuando la tarea Script está configurada para el lenguaje de programación Visual C#, la clase **ScriptMain** tiene un método público, **Main**. Se llama al método cuando se ejecuta la tarea Script.  
  
     De manera predeterminada, el método **Main** incluye la línea `Dts.TaskResult = (int)ScriptResults.Success`. Esta línea informa al módulo ejecutable que la tarea se realizó correctamente en su operación.  
  
 El elemento **ScriptMain** puede contener clases distintas de la clase **ScriptMain**. Las clases solo están disponibles en la tarea Script en la que residen.  
  
 De forma predeterminada, el elemento de proyecto **ScriptMain** contiene el código siguiente generado automáticamente. La plantilla de código también proporciona información general sobre la tarea Script, así como información adicional sobre cómo recuperar y manipular objetos SSIS, como variables, eventos y conexiones.  
  
```vb  
' Microsoft SQL Server Integration Services Script Task  
' Write scripts using Microsoft Visual Basic 2008.  
' The ScriptMain is the entry point class of the script.  
  
Imports System  
Imports System.Data  
Imports System.Math  
Imports Microsoft.SqlServer.Dts.Runtime.VSTAProxy  
  
<System.AddIn.AddIn("ScriptMain", Version:="1.0", Publisher:="", Description:="")> _  
Partial Class ScriptMain  
  
Private Sub ScriptMain_Startup(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Startup  
  
End Sub  
  
Private Sub ScriptMain_Shutdown(ByVal sender As Object, ByVal e As System.EventArgs) Handles Me.Shutdown  
Try  
' Unlock variables from the read-only and read-write variable collection properties  
If (Dts.Variables.Count <> 0) Then  
Dts.Variables.Unlock()  
End If  
Catch ex As Exception  
        End Try  
End Sub  
  
Enum ScriptResults  
Success = DTSExecResult.Success  
Failure = DTSExecResult.Failure  
End Enum  
  
' The execution engine calls this method when the task executes.  
' To access the object model, use the Dts property. Connections, variables, events,  
' and logging features are available as members of the Dts property as shown in the following examples.  
'  
' To reference a variable, call Dts.Variables("MyCaseSensitiveVariableName").Value  
' To post a log entry, call Dts.Log("This is my log text", 999, Nothing)  
' To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, True)  
'  
' To use the connections collection use something like the following:  
' ConnectionManager cm = Dts.Connections.Add("OLEDB")  
' cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;"  
'  
' Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
'   
' To open Help, press F1.  
  
Public Sub Main()  
'  
' Add your code here  
'  
Dts.TaskResult = ScriptResults.Success  
End Sub  
  
End Class  
```  
  
```csharp  
/*  
   Microsoft SQL Server Integration Services Script Task  
   Write scripts using Microsoft Visual C# 2008.  
   The ScriptMain is the entry point class of the script.  
*/  
  
using System;  
using System.Data;  
using Microsoft.SqlServer.Dts.Runtime.VSTAProxy;  
using System.Windows.Forms;  
  
namespace ST_1bcfdbad36d94f8ba9f23a10375abe53.csproj  
{  
    [System.AddIn.AddIn("ScriptMain", Version = "1.0", Publisher = "", Description = "")]  
    public partial class ScriptMain  
    {  
        private void ScriptMain_Startup(object sender, EventArgs e)  
        {  
  
        }  
  
        private void ScriptMain_Shutdown(object sender, EventArgs e)  
        {  
            try  
            {  
                // Unlock variables from the read-only and read-write variable collection properties  
                if (Dts.Variables.Count != 0)  
                {  
                    Dts.Variables.Unlock();  
                }  
            }  
            catch  
            {  
            }  
        }  
  
        #region VSTA generated code  
        private void InternalStartup()  
        {  
            this.Startup += new System.EventHandler(ScriptMain_Startup);  
            this.Shutdown += new System.EventHandler(ScriptMain_Shutdown);  
        }  
        enum ScriptResults  
        {  
            Success = DTSExecResult.Success,  
            Failure = DTSExecResult.Failure  
        };  
  
        #endregion  
  
        /*  
The execution engine calls this method when the task executes.  
To access the object model, use the Dts property. Connections, variables, events,  
and logging features are available as members of the Dts property as shown in the following examples.  
  
To reference a variable, call Dts.Variables["MyCaseSensitiveVariableName"].Value;  
To post a log entry, call Dts.Log("This is my log text", 999, null);  
To fire an event, call Dts.Events.FireInformation(99, "test", "hit the help message", "", 0, true);  
  
To use the connections collection use something like the following:  
ConnectionManager cm = Dts.Connections.Add("OLEDB");  
cm.ConnectionString = "Data Source=localhost;Initial Catalog=AdventureWorks;Provider=SQLNCLI10;Integrated Security=SSPI;Auto Translate=False;";  
  
Before returning from this method, set the value of Dts.TaskResult to indicate success or failure.  
  
To open Help, press F1.  
*/  
  
        public void Main()  
        {  
            // TODO: Add your code here  
            Dts.TaskResult = (int)ScriptResults.Success;  
        }  
    }  
```  
  
### <a name="additional-project-items-in-the-script-task-project"></a>Elementos de proyecto adicionales del proyecto de la tarea Script  
 El proyecto de la tarea Script puede incluir elementos distintos del elemento **ScriptMain** predeterminado. Puede agregar clases, módulos y archivos de código al proyecto. También puede utilizar las carpetas para organizar los grupos de elementos. Todos los elementos que se agregan se conservan dentro del paquete.  
  
### <a name="references-in-the-script-task-project"></a>Referencias en el proyecto de la tarea Script  
 Para agregar referencias a los ensamblados administrados, haga clic con el botón derecho en el proyecto de la tarea Script en el **Explorador de proyectos** y, después, haga clic en **Agregar referencia**. Para obtener más información, consulte [Hacer referencia a otros ensamblados en soluciones de scripting](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md).  
  
> [!NOTE]  
>  Puede ver las referencias de proyecto en el IDE de VSTA en la **Vista de clases** o en el **Explorador de proyectos**. Puede abrir cualquiera de estas ventanas en el menú **Ver**. Puede agregar una referencia nueva en el menú **Proyecto**, en el **Explorador de proyectos** o en **Vista de clases**.  
  
## <a name="interacting-with-the-package-in-the-script-task"></a>Interactuar con el paquete de la tarea Script  
 La tarea Script utiliza el objeto **Dts** global, que es una instancia de la clase <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> y sus miembros para interactuar con el paquete contenedor y con el módulo ejecutable de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)].  
  
 En la tabla siguiente se enumeran los miembros públicos principales de la clase <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel>, que se expone al código de la tarea Script a través del objeto **Dts** global. Los temas de esta sección describen con más detalle el uso de estos miembros.  
  
|Member|Propósito|  
|------------|-------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Connections%2A>|Proporciona acceso a los administradores de conexión definidos en el paquete.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Events%2A>|Proporciona una interfaz de eventos que permite a la tarea Script generar errores, advertencias y mensajes informativos.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.ExecutionValue%2A>|Proporciona una manera simple de devolver un objeto único al módulo ejecutable (además de **TaskResult**) que también se puede utilizar para la bifurcación del flujo de trabajo.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Log%2A>|Registra información como el progreso y los resultados de las tareas en proveedores de registro habilitados.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.TaskResult%2A>|Informa del éxito o error de la tarea.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Transaction%2A>|Proporciona la transacción, si hay alguna, dentro de la que el contenedor de la tarea se está ejecutando.|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>|Proporciona acceso a las variables enumeradas en las propiedades de tarea **ReadOnlyVariables** y **ReadWriteVariables** para su uso en el script.|  
  
 La clase <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel> contiene también algunos miembros públicos que probablemente no utilizará.  
  
|Member|Descripción|  
|------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>|La propiedad <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A> proporciona un acceso más cómodo a las variables. Aunque puede utilizar <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.VariableDispenser%2A>, debe llamar explícitamente a los métodos para bloquear y desbloquear las variables para leer y escribir. La tarea Script controla la semántica de bloqueo automáticamente cuando usa la propiedad <xref:Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptObjectModel.Variables%2A>.|  
  
## <a name="debugging-the-script-task"></a>Depurar la tarea Script  
 Para depurar el código de la tarea Script, establezca al menos un punto de interrupción en el código y, a continuación, cierre el IDE de VSTA para ejecutar el paquete en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. Cuando la ejecución del paquete entra en la tarea Script, el IDE de VSTA se vuelve a abrir y muestra el código en modo de solo lectura. Después de que la ejecución llega al punto de interrupción, puede examinar los valores de variable y completar el código restante.  
  
> [!WARNING]  
>  No se puede depurar la tarea Script al ejecutar el paquete en modo de 64 bits.  
  
> [!NOTE]  
>  Debe ejecutar el paquete para depurar en la tarea Script. Si ejecuta solamente la tarea individual, se pasan por alto los puntos de interrupción del código de la tarea Script.  
  
> [!NOTE]  
>  No puede depurar una tarea Script al ejecutar esta última como parte de un paquete secundario que se ejecuta desde una tarea Ejecutar paquete. Los puntos de interrupción establecidos en la tarea Script del paquete secundario se descartan en estas circunstancias. Puede depurar el paquete secundario ejecutándolo normalmente por separado.  
  
> [!NOTE]  
>  Al depurar un paquete que contiene varias tareas Script, el depurador depura una tarea Script. El sistema puede depurar otra tarea Script si el depurador se completa, como en el caso de un bucle Foreach o un contenedor de bucles For.  
  
## <a name="external-resources"></a>Recursos externos  
  
-   Entrada de blog, [VSTA setup and configuration troubles for SSIS 2008 and R2 installations](/archive/blogs/jason_howell/vsta-setup-and-configuration-troubles-for-ssis-2008-and-r2-installations) (Problemas de instalación y configuración de VSTA en instalaciones de SSIS 2008 y R2), en blogs.msdn.com.  
  
## <a name="see-also"></a>Consulte también  
 [Hacer referencia a otros ensamblados en soluciones de scripting](../../../integration-services/extending-packages-scripting/referencing-other-assemblies-in-scripting-solutions.md)   
 [Configurar la tarea Script en el editor de la tarea Script](../../../integration-services/extending-packages-scripting/task/configuring-the-script-task-in-the-script-task-editor.md)  
  
