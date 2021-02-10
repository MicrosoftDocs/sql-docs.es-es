---
description: Ejemplo de propiedad ActiveConnection de catálogo (VB)
title: Ejemplo de propiedad ActiveConnection de catálogo (VB) | Microsoft Docs
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
- ActiveConnection property [ADOX], Visual Basic example
ms.assetid: bb3274b1-764d-43a7-a49f-ef55680ecd26
author: rothja
ms.author: jroth
ms.openlocfilehash: 7ccc2a40bfde1ac4a44ed0c415cea0f90b92e31d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050356"
---
# <a name="catalog-activeconnection-property-example-vb"></a>Ejemplo de propiedad ActiveConnection de catálogo (VB)
Si se establece la propiedad [ActiveConnection](./activeconnection-property-adox.md) en una conexión válida abierta, "abre" el catálogo. En un catálogo abierto, puede tener acceso a los objetos de esquema que contiene el catálogo.  
  
```  
' BeginOpenConnectionVB  
Sub Main()  
    On Error GoTo OpenConnectionError  
  
    Dim cnn As New ADODB.Connection  
    Dim cat As New ADOX.Catalog  
  
    cnn.Open "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source= 'Northwind.mdb';"  
    Set cat.ActiveConnection = cnn  
    Debug.Print cat.Tables(0).Type  
  
    'Clean up  
    cnn.Close  
    Set cat = Nothing  
    Set cnn = Nothing  
    Exit Sub  
  
OpenConnectionError:  
  
    Set cat = Nothing  
  
    If Not cnn Is Nothing Then  
        If cnn.State = adStateOpen Then cnn.Close  
    End If  
    Set cnn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndOpenConnectionVB  
```  
  
 Al establecer la propiedad **ActiveConnection** en una cadena de conexión válida, también se "abre" el catálogo.  
  
```  
Attribute VB_Name = "Catalog"  
```  
  
## <a name="see-also"></a>Consulte también  
 [ActiveConnection (propiedad, ADOX)](./activeconnection-property-adox.md)   
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Objeto Table (ADOX)](./table-object-adox.md)   
 [Colección de tablas (ADOX)](./tables-collection-adox.md)   
 [Type (propiedad, tabla, ADOX)](./type-property-table-adox.md)