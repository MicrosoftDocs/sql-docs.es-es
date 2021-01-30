---
description: NumericScale (propiedad, ADO)
title: Propiedad NumericScale (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::NumericScale
- Field20::NumericScale
helpviewer_keywords:
- NumericScale property [ADO]
ms.assetid: 29a02992-64be-4fcd-be13-445cba205893
author: rothja
ms.author: jroth
ms.openlocfilehash: 3164dcc61165171ff0d1c33956f152bcb65f33b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170737"
---
# <a name="numericscale-property-ado"></a>NumericScale (propiedad, ADO)
Indica la escala de valores numéricos en un [parámetro](./parameter-object.md) o un objeto de [campo](./field-object.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **byte** que indica el número de posiciones decimales en las que se resolverán los valores numéricos.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **NumericScale** para determinar cuántos dígitos a la derecha del separador decimal se utilizarán para representar los valores de un **parámetro** numérico o un objeto de **campo** .  
  
 En el caso de los objetos de **parámetro** , la propiedad **NumericScale** es de lectura y escritura.  
  
 Para un objeto de **campo**, **NumericScale** es normalmente de solo lectura. Sin embargo, para los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un [registro](./record-object-ado.md), **NumericScale** es Read/Write solo después de que se haya especificado la propiedad [Value](./value-property-ado.md) para el **campo** y el proveedor de datos haya agregado correctamente el nuevo **campo** llamando al método [Update](./update-method.md) de la colección [Fields](./fields-collection-ado.md) .  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objeto Parameter](./parameter-object.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades NumericScale y Precision (VB)](./numericscale-and-precision-properties-example-vb.md)   
 [Ejemplo de las propiedades NumericScale y Precision (VC + +)](./numericscale-and-precision-properties-example-vc.md)   
 [Propiedad Precision (ADO)](./precision-property-ado.md)