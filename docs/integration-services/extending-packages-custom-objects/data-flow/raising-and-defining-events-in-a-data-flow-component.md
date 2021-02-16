---
description: Provocar y definir eventos en un componente de flujo de datos
title: Provocar y definir eventos en un componente de flujo de datos | Microsoft Docs
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
- data flow components [Integration Services], events
- events [Integration Services], custom
- custom events [Integration Services]
- custom data flow components [Integration Services], events
- events [Integration Services], raising
- predefined events [Integration Services]
ms.assetid: 1d8c5358-9384-47a8-b7cb-7b0650384119
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 1d6e0a5d7694323f22a9262df37353f8117b9666
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343658"
---
# <a name="raising-and-defining-events-in-a-data-flow-component"></a>Provocar y definir eventos en un componente de flujo de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Los desarrolladores de componentes pueden producir un subconjunto de los eventos definido en la interfaz <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents> mediante una llamada a los métodos expuestos en la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. También puede definir eventos personalizados con la colección <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A> y provocarlos durante la ejecución con el método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>. En esta sección se describe cómo crear y provocar un evento y se proporcionan instrucciones sobre el momento en que se deben provocar los eventos en tiempo de diseño.  
  
## <a name="raising-events"></a>Provocar eventos  
 Los componentes generan eventos mediante los métodos **Fire\<X>** de la interfaz <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100>. Puede provocar eventos durante el diseño y la ejecución de los componentes. Normalmente, en el diseño de los componentes, se llama a los métodos <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireWarning%2A> durante la validación. Estos eventos muestran mensajes en el panel **Lista de errores** de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)] y proporcionan información a los usuarios del componente cuando este está configurado incorrectamente.  
  
 Los componentes también pueden provocar eventos en cualquier momento durante la ejecución. Los eventos permiten a los programadores de componentes proporcionar información a los usuarios del componente cuando éste se ejecuta. Es probable que la llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireError%2A> durante la ejecución produzca un error en el paquete.  
  
## <a name="defining-and-raising-custom-events"></a>Definir y provocar eventos personalizados  
  
### <a name="defining-a-custom-event"></a>Definir un evento personalizado  
 Los eventos personalizados se crean mediante una llamada al método <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> de la colección <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.EventInfos%2A>. La tarea de flujo de datos establece esta colección, que se proporciona al programador de componentes a través de la clase base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent>. Esta clase contiene eventos personalizados definidos por la tarea de flujo de datos y eventos personalizados definidos por el componente durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A>.  
  
 Los eventos personalizados de un componente no se conservan en el paquete XML. Por tanto, se llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> durante el diseño y la ejecución para permitir al componente definir los eventos que provoca.  
  
 El parámetro *allowEventHandlers* del método <xref:Microsoft.SqlServer.Dts.Runtime.Wrapper.IDTSEventInfos100.Add%2A> especifica si el componente permite crear objetos <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> para el evento. Observe que los objetos <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> son sincrónicos. Por tanto, el componente no reanuda la ejecución hasta que ha terminado de ejecutarse un objeto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> adjuntado al evento personalizado. Si el parámetro *allowEventHandlers* es **true**, cada parámetro del evento pasa a estar disponible automáticamente para cualquier objeto <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandler> a través de variables que el tiempo de ejecución de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)] crea y rellena de forma automática.  
  
### <a name="raising-a-custom-event"></a>Provocar un evento personalizado  
 Los componentes provocan eventos personalizados mediante una llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A> y proporcionando el nombre, texto y parámetros del evento. Si el parámetro *allowEventHandlers* es **true**, el entorno de ejecución de [!INCLUDE[ssIS](../../../includes/ssis-md.md)] ejecuta los elementos <xref:Microsoft.SqlServer.Dts.Runtime.DtsEventHandlers> creados para el evento personalizado.  
  
### <a name="custom-event-sample"></a>Ejemplo de evento personalizado  
 En el ejemplo de código siguiente se muestra un componente que define un evento personalizado durante el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.RegisterEvents%2A> y, a continuación, provoca el evento en tiempo de ejecución mediante una llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSComponentMetaData100.FireCustomEvent%2A>.  
  
```csharp  
public override void RegisterEvents()  
{  
    string [] parameterNames = new string[2]{"RowCount", "StartTime"};  
    ushort [] parameterTypes = new ushort[2]{ DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)};  
    string [] parameterDescriptions = new string[2]{"The number of rows to sort.", "The start time of the Sort operation."};  
    EventInfos.Add("StartingSort","Fires when the component begins sorting the rows.",false,ref parameterNames, ref parameterTypes, ref parameterDescriptions);  
}  
public override void ProcessInput(int inputID, PipelineBuffer buffer)  
{  
    while (buffer.NextRow())  
    {  
       // Process buffer rows.  
    }  
  
    IDTSEventInfo100 eventInfo = EventInfos["StartingSort"];  
    object []arguments = new object[2]{buffer.RowCount, DateTime.Now };  
    ComponentMetaData.FireCustomEvent("StartingSort", "Beginning sort operation.", ref arguments, ComponentMetaData.Name, ref FireSortEventAgain);  
}  
```  
  
```vb  
Public  Overrides Sub RegisterEvents()   
  Dim parameterNames As String() = New String(2) {"RowCount", "StartTime"}   
  Dim parameterTypes As System.UInt16() = New System.UInt16(2) {DtsConvert.VarTypeFromTypeCode(TypeCode.Int32), DtsConvert.VarTypeFromTypeCode(TypeCode.DateTime)}   
  Dim parameterDescriptions As String() = New String(2) {"The number of rows to sort.", "The start time of the Sort operation."}   
  EventInfos.Add("StartingSort", "Fires when the component begins sorting the rows.", False, parameterNames, parameterTypes, parameterDescriptions)   
End Sub   
  
Public  Overrides Sub ProcessInput(ByVal inputID As Integer, ByVal buffer As PipelineBuffer)   
  While buffer.NextRow   
  End While   
  Dim eventInfo As IDTSEventInfo100 = EventInfos("StartingSort")   
  Dim arguments As Object() = New Object(2) {buffer.RowCount, DateTime.Now}   
  ComponentMetaData.FireCustomEvent("StartingSort", _  
    "Beginning sort operation.", arguments, _  
    ComponentMetaData.Name, FireSortEventAgain)   
End Sub  
```  

## <a name="see-also"></a>Consulte también  
 [Controladores de eventos de Integration Services &#40;SSIS&#41;](../../../integration-services/integration-services-ssis-event-handlers.md)   
 [Agregar un controlador de eventos a un paquete](../../integration-services-ssis-event-handlers.md)  
  
