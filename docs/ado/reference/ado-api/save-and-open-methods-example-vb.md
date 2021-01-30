---
description: Ejemplo de los métodos Save y Open (VB)
title: Ejemplo de métodos Save y Open (VB) | Microsoft Docs
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
- Save method [ADO], Visual Basic example
- Open method [ADO]
ms.assetid: ddccdf58-9c57-4c9b-8b7f-0cf193f955fb
author: rothja
ms.author: jroth
ms.openlocfilehash: c76d26032d9986cba52442d62bb7ef8c10db5419
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166609"
---
# <a name="save-and-open-methods-example-vb"></a>Ejemplo de los métodos Save y Open (VB)
En estos tres ejemplos se muestra cómo se pueden usar los métodos [Save](./save-method.md) y [Open](./open-method-ado-recordset.md) juntos.  
  
 Supongamos que está trabajando en un viaje de negocios y desea tomar una tabla de una base de datos. Antes de comenzar, debe tener acceso a los datos como un [conjunto de registros](./recordset-object-ado.md) y guardarlos en un formulario transportable. Cuando llegue a su destino, tendrá acceso al **conjunto** de registros como un **conjunto de registros** local y desconectado. Realice cambios en el **conjunto de registros** y, a continuación, guárdelo de nuevo. Por último, cuando vuelva a la Página principal, vuelva a conectarse a la base de datos y actualícelo con los cambios que haya realizado en la carretera.  
  
 En primer lugar, acceda a la tabla **_authors_* _ y guárdela.  
  
```  
'BeginSaveVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    'recordset and connection variables  
    Dim rstAuthors As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strCnxn As String  
    Dim strSQLAuthors As String  
  
    ' Open connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Pubs';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    Set rstAuthors = New ADODB.Recordset  
    strSQLAuthors = "SELECT au_id, au_lname, au_fname, city, phone FROM Authors"  
    rstAuthors.Open strSQLAuthors, Cnxn, adOpenDynamic, adLockOptimistic, adCmdText  
  
    'For sake of illustration, save the Recordset to a diskette in XML format  
    rstAuthors.Save "c:\Pubs.xml", adPersistXML  
  
    ' clean up  
    rstAuthors.Close  
    Cnxn.Close  
    Set rstAuthors = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    'clean up  
    If Not rstAuthors Is Nothing Then  
        If rstAuthors.State = adStateOpen Then rstAuthors.Close  
    End If  
    Set rstAuthors = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndSaveVB  
```  
  
 Llegados a este punto, ha llegado a su destino. Tendrá acceso a la tabla _*_authors_*_ como un conjunto de registros local, desconectado _ * * *. Debe tener el proveedor **MSPersist** en el equipo que está usando para obtener acceso al archivo guardado, a:\Pubs.xml.  
  
```  
Attribute VB_Name = "Save"  
```  
  
 Por último, vuelva a casa. Ahora, actualice la base de datos con los cambios.  
  
```  
Attribute VB_Name = "Save"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)   
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)   
 [Más información sobre la persistencia de conjuntos de registros](../../guide/data/more-about-recordset-persistence.md)   
 [Save (método)](./save-method.md)