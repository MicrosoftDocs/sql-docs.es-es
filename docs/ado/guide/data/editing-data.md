---
description: Edición de datos
title: Editar datos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, editing data
- AdUseClient [ADO]
- editing data [ADO]
ms.assetid: ef514f85-c446-4f05-824e-c9313b2ffae1
author: rothja
ms.author: jroth
ms.openlocfilehash: 2f8fa6b7a3fb898d425d439b11b42451d42ac17a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033235"
---
# <a name="editing-data"></a>Edición de datos
Hemos explicado cómo usar ADO para conectarse a un origen de datos, ejecutar un comando, obtener los resultados en un objeto de **conjunto de registros** y navegar por el **conjunto de registros**. Esta sección se centra en la siguiente operación fundamental de ADO: editar datos.  
  
 En esta sección se sigue usando el **conjunto de registros** de ejemplo incluido en el [examen de datos](./examining-data.md), con un cambio importante. El código siguiente se utiliza para abrir el **conjunto de registros**:  
  
```  
'BeginEditIntro  
    Dim strSQL As String  
    Dim objRs1 As ADODB.Recordset  
  
    strSQL = "SELECT * FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
  
    objRs1.Open strSQL, GetNewConnection, adOpenStatic, _  
                adLockBatchOptimistic, adCmdText  
  
    ' Disconnect the Recordset from the Connection object.  
    Set objRs1.ActiveConnection = Nothing  
'EndEditIntro  
```  
  
 El cambio importante en el código implica establecer la propiedad **CursorLocation** del objeto de **conexión** igual a **adUseClient** en la función *GetNewConnection* (que se muestra en el ejemplo siguiente), que indica el uso de un cursor de cliente. Para obtener más información sobre las diferencias entre los cursores de cliente y de servidor, vea [Descripción de los cursores y bloqueos](./understanding-cursors-and-locks.md).  
  
 El valor **adUseClient** de la propiedad **CursorLocation** mueve la ubicación del cursor desde el origen de datos (el SQL Server, en este caso) a la ubicación del código de cliente (la estación de trabajo de escritorio). Esta configuración obliga a ADO a invocar el motor de cursor de cliente para OLE DB en el cliente con el fin de crear y administrar el cursor.  
  
 Es posible que también haya observado que el parámetro **LockType** del método **Open** ha cambiado a **adLockBatchOptimistic**. Se abre el cursor en modo por lotes. (El proveedor almacena en caché varios cambios y los escribe en el origen de datos subyacente solo cuando se llama al método **UpdateBatch** ). Los cambios que se realizaron en el **conjunto de registros** no se actualizarán en la base de datos hasta que se llame al método **UpdateBatch** .  
  
 Por último, el código de esta sección usa una versión modificada de la función GetNewConnection. Esta versión de la función ahora devuelve un cursor del lado cliente. La función tiene el siguiente aspecto:  
  
```  
'BeginNewConnection  
Public Function GetNewConnection() As ADODB.Connection  
    On Error GoTo ErrHandler:  
  
    Dim objConn As New ADODB.Connection  
    Dim strConnStr As String  
  
    strConnStr = "Provider='SQLOLEDB';Initial Catalog='Northwind';" & _  
                 "Data Source='MySqlServer';Integrated Security='SSPI';"  
  
    objConn.ConnectionString = strConnStr  
    objConn.CursorLocation = adUseClient  
    objConn.Open  
  
    Set GetNewConnection = objConn  
  
    Exit Function  
  
ErrHandler:  
    Set objConn = Nothing  
    Set GetNewConnection = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Function  
'EndNewConnection  
```  
  
 Esta sección contiene los temas siguientes.  
  
-   [Editar los registros existentes](./editing-existing-records.md)  
  
-   [Agregar registros a un conjunto de registros](./adding-records.md)  
  
-   [Determinar qué se admite](./determining-what-is-supported.md)  
  
-   [Eliminar registros mediante el método Delete](./deleting-records-using-the-delete-method.md)  
  
-   [Alternativas: Uso de instrucciones SQL](./alternatives-using-sql-statements.md)