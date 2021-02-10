---
description: Colección de errores (ADO)
title: Colección de errores (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Errors
- Connection15::get_Errors
- Connection15::GetErrors
helpviewer_keywords:
- Errors collection [ADO]
ms.assetid: 290819e1-7b39-4e1e-a93b-801257138b00
author: rothja
ms.author: jroth
ms.openlocfilehash: c243b27a03a5fb80b3444267a184ab995212226d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100025042"
---
# <a name="errors-collection-ado"></a>Colección de errores (ADO)
Contiene todos los objetos de [error](../../../ado/reference/ado-api/error-object.md) creados en respuesta a un error único relacionado con el proveedor.  
  
## <a name="remarks"></a>Observaciones  
 Cualquier operación relacionada con objetos ADO puede generar uno o más errores de proveedor. Cuando se produce cada error, se pueden colocar uno o más objetos de **error** en la colección de **errores** del objeto de [conexión](../../../ado/reference/ado-api/connection-object-ado.md) . Cuando otra operación de ADO genera un error, la colección de **errores** se borra y el nuevo conjunto de objetos de **error** se puede colocar en la colección de **errores** .  
  
 Cada objeto de **error** representa un error de proveedor concreto, no un error de ADO. Los errores de ADO se exponen al mecanismo de control de excepciones en tiempo de ejecución. Por ejemplo, en Microsoft Visual Basic, la aparición de un error específico de ADO desencadenará un evento [OnError](../../../ado/reference/rds-api/onerror-event-rds.md) y aparecerá en el objeto **Err** .  
  
 Las operaciones de ADO que no generan un error no tienen ningún efecto en la colección de **errores** . Utilice el método [Clear](../../../ado/reference/ado-api/clear-method-ado.md) para borrar manualmente la colección de **errores** .  
  
 El conjunto de objetos de **error** de la colección de **errores** describe todos los errores que se produjeron en respuesta a una única instrucción. Enumerar los errores específicos de la colección de **errores** permite que las rutinas de control de errores determinen con mayor precisión la causa y el origen de un error, y adopten los pasos adecuados para realizar la recuperación.  
  
 Algunas propiedades y métodos devuelven advertencias que aparecen como objetos de **error** en la colección de **errores** , pero no detienen la ejecución de un programa. Antes de llamar a los métodos [Resync](../../../ado/reference/ado-api/resync-method.md), [UpdateBatch](../../../ado/reference/ado-api/updatebatch-method.md)o [CancelBatch](../../../ado/reference/ado-api/cancelbatch-method-ado.md) en un objeto de [conjunto de registros](../../../ado/reference/ado-api/recordset-object-ado.md) , el método [Open](../../../ado/reference/ado-api/open-method-ado-connection.md) en un objeto **Connection** o establecer la propiedad [Filter](../../../ado/reference/ado-api/filter-property.md) en un objeto de **conjunto de registros** , llame al método **Clear** en la colección de **errores** . De este modo, puede leer la propiedad [Count](../../../ado/reference/ado-api/count-property-ado.md) de la colección de errores para probar si **hay** advertencias devueltas.  
  
> [!NOTE]
>  Vea el tema sobre el objeto de **error** para obtener una explicación más detallada de la forma en que una única operación de ADO puede generar varios errores.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de la colección de errores](../../../ado/reference/ado-api/errors-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Error (objeto)](../../../ado/reference/ado-api/error-object.md)   
 [Apéndice A: Proveedores](../../../ado/guide/appendixes/appendix-a-providers.md)
