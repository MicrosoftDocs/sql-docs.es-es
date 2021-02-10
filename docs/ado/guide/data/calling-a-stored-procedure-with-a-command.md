---
description: Llamar a un procedimiento almacenado con un comando
title: Llamar a un procedimiento almacenado con un comando | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- calling stored procedures [ADO]
- stored procedures [ADO]
- commands [ADO]
ms.assetid: 685f7652-2271-4ede-b552-2eeb8c756b4c
author: rothja
ms.author: jroth
ms.openlocfilehash: c34dbecd5e078e19029161b29b66187b02514166
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033335"
---
# <a name="calling-a-stored-procedure-with-a-command"></a>Llamar a un procedimiento almacenado con un comando
Puede utilizar un comando para llamar a un procedimiento almacenado. El ejemplo de código al final de este tema hace referencia a un procedimiento almacenado en la base de datos de ejemplo Northwind, denominado CustOrdersOrders, que se define de la siguiente manera.  
  
```  
CREATE PROCEDURE CustOrdersOrders @CustomerID nchar(5) AS  
SELECT OrderID, OrderDate, RequiredDate, ShippedDate  
FROM Orders  
WHERE CustomerID = @CustomerID  
ORDER BY OrderID  
```  
  
 Consulte la documentación de SQL Server para obtener más información sobre cómo definir y llamar a procedimientos almacenados.  
  
 Este procedimiento almacenado es similar al comando que se usa en [los parámetros del objeto de comando](./command-object-parameters.md). Toma un parámetro de identificador de cliente y devuelve información acerca de los pedidos de ese cliente. En el ejemplo de código siguiente se usa este procedimiento almacenado como el origen de un **conjunto de registros** ADO.  
  
 El uso del procedimiento almacenado permite tener acceso a otra funcionalidad de ADO: el método de **actualización** de la colección de **parámetros** . Con este método, ADO puede rellenar automáticamente toda la información sobre los parámetros requeridos por el comando en tiempo de ejecución. El uso de esta técnica produce una reducción del rendimiento, ya que ADO debe consultar el origen de datos para obtener información sobre los parámetros.  
  
 Existen otras diferencias importantes entre el ejemplo de código siguiente y el código de [los parámetros del objeto de comando](./command-object-parameters.md), donde los parámetros se escribieron manualmente. En primer lugar, este código no establece la propiedad **Prepared** en **true** porque es un SQL Server procedimiento almacenado y está precompilado por definición. En segundo lugar, la propiedad **CommandType** del objeto **Command** cambió a **adCmdStoredProc** en el segundo ejemplo para informar a ADO de que el comando era un procedimiento almacenado.  
  
 Por último, en el segundo ejemplo, se debe hacer referencia al parámetro por índice al establecer el valor, porque es posible que no conozca el nombre del parámetro en tiempo de diseño. Si conoce el nombre del parámetro, puede establecer la nueva propiedad [NamedParameters](../../reference/ado-api/namedparameters-property-ado.md) del objeto de **comando** en true y hacer referencia al nombre de la propiedad. Es posible que se pregunte por qué la posición del primer parámetro mencionado en el procedimiento almacenado ( @CustomerID ) es 1 en lugar de 0 ( `objCmd(1) = "ALFKI"` ). Esto se debe a que el parámetro 0 contiene un valor devuelto por SQL Server procedimiento almacenado.  
  
```  
'BeginAutoParamCmd  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim objCmd As New ADODB.Command  
    Dim objParm1 As New ADODB.Parameter  
    Dim objRs As New ADODB.Recordset  
  
    ' Set CommandText equal to the stored procedure name.  
    objCmd.CommandText = "CustOrdersOrders"  
    objCmd.CommandType = adCmdStoredProc  
  
    ' Connect to the data source.  
    Set objConn = GetNewConnection  
    objCmd.ActiveConnection = objConn  
  
    ' Automatically fill in parameter info from stored procedure.  
    objCmd.Parameters.Refresh  
  
    ' Set the param value.  
    objCmd(1) = "ALFKI"  
  
    ' Execute once and display...  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    ' ...then set new param value, re-execute command, and display.  
    objCmd(1) = "CACTU"  
    Set objRs = objCmd.Execute  
  
    Debug.Print objParm1.Value  
    Do While Not objRs.EOF  
        Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                    objRs(2) & vbTab & objRs(3)  
        objRs.MoveNext  
    Loop  
  
    'clean up  
    objRs.Close  
    objConn.Close  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
    Exit Sub  
  
ErrHandler:  
    'clean up  
    If objRs.State = adStateOpen Then  
        objRs.Close  
    End If  
  
    If objConn.State = adStateOpen Then  
        objConn.Close  
    End If  
  
    Set objRs = Nothing  
    Set objConn = Nothing  
    Set objCmd = Nothing  
    Set objParm1 = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
'EndAutoParamCmd  
  
'BeginNewConnection  
Private Function GetNewConnection() As ADODB.Connection  
    Dim oCn As New ADODB.Connection  
    Dim sCnStr As String  
  
    sCnStr = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Integrated Security='SSPI';Initial Catalog='Northwind';"  
    oCn.Open sCnStr  
  
    If oCn.State = adStateOpen Then  
        Set GetNewConnection = oCn  
    End If  
  
End Function  
'EndNewConnection  
```  
  
## <a name="see-also"></a>Consulte también  
 [Artículo 117500 de Knowledge base](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/185125)
