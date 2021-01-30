---
description: Objeto Column (ADOX)
title: Objeto column (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Column
helpviewer_keywords:
- Column object [ADOX]
ms.assetid: 6e772783-1bc8-4ea7-94b2-7d7a52ea5c47
author: rothja
ms.author: jroth
ms.openlocfilehash: 7501381174ddf522b60a596cd5846f012d59d88e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164255"
---
# <a name="column-object-adox"></a>Objeto Column (ADOX)
Representa una columna de una tabla, un índice o una clave.  
  
## <a name="remarks"></a>Observaciones  
 En el código siguiente se crea una nueva **columna**:  
  
 `Dim obj As New Column`  
  
 Con las propiedades y las colecciones de un objeto de **columna** , puede:  
  
-   Identifique la columna con la propiedad [Name (Adox)](./name-property-adox.md) .  
  
-   Especifique el tipo de datos de la columna con la propiedad [Type Property (Key) (Adox)](./type-property-key-adox.md) .  
  
-   Determine si la columna tiene una longitud fija o si puede contener valores NULL con la propiedad [attributes (Adox)](./attributes-property-adox.md) .  
  
-   Especifique el tamaño máximo de la columna con la propiedad [DefinedSize (propiedad)](./definedsize-property-adox.md) .  
  
-   Para los valores de datos numéricos, especifique la escala con la propiedad [NumericScale Property (Adox)](./numericscale-property-adox.md) .  
  
-   En valor de datos numéricos, especifique la precisión máxima con la propiedad [precisión (Adox)](./precision-property-adox.md) .  
  
-   Especifique el [objeto de catálogo (Adox)](./catalog-object-adox.md) que posee la columna con la propiedad [PARENTCATALOG Property (Adox)](./parentcatalog-property-adox.md) .  
  
-   En el caso de las columnas de clave, especifique el nombre de la columna relacionada en la tabla relacionada con la propiedad [RelatedColumn Property (Adox)](./relatedcolumn-property-adox.md) .  
  
-   Para las columnas de índice, especifique si el criterio de ordenación es ascendente o descendente con la propiedad [SortOrder (propiedad SortOrder)](./sortorder-property-adox.md) .  
  
-   Acceder a las propiedades específicas del proveedor con la colección de la [colección de propiedades (ADO)](../ado-api/properties-collection-ado.md) .  
  
> [!NOTE]
>  No todas las propiedades de los objetos de **columna** pueden ser compatibles con el proveedor de datos. Se producirá un error si se ha establecido un valor para una propiedad que el proveedor no admite. En el caso de los objetos de **columna** nuevos, el error se producirá cuando el objeto se anexe a la colección. En el caso de los objetos existentes, el error se producirá al establecer la propiedad.  
>   
>  Al crear objetos de **columna** , la existencia de un valor predeterminado adecuado para una propiedad opcional no garantiza que el proveedor admita la propiedad. Para obtener más información sobre las propiedades que admite el proveedor, consulte la documentación del proveedor.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Column](./column-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad Name, métodos Append de columnas y tablas (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Ejemplo de propiedad de tipo de tabla método de cierre de conexión (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Ejemplo de código ADOX: ejemplo de propiedades NumericScale y Precision (VB)](./adox-code-example-numericscale-and-precision-properties-example-vb.md)   
 [Ejemplo de propiedad ParentCatalog (VB)](./parentcatalog-property-example-vb.md)   
 [Ejemplo de la propiedad SortOrder (VB)](./sortorder-property-example-vb.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Colección de propiedades (ADO)](../ado-api/properties-collection-ado.md)