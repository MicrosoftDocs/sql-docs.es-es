---
description: Objeto Catalog (ADO MD)
title: Objeto Catalog (ADO MD) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog
helpviewer_keywords:
- Catalog object [ADO MD]
ms.assetid: 11f6f896-d69c-44a4-94cd-d54c93140e4a
author: rothja
ms.author: jroth
ms.openlocfilehash: 73ae5da9b73093251dcf2355643e038264182b90
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100055740"
---
# <a name="catalog-object-ado-md"></a>Objeto Catalog (ADO MD)
Contiene información de esquema multidimensional (es decir, cubos y dimensiones subyacentes, jerarquías, niveles y miembros) específicas de un proveedor de datos multidimensionales (MDP).  
  
## <a name="remarks"></a>Observaciones  
 Con las colecciones y las propiedades de un objeto de **Catálogo** , puede hacer lo siguiente:  
  
-   Abra el catálogo estableciendo la propiedad [ActiveConnection](./activeconnection-property-ado-md.md) en un objeto de [conexión](../ado-api/connection-object-ado.md) ADO estándar o en una cadena de conexión válida.  
  
-   Identifique el **Catálogo** con la propiedad [Name](./name-property-ado-md.md) .  
  
-   Recorrer en iteración los cubos de un catálogo mediante la colección [CubeDefs](./cubedefs-collection-ado-md.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos](./catalog-object-properties-methods-and-events-ado-md.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de catálogo (VB)](./catalog-example-vb.md)   
 [Connection (objeto) (ADO)](../ado-api/connection-object-ado.md)   
 [Colección CubeDefs (ADO MD)](./cubedefs-collection-ado-md.md)