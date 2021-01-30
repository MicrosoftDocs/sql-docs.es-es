---
description: Cancel (método) (ADO)
title: CANCEL (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset20::Cancel
- _Record::Cancel
- _Connection::Cancel
- Command25::Cancel
- _Stream::Cancel
helpviewer_keywords:
- Cancel method [ADO]
ms.assetid: e0db4e15-6787-41e2-8f13-9e9b524d620a
author: rothja
ms.author: jroth
ms.openlocfilehash: adc282492b4c6d9a21683693154e43f1177bdca0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164752"
---
# <a name="cancel-method-ado"></a>Cancel (método) (ADO)
Cancela la ejecución de una llamada de método asincrónica pendiente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.Cancel  
```  
  
## <a name="remarks"></a>Observaciones  
 Use el método **Cancel** para finalizar la ejecución de una llamada de método asincrónico: es decir, un método invocado con la opción **adAsyncConnect**, **adAsyncExecute** o **adAsyncFetch** .  
  
 En la tabla siguiente se muestra qué tarea se termina cuando se usa el método **Cancel** en un tipo de objeto determinado.  
  
|Si el *objeto* es un|Finalizó la última llamada asincrónica a este método|  
|----------------------|-------------------------------------------------------------|  
|[Comando](./command-object-ado.md)|[Ejecutar](./execute-method-ado-command.md)|  
|[Connection](./connection-object-ado.md)|[Ejecutar](./execute-method-ado-connection.md) o [abrir](./open-method-ado-connection.md)|  
|[Registro](./record-object-ado.md)|[CopyRecord](./copyrecord-method-ado.md), [DeleteRecord](./deleterecord-method-ado.md), [MoveRecord](./moverecord-method-ado.md)o [Open](./open-method-ado-record.md)|  
|[DataRecordsets](./recordset-object-ado.md)|[Abrir](./open-method-ado-recordset.md)|  
|[Stream](./stream-object-ado.md)|[Abrir](./open-method-ado-stream.md)|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Command (ADO)](./command-object-ado.md)  
        [Objeto de conexión (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto Record (ADO)](./record-object-ado.md)  
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo del método CANCEL (VB)](./cancel-method-example-vb.md)   
 [Ejemplo del método CANCEL (VBScript)](../rds-api/cancel-method-example-vbscript.md)   
 [Ejemplo del método CANCEL (VC + +)](./cancel-method-example-vc.md)   
 [Método CANCEL (RDS)](../rds-api/cancel-method-rds.md)   
 [CancelBatch (método) (ADO)](./cancelbatch-method-ado.md)   
 [CancelUpdate (método) (ADO)](./cancelupdate-method-ado.md)   
 [Método CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Método execute (comando de ADO)](./execute-method-ado-command.md)   
 [Método execute (conexión ADO)](./execute-method-ado-connection.md)   
 [Open (método) (conexión de ADO)](./open-method-ado-connection.md)   
 [Open (método) (conjunto de registros ADO)](./open-method-ado-recordset.md)