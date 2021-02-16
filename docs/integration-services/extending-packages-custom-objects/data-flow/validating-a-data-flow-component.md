---
description: Validar un componente de flujo de datos
title: Validar un componente de flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
- CSharp
helpviewer_keywords:
- ReinitializeMetaData method
- Validate method
- component validation [Integration Services]
- custom data flow components [Integration Services], validating
- validation [Integration Services], components
- data flow components [Integration Services], validating
- validation [Integration Services]
ms.assetid: 1a7d5925-b387-4e31-af7f-c7f3c5151040
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fc86f379cdbd324c47adcbdb9b610399ca2f28af
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354380"
---
# <a name="validating-a-data-flow-component"></a>Validar un componente de flujo de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  El método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> de la clase base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> se proporciona para impedir la ejecución de un componente que no se ha configurado correctamente. Utilice este método para comprobar que un componente tiene el número esperado de objetos de entrada y salida, que las propiedades personalizadas del componente tienen valores aceptables y que se especifican las conexiones necesarias. Utilice este método también para comprobar que las columnas de las colecciones de entrada y salida tienen los tipos de datos correctos y que el elemento <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSUsageType> de cada columna se ha establecido de forma adecuada para el componente. La implementación de la clase base ayuda en el proceso de validación al comprobar la colección de columnas de entrada del componente y asegurarse de que cada columna de la colección hace referencia a una columna del elemento <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.IDTSOutputCollection100> del componente de nivel superior.  
  
## <a name="validate-method"></a>Método Validate  
 Se llama repetidamente al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> al editar un componente en el Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)]. Puede proporcionar comentarios al diseñador y a los usuarios del componente a través del valor de enumeración <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus> devuelto y de la exposición de advertencias y errores. La enumeración <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus> contiene tres valores que indican diversas fases de error y un cuarto valor, <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISVALID>, que indica si el componente se ha configurado correctamente y si está listo para ejecutarse.  
  
 El valor <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> indica que existe un error en <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> y que el componente puede reparar los errores. Si un componente detecta un error de metadatos que puede reparar, no debería corregir el error en el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> y <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> no debería modificarse durante la validación. En su lugar, el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A> debería devolver solamente <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA>, y el componente debería reparar el error en una llamada al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>, como se describe más adelante en esta sección.  
  
 El valor <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISBROKEN> indica que el componente tiene un error que puede rectificarse mediante la edición del componente en el diseñador. Este error se produce normalmente porque no se ha especificado una propiedad personalizada o una conexión necesaria, o bien se ha establecido de forma incorrecta.  
  
 El valor final del error es <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_ISCORRUPT>, lo que indica que el componente ha detectado errores que solo deberían producirse si la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A> se ha modificado directamente, ya sea mediante la edición del XML del paquete o el uso del modelo de objetos. Por ejemplo, este tipo de error se produce cuando un componente ha agregado una sola entrada, pero la validación detecta que existe más de una entrada en <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ComponentMetaData%2A>. Los errores que generan este valor devuelto solamente pueden repararse si se restablece el componente mediante el botón **Restablecer** del cuadro de diálogo **Editor avanzado**.  
  
 Además de devolver valores de error, los componentes proporcionan comentarios mediante la exposición de advertencias o errores durante la validación. Los métodos <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireWarning%2A> y <xref:Microsoft.SqlServer.Dts.Runtime.IDTSComponentEvents.FireError%2A> proporcionan este mecanismo. Cuando se llama a estos métodos, estos eventos se exponen en la ventana **Lista de errores** de [!INCLUDE[ssBIDevStudioFull](../../../includes/ssbidevstudiofull-md.md)]. A continuación, los desarrolladores de componentes pueden proporcionar comentarios directamente a los usuarios sobre los errores que se han producido y, si procede, sobre cómo corregirlos.  
  
 En el siguiente ejemplo de código se muestra una implementación invalidada del método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>.  
  
```csharp  
public override DTSValidationStatus Validate()  
{  
    bool pbCancel = false;  
  
    // Validate that there is one input.  
    if (ComponentMetaData.InputCollection.Count != 1)  
    {  
    ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, out pbCancel);  
    return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Validate that the UserName custom property is set.  
    if (ComponentMetaData.CustomPropertyCollection["UserName"].Value == null || ((string)ComponentMetaData.CustomPropertyCollection["UserName"].Value).Length == 0)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISBROKEN;  
    }  
  
    // Validate that there is one output.  
    if (ComponentMetaData.OutputCollection.Count != 1)  
    {  
        ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, out pbCancel);  
        return DTSValidationStatus.VS_ISCORRUPT;  
    }  
  
    // Let the base class verify that the input column reflects the output   
    // of the upstream component.  
    return base.Validate();  
}  
```  
  
```vb  
Public  Overrides Function Validate() As DTSValidationStatus   
  
 Dim pbCancel As Boolean = False  
  
 ' Validate that there is one input.  
 If Not (ComponentMetaData.InputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of inputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Validate that the UserName custom property is set.  
 If ComponentMetaData.CustomPropertyCollection("UserName").Value Is Nothing OrElse CType(ComponentMetaData.CustomPropertyCollection("UserName").Value, String).Length = 0 Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "The UserName property must be set.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISBROKEN   
 End If   
  
 ' Validate that there is one output.  
 If Not (ComponentMetaData.OutputCollection.Count = 1) Then   
   ComponentMetaData.FireError(0, ComponentMetaData.Name, "Incorrect number of outputs.", "", 0, pbCancel)   
   Return DTSValidationStatus.VS_ISCORRUPT   
 End If   
  
 ' Let the base class verify that the input column reflects the output   
 ' of the upstream component.  
  
 Return MyBase.Validate   
  
End Function  
```  
  
## <a name="reinitializemetadata-method"></a>Método ReinitializeMetaData  
 El Diseñador [!INCLUDE[ssIS](../../../includes/ssis-md.md)] llama al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A> siempre que se edita un componente que devuelve <xref:Microsoft.SqlServer.Dts.Pipeline.Wrapper.DTSValidationStatus.VS_NEEDSNEWMETADATA> desde su método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.Validate%2A>. Los componentes deben contener código que detecte y corrija los problemas identificados por el componente durante la validación.  
  
 En el ejemplo siguiente se muestra un componente que detecta problemas durante la validación y los corrige en el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.ReinitializeMetaData%2A>.  
  
```csharp  
private bool areInputColumnsValid = true;  
public override DTSValidationStatus Validate()  
{  
    IDTSInput100 input = ComponentMetaData.InputCollection[0];  
    IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
    bool Cancel = false;  
    foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
    {  
        try  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
        }  
        catch  
        {  
            ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, out Cancel);  
            areInputColumnsValid = false;  
            return DTSValidationStatus.VS_NEEDSNEWMETADATA;  
        }  
    }  
  
    return DTSValidationStatus.VS_ISVALID;  
}  
public override void ReinitializeMetaData()  
{  
    if (!areInputColumnsValid)  
    {  
        IDTSInput100 input = ComponentMetaData.InputCollection[0];  
        IDTSVirtualInput100 vInput = input.GetVirtualInput();  
  
        foreach (IDTSInputColumn100 column in input.InputColumnCollection)  
        {  
            IDTSVirtualInputColumn100 vColumn = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID);  
  
            if (vColumn == null)  
                input.InputColumnCollection.RemoveObjectByID(column.ID);  
        }  
        areInputColumnsValid = true;  
    }  
}  
```  
  
```vb  
Private areInputColumnsValid As Boolean = True   
  
Public  Overrides Function Validate() As DTSValidationStatus   
 Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
 Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
 Dim Cancel As Boolean = False   
 For Each column As IDTSInputColumn100 In input.InputColumnCollection   
   Try   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
   Catch   
     ComponentMetaData.FireError(0, ComponentMetaData.Name, "The input column " + column.IdentificationString + " does not match a column in the upstream component.", "", 0, Cancel)   
     areInputColumnsValid = False   
     Return DTSValidationStatus.VS_NEEDSNEWMETADATA   
   End Try   
 Next   
 Return DTSValidationStatus.VS_ISVALID   
End Function   
  
Public  Overrides Sub ReinitializeMetaData()   
 If Not areInputColumnsValid Then   
   Dim input As IDTSInput100 = ComponentMetaData.InputCollection(0)   
   Dim vInput As IDTSVirtualInput100 = input.GetVirtualInput   
   For Each column As IDTSInputColumn100 In input.InputColumnCollection   
     Dim vColumn As IDTSVirtualInputColumn100 = vInput.VirtualInputColumnCollection.GetVirtualInputColumnByLineageID(column.LineageID)   
     If vColumn Is Nothing Then   
       input.InputColumnCollection.RemoveObjectByID(column.ID)   
     End If   
   Next   
   areInputColumnsValid = True   
 End If   
End Sub  
```  
  
