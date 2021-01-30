---
description: Objeto CubeDef (ADO MD)
title: Objeto CubeDef (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- CubeDef
helpviewer_keywords:
- CubeDef object [ADO MD]
ms.assetid: feb2581c-fc41-471c-bb69-29f8a55fda70
author: rothja
ms.author: jroth
ms.openlocfilehash: bae255b0de860a1caee016d5e8867c7fb9b3ffee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174318"
---
# <a name="cubedef-object-ado-md"></a>Objeto CubeDef (ADO MD)
Representa un cubo de un esquema multidimensional que contiene un conjunto de dimensiones relacionadas.  
  
## <a name="remarks"></a>Observaciones  
 Con las colecciones y las propiedades de un objeto **CubeDef** , puede hacer lo siguiente:  
  
-   Identifique un **CubeDef** con la propiedad [Name](./name-property-ado-md.md) .  
  
-   Devuelve una cadena que describe el cubo con la propiedad [Description](./description-property-ado-md.md) .  
  
-   Devuelve las dimensiones que componen el cubo con la colección de [dimensiones](./dimensions-collection-ado-md.md) .  
  
-   Obtenga información adicional sobre el **CubeDef** con la colección de [propiedades](../ado-api/properties-collection-ado.md) de ADO estándar.  
  
 La colección **Properties** contiene propiedades proporcionadas por el proveedor. En la tabla siguiente se enumeran las propiedades que pueden estar disponibles. La lista de propiedades real puede variar en función de la implementación del proveedor. Consulte la documentación del proveedor para obtener una lista más completa de las propiedades disponibles.  
  
|Nombre|Descripción|  
|----------|-----------------|  
|CatalogName|Nombre del catálogo al que pertenece este cubo.|  
|CreatedOn|Fecha y hora de creación del cubo.|  
|CubeGUID|GUID del cubo.|  
|CubeName|Nombre del cubo.|  
|CubeType|El tipo de cubo.|  
|DataUpdatedBy|IDENTIFICADOR de usuario de la persona que realiza la última actualización de datos.|  
|Descripción|Una descripción significativa del cubo.|  
|LastSchemaUpdate|Fecha y hora de la última actualización del esquema.|  
|SchemaName|Nombre del esquema al que pertenece este cubo.|  
|SchemaUpdatedBy|IDENTIFICADOR de usuario de la persona que realiza la última actualización del esquema.|  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./cubedef-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de CubeDef (VBScript)](./cubedef-example-vbscript.md)   
 [Objeto Catalog (ADO MD)](./catalog-object-ado-md.md)   
 [Colección CubeDefs (ADO MD)](./cubedefs-collection-ado-md.md)   
 [Colección de dimensiones (ADO MD)](./dimensions-collection-ado-md.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)