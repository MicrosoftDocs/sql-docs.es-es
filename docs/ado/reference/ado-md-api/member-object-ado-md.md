---
description: Objeto de miembro (ADO MD)
title: Objeto miembro (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Member
helpviewer_keywords:
- Member object [ADO MD], members
ms.assetid: 3dedf755-0741-4c3f-8b4e-bff8ff8809c8
author: rothja
ms.author: jroth
ms.openlocfilehash: 36c5220d99d9f5bfb8bd8d711f93380e837111f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054520"
---
# <a name="member-object-ado-md"></a>Objeto de miembro (ADO MD)
Representa un miembro de un nivel de un cubo, los elementos secundarios de un miembro de un nivel o un miembro de una posición a lo largo de un eje de un Cellset.  
  
## <a name="remarks"></a>Observaciones  
 Las propiedades de un **miembro** difieren en función del contexto en el que se utiliza. Un **miembro** de un [nivel](./level-object-ado-md.md) de un [CubeDef](./cubedef-object-ado-md.md) tiene una propiedad [Children](./children-property-ado-md.md) que devuelve los **miembros** del siguiente nivel inferior de la jerarquía del **miembro** actual. Para un **miembro** de una [posición](./position-object-ado-md.md), la colección **Children** siempre está vacía. Además, la propiedad [Type](./type-property-ado-md.md) solo se aplica a **los miembros** de un **nivel**.  
  
 Un **miembro** de **Position** tiene dos propiedades que son útiles al mostrar el [Cellset](./cellset-object-ado-md.md): [DrilledDown](./drilleddown-property-ado-md.md) y [ParentSameAsPrev](./parentsameasprev-property-ado-md.md). Se producirá un error si se tiene acceso a estas propiedades en un **miembro** de un **nivel**.  
  
 Con las colecciones y las propiedades de un objeto de **miembro** de un **nivel**, puede hacer lo siguiente:  
  
-   Identifique el **miembro** con las propiedades [Name](./name-property-ado-md.md) y [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Devuelve una cadena que se va a usar al mostrar el **miembro** con la propiedad [Caption](./caption-property-ado-md.md) .  
  
-   Devuelva una cadena significativa que describa una medida o un **miembro** de fórmula con la propiedad [Description](./description-property-ado-md.md) .  
  
-   Determinar la naturaleza del **miembro** con la propiedad [Type](./type-property-ado-md.md) .  
  
-   Obtenga información sobre el **nivel** del **miembro** con las propiedades [LevelDepth](./leveldepth-property-ado-md.md) y [LevelName](./levelname-property-ado-md.md) .  
  
-   Obtener **miembros** relacionados en una [jerarquía](./hierarchy-object-ado-md.md) con las propiedades [Parent](./parent-property-ado-md.md) y [Children](./children-property-ado-md.md) .  
  
-   Cuente los elementos secundarios de un **miembro** con la propiedad [ChildCount](./childcount-property-ado-md.md) .  
  
-   Utilice la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar para obtener información adicional sobre el objeto de **nivel** .  
  
 Con las colecciones y las propiedades de un **miembro** de una **posición** a lo largo de un [eje](./axis-object-ado-md.md), puede hacer lo siguiente:  
  
-   Identifique el **miembro** con las propiedades [Name](./name-property-ado-md.md) y [UniqueName](./uniquename-property-ado-md.md) .  
  
-   Devuelve una cadena que se va a usar al mostrar el **miembro** con la propiedad [Caption](./caption-property-ado-md.md) .  
  
-   Devuelva una cadena significativa que describa una medida o un **miembro** de fórmula con la propiedad [Description](./description-property-ado-md.md) .  
  
-   Obtenga información sobre el **nivel** del **miembro** con las propiedades [LevelDepth](./leveldepth-property-ado-md.md) y [LevelName](./levelname-property-ado-md.md) .  
  
-   Cuente los elementos secundarios de un **miembro** con la propiedad [ChildCount](./childcount-property-ado-md.md) .  
  
-   Utilice la propiedad [DrilledDown](./drilleddown-property-ado-md.md) para determinar si existe al menos un elemento secundario en el **eje** que sigue inmediatamente a este **miembro**.  
  
-   Utilice la propiedad [ParentSameAsPrev](./parentsameasprev-property-ado-md.md) para determinar si el elemento primario de este **miembro** es igual que el elemento primario del **miembro** inmediatamente anterior.  
  
-   Utilice la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar para obtener información adicional sobre el objeto de **nivel** .  
  
 La colección **Properties** contiene propiedades proporcionadas por el proveedor. En la tabla siguiente se enumeran las propiedades que pueden estar disponibles. La lista de propiedades real puede variar en función de la implementación del proveedor. Consulte la documentación del proveedor para obtener una lista más completa de las propiedades disponibles.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|CatalogName|Nombre del catálogo al que pertenece este cubo.|  
|ChildrenCardinality|Número de elementos secundarios que tiene este miembro.|  
|CubeName|Nombre del cubo.|  
|Descripción|Una descripción significativa del miembro.|  
|DimensionUniqueName|Nombre no ambiguo de la [dimensión](./dimension-object-ado-md.md).|  
|HierarchyUniqueName|Nombre no ambiguo de la jerarquía.|  
|LevelNumber|Distancia entre el nivel y la raíz de la jerarquía.|  
|LevelUniqueName|Nombre no ambiguo del nivel.|  
|MemberCaption|Etiqueta o descripción asociada al miembro.|  
|MemberGUID|GUID del miembro.|  
|MemberName|Nombre del miembro.|  
|MemberOrdinal|Número ordinal del miembro.|  
|MemberType|Tipo del miembro.|  
|MemberUniqueName|Nombre no ambiguo del miembro.|  
|ParentCount|Recuento del número de elementos primarios que tiene este miembro.|  
|ParentLevel|Número de nivel del elemento primario del miembro.|  
|ParentUniqueName|Nombre no ambiguo del elemento primario del miembro.|  
|SchemaName|Nombre del esquema al que pertenece este cubo.|  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./member-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de catálogo (VB)](./catalog-example-vb.md)   
 [Colección de miembros (ADO MD)](./members-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)