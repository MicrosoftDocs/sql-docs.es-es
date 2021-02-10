---
description: Uso de un objeto de conexión
title: Usar un objeto de conexión | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- connections [ADO]
ms.assetid: 4b34f971-5699-43e7-9b15-137d334fa66e
author: rothja
ms.author: jroth
ms.openlocfilehash: 36344c8b8c03677ac3b0ae25db097917d8d10d2a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036805"
---
# <a name="using-a-connection-object"></a>Uso de un objeto de conexión
Antes de abrir un objeto de **conexión** , debe definir cierta información sobre el origen de datos y el tipo de conexión. La mayor parte de esta información se mantiene mediante el parámetro *ConnectionString* del [método Open](../../../ado/reference/ado-api/open-method-ado-connection.md) en el objeto **Connection** , o mediante la [propiedad ConnectionString](../../../ado/reference/ado-api/connectionstring-property-ado.md) en el objeto **Connection** . Una cadena de conexión consta de una lista de pares de argumentos y valores separados por punto y coma, con los valores entre comillas simples. Por ejemplo:  
  
```  
Dim sConn As String  
sConn = "Provider='SQLOLEDB';Data Source='MySqlServer';" & _  
             "Initial Catalog='Northwind';Integrated Security='SSPI';"  
```  
  
> [!NOTE]
>  También puede especificar un nombre de origen de datos ODBC (DSN) o un archivo de vínculo de datos (UDL) en una cadena de conexión. Para obtener más información acerca de los DSN, vea [administrar orígenes de datos](../../../odbc/admin/managing-data-sources.md) en la referencia del programador de ODBC. Para obtener más información sobre los UdL, consulte [información general](/previous-versions/windows/desktop/ms718102(v=vs.85)) sobre la API de vínculo de datos en la referencia del programador de OLE DB.  
  
 Normalmente, se establece una conexión llamando al método **Connection. Open** con una *cadena de conexión* adecuada como su parámetro. Un ejemplo se muestra en el siguiente fragmento de código de Visual Basic:  
  
```  
Dim oConn As ADODB.Connection  
Dim oRs As ADODB.Recordset  
Dim sConn As String  
Dim sSQL as String  
  
' Open a connection.  
Set oConn = New ADODB.Connection  
.Open   
  
' Make a query over the connection.  
sSQL = "SELECT ProductID, ProductName, CategoryID, UnitPrice " & _  
             "FROM Products"  
Set oRs = New ADODB.Recordset  
oRs.Open sSQL, , adOpenStatic, adLockBatchOptimistic, adCmdText  
  
MsgBox oRs.RecordCount  
  
' Close the connection.  
oConn.Close  
Set oConn = Nothing  
  
```  
  
 Aquí **oRs. Open** toma una variable de objeto de **conexión** (*oConn*) como valor de su parámetro *ActiveConnection* . Además, la propiedad **Connection. CursorLocation** asume el valor predeterminado de **adUseServer**. Compare esto con el ejemplo de [HelloData](../../../ado/guide/data/hellodata-a-simple-ado-application.md) de la sección anterior. La siguiente instrucción daría como resultado errores en tiempo de ejecución.  
  
```  
oRs.MarshalOptions = adMarshalModifiedOnly  
' Disconnect the Recordset.  
Set oRs.ActiveConnection = Nothing  
```