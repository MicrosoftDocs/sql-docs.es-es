---
description: Errores del proveedor
title: Errores del proveedor | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors collection [ADO]
- provider errors [ADO]
- event-related errors [ADO]
- errors [ADO], provider
- Error object [ADO], provider errors
ms.assetid: cc7d6ff9-2034-45c6-9d61-90b177010054
author: rothja
ms.author: jroth
ms.openlocfilehash: a13bcc83c360533b1137af69b688221157dc0dc0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032616"
---
# <a name="provider-errors"></a>Errores del proveedor
Cuando se produce un error de proveedor, se devuelve un error en tiempo de ejecución de-2147467259. Cuando reciba este error, Compruebe la colección de **errores** del objeto de **conexión** activo, que contendrá uno o más errores que describan lo que ha ocurrido.  
  
## <a name="the-ado-errors-collection"></a>La colección de errores de ADO  
 Dado que una operación de ADO determinada puede producir varios errores de proveedor, ADO expone una colección de objetos de error a través del objeto de **conexión** . Esta colección no contiene ningún objeto si una operación concluye correctamente y contiene uno o varios objetos de error en caso de que se haya producido un **error** y el proveedor haya provocado uno o varios errores. Examine cada objeto de error para determinar la causa exacta del error.  
  
 En cuanto haya terminado de controlar los errores que se hayan producido, puede borrar la colección llamando al método **Clear** . Es especialmente importante borrar explícitamente la colección de **errores** antes de llamar al método **Resync**, **UpdateBatch** o **CancelBatch** en un objeto de **conjunto de registros** , el método **Open** en un objeto de **conexión** o establecer la propiedad **Filter** en un objeto de **conjunto de registros** . Al borrar la colección explícitamente, puede estar seguro de que los objetos de **error** de la colección no quedan de una operación anterior.  
  
 Algunas operaciones pueden generar advertencias además de errores. Las advertencias también se representan mediante objetos de **error** en la colección de **errores** . Cuando un proveedor agrega una advertencia a la colección, no genera un error en tiempo de ejecución. Compruebe la propiedad **Count** de la colección de **errores** para determinar si se produjo una advertencia en una operación determinada. Si el recuento es uno o más, se ha agregado un objeto de **error** a la colección. En cuanto haya determinado que la colección de **errores** contiene errores o advertencias, puede recorrer en iteración la colección y recuperar información sobre cada uno de los objetos de **error** que contiene. En el siguiente ejemplo de Visual Basic breve se muestra esto:  
  
```  
' BeginErrorHandlingVB02  
Private Function DeleteCustomer(ByVal CompanyName As String) As Long  
    On Error GoTo DeleteCustomerError  
  
    rst.Find "CompanyName='" & CompanyName & "'"  
DeleteCustomerError:  
Dim objError As ADODB.Error  
Dim strError As String  
  
    If cnn.Errors.Count > 0 Then  
        For Each objError In cnn.Errors  
            strError = strError & "Error #" & objError.Number & _  
                " " & objError.Description & vbCrLf & _  
                "NativeError: " & objError.NativeError & vbCrLf & _  
                "SQLState: " & objError.SQLState & vbCrLf & _  
                "Reported by: " & objError.Source & vbCrLf & _  
                "Help file: " & objError.HelpFile & vbCrLf & _  
                "Help Context ID: " & objError.HelpContext  
        Next  
        MsgBox strError  
    End If  
End Function  
' EndErrorHandlingVB02  
```  
  
 La rutina de control de errores incluye un bucle **for each** que examina cada objeto de la colección de **errores** . En este ejemplo, se acumula un mensaje para mostrarlo. En un programa en funcionamiento, escribiría código para realizar una tarea adecuada para cada error, como cerrar todos los archivos abiertos y cerrar el programa de forma ordenada.  
  
## <a name="the-error-object"></a>El objeto de error  
 Al examinar un objeto de **error** , puede determinar qué error se produjo y más importante, qué aplicación o qué objeto provocó el error. El objeto de **error** tiene las siguientes propiedades:  
  
|Nombre de propiedad|Descripción|  
|-------------------|-----------------|  
|**Descripción**|Descripción de texto del error que se ha producido.|  
|**HelpContext, HelpFile**|Hace referencia al tema de ayuda y al archivo de ayuda que contiene una descripción del error que se ha producido.|  
|**NativeError**|El número de error específico del proveedor.|  
|**Number**|Entero largo que representa el número (incluido en **ErrorValueEnum**) del error que se ha producido.|  
|**Origen**|Indica el nombre del objeto o la aplicación que ha generado un error.|  
|**SQLState**|Un código de error de cinco caracteres que el proveedor devuelve durante el proceso de una instrucción SQL.|  
  
 El objeto de **error** de ADO es muy similar al objeto de Visual Basic **Err** estándar. Sus propiedades describen el error que se ha producido. Además del número de errores, también recibe dos fragmentos de información relacionados. La propiedad **NativeError** contiene un número de error específico del proveedor que se está usando. En el ejemplo anterior, el proveedor es el proveedor de Microsoft OLE DB para SQL Server, por lo que **NativeError** contendrá errores específicos de SQL Server. La propiedad **SQLSTATE** tiene un código de cinco letras que describe un error en una instrucción SQL.  
  
## <a name="event-related-errors"></a>Errores de Event-Related  
 El objeto de **error** también se usa cuando se producen errores relacionados con eventos. Puede determinar si se ha producido un error en el proceso que ha generado un evento de ADO comprobando el objeto de **error** que se ha pasado como un parámetro de evento.  
  
 Si la operación que provoca un evento se finaliza correctamente, el parámetro *adStatus* del controlador de eventos se establecerá en *adStatusOK*. Por otro lado, si la operación que generó el evento no se ha realizado correctamente, el parámetro *adStatus* se establece en *adStatusErrorsOccurred*. En ese caso, el parámetro *perror* contendrá un objeto de **error** que describe el error.
