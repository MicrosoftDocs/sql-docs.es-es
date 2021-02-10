---
description: Los elementos secundarios (propiedad, ADO MD)
title: Propiedad Children (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member::Children
- Children
helpviewer_keywords:
- Children property [ADO MD]
ms.assetid: 61d36468-1ccd-467a-9cb5-17d0bfacc766
author: rothja
ms.author: jroth
ms.openlocfilehash: 7fae1b50b219abc4e5841bb537d0befaf35a4b3e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055680"
---
# <a name="children-property-ado-md"></a>Los elementos secundarios (propiedad, ADO MD)
Devuelve una colección de [miembros](./members-collection-ado-md.md) para la que el [miembro](./member-object-ado-md.md) actual es el elemento primario de la jerarquía.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve una colección de **miembros** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 La propiedad **Children** contiene una colección de **miembros** para la que el **miembro** actual es el elemento primario jerárquico. Los objetos **miembro** de nivel hoja no tienen miembros secundarios en la colección de **miembros** . Esta propiedad solo se admite en los objetos **member** que pertenecen a un objeto [LEVEL](./level-object-ado-md.md) . Se produce un error cuando se hace referencia a esta propiedad desde objetos **member** que pertenecen a un objeto [Position](./position-object-ado-md.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de miembro (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Propiedad ChildCount (ADO MD)](./childcount-property-ado-md.md)