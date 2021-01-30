---
description: Propiedad Index
title: Propiedad de índice | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset21::Index
helpviewer_keywords:
- Index property
ms.assetid: 1c79e271-21ec-41a8-8163-c5e89f0001a7
author: rothja
ms.author: jroth
ms.openlocfilehash: 9e8c346ac48092d7a5dcaf09068bb64d63316a38
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170961"
---
# <a name="index-property"></a>Propiedad Index
Indica el nombre del índice actualmente activo para un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** , que es el nombre del índice.  
  
## <a name="remarks"></a>Observaciones  
 El índice denominado por la propiedad de **Índice** debe haberse declarado previamente en la tabla base subyacente al objeto de **conjunto de registros** . Es decir, el índice se debe haber declarado mediante programación como un objeto de [Índice](../adox-api/index-object-adox.md) ADOX o cuando se haya creado la tabla base.  
  
 Si no se puede establecer el índice, se producirá un error en tiempo de ejecución. No se puede establecer la propiedad de **Índice** en las siguientes condiciones:  
  
-   Dentro de un controlador de eventos [WillChangeRecordset](./willchangerecordset-and-recordsetchangecomplete-events-ado.md) o **RecordsetChangeComplete** .  
  
-   Si el **conjunto de registros** sigue ejecutando una operación (que puede estar determinada por la propiedad [State](./state-property-ado.md) ).  
  
-   Si se ha establecido un filtro en el **conjunto de registros** con la propiedad [Filter](./filter-property.md) .  
  
 La propiedad de **Índice** siempre se puede establecer correctamente si el **conjunto de registros** está cerrado, pero el conjunto de **registros** no se abrirá correctamente o el índice no se podrá usar, si el proveedor subyacente no admite índices.  
  
 Si se puede establecer el índice, la posición de la fila actual puede cambiar. Esto producirá una actualización de la propiedad [AbsolutePosition](./absoluteposition-property-ado.md) y desencadenará los eventos **WillChangeRecordset**, **RecordsetChangeComplete**, [WillMove](./willmove-and-movecomplete-events-ado.md)y [MoveComplete](./willmove-and-movecomplete-events-ado.md) .  
  
 Si se puede establecer el índice y la propiedad [LockType](./locktype-property-ado.md) es **adLockPessimistic** o **adLockOptimistic**, se realiza una operación de [UpdateBatch](./updatebatch-method.md) implícita. Esto libera los grupos actuales y afectados. Se libera cualquier filtro existente y la posición de la fila actual se cambia a la primera fila del **conjunto de registros** reordenado.  
  
 La propiedad de **Índice** se utiliza junto con el método [Seek](./seek-method.md) . Si el proveedor subyacente no admite la propiedad de **Índice** y, por lo tanto, el método **Seek** , considere la posibilidad de usar el método [Find](./find-method-ado.md) en su lugar. Determine si el objeto de **conjunto de registros** admite índices con el método [Supports](./supports-method.md)**(adIndex)** .  
  
 La propiedad de **Índice** integrada no está relacionada con la propiedad [optimizada](./optimize-property-dynamic-ado.md) dinámica, aunque ambos tratan los índices.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad index y el método Seek (VB)](./seek-method-and-index-property-example-vb.md)   
 [Index (objeto) (ADOX)](../adox-api/index-object-adox.md)   
 [El método de búsqueda](./seek-method.md)