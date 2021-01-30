---
description: Propiedad Status (Field ADO)
title: Propiedad Status (campo ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field::Status
- Field::get_Status
- Field::GetStatus
helpviewer_keywords:
- Status property [ADO Field]
ms.assetid: 8cd1f7f4-0a3a-4f07-b8ba-6582e70140ad
author: rothja
ms.author: jroth
ms.openlocfilehash: b0ae4ed8f72dca085edd0c82220296d65795d6ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166470"
---
# <a name="status-property-ado-field"></a>Propiedad Status (Field ADO)
Indica el estado de un objeto de [campo](./field-object.md) .  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de [FieldStatusEnum](./fieldstatusenum.md) . El valor predeterminado es **adFieldOK**.  
  
## <a name="remarks"></a>Observaciones  
  
## <a name="record-field-status"></a>Estado del campo de registro  
 Los cambios en el valor de un objeto de **campo** en la colección Fields de un objeto [Record](./record-object-ado.md) se almacenan en caché hasta que se llama al método [Update](./update-method.md) del objeto. En ese momento, si el cambio en el valor del campo produjo un error, OLE DB genera el error **DB_E_ERRORSOCCURRED** (2147749409). La propiedad status de cualquiera de los objetos **Field** de la colección **Fields** que causó el error contendrá un valor de [FieldStatusEnum](./fieldstatusenum.md) que describe la causa del problema.  
  
 Para mejorar el rendimiento, las adiciones y eliminaciones de las colecciones de [campos](./fields-collection-ado.md) del objeto de **registro** se almacenan en caché hasta que se llama al método **Update** y, a continuación, los cambios se realizan en una actualización optimista de batch. Si no se llama al método **Update** , no se actualiza el servidor. Si se producen errores en las actualizaciones, se devuelve un error del proveedor de OLE DB (DB_E_ERRORSOCCURRED) y la propiedad **status** indica los valores combinados de la operación y el código de estado de error. Por ejemplo, **ADFIELDPENDINGINSERT o adFieldPermissionDenied**. La propiedad **status** de cada **campo** se puede usar para determinar por qué no se agregó, modificó o eliminó el **campo** .  
  
 Muchos tipos de problemas que se producen al agregar, modificar o eliminar un **campo** se indican a través de la propiedad **status** . Por ejemplo, si el usuario elimina un **campo**, se marca para su eliminación en la colección de **campos** . Si la **actualización** siguiente devuelve un error porque el usuario intentó eliminar un **campo** para el que no tiene permiso, el **campo** tendrá un **Estado** de **adFieldPermissionDenied o adFieldPendingDelete**. Al llamar al método [CancelUpdate](./cancelupdate-method-ado.md) , se restauran los valores originales y se establece el **Estado** en **adFieldOK**.  
  
 Del mismo modo, el método **Update** puede devolver un error porque se agregó un nuevo **campo** y se le dio un valor inadecuado. En ese caso, el nuevo **campo** estará en la colección de **campos** y tendrá un estado de **adFieldPendingInsert** y quizás **adFieldCantCreate** (según el proveedor). Puede proporcionar un valor adecuado para el nuevo **campo** y volver a llamar a **Update** .  
  
## <a name="recordset-field-status"></a>Estado del campo de conjunto de registros  
 Los cambios en el valor de un objeto de **campo** en la colección Fields de un [conjunto de registros](./recordset-object-ado.md) se almacenan en caché hasta que se llama al método [Update](./update-method.md) del objeto. En ese momento, si el cambio en el valor del campo produjo un error, OLE DB genera el error **DB_E_ERRORSOCCURRED** (2147749409). La propiedad status de cualquiera de los objetos **Field** de la colección **Fields** que causó el error contendrá un valor de [FieldStatusEnum](./fieldstatusenum.md) que describe la causa del problema.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Field](./field-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad de estado (campo) (VB)](./status-property-example-field-vb.md)   
 [Ejemplo de la propiedad de estado (VC ++)](./status-property-example-vc.md)