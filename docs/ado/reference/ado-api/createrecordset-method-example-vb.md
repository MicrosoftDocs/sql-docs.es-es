---
description: Ejemplo del método CreateRecordset (VB)
title: Ejemplo del método CreateRecordset (VB) | Microsoft Docs
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
- CreateRecordset method [RDS], Visual Basic example
ms.assetid: 2de8fd02-0f49-4d47-8bd3-397726d1c644
author: rothja
ms.author: jroth
ms.openlocfilehash: 9dc5925a68b977e8beda03a0cb105739e323ebb1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167643"
---
# <a name="createrecordset-method-example-vb"></a>Ejemplo del método CreateRecordset (VB)
Puede crear un objeto de [conjunto de registros](./recordset-object-ado.md) y especificar la información de la columna. A continuación, puede insertar datos en el objeto de **conjunto de registros** . el conjunto de filas subyacente almacena en búfer las inserciones.  
  
 En el ejemplo de código siguiente se muestra cómo definir un **conjunto de registros** mediante el objeto [RDSServer. DataFactory](../rds-api/datafactory-object-rdsserver.md) . También puede hacer esto con el [RDS. Objeto DataControl](../rds-api/datacontrol-object-rds.md) .  
  
```  
'BeginRsDefineShapeVB  
Sub Main()  
    On Error GoTo ErrorHandler  
  
    Dim ADF As RDSServer.DataFactory  
    Dim vntRecordShape(3)  
    Dim vntField1Shape(3)  
    Dim vntField2Shape(3)  
    Dim vntField3Shape(3)  
    Dim vntField4Shape(3)  
  
    Set ADF = New RDSServer.DataFactory  
  
    ' For each field, specify the name,  
    ' type, size, and nullability.  
  
    vntField1Shape(0) = "Name"   ' Column name.  
    vntField1Shape(1) = CInt(129)   ' Column type.  
    vntField1Shape(2) = CInt(40)   ' Column size.  
    vntField1Shape(3) = False      ' Nullable?  
  
    vntField2Shape(0) = "Age"  
    vntField2Shape(1) = CInt(3)  
    vntField2Shape(2) = CInt(-1)  
    vntField2Shape(3) = True  
  
    vntField3Shape(0) = "DateOfBirth"  
    vntField3Shape(1) = CInt(7)  
    vntField3Shape(2) = CInt(-1)  
    vntField3Shape(3) = True  
  
    vntField4Shape(0) = "Balance"  
    vntField4Shape(1) = CInt(6)  
    vntField4Shape(2) = CInt(-1)  
    vntField4Shape(3) = True  
  
    ' Put all fields into an array of arrays.  
    vntRecordShape(0) = vntField1Shape  
    vntRecordShape(1) = vntField2Shape  
    vntRecordShape(2) = vntField3Shape  
    vntRecordShape(3) = vntField4Shape  
  
    ' Use the RDSServer.DataFactory to create an empty  
    ' recordset. It takes an array of variants where  
    ' every element is itself another array of  
    ' variants, one for every column required in the  
    ' recordset.  
    ' The elements of the inner array are the column's  
    ' name, type, size, and nullability.  
    '  
    ' NOTE: You could just use the RDS.DataControl object  
    ' instead of the RDSServer.DataFactory object. In  
    ' that case, the following code would be Set NewRS  
    ' = ADC1.CreateRecordset(vntRecordShape)  
    Dim NewRs As ADODB.Recordset  
    Set NewRs = ADF.CreateRecordSet(vntRecordShape)  
  
    Dim fields(3)  
    fields(0) = vntField1Shape(0)  
    fields(1) = vntField2Shape(0)  
    fields(2) = vntField3Shape(0)  
    fields(3) = vntField4Shape(0)  
  
    ' Populate the new recordset with data values.  
    Dim fieldVals(3)  
  
    ' Use AddNew to add the records.  
    fieldVals(0) = "Joe"  
    fieldVals(1) = 5  
    fieldVals(2) = CDate(#1/5/1996#)  
    fieldVals(3) = 123.456  
    NewRs.AddNew fields, fieldVals  
  
    fieldVals(0) = "Mary"  
    fieldVals(1) = 6  
    fieldVals(2) = CDate(#6/5/1996#)  
    fieldVals(3) = 31  
    NewRs.AddNew fields, fieldVals  
  
    fieldVals(0) = "Alex"  
    fieldVals(1) = 13  
    fieldVals(2) = CDate(#1/6/1996#)  
    fieldVals(3) = 34.0001  
    NewRs.AddNew fields, fieldVals  
  
    fieldVals(0) = "Susan"  
    fieldVals(1) = 13  
    fieldVals(2) = CDate(#8/6/1996#)  
    fieldVals(3) = 0#  
    NewRs.AddNew fields, fieldVals  
  
    NewRs.MoveFirst  
  
    ' Set the newly created and populated Recordset to  
    ' the SourceRecordset property of the  
    ' RDS.DataControl to bind to visual controls  
    Dim ADC1 As RDS.DataControl  
    Set ADC1 = New RDS.DataControl  
    Set ADC1.SourceRecordset = NewRs  
  
    'Clean up  
    If NewRs.State = adStateOpen Then NewRs.Close  
    Set NewRs = Nothing  
    Set ADC1 = Nothing  
    Set ADF = Nothing  
    Exit Sub  
  
ErrorHandler:  
    ' clean up  
    If Not NewRs Is Nothing Then  
        If NewRs.State = adStateOpen Then NewRs.Close  
    End If  
    Set NewRs = Nothing  
    Set ADC1 = Nothing  
    Set ADF = Nothing  
  
    If Err <> 0 Then  
        MsgBox Err.Source & "-->" & Err.Description, , "Error"  
    End If  
End Sub  
'EndRsDefineShapeVB  
```