---
description: Desarrollar una interfaz de usuario para una tarea personalizada
title: Desarrollar una interfaz de usuario para una tarea personalizada | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom user interfaces [Integration Services]
- IDtsTaskUI interface
- DtsTaskAttribute attribute
- custom tasks [Integration Services], user interface
- custom user interface [Integration Services], custom tasks
- user interface [Integration Services]
- SSIS custom tasks, user interface
ms.assetid: 1e940cd1-c5f8-4527-b678-e89ba5dc398a
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f0536913b55a3e798b7306f5bd2fa36d115e5f84
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337697"
---
# <a name="developing-a-user-interface-for-a-custom-task"></a>Desarrollar una interfaz de usuario para una tarea personalizada

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  El modelo de objetos de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] permite a los desarrolladores de tareas personalizadas crear con facilidad una interfaz de usuario personalizada para una tarea que posteriormente se puede integrar y mostrar en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. La interfaz de usuario puede proporcionar información útil al usuario en el Diseñador de [!INCLUDE[ssIS](../../../includes/ssis-md.md)] y guiar a los usuarios para configurar correctamente las propiedades y los valores de la tarea personalizada.  
  
 El desarrollo de una interfaz de usuario personalizada para una tarea implica la utilización de dos clases importantes. En la siguiente tabla se describen estas clases.  
  
|Clase|Descripción|  
|-----------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>|Un atributo que identifica una tarea administrada y proporciona información en tiempo de diseño a través de sus propiedades para controlar cómo el Diseñador de [!INCLUDE[ssIS](../../../includes/ssis-md.md)] muestra e interactúa con el objeto.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI>|Una interfaz que utiliza la tarea para asociar ésta a su interfaz de usuario personalizada.|  
  
 En esta sección se describe el rol del atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> y la interfaz <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> cuando se está desarrollando una interfaz de usuario para una tarea personalizada y se proporcionan detalles sobre cómo crear, integrar, implementar y depurar la tarea en el Diseñador de [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 El Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] proporciona varios puntos de entrada a la interfaz de usuario de la tarea: el usuario puede seleccionar **Editar** en el menú contextual, hacer doble clic en la tarea o hacer clic en el vínculo **Mostrar editor** en la parte inferior de la hoja de propiedades. Cuando el usuario tiene acceso a uno de estos puntos de entrada, el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] busca y carga el ensamblado que contiene la interfaz de usuario para la tarea. Ésta es responsable de la creación del cuadro de diálogo de propiedades que se muestra al usuario en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)].  
  
 Una tarea y su interfaz de usuario son entidades independientes. Se deben implementar en ensamblados independientes para reducir el trabajo de localización, implementación y mantenimiento. La DLL de la tarea no carga, llama ni generalmente contiene ningún conocimiento de su interfaz de usuario, salvo la información que se incluye en los valores de atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> codificados en la tarea. Ésta es la única manera en que se asocian una tarea y su interfaz de usuario.  
  
## <a name="the-dtstask-attribute"></a>El atributo DtsTask  
 El atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> se incluye en el código de clase de la tarea para asociar una tarea a su interfaz de usuario. El Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utiliza las propiedades del atributo para determinar cómo se muestra la tarea en el diseñador. Estas propiedades incluyen el nombre para mostrar y el icono, si existe.  
  
 En la tabla siguiente se describen las propiedades del atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute>.  
  
|Propiedad|Descripción|  
|--------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>|Muestra el nombre de tarea en el cuadro de herramientas Flujo de control.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A>|La descripción de la tarea (se hereda de <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute>). Esta propiedad se muestra en la información sobre herramientas.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A>|El icono que se muestra en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)].|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.RequiredProductLevel%2A>|Si se utiliza, establézcala en uno de los valores de la enumeración <xref:Microsoft.SqlServer.Dts.Runtime.DTSProductLevel>. Por ejemplo: `RequiredProductLevel = DTSProductLevel.None`.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskContact%2A>|Contiene información de contacto para las ocasiones en que la tarea requiere soporte técnico.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.TaskType%2A>|Asigna un tipo a la tarea.|  
|Attribute.TypeId|Cuando se implementa en una clase derivada, obtiene un identificador único para este atributo. Para obtener más información, vea la propiedad **Attribute.TypeID** en la biblioteca de clases de .NET Framework.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A>|El nombre de tipo del ensamblado que utiliza el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] para cargar el ensamblado. Esta propiedad se utiliza para buscar el ensamblado de interfaz de usuario para la tarea.|  
  
 En el ejemplo de código siguiente se muestra <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute> tal como aparecería, codificado sobre la definición de clase.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
namespace Microsoft.SSIS.Samples  
{  
  [DtsTask  
  (  
   DisplayName = "MyTask",  
   IconResource = "MyTask.MyTaskIcon.ico",  
   UITypeName = "My Custom Task," +  
   "Version=1.0.0.0," +  
   "Culture = Neutral," +  
   "PublicKeyToken = 12345abc6789de01",  
   TaskType = "PackageMaintenance",  
   TaskContact = "MyTask; company name; any other information",  
   RequiredProductLevel = DTSProductLevel.None  
   )]  
  public class MyTask : Task  
  {  
    // Your code here.  
  }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsTask(DisplayName:="MyTask", _  
 IconResource:="MyTask.MyTaskIcon.ico", _  
 UITypeName:="My Custom Task," & _  
 "Version=1.0.0.0,Culture=Neutral," & _  
 "PublicKeyToken=12345abc6789de01", _  
 TaskType:="PackageMaintenance", _  
 TaskContact:="MyTask; company name; any other information", _  
 RequiredProductLevel:=DTSProductLevel.None)> _  
Public Class MyTask  
  Inherits Task  
  
  ' Your code here.  
  
End Class 'MyTask  
```  
  
 El Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utiliza la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.UITypeName%2A> del atributo que incluye el nombre del ensamblado, nombre de tipo, versión, referencia cultural y token de clave pública, para buscar el ensamblado en la Caché de ensamblados global (GAC) y cargarlo para que lo utilice el diseñador.  
  
 Una vez encontrado el ensamblado, el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] utiliza las demás propiedades del atributo para mostrar información adicional sobre la tarea en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)], como el nombre, icono y descripción de la tarea.  
  
 Las propiedades <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.DisplayName%2A>, <xref:Microsoft.SqlServer.Dts.Runtime.Localization.DtsLocalizableAttribute.Description%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> especifican cómo se presenta la tarea al usuario. La propiedad <xref:Microsoft.SqlServer.Dts.Runtime.DtsTaskAttribute.IconResource%2A> contiene el identificador de recurso del icono incrustado en el ensamblado de interfaz de usuario. El diseñador carga el recurso de icono por identificador del ensamblado y lo muestra junto al nombre de tarea en el cuadro de herramientas y en la superficie del diseñador cuando la tarea se agrega a un paquete. Si una tarea no proporciona un recurso de icono, el diseñador utiliza un icono predeterminado para la tarea.  
  
## <a name="the-idtstaskui-interface"></a>La interfaz IDTSTaskUI  
 La interfaz <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> define la colección de métodos y propiedades invocada por el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] para inicializar y mostrar la interfaz de usuario asociada a la tarea. Cuando se invoca la interfaz de usuario de una tarea, el diseñador llama al método <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.Initialize%2A>, que implementa la interfaz de usuario de la tarea al escribirla y, a continuación, proporciona las colecciones <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> y <xref:Microsoft.SqlServer.Dts.Runtime.Connections> de la tarea y el paquete, respectivamente, como parámetros. Estas colecciones se almacenan localmente y se utilizan posteriormente en el método <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A>.  
  
 El diseñador llama al método <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> para solicitar la ventana que se muestra en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. La tarea crea una instancia de la ventana que contiene la interfaz de usuario para la tarea y devuelve la interfaz de usuario al diseñador para mostrarla. Normalmente, los objetos <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> y <xref:Microsoft.SqlServer.Dts.Runtime.Connections> se proporcionan a la ventana a través de un constructor sobrecargado de modo que se puedan utilizar para configurar la tarea.  
  
 El Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] llama al método <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI.GetView%2A> de la UI de la tarea para mostrar la interfaz de usuario para la tarea. La interfaz de usuario de la tarea devuelve el formulario Windows Forms de este método y el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] muestra este formulario como un cuadro de diálogo modal. Cuando se cierra el formulario, el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] examina el valor de la propiedad **DialogResult** del formulario para determinar si se ha modificado la tarea y si estas modificaciones se deben guardar. Si el valor de la propiedad **DialogResult** es **correcto**, el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] llama a los métodos de persistencia de la tarea para guardar los cambios; de lo contrario, se descartan esos cambios.  
  
 En el ejemplo de código siguiente se implementa la interfaz <xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsTaskUI> y se supone la existencia de una clase de formulario Windows Forms denominada SampleTaskForm.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Design;  
  
namespace Sample  
{  
   public class HelloWorldTaskUI : IDtsTaskUI  
   {  
      TaskHost   taskHost;  
      Connections connections;  
      public void Initialize(TaskHost taskHost, IServiceProvider serviceProvider)  
      {  
         this.taskHost = taskHost;  
         IDtsConnectionService cs = serviceProvider.GetService  
         ( typeof( IDtsConnectionService ) ) as   IDtsConnectionService;   
         this.connections = cs.GetConnections();  
      }  
      public ContainerControl GetView()  
      {  
        return new HelloWorldTaskForm(this.taskHost, this.connections);  
      }  
     public void Delete(IWin32Window parentWindow)  
     {  
     }  
     public void New(IWin32Window parentWindow)  
     {  
     }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Design  
Imports System.Windows.Forms  
  
Public Class HelloWorldTaskUI  
  Implements IDtsTaskUI  
  
  Dim taskHost As TaskHost  
  Dim connections As Connections  
  
  Public Sub Initialize(ByVal taskHost As TaskHost, ByVal serviceProvider As IServiceProvider) _  
    Implements IDtsTaskUI.Initialize  
  
    Dim cs As IDtsConnectionService  
  
    Me.taskHost = taskHost  
    cs = DirectCast(serviceProvider.GetService(GetType(IDtsConnectionService)), IDtsConnectionService)  
    Me.connections = cs.GetConnections()  
  
  End Sub  
  
  Public Function GetView() As ContainerControl _  
    Implements IDtsTaskUI.GetView  
  
    Return New HelloWorldTaskForm(Me.taskHost, Me.connections)  
  
  End Function  
  
  Public Sub Delete(ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.Delete  
  
  End Sub  
  
  Public Sub [New](ByVal parentWindow As IWin32Window) _  
    Implements IDtsTaskUI.[New]  
  
  End Sub  
  
End Class  
```  
 
## <a name="see-also"></a>Consulte también  
 [Creating a Custom Task](../../../integration-services/extending-packages-custom-objects/task/creating-a-custom-task.md)  (Crear una tarea personalizada)  
 [Programar una tarea personalizada](../../../integration-services/extending-packages-custom-objects/task/coding-a-custom-task.md)   
 [Desarrollar una interfaz de usuario para una tarea personalizada](../../../integration-services/extending-packages-custom-objects/task/developing-a-user-interface-for-a-custom-task.md)  
  
  
