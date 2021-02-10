---
description: Objeto Hierarchy (ADO MD)
title: Objeto Hierarchy (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Hierarchy
helpviewer_keywords:
- Hierarchy object [ADO MD]
ms.assetid: 034af340-ac79-494e-ba5e-2b57da1cb9de
author: rothja
ms.author: jroth
ms.openlocfilehash: 4f84fe2157e9da38ad55a56334436a5d54128823
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051156"
---
# <a name="hierarchy-object-ado-md"></a>Objeto Hierarchy (ADO MD)
Representa una manera en la que los miembros de una [dimensión](./dimension-object-ado-md.md) se pueden agregar o "acumular". Una dimensión se puede Agregar A una o más jerarquías.  
  
## <a name="remarks"></a>Observaciones  
 Con las colecciones y las propiedades de un objeto **Hierarchy** , puede hacer lo siguiente:  
  
-   Identifique la **jerarquía** con las propiedades [Name](./name-property-ado-md.md) y [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Devuelve una cadena significativa que describe la **jerarquía** con la propiedad [Description](./description-property-ado-md.md) .  
  
-   Devuelve los objetos de [nivel](./level-object-ado-md.md) que componen la **jerarquía** con la colección [Levels](./levels-collection-ado-md.md) .  
  
-   Utilice la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar para obtener información adicional sobre el objeto de **jerarquía** .  
  
 La colección **Properties** contiene propiedades proporcionadas por el proveedor. En la tabla siguiente se enumeran las propiedades que pueden estar disponibles. La lista de propiedades real puede variar en función de la implementación del proveedor. Consulte la documentación del proveedor para obtener una lista más completa de las propiedades disponibles.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|AllMember|Miembro en el nivel más alto de acumulación en la jerarquía.|  
|CatalogName|Nombre del catálogo al que pertenece este cubo.|  
|CubeName|Nombre del cubo.|  
|DefaultMember|Nombre único del miembro predeterminado para esta jerarquía.|  
|Descripción|Una descripción significativa de la jerarquía.|  
|DimensionType|Tipo de dimensión a la que pertenece esta jerarquía.|  
|DimensionUniqueName|Nombre no ambiguo de la dimensión.|  
|HierarchyCaption|Etiqueta o título asociado a la jerarquía.|  
|HierarchyCardinality|Número de miembros de la jerarquía.|  
|HierarchyGUID|GUID de la jerarquía.|  
|HierarchyName|Nombre de la jerarquía.|  
|HierarchyUniqueName|Nombre no ambiguo de la jerarquía.|  
|SchemaName|Nombre del esquema al que pertenece este cubo.|  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./hierarchy-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Objeto Dimension (ADO MD)](./dimension-object-ado-md.md)   
 [Colección Hierarchies (ADO MD)](./hierarchies-collection-ado-md.md)   
 [Colección de niveles (ADO MD)](./levels-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)