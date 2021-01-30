---
description: Propiedad Status (conjunto de registros ADO)
title: Propiedad Status (conjunto de registros ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::GetStatus
- Recordset15::Status
helpviewer_keywords:
- Status property [ADO Recordset]
ms.assetid: 41d70d89-880f-4850-9d17-19d9790cc8eb
author: rothja
ms.author: jroth
ms.openlocfilehash: e09b3d093f58890efd893ceb0ac204ab6ed12a46
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170191"
---
# <a name="status-property-ado-recordset"></a>Propiedad Status (conjunto de registros ADO)
Indica el estado del registro actual con respecto a las actualizaciones por lotes o a otras operaciones masivas.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve una suma de uno o más valores de [RecordStatusEnum](./recordstatusenum.md) .  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **status** para ver qué cambios están pendientes para los registros modificados durante la actualización por lotes. También puede usar la propiedad **status** para ver el estado de los registros que no se superan durante las operaciones masivas, como cuando se llama a los métodos [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)o [CancelBatch](./cancelbatch-method-ado.md) en un objeto de [conjunto de registros](./recordset-object-ado.md) , o se establece la propiedad [Filter](./filter-property.md) en un objeto de **conjunto de registros** en una matriz de marcadores. Con esta propiedad, puede determinar cómo se produjo un error en un registro determinado y resolverlo en consecuencia.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad de estado (conjunto de registros) (VB)](./status-property-example-recordset-vb.md)   
 [Ejemplo de la propiedad de estado (VC ++)](./status-property-example-vc.md)