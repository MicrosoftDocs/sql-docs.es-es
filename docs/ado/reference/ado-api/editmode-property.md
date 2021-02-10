---
description: Propiedad EditMode
title: Propiedad EditMode | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::EditMode
helpviewer_keywords:
- EditMode property
ms.assetid: a1b04bb2-8c8b-47f9-8477-bfd0368b6f68
author: rothja
ms.author: jroth
ms.openlocfilehash: 5cec956ced454f0220b8d545e8e05c8fbcf8a760
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025416"
---
# <a name="editmode-property"></a>Propiedad EditMode
Indica el estado de edición del registro actual.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de [EditModeEnum](../../../ado/reference/ado-api/editmodeenum.md) .  
  
## <a name="remarks"></a>Observaciones  
 ADO mantiene un búfer de edición asociado al registro actual. Esta propiedad indica si se han realizado cambios en este búfer o si se ha creado un nuevo registro. Utilice la propiedad **EditMode** para determinar el estado de edición del registro actual. Puede comprobar los cambios pendientes si se ha interrumpido un proceso de edición y determinar si necesita usar el método [Update](../../../ado/reference/ado-api/update-method.md) o [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) .  
  
 En el *modo de actualización inmediata* , la propiedad **EditMode** se restablece a **adEditNone** después de que se llame a la llamada correcta al método **Update** . Cuando una llamada a [Delete](../../../ado/reference/ado-api/delete-method-ado-recordset.md) no elimina correctamente el registro o los registros del origen de datos (por ejemplo, debido a infracciones de la integridad referencial), el [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) permanece en modo de edición (**EditMode**  =  **adEditInProgress**). Por consiguiente, se debe llamar a **CancelUpdate** antes de salir del registro actual (por ejemplo, con [Move](../../../ado/reference/ado-api/move-method-ado.md), [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)o [Close](../../../ado/reference/ado-api/close-method-ado.md) ).  
  
 En el *modo de actualización por lotes* (en el que el proveedor almacena en caché varios cambios y los escribe en el origen de datos subyacente solo cuando se llama al método [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) ), el valor de la propiedad **EditMode** se cambia cuando se realiza la primera operación y no se restablece mediante una llamada al método **Update** . Las operaciones posteriores no cambian el valor de la propiedad **EditMode** , aunque se realicen diferentes operaciones. Por ejemplo, si la primera operación es agregar un nuevo registro y la segunda realiza cambios en un registro existente, la propiedad de **EditMode** seguirá siendo **adEditAdd**. La propiedad **EditMode** no se restablece en **adEditNone** hasta después de la llamada a **UpdateBatch**. Para determinar qué operaciones se han realizado, establezca la propiedad [Filter](../../../ado/reference/ado-api/filter-property.md) en [adFilterPending](../../../ado/reference/ado-api/filtergroupenum.md) para que solo los registros con cambios pendientes sean visibles y examine la propiedad [status](../../../ado/reference/ado-api/status-property-ado-recordset.md) de cada registro para determinar qué cambios se han realizado en los datos.  
  
> [!NOTE]
>  **EditMode** solo puede devolver un valor válido si hay un registro actual. **EditMode** devolverá un error si [BOF o EOF](../../../ado/reference/ado-api/bof-eof-properties-ado.md) es true, o si se ha eliminado el registro actual.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades CursorType, LockType y EditMode (VB)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vb.md)   
 [Ejemplo de las propiedades CursorType, LockType y EditMode (VC + +)](../../../ado/reference/ado-api/cursortype-locktype-and-editmode-properties-example-vc.md)   
 [AddNew (método) (ADO)](../../../ado/reference/ado-api/addnew-method-ado.md)   
 [Delete (método) (conjunto de registros ADO)](../../../ado/reference/ado-api/delete-method-ado-recordset.md)   
 [CancelUpdate (método) (ADO)](../../../ado/reference/ado-api/cancelupdate-method-ado.md)   
 [Update (método)](../../../ado/reference/ado-api/update-method.md)
