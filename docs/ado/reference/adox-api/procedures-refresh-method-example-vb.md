---
description: Ejemplo de método Refresh de procedimientos (VB)
title: Ejemplo de método de actualización de procedimientos (VB) | Microsoft Docs
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
- Refresh method [ADOX], Visual Basic example
ms.assetid: 499679bd-287b-487d-bdfb-3803abffec1c
author: rothja
ms.author: jroth
ms.openlocfilehash: 289cee12f65511cf64a68837043eacb37d2f2763
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054850"
---
# <a name="procedures-refresh-method-example-vb"></a>Ejemplo de método Refresh de procedimientos (VB)
En el código siguiente se muestra cómo actualizar la colección [Procedures](./procedures-collection-adox.md) de un [Catálogo](./catalog-object-adox.md). Esto es necesario antes de que se pueda tener acceso a los objetos de [procedimiento](./procedure-object-adox.md) del **Catálogo** .  
  
```  
' BeginProceduresRefreshVB  
Sub Main()  
    On Error GoTo ProcedureRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the Catalog  
    cat.ActiveConnection = _  
        "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection  
    cat.Procedures.Refresh  
  
    'Clean up  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureRefreshError:  
    Set cat = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndProceduresRefreshVB  
```  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Colección Procedures (ADOX)](./procedures-collection-adox.md)   
 [Actualizar (método, ADO)](../ado-api/refresh-method-ado.md)