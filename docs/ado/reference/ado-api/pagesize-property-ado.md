---
description: Propiedad PageSize (ADO)
title: PageSize (propiedad, ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Recordset15::PageSize
helpviewer_keywords:
- PageSize property [ADO]
ms.assetid: e57930a6-46c4-4a17-a3b6-f79e94d5c9c7
author: rothja
ms.author: jroth
ms.openlocfilehash: 8d37ac91b1899c0eb515d8443cfc5c3e2f9463cb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041215"
---
# <a name="pagesize-property-ado"></a>Propiedad PageSize (ADO)
Indica el número de registros que constituyen una página en el [conjunto de registros](./recordset-object-ado.md).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor **Long** que indica el número de registros que hay en una página. El valor predeterminado es **10**.  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **pageSize** para determinar el número de registros que conforman una página de datos lógica. Establecer un tamaño de página permite utilizar la propiedad [AbsolutePage](./absolutepage-property-ado.md) para desplace al primer registro de una página determinada. Esto resulta útil en escenarios Web-Server cuando desea permitir que el usuario recorra en páginas los datos, viendo un número determinado de registros a la vez.  
  
 Esta propiedad se puede establecer en cualquier momento y su valor se usará para calcular la ubicación del primer registro de una página determinada.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de registros (ADO)](./recordset-object-ado.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedades AbsolutePage, PageCount y PageSize (VB)](./absolutepage-pagecount-and-pagesize-properties-example-vb.md)   
 [Ejemplo de propiedades AbsolutePage, PageCount y PageSize (VC + +)](./absolutepage-pagecount-and-pagesize-properties-example-vc.md)   
 [Propiedad AbsolutePage (ADO)](./absolutepage-property-ado.md)   
 [PageCount (propiedad, ADO)](./pagecount-property-ado.md)