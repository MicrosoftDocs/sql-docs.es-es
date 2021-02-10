---
description: Detectar y resolver conflictos
title: Detección y resolución de conflictos | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- conflicts [ADO], detecting and resolving
- ADO, detecting and resolving conflicts
ms.assetid: b28fdd26-c1a4-40ce-a700-2b0c9d201514
author: rothja
ms.author: jroth
ms.openlocfilehash: 4c92b847b95aacd5cf642c9646f4618dc5986abf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037635"
---
# <a name="detecting-and-resolving-conflicts"></a>Detectar y resolver conflictos
Si trabaja con el conjunto de registros en modo inmediato, hay mucha menos posibilidad de que se produzcan problemas de simultaneidad. Por otro lado, si la aplicación usa la actualización del modo por lotes, puede haber una buena posibilidad de que un usuario cambie un registro antes de que se guarden los cambios realizados por otro usuario que edite el mismo registro. En tal caso, querrá que la aplicación controle correctamente el conflicto. Es posible que desee que la última persona envíe una actualización al servidor "gana". O bien, puede que desee permitir que el usuario más reciente decida qué actualización debe tener prioridad proporcionándole una opción entre los dos valores en conflicto.  
  
 Sea cual sea el caso, ADO proporciona las propiedades UnderlyingValue y OriginalValue del objeto de campo para controlar estos tipos de conflictos. Use estas propiedades en combinación con el método Resync y la propiedad Filter del conjunto de registros.  
  
## <a name="remarks"></a>Observaciones  
 Cuando ADO encuentra un conflicto durante una actualización por lotes, se agregará una advertencia a la colección de errores. Por lo tanto, siempre debe comprobar si hay errores inmediatamente después de llamar a BatchUpdate y, si los encuentra, comenzar a probar la suposición de que ha encontrado un conflicto. El primer paso es establecer la propiedad Filter en el conjunto de registros igual a adFilterConflictingRecords. Esto limita la vista del conjunto de registros solo a los registros que están en conflicto. Si el valor de la propiedad RecordCount es igual a cero después de este paso, sabrá que el error se produjo por algo que no sea un conflicto.  
  
 Cuando se llama a BatchUpdate, ADO y el proveedor generan instrucciones SQL para realizar actualizaciones en el origen de datos. Recuerde que determinados orígenes de datos tienen limitaciones en cuanto a los tipos de columnas que se pueden usar en una cláusula WHERE.  
  
 A continuación, llame al método Resync del conjunto de registros con el argumento AffectRecords establecido en adAffectGroup y el argumento ResyncValues establecido en adResyncUnderlyingValues. El método Resync actualiza los datos del objeto de conjunto de registros actual de la base de datos subyacente. Mediante el uso de adAffectGroup, se asegura de que solo los registros visibles con la configuración de filtro actual, es decir, solo los registros en conflicto, se vuelven a sincronizar con la base de datos. Esto podría suponer una diferencia significativa en el rendimiento si se trabaja con un conjunto de registros grande. Al establecer el argumento ResyncValues en adResyncUnderlyingValues al llamar a Resync, se asegura de que la propiedad UnderlyingValue contendrá el valor (conflictivo) de la base de datos, que la propiedad Value mantendrá el valor especificado por el usuario, y que la propiedad OriginalValue contendrá el valor original para el campo (el valor que tenía antes de que se realizara la última llamada a UpdateBatch correcta). Después, puede usar estos valores para resolver el conflicto mediante programación o requerir al usuario que seleccione el valor que se utilizará.  
  
 Esta técnica se muestra en el ejemplo de código siguiente. En el ejemplo se crea de forma artificial un conflicto mediante un conjunto de registros independiente para cambiar un valor en la tabla subyacente antes de que se llame a UpdateBatch.  
  
```  
'BeginConflicts  
    strConn = "Provider=SQLOLEDB;Initial Catalog=Northwind;" & _  
              "Data Source=MySQLServer;Integrated Security=SSPI;"  
  
    strSQL = "SELECT * FROM Shippers WHERE ShipperID = 2"  
  
    'Open Rs and change a value  
    Set objRs1 = New ADODB.Recordset  
    Set objRs2 = New ADODB.Recordset  
    objRs1.CursorLocation = adUseClient  
    objRs1.Open strSQL, strConn, adOpenStatic, adLockBatchOptimistic, adCmdText  
    objRs1("Phone") = "(111) 555-1111"  
  
    'Introduce a conflict at the db...  
    objRs2.Open strSQL, strConn, adOpenKeyset, adLockOptimistic, adCmdText  
    objRs2("Phone") = "(999) 555-9999"  
    objRs2.Update  
    objRs2.Close  
    Set objRs2 = Nothing  
  
    On Error Resume Next  
    objRs1.UpdateBatch  
  
    If objRs1.ActiveConnection.Errors.Count <> 0 Then  
        Dim intConflicts As Integer  
  
        intConflicts = 0  
  
        objRs1.Filter = adFilterConflictingRecords  
  
        intConflicts = objRs1.RecordCount  
  
        'Resync so we can see the UnderlyingValue and offer user a choice.  
        'This sample only displays all three values and resets to original.  
        objRs1.Resync adAffectGroup, adResyncUnderlyingValues  
  
        If intConflicts > 0 Then  
            strMsg = "A conflict occurred with updates for " & intConflicts & _  
                     " record(s)." & vbCrLf & "The values will be restored" & _  
                     " to their original values." & vbCrLf & vbCrLf  
  
            objRs1.MoveFirst  
            While Not objRs1.EOF  
              strMsg = strMsg & "SHIPPER = " & objRs1("CompanyName") & vbCrLf  
              strMsg = strMsg & "Value = " & objRs1("Phone").Value & vbCrLf  
              strMsg = strMsg & "UnderlyingValue = " & _  
                                 objRs1("Phone").UnderlyingValue & vbCrLf  
              strMsg = strMsg & "OriginalValue = " & _  
                                 objRs1("Phone").OriginalValue & vbCrLf  
              strMsg = strMsg & vbCrLf & "Original value has been restored."  
  
              MsgBox strMsg, vbOKOnly, _  
                    "Conflict " & objRs1.AbsolutePosition & _  
                    " of " & intConflicts  
  
              objRs1("Phone").Value = objRs1("Phone").OriginalValue  
              objRs1.MoveNext  
            Wend  
  
            objRs1.UpdateBatch adAffectGroup  
        Else  
            'Other error occurred. Minimal handling in this example.  
             strMsg = "Errors occurred during the update. " & _  
                        objRs1.ActiveConnection.Errors(0).Number & " " & _  
                        objRs1.ActiveConnection.Errors(0).Description  
        End If  
  
        On Error GoTo 0  
    End If  
  
    objRs1.MoveFirst  
    objRs1.Close  
    Set objRs1 = Nothing  
'EndConflicts  
```  
  
 Puede usar la propiedad status del registro actual o de un campo específico para determinar el tipo de conflicto que se ha producido.  
  
 Para obtener información detallada sobre el control de errores, vea [control de errores](./error-handling.md).  
  
## <a name="see-also"></a>Consulte también  
 [Batch Mode](./batch-mode.md)