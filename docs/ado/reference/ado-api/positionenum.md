---
description: PositionEnum
title: PositionEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- PositionEnum
helpviewer_keywords:
- PositionEnum enumeration
ms.assetid: e69af0a5-3405-4b72-9c6e-6b188ff746fd
author: rothja
ms.author: jroth
ms.openlocfilehash: 22760113f966379d8e1705420853705114d9c362
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170563"
---
# <a name="positionenum"></a>PositionEnum
Especifica la posición actual del puntero de registro dentro de un [conjunto de registros](./recordset-object-ado.md).  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adPosBOF**|-2|Indica que el puntero del registro actual está en BOF (es decir, la propiedad [BOF](./bof-eof-properties-ado.md) es **true**).|  
|**adPosEOF**|-3|Indica que el puntero del registro actual está en EOF (es decir, la propiedad [EOF](./bof-eof-properties-ado.md) es **true**).|  
|**adPosUnknown**|-1|Indica que el **conjunto de registros** está vacío, se desconoce la posición actual o el proveedor no admite la propiedad [AbsolutePage](./absolutepage-property-ado.md) o [AbsolutePosition](./absoluteposition-property-ado.md) .|  
  
## <a name="adowfc-equivalent"></a>Equivalente de ADO/WFC  
 Paquete: **com. ms. wfc. Data**  
  
|Constante|  
|--------------|  
|AdoEnums. position. BOF|  
|AdoEnums. position. EOF|  
|AdoEnums. position. UNKNOWN|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Propiedad AbsolutePage (ADO)](./absolutepage-property-ado.md)  
    :::column-end:::
    :::column:::
        [Propiedad AbsolutePosition (ADO)](./absoluteposition-property-ado.md)  
    :::column-end:::
:::row-end:::