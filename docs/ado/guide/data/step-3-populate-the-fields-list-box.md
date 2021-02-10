---
description: 'Paso 3: Relleno del cuadro de lista de campos'
title: 'Paso 3: rellenar el cuadro de lista campos | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 315c32dc-aeb1-4629-b30e-87b44e8f84d1
author: rothja
ms.author: jroth
ms.openlocfilehash: 6555f71d210524622e936026c8945168635de140
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032437"
---
# <a name="step-3-populate-the-fields-list-box"></a>Paso 3: Relleno del cuadro de lista de campos
Para rellenar el cuadro de lista campos, inserte el siguiente código en el controlador de eventos Click de `lstMain` :  
  
```  
Private Sub lstMain_Click()  
    Dim rec As Record  
    Dim rs As Recordset  
    Set rec = New Record  
    Set rs = New Recordset  
    grs.MoveFirst  
    grs.Move lstMain.ListIndex  
    lstDetails.Clear  
    rec.Open grs  
    Select Case rec.RecordType  
        Case adCollectionRecord:  
            Set rs = rec.GetChildren  
            While Not rs.EOF  
                lstDetails.AddItem rs(0)  
                rs.MoveNext  
            Wend  
        Case adSimpleRecord:  
            recFields rec, lstDetails, txtDetails  
  
        Case adStructDoc:  
    End Select  
  
End Sub  
```  
  
 Este código declara y crea instancias de los objetos de registro local y de conjunto de registros, `rec` y `rs` , respectivamente.  
  
 La fila correspondiente al recurso seleccionado en `lstMain` se convierte en la fila actual de `grs` . A continuación, el cuadro de lista detalles está desactivado y `rec` se abre con la fila actual de `grs` como origen.  
  
 Si el recurso es un registro de colección, como se especifica en [RecordType](../../../ado/reference/ado-api/recordtype-property-ado.md), se abre el conjunto de registros local `rs` en los elementos secundarios de REC. A continuación, `lstDetails` se rellena con los valores de las filas de `rs` .  
  
 Si el recurso es un registro simple, `recFields` se llama a. Para obtener más información acerca de `recFields` , vea el siguiente paso.  
  
 No se implementa ningún código si el recurso es un documento estructurado.  
  
## <a name="see-also"></a>Consulte también  
 [Escenario de publicación en Internet](../../../ado/guide/data/internet-publishing-scenario.md)   
 [Paso 2: inicializar el cuadro de lista principal](../../../ado/guide/data/step-2-initialize-the-main-list-box.md)   
 [Paso 4: Relleno del cuadro de texto de detalles](../../../ado/guide/data/step-4-populate-the-details-text-box.md)
