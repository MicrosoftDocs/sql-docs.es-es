---
description: Pasar parámetros a un comando con nombre
title: Pasar parámetros a un comando con nombre | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- named commands [ADO]
- commands [ADO], passing parameters to a named command
ms.assetid: 36e0cdbe-7f50-40f5-af0d-700f5d8dc75a
author: rothja
ms.author: jroth
ms.openlocfilehash: 11fa07dd7af4512f8b7f41917d2778fecd6584ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032647"
---
# <a name="passing-parameters-to-a-named-command"></a>Pasar parámetros a un comando con nombre
Del mismo modo que el resultado del comando se pasa como una variable *out* del comando con nombre, los parámetros de un comando con parámetros se pueden pasar como *variables al* comando con nombre.  
  
 En el ejemplo de código siguiente se intenta recuperar todos los pedidos realizados por el cliente cuyo **CustomerID** es "ALKFI" de la base de datos Northwind. El valor de **CustomerID** se proporciona en el momento en que se llama al comando con nombre.  
  
```  
Const DS = "MySqlServer"  
Const DB = "Northwind"  
Const DP = "SQLOLEDB"  
  
Dim objConn As New ADODB.Connection  
Dim objRs As New ADODB.Recordset  
Dim objComm As New ADODB.Command  
  
CommandText = "SELECT OrderID, OrderDate, " & _  
                     "RequiredDate, ShippedDate " & _  
                     "FROM Orders " & _  
                     "WHERE CustomerID = ? " & _  
                     "ORDER BY OrderID"  
  
ConnectionString = "Provider=" & DP & _  
                   ";Data Source=" & DS & _  
                   ";Initial Catalog=" & DB & _  
                   ";Integrated Security=SSPI;"  
  
' Connect to the data source.  
objConn.Open ConnectionString  
  
' Set a named command.  
objComm.CommandText = CommandText  
objComm.CommandType = adCmdText  
objComm.Name = "GetOrdersOf"  
Set objComm.ActiveConnection = objConn  
  
' Call the named command, passing a CustomerID value  
' as the input parameter.   
'    "ALFKI" is the required input parameter,  
'    objRs is the resultant output variable.  
objConn.GetOrdersOf "ALKFI", objRs  
  
' Display the result.  
Debug.Print "All orders by ALFKI:"  
Do While Not objRs.EOF  
    Debug.Print vbTab & objRs(0) & vbTab & objRs(1) & vbTab & _  
                objRs(2) & vbTab & objRs(3)  
    objRs.MoveNext  
Loop  
  
' Clean up.  
objRs.Close  
objConn.Close  
Set objRs = Nothing  
Set objConn = Nothing  
Set objComm = Nothing  
```  
  
 Observe que todos los parámetros de entrada deben preceder a cualquier variable de salida y los tipos de datos de los parámetros deben coincidir o se pueden convertir en los de los campos correspondientes. La instrucción siguiente:  
  
```  
objConn.GetOrdersOf 12345, objRs  
```  
  
 -producirá un error de tipos de datos no coincidentes, porque el parámetro de entrada necesario es de un tipo de **cadena** , no de un tipo **entero** .  
  
 La siguiente llamada:  
  
```  
objConn.GetOrdersOf "12345", objRs  
```  
  
 : es válido, pero producirá un conjunto de resultados vacío porque no hay registros en la base de datos.  
  
## <a name="see-also"></a>Consulte también  
 [Objeto de conexión (ADO)](../../../ado/reference/ado-api/connection-object-ado.md)
