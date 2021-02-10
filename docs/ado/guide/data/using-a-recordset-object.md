---
description: Mediante un objeto de conjunto de registros
title: Usar un objeto de conjunto de registros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 01c630d8-eb35-4bd0-a99f-7c0f85316cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 44fc7a242887419ba3013113632c8d1acd2a3337
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032307"
---
# <a name="using-a-recordset-object"></a>Mediante un objeto de conjunto de registros
Como alternativa, puede usar **Recordset. Open** para establecer implícitamente una conexión y emitir un comando a través de esa conexión en una sola operación. Por ejemplo, en Visual Basic:  
  
```  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
  
' Create and Open the Recordset object.  
Set oRs = New ADODB.Recordset  
oRs.CursorLocation = adUseClient  
oRs.Open sSQL, sConn, adOpenStatic, _  
               adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
oRs.Close          
Set oRs = Nothing  
```  
  
 Observe que **oRs. Open** toma una cadena de conexión (*sConn*), en lugar de un objeto de **conexión** (*oConn*), como el valor de su parámetro **ActiveConnection** . Además, el tipo de cursor del lado cliente se aplica estableciendo la propiedad **CursorLocation** en el objeto de **conjunto de registros** . De nuevo, Compare esto con el ejemplo de **HelloData** .
