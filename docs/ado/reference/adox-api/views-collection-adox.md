---
description: Colección de vistas (ADOX)
title: Colección views (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Views
- Views
helpviewer_keywords:
- Views collection [ADOX]
ms.assetid: a55d380c-2b7b-4b57-af74-8ba0b3de0db9
author: rothja
ms.author: jroth
ms.openlocfilehash: 51a32bb1952e5c8100ed1d13cb7ba72b99e8994d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169102"
---
# <a name="views-collection-adox"></a>Colección de vistas (ADOX)
Contiene todos los objetos de [vista](./view-object-adox.md) de un catálogo.  
  
## <a name="remarks"></a>Observaciones  
 El método [Append](./append-method-adox-views.md) de una colección **views** es único para ADOX. Puede:  
  
-   Agregue una nueva vista a la colección con el método **Append** .  
  
 Las propiedades y los métodos restantes son estándar para las colecciones de ADO. Puede:  
  
-   Obtener acceso a una vista de la colección con la propiedad [Item](../ado-api/item-property-ado.md) .  
  
-   Devuelve el número de vistas contenidas en la colección con la propiedad [Count](../ado-api/count-property-ado.md) .  
  
-   Quitar una vista de la colección con el método [Delete](./delete-method-adox-collections.md) .  
  
-   Actualice los objetos de la colección para que reflejen el esquema de la base de datos actual con el método [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de la colección de vistas](./views-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de colecciones y campos de vistas (VB)](./views-and-fields-collections-example-vb.md)   
 [Ejemplo del método Append de vistas (VB)](./views-append-method-example-vb.md)   
 [Colección views, ejemplo de propiedad CommandText (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Ejemplo de método Delete de vistas (VB)](./views-delete-method-example-vb.md)   
 [Ejemplo del método de actualización de vistas (VB)](./views-refresh-method-example-vb.md)   
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Objeto View (ADOX)](./view-object-adox.md)