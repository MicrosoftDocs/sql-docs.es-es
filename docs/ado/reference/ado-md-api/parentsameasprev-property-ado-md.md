---
description: ParentSameAsPrev (propiedad, ADO MD)
title: Propiedad ParentSameAsPrev (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ParentSameAsPrev
- Member::ParentSameAsPrev
helpviewer_keywords:
- ParentSameAsPrev property [ADO MD]
ms.assetid: 510842e0-e8dc-4b33-9517-bd1c6df0cf3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 751bc45cf90a678a8feb3784e83290be32ea3e76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169732"
---
# <a name="parentsameasprev-property-ado-md"></a>ParentSameAsPrev (propiedad, ADO MD)
Indica si el elemento primario de este [miembro](./member-object-ado-md.md) de posición es igual que el elemento primario del miembro inmediatamente anterior.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un valor **booleano** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 Esta propiedad solo se admite en los objetos [member](./member-object-ado-md.md) que pertenecen a un objeto [Position](./position-object-ado-md.md) . Se produce un error cuando se hace referencia a esta propiedad desde objetos **member** que pertenecen a un objeto [LEVEL](./level-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de miembro (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [DrilledDown (propiedad, ADO MD)](./drilleddown-property-ado-md.md)