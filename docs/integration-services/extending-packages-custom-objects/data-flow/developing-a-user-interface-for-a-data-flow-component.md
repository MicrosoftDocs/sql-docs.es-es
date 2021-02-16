---
description: Desarrollar una interfaz de usuario para un componente de flujo de datos
title: Desarrollar una interfaz de usuario para un componente de flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow components [Integration Services], custom editors
- user interfaces [Integration Services]
- custom data flow components [Integration Services], custom editors
- custom component editors [Integration Services]
- IDtsComponentUI interface
- UITypeName property
- custom user interface [Integration Services], custom data flow component
- editors [Integration Services]
ms.assetid: 10b829a1-609b-42e3-9070-cfe5a2bb698c
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cfe2ebf4e63237f9e5039188c5f9fec883fd797e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343700"
---
# <a name="developing-a-user-interface-for-a-data-flow-component"></a>Desarrollar una interfaz de usuario para un componente de flujo de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Los desarrolladores de componentes pueden proporcionar una interfaz de usuario personalizada para un componente, que se muestre en [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] cuando se edite el componente. La implementación de una interfaz de usuario personalizada proporciona notificaciones cuando el componente se agrega o se elimina en una tarea de flujo de datos, así como cuando se solicita ayuda para el componente.  
  
 Aunque no proporcione una interfaz de usuario personalizada para su componente, los usuarios podrán configurar el componente y sus propiedades personalizadas mediante el editor avanzado. Para asegurarse de que el editor avanzado permita a los usuarios editar los valores de las propiedades personalizadas de forma adecuada, use las propiedades <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.TypeConverter%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100.UITypeEditor%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> cuando proceda. Para obtener más información, consulte la sección "Creating Custom Properties (Crear propiedades personalizadas)" en [Design-time Methods of a Data Flow Component (Métodos en tiempo de diseño de un componente de flujo de datos)](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md).  
  
## <a name="setting-the-uitypename-property"></a>Establecer la propiedad UITypeName  
 Para proporcionar una interfaz de usuario personalizada, el desarrollador debe establecer la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute> en el nombre de una clase que implemente la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. Cuando el componente establece esta propiedad, [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] carga y llama a la interfaz de usuario personalizada mientras el componente se edita en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)].  
  
 La propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A> es una cadena delimitada por comas que identifica el nombre completo del tipo. En la lista siguiente se muestran en orden los elementos que identifican el tipo:  
  
-   Nombre de tipo  
  
-   Nombre del ensamblado  
  
-   Versión del archivo  
  
-   Referencia cultural  
  
-   Token de clave pública  
  
 En el ejemplo de código siguiente se muestra una clase que deriva de la clase base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> y especifica la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.UITypeName%2A>.  
  
```csharp  
[DtsPipelineComponent(  
DisplayName="SampleComponent",  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...",  
ComponentType = ComponentType.Transform)]  
public class SampleComponent : PipelineComponent  
{  
//TODO: Implement the component here.  
}  
```  
  
```vb  
<DtsPipelineComponent(DisplayName="SampleComponent", _  
UITypeName="MyNamespace.MyComponentUIClassName,MyAssemblyName,Version=1.0.0.0,Culture=neutral,PublicKeyToken=abcd...", ComponentType=ComponentType.Transform)> _   
Public Class SampleComponent   
 Inherits PipelineComponent   
End Class  
```  
  
## <a name="implementing-the-idtscomponentui-interface"></a>Implementar la interfaz IDtsComponentUI  
 La interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> contiene métodos a los que llama el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] cuando se agrega, elimina o edita un componente. Los desarrolladores de componentes pueden proporcionar código en su implementación de estos métodos para interactuar con los usuarios del componente.  
  
 Normalmente, esta clase se implementa en un ensamblado independiente del propio componente. Aunque no se requiere el uso de un ensamblado independiente, esto permite al desarrollador generar e implementar el componente y la interfaz de usuario de forma independiente entre sí y mantiene una superficie binaria reducida del componente.  
  
 La implementación de una interfaz de usuario personalizada proporciona al desarrollador de componentes un mayor control sobre el componente al editarlo en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Por ejemplo, un componente puede agregar código al método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> (al que se llama cuando un componente se agrega inicialmente a una tarea de flujo de datos) y mostrar un asistente que guía al usuario a lo largo del proceso de configuración inicial del componente.  
  
 Después de haber creado una clase que implementa la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>, debe agregar código para responder a la interacción del usuario con el componente. El método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> proporciona la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> del componente y se llama a este método antes que a los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.New%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>. Esta referencia debería almacenarse en una variable miembro privada y usarse para modificar los metadatos del componente posteriormente.  
  
## <a name="modifying-a-component-and-persisting-changes"></a>Modificar un componente y conservar los cambios  
 La interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> se proporciona como parámetro para el método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A>. Esta referencia debería almacenarse en caché en una variable miembro mediante el código de interfaz de usuario y usarse después para modificar el componente como respuesta a la interacción del usuario con la interfaz de usuario.  
  
 Aunque puede modificar directamente el componente a través de la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>, es mejor crear una instancia de <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> mediante el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A>. Al editar el componente directamente con la interfaz, se omiten las medidas de seguridad de validación del componente. La ventaja de usar la instancia en tiempo de diseño del componente mediante <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper> es que se asegura de que el componente tenga control sobre los cambios realizados en él.  
  
 El valor devuelto por el método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> determina si se conservan o se descartan los cambios realizados en un componente. Cuando este método devuelve **false**, se descartan todos los cambios; si devuelve **true**, se conservan los cambios realizados en el componente y el paquete se marca con la indicación de que debe guardarse.  
  
### <a name="using-the-services-of-the-ssis-designer"></a>Usar los servicios del Diseñador SSIS  
 El parámetro **IServiceProvide** del método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Initialize%2A> proporciona acceso a los siguientes servicios del Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)]:  
  
|Servicio|Descripción|  
|-------------|-----------------|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsClipboardService>|Se usa para determinar si el componente se generó como parte de una operación de copiar y pegar o de cortar y pegar.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsConnectionService>|Se usa para obtener acceso a las conexiones existentes o crear nuevas conexiones en el paquete.|  
|<xref:Microsoft.SqlServer.Dts.Design.IErrorCollectionService>|Se usa para capturar eventos de los componentes de flujo de datos cuando es necesario capturar todos los errores y advertencias generados por el componente en lugar de recibir solamente el último error o advertencia.|  
|<xref:Microsoft.SqlServer.Dts.Runtime.Design.IDtsVariableService>|Se usa para obtener acceso a las variables existentes o para crear nuevas variables en el paquete.|  
|<xref:Microsoft.SqlServer.Dts.Design.IDtsPipelineEnvironmentService>|Lo usan los componentes de flujo de datos para obtener acceso a la tarea Flujo de datos primaria y a otros componentes del flujo de datos. Esta característica podría usarse para desarrollar un componente como el Asistente para dimensiones de variación lenta, que crea y conecta componentes de flujo de datos adicionales según sea necesario.|  
  
 Estos servicios proporcionan a los desarrolladores de componentes la capacidad de crear objetos en el paquete donde se carga el componente y de obtener acceso a ellos.  
  
## <a name="sample"></a>Muestra  
 En el ejemplo de código siguiente se muestra la integración de una clase de interfaz de usuario personalizada que implementa la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI> y un formulario Windows Forms que actúa como editor de un componente.  
  
### <a name="custom-user-interface-class"></a>Clase de interfaz de usuario personalizada  
 El código siguiente muestra la clase que implementa la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI>. El método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A> crea el editor de componentes y, a continuación, muestra el formulario. El valor devuelto por el formulario determina si se conservan los cambios realizados en el componente.  
  
```csharp  
using System;  
using System.Windows.Forms;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline.Design;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public class SampleComponentUI : IDtsComponentUI  
    {  
        IDTSComponentMetaData100 md;  
        IServiceProvider sp;  
  
        public void Help(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void New(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public void Delete(System.Windows.Forms.IWin32Window parentWindow)  
        {  
        }  
        public bool Edit(System.Windows.Forms.IWin32Window parentWindow, Variables vars, Connections cons)  
        {  
            // Create and display the form for the user interface.  
            SampleComponentUIForm componentEditor = new SampleComponentUIForm(cons, vars, md);  
  
            DialogResult result  = componentEditor.ShowDialog(parentWindow);  
  
            if (result == DialogResult.OK)  
                return true;  
  
            return false;  
        }  
        public void Initialize(IDTSComponentMetaData100 dtsComponentMetadata, IServiceProvider serviceProvider)  
        {  
            // Store the component metadata.  
            this.md = dtsComponentMetadata;  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Windows.Forms   
Imports Microsoft.SqlServer.Dts.Runtime   
Imports Microsoft.SqlServer.Dts.Pipeline.Design   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Class SampleComponentUI   
 Implements IDtsComponentUI   
   Private md As IDTSComponentMetaData100   
   Private sp As IServiceProvider   
  
   Public Sub Help(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub New(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Sub Delete(ByVal parentWindow As System.Windows.Forms.IWin32Window)   
   End Sub   
  
   Public Function Edit(ByVal parentWindow As System.Windows.Forms.IWin32Window, ByVal vars As Variables, ByVal cons As Connections) As Boolean   
     ' Create and display the form for the user interface.  
     Dim componentEditor As SampleComponentUIForm = New SampleComponentUIForm(cons, vars, md)   
     Dim result As DialogResult = componentEditor.ShowDialog(parentWindow)   
     If result = DialogResult.OK Then   
       Return True   
     End If   
     Return False   
   End Function   
  
   Public Sub Initialize(ByVal dtsComponentMetadata As IDTSComponentMetaData100, ByVal serviceProvider As IServiceProvider)   
     Me.md = dtsComponentMetadata   
   End Sub   
 End Class   
  
End Namespace  
```  
  
### <a name="custom-editor"></a>Editor personalizado  
 El código siguiente muestra la implementación del formulario Windows Forms que se muestra durante la llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.Design.IDtsComponentUI.Edit%2A>.  
  
```csharp  
using System;  
using System.Drawing;  
using System.Collections;  
using System.ComponentModel;  
using System.Windows.Forms;  
using System.Data;  
  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    public partial class SampleComponentUIForm : System.Windows.Forms.Form  
    {  
        private Connections connections;  
        private Variables variables;  
        private IDTSComponentMetaData100 metaData;  
        private CManagedComponentWrapper designTimeInstance;  
        private System.ComponentModel.IContainer components = null;  
  
        public SampleComponentUIForm( Connections cons, Variables vars, IDTSComponentMetaData100 md)  
        {  
            variables = vars;  
            connections = cons;  
            metaData = md;  
        }  
  
        private void btnOk_Click(object sender, System.EventArgs e)  
        {  
            if (designTimeInstance == null)  
                designTimeInstance = metaData.Instantiate();  
  
            designTimeInstance.SetComponentProperty( "CustomProperty", txtCustomPropertyValue.Text);  
  
            this.Close();  
        }  
  
        private void btnCancel_Click(object sender, System.EventArgs e)  
        {  
            this.Close();  
        }  
    }  
}  
```  
  
```vb  
Imports System   
Imports System.Drawing   
Imports System.Collections   
Imports System.ComponentModel   
Imports System.Windows.Forms   
Imports System.Data   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime   
  
Namespace Microsoft.Samples.SqlServer.Dts   
  
 Public Partial Class SampleComponentUIForm   
  Inherits System.Windows.Forms.Form   
   Private connections As Connections   
   Private variables As Variables   
   Private metaData As IDTSComponentMetaData100   
   Private designTimeInstance As CManagedComponentWrapper   
   Private components As System.ComponentModel.IContainer = Nothing   
  
   Public Sub New(ByVal cons As Connections, ByVal vars As Variables, ByVal md As IDTSComponentMetaData100)   
     variables = vars   
     connections = cons   
     metaData = md   
   End Sub   
  
   Private Sub btnOk_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     If designTimeInstance Is Nothing Then   
       designTimeInstance = metaData.Instantiate   
     End If   
     designTimeInstance.SetComponentProperty("CustomProperty", txtCustomPropertyValue.Text)   
     Me.Close   
   End Sub   
  
   Private Sub btnCancel_Click(ByVal sender As Object, ByVal e As System.EventArgs)   
     Me.Close   
   End Sub   
 End Class   
  
End Namespace  
```
  
## <a name="see-also"></a>Consulte también  
 [Crear un componente de flujo de datos personalizado](../../../integration-services/extending-packages-custom-objects/data-flow/creating-a-custom-data-flow-component.md)  
  
  
