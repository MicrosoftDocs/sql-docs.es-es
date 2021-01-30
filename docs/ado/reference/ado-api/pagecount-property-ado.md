---
description: PageCount (propiedad, ADO)
title: PageCount (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageCount
helpviewer_keywords:
- PageCount property [ADO]
ms.assetid: b601b56c-0ac4-44ee-bc91-c3d2d104f00a
author: rothja
ms.author: jroth
ms.openlocfilehash: 07eb8ef6a51b67b70b1a0bd4b8fecba116df58a4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166923"
---
# <a name="pagecount-property-ado"></a>PageCount (propiedad, ADO)
Indica el número de páginas de datos que contiene el objeto de [conjunto de registros](./recordset-object-ado.md) .  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Long** que indica el número de páginas del **conjunto de registros**.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **PageCount** para determinar el número de páginas de datos que se encuentran en el objeto de **conjunto de registros** . *Las páginas* son grupos de registros cuyo tamaño es igual al valor de la propiedad [pageSize](./pagesize-property-ado.md) . Incluso si la última página está incompleta porque hay menos registros que el valor de **pageSize** , cuenta como página adicional en el valor de **PageCount** . Si el objeto de **conjunto de registros** no admite esta propiedad, el valor será-1 para indicar que **PageCount** es Indeterminista.  
  
 Vea las propiedades **pageSize** y [AbsolutePage](./absolutepage-property-ado.md) para obtener más información sobre la funcionalidad de la página.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedades AbsolutePage, PageCount y PageSize (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [Ejemplo de propiedades AbsolutePage, PageCount y PageSize (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [Propiedad AbsolutePage (ADO)](./absolutepage-property-ado.md)   
 [PageSize (propiedad, ADO)](./pagesize-property-ado.md)   
 [Propiedad RecordCount (ADO)](./recordcount-property-ado.md)