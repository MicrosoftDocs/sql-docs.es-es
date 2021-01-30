---
description: Propiedad RecordCount (ADO)
title: Propiedad RecordCount (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 03/20/2018
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::RecordCount
- Recordset15::GetRecordCount
- Recordset15::get_RecordCount
helpviewer_keywords:
- RecordCount property [ADO]
ms.assetid: 834f0121-394a-44d4-ad7d-999b43a6fe63
author: rothja
ms.author: jroth
ms.openlocfilehash: 76f3911ccc240a29829efcd5aa301ff5dfb8cbd5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170457"
---
# <a name="recordcount-property-ado"></a>Propiedad RecordCount (ADO)

Indica el número de registros de un objeto de [conjunto de registros](./recordset-object-ado.md) .
  
## <a name="return-value"></a>Valor devuelto

Devuelve un valor de **tipo Long** que indica el número de registros del **conjunto** de registros.
  
## <a name="remarks"></a>Observaciones

Use la propiedad **RecordCount** para averiguar el número de registros que hay en un objeto de **conjunto de registros** . La propiedad devuelve-1 cuando ADO no puede determinar el número de registros o si el tipo de proveedor o cursor no admite **RecordCount**. La lectura de la propiedad **RecordCount** en un **conjunto de registros** cerrado produce un error.

#### <a name="bookmarks-or-approximate-positioning"></a>Marcadores o posicionamiento aproximado

Si el objeto de conjunto de *registros admite marcadores* o una posición aproximada, esta propiedad devuelve el número exacto de registros del conjunto de registros. Esta propiedad devuelve el número exacto sin tener en cuenta si el conjunto de registros se ha rellenado por completo.

Por el contrario, si el objeto de conjunto de registros *no* admite marcadores o una posición aproximada, el acceso a esta propiedad podría suponer un consumo significativo de recursos. La purga se produce porque todos los registros deben recuperarse y contarse para devolver un valor de RecordCount preciso.

- **adBookmark** relacionado con marcadores.
- **adApproxPosition** se relaciona con una posición aproximada.

> [!NOTE]
> En las versiones 2,8 y anteriores de ADO, el proveedor SQLOLEDB captura todos los registros cuando se utiliza un cursor de servidor, a pesar de que el hecho de que devuelva **true** para ambos **admita (AdApproxPosition)** y **admita (adBookmark)**.
  
El tipo de cursor del objeto de **conjunto de registros** afecta a si se puede determinar el número de registros. La propiedad **RecordCount** devolverá-1 para un cursor de solo avance; recuento real de un cursor estático o de conjunto de claves. y-1 o el recuento real de un cursor dinámico, dependiendo del origen de datos.
  
## <a name="applies-to"></a>Se aplica a

[Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también

[Ejemplo de las propiedades Filter y RecordCount (VB)](./filter-and-recordcount-properties-example-vb.md)   
[Ejemplo de las propiedades Filter y RecordCount (VC + +)](./filter-and-recordcount-properties-example-vc.md)   
[AbsolutePosition (propiedad, ADO)](./absoluteposition-property-ado.md)   
[PageCount (propiedad, ADO)](./pagecount-property-ado.md)