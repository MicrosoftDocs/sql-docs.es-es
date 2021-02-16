---
description: Actualizar la versión de un componente de flujo de datos
title: Actualizar la versión de un componente de flujo de datos | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- PerformUpgrade method
- custom data flow components [Integration Services], upgrading version
- data flow components [Integration Services], upgrading version
- upgrading data flow components [Integration Services]
ms.assetid: c2a298c6-01b3-4ad1-884d-6108165eb56e
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7c544070aba868f257e20e9028893a89172c0726
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343624"
---
# <a name="upgrading-the-version-of-a-data-flow-component"></a>Actualizar la versión de un componente de flujo de datos

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Los paquetes que se crearon con una versión anterior de su componente pueden contener metadatos que ya no son válidos, como propiedades personalizadas cuyo uso se ha modificado en versiones más recientes del componente. Puede invalidar el método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A> de la clase base <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent> para actualizar los metadatos previamente guardados en paquetes anteriores para reflejar las propiedades actuales de su componente.  
  
> [!NOTE]  
>  Al volver a compilar un componente personalizado para una nueva versión de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], no tiene que cambiar el valor de la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.CurrentVersion%2A> si las propiedades del componente no han cambiado.  
  
## <a name="example"></a>Ejemplo  
 El ejemplo siguiente contiene el código de la versión 2.0 de un componente de flujo de datos ficticio. El nuevo número de versión se define en la propiedad <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute.CurrentVersion%2A> de <xref:Microsoft.SqlServer.Dts.Pipeline.DtsPipelineComponentAttribute>. El componente tiene una propiedad que define la manera de administrar los valores numéricos que superan un umbral. En la versión 1.0 del componente ficticio, esta propiedad se denominó `RaiseErrorOnInvalidValue` y aceptó un valor booleano de true o false. En la versión 2.0 del componente ficticio, se ha cambiado el nombre de la propiedad a `InvalidValueHandling` y acepta uno de cuatro valores posibles de una enumeración personalizada.  
  
 El método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A> que se invalida en el ejemplo siguiente realiza las acciones que se enumeran a continuación:  
  
-   Obtiene la versión actual del componente.  
  
-   Obtiene el valor de la propiedad personalizada anterior.  
  
-   Quita una la propiedad anterior de la colección de propiedades personalizadas.  
  
-   Si es posible, establece el valor de la nueva propiedad personalizada a partir del valor de la propiedad anterior.  
  
-   Establece los metadatos de la versión en la versión actual del componente.  
  
> [!NOTE]  
>  El motor de flujo de datos pasa su propio número de versión al método <xref:Microsoft.SqlServer.Dts.Pipeline.PipelineComponent.PerformUpgrade%2A> en el parámetro *pipelineVersion*. Este parámetro no es útil en la versión 1.0 de [!INCLUDE[ssISnoversion](../../../includes/ssisnoversion-md.md)], pero sí puede ser útil en versiones posteriores.  
  
 El código de ejemplo usa únicamente los dos valores de enumeración que asignan directamente a los valores booleanos anteriores para la propiedad personalizada. Los usuarios pueden seleccionar otros valores de enumeración disponibles a través de la interfaz de usuario personalizada del componente, bien en el Editor avanzado o bien mediante programación. Para obtener información acerca de cómo mostrar valores de enumeración para una propiedad personalizada en el Editor avanzado, vea "Crear propiedades personalizadas" en [Métodos en tiempo de diseño de un componente de flujo de datos](../../../integration-services/extending-packages-custom-objects/data-flow/design-time-methods-of-a-data-flow-component.md).  
  
```vb  
Imports Microsoft.SqlServer.Dts.Pipeline  
Imports Microsoft.SqlServer.Dts.Pipeline.Wrapper  
  
<DtsPipelineComponent(ComponentType:=ComponentType.Transform, CurrentVersion:=2)> _  
Public Class PerformUpgrade  
  Inherits PipelineComponent  
  
  ' Define the set of possible values for the new custom property.  
  Private Enum InvalidValueHandling  
    Ignore  
    FireInformation  
    FireWarning  
    FireError  
  End Enum  
  
  Public Overloads Overrides Sub PerformUpgrade(ByVal pipelineVersion As Integer)  
  
    ' Obtain the current component version from the attribute.  
    Dim componentAttribute As DtsPipelineComponentAttribute = _  
      CType(Attribute.GetCustomAttribute(Me.GetType, _  
      GetType(DtsPipelineComponentAttribute), False), _  
      DtsPipelineComponentAttribute)  
    Dim currentVersion As Integer = componentAttribute.CurrentVersion  
  
    ' If the component version saved in the package is less than  
    '  the current version, Version 2, perform the upgrade.  
    If ComponentMetaData.Version < currentVersion Then  
  
      ' Get the current value of the old custom property, RaiseErrorOnInvalidValue,   
      ' and then remove the property from the custom property collection.  
      Dim oldValue As Boolean = False  
      Try  
        Dim oldProperty As IDTSCustomProperty100 = _  
          ComponentMetaData.CustomPropertyCollection("RaiseErrorOnInvalidValue")  
        oldValue = CType(oldProperty.Value, Boolean)  
        ComponentMetaData.CustomPropertyCollection.RemoveObjectByIndex("RaiseErrorOnInvalidValue")  
      Catch ex As Exception  
        ' If the old custom property is not available, ignore the error.  
      End Try  
  
      ' Set the value of the new custom property, InvalidValueHandling,  
      '  by using the appropriate enumeration value.  
      Dim newProperty As IDTSCustomProperty100 = _  
        ComponentMetaData.CustomPropertyCollection("InvalidValueHandling")  
      If oldValue = True Then  
        newProperty.Value = InvalidValueHandling.FireError  
      Else  
        newProperty.Value = InvalidValueHandling.Ignore  
      End If  
  
    End If  
  
    ' Update the saved component version metadata to the current version.  
    ComponentMetaData.Version = currentVersion  
  
  End Sub  
  
End Class  
```  
  
```csharp  
using System;  
using Microsoft.SqlServer.Dts.Pipeline;  
using Microsoft.SqlServer.Dts.Pipeline.Wrapper;  
  
[DtsPipelineComponent(ComponentType = ComponentType.Transform, CurrentVersion = 2)]  
public class PerformUpgradeCS :  
  PipelineComponent  
  
  // Define the set of possible values for the new custom property.  
{  
  private enum InvalidValueHandling  
  {  
    Ignore,  
    FireInformation,  
    FireWarning,  
    FireError  
  };  
  
  public override void PerformUpgrade(int pipelineVersion)  
  {  
  
    // Obtain the current component version from the attribute.  
    DtsPipelineComponentAttribute componentAttribute =   
      (DtsPipelineComponentAttribute)Attribute.GetCustomAttribute(this.GetType(), typeof(DtsPipelineComponentAttribute), false);  
    int currentVersion = componentAttribute.CurrentVersion;  
  
    // If the component version saved in the package is less than  
    //  the current version, Version 2, perform the upgrade.  
    if (ComponentMetaData.Version < currentVersion)  
  
    // Get the current value of the old custom property, RaiseErrorOnInvalidValue,   
    // and then remove the property from the custom property collection.  
    {  
      bool oldValue = false;  
      try  
      {  
        IDTSCustomProperty100 oldProperty =   
          ComponentMetaData.CustomPropertyCollection["RaiseErrorOnInvalidValue"];  
        oldValue = (bool)oldProperty.Value;  
        ComponentMetaData.CustomPropertyCollection.RemoveObjectByIndex("RaiseErrorOnInvalidValue");  
      }  
      catch (Exception ex)  
      {  
        // If the old custom property is not available, ignore the error.  
      }  
  
      // Set the value of the new custom property, InvalidValueHandling,  
      //  by using the appropriate enumeration value.  
      IDTSCustomProperty100 newProperty =   
         ComponentMetaData.CustomPropertyCollection["InvalidValueHandling"];  
      if (oldValue == true)  
      {  
        newProperty.Value = InvalidValueHandling.FireError;  
      }  
      else  
      {  
        newProperty.Value = InvalidValueHandling.Ignore;  
      }  
  
    }  
  
    // Update the saved component version metadata to the current version.  
    ComponentMetaData.Version = currentVersion;  
  
  }  
  
}  
```
