---
description: Propiedad dinámica Optimize (ADO)
title: Optimizar Property-Dynamic (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Optimize property [ADO]
ms.assetid: a491c4ce-2b04-4c84-be83-3846bde8d16b
author: rothja
ms.author: jroth
ms.openlocfilehash: 60fb0e98e2adbd57ad9fa3a98d4d04f7def8d30d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041295"
---
# <a name="optimize-property-dynamic-ado"></a>Propiedad dinámica Optimize (ADO)
Especifica si se debe crear un índice en un [campo](./field-object.md).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **booleano** que indica si se debe crear un índice.  
  
## <a name="remarks"></a>Observaciones  
 Un índice puede mejorar el rendimiento de las operaciones que buscan u ordenan valores en un [conjunto de registros](./recordset-object-ado.md). El índice es interno de ADO; no se puede obtener acceso a él ni usarlo explícitamente en la aplicación.  
  
 Para crear un índice en un campo, establezca la propiedad **Optimize** en **true**. Para eliminar el índice, establezca esta propiedad en **false**.  
  
 **Optimize** es una propiedad dinámica anexada a la colección de [propiedades](./properties-collection-ado.md) de objeto de [campo](./field-object.md) cuando la propiedad [CursorLocation](./cursorlocation-property-ado.md) está establecida en **adUseClient**.  
  
## <a name="usage"></a>Uso  
  
```  
Dim rs As New Recordset  
Dim fld As Field  
rs.CursorLocation = adUseClient      'Enable index creation  
rs.Fields.Append "Field1", adChar, 35, adFldIsNullable  
rs.Open  
Set fld = rs.Fields(0)  
fld.Properties("Optimize") = True    'Create an index  
fld.Properties("Optimize") = False   'Delete an index  
```  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Field](./field-object.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad Optimize (VB)](./optimize-property-example-vb.md)   
 [Ejemplo de la propiedad Optimize (VC + +)](./optimize-property-example-vc.md)   
 [Propiedad Filter](./filter-property.md)   
 [Find (método) (ADO)](./find-method-ado.md)   
 [Propiedad de ordenación](./sort-property.md)