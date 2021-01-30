---
description: Propiedad AbsolutePosition (ADO)
title: AbsolutePosition (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::AbsolutePosition
helpviewer_keywords:
- AbsolutePosition property [ADO]
ms.assetid: 79f8ee5e-fc70-46d8-8c29-ebf943c66592
author: rothja
ms.author: jroth
ms.openlocfilehash: b394ab30d4ee8d73e879abe2687e1208ba3188ad
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161018"
---
# <a name="absoluteposition-property-ado"></a>Propiedad AbsolutePosition (ADO)
Indica la posición ordinal del registro actual de un objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Para el código de 32 bits, establece o devuelve un valor **Long** de 1 al número de registros en el objeto de **conjunto de registros** ([RecordCount](./recordcount-property-ado.md)) o devuelve uno de los valores de [PositionEnum](./positionenum.md) .  
  
 Para el código de 64 bits, use un tipo de datos que proporcione para el almacenamiento de un valor de 64 bits. Por ejemplo, puede usar un valor Long u otro que tenga una longitud de 64 bits como DBORDINAL. No use valores **PositionEnum** , ya que están limitados a una longitud de 32 bits.  
  
## <a name="remarks"></a>Observaciones  
 Para establecer la propiedad **AbsolutePosition** , ADO requiere que el OLE DB proveedor que está usando implemente la interfaz [IRowsetLocate: IRowset](/previous-versions/windows/desktop/ms721190(v=vs.85)) .  
  
 El acceso a la propiedad **AbsolutePosition** de un **conjunto de registros** que se abrió con un cursor de solo avance o dinámico genera el error **adErrFeatureNotAvailable**. Con otros tipos de cursor, se devolverá la posición correcta siempre que el proveedor de OLE DB admita la interfaz **IRowsetScroll: IRowsetLocate** . Si el proveedor no admite la interfaz **IRowsetScroll** , la propiedad se establece en **adPosUnknown**. Consulte la documentación del proveedor para determinar si admite **IRowsetScroll**.  
  
 Use la propiedad **AbsolutePosition** para desplace un registro basándose en su posición ordinal en el objeto de **conjunto de registros** o para determinar la posición ordinal del registro actual. El proveedor debe admitir la funcionalidad adecuada para que esta propiedad esté disponible.  
  
 Al igual que la propiedad [AbsolutePage](./absolutepage-property-ado.md) , **AbsolutePosition** es de base 1 y es igual a 1 cuando el registro actual es el primer registro del **conjunto de registros**. Puede obtener el número total de registros en el objeto de **conjunto de registros** de la propiedad [RecordCount](./recordcount-property-ado.md) .  
  
 Cuando se establece la propiedad **AbsolutePosition** , incluso si se trata de un registro en la caché actual, ADO vuelve a cargar la memoria caché con un nuevo grupo de registros a partir del registro especificado. La propiedad [CacheSize](./cachesize-property-ado.md) determina el tamaño de este grupo.  
  
> [!NOTE]
>  No debe utilizar la propiedad **AbsolutePosition** como un número de registro suplente. La posición de un registro determinado cambia cuando se elimina un registro anterior. Tampoco existe la garantía de que un registro determinado tendrá el mismo **AbsolutePosition** si el objeto de **conjunto de registros** se reconsulta o se vuelve a abrir. Los marcadores siguen siendo la forma recomendada de conservar y volver a una posición determinada y son la única forma de colocar en todos los tipos de objetos de **conjunto de registros** .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades AbsolutePosition y CursorLocation (VB)](./absoluteposition-and-cursorlocation-properties-example-vb.md)   
 [Ejemplo de las propiedades AbsolutePosition y CursorLocation (VC + +)](./absoluteposition-and-cursorlocation-properties-example-vc.md)   
 [Propiedad AbsolutePage (ADO)](./absolutepage-property-ado.md)   
 [Propiedad RecordCount (ADO)](./recordcount-property-ado.md)