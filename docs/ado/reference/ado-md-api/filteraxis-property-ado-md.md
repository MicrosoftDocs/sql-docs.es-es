---
description: Propiedad FilterAxis (ADO MD)
title: Propiedad FilterAxis (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Cellset::FilterAxis
- FilterAxis
helpviewer_keywords:
- FilterAxis property [ADO MD]
ms.assetid: 9c656963-531e-4cd1-b698-d5f42a9b7ba3
author: rothja
ms.author: jroth
ms.openlocfilehash: e53967bf577bfc69c07d7c1e5e49e42a3ca613d7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054720"
---
# <a name="filteraxis-property-ado-md"></a>Propiedad FilterAxis (ADO MD)
Indica la información de filtro sobre el [Cellset](./cellset-object-ado-md.md)actual.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un objeto de [eje](./axis-object-ado-md.md) y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **FilterAxis** para devolver información sobre las dimensiones que se usaron para segmentar los datos. La propiedad [DimensionCount](./dimensioncount-property-ado-md.md) del **eje** devuelve el número de dimensiones de segmentación. Este eje normalmente tiene solo una fila.  
  
 El **eje** devuelto por **FilterAxis** no está incluido en la colección [Axes](./axes-collection-ado-md.md) de un objeto [Cellset](./cellset-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de conjunto de celdas (ADO MD)](./cellset-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Objeto AXIS (ADO MD)](./axis-object-ado-md.md)   
 [Objeto Dimension (ADO MD)](./dimension-object-ado-md.md)   
 [DimensionCount (propiedad, ADO MD)](./dimensioncount-property-ado-md.md)