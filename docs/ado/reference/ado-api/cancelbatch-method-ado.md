---
description: Método CancelBatch (ADO)
title: CancelBatch (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::raw_CancelBatch
- Recordset15::CancelBatch
helpviewer_keywords:
- CancelBatch method [ADO]
ms.assetid: dbdc2574-e44e-4d95-b03d-4a5d9e9adf3c
author: rothja
ms.author: jroth
ms.openlocfilehash: edbafa81c787bb4dafed6688f43907a87504c3c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164735"
---
# <a name="cancelbatch-method-ado"></a>Método CancelBatch (ADO)
Cancela una actualización por lotes pendiente.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
recordset.CancelBatchAffectRecords  
```  
  
#### <a name="parameters"></a>Parámetros  
 *AffectRecords*  
 Opcional. Valor de [AffectEnum](./affectenum.md) que indica el número de registros a los que afectará el método **CancelBatch** .  
  
## <a name="remarks"></a>Observaciones  
 Use el método **CancelBatch** para cancelar todas las actualizaciones pendientes en un [conjunto de registros](./recordset-object-ado.md) en modo de actualización por lotes. Si el **conjunto de registros** está en modo de actualización inmediata, al llamar a **CancelBatch** sin **adAffectCurrent** se genera un error.  
  
 Si está editando el registro actual o está agregando un nuevo registro cuando se llama a **CancelBatch**, ADO llama primero al método [CancelUpdate](./cancelupdate-method-ado.md) para cancelar los cambios almacenados en caché. Después, se cancelan todos los cambios pendientes en el **conjunto de registros** .  
  
 El registro actual puede ser indeterminable después de una llamada a **CancelBatch** , especialmente si estaba en el proceso de agregar un nuevo registro. Por esta razón, es prudente establecer la posición del registro actual en una ubicación conocida del conjunto de **registros** después de la llamada a **CancelBatch** . Por ejemplo, llame al método [MoveFirst](./movefirst-movelast-movenext-and-moveprevious-methods-ado.md) .  
  
 Si se produce un error al intentar cancelar las actualizaciones pendientes debido a un conflicto con los datos subyacentes (por ejemplo, si otro usuario ha eliminado un registro), el proveedor devuelve advertencias a la colección de [errores](./errors-collection-ado.md) , pero no detiene la ejecución del programa. Solo se produce un error en tiempo de ejecución si hay conflictos en todos los registros solicitados. Use la propiedad [Filter](./filter-property.md) (**adFilterAffectedRecords**) y la propiedad [status](./status-property-ado-recordset.md) para buscar registros con conflictos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos UpdateBatch y CancelBatch (VB)](./updatebatch-and-cancelbatch-methods-example-vb.md)   
 [Ejemplo de los métodos UpdateBatch y CancelBatch (VC + +)](./updatebatch-and-cancelbatch-methods-example-vc.md)   
 [CANCEL (método) (ADO)](./cancel-method-ado.md)   
 [Método CANCEL (RDS)](../rds-api/cancel-method-rds.md)   
 [CancelUpdate (método) (ADO)](./cancelupdate-method-ado.md)   
 [Método CancelUpdate (RDS)](../rds-api/cancelupdate-method-rds.md)   
 [Clear (método) (ADO)](./clear-method-ado.md)   
 [LockType (propiedad, ADO)](./locktype-property-ado.md)   
 [Método UpdateBatch](./updatebatch-method.md)