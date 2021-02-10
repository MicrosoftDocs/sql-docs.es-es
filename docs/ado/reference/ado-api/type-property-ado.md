---
description: Tipo (propiedad, ADO)
title: Type (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Type
- Field20::Type
helpviewer_keywords:
- Type property [ADO]
ms.assetid: 8a4c079f-9f4f-4545-801d-85983b8db71e
author: rothja
ms.author: jroth
ms.openlocfilehash: feb964fbc6cdd59be000e82c4a58b359cfc23d57
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056440"
---
# <a name="type-property-ado"></a>Tipo (propiedad, ADO)
Indica el tipo operativo o el tipo de datos de un [parámetro](./parameter-object.md), [campo](./field-object.md)o objeto de [propiedad](./property-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor [DataTypeEnum](./datatypeenum.md) .  
  
## <a name="remarks"></a>Observaciones  
 En el caso de los objetos de **parámetro** , la propiedad **Type** es de lectura y escritura. En el caso de los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un [registro](./record-object-ado.md), el **tipo** es Read/Write solo después de que se haya especificado la propiedad [Value](./value-property-ado.md) para el **campo** y el proveedor de datos haya agregado correctamente el nuevo **campo** llamando al método [Update](./update-method.md) de la colección **Fields** .  
  
 Para todos los demás objetos, la propiedad **Type** es de solo lectura.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objeto Parameter](./parameter-object.md)  
    :::column-end:::
    :::column:::
        [Objeto Property (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad de tipo (campo) (VB)](./type-property-example-field-vb.md)   
 [Ejemplo de propiedad de tipo (propiedad) (VC + +)](./type-property-example-property-vc.md)   
 [RecordType (propiedad, ADO)](./recordtype-property-ado.md)   
 [Propiedad Type (objeto Stream de ADO)](./type-property-ado-stream.md)