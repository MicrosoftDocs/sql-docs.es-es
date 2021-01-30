---
description: Propiedad ordinal (posición de ADO MD)
title: Propiedad ordinal (posición ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Position::Ordinal
- Ordinal
helpviewer_keywords:
- Ordinal property [ADO MD]
ms.assetid: 6efe8b5d-a2d5-43a9-a5ea-f9244f8d4ec9
author: rothja
ms.author: jroth
ms.openlocfilehash: a560b34e6ec55892a6f779c4808e5a7c69d99bca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169739"
---
# <a name="ordinal-property-ado-md-position"></a>Propiedad ordinal (posición de ADO MD)
Identifica de forma única una [posición](./position-object-ado-md.md) a lo largo de un eje.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un entero **largo** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad **ordinal** de un objeto de [posición](./position-object-ado-md.md) corresponde al índice de la **posición** dentro de la colección de [posiciones](./positions-collection-ado-md.md) .  
  
 Una celda se puede recuperar rápidamente utilizando el **ordinal** de la **posición** a lo largo de cada eje con la propiedad [Item](./item-property-ado-md-cellset.md) del objeto [Cellset](./cellset-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Position (ADO MD)](./position-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto Cellset (ADO MD)](./cellset-object-ado-md.md)   
 [Propiedad Item (ADO MD Cellset)](./item-property-ado-md-cellset.md)   
 [Propiedad ordinal (celda de ADO MD)](./ordinal-property-ado-md-cell.md)