---
description: Ejemplo de la propiedad de recuento (VB)
title: Ejemplo de la propiedad Count (VB) | Microsoft Docs
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
- Count property [ADO], Visual Basic example
ms.assetid: 35033910-623b-449a-a57d-baff3ed5ab8f
author: rothja
ms.author: jroth
ms.openlocfilehash: 15d8a58584a70be849050d32733a73149e865f69
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100026033"
---
# <a name="count-property-example-vb"></a>Ejemplo de la propiedad de recuento (VB)
En este ejemplo se muestra la propiedad [Count](./count-property-ado.md) con dos colecciones en la base de datos ***Employee**. La propiedad obtiene el número de objetos de cada colección y establece el límite superior de los bucles que enumeran estas colecciones. Otra manera de enumerar estas colecciones sin usar la propiedad _ *Count** sería utilizar `For Each...Next` instrucciones.  
  
```  
'BeginCountVB  
  
    'To integrate this code  
    'replace the data source and initial catalog values  
    'in the connection string  
  
Public Sub Main()  
    On Error GoTo ErrorHandler  
  
    ' recordset and connection variables  
    Dim rstEmployees As ADODB.Recordset  
    Dim Cnxn As ADODB.Connection  
    Dim strSQLEmployees As String  
    Dim strCnxn As String  
  
    Dim intLoop As Integer  
  
    ' Open a connection  
    Set Cnxn = New ADODB.Connection  
    strCnxn = "Provider='sqloledb';Data Source='MySqlServer';" & _  
        "Initial Catalog='Northwind';Integrated Security='SSPI';"  
    Cnxn.Open strCnxn  
  
    ' Open recordset with data from Employee table  
    Set rstEmployees = New ADODB.Recordset  
    strSQLEmployees = "Employee"  
    'rstEmployees.Open strSQLEmployee, Cnxn, , , adCmdTable  
    rstEmployees.Open strSQLEmployees, Cnxn, adOpenForwardOnly, adLockReadOnly, adCmdTable  
    'the above two lines opening the recordset are identical as  
    'the default values for CursorType and LockType arguments match those specified  
  
    ' Print information about Fields collection  
    Debug.Print rstEmployees.Fields.Count & " Fields in Employee"  
  
    For intLoop = 0 To rstEmployees.Fields.Count - 1  
        Debug.Print "   " & rstEmployees.Fields(intLoop).Name  
    Next intLoop  
  
    ' Print information about Properties collection  
    Debug.Print rstEmployees.Properties.Count & " Properties in Employee"  
  
    For intLoop = 0 To rstEmployees.Properties.Count - 1  
        Debug.Print "   " & rstEmployees.Properties(intLoop).Name  
    Next intLoop  
  
    ' clean up  
    rstEmployees.Close  
    Cnxn.Close  
    Set rstEmployees = Nothing  
    Set Cnxn = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not rstEmployees Is Nothing Then  
        If rstEmployees.State = adStateOpen Then rstEmployees.Close  
    End If  
    Set rstEmployees = Nothing  
  
    If Not Cnxn Is Nothing Then  
        If Cnxn.State = adStateOpen Then Cnxn.Close  
    End If  
    Set Cnxn = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndCountVB  
```  
  
## <a name="see-also"></a>Consulte también  
 [Count (propiedad, ADO)](./count-property-ado.md)