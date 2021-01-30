---
description: Propiedad primaria (ADO MD)
title: Propiedad Parent (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parent
- Member::Parent
helpviewer_keywords:
- Parent property [ADO MD]
ms.assetid: 32c278c1-d8e1-4bb7-9ecd-2fbfdffee34b
author: rothja
ms.author: jroth
ms.openlocfilehash: fd5b6c1adafca65b48f79eb5debc61ea45381705
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164452"
---
# <a name="parent-property-ado-md"></a>Propiedad primaria (ADO MD)
Indica el miembro que es primario del [miembro](./member-object-ado-md.md) actual de una jerarquía.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un objeto de [miembro](./member-object-ado-md.md) y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 Un miembro que está en el nivel superior de una jerarquía (la raíz) no tiene ningún elemento primario. Esta propiedad solo se admite en los objetos **member** que pertenecen a un objeto [LEVEL](./level-object-ado-md.md) . Se produce un error cuando se hace referencia a esta propiedad desde objetos **member** que pertenecen a un objeto [Position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de miembro (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Los elementos secundarios (propiedad, ADO MD)](./children-property-ado-md.md)