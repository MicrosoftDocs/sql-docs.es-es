---
description: Update (método)
title: Método Update | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::Update
helpviewer_keywords:
- Update method [ADO]
ms.assetid: 6b2a9c31-1a7e-40db-8a53-30720d0f6cc1
author: rothja
ms.author: jroth
ms.openlocfilehash: 279d0e82bff4d71a2c3b18bbdc7ff88f0b9581a9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172430"
---
# <a name="update-method"></a>Update (método)
Guarda los cambios realizados en la fila actual de un objeto de [conjunto de registros](./recordset-object-ado.md) o la colección de [campos](./fields-collection-ado.md) de un objeto de [registro](./record-object-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.Update Fields, Values  
record.Fields.Update  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Fields*  
 Opcional. **Variante** que representa un nombre único o una matriz de **variantes** que representa los nombres o posiciones ordinales del campo o los campos que desea modificar.  
  
 *Valores*  
 Opcional. **Variante** que representa un valor único o una matriz de **variantes** que representa los valores para el campo o los campos del nuevo registro.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="recordset"></a>DataRecordsets  
 Utilice el método **Update** para guardar los cambios realizados en el registro actual de un objeto de **conjunto de registros** desde la llamada al método [AddNew](./addnew-method-ado.md) o desde la modificación de los valores de campo de un registro existente. El objeto de **conjunto de registros** debe admitir actualizaciones.  
  
 Para establecer los valores de los campos, realice una de las acciones siguientes:  
  
-   Asigne valores a la propiedad [Value](./value-property-ado.md) de un objeto [Field](./field-object.md) y llame al método **Update** .  
  
-   Pase un nombre de campo y un valor como argumentos con la llamada de **actualización** .  
  
-   Pase una matriz de nombres de campo y una matriz de valores con la llamada de **actualización** .  
  
 Cuando se usan matrices de campos y valores, debe haber un número igual de elementos en ambas matrices. Además, el orden de los nombres de campo debe coincidir con el orden de los valores de campo. Si el número y el orden de los campos y valores no coinciden, se produce un error.  
  
 Si el objeto de **conjunto de registros** admite la actualización por lotes, puede almacenar en caché varios cambios en uno o más registros localmente hasta que se llame al método [UpdateBatch](./updatebatch-method.md) . Si está editando el registro actual o agregando un nuevo registro cuando se llama al método **UpdateBatch** , ADO llamará automáticamente al método **Update** para guardar los cambios pendientes en el registro actual antes de transmitir los cambios por lotes al proveedor.  
  
 Si se mueve del registro que está agregando o editando antes de llamar al método **Update** , ADO llamará automáticamente a **Update** para guardar los cambios. Debe llamar al método [CancelUpdate](./cancelupdate-method-ado.md) si desea cancelar los cambios realizados en el registro actual o descartar un registro recién agregado.  
  
 El registro actual sigue siendo actual después de llamar al método **Update** .  
  
## <a name="record"></a>Grabar  
 El método **Update** finaliza las adiciones, eliminaciones y actualizaciones de los campos de la colección [Fields](./fields-collection-ado.md) de un objeto **Record** .  
  
 Por ejemplo, los campos eliminados con el método **Delete** se marcan para su eliminación inmediatamente, pero permanecen en la colección. Se debe llamar al método **Update** para eliminar realmente estos campos de la colección del proveedor.  
  
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
 [CancelUpdate (método) (ADO)](./cancelupdate-method-ado.md)   
 [Propiedad EditMode](./editmode-property.md)   
 [Método UpdateBatch](./updatebatch-method.md)