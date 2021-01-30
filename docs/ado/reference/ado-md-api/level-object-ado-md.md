---
description: Objeto Level (ADO MD)
title: Objeto LEVEL (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Level
helpviewer_keywords:
- Level object [ADO MD]
ms.assetid: 37815869-ed30-45fd-9aea-0a986c1b305c
author: rothja
ms.author: jroth
ms.openlocfilehash: ebbebb2150cbe9c03e51ac296c39c362a6670e1d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169804"
---
# <a name="level-object-ado-md"></a>Objeto Level (ADO MD)
Contiene un conjunto de miembros, cada uno de los cuales tiene el mismo rango dentro de una jerarquía.  
  
## <a name="remarks"></a>Observaciones  
 Con las colecciones y las propiedades de un objeto de **nivel** , puede hacer lo siguiente:  
  
-   Identifique el **nivel** con las propiedades [Name](./name-property-ado-md.md) y [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Devuelve una cadena que se va a usar al mostrar el **nivel** con la propiedad [Caption](./caption-property-ado-md.md) .  
  
-   Devuelva una cadena significativa que describa el **nivel** con la propiedad [Description](./description-property-ado-md.md) .  
  
-   Devuelve los objetos [miembro](./member-object-ado-md.md) que componen el **nivel** con la colección de [miembros](./members-collection-ado-md.md) .  
  
-   Devuelve el número de niveles de la raíz del **nivel** con la propiedad [Depth](./depth-property-ado-md.md) .  
  
-   Utilice la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar para obtener información adicional sobre el objeto de **nivel** .  
  
 La colección **Properties** contiene propiedades proporcionadas por el proveedor. En la tabla siguiente se enumeran las propiedades que pueden estar disponibles. La lista de propiedades real puede variar en función de la implementación del proveedor. Consulte la documentación del proveedor para obtener una lista más completa de las propiedades disponibles.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|CatalogName|Nombre del catálogo al que pertenece este cubo.|  
|CubeName|Nombre del cubo.|  
|Descripción|Una descripción significativa del nivel.|  
|DimensionUniqueName|Nombre no ambiguo de la [dimensión](./dimension-object-ado-md.md).|  
|HierarchyUniqueName|Nombre no ambiguo de la jerarquía.|  
|LevelCaption|Etiqueta o título asociado al nivel.|  
|LevelCardinality|Número de miembros del nivel.|  
|LevelGUID|GUID del nivel.|  
|LevelName|Nombre del nivel.|  
|LevelNumber|Distancia entre el nivel y la raíz de la jerarquía.|  
|LevelType|Tipo de nivel.|  
|LevelUniqueName|Nombre no ambiguo del nivel.|  
|SchemaName|Nombre del esquema al que pertenece este cubo.|  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./level-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Objeto Hierarchy (ADO MD)](./hierarchy-object-ado-md.md)   
 [Colección de niveles (ADO MD)](./levels-collection-ado-md.md)   
 [Colección de miembros (ADO MD)](./members-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)