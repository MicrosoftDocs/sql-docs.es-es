---
description: Desarrollar un componente de transformación personalizado con salidas asincrónicas
title: Desarrollar un componente de transformación personalizado con salidas asincrónicas | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- custom data flow components [Integration Services], transformation components
- asynchronous outputs [Integration Services]
- ProcessInput method
- cache [Integration Services]
- buffer allocations [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- PrimeOutput method
- data flow components [Integration Services], transformation components
ms.assetid: 1c3e92c7-a4fa-4fdd-b9ca-ac3069536274
author: chugugrace
ms.author: chugu
ms.openlocfilehash: d6eeddfc5c4fff29b27462889d39748c8df2a01e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348253"
---
# <a name="developing-a-custom-transformation-component-with-asynchronous-outputs"></a>Desarrollar un componente de transformación personalizado con salidas asincrónicas

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Cuando una transformación no puede generar filas hasta que un componente ha recibido todas sus filas de entrada o no genera exactamente una fila de salida por cada fila recibida como entrada, se utiliza un componente con salidas asincrónicas. Por ejemplo, la transformación Agregado no puede calcular una suma de las filas hasta que las ha leído todas. En cambio, puede utilizar un componente con salidas sincrónicas en cualquier momento al modificar cada fila de datos a medida que las atraviesa. Puede modificar los datos para cada fila en su lugar o bien crear una o más columnas nuevas, cada una de las cuales tiene un valor para cada una de las filas de entrada. Para obtener más información acerca de la diferencia entre los componentes sincrónicos y asincrónicos, vea [Descripción de las transformaciones sincrónicas y asincrónicas](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Los componentes de transformación con salidas asincrónicas son únicos porque actúan como componentes de destino y componentes de origen. Este tipo de componente recibe filas de componentes de nivel superior y agrega filas que consumen los componentes de nivel inferior. Ningún otro componente de flujo de datos realiza estas dos operaciones.  
  
 Las columnas de los componentes de nivel superior disponibles en un componente con salidas sincrónicas están automáticamente disponibles para los componentes de nivel inferior del componente. Por tanto, un componente con salidas sincrónicas no tiene que definir ninguna columna de salida para proporcionar columnas y filas al componente siguiente. Los componentes con salidas asincrónicas, por otro lado, deben definir columnas de salida y proporcionar filas a los componentes de nivel inferior. Así, un componente con salidas asincrónicas tiene más tareas que realizar durante el tiempo de diseño y el tiempo de ejecución y el programador de componentes tiene más código que implementar.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] contiene varias transformaciones con salidas asincrónicas. Por ejemplo, la transformación Ordenar requiere todas sus filas antes de poder ordenarlas y lo consigue mediante salidas asincrónicas. Después de recibir todas sus filas, las ordena y las agrega a su salida.  
  
 En esta sección se explica en detalle cómo desarrollar transformaciones con salidas asincrónicas. Para obtener más información acerca del desarrollo de componentes de origen, vea [Desarrollar un componente de origen personalizado](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md).  
  
## <a name="design-time"></a>Tiempo de diseño  
  
### <a name="creating-the-component"></a>Crear el componente  
 La propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> en el objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> identifica si una salida es sincrónica o asincrónica. Para crear una salida asincrónica, agregue la salida al componente y establezca <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> en cero. Al establecer esta propiedad también se determina si la tarea de flujo de datos asigna objetos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> tanto a la entrada como a la salida del componente o si se asigna y se comparte un único búfer entre los dos objetos.  
  
 En el código de ejemplo siguiente se muestra un componente que crea una salida asincrónica en su implementación <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "AsyncComponent",ComponentType = ComponentType.Transform)]  
    public class AsyncComponent : PipelineComponent  
    {  
        public override void ProvideComponentProperties()  
        {  
            // Call the base class, which adds a synchronous input  
            // and output.  
            base.ProvideComponentProperties();  
  
            // Make the output asynchronous.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
            output.SynchronousInputID = 0;  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="AsyncComponent", ComponentType:=ComponentType.Transform)> _  
Public Class AsyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Call the base class, which adds a synchronous input  
        ' and output.  
        Me.ProvideComponentProperties()  
  
        ' Make the output asynchronous.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
        output.SynchronousInputID = 0  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Crear y configurar columnas de salida  
 Tal y como se ha mencionado anteriormente, un componente asincrónico agrega columnas a su colección de columnas de salida para proporcionar columnas a los componentes de nivel inferior. Existen varios métodos en tiempo de diseño entre los que elegir, en función de las necesidades del componente. Por ejemplo, si desea pasar todas las columnas de los componentes de nivel superior a los componentes de nivel inferior, debería invalidar el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.OnInputPathAttached%2A> para agregar las columnas, porque se trata del primer método en el que las columnas de entrada están disponibles para el componente.  
  
 Si el componente crea columnas de salida basándose en las columnas seleccionadas para su entrada, invalide el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.SetUsageType%2A> para seleccionar las columnas de salida e indicar cómo se utilizarán.  
  
 Si un componente con salidas asincrónicas crea columnas de salida basándose en las columnas de los componentes de nivel superior y cambian las columnas de nivel superior disponibles, el componente debe actualizar su colección de columnas de salida. El componente debe detectar estos cambios durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> y corregirlos durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
> [!NOTE]  
>  Cuando una columna de resultados se quita de la colección de columnas de salida, los componentes de nivel inferior del flujo de datos que hacen referencia a la columna se ven afectados negativamente. La columna de salida se debe reparar sin quitar y volver a crear la columna para evitar la ruptura de los componentes de nivel inferior. Por ejemplo, si el tipo de datos de la columna ha cambiado, debe actualizar el tipo de datos.  
  
 En el ejemplo de código siguiente se muestra un componente que agrega una columna de salida a su colección de columnas de salida para cada columna disponible del componente de nivel superior.  
  
```csharp  
public override void OnInputPathAttached(int inputID)  
{  
   IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
   IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
   IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
   foreach (IDTSVirtualInputColumn100 vCol in vInput.VirtualInputColumnCollection)  
   {  
      IDTSOutputColumn100 outCol = output.OutputColumnCollection.New();  
      outCol.Name = vCol.Name;  
      outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage);  
   }  
}  
```  
  
```vb  
Public Overrides Sub OnInputPathAttached(ByVal inputID As Integer)  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
    Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput()  
  
    For Each vCol As IDTSVirtualInputColumn100 In vInput.VirtualInputColumnCollection  
  
        Dim outCol As IDTSOutputColumn100 = output.OutputColumnCollection.New()  
        outCol.Name = vCol.Name  
        outCol.SetDataTypeProperties(vCol.DataType, vCol.Length, vCol.Precision, vCol.Scale, vCol.CodePage)  
  
    Next  
End Sub  
```  
  
## <a name="run-time"></a>Tiempo de ejecución  
 Los componentes con salidas asincrónicas también ejecutan una secuencia diferente de métodos en tiempo de ejecución que otros tipos de componentes. En primer lugar, son los únicos componentes que reciben una llamada tanto a los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> como a los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Los componentes con salidas asincrónicas también requieren acceso a todas las filas entrantes antes de poder iniciar el procesamiento; por tanto, deben almacenar internamente en memoria caché las filas de entrada hasta que se hayan leído todas las filas. Por último, a diferencia de los demás componentes, los componentes con salidas asincrónicas reciben un búfer de entrada y un búfer de salida.  
  
### <a name="understanding-the-buffers"></a>Descripción de los búferes  
 El componente recibe el búfer de entrada durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Este búfer contiene las filas agregadas al búfer por componentes de nivel superior. El búfer también contiene las columnas de la entrada del componente, además de las columnas proporcionadas en la salida de un componente de nivel superior pero que no se agregaron a la colección de entradas del componente asincrónico.  
  
 El búfer de salida, que se proporciona al componente en <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, no contiene inicialmente ninguna fila. El componente agrega filas a este búfer y proporciona el búfer a los componentes de nivel inferior cuando está lleno. El búfer de salida contiene las columnas definidas en la colección de columnas de salida del componente, además de cualquier columna que otros componentes de nivel inferior hayan agregado a sus salidas.  
  
 Éste es un comportamiento diferente del que sucede en los componentes con salidas sincrónicas, que reciben un único búfer compartido. El búfer compartido de un componente con salidas sincrónicas contiene las columnas de entrada y salida del componente, además de las columnas agregadas a las salidas de los componentes de nivel superior y de nivel inferior.  
  
### <a name="processing-rows"></a>Procesar las filas  
  
#### <a name="caching-input-rows"></a>Almacenar en memoria caché las filas de entrada  
 Al escribir un componente con salidas asincrónicas, existen tres opciones para agregar filas al búfer de salida. Puede agregarlas cuando se reciben las filas de entrada, puede almacenarlas en caché hasta que el componente haya recibido todas las filas del componente de nivel superior o puede agregarlas cuando sea adecuado realizarlo para el componente. El método que elija dependerá de los requisitos del componente. Por ejemplo, el componente Sort requiere que se reciban todas las filas de nivel superior antes de ordenarlas. Por tanto, espera hasta que se han leído todas las filas antes de agregarlas al búfer de salida.  
  
 El componente debe almacenar internamente en la memoria caché las filas que se reciben en el búfer de entrada hasta estar listo para procesarlas. Las filas del búfer de entrada se pueden almacenar en caché en una tabla de datos, una matriz multidimensional o cualquier otra estructura interna.  
  
#### <a name="adding-output-rows"></a>Agregar las filas de salida  
 Tanto si agrega las filas al búfer de salida a medida que se reciben como si lo hace después de recibir todas las filas, esta operación se lleva a cabo mediante una llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.AddRow%2A> en el búfer de salida. Después de agregar la fila, establece los valores de las columnas de la nueva fila.  
  
 En ocasiones existen más columnas en el búfer de salida que en la colección de columnas de salida del componente, por lo que debe localizar el índice de la columna correspondiente en el búfer antes de establecer su valor. El método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> devuelve el índice de la columna en la fila del búfer con el identificador de linaje especificado, que se utiliza a continuación para asignar el valor a la columna de búfer.  
  
 El método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, al que se llama antes del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, es el primer método donde está disponible la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> y la primera oportunidad para localizar los índices de las columnas en los búferes de entrada y salida.  
  
## <a name="sample"></a>Muestra  
 En el ejemplo siguiente se muestra un componente de transformación simple con salidas asincrónicas que agrega filas al búfer de salida a medida que se reciben. En este ejemplo no se muestran todos los métodos ni funcionalidad tratados en este tema. Muestra los métodos importantes que cada componente de transformación personalizado con salidas asincrónicas debe invalidar, pero no contiene código para la validación en tiempo de diseño. Además, el código de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> supone que la colección de columnas de salida incluye una columna por cada columna de la colección de columnas de entrada.  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
   [DtsPipelineComponent(DisplayName = "AsynchronousOutput")]  
   public class AsynchronousOutput : PipelineComponent  
   {  
      PipelineBuffer outputBuffer;  
      int[] inputColumnBufferIndexes;  
      int[] outputColumnBufferIndexes;  
  
      public override void ProvideComponentProperties()  
      {  
         // Let the base class add the input and output objects.  
         base.ProvideComponentProperties();  
  
         // Name the input and output, and make the  
         // output asynchronous.  
         ComponentMetaData.InputCollection[0].Name = "Input";  
         ComponentMetaData.OutputCollection[0].Name = "AsyncOutput";  
         ComponentMetaData.OutputCollection[0].SynchronousInputID = 0;  
      }  
      public override void PreExecute()  
      {  
         IDTSInput100 input = ComponentMetaData.InputCollection[0];  
         IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
         inputColumnBufferIndexes = new int[input.InputColumnCollection.Count];  
         outputColumnBufferIndexes = new int[output.OutputColumnCollection.Count];  
  
         for (int col = 0; col < input.InputColumnCollection.Count; col++)  
            inputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection[col].LineageID);  
  
         for (int col = 0; col < output.OutputColumnCollection.Count; col++)  
            outputColumnBufferIndexes[col] = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection[col].LineageID);  
  
      }  
  
      public override void PrimeOutput(int outputs, int[] outputIDs, PipelineBuffer[] buffers)  
      {  
         if (buffers.Length != 0)  
            outputBuffer = buffers[0];  
      }  
      public override void ProcessInput(int inputID, PipelineBuffer buffer)  
      {  
            // Advance the buffer to the next row.  
            while (buffer.NextRow())  
            {  
               // Add a row to the output buffer.  
               outputBuffer.AddRow();  
               for (int x = 0; x < inputColumnBufferIndexes.Length; x++)  
               {  
                  // Copy the data from the input buffer column to the output buffer column.  
                  outputBuffer[outputColumnBufferIndexes[x]] = buffer[inputColumnBufferIndexes[x]];  
               }  
            }  
         if (buffer.EndOfRowset)  
         {  
            // EndOfRowset on the input buffer is true.  
            // Set EndOfRowset on the output buffer.  
            outputBuffer.SetEndOfRowset();  
         }  
      }  
   }  
}  
```  
  
```vb  
Imports System  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper  
  
Namespace Microsoft.Samples.SqlServer.Dts  
  
    <DtsPipelineComponent(DisplayName:="AsynchronousOutput")> _  
    Public Class AsynchronousOutput  
  
        Inherits PipelineComponent  
  
        Private outputBuffer As PipelineBuffer  
        Private inputColumnBufferIndexes As Integer()  
        Private outputColumnBufferIndexes As Integer()  
  
        Public Overrides Sub ProvideComponentProperties()  
  
            ' Let the base class add the input and output objects.  
            Me.ProvideComponentProperties()  
  
            ' Name the input and output, and make the  
            ' output asynchronous.  
            ComponentMetaData.InputCollection(0).Name = "Input"  
            ComponentMetaData.OutputCollection(0).Name = "AsyncOutput"  
            ComponentMetaData.OutputCollection(0).SynchronousInputID = 0  
        End Sub  
  
        Public Overrides Sub PreExecute()  
  
            Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
            Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
            ReDim inputColumnBufferIndexes(input.InputColumnCollection.Count)  
            ReDim outputColumnBufferIndexes(output.OutputColumnCollection.Count)  
  
            For col As Integer = 0 To input.InputColumnCollection.Count  
                inputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(input.Buffer, input.InputColumnCollection(col).LineageID)  
            Next  
  
            For col As Integer = 0 To output.OutputColumnCollection.Count  
                outputColumnBufferIndexes(col) = BufferManager.FindColumnByLineageID(output.Buffer, output.OutputColumnCollection(col).LineageID)  
            Next  
  
        End Sub  
        Public Overrides Sub PrimeOutput(ByVal outputs As Integer, ByVal outputIDs As Integer(), ByVal buffers As PipelineBuffer())  
  
            If buffers.Length <> 0 Then  
                outputBuffer = buffers(0)  
            End If  
  
        End Sub  
  
        Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
                ' Advance the buffer to the next row.  
                While (buffer.NextRow())  
  
                    ' Add a row to the output buffer.  
                    outputBuffer.AddRow()  
                    For x As Integer = 0 To inputColumnBufferIndexes.Length  
  
                        ' Copy the data from the input buffer column to the output buffer column.  
                        outputBuffer(outputColumnBufferIndexes(x)) = buffer(inputColumnBufferIndexes(x))  
  
                    Next  
                End While  
  
            If buffer.EndOfRowset = True Then  
                ' EndOfRowset on the input buffer is true.  
                ' Set the end of row set on the output buffer.  
                outputBuffer.SetEndOfRowset()  
            End If  
        End Sub  
    End Class  
End Namespace  
```  
  
## <a name="see-also"></a>Vea también  
 [Desarrollar un componente de transformación personalizado con salidas sincrónicas](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)   
 [Understanding Synchronous and Asynchronous Transformations](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)  (Descripción de las transformaciones sincrónicas y asincrónicas)  
 [Crear una transformación asincrónica con el componente de script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
  
  
