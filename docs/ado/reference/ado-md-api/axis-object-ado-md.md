---
description: Objeto Axis (ADO MD)
title: Objeto de eje (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Axis
helpviewer_keywords:
- Axis object [ADO MD]
ms.assetid: 5f498c9a-b1e7-4e6e-9ae6-71eadaf9aada
author: rothja
ms.author: jroth
ms.openlocfilehash: 591e3ea0a1fd9ebc3e6d8321da1af1638102749c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055780"
---
# <a name="axis-object-ado-md"></a>Objeto Axis (ADO MD)
Representa un eje posicional o de filtro de un objeto Cellset, que contiene los miembros seleccionados de una o más dimensiones.  
  
## <a name="remarks"></a>Observaciones  
 Un objeto de **eje** puede estar incluido en una colección de [ejes](./axes-collection-ado-md.md) o ser devuelto por la propiedad [FilterAxis](./filteraxis-property-ado-md.md) de un conjunto de [celdas](./cellset-object-ado-md.md).  
  
 Con las colecciones y las propiedades de un objeto de **eje** , puede hacer lo siguiente:  
  
-   Identifique el **eje** con la propiedad [Name](./name-property-ado-md.md) .  
  
-   Recorrer en iteración cada posición a lo largo de un **eje** mediante la colección [positions](./positions-collection-ado-md.md) .  
  
-   Obtenga el número de dimensiones del **eje** con la propiedad [DimensionCount](./dimensioncount-property-ado-md.md) .  
  
-   Obtener los atributos específicos del proveedor del **eje** con la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./axis-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de eje (VBScript)](./axis-example-vbscript.md)   
 [Colección axes (ADO MD)](./axes-collection-ado-md.md)   
 [Colección positions (ADO MD)](./positions-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)