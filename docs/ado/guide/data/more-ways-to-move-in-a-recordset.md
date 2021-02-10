---
description: Otras formas de mover en un conjunto de registros
title: Más formas de moverse en un conjunto de registros | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- MoveNext method [ADO]
- MoveLast method [ADO]
- MoveFirst method [ADO]
- Recordset object [ADO], moving
- MovePrevious method [ADO]
ms.assetid: 9f8cf1b2-3def-453f-a0ff-4646c5f15262
author: rothja
ms.author: jroth
ms.openlocfilehash: 78acb389a38896d05a684b0dbd940942a026655e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032717"
---
# <a name="more-ways-to-move-in-a-recordset"></a>Otras formas de mover en un conjunto de registros
Los cuatro métodos siguientes se usan para moverse o desplazarse por el **conjunto de registros**: [MoveFirst, MoveLast, MoveNext y MovePrevious](../../reference/ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md). (Algunos de estos métodos no están disponibles en los cursores de solo avance).  
  
 **MoveFirst** cambia la posición del registro actual al primer registro del **conjunto de registros**. **MoveLast** cambia la posición del registro actual al último registro del **conjunto de registros**. Para usar **MoveFirst** o **MoveLast**, el objeto de **conjunto de registros** debe admitir marcadores o movimiento de cursor hacia atrás; de lo contrario, la llamada al método generará un error.  
  
 **MoveNext** mueve la posición del registro actual una vez hacia delante. Si se está en el último registro cuando se llama a **MoveNext**, **EOF** se establecerá en **true**. **MovePrevious** mueve hacia atrás la posición del registro actual un lugar. Si se está en el primer registro cuando se llama a **MovePrevious**, **BOF** se establecerá en **true**. Es aconsejable comprobar las propiedades **EOF** y **BOF** al utilizar estos métodos y devolver el cursor a una posición de registro actual válida si se sale de cualquier extremo del **conjunto de registros**, como se muestra aquí:  
  
```  
. . .  
oRs.MoveNext  
If oRs.EOF Then oRs.MoveLast  
. . .   
```  
  
 O bien, en el caso del método **MovePrevious** :  
  
```  
. . .   
oRs.MovePrevious  
If oRs.BOF Then oRs.MoveFirst  
. . .  
```  
  
 En los casos en que el **conjunto de registros** se ha filtrado u ordenado y se han modificado los datos del registro actual, también puede cambiar la posición. En tales casos, el método **MoveNext** funciona con normalidad, pero tenga en cuenta que la posición se mueve un registro hacia delante desde la nueva posición, no la posición anterior. Por ejemplo, si se cambian los datos en el registro actual, de modo que el registro se mueve al final del **conjunto de registros** ordenado, significaría que, al llamar a **MoveNext** , se obtiene el valor de ADO al establecer el registro actual en la posición posterior al último registro del **conjunto de registros** (**EOF**  =  **true**).  
  
 El comportamiento de los distintos métodos de movimiento del objeto de **conjunto de registros** depende, en cierta medida, de los datos del **conjunto de registros**. Los nuevos registros agregados al **conjunto de registros** se agregan inicialmente en un orden determinado, definido por el origen de datos y pueden ser dependientes implícita o explícitamente en los datos del nuevo registro. Por ejemplo, si se realiza una ordenación o una combinación dentro de la consulta que rellena el **conjunto de registros**, el nuevo registro se insertará en el lugar adecuado del conjunto de **registros**. Si no se especifica explícitamente el orden al crear el **conjunto de registros**, los cambios en la implementación del origen de datos pueden hacer que el orden de las filas devueltas cambie sin darse cuenta. Además, las funciones de ordenación, filtrado y edición del conjunto de **registros** pueden afectar al orden y, posiblemente, las filas del conjunto de registros que serán visibles.  
  
 Por lo tanto, **MoveNext**, **MovePrevious**, **MoveFirst**, **MoveLast** y **Move** son confidenciales para otras operaciones realizadas en el mismo **conjunto de registros**. ADO siempre intentará mantener la posición actual hasta que la mueva explícitamente, pero a veces los cambios intermedios hacen que sea difícil comprender los efectos de un movimiento posterior. Por ejemplo, si llama a **MoveFirst** para colocar en la primera fila de un conjunto de **registros** ordenado y cambia el orden de ascendente a descendente, todavía está en la misma fila, pero ahora es la última fila del **conjunto de registros**. **MoveFirst** le llevará a una fila diferente (la nueva primera fila).  
  
 Otro ejemplo: si se coloca en una fila determinada en medio de un **conjunto de registros** y llama a **Delete** y, a continuación, llama a **MoveNext**, ahora está en el registro inmediatamente después del registro eliminado. Pero si se llama a **MovePrevious** , el registro precede al que se ha eliminado del registro actual, ya que el registro eliminado ya no se cuenta en la pertenencia activa del **conjunto de registros**.  
  
 Es especialmente difícil definir semánticas de movimiento coherentes en todos los proveedores para los métodos que se mueven en relación con el registro actual: **MovePrevious**, **MoveNext** y **Move** -in the cambiante Data in the current record. Por ejemplo, si está trabajando con un **conjunto de registros** ordenado y filtrado, y cambia los datos en el registro actual para que preceda a todos los demás registros, pero los datos modificados tampoco coinciden con el filtro, no queda claro dónde debería llevarle una operación **MoveNext** . La conclusión más segura es que el movimiento relativo dentro de un **conjunto de registros** es más arriesgado que el movimiento absoluto (por ejemplo, mediante el uso de **MoveFirst** o **MoveLast**) cuando los datos cambian mientras se editan, agregan o eliminan registros. La ordenación y el filtrado deberían basarse en una clave principal o un identificador, ya que este tipo de valor no debe cambiar.