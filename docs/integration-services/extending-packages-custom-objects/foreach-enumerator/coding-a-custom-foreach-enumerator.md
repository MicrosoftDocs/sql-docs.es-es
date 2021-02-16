---
description: Codificar un enumerador foreach personalizado
title: Programar un enumerador Para cada uno personalizado | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
helpviewer_keywords:
- custom foreach enumerators [Integration Services], coding
ms.assetid: 279cf6de-d06f-40e7-b8ca-569310449f36
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fd0115df8326a1835c5debb10df977df9492666c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350290"
---
# <a name="coding-a-custom-foreach-enumerator"></a>Codificar un enumerador foreach personalizado

[!INCLUDE[sqlserver-ssis](../../../includes/applies-to-version/sqlserver-ssis.md)]


  Una vez que haya creado una clase que herede de la clase base <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator> y haya aplicado el atributo <xref:Microsoft.SqlServer.Dts.Runtime.DtsForEachEnumeratorAttribute> a la clase, debe invalidar la implementación de las propiedades y los métodos de la clase base para proporcionar su funcionalidad personalizada.  
  
 Para obtener un ejemplo funcional de un enumerador personalizado, consulte [Developing a User Interface for a Custom ForEach Enumerator](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md) (Desarrollar una interfaz de usuario para un enumerador Para cada uno personalizado).  
  
## <a name="initializing-the-enumerator"></a>Inicializar el enumerador  
 Puede invalidar el método <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.InitializeForEachEnumerator%2A> para almacenar en memoria caché las referencias a los administradores de conexión definidos en el paquete y las referencias a la interfaz de eventos que puede utilizar para provocar errores, advertencias y mensajes informativos.  
  
## <a name="validating-the-enumerator"></a>Validar el enumerador  
 Invalide el método <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.Validate%2A> para comprobar que el enumerador se ha configurado correctamente. Si el método devuelve **error**, no se ejecutarán el enumerador ni el paquete que contiene el enumerador. La implementación de este método es específica de cada enumerador, pero si el enumerador se basa en los objetos <xref:Microsoft.SqlServer.Dts.Runtime.Variable> o <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager>, debe agregar código para comprobar que estos objetos existen en las colecciones que se proporcionan al método.  
  
 En el ejemplo de código siguiente se muestra una implementación de <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.Validate%2A> que comprueba una variable especificada en una propiedad del enumerador.  
  
```csharp  
private string variableNameValue;  
  
public string VariableName  
{  
    get{ return this.variableNameValue; }  
    set{ this.variableNameValue = value; }  
}  
  
public override DTSExecResult Validate(Connections connections, VariableDispenser variableDispenser, IDTSInfoEvents infoEvents, IDTSLogging log)  
{  
    if (!variableDispenser.Contains(this.variableNameValue))  
    {  
        infoEvents.FireError(0, "MyEnumerator", "The Variable " + this.variableNameValue + " does not exist in the collection.", "", 0);  
            return DTSExecResult.Failure;  
    }  
    return DTSExecResult.Success;  
}  
```  
  
```vb  
Private variableNameValue As String  
  
Public Property VariableName() As String  
    Get   
         Return Me.variableNameValue  
    End Get  
    Set (ByVal Value As String)   
         Me.variableNameValue = value  
    End Set  
End Property  
  
Public Overrides Function Validate(ByVal connections As Connections, ByVal variableDispenser As VariableDispenser, ByVal infoEvents As IDTSInfoEvents, ByVal log As IDTSLogging) As DTSExecResult  
    If Not variableDispenser.Contains(Me.variableNameValue) Then  
        infoEvents.FireError(0, "MyEnumerator", "The Variable " + Me.variableNameValue + " does not exist in the collection.", "", 0)  
            Return DTSExecResult.Failure  
    End If  
    Return DTSExecResult.Success  
End Function  
```  
  
## <a name="returning-the-collection"></a>Devolver la colección  
 Durante la ejecución, el contenedor <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> llama al método <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> del enumerador personalizado. En este método, el enumerador crea y rellena su colección de elementos y, a continuación, devuelve la colección. A continuación, <xref:Microsoft.SqlServer.Dts.Runtime.ForEachLoop> itera los elementos de la colección y ejecuta su flujo de control para cada elemento de la colección.  
  
 En el ejemplo siguiente se muestra una implementación de <xref:Microsoft.SqlServer.Dts.Runtime.ForEachEnumerator.GetEnumerator%2A> que devuelve una matriz de enteros aleatorios.  
  
```csharp  
public override object GetEnumerator()  
{  
    ArrayList numbers = new ArrayList();  
  
    Random randomNumber = new Random(DateTime.Now);  
  
    for( int x=0; x < 100; x++ )  
        numbers.Add( randomNumber.Next());  
  
    return numbers;  
}  
```  
  
```vb  
Public Overrides Function GetEnumerator() As Object  
    Dim numbers As ArrayList =  New ArrayList()   
  
    Dim randomNumber As Random =  New Random(DateTime.Now)   
  
        Dim x As Integer  
        For  x = 0 To  100- 1  Step  x + 1  
        numbers.Add(randomNumber.Next())  
        Next  
  
    Return numbers  
End Function  
```  
 
## <a name="see-also"></a>Consulte también  
 [Crear un enumerador Para cada uno personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/creating-a-custom-foreach-enumerator.md)   
 [Desarrollar una interfaz de usuario para un enumerador foreach personalizado](../../../integration-services/extending-packages-custom-objects/foreach-enumerator/developing-a-user-interface-for-a-custom-foreach-enumerator.md)  
  
  
