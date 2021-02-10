---
description: 'Envío de actualizaciones: Método UpdateBatch'
title: 'Enviar las actualizaciones: método UpdateBatch | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 87123797-831f-48e0-94b5-f669f9ca194a
author: rothja
ms.author: jroth
ms.openlocfilehash: 88e441fd0ff58f63cf5ac701ec2620c29048b1e6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032497"
---
# <a name="sending-the-updates-updatebatch-method"></a>Envío de actualizaciones: Método UpdateBatch
El código siguiente abre un conjunto de registros en modo por lotes estableciendo la propiedad LockType en adLockBatchOptimistic y CursorLocation en adUseClient. Agrega dos nuevos registros y cambia el valor de un campo en un registro existente, guarda los valores originales y, a continuación, llama a UpdateBatch para devolver los cambios al origen de datos.  
  
## <a name="remarks"></a>Observaciones  
  
```  
'BeginBatchUpdate  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT ShipperId, CompanyName, Phone FROM Shippers"  
  
    Set objRs1 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
  
    ' Change value of Phone field for first record in Recordset, saving value  
    ' for later restoration.  
    intId = objRs1("ShipperId")  
    sPhone = objRs1("Phone")  
  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Add two new records  
    For i = 0 To 1  
        objRs1.AddNew  
        objRs1(1) = "New Shipper #" & CStr((i + 1))  
        objRs1(2) = "(nnn) 555-" & i & i & i & i  
    Next i  
  
    ' Send the updates  
    objRs1.UpdateBatch  
'EndBatchUpdate  
```  
  
 Si está editando el registro actual o agregando un nuevo registro cuando se llama al método UpdateBatch, ADO llamará automáticamente al método Update para guardar los cambios pendientes en el registro actual antes de transmitir los cambios por lotes al proveedor.  
  
## <a name="see-also"></a>Consulte también  
 [Batch Mode](../../../ado/guide/data/batch-mode.md)
