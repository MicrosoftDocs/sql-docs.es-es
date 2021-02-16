---
description: Métodos en tiempo de ejecución de un componente de flujo de datos
title: Métodos en tiempo de ejecución de un componente de flujo de datos | Microsoft Docs
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
- run-time [Integration Services]
- data flow components [Integration Services], run-time methods
ms.assetid: fd9e4317-18dd-43af-bbdc-79db32183ac4
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 8fa0106c903a509eb2b4f734b667323b6d71fd74
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343634"
---
# <a name="run-time-methods-of-a-data-flow-component"></a>Métodos en tiempo de ejecución de un componente de flujo de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  En tiempo de ejecución, la tarea de flujo de datos examina la secuencia de componentes, prepara un plan de ejecución y administra un grupo de subprocesos de trabajo que ejecutan el plan de trabajo. La tarea carga filas de datos de los orígenes, las procesa a través de las transformaciones y, a continuación, las guarda en los destinos.  
  
## <a name="sequence-of-method-execution"></a>Secuencia de ejecución del método  
 Durante la ejecución de un componente de flujo de datos, se llama a un subconjunto de métodos en la clase base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>. Los métodos, y la secuencia en la que se llaman, siempre son los mismos, con la excepción de los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Se llama a estos dos métodos dependiendo de la existencia y configuración de los objetos <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> de un componente.  
  
 La lista siguiente muestra los métodos en el orden en el que se llaman durante la ejecución del componente. Tenga en cuenta que cuando se llama a <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>, siempre se llama antes de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrepareForExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.AcquireConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PostExecute%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReleaseConnections%2A>  
  
-   <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Cleanup%2A>  
  
### <a name="primeoutput-method"></a>Método PrimeOutput  
 Se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> cuando un componente tiene por lo menos una salida, adjuntada a un componente de nivel inferior a través de un objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100>, y la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100.SynchronousInputID%2A> de la salida es cero. Se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.PrimeOutput%2A> para los componentes de origen y para las transformaciones con salidas asincrónicas. A diferencia del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> que se describe a continuación, cada componente que lo requiere llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> una sola vez.  
  
### <a name="processinput-method"></a>Método ProcessInput   
 Un objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSPath100> para los componentes que tienen al menos una salida adjuntada a un componente de nivel superior. Se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSRuntimeComponent100.ProcessInput%2A> para los componentes de destino y para las transformaciones con salidas sincrónicas. Se llama a <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> varias veces hasta que no hay más filas de componentes de nivel superior que procesar.  
  
## <a name="working-with-inputs-and-outputs"></a>Trabajar con entradas y salidas  
 En tiempo de ejecución, los componentes de flujo de datos realizan las tareas siguientes:  
  
-   Los componentes de origen agregan filas.  
  
-   Los componentes de transformación con salidas sincrónicas reciben las filas que proporcionan los componentes de origen.  
  
-   Los componentes de transformación con salidas asincrónicas reciben filas y agregan filas.  
  
-   Los componentes de destino reciben filas y, a continuación, las cargan en un destino.  
  
 Durante la ejecución, la tarea de flujo de datos asigna los objetos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> que contienen todas las columnas definidas en las colecciones de columna de salida de una secuencia de componentes. Por ejemplo, si uno de cada cuatro componentes en una secuencia del flujo de datos agrega una columna de salida a su colección de columna de salida, el búfer que se proporciona a cada componente contiene cuatro columnas, una por cada columna de salida por componente. Debido a este comportamiento, un componente recibe a veces búferes que contienen columnas que no usará el componente.  
  
 Puesto que los búferes que recibe su componente pueden contener columnas que el componente no usará, busque las columnas que desea usar en las colecciones de columna de entrada y salida de su componente en el búfer que la tarea de flujo de datos proporciona al componente. Para ello, use el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Por razones de rendimiento, esta tarea normalmente se realiza durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>, en lugar de en <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 Se llama a <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> antes que a los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, y es la primera oportunidad para que un componente realice esta tarea después de que <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> pase a estar disponible para el componente. Durante este método, el componente debería buscar sus columnas en los búferes y almacenar internamente esta información, de manera que las columnas se puedan usar en cualquiera de los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> o <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
 En el siguiente ejemplo de código se muestra cómo un componente de transformación con una salida sincrónica busca sus columnas de entrada en el búfer durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>.  
  
```csharp  
private int []bufferColumnIndex;  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    bufferColumnIndex = new int[input.InputColumnCollection.Count];  
  
    for( int x=0; x < input.InputColumnCollection.Count; x++)  
    {  
        IDTSInputColumn100 column = input.InputColumnCollection[x];  
        bufferColumnIndex[x] = BufferManager.FindColumnByLineageID( input.Buffer, column.LineageID);  
    }  
}  
```  
  
```vb  
Dim bufferColumnIndex As Integer()  
  
    Public Overrides Sub PreExecute()  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
  
        ReDim bufferColumnIndex(input.InputColumnCollection.Count)  
  
        For x As Integer = 0 To input.InputColumnCollection.Count  
  
            Dim column As IDTSInputColumn100 = input.InputColumnCollection(x)  
            bufferColumnIndex(x) = BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID)  
  
        Next  
  
    End Sub  
```  
  
### <a name="adding-rows"></a>Agregar filas  
 Los componentes proporcionan filas a los componentes de nivel inferior agregándolas a los objetos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. La tarea de flujo de datos proporciona una matriz de búferes de salida, uno para cada objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> que está conectado a un componente de nivel inferior, como un parámetro al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A>. Los componentes de origen y componentes de transformación con salidas asincrónicas agregan filas a los búferes y llaman al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A> cuando terminan de agregar filas. La tarea de flujo de datos administra los búferes de salida que proporciona a los componentes y, cuando un búfer se llena, automáticamente mueve las filas del búfer al componente siguiente. Se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> una vez por componente, a diferencia del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>, al que se llama varias veces.  
  
 En el siguiente ejemplo de código se muestra cómo un componente agrega filas a sus búferes de salida durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PrimeOutput%2A> y, a continuación, llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.SetEndOfRowset%2A>.  
  
```csharp  
public override void PrimeOutput( int outputs, int []outputIDs,PipelineBuffer []buffers)  
{  
    for( int x=0; x < outputs; x++ )  
    {  
        IDTSOutput100 output = ComponentMetaData.OutputCollection.GetObjectByID( outputIDs[x]);  
        PipelineBuffer buffer = buffers[x];  
  
        // TODO: Add rows to the output buffer.  
    }  
    foreach( PipelineBuffer buffer in buffers )  
    {  
        /// Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset();  
    }  
}  
```  
  
```vb  
public overrides sub PrimeOutput( outputs as Integer , outputIDs() as Integer ,buffers() as PipelineBuffer buffers)  
  
    For x As Integer = 0 To outputs.MaxValue  
  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.GetObjectByID(outputIDs(x))  
        Dim buffer As PipelineBuffer = buffers(x)  
  
        ' TODO: Add rows to the output buffer.  
  
    Next  
  
    For Each buffer As PipelineBuffer In buffers  
  
        ' Notify the data flow task that no more rows are coming.  
        buffer.SetEndOfRowset()  
  
    Next  
  
End Sub  
```  
  
 Para obtener más información sobre el desarrollo de componentes que agregan filas a los búferes de salida, vea [Desarrollar un componente de origen personalizado](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-source-component.md) y [Desarrollar un componente de transformación personalizado con salidas asincrónicas](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md).  
  
### <a name="receiving-rows"></a>Recibir filas  
 Los componentes reciben filas de los componentes de nivel superior en objetos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer>. La tarea de flujo de datos proporciona un objeto <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> que contiene las filas agregadas al flujo de datos por componentes de nivel superior como un parámetro al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Este búfer de entrada se puede usar para examinar y modificar filas y columnas en el búfer, pero no se puede usar para agregar o quitar filas. Se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> varias veces hasta que no hay más búferes disponibles. En la última llamada, la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A> es **true**. Puede iterar sobre la colección de filas en el búfer mediante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A>, que hace avanzar el búfer a la siguiente fila. Este método devuelve **false** cuando el búfer está en la última fila de la colección. No tiene que comprobar la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A>, a menos que tenga que realizar una acción adicional después de que las últimas filas de datos se hayan procesado.  
  
 El texto siguiente muestra el patrón correcto para usar el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.NextRow%2A> y la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer.EndOfRowset%2A>:  
  
 `while (buffer.NextRow())`  
  
 `{`  
  
 `// Do something with each row.`  
  
 `}`  
  
 `if (buffer.EndOfRowset)`  
  
 `{`  
  
 `// Optionally, do something after all rows have been processed.`  
  
 `}`  
  
 En el siguiente ejemplo de código se muestra cómo un componente procesa filas en búferes de entrada durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
public override void ProcessInput( int inputID, PipelineBuffer buffer )  
{  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection.GetObjectByID(inputID);  
        while( buffer.NextRow())  
        {  
            // TODO: Examine the columns in the current row.  
        }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)  
  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.GetObjectByID(inputID)  
        While buffer.NextRow() = True  
  
            ' TODO: Examine the columns in the current row.  
        End While  
  
End Sub  
```  
  
 Para obtener más información sobre el desarrollo de componentes que reciben filas en los búferes de entrada, vea [Desarrollar un componente de destino personalizado](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-destination-component.md) y [Desarrollar un componente de transformación personalizado con salidas sincrónicas](../../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="see-also"></a>Consulte también  
 [Métodos en tiempo de diseño de un componente de flujo de datos](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md)  
  
  
