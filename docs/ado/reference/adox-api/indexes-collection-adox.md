---
description: Colección de índices (ADOX)
title: Colección Indexes (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table::Indexes
- Indexes
helpviewer_keywords:
- Indexes collection [ADOX]
ms.assetid: 184cf536-455c-42be-bf1c-a5c25bade961
author: rothja
ms.author: jroth
ms.openlocfilehash: 74147f1168e9bf9789c0ab1111daa27536dfd64e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172015"
---
# <a name="indexes-collection-adox"></a>Colección de índices (ADOX)
Contiene todos los objetos de [Índice](./index-object-adox.md) de una tabla.  
  
## <a name="remarks"></a>Observaciones  
 El método [Append](./append-method-adox-indexes.md) de una colección **Indexes** es único para ADOX. Puede:  
  
-   Agregue un nuevo índice a la colección con el método **Append** .  
  
 Las propiedades y los métodos restantes son estándar para las colecciones de ADO. Puede:  
  
-   Obtener acceso a un índice de la colección con la propiedad [Item](../ado-api/item-property-ado.md) .  
  
-   Devuelve el número de índices contenidos en la colección con la propiedad [Count](../ado-api/count-property-ado.md) .  
  
-   Quitar un índice de la colección con el método [Delete](./delete-method-adox-collections.md) .  
  
-   Actualice los objetos de la colección para que reflejen el esquema de la base de datos actual con el método [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de la colección de índices](./indexes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de método Append de índices (VB)](./indexes-append-method-example-vb.md)   
 [Objeto Index (ADOX)](./index-object-adox.md)