---
description: Value (propiedad) (ADO)
title: Value (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Field20::Value
- _Parameter::Value
helpviewer_keywords:
- Value property [ADO]
ms.assetid: 48919c74-86d4-462e-99b9-8854ceb8d683
author: rothja
ms.author: jroth
ms.openlocfilehash: b809a24429b490961e1d0d009656539db9fe81d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172404"
---
# <a name="value-property-ado"></a>Value (propiedad) (ADO)

Indica el valor asignado a un [campo](./field-object.md), un [parámetro](./parameter-object.md)o un objeto de [propiedad](./property-object-ado.md) .
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos

Establece o devuelve un valor **Variant** que indica el valor del objeto. El valor predeterminado depende de la propiedad [Type](./type-property-ado.md) .
  
## <a name="remarks"></a>Observaciones

Utilice la propiedad **Value** para establecer o devolver datos de objetos de **campo** , para establecer o devolver valores de parámetro con objetos de **parámetro** , o para establecer o devolver la configuración de propiedades con objetos de **propiedad** . El hecho de que la propiedad **Value** sea de lectura/escritura o de solo lectura depende de varios factores. Vea los temas de objetos correspondientes para obtener más información.

ADO permite establecer y devolver datos binarios largos con la propiedad **Value** .
  
> [!NOTE]
> En el caso de los objetos de **parámetro** , ADO lee la propiedad **Value** solo una vez desde el proveedor. Si un comando contiene un **parámetro** cuya **propiedad Value** está vacía y crea un conjunto de [registros](./recordset-object-ado.md) a partir del comando, asegúrese de cerrar primero el **conjunto de registros** antes de recuperar la propiedad **Value** . De lo contrario, para algunos proveedores, la propiedad **Value** puede estar vacía y no contendrá el valor correcto.
> 
> En el caso de los nuevos objetos de **campo** que se han anexado a la colección [Fields](./fields-collection-ado.md) de un objeto [Record](./record-object-ado.md) , la propiedad **Value** debe establecerse antes de que se puedan especificar otras propiedades de **campo** . En primer lugar, se debe haber asignado un valor específico para la propiedad **Value** y [actualizarse](./update-method.md) en la colección **Fields** denominada. A continuación, se puede tener acceso a otras propiedades, como el [tipo](./type-property-ado.md) o [los atributos](./attributes-property-ado.md) .
  
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

[Ejemplo de la propiedad Value (VB)](./value-property-example-vb.md) 
 [Ejemplo de la propiedad Value (VC + +)](./value-property-example-vc.md)