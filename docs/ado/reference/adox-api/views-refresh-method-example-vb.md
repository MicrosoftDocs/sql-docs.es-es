---
description: Ejemplo de método Refresh de vistas (VB)
title: Ejemplo del método de actualización de vistas (VB) | Microsoft Docs
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
- Refresh method [ADOX]
ms.assetid: cdad2d66-6ade-40dc-9e74-e40cfa9bc127
author: rothja
ms.author: jroth
ms.openlocfilehash: 70795cf5c645ba3eaf9d6c869a067b02fad9d0c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169073"
---
# <a name="views-refresh-method-example-vb"></a>Ejemplo de método Refresh de vistas (VB)
En el código siguiente se muestra cómo actualizar la colección [views](./views-collection-adox.md) de un [Catálogo](./catalog-object-adox.md). Esto es necesario antes de que se pueda tener acceso a los objetos de [vista](./view-object-adox.md) del **Catálogo** .  
  
```  
' BeginViewsRefreshVB  
Sub Main()  
    On Error GoTo ProcedureViewsRefreshError  
  
    Dim cat As New ADOX.Catalog  
  
    ' Open the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Refresh the Procedures collection.  
    cat.Views.Refresh  
  
    'Clean up  
    Set cat = Nothing  
    Exit Sub  
  
ProcedureViewsRefreshError:  
  
    If Not cat Is Nothing Then  
        Set cat = Nothing  
    End If  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
' EndViewsRefreshVB  
```  
  
## <a name="see-also"></a>Consulte también  
 [Refresh (método) (ADO)](../ado-api/refresh-method-ado.md)   
 [Colección de vistas (ADOX)](./views-collection-adox.md)