---
description: Ejemplo de la propiedad de estado (campo) (VB)
title: Ejemplo de la propiedad de estado (campo) (VB) | Microsoft Docs
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
- Status property [ADO Field], Visual Basic example
ms.assetid: fdd09b60-39c7-44be-8008-e891a031f80e
author: rothja
ms.author: jroth
ms.openlocfilehash: 69e80f025c29e379495507447ef78af5bcc8868f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166453"
---
# <a name="status-property-example-field-vb"></a>Ejemplo de la propiedad de estado (campo) (VB)
En el ejemplo siguiente se abre un documento desde una carpeta de lectura/escritura mediante el [proveedor de publicación en Internet](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md). La propiedad [status](./status-property-ado-field.md) de un objeto [Field](./field-object.md) del [registro](./record-object-ado.md) se establecerá primero en **adFieldPendingInsert** y, a continuación, se actualizará a **adFieldOk**.  
  
```  
'BeginStatusFieldVB  
  
 ' to integrate this code replace the values in the source string  
  
Sub Main()  
  
   Dim File As ADODB.Record  
   Dim strFile As String  
   Dim Cnxn As ADODB.Connection  
   Dim strCnxn As String  
  
   Set Cnxn = New ADODB.Connection  
   strCnxn = "url=https://MyServer/"  
   Cnxn.Open strCnxn  
  
   Set File = New ADODB.Record  
   strFile = "Folder/FileName"  
   ' Open a read/write document  
   File.Source = strFile  
   File.ActiveConnection = Cnxn  
   File.Mode = adModeReadWrite  
   File.Open  
  
   Debug.Print "Append a couple of fields"  
  
   File.Fields.Append "chektest:fld1", adWChar, 42, adFldUpdatable, "fld1"  
   File.Fields.Append "chektest:fld2", adWChar, 42, adFldUpdatable, "fld2"  
  
   Debug.Print "status for the fields"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert  
  
    'turn off error-handling to verify field status  
   On Error Resume Next  
  
   File.Fields.Update  
  
   Debug.Print "Update succeeds"  
   Debug.Print File.Fields("chektest:fld1").Status 'adfldpendinginsert + adFieldUnavailable  
   Debug.Print File.Fields("chektest:fld2").Status 'adfldpendinginsert + adFieldUnavailable  
  
    ' resume default error-handling  
   On Error GoTo 0  
  
     ' clean up  
    File.Close  
    Cnxn.Close  
    Set File = Nothing  
    Set Cnxn = Nothing  
  
End Sub  
'EndStatusFieldVB  
```  
  
 En el ejemplo siguiente se elimina un **campo** conocido de un **registro** abierto desde un documento. La propiedad **status** se establecerá primero en **adFieldOK** y luego en **adFieldPendingUnknown**.  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
 El código siguiente elimina un **campo** de un **registro** abierto en un documento de solo lectura. **Status** se establecerá en **adFieldPendingDelete**. En la [actualización](./update-method.md), se producirá un error en la eliminación y el **Estado** será **adFieldPendingDelete** más **adFieldPermissionDenied**. [CancelUpdate](./cancelupdate-method-ado.md) borra la configuración de **Estado** pendiente.  
  
```  
Attribute VB_Name = "StatusField"  
```  
  
## <a name="see-also"></a>Consulte también  
 [Field (objeto)](./field-object.md)   
 [Objeto Record (ADO)](./record-object-ado.md)   
 [Propiedad Status (Field ADO)](./status-property-ado-field.md)