---
description: Método UpdateBatch
title: Método UpdateBatch | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::UpdateBatch
- Recordset15::raw_UpdateBatch
helpviewer_keywords:
- UpdateBatch method [ADO]
ms.assetid: 23f9314c-b027-4a51-aeae-50caa2977740
author: rothja
ms.author: jroth
ms.openlocfilehash: 8bc846e849df5980efca8f5e28982c964276f8bb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166322"
---
# <a name="updatebatch-method"></a>Método UpdateBatch
Escribe todas las actualizaciones de Batch pendientes en el disco.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.UpdateBatch AffectRecords, PreserveStatus  
```  
  
#### <a name="parameters"></a>Parámetros  
 *AffectRecords*  
 Opcional. Valor de [AffectEnum](./affectenum.md) que indica el número de registros a los que afectará el método **UpdateBatch** .  
  
 *PreserveStatus*  
 Opcional. Valor **booleano** que especifica si se deben confirmar o no los cambios locales, como indica la propiedad de [Estado](./status-property-ado-recordset.md) . Si este valor se establece en **true**, la propiedad **status** de cada registro permanece sin cambios una vez completada la actualización.  
  
## <a name="remarks"></a>Observaciones  
 Use el método **UpdateBatch** al modificar un objeto de **conjunto de registros** en modo de actualización por lotes para transmitir todos los cambios realizados en un objeto de conjunto de **registros** a la base de datos subyacente.  
  
 Si el objeto de **conjunto de registros** admite la actualización por lotes, puede almacenar en caché varios cambios en uno o más registros localmente hasta que se llame al método **UpdateBatch** . Si está editando el registro actual o agregando un nuevo registro cuando se llama al método **UpdateBatch** , ADO llamará automáticamente al método [Update](./update-method.md) para guardar los cambios pendientes en el registro actual antes de transmitir los cambios por lotes al proveedor. Debe usar la actualización por lotes solo con un cursor Keyset o static.  
  
> [!NOTE]
>  Si se especifica **adAffectGroup** como valor para este parámetro, se producirá un error cuando no haya registros visibles en el **conjunto de registros** actual (por ejemplo, un filtro para el que no coincidan los registros).  
  
 Si se produce un error al intentar transmitir los cambios para alguno o todos los registros debido a un conflicto con los datos subyacentes (por ejemplo, otro usuario ya ha eliminado un registro), el proveedor devuelve advertencias a la colección de [errores](./errors-collection-ado.md) y se produce un error en tiempo de ejecución. Use la propiedad [Filter](./filter-property.md) (**adFilterAffectedRecords**) y la propiedad [status](./status-property-ado-recordset.md) para buscar registros con conflictos.  
  
 Para cancelar todas las actualizaciones de Batch pendientes, use el método [CancelBatch](./cancelbatch-method-ado.md) .  
  
 Si se establecen las propiedades dinámicas de [tabla única](./unique-table-unique-schema-unique-catalog-properties-dynamic-ado.md) y de [resincronización de actualizaciones](./update-resync-property-dynamic-ado.md) , y el **conjunto de registros** es el resultado de ejecutar una operación de combinación en varias tablas, la ejecución del método **UpdateBatch** va seguida implícitamente por el método [Resync](./resync-method.md) , dependiendo de la configuración de la propiedad de [resincronización](./update-resync-property-dynamic-ado.md) de la actualización.  
  
 El orden en que se realizan las actualizaciones individuales de un lote en el origen de datos no es necesariamente el mismo que el orden en el que se realizaron en el **conjunto de registros** local. El orden de actualización depende del proveedor. Tenga esto en cuenta al codificar las actualizaciones relacionadas entre sí, como las restricciones Foreign Key en una instrucción INSERT o Update.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos UpdateBatch y CancelBatch (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [Ejemplo de los métodos UpdateBatch y CancelBatch (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [CancelBatch (método) (ADO)](./cancelbatch-method-ado.md)   
 [Clear (método) (ADO)](./clear-method-ado.md)   
 [LockType (propiedad, ADO)](./locktype-property-ado.md)   
 [Update (método)](./update-method.md)