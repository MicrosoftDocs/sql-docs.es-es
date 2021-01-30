---
description: Método AppendChunk (ADO)
title: AppendChunk (método) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Parameter::AppendChunk
- Field20::AppendChunk
helpviewer_keywords:
- AppendChunk method [ADO]
ms.assetid: c648b5a8-d4f1-4d16-836e-3957feb03617
author: rothja
ms.author: jroth
ms.openlocfilehash: b7abcebce9f587260d7147745ab99c2b4cecc78a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167880"
---
# <a name="appendchunk-method-ado"></a>Método AppendChunk (ADO)
Anexa datos a un [campo](./field-object.md)de datos binario o de texto grande, o a un objeto de [parámetro](./parameter-object.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
object.AppendChunk Data  
```  
  
#### <a name="parameters"></a>Parámetros  
 *object*  
 Un **campo** o un objeto de **parámetro** .  
  
 *Data*  
 **Variante** que contiene los datos que se van a anexar al objeto.  
  
## <a name="remarks"></a>Observaciones  
 Use el método **AppendChunk** en un **campo** o un objeto de **parámetro** para rellenarlo con datos binarios o de caracteres largos. En situaciones en las que la memoria del sistema es limitada, puede utilizar el método **AppendChunk** para manipular valores largos en partes en lugar de en su totalidad.  
  
## <a name="field"></a>Campo  
 Si el **bit adFldLong** en la propiedad [attributes](./attributes-property-ado.md) de un objeto **Field** está establecido en **true**, puede usar el método **AppendChunk** para ese campo.  
  
 La primera llamada a **AppendChunk** en un objeto de **campo** escribe datos en el campo, sobrescribiendo los datos existentes. Las llamadas posteriores a **AppendChunk** se agregan a los datos existentes. Si va a anexar datos a un campo y, a continuación, establece o lee el valor de otro campo en el registro actual, ADO supone que ha terminado de anexar los datos al primer campo. Si llama de nuevo al método **AppendChunk** en el primer campo, ADO interpreta la llamada como una nueva operación de **AppendChunk** y sobrescribe los datos existentes. El acceso a los campos de otros objetos de [conjunto de registros](./recordset-object-ado.md) que no son clones del primer objeto de **conjunto de registros** no interrumpirá las operaciones de **AppendChunk** .  
  
 Si no hay ningún registro actual al llamar a **AppendChunk** en un objeto **Field** , se produce un error.  
  
> [!NOTE]
>  El método **AppendChunk** no funciona en objetos de **campo** de un objeto de [objeto de registro (ADO)](./record-object-ado.md) . No realiza ninguna operación y producirá un error en tiempo de ejecución.  
  
## <a name="parameter"></a>Parámetro  
 Si el **bit adParamLong** en la propiedad **attributes** de un objeto de **parámetro** se establece en **true**, puede usar el método **AppendChunk** para ese parámetro.  
  
 La primera llamada a **AppendChunk** en un objeto de **parámetro** escribe datos en el parámetro, sobrescribiendo los datos existentes. Las llamadas a **AppendChunk** posteriores en un objeto de **parámetro** se agregan a los datos de parámetro existentes. Una llamada a **AppendChunk** que pasa un valor null descarta todos los datos de parámetros.  
  
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
 [Ejemplo de los métodos AppendChunk y GetChunk (VB)](./appendchunk-and-getchunk-methods-example-vb.md)   
 [Ejemplo de los métodos AppendChunk y GetChunk (VC + +)](./appendchunk-and-getchunk-methods-example-vc.md)   
 [Propiedad Attributes (ADO)](./attributes-property-ado.md)   
 [Método GetChunk (ADO)](./getchunk-method-ado.md)