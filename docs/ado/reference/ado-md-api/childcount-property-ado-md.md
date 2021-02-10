---
description: Propiedad ChildCount (ADO MD)
title: Propiedad ChildCount (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ChildCount
- Member::ChildCount
helpviewer_keywords:
- ChildCount property [ADO MD]
ms.assetid: 5463be22-ca50-43ea-9c92-468fc8eda280
author: rothja
ms.author: jroth
ms.openlocfilehash: 5515aaf6d3080057a725d40b1bd6cbcbe4c0c945
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055710"
---
# <a name="childcount-property-ado-md"></a>Propiedad ChildCount (ADO MD)
Indica el número de miembros para los que el objeto [miembro](./member-object-ado-md.md) actual es el elemento primario de una jerarquía.  
  
## <a name="return-values"></a>Valores devueltos  
 Devuelve un entero **largo** y es de solo lectura.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **ChildCount** para devolver una estimación del número de elementos secundarios que tiene un **miembro** . Los elementos secundarios reales de un **miembro** pueden ser devueltos por la propiedad [Children](./children-property-ado-md.md) .  
  
 Para los objetos **miembro** de un objeto [Position](./position-object-ado-md.md) , el número máximo devuelto es 65536. Si el número real de elementos secundarios supera 65536, el valor devuelto seguirá siendo 65536. Por lo tanto, la aplicación debe interpretar un valor de **ChildCount** de 65536 como igual o superior a 65536 elementos secundarios.  
  
 Para los objetos de **miembro** de un objeto de [nivel](./level-object-ado-md.md) , use la propiedad [recuento](../ado-api/count-property-ado.md) de colecciones de ADO en la colección de **elementos secundarios** para determinar el número exacto de elementos secundarios. La determinación del número exacto de elementos secundarios puede ser lenta si el número de elementos secundarios de la colección es grande.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto de miembro (ADO MD)](./member-object-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Propiedad Children (ADO MD)](./children-property-ado-md.md)   
 [Propiedad Count (ADO)](../ado-api/count-property-ado.md)   
 [Colección Members (ADO MD)](./members-collection-ado-md.md)