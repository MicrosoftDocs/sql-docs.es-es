---
description: Agregar componentes de flujo de datos mediante programación
title: Agregar componentes de flujo de datos mediante programación | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.technology: integration-services
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- data flow task [Integration Services], components
- components [Integration Services], data flow
- data flow [Integration Services], components
ms.assetid: c06065cf-43e5-4b6b-9824-7309d7f5e35e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cd3a122993913d524547d6c9e57945956f1099de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350998"
---
# <a name="adding-data-flow-components-programmatically"></a>Agregar componentes de flujo de datos mediante programación

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cuando se genera un flujo de datos, se inicia agregando componentes. A continuación, se configuran esos componentes y se conectan juntos para establecer el flujo de datos en tiempo de ejecución. En esta sección se describe cómo agregar un componente a la tarea de flujo de datos, crear la instancia del componente en tiempo de diseño y, a continuación, configurar el componente. Para obtener más información acerca de cómo conectar componentes, vea [Conectar componentes de flujo de datos mediante programación](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
## <a name="adding-a-component"></a>Agregar componentes  
 Llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaDataCollection100.New%2A> de la colección <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.MainPipeClass.ComponentMetaDataCollection%2A> para crear un nuevo componente y agregarlo a la tarea de flujo de datos. Este método devuelve la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> del componente. Sin embargo, en este punto, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100> no contiene información específica de ningún componente. Establezca la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> para identificar el tipo de componente. La tarea de flujo de datos usa el valor de esta propiedad para crear una instancia del componente en tiempo de ejecución.  
  
 El valor especificado en la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> puede ser la propiedad CLSID, PROGID o <xref:Microsoft.SqlServer.Dts.Runtime.PipelineComponentInfo.CreationName%2A> del componente. Normalmente, CLSID se muestra en la ventana Propiedades como el valor de la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A> del componente. Para obtener información acerca de cómo obtener esta propiedad y otras propiedades de componentes disponibles, vea [Detectar componentes de flujo de datos mediante programación](../../integration-services/building-packages-programmatically/discovering-data-flow-components-programmatically.md).  
  
## <a name="adding-a-managed-component"></a>Agregar un componente administrado  
 No se puede usar la propiedad CLSID o PROGID para agregar uno de los componentes de flujo de datos administrados al flujo de datos, debido a que estos valores señalan a un contenedor y no al propio componente. En su lugar, se puede usar la propiedad **CreationName** o la propiedad **AssemblyQualifiedName** como se muestra en el ejemplo siguiente.  
  
 Si piensa usar la propiedad **AssemblyQualifiedName**, debe agregar en su proyecto de [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] una referencia al ensamblado que contiene el componente administrado. Estos ensamblados no se enumeran en la pestaña .NET del cuadro de diálogo **Agregar referencia**. Normalmente, debe desplazarse para localizar el ensamblado en la carpeta **C:\Program Files\Microsoft SQL Server\100\DTS\PipelineComponents**.  
  
 Los componentes de flujo de datos administrados integrados incluyen:  
  
-   Origen [!INCLUDE[vstecado](../../includes/vstecado-md.md)]  
  
-   Origen XML  
  
-   DataReader, destino  
  
-   Destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Compact  
  
-   Componente de script  
  
 En el siguiente ejemplo de código se muestran ambas maneras de agregar un componente administrado al flujo de datos:  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Microsoft.SqlServer.Dts.Runtime.Package package = new Microsoft.SqlServer.Dts.Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Microsoft.SqlServer.Dts.Runtime.TaskHost thMainPipe = (Microsoft.SqlServer.Dts.Runtime.TaskHost)e;  
      MainPipe dataFlowTask = (MainPipe)thMainPipe.InnerObject;  
  
      // The Application object will be used to obtain the CreationName  
      //  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
      Application app = new Application();  
  
      // Add a first ADO NET source to the data flow.  
      //  The CreationName property requires an Application instance.  
      IDTSComponentMetaData100 component1 = dataFlowTask.ComponentMetaDataCollection.New();  
      component1.Name = "DataReader Source";  
      component1.ComponentClassID = app.PipelineComponentInfos["DataReader Source"].CreationName;  
  
      // Add a second ADO NET source to the data flow.  
      //  The AssemblyQualifiedName property requires a reference to the assembly.  
      IDTSComponentMetaData100 component2 = dataFlowTask.ComponentMetaDataCollection.New();  
      component2.Name = "DataReader Source";  
      component2.ComponentClassID = typeof(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName;  
    }  
  }  
}  
  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' The Application object will be used to obtain the CreationName  
    '  of a PipelineComponentInfo from its PipelineComponentInfos collection.  
    Dim app As New Application()  
  
    ' Add a first ADO NET source to the data flow.  
    '  The CreationName property requires an Application instance.  
    Dim component1 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component1.Name = "DataReader Source"  
    component1.ComponentClassID = app.PipelineComponentInfos("DataReader Source").CreationName  
  
    ' Add a second ADO NET source to the data flow.  
    '  The AssemblyQualifiedName property requires a reference to the assembly.  
    Dim component2 As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component2.Name = "DataReader Source"  
    component2.ComponentClassID = _  
      GetType(Microsoft.SqlServer.Dts.Pipeline.DataReaderSourceAdapter).AssemblyQualifiedName  
  
  End Sub  
  
End Module  
```  
  
## <a name="creating-the-design-time-instance-of-the-component"></a>Crear la instancia en tiempo de diseño del componente  
 Llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.Instantiate%2A> para crear la instancia en tiempo de diseño del componente identificado por la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.ComponentClassID%2A>. Este método devuelve el objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapper>, que es el contenedor administrado para la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSDesigntimeComponent100>.  
  
 Siempre que sea posible, debería modificar un componente mediante los métodos de la instancia en tiempo de diseño, en lugar de modificando directamente los metadatos del componente. Aunque hay elementos en los metadatos que se deben establecer directamente, como conexiones, generalmente se recomienda modificar directamente los metadatos puesto que se omite la capacidad del componente para supervisar y validar los cambios.  
  
## <a name="assigning-connections"></a>Asignar conexiones  
 Algunos componentes, como el componente de origen de OLE DB, requieren una conexión a datos externos y usan un objeto <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> existente en el paquete para este propósito. La propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnectionCollection100.Count%2A> de la colección <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A> indica el número de objetos <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> en tiempo de ejecución que requiere el componente. Si el recuento es mayor que cero, el componente requiere una conexión. Asigne un administrador de conexiones del paquete al componente especificando las propiedades <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.ConnectionManager%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeConnection100.Name%2A> de la primera conexión en <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.RuntimeConnectionCollection%2A>. Observe que el nombre del administrador de conexiones en la colección de conexiones en tiempo de ejecución debe coincidir con el nombre del administrador de conexiones al que se hace referencia desde el paquete.  
  
## <a name="setting-the-values-of-custom-properties"></a>Establecer los valores de propiedades personalizadas  
 Después de crear la instancia en tiempo de diseño del componente, llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>. Este método es similar a un constructor porque inicializa un componente recién creado al crear sus propiedades personalizadas y sus objetos de entrada y salida. No llame a <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A> más de una vez en un componente porque el propio componente se puede restablecer y perder cualquier modificación realizada previamente en sus metadatos.  
  
 <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.CustomPropertyCollection%2A> del componente contiene una colección de objetos <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSCustomProperty100> específicos del componente. A diferencia de otros modelos de programación, donde las propiedades de un objeto siempre están visibles en el objeto, los componentes únicamente rellenan sus colecciones de propiedad personalizada al llamar al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ProvideComponentProperties%2A>. Después de llamar al método, use el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.SetComponentProperty%2A> de la instancia en tiempo de diseño del componente para asignar valores a sus propiedades personalizadas. Este método acepta un par nombre/valor que identifica la propiedad personalizada y proporciona su nuevo valor.  
  
## <a name="initializing-output-columns"></a>Inicializar columnas de salida  
 Después de agregar un componente a la tarea y configurarlo, inicialice la colección de columnas en <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> del objeto. Este paso es especialmente relevante para los componentes de origen, pero puede que no inicialice las columnas para los componentes de transformación y destino, porque generalmente dependen de las columnas que reciben de los componentes de nivel superior.  
  
 Llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A> para inicializar las columnas en las salidas de un componente de origen. Dado que los componentes no se conectan automáticamente a los orígenes de datos externos, llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.AcquireConnections%2A> antes de llamar <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReinitializeMetaData%2A>, para proporcionar el acceso de componente a su origen de datos externo y la capacidad de rellenar sus metadatos de columna. Finalmente, llame al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.CManagedComponentWrapperClass.ReleaseConnections%2A> para liberar la conexión.  
  
## <a name="next-step"></a>siguiente paso  
 Después de agregar y configurar el componente, el paso siguiente será crear rutas de acceso entre componentes, que se describe en el tema [Crear una ruta de acceso entre dos componentes](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md).  
  
## <a name="sample"></a>Muestra  
 En el siguiente ejemplo de código se agrega el componente de origen de OLE DB a una tarea de flujo de datos, se crea una instancia en tiempo de diseño del componente y se configuran las propiedades del componente. En este ejemplo se requiere una referencia adicional al ensamblado Microsoft.SqlServer.DTSRuntimeWrap.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Runtime.Package package = new Runtime.Package();  
      Executable e = package.Executables.Add("STOCK:PipelineTask");  
      Runtime.TaskHost thMainPipe = e as Runtime.TaskHost;  
      MainPipe dataFlowTask = thMainPipe.InnerObject as MainPipe;  
  
      // Add an OLEDB connection manager to the package.  
      ConnectionManager cm = package.Connections.Add("OLEDB");  
      cm.Name = "OLEDB ConnectionManager";  
      cm.ConnectionString = "Data Source=(local);" +   
        "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" +   
        "Integrated Security=SSPI;"  
  
      // Add an OLE DB source to the data flow.  
      IDTSComponentMetaData100 component =   
        dataFlowTask.ComponentMetaDataCollection.New();  
      component.Name = "OLEDBSource";  
      component.ComponentClassID = "DTSAdapter.OleDbSource.1";  
      // You can also use the CLSID of the component instead of the PROGID.  
      //component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
      // Get the design time instance of the component.  
      CManagedComponentWrapper instance = component.Instantiate();  
  
      // Initialize the component  
      instance.ProvideComponentProperties();  
  
      // Specify the connection manager.  
      if (component.RuntimeConnectionCollection.Count > 0)  
      {  
        component.RuntimeConnectionCollection[0].ConnectionManager =   
          DtsConvert.GetExtendedInterface(package.Connections[0]);  
        component.RuntimeConnectionCollection[0].ConnectionManagerID =   
          package.Connections[0].ID;      }  
  
      // Set the custom properties.  
      instance.SetComponentProperty("AccessMode", 2);  
      instance.SetComponentProperty("SqlCommand",   
        "Select * from Production.Product");  
  
      // Reinitialize the metadata.  
      instance.AcquireConnections(null);  
      instance.ReinitializeMetaData();  
      instance.ReleaseConnections();  
  
      // Add other components to the data flow and connect them.  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Microsoft.SqlServer.Dts.Runtime.Package = _  
      New Microsoft.SqlServer.Dts.Runtime.Package()  
    Dim e As Executable = package.Executables.Add("STOCK:PipelineTask")  
    Dim thMainPipe As Microsoft.SqlServer.Dts.Runtime.TaskHost = _  
      CType(e, Microsoft.SqlServer.Dts.Runtime.TaskHost)  
    Dim dataFlowTask As MainPipe = CType(thMainPipe.InnerObject, MainPipe)  
  
    ' Add an OLEDB connection manager to the package.  
    Dim cm As ConnectionManager = package.Connections.Add("OLEDB")  
    cm.Name = "OLEDB ConnectionManager"  
    cm.ConnectionString = "Data Source=(local);" & _  
      "Initial Catalog=AdventureWorks;Provider=SQLOLEDB.1;" & _  
      "Integrated Security=SSPI;"  
  
    ' Add an OLE DB source to the data flow.  
    Dim component As IDTSComponentMetaData100 = _  
      dataFlowTask.ComponentMetaDataCollection.New()  
    component.Name = "OLEDBSource"  
    component.ComponentClassID = "DTSAdapter.OleDbSource.1"  
    ' You can also use the CLSID of the component instead of the PROGID.  
    'component.ComponentClassID = "{2C0A8BE5-1EDC-4353-A0EF-B778599C65A0}";  
  
    ' Get the design time instance of the component.  
    Dim instance As CManagedComponentWrapper = component.Instantiate()  
  
    ' Initialize the component.  
    instance.ProvideComponentProperties()  
  
    ' Specify the connection manager.  
    If component.RuntimeConnectionCollection.Count > 0 Then  
      component.RuntimeConnectionCollection(0).ConnectionManager = _  
        DtsConvert.GetExtendedInterface(package.Connections(0))  
      component.RuntimeConnectionCollection(0).ConnectionManagerID = _  
        package.Connections(0).ID  
    End If  
  
    ' Set the custom properties.  
    instance.SetComponentProperty("AccessMode", 2)  
    instance.SetComponentProperty("SqlCommand", _  
      "Select * from Production.Product")  
  
    ' Reinitialize the metadata.  
    instance.AcquireConnections(vbNull)  
    instance.ReinitializeMetaData()  
    instance.ReleaseConnections()  
  
    ' Add other components to the data flow and connect them.  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog sobre [EzAPI, actualizado para SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), en blogs.msdn.com.  

## <a name="see-also"></a>Consulte también  
 [Conectar componentes de flujo de datos mediante programación](../../integration-services/building-packages-programmatically/connecting-data-flow-components-programmatically.md)  
  
  
