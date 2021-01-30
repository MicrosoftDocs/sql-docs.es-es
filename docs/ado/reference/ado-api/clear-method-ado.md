---
description: Clear (método) (ADO)
title: Clear (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Errors::raw_Clear
- Errors::Clear
helpviewer_keywords:
- Clear method [ADO]
ms.assetid: 0a61ba7a-20b8-426a-91a0-9040e7c5a98a
author: rothja
ms.author: jroth
ms.openlocfilehash: 1f79e81947a549057db5dbff7f6715492d08923a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164713"
---
# <a name="clear-method-ado"></a>Clear (método) (ADO)
Quita todos los objetos de [error](./error-object.md) de la colección de [errores](./errors-collection-ado.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Errors.Clear  
```  
  
## <a name="remarks"></a>Observaciones  
 Utilice el método **Clear** en la colección de [errores](./errors-collection-ado.md) para quitar todos los objetos de [error](./error-object.md) existentes de la colección. Cuando se produce un error, ADO borra automáticamente la colección de **errores** y la rellena con objetos de **error** en función del nuevo error.  
  
 Algunas propiedades y métodos devuelven advertencias que aparecen como objetos de **error** en la colección de **errores** , pero no detienen la ejecución de un programa. Antes de llamar a los métodos [Resync](./resync-method.md), [UpdateBatch](./updatebatch-method.md)o [CancelBatch](./cancelbatch-method-ado.md) en un objeto de [conjunto de registros](./recordset-object-ado.md) ; el método [Open](./open-method-ado-connection.md) en un objeto [Connection](./connection-object-ado.md) ; o bien, establezca la propiedad [Filter](./filter-property.md) en un objeto de **conjunto de registros** , llame al método **Clear** en la colección de **errores** . De este modo, puede leer la propiedad [Count](./count-property-ado.md) de la colección de errores para probar si **hay** advertencias devueltas.  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de errores (ADO)](./errors-collection-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos Execute, Requery y Clear (VB)](./execute-requery-and-clear-methods-example-vb.md)   
 [Ejemplo de métodos Execute, Requery y Clear (VBScript)](./execute-requery-and-clear-methods-example-vbscript.md)   
 [Ejemplo de los métodos Execute, Requery y Clear (VC + +)](./execute-requery-and-clear-methods-example-vc.md)   
 [CancelBatch (método) (ADO)](./cancelbatch-method-ado.md)   
 [Método Delete (colección Fields de ADO)](./delete-method-ado-fields-collection.md)   
 [Método Delete (colección de parámetros de ADO)](./delete-method-ado-parameters-collection.md)   
 [Delete (método) (conjunto de registros ADO)](./delete-method-ado-recordset.md)   
 [Propiedad Filter](./filter-property.md)   
 [Método Resync](./resync-method.md)   
 [Método UpdateBatch](./updatebatch-method.md)