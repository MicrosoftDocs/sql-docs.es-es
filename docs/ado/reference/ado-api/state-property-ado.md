---
description: Propiedad State (ADO)
title: Propiedad State (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command25::State
helpviewer_keywords:
- State property [ADO]
ms.assetid: 0b993bac-2653-40b1-bcbb-5b57b6aae2bf
author: rothja
ms.author: jroth
ms.openlocfilehash: 517ffc71b5e2a95ba5f85fcc9d3561ce82b9ec86
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170211"
---
# <a name="state-property-ado"></a>Propiedad State (ADO)
Indica para todos los objetos aplicables si el estado del objeto es abierto o cerrado. Si el objeto está ejecutando un método asincrónico, indica si el estado actual del objeto es conexión, ejecución o recuperación.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Long** que puede ser un valor de [ObjectStateEnum](./objectstateenum.md) . El valor predeterminado es **adStateClosed**.  
  
## <a name="remarks"></a>Observaciones  
 Puede utilizar la propiedad **State** para determinar el estado actual de un objeto determinado en cualquier momento.  
  
 La propiedad de **Estado** del objeto puede tener una combinación de valores. Por ejemplo, si se está ejecutando una instrucción, esta propiedad tendrá un valor combinado de **adStateOpen** y **adStateExecuting**.  
  
 La propiedad **State** es de solo lectura.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Command (ADO)](./command-object-ado.md)  
        [Objeto de conexión (ADO)](./connection-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto Record (ADO)](./record-object-ado.md)  
        [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto de secuencia (ADO)](./stream-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VB)](./connectionstring-connectiontimeout-and-state-properties-example-vb.md)   
 [Ejemplo de las propiedades ConnectionString, ConnectionTimeout y State (VC + +)](./connectionstring-connectiontimeout-and-state-properties-example-vc.md)