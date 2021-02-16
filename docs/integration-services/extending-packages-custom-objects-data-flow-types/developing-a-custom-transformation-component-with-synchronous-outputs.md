---
description: Desarrollar un componente de transformación personalizado con salidas sincrónicas
title: Desarrollar un componente de transformación personalizado mediante salidas sincrónicas | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
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
- ProcessInput method
- buffer allocations [Integration Services]
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- output columns [Integration Services]
- data flow components [Integration Services], transformation components
ms.assetid: b694d21f-9919-402d-9192-666c6449b0b7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 87a53ff91f4ae7d6237fb3f9640420dd964b607f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348229"
---
# <a name="developing-a-custom-transformation-component-with-synchronous-outputs"></a>Desarrollar un componente de transformación personalizado con salidas sincrónicas

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Los componentes de transformación con salidas sincrónicas reciben filas de los componentes de nivel superior y leen o modifican los valores de las columnas de estas filas al pasarlas a los componentes de nivel inferior. También pueden definir columnas de salida adicionales que se derivan de las columnas que proporcionan los componentes de nivel superior, pero no agregan filas al flujo de datos. Para obtener más información acerca de la diferencia entre los componentes sincrónicos y asincrónicos, vea [Descripción de las transformaciones sincrónicas y asincrónicas](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Este tipo de componente es el adecuado para las tareas en las que se modifican los datos insertados cuando se proporcionan al componente, y donde el componente no tiene que ver todas las filas antes de procesarlas. Se trata de desarrollar el componente más sencillo debido a que las transformaciones con salidas sincrónicas no conectan normalmente a orígenes de datos externos, administran columnas de metadatos externas o agregan filas a búferes de salida.  
  
 Crear un componente de transformación con salidas sincrónicas implica agregar un objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSInput100> que contendrá las columnas de nivel superior seleccionadas para el componente, así como un objeto <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutput100> que puede contener las columnas derivadas creadas por el componente. También incluye implementar métodos en tiempo de diseño y proporcionar código que lee o modifica las columnas en las filas del búfer de entrada durante la ejecución.  
  
 En esta sección se facilita la información necesaria para implementar un componente de transformación personalizado y se proporcionan ejemplos de código que sirven de ayuda para comprender mejor los conceptos.  
  
## <a name="design-time"></a>Tiempo de diseño  
 El código en tiempo de diseño para este componente implica crear entradas y salidas, agregar cualquier columna de salida adicional que genere el componente y validar la configuración del componente.  
  
### <a name="creating-the-component"></a>Crear el componente  
 Las entradas, salidas y las propiedades personalizadas del componente se crean normalmente durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A>. Hay dos maneras para agregar la entrada y salida de un componente de transformación con salidas sincrónicas. Puede usar la implementación de la clase base del método y, a continuación, modificar la entrada y salida predeterminada que se crea o puede agregar explícitamente usted mismo la entrada y salida.  
  
 En el ejemplo de código siguiente se muestra una implementación de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProvideComponentProperties%2A> que agrega explícitamente los objetos de entrada y salida. La llamada a la clase base que realizaría la misma acción se incluye en un comentario.  
  
```csharp  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime;  
  
namespace Microsoft.Samples.SqlServer.Dts  
{  
    [DtsPipelineComponent(DisplayName = "SynchronousComponent", ComponentType = ComponentType.Transform)]  
    public class SyncComponent : PipelineComponent  
    {  
  
        public override void ProvideComponentProperties()  
        {  
            // Add the input.  
            IDTSInput100 input = ComponentMetaData.InputCollection.New();  
            input.Name = "Input";  
  
            // Add the output.  
            IDTSOutput100 output = ComponentMetaData.OutputCollection.New();  
            output.Name = "Output";  
            output.SynchronousInputID = input.ID;  
  
            // Alternatively, you can let the base class add the input and output  
            // and set the SynchronousInputID of the output to the ID of the input.  
            // base.ProvideComponentProperties();  
        }  
    }  
}  
```  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
Imports Microsoft.SqlServer.Dts.Runtime  
  
<DtsPipelineComponent(DisplayName:="SynchronousComponent", ComponentType:=ComponentType.Transform)> _  
Public Class SyncComponent  
    Inherits PipelineComponent  
  
    Public Overrides Sub ProvideComponentProperties()  
  
        ' Add the input.  
        Dim input As IDTSInput100 = ComponentMetaData.InputCollection.New()  
        input.Name = "Input"  
  
        ' Add the output.  
        Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection.New()  
        output.Name = "Output"  
        output.SynchronousInputID = Input.ID  
  
        ' Alternatively, you can let the base class add the input and output  
        ' and set the SynchronousInputID of the output to the ID of the input.  
        ' base.ProvideComponentProperties();  
  
    End Sub  
  
End Class  
```  
  
### <a name="creating-and-configuring-output-columns"></a>Crear y configurar columnas de salida  
 Aunque los componentes de transformación con salidas sincrónicas no agregan filas a los búferes, pueden agregar columnas de salidas adicionales a su salida. Normalmente, cuando un componente agrega una columna de salida, los valores de la nueva columna de salida se derivan en tiempo de ejecución de los datos contenidos en una o varias de las columnas que el componente de nivel superior proporciona al componente.  
  
 Una vez creada una columna de salida, se deben establecer sus propiedades de tipo de datos. Para establecer las propiedades de tipo de datos de una columna de salida se requiere un tratamiento especial y se realiza llamando al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.SetDataTypeProperties%2A>. Este método es obligatorio porque las propiedades <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Length%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.Precision%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.CodePage%2A> son de solo lectura individualmente, debido a que cada propiedad depende de los valores de la otra. Este método garantiza que los valores de las propiedades se establezcan de forma coherente y que la tarea de flujo de datos valide que se establezcan correctamente.  
  
 El <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A> de la columna determina los valores que se establecen para otras propiedades. En la tabla siguiente se muestran los requisitos de las propiedades dependientes para cada <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputColumn100.DataType%2A>. En los tipos de datos no enumerados, sus propiedades dependientes se establecen en cero.  
  
|DataType|Length|Escala|Precisión|CodePage|  
|--------------|------------|-----------|---------------|--------------|  
|DT_DECIMAL|0|Mayor que 0 y menor o igual que 28.|0|0|  
|DT_CY|0|0|0|0|  
|DT_NUMERIC|0|Mayor que 0 y menor o igual que 28 y menor que Precisión.|Mayor o igual que 1 y menor o igual que 38.|0|  
|DT_BYTES|Mayor que 0.|0|0|0|  
|DT_STR|Mayor que 0 y menor que 8000.|0|0|Distinto de 0 y una página de códigos válida.|  
|DT_WSTR|Mayor que 0 y menor que 4.000.|0|0|0|  
  
 Puesto que las restricciones en las propiedades de tipo de datos se basan en el tipo de datos de la columna de salida, debe elegir el tipo de datos de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] correcto al trabajar con tipos administrados. La clase base proporciona tres métodos del asistente, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ConvertBufferDataTypeToFitManaged%2A>, <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferTypeToDataRecordType%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.DataRecordTypeToBufferType%2A>, que ayudan a los programadores de componentes administrados a seleccionar un tipo de datos de [!INCLUDE[ssIS](../../includes/ssis-md.md)] dado un tipo administrado. Estos métodos convierten los tipos de datos administrados en tipos de datos de [!INCLUDE[ssIS](../../includes/ssis-md.md)] y viceversa.  
  
## <a name="run-time"></a>Tiempo de ejecución  
 En general, la implementación en tiempo de ejecución de un elemento del componente se divide en dos tareas: búsqueda de las columnas de entrada y salida del componente en el búfer, y lectura o escritura de los valores de estas columnas en las filas del búfer de entrada.  
  
### <a name="locating-columns-in-the-buffer"></a>Localizar columnas en el búfer  
 El número de columnas en los búferes que se proporcionan a un componente durante la ejecución, superará probablemente el número de columnas en las colecciones de entrada o salida del componente. Esto se debe a que cada búfer contiene todas las columnas de salida definidas en los componentes de un flujo de datos. Para asegurarse de que las columnas de búfer coinciden correctamente con las columnas de entrada o salida, los programadores de componentes deben usar el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSBufferManager100.FindColumnByLineageID%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A>. Este método busca una columna en el búfer especificado por su identificador de linaje. Normalmente, las columnas se encuentran durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> porque se trata del primer método en tiempo de ejecución donde la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.BufferManager%2A> pasa a estar disponible.  
  
 En el siguiente ejemplo de código se muestra un componente que busca índices de columnas en su columna de colección de columnas de entrada y salida durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A>. Los índices de columna se almacenan en una matriz entera y el componente puede tener acceso a ellos durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>.  
  
```csharp  
int []inputColumns;  
int []outputColumns;  
  
public override void PreExecute()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSOutput100 output = ComponentMetaData.OutputCollection[0];  
  
    inputColumns = new int[input.InputColumnCollection.Count];  
    outputColumns = new int[output.OutputColumnCollection.Count];  
  
    for(int col=0; col < input.InputColumnCollection.Count; col++)  
    {  
        IDTSInputColumn100 inputColumn = input.InputColumnCollection[col];  
        inputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID);  
    }  
  
    for(int col=0; col < output.OutputColumnCollection.Count; col++)  
    {  
        IDTSOutputColumn100 outputColumn = output.OutputColumnCollection[col];  
        outputColumns[col] = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID);  
    }  
  
}  
```  
  
```vb  
Public Overrides Sub PreExecute()  
  
    Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)  
    Dim output As IDTSOutput100 = ComponentMetaData.OutputCollection(0)  
  
    ReDim inputColumns(input.InputColumnCollection.Count)  
    ReDim outputColumns(output.OutputColumnCollection.Count)  
  
    For col As Integer = 0 To input.InputColumnCollection.Count  
  
        Dim inputColumn As IDTSInputColumn100 = input.InputColumnCollection(col)  
        inputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, inputColumn.LineageID)  
    Next  
  
    For col As Integer = 0 To output.OutputColumnCollection.Count  
  
        Dim outputColumn As IDTSOutputColumn100 = output.OutputColumnCollection(col)  
        outputColumns(col) = BufferManager.FindColumnByLineageID(input.Buffer, outputColumn.LineageID)  
    Next  
  
End Sub  
```  
  
### <a name="processing-rows"></a>Procesar las filas  
 Los componentes reciben objetos <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineBuffer> que contienen filas y columnas del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A>. Durante este método las filas en el búfer se iteran y las columnas identificadas durante <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PreExecute%2A> se pueden leer y modificar. La tarea de flujo de datos llama repetidamente al método hasta que el componente de nivel superior no proporciona más filas.  
  
 Se lee o se escribe una columna individual en el búfer mediante el método de acceso al indizador de matrices o mediante uno de los métodos **Get** o **Set**. Los métodos **Get** y **Set** son más eficaces y se deben usar cuando se conoce el tipo de datos de la columna en el búfer.  
  
 En el siguiente ejemplo de código se muestra una implementación del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ProcessInput%2A> que procesa las filas entrantes.  
  
```csharp  
public override void ProcessInput( int InputID, PipelineBuffer buffer)  
{  
       while( buffer.NextRow())  
       {  
            for(int x=0; x < inputColumns.Length;x++)  
            {  
                if(!buffer.IsNull(inputColumns[x]))  
                {  
                    object columnData = buffer[inputColumns[x]];  
                    // TODO: Modify the column data.  
                    buffer[inputColumns[x]] = columnData;  
                }  
            }  
  
      }  
}  
```  
  
```vb  
Public Overrides Sub ProcessInput(ByVal InputID As Integer, ByVal buffer As PipelineBuffer)  
  
        While (buffer.NextRow())  
  
            For x As Integer = 0 To inputColumns.Length  
  
                if buffer.IsNull(inputColumns(x)) = false then  
  
                    Dim columnData As Object = buffer(inputColumns(x))  
                    ' TODO: Modify the column data.  
                    buffer(inputColumns(x)) = columnData  
  
                End If  
            Next  
  
        End While  
End Sub  
```  
  
## <a name="sample"></a>Muestra  
 En el ejemplo siguiente se muestra un componente de transformación simple con salidas sincrónicas que convierte en mayúsculas los valores de todas las columnas de cadena. En este ejemplo no se muestran todos los métodos ni funcionalidad tratados en este tema. Muestra los métodos importantes que cada componente de transformación personalizado con salidas sincrónicas debe invalidar, pero no contiene código para la validación en tiempo de diseño.  
  
```csharp  
using System;  
using System.Collections;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
using Microsoft.SqlServer.Dts.Runtime.Wrapper;  
  
namespace Uppercase  
{  
  [DtsPipelineComponent(DisplayName = "Uppercase")]  
  public class Uppercase : PipelineComponent  
  {  
    ArrayList m_ColumnIndexList = new ArrayList();  
  
    public override void ProvideComponentProperties()  
    {  
      base.ProvideComponentProperties();  
      ComponentMetaData.InputCollection[0].Name = "Uppercase Input";  
      ComponentMetaData.OutputCollection[0].Name = "Uppercase Output";  
    }  
  
    public override void PreExecute()  
    {  
      IDTSInput100 input = ComponentMetaData.InputCollection[0];  
      IDTSInputColumnCollection100 inputColumns = input.InputColumnCollection;  
  
      foreach (IDTSInputColumn100 column in inputColumns)  
      {  
        if (column.DataType == DataType.DT_STR || column.DataType == DataType.DT_WSTR)  
        {  
          m_ColumnIndexList.Add((int)BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID));  
        }  
      }  
    }  
  
    public override void ProcessInput(int inputID, PipelineBuffer buffer)  
    {  
      while (buffer.NextRow())  
      {  
        foreach (int columnIndex in m_ColumnIndexList)  
        {  
          string str = buffer.GetString(columnIndex);  
          buffer.SetString(columnIndex, str.ToUpper());  
        }  
      }  
    }  
  }  
}  
```  
  
```vb  
Imports System   
Imports System.Collections   
Imports Microsoft.SqlServer.Dts.Pipeline   
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper   
Imports Microsoft.SqlServer.Dts.Runtime.Wrapper   
Namespace Uppercase   
  
 <DtsPipelineComponent(DisplayName="Uppercase")> _   
 Public Class Uppercase   
 Inherits PipelineComponent   
   Private m_ColumnIndexList As ArrayList = New ArrayList   
  
   Public  Overrides Sub ProvideComponentProperties()   
     MyBase.ProvideComponentProperties   
     ComponentMetaData.InputCollection(0).Name = "Uppercase Input"   
     ComponentMetaData.OutputCollection(0).Name = "Uppercase Output"   
   End Sub   
  
   Public  Overrides Sub PreExecute()   
     Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
     Dim inputColumns As IDTSInputColumnCollection100 = input.InputColumnCollection   
     For Each column As IDTSInputColumn100 In inputColumns   
       If column.DataType = DataType.DT_STR OrElse column.DataType = DataType.DT_WSTR Then   
         m_ColumnIndexList.Add(CType(BufferManager.FindColumnByLineageID(input.Buffer, column.LineageID), Integer))   
       End If   
     Next   
   End Sub   
  
   Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
     While buffer.NextRow   
       For Each columnIndex As Integer In m_ColumnIndexList   
         Dim str As String = buffer.GetString(columnIndex)   
         buffer.SetString(columnIndex, str.ToUpper)   
       Next   
     End While   
   End Sub   
 End Class   
End Namespace  
```  
  
## <a name="see-also"></a>Vea también  
 [Developing a Custom Transformation Component with Asynchronous Outputs (Desarrollar un componente de transformación personalizado con salidas asincrónicas)](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-asynchronous-outputs.md)   
 [Understanding Synchronous and Asynchronous Transformations](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md)  (Descripción de las transformaciones sincrónicas y asincrónicas)  
 [Crear una transformación sincrónica con el componente de script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-synchronous-transformation-with-the-script-component.md)  
  
  
