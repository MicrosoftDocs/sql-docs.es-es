---
description: BOF, EOF (propiedades) (ADO)
title: Propiedades BOF, EOF (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::BOF
- Recordset15::EOF
helpviewer_keywords:
- EOF property [ADO]
- BOF property [ADO]
ms.assetid: 36c31ab2-f3b6-4281-89b6-db7e04e38fd2
author: rothja
ms.author: jroth
ms.openlocfilehash: 71424db7a06dc8b1d5d47a3505e90ffaf175cc3b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167829"
---
# <a name="bof-eof-properties-ado"></a>BOF, EOF (propiedades) (ADO)
-   **BOF** Indica que la posición del registro actual es anterior al primer registro de un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
-   **EOF** Indica que la posición del registro actual es posterior al último registro de un objeto de **conjunto de registros** .  
  
## <a name="return-value"></a>Valor devuelto  
 Las propiedades **BOF** y **EOF** devuelven valores **booleanos** .  
  
## <a name="remarks"></a>Observaciones  
 Use las propiedades **BOF** y **EOF** para determinar si un objeto de **conjunto de registros** contiene registros o si ha ido más allá de los límites de un objeto de conjunto de **registros** al desplazarse de un registro a otro.  
  
 La propiedad **BOF** devuelve **true** (-1) si la posición del registro actual es anterior al primer registro y **false** (0) si la posición del registro actual está en el primer registro o después del mismo.  
  
 La propiedad **EOF** devuelve **true** si la posición del registro actual está después del último registro y **false** si la posición del registro actual es el último registro o anterior.  
  
 Si el valor de la propiedad **BOF** o **EOF** es **true**, no hay ningún registro actual.  
  
 Si abre un objeto **de conjunto de registros** que no contiene registros, las propiedades **BOF** y **EOF** se establecen en **true** (vea la propiedad [RecordCount](./recordcount-property-ado.md) para obtener más información sobre este estado de un **conjunto de registros**). Al abrir un objeto de **conjunto de registros** que contiene al menos un registro, el primer registro es el registro actual y las propiedades **BOF** y **EOF** son **false**.  
  
 Si elimina el último registro restante en el objeto de **conjunto de registros** , las propiedades **BOF** y **EOF** pueden seguir siendo **false** hasta que intente volver a colocar el registro actual.  
  
 En esta tabla se muestran los métodos de **movimiento** que se permiten con diferentes combinaciones de las propiedades **BOF** y **EOF** .  
  
||MoveFirst<br /><br /> MoveLast|MovePrevious<br /><br /> Desplazamiento < 0|Movimiento 0|MoveNext<br /><br /> Desplazamiento > 0|  
|------|-----------------------------|---------------------------------|------------|-----------------------------|  
|**BOF** = **True**, **EOF** = **false**|Permitido|Error|Error|Permitido|  
|**BOF** = **False**, **EOF** = **true**|Permitida|Permitida|Error|Error|  
|Ambos **verdaderos**|Error|Error|Error|Error|  
|Ambos **falsos**|Permitida|Permitida|Permitida|Permitida|  
  
 Al permitir un método **Move** no se garantiza que el método busque un registro correctamente. solo significa que al llamar al método de **movimiento** especificado no se generará un error.  
  
 En la tabla siguiente se muestra lo que ocurre con los valores de la propiedad **BOF** y **EOF** cuando se llama a varios métodos de **movimiento** , pero no se puede localizar correctamente un registro.  
  
||BOF|EOF|  
|------|---------|---------|  
|**MoveFirst**, **MoveLast**|Establézcalo en **true**|Establézcalo en **true**|  
|**Movimiento** 0|Sin cambios|Sin cambios|  
|**MovePrevious**, **Move** < 0|Establézcalo en **true**|Sin cambios|  
|**MoveNext**, **Move** > 0|Sin cambios|Establézcalo en **true**|  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades BOF, EOF y Bookmark (VB)](./bof-eof-and-bookmark-properties-example-vb.md)   
 [Ejemplo de las propiedades BOF, EOF y Bookmark (VC + +)](./bof-eof-and-bookmark-properties-example-vc.md)