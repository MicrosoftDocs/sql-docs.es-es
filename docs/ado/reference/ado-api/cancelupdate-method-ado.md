---
description: Método CancelUpdate (ADO)
title: CancelUpdate (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::CancelUpdate
helpviewer_keywords:
- CancelUpdate method [ADO]
ms.assetid: eaa856cc-c786-462e-890c-c896261b1741
author: rothja
ms.author: jroth
ms.openlocfilehash: 12ec3d30de9fc938d51ea342c468b522674b0086
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167797"
---
# <a name="cancelupdate-method-ado"></a>Método CancelUpdate (ADO)
Cancela los cambios realizados en la fila actual o nueva de un objeto de [conjunto de registros](./recordset-object-ado.md) , o la colección de [campos](./fields-collection-ado.md) de un objeto de [registro](./record-object-ado.md) , antes de llamar al método [Update](./update-method.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.CancelUpdaterecord.Fields.CancelUpdate  
```  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="recordset"></a>DataRecordsets  
 Utilice el método **CancelUpdate** para cancelar los cambios realizados en la fila actual o para descartar una fila recién agregada. No se pueden cancelar los cambios realizados en la fila actual o en una nueva una vez que se llama al método **Update** , a menos que los cambios formen parte de una transacción que se pueda revertir con el método [RollbackTrans](./begintrans-committrans-and-rollbacktrans-methods-ado.md) o que forme parte de una actualización por lotes. En el caso de una actualización por lotes, puede cancelar la **actualización** con el método **CancelUpdate** o [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Si va a agregar una nueva fila al llamar al método **CancelUpdate** , la fila actual se convierte en la fila que era la actual antes de la llamada a [AddNew](./addnew-method-ado.md) .  
  
 Si está en modo de edición y desea salir del registro actual (por ejemplo, con los métodos [Move](./move-method-ado.md), [NextRecordset](./nextrecordset-method-ado.md)o [Close](./close-method-ado.md) ), puede usar **CancelUpdate** para cancelar los cambios pendientes. Es posible que tenga que hacer esto si la actualización no se puede publicar correctamente en el origen de datos. Por ejemplo, un intento de eliminación que genera un error debido a infracciones de integridad referencial deja el **conjunto de registros** en modo de edición después de una llamada a [Delete](./delete-method-ado-recordset.md).  
  
## <a name="record"></a>Grabar  
 El método **CancelUpdate** cancela cualquier inserciones o eliminaciones pendientes de objetos de [campo](./field-object.md) , y cancela las actualizaciones pendientes de los campos existentes y los restaura a sus valores originales. La propiedad [status](./status-property-ado-recordset.md) de todos los campos de la colección **Fields** se establece en **adFieldOK**.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Fields (colección) (ADO)](./fields-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Update y CancelUpdate (VB)](./update-and-cancelupdate-methods-example-vb.md)   
 [Ejemplo de los métodos Update y CancelUpdate (VC + +)](./update-and-cancelupdate-methods-example-vc.md)   
 [AddNew (método) (ADO)](./addnew-method-ado.md)   
 [CANCEL (método) (ADO)](./cancel-method-ado.md)   
 [Método CANCEL (RDS)](../rds-api/cancel-method-rds.md)   
 [CancelBatch (método) (ADO)](./cancelbatch-method-ado.md)   
 [Método CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Propiedad EditMode](./editmode-property.md)   
 [Update (método)](./update-method.md)