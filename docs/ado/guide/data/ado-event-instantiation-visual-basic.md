---
description: 'Creación de instancias de eventos de ADO: Visual Basic'
title: 'Creación de instancias de eventos de ADO: Visual Basic | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
ms.assetid: dce0a2a3-326f-4aaf-a822-6c5549833afa
author: rothja
ms.author: jroth
ms.openlocfilehash: 764c1d06be70c808881d74c8e458b74a432ddfe3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033355"
---
# <a name="ado-event-instantiation-visual-basic"></a>Creación de instancias de eventos de ADO: Visual Basic
Para controlar los eventos de ADO en Microsoft® Visual Basic®, debe declarar una variable de nivel de módulo mediante la palabra clave **WithEvents** . La variable solo se puede declarar como parte de un módulo de clase y se debe declarar en el nivel de módulo. No obstante, esto no es tan restrictivo como parece, porque Visual Basic objetos **Form** también son clases. La manera más sencilla de controlar los eventos de ADO es declarar una variable mediante **WithEvents**. En el ejemplo siguiente se controla el evento **ConnectComplete** para un objeto **Connection** :  
  
```  
' BeginEventExampleVB02  
Dim WithEvents connEvent As Connection  
Attribute connEvent.VB_VarHelpID = -1  
  
Private Sub Form_Load()  
Dim strConn As String  
  
   ' Create a new object with event  
   ' handling enabled.  
   strConn = "Provider=sqloledb;" & _  
      "Data Source=MyServer;" & _  
      "Initial Catalog=Northwind;" & _  
      "Integrated Security=SSPI;"  
   Set connEvent = New ADODB.Connection  
   connEvent.Open strConn  
End Sub  
  
Private Sub connEvent_ConnectComplete(ByVal pError As ADODB.Error, _  
    adStatus As ADODB.EventStatusEnum, _  
    ByVal pConnection As ADODB.Connection)  
Dim strMsg As String  
  
   If adStatus = adStatusErrorsOccurred Then  
      Select Case pError.Number  
         Case adErrOperationCancelled  
            ' The operation was cancelled in the  
            ' Will event. Notify the user and exit.  
            strMsg = "I'm sorry you can't connect right now." & vbCrLf  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
         Case Else  
            strMsg = "Error " & Format(pError.Number) & vbCrLf  
            strMsg = strMsg & pError.Description  
            strMsg = strMsg & "Click OK to exit."  
            Unload Me  
      End Select  
   End If  
   frmWait.btnOK.Enabled = True  
End Sub  
' EndEventExampleVB02  
```  
  
 El objeto de **conexión** se declara en el nivel de **formulario** mediante la palabra clave **WithEvents** para habilitar el control de eventos. En realidad, el controlador de eventos Form_Load crea el objeto mediante la asignación de un nuevo objeto de **conexión** a *connEvent* y, a continuación, abre la conexión. Por supuesto, una aplicación real haría más procesamiento en el controlador de eventos Form_Load que el que se muestra aquí.
