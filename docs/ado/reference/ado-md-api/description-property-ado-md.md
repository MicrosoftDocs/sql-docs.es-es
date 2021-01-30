---
description: Description (propiedad) (ADO MD)
title: Propiedad Description (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Description
- Level::Description
- CubeDef::Description
- Hierarchy::Description
- Description
- Dimension::Description
helpviewer_keywords:
- Description property [ADO MD]
ms.assetid: 6d626d35-0bf3-4f24-9934-ad9c9c91273a
author: rothja
ms.author: jroth
ms.openlocfilehash: 7339bcafe8df6678ec5cd9a0bef43dd92dd6edbd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169888"
---
# <a name="description-property-ado-md"></a>Description (propiedad) (ADO MD)
Devuelve una explicación de texto del objeto actual.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve una **cadena** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 En el caso de los objetos [miembro](./member-object-ado-md.md) , la **Descripción** solo se aplica a los miembros Measure y formula. **Description** devuelve una cadena vacía ("") para todos los demás tipos de miembros. Para obtener más información sobre los distintos tipos de miembros, vea la propiedad [Type](./type-property-ado-md.md) .  
  
 Esta propiedad solo se admite en los objetos **member** que pertenezcan a un objeto [LEVEL](./level-object-ado-md.md) . Se produce un error cuando se hace referencia a esta propiedad desde objetos **member** que pertenecen a un objeto [Position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto CubeDef (ADO MD)](./cubedef-object-ado-md.md)  
        [Objeto de dimensión (ADO MD)](./dimension-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Objeto Hierarchy (ADO MD)](./hierarchy-object-ado-md.md)  
        [Objeto Level (ADO MD)](./level-object-ado-md.md)  
    :::column-end:::
    :::column:::
        [Objeto de miembro (ADO MD)](./member-object-ado-md.md)  
    :::column-end:::
:::row-end:::