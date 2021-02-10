---
description: Ejemplo de propiedades Command y CommandText (VB)
title: Ejemplo de propiedades Command y CommandText (VB) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- CommandText property [ADOX], Visual Basic example
- Command property [ADOX], Visual Basic example
ms.assetid: 413263a8-05c0-4404-929d-69f82b987ba3
author: rothja
ms.author: jroth
ms.openlocfilehash: 9393c247f3361576516957fbeccb5ca5d09f64d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050265"
---
# <a name="command-and-commandtext-properties-example-vb"></a>Ejemplo de propiedades Command y CommandText (VB)
En el código siguiente se muestra cómo utilizar la propiedad [Command](./command-property-adox.md) para actualizar el texto de un procedimiento.  
  
```  
' BeginProcedureTextVB  
Sub Main()  
    On Error GoTo ProcedureTextError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
    Dim cmd As New ADODB.Command  
  
    ' Open the connection.  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Open the catalog.  
    Set cat.ActiveConnection = cnn  
  
    ' Get the command.  
    Set cmd = cat.Procedures("CustomerById").Command  
  
    ' Update the CommandText.  
    cmd.CommandText = "Select CustomerId, CompanyName, ContactName " & _  
        "From Customers " & _  
        "Where CustomerId = [CustId]"  
  
    ' Update the procedure.  
    Set cat.Procedures("CustomerById").Command = cmd  
  
    'Clean up.  
    cnn.Close  
    Set cat = Nothing  
    Set cmd = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
ProcedureTextError:  
    Set cat = Nothing  
    Set cmd = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProcedureTextVB  
```  
  
## <a name="see-also"></a>Consulte también  
 [ActiveConnection (propiedad, ADOX)](./activeconnection-property-adox.md)   
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Command (propiedad, ADOX)](./command-property-adox.md)   
 [Procedure (objeto) (ADOX)](./procedure-object-adox.md)   
 [Colección de procedimientos (ADOX)](./procedures-collection-adox.md)