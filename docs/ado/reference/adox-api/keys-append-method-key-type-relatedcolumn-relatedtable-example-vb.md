---
description: Ejemplo de propiedades Type, RelatedColumn, RelatedTable y UpdateRule de clave, método Append de claves (VB)
title: Ejemplo de creación de una nueva relación de clave externa entre tablas (VB) | Microsoft Docs
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
- Key Type property [ADOX], Visual Basic example
- RelatedTable property [ADOX], Visual Basic example
- Keys Append method [ADOX], Visual Basic example
- UpdateRule property [ADOX], Visual Basic example
- RelatedColumn property [ADOX], Visual Basic example
ms.assetid: 13b5b1c3-6af6-439e-bb65-976578ba6bc2
author: rothja
ms.author: jroth
ms.openlocfilehash: 3fb89ca61eacd7f10b0fe5546c569e9ebe16f73c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169329"
---
# <a name="keys-append-method-key-type-relatedcolumn-relatedtable-and-updaterule-properties-example-vb"></a>Ejemplo de propiedades Type, RelatedColumn, RelatedTable y UpdateRule de clave, método Append de claves (VB)
En el código siguiente se muestra cómo crear una nueva relación de clave externa entre dos tablas existentes denominadas **Customers** y **Orders**.  
  
```  
' BeginCreateKeyVB  
Sub Main()  
    On Error GoTo CreateKeyError  
  
    Dim kyForeign As New ADOX.Key  
    Dim cat As New ADOX.Catalog  
  
    ' Connect to the catalog.  
    cat.ActiveConnection = "Provider='Microsoft.Jet.OLEDB.4.0';" & _  
        "Data Source='Northwind.mdb';"  
  
    ' Define the foreign key.  
    kyForeign.Name = "CustOrder"  
    kyForeign.Type = adKeyForeign  
    kyForeign.RelatedTable = "Customers"  
    kyForeign.Columns.Append "CustomerId"  
    kyForeign.Columns("CustomerId").RelatedColumn = "CustomerId"  
    kyForeign.UpdateRule = adRICascade  
  
    ' Append the foreign key to the keys collection.  
    cat.Tables("Orders").Keys.Append kyForeign  
  
    'Delete the key t demonstrate the Delete method.  
    cat.Tables("Orders").Keys.Delete kyForeign.Name  
  
    'Clean up.  
    Set cat.ActiveConnection = Nothing  
    Set cat = Nothing  
    Set kyForeign = Nothing  
    Exit Sub  
  
CreateKeyError:  
    Set cat = Nothing  
    Set kyForeign = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
  
End Sub  
' EndCreateKeyVB  
```  
  
## <a name="see-also"></a>Consulte también  
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Objeto column (ADOX)](./column-object-adox.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Objeto Key (ADOX)](./key-object-adox.md)   
 [Colección de claves (ADOX)](./keys-collection-adox.md)   
 [Name (propiedad, ADOX)](./name-property-adox.md)   
 [Propiedad RelatedColumn (ADOX)](./relatedcolumn-property-adox.md)   
 [Propiedad RelatedTable (ADOX)](./relatedtable-property-adox.md)   
 [Objeto Table (ADOX)](./table-object-adox.md)   
 [Colección de tablas (ADOX)](./tables-collection-adox.md)   
 [Type (propiedad, Key) (ADOX)](./type-property-key-adox.md)   
 [UpdateRule (propiedad, ADOX)](./updaterule-property-adox.md)