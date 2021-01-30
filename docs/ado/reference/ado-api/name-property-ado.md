---
description: Propiedad Name (ADO)
title: Name (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::Name
- Field20::Name
helpviewer_keywords:
- Name property [ADO]
ms.assetid: cfd0e29c-8310-44ab-85c3-5761184b865d
author: rothja
ms.author: jroth
ms.openlocfilehash: 384c1e96eb735d8cf9569039f45c2d420679ce0e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167072"
---
# <a name="name-property-ado"></a>Propiedad Name (ADO)
Indica el nombre de un objeto.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** que indica el nombre de un objeto.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **nombre** para asignar un nombre al nombre de un objeto de **comando**, **propiedad**, **campo** o **parámetro** .  
  
 El valor es de lectura y escritura en un objeto de **comando** y de solo lectura en un objeto de **propiedad** .  
  
 Para un objeto de **campo** , **el nombre** es normalmente de solo lectura. Sin embargo, para los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un [registro](./record-object-ado.md), **Name** es Read/Write solo después de que se haya especificado la propiedad [Value](./value-property-ado.md) para el **campo** y el proveedor de datos haya agregado correctamente el nuevo **campo** llamando al método [Update](./update-method.md) de la colección **Fields** .  
  
 En el caso de los objetos de **parámetro** que todavía no se han anexado a la colección [Parameters](./parameters-collection-ado.md) , la propiedad **Name** es de lectura y escritura. Para los objetos de **parámetro** anexados y todos los demás objetos, la propiedad **Name** es de solo lectura. Los nombres no tienen que ser únicos dentro de una colección.  
  
 Puede recuperar la propiedad **Name** de un objeto mediante una referencia ordinal, después de la cual puede hacer referencia al objeto directamente por su nombre. Por ejemplo, si `rstMain.Properties(20).Name` produce `Updatability` , puede hacer referencia posteriormente a esta propiedad como `rstMain.Properties("Updatability")` .  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Command (ADO)](./command-object-ado.md)  
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