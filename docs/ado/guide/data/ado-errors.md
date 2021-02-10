---
description: Errores de Run-Time de ADO
title: Errores de ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO]
ms.assetid: 9bb84114-a1df-4122-a1b8-ad98dcd85cc3
author: rothja
ms.author: jroth
ms.openlocfilehash: bed9958455af424c4f18cb0f6051eb30e2efaa3b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033395"
---
# <a name="ado-run-time-errors"></a>Errores de Run-Time de ADO
Los errores de ADO se envían al programa como errores en tiempo de ejecución. Puede usar el mecanismo de captura de errores del lenguaje de programación para interceptarlos y controlarlos. Por ejemplo, en Visual Basic, use la instrucción **on error** . En Visual C++, depende del método que use para tener acceso a las bibliotecas de ADO. Con #import, use un bloque **try-catch** . De lo contrario, los programadores de C++ deben recuperar explícitamente el objeto de error llamando a **GetErrorInfo**. En el siguiente procedimiento Sub Visual Basic se muestra cómo interceptar un error de ADO:

```
' BeginErrorHandlingVB01
Private Sub Form_Load()
' Turn on error handling
On Error GoTo FormLoadError

'Open the database and the recordset for processing.
'
Dim strCnn As String
strCnn = "Provider=sqloledb;" & _
    "Data Source=a-iresmi2000;" & _
    "Initial Catalog=Northwind;Integrated Security=SSPI"

' cnn is a Public Connection Object because
' it was defined WithEvents
Set cnn = New ADODB.Connection
cnn.Open strCnn

' The next line of code intentionally causes
' an error by trying to open a connection
' that has already been opened.
cnn.Open strCnn

' rst is a Public Recordset because it
' was defined WithEvents
Set rst = New ADODB.Recordset
rst.Open "Customers", cnn

Exit Sub

' Error handler
FormLoadError:
    Dim strErr As String
    Select Case Err
        Case adErrObjectOpen
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            strErr = strErr & "Error reported by: " & Err.Source & vbCrLf
            strErr = strErr & "Help File: " & Err.HelpFile & vbCrLf
            strErr = strErr & "Topic ID: " & Err.HelpContext
            MsgBox strErr
            Debug.Print strErr
            Err.Clear
            Resume Next
        ' If some other error occurs that
        ' has nothing to do with ADO, show
        ' the number and description and exit.
        Case Else
            strErr = "Error #" & Err.Number & ": " & Err.Description & vbCrLf
            MsgBox strErr
            Debug.Print strErr
            Unload Me
    End Select
End Sub
' EndErrorHandlingVB01
```

 Este procedimiento de eventos **Form_Load** crea intencionadamente un error al intentar abrir el mismo objeto de **conexión** dos veces. La segunda vez que se llama al método **Open** , se activa el controlador de errores. En este caso, el error es de tipo **adErrObjectOpen**, por lo que el controlador de errores muestra el siguiente mensaje antes de reanudar la ejecución del programa:

```
Error #3705: Operation is not allowed when the object is open.
Error reported by: ADODB.Connection
Help File: E:\WINNT\HELP\ADO260.CHM Topic ID: 1003705
```

 El mensaje de error incluye cada parte de la información proporcionada por el Visual Basic objeto **Err** , excepto el valor **LastDllError** , que no se aplica aquí. El número de error indica el error que se ha producido. La descripción es útil en los casos en los que no desea controlar el error usted mismo. Simplemente puede pasarlo al usuario. Aunque normalmente querrá usar mensajes personalizados para su aplicación, no puede prever cada error; la descripción proporciona alguna pista sobre el problema. En el código de ejemplo, el objeto de **conexión** comunicó el error. Verá el tipo de objeto o el identificador de programación aquí, no un nombre de variable.

> [!NOTE]
>  El objeto Visual Basic **Err** solo contiene información sobre el error más reciente. La colección de **errores** de ADO del objeto de **conexión** contiene un objeto de **error** para cada error generado por la operación de ADO más reciente. Utilice la colección de **errores** en lugar del objeto **Err** para controlar varios errores. Para obtener más información sobre la colección de **errores** , vea [errores de proveedor](./provider-errors.md). Sin embargo, si no hay ningún objeto de **conexión** válido, el objeto **Err** es el único origen de información sobre los errores de ADO.

 ¿Es probable que se produzcan errores de ADO en los tipos de operaciones? Los errores comunes de ADO pueden implicar la apertura de un objeto, como una **conexión** o un **conjunto de registros**, el intento de actualizar datos o la llamada a un método o una propiedad que no admite el proveedor.

 Los errores de OLE DB también se pueden pasar a la aplicación en forma de errores en tiempo de ejecución en la colección de **errores** .

 En el tema siguiente se proporciona más información acerca de los errores de ADO.

-   [Errores de ADO](./ado-error-reference.md)