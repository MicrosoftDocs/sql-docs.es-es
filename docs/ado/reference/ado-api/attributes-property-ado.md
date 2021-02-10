---
description: Propiedad Attributes (ADO)
title: Propiedad Attributes (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::Attributes
- Field20::Attributes
- _Parameter::Attributes
helpviewer_keywords:
- Attributes property [ADO]
ms.assetid: acc15d40-68a6-4ba9-85bd-12d331aecaa6
author: rothja
ms.author: jroth
ms.openlocfilehash: 25899a7e6c22b119e58d21c51a514ebe311576f0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100027595"
---
# <a name="attributes-property-ado"></a>Propiedad Attributes (ADO)
Indica una o más características de un objeto.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **Long** .  
  
 Para un objeto de [conexión](./connection-object-ado.md) , la propiedad **attributes** es de lectura y escritura, y su valor puede ser la suma de uno o más valores [XactAttributeEnum](./xactattributeenum.md) . El valor predeterminado es cero (0).  
  
 Para un objeto de [parámetro](./parameter-object.md) , la propiedad **attributes** es de lectura y escritura, y su valor puede ser la suma de uno o más valores [ParameterAttributesEnum](./parameterattributesenum.md) . El valor predeterminado es **adParamSigned**.  
  
 Para un objeto de [campo](./field-object.md) , la propiedad **attributes** puede ser la suma de uno o más valores de [FieldAttributeEnum](./fieldattributeenum.md) . Normalmente es de solo lectura. Sin embargo, para los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un [registro](./record-object-ado.md), **los atributos** son de lectura y escritura solo después de que se haya especificado la propiedad [Value](./value-property-ado.md) para el **campo** y el proveedor de datos haya agregado correctamente el nuevo **campo** llamando al método [Update](./update-method.md) de la colección **Fields** .  
  
 Para un objeto de [propiedad](./property-object-ado.md) , la propiedad **attributes** es de solo lectura y su valor puede ser la suma de uno o más valores [PropertyAttributesEnum](./propertyattributesenum.md) .  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **attributes** para establecer o devolver características de objetos de **conexión** , objetos de **parámetro** , objetos de **campo** o objetos de **propiedad** .  
  
 Al establecer varios atributos, puede sumar las constantes adecuadas. Si establece el valor de la propiedad en una suma que incluye constantes incompatibles, se produce un error.  
  
> [!NOTE]
>  **Uso del servicio de datos remotos** Esta propiedad no está disponible en un objeto de **conexión** del lado cliente.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto de conexión (ADO)](./connection-object-ado.md)  
        [Objeto Field](./field-object.md)  
    :::column-end:::
    :::column:::
        [Objeto Parameter](./parameter-object.md)  
        [Objeto Property (ADO)](./property-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedades de atributos y nombres (VB)](./attributes-and-name-properties-example-vb.md)   
 [Ejemplo de propiedades de atributos y nombres (VC + +)](./attributes-and-name-properties-example-vc.md)   
 [AppendChunk (método) (ADO)](./appendchunk-method-ado.md)   
 [Métodos BeginTrans, CommitTrans y RollbackTrans (ADO)](./begintrans-committrans-and-rollbacktrans-methods-ado.md)   
 [Método GetChunk (ADO)](./getchunk-method-ado.md)