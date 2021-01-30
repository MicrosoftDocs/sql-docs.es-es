---
description: Objeto Index (ADOX)
title: Objeto index (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Index
helpviewer_keywords:
- Index object [ADOX]
ms.assetid: 6b9578c0-bc94-46b9-b801-c18e14b04b31
author: rothja
ms.author: jroth
ms.openlocfilehash: a11389324337d4e5a9deea347dda8ecd5cea3016
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172038"
---
# <a name="index-object-adox"></a>Objeto Index (ADOX)
Representa un índice de una tabla de base de datos.  
  
## <a name="remarks"></a>Observaciones  
 En el código siguiente se crea un nuevo **Índice**:  
  
```  
Dim obj As New Index  
```  
  
 Con las propiedades y las colecciones de un objeto **index** , puede:  
  
-   Identifique el índice con la propiedad [Name](./name-property-adox.md) .  
  
-   Obtener acceso a las columnas de base de datos del índice con la colección de [columnas](./columns-collection-adox.md) .  
  
-   Especifique si las claves de índice deben ser únicas con la propiedad [Unique](./unique-property-adox.md) .  
  
-   Especifique si el índice es la clave principal de una tabla con la propiedad [PrimaryKey](./primarykey-property-adox.md) .  
  
-   Especifique si los registros que tienen valores NULL en sus campos de índice tienen entradas de índice con la propiedad [IndexNulls](./indexnulls-property-adox.md) .  
  
-   Especifique si el índice está agrupado con la propiedad [Clustered](./clustered-property-adox.md) .  
  
-   Obtener acceso a las propiedades de índice específicas del proveedor con la colección [Properties](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  Se producirá un error al anexar una [columna](./column-object-adox.md) a la colección de **columnas** de un **Índice** si la **columna** no existe en un objeto de [tabla](./table-object-adox.md) ya anexado a la colección de [tablas](./tables-collection-adox.md) .  
  
> [!NOTE]
>  Es posible que el proveedor de datos no admita todas las propiedades de los objetos de **Índice** . Se producirá un error si se ha establecido un valor para una propiedad que no es compatible con el proveedor. En el caso de los nuevos objetos **index** , el error se producirá cuando el objeto se anexe a la colección. En el caso de los objetos existentes, el error se producirá al establecer la propiedad.  
  
> [!NOTE]
>  Al crear objetos de **Índice** , la existencia de un valor predeterminado adecuado para una propiedad opcional no garantiza que el proveedor admita la propiedad. Para obtener más información sobre las propiedades que admite el proveedor, consulte la documentación del proveedor.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Index](./index-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de método Append de índices (VB)](./indexes-append-method-example-vb.md)   
 [Ejemplo de propiedad IndexNulls (VB)](./indexnulls-property-example-vb.md)   
 [Ejemplo de propiedades PrimaryKey y Unique (VB)](./primarykey-and-unique-properties-example-vb.md)   
 [Ejemplo de la propiedad SortOrder (VB)](./sortorder-property-example-vb.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Colección Indexes (ADOX)](./indexes-collection-adox.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)