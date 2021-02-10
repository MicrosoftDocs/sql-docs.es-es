---
description: Colección de claves (ADOX)
title: Colección de claves (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Table::Keys
- Keys
helpviewer_keywords:
- Keys collection [ADOX]
ms.assetid: cdb31c76-e559-475c-b33a-aac24f73e70e
author: rothja
ms.author: jroth
ms.openlocfilehash: 08bb9e7bb82484080f969bce5e817a1d21724809
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054040"
---
# <a name="keys-collection-adox"></a>Colección de claves (ADOX)
Contiene todos los objetos de [clave](./key-object-adox.md) de una [tabla](./table-object-adox.md).  
  
## <a name="remarks"></a>Observaciones  
 El método [Append](./append-method-adox-keys.md) de una [colección Keys]() es único para ADOX. Puede:  
  
-   Agregue una nueva clave a la colección con el método [Append](./append-method-adox-keys.md) .  
  
 Las propiedades y los métodos restantes son estándar para las colecciones de ADO. Puede:  
  
-   Obtener acceso a una clave en la colección con la propiedad [Item](../ado-api/item-property-ado.md) .  
  
-   Devuelve el número de claves contenidas en la colección con la propiedad [Count](../ado-api/count-property-ado.md) .  
  
-   Quite una clave de la colección con el método [Delete](./delete-method-adox-collections.md) .  
  
-   Actualice los objetos de la colección para que reflejen el esquema de la base de datos actual con el método [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de la colección de índices](./indexes-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Propiedades, métodos y eventos de la colección Keys](./keys-collection-properties-methods-and-events.md)   
 [Objeto Key (ADOX)](./key-object-adox.md)