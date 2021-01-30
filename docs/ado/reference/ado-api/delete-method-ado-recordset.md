---
description: Delete (método) (conjunto de registros ADO)
title: Método Delete (conjunto de registros ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_Delete
- Recordset15::Delete
helpviewer_keywords:
- Delete method [ADO]
ms.assetid: 1eb9209c-602c-4507-b0c2-6527a599b67d
author: rothja
ms.author: jroth
ms.openlocfilehash: d445a6bfc705821b49408027c13ef1474cdc0d94
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167572"
---
# <a name="delete-method-ado-recordset"></a>Delete (método) (conjunto de registros ADO)
Elimina el registro actual o un grupo de registros.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.Delete AffectRecords  
```  
  
#### <a name="parameters"></a>Parámetros  
 *AffectRecords*  
 Valor de [AffectEnum](../../../ado/reference/ado-api/affectenum.md) que determina el número de registros a los que afectará el método de **eliminación** . El valor predeterminado es **adAffectCurrent**.  
  
> [!NOTE]
>  **adAffectAll** y **adAffectAllChapters** no son argumentos válidos para **eliminar**.  
  
## <a name="remarks"></a>Observaciones  
 El uso del método **Delete** marca el registro actual o un grupo de registros en un objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) para su eliminación. Si el objeto de **conjunto de registros** no permite la eliminación de registros, se produce un error. Si está en modo de actualización inmediata, las eliminaciones se producen inmediatamente en la base de datos. Si un registro no se puede eliminar correctamente (debido a infracciones de la integridad de la base de datos, por ejemplo), el registro permanecerá en modo de edición después de la llamada a [Update](../../../ado/reference/ado-api/update-method.md). Esto significa que debe cancelar la actualización con [CancelUpdate](../../../ado/reference/ado-api/cancelupdate-method-ado.md) antes de salir del registro actual (por ejemplo, con [Close](../../../ado/reference/ado-api/close-method-ado.md), [Move](../../../ado/reference/ado-api/move-method-ado.md)o [NextRecordset](../../../ado/reference/ado-api/nextrecordset-method-ado.md)).  
  
 Si está en modo de actualización por lotes, los registros se marcan para su eliminación de la memoria caché y la eliminación real se produce cuando se llama al método [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md) . Utilice la propiedad [Filter](../../../ado/reference/ado-api/filter-property.md) para ver los registros eliminados.  
  
 Al recuperar valores de campo del registro eliminado, se genera un error. Después de eliminar el registro actual, el registro eliminado permanece activo hasta que se mueve a otro registro. Una vez que salga del registro eliminado, ya no estará accesible.  
  
 Si anida eliminaciones en una transacción, puede recuperar los registros eliminados con el método [RollbackTrans](../../../ado/reference/ado-api/begintrans-committrans-and-rollbacktrans-methods-ado.md) . Si está en modo de actualización por lotes, puede cancelar una eliminación pendiente o un grupo de eliminaciones pendientes con el método [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) .  
  
 Si se produce un error al intentar eliminar registros debido a un conflicto con los datos subyacentes (por ejemplo, otro usuario ya ha eliminado un registro), el proveedor devuelve advertencias a la colección de [errores](../../../ado/reference/ado-api/errors-collection-ado.md) , pero no detiene la ejecución del programa. Solo se produce un error en tiempo de ejecución si hay conflictos en todos los registros solicitados.  
  
 Si se establece la propiedad dinámica de [tabla única](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) y el **conjunto de registros** es el resultado de ejecutar una operación de combinación en varias tablas, el método **Delete** solo eliminará las filas de la tabla mencionada en la propiedad de [tabla única](../../../ado/reference/ado-api/unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](../../../ado/reference/ado-api/recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo del método Delete (VB)](../../../ado/reference/ado-api/delete-method-example-vb.md)   
 [Ejemplo del método Delete (VBScript)](../../../ado/reference/ado-api/delete-method-example-vbscript.md)   
 [Ejemplo del método Delete (VC + +)](../../../ado/reference/ado-api/delete-method-example-vc.md)   
 [Método Delete (colección Fields de ADO)](../../../ado/reference/ado-api/delete-method-ado-fields-collection.md)   
 [Método Delete (colección de parámetros de ADO)](../../../ado/reference/ado-api/delete-method-ado-parameters-collection.md)   
 [DeleteRecord (método, ADO)](../../../ado/reference/ado-api/deleterecord-method-ado.md)
