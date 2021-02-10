---
description: Propiedad Precision (ADO)
title: Propiedad Precision (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Precision
- Field20::Precision
helpviewer_keywords:
- Precision property [ADO]
ms.assetid: 1fa38e78-6b5b-414d-ba0a-3dd26b29b766
author: rothja
ms.author: jroth
ms.openlocfilehash: 0060418a7b2cce6f7d82ecfe1095e35102aadbff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041015"
---
# <a name="precision-property-ado"></a>Propiedad Precision (ADO)
Indica el grado de precisión de los valores numéricos de un objeto de [parámetro](./parameter-object.md) o de los objetos de [campo](./field-object.md) numérico.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **byte** que indica el número máximo de dígitos utilizados para representar valores.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **precisión** para determinar el número máximo de dígitos que se usan para representar valores para un **parámetro** numérico o un objeto de **campo** .  
  
 El valor es de lectura y escritura en un objeto de **parámetro** .  
  
 Para un objeto de **campo**, la **precisión** suele ser de solo lectura. Sin embargo, para los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un [registro](./record-object-ado.md), la **precisión** es Read/Write solo después de que se haya especificado la propiedad [Value](./value-property-ado.md) para el **campo** y el proveedor de datos haya agregado correctamente el nuevo **campo** llamando al método [Update](./update-method.md) de la colección **Fields** .  
  
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
 [NumericScale (propiedad, ADO)](./numericscale-property-ado.md)