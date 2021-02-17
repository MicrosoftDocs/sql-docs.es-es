---
description: Agregar tareas mediante programación
title: Agregar tareas mediante programación | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- tasks [Integration Services], packages
- adding package tasks
ms.assetid: 5d4652d5-228c-4238-905c-346dd8503fdf
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1650323cede5f91401faa97e820917b2549d5050
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355112"
---
# <a name="adding-tasks-programmatically"></a>Agregar tareas mediante programación

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Se pueden agregar tareas a los siguientes tipos de objetos en el motor en tiempo de ejecución:  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Package>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.Sequence>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop>  
  
-   <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler>  
  
 Estas clases se consideran contenedores y todas ellas heredan la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.IDTSSequence.Executables%2A>. Los contenedores pueden contener una colección de tareas, que son objetos ejecutables procesados por el motor en tiempo de ejecución durante la ejecución del contenedor. El orden de ejecución de los objetos en la colección se determina mediante cualquier conjunto <xref:Microsoft.SqlServer.Dts.Runtime.PrecedenceConstraint> de cada tarea en los contenedores. Las restricciones de precedencia habilitan la bifurcación de la ejecución en función del éxito, error o finalización de <xref:Microsoft.SqlServer.Dts.Runtime.Executable> en la colección.  
  
 Cada contenedor incluye una colección <xref:Microsoft.SqlServer.Dts.Runtime.Executables> que contiene los objetos <xref:Microsoft.SqlServer.Dts.Runtime.Executable> individuales. Cada tarea ejecutable hereda e implementa el método <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Execute%2A> y el método <xref:Microsoft.SqlServer.Dts.Runtime.Executable.Validate%2A>. El motor en tiempo de ejecución llama a estos dos métodos para procesar cada <xref:Microsoft.SqlServer.Dts.Runtime.Executable>.  
  
 Para agregar una tarea a un paquete, necesita un contenedor con una colección <xref:Microsoft.SqlServer.Dts.Runtime.Executables> existente. En la mayoría de las ocasiones, la tarea que agregará a la colección es un paquete. Para agregar el nuevo ejecutable de tarea a la colección de dicho contenedor, debe llamar al método <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. El método tiene un parámetro único, una cadena, que contiene los valores CLSID, PROGID, moniker STOCK o <xref:Microsoft.SqlServer.Dts.Runtime.TaskInfo.CreationName%2A> de la tarea que agrega.  
  
## <a name="task-names"></a>Nombres de tarea  
 Aunque puede especificar una tarea por nombre o por identificador, el moniker **STOCK** es el parámetro que se usa con mayor frecuencia en el método <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A>. Para agregar a un ejecutable una tarea identificada por el moniker **STOCK**, use la sintaxis siguiente:  
  
```csharp  
Executable exec = package.Executables.Add("STOCK:BulkInsertTask");  
  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add("STOCK:BulkInsertTask")  
  
```  
  
 En la lista siguiente se muestran los nombres para cada tarea que se utilizan después del moniker **STOCK**.  
  
-   ActiveXScriptTask  
  
-   BulkInsertTask  
  
-   ExecuteProcessTask  
  
-   ExecutePackageTask  
  
-   Exec80PackageTask  
  
-   FileSystemTask  
  
-   FTPTask  
  
-   MSMQTask  
  
-   PipelineTask  
  
-   ScriptTask  
  
-   SendMailTask  
  
-   SQLTask  
  
-   TransferStoredProceduresTask  
  
-   TransferLoginsTask  
  
-   TransferErrorMessagesTask  
  
-   TransferJobsTask  
  
-   TransferObjectsTask  
  
-   TransferDatabaseTask  
  
-   WebServiceTask  
  
-   WmiDataReaderTask  
  
-   WmiEventWatcherTask  
  
-   XMLTask  
  
 Si prefiere una sintaxis más explícita o si la tarea que desea agregar no tiene un moniker STOCK, puede agregar la tarea al ejecutable utilizando su nombre largo. Esta sintaxis requiere que también especifique el número de versión de la tarea.  
  
```csharp  
Executable exec = package.Executables.Add(  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " +  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " +  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91");  
```  
  
```vb  
Dim exec As Executable = package.Executables.Add( _  
  "Microsoft.SqlServer.Dts.Tasks.ScriptTask.ScriptTask, " & _  
  "Microsoft.SqlServer.ScriptTask, Version=10.0.000.0, " & _  
  "Culture=neutral, PublicKeyToken=89845dcd8080cc91")  
```  
  
 Puede obtener el nombre largo de la tarea mediante programación, sin tener que especificar la versión de la tarea, con la propiedad **AssemblyQualifiedName** de la clase, como se muestra en el ejemplo siguiente. En este ejemplo se requiere una referencia al ensamblado Microsoft.SqlServer.SQLTask.  
  
```csharp  
using Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask;  
...  
      Executable exec = package.Executables.Add(  
        typeof(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName);  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask  
...  
    Dim exec As Executable = package.Executables.Add( _  
      GetType(Microsoft.SqlServer.Dts.Tasks.ExecuteSQLTask.ExecuteSQLTask).AssemblyQualifiedName)  
```  
  
 En el ejemplo de código siguiente se muestra cómo crear una colección <xref:Microsoft.SqlServer.Dts.Runtime.Executables> a partir de un nuevo paquete y, después, cómo agregar una tarea de sistema de archivos y una tarea de inserción masiva a la colección, con sus monikers **STOCK**. En este ejemplo se requiere una referencia a los ensamblados Microsoft.SqlServer.FileSystemTask y Microsoft.SqlServer.BulkInsertTask.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thBulkInsertTask = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        Console.WriteLine("Type {0}", taskHost.InnerObject.ToString());  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thBulkInsertTask As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      Console.WriteLine("Type {0}", taskHost.InnerObject.ToString())  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Salida del ejemplo:**  
  
 `Type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
## <a name="taskhost-container"></a>Contenedor TaskHost  
 La clase <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> es un contenedor que no aparece en la interfaz gráfica de usuario, pero es muy importante en la programación. Esta clase es un contenedor para cada tarea. Las tareas que se agregan al paquete mediante el método <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A> como un objeto <xref:Microsoft.SqlServer.Dts.Runtime.Executable> se pueden convertir como un objeto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Cuando una tarea se convierte como <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, puede utilizar propiedades y métodos adicionales en la tarea. Además, se puede tener acceso a la propia tarea mediante la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Según sus necesidades, puede mantener la tarea como un objeto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> para utilizar sus propiedades a través de la colección <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A>. La ventaja de utilizar <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> es que puede escribir código más genérico. Si necesita código muy específico para una tarea, debe convertir la tarea a su objeto adecuado.  
  
 En el ejemplo de código siguiente se muestra cómo convertir <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, thBulkInsertTask, que contiene <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>, a un objeto <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask>.  
  
```csharp  
BulkInsertTask myTask = thBulkInsertTask.InnerObject as BulkInsertTask;  
```  
  
```vb  
Dim myTask As BulkInsertTask = CType(thBulkInsertTask.InnerObject, BulkInsertTask)  
```  
  
 En el ejemplo de código siguiente se muestra cómo convertir el ejecutable a <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> y, a continuación, cómo utilizar la propiedad <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> para determinar qué tipo de ejecutable contiene el host.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
using Microsoft.SqlServer.Dts.Tasks.FileSystemTask;  
using Microsoft.SqlServer.Dts.Tasks.BulkInsertTask;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package p = new Package();  
      // Add a File System task to the package.  
      Executable exec1 = p.Executables.Add("STOCK:FileSystemTask");  
      TaskHost thFileSystemTask1 = exec1 as TaskHost;  
      // Add a Bulk Insert task to the package.  
      Executable exec2 = p.Executables.Add("STOCK:BulkInsertTask");  
      TaskHost thFileSystemTask2 = exec2 as TaskHost;  
  
      // Iterate through the package Executables collection.  
      Executables pExecs = p.Executables;  
      foreach (Executable pExec in pExecs)  
      {  
        TaskHost taskHost = (TaskHost)pExec;  
        if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask)  
        {  
          // Do work with FileSystemTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        else if (taskHost.InnerObject is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask)  
        {  
          // Do work with BulkInsertTask here.  
          Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString());  
        }  
        // Add additional statements to check InnerObject, if desired.  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
Imports Microsoft.SqlServer.Dts.Tasks.FileSystemTask  
Imports Microsoft.SqlServer.Dts.Tasks.BulkInsertTask  
  
Module Module1  
  
  Sub Main()  
  
    Dim p As Package = New Package()  
    ' Add a File System task to the package.  
    Dim exec1 As Executable = p.Executables.Add("STOCK:FileSystemTask")  
    Dim thFileSystemTask1 As TaskHost = CType(exec1, TaskHost)  
    ' Add a Bulk Insert task to the package.  
    Dim exec2 As Executable = p.Executables.Add("STOCK:BulkInsertTask")  
    Dim thFileSystemTask2 As TaskHost = CType(exec2, TaskHost)  
  
    ' Iterate through the package Executables collection.  
    Dim pExecs As Executables = p.Executables  
    Dim pExec As Executable  
    For Each pExec In pExecs  
      Dim taskHost As TaskHost = CType(pExec, TaskHost)  
      If TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask Then  
        ' Do work with FileSystemTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      ElseIf TypeOf taskHost.InnerObject Is Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask Then  
        ' Do work with BulkInsertTask here.  
        Console.WriteLine("Found task of type {0}", taskHost.InnerObject.ToString())  
      End If  
      ' Add additional statements to check InnerObject, if desired.  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
 **Salida del ejemplo:**  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.FileSystemTask.FileSystemTask`  
  
 `Found task of type Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask`  
  
 La instrucción <xref:Microsoft.SqlServer.Dts.Runtime.Executables.Add%2A> devuelve un ejecutable que se convierte a un objeto <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> a partir del objeto <xref:Microsoft.SqlServer.Dts.Runtime.Executable> recién creado.  
  
 Para establecer propiedades o llamar a métodos en el nuevo objeto, existen dos opciones:  
  
1.  Utilice la colección <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>. Por ejemplo, para obtener una propiedad del objeto, utilice `th.Properties["propertyname"].GetValue(th))`. Para establecer una propiedad, utilice `th.Properties["propertyname"].SetValue(th, <value>);`.  
  
2.  Convierta <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.InnerObject%2A> de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> a la clase de tarea. Por ejemplo, para convertir la tarea Inserción masiva a <xref:Microsoft.SqlServer.Dts.Tasks.BulkInsertTask.BulkInsertTask> después de agregarla a un paquete como <xref:Microsoft.SqlServer.Dts.Runtime.Executable> y posteriormente convertirla a <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost>, utilice `BulkInsertTask myTask = th.InnerObject as BulkInsertTask;`.  
  
 La utilización de la clase <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> en código, en lugar de convertir a la clase específica de la tarea presenta las siguientes ventajas:  
  
-   El proveedor <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> de la clase <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> no requiere una referencia al ensamblado en el código.  
  
-   Puede codificar rutinas genéricas que funcionan en cualquier tarea, ya que no es necesario conocer el nombre de la tarea en tiempo de compilación. Estas rutinas genéricas incluyen métodos donde se pasa el nombre de la tarea al método; el código del método funciona en todas las tareas. Éste es un método adecuado para escribir código de prueba.  
  
 La conversión de <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> a la clase específica de la tarea presenta las siguientes ventajas:  
  
-   El proyecto de Visual Studio proporciona finalización de instrucciones (IntelliSense).  
  
-   El código se puede ejecutar con más rapidez.  
  
-   Los objetos específicos de la tarea habilitan el enlace anticipado y las optimizaciones resultantes. Para obtener más información el enlace anticipado y el enlace en tiempo de ejecución, vea el tema correspondiente en Conceptos del lenguaje Visual Basic.  
  
 En el ejemplo de código siguiente se amplía el concepto de reutilización de código de la tarea. En lugar de convertir las tareas a sus equivalentes de clase específicos, el ejemplo de código muestra cómo convertir el ejecutable a <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost> y, a continuación, cómo utilizar <xref:Microsoft.SqlServer.Dts.Runtime.TaskHost.Properties%2A> para escribir código genérico para todas las tareas.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.SqlServer.Dts.Samples  
{  
  class Program  
  {  
    static void Main(string[] args)  
    {  
      Package package = new Package();  
  
      string[] tasks = { "STOCK:SQLTask", "STOCK:ScriptTask",   
        "STOCK:ExecuteProcessTask", "STOCK:PipelineTask",   
        "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask" };  
  
      foreach (string s in tasks)  
      {  
        TaskHost taskhost = package.Executables.Add(s) as TaskHost;  
        DtsProperties props = taskhost.Properties;  
        Console.WriteLine("Enumerating properties on " + taskhost.Name);  
        Console.WriteLine(" TaskHost.InnerObject is " + taskhost.InnerObject.ToString());  
        Console.WriteLine();  
  
        foreach (DtsProperty prop in props)  
        {  
          Console.WriteLine("Properties for " + prop.Name);  
          Console.WriteLine("Name : " + prop.Name);  
          Console.WriteLine("Type : " + prop.Type.ToString());  
          Console.WriteLine("Readable : " + prop.Get.ToString());  
          Console.WriteLine("Writable : " + prop.Set.ToString());  
          Console.WriteLine();  
        }  
      }  
      Console.Read();  
    }  
  }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Runtime  
  
Module Module1  
  
  Sub Main()  
  
    Dim package As Package = New Package()  
  
    Dim tasks() As String = New String() {"STOCK:SQLTask", "STOCK:ScriptTask", _  
              "STOCK:ExecuteProcessTask", "STOCK:PipelineTask", _  
              "STOCK:FTPTask", "STOCK:SendMailTask", "STOCK:MSMQTask"}  
  
    For Each s As String In tasks  
  
      Dim taskhost As TaskHost = CType(package.Executables.Add(s), TaskHost)  
      Dim props As DtsProperties = taskhost.Properties  
      Console.WriteLine("Enumerating properties on " & taskhost.Name)  
      Console.WriteLine(" TaskHost.InnerObject is " & taskhost.InnerObject.ToString())  
      Console.WriteLine()  
  
      For Each prop As DtsProperty In props  
        Console.WriteLine("Properties for " + prop.Name)  
        Console.WriteLine(" Name : " + prop.Name)  
        Console.WriteLine(" Type : " + prop.Type.ToString())  
        Console.WriteLine(" Readable : " + prop.Get.ToString())  
        Console.WriteLine(" Writable : " + prop.Set.ToString())  
        Console.WriteLine()  
      Next  
  
    Next  
    Console.Read()  
  
  End Sub  
  
End Module  
```  
  
## <a name="external-resources"></a>Recursos externos  
 Entrada de blog sobre [EzAPI, actualizado para SQL Server 2012](https://techcommunity.microsoft.com/t5/sql-server-integration-services/ezapi-updated-for-sql-server-2012/ba-p/388042), en blogs.msdn.com.  

## <a name="see-also"></a>Consulte también  
 [Conectar tareas mediante programación](../../integration-services/building-packages-programmatically/connecting-tasks-programmatically.md)  
  
  
