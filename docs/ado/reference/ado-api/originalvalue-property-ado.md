---
description: Propiedad OriginalValue (ADO)
title: OriginalValue (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::OriginalValue
helpviewer_keywords:
- OriginalValue property [ADO]
ms.assetid: 6e33c6ec-14d9-4b1d-ba9b-cb99862e7bac
author: rothja
ms.author: jroth
ms.openlocfilehash: e0948b7d5dd6fef29336fbb9af49e2578b2bb4ba
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041245"
---
# <a name="originalvalue-property-ado"></a>Propiedad OriginalValue (ADO)
Indica el valor de un [campo](./field-object.md) que existía en el registro antes de que se realizaran cambios.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Variant** que representa el valor de un campo antes de cualquier cambio.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **OriginalValue** para devolver el valor de campo original de un campo del registro actual.  
  
 En el *modo de actualización inmediata* (en el que el proveedor escribe los cambios en el origen de datos subyacente después de llamar al método [Update](./update-method.md) ), la propiedad **OriginalValue** devuelve el valor del campo que existía antes de cualquier cambio (es decir, desde la última llamada al método **Update** ). Este es el mismo valor que utiliza el método [CancelUpdate](./cancelupdate-method-ado.md) para reemplazar la propiedad [Value](./value-property-ado.md) .  
  
 En el *modo de actualización por lotes* (en el que el proveedor almacena en caché varios cambios y los escribe en el origen de datos subyacente solo cuando se llama al método [UpdateBatch](./updatebatch-method.md) ), la propiedad **OriginalValue** devuelve el valor del campo que existía antes de cualquier cambio (es decir, desde la última llamada al método **UpdateBatch** ). Este es el mismo valor que utiliza el método [CancelBatch](./cancelbatch-method-ado.md) para reemplazar la propiedad **Value** . Cuando se usa esta propiedad con la propiedad [UnderlyingValue](./underlyingvalue-property.md) , se pueden resolver los conflictos que surgen de las actualizaciones por lotes.  
  
## <a name="record"></a>Grabar  
 En el caso de los objetos de [registro](./record-object-ado.md) , la propiedad **OriginalValue** estará vacía para los campos agregados antes de llamar a [Update](./update-method.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Field](./field-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades OriginalValue y UnderlyingValue (VB)](./originalvalue-and-underlyingvalue-properties-example-vb.md)   
 [Ejemplo de las propiedades OriginalValue y UnderlyingValue (VC + +)](./originalvalue-and-underlyingvalue-properties-example-vc.md)   
 [Propiedad UnderlyingValue](./underlyingvalue-property.md)