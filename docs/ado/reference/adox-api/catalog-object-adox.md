---
description: Objeto Catalog (ADOX)
title: Objeto Catalog (ADOX) | Microsoft Docs
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
- Catalog object [ADOX]
ms.assetid: bb651639-a488-4e38-b6de-0ed99fa4dd92
author: rothja
ms.author: jroth
ms.openlocfilehash: cfb44f5e35b1f30e504cfbf83a85c5a61aceb987
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164296"
---
# <a name="catalog-object-adox"></a>Objeto Catalog (ADOX)
Contiene colecciones ([tablas](./tables-collection-adox.md), [vistas](./views-collection-adox.md), [usuarios](./users-collection-adox.md), [grupos](./groups-collection-adox.md)y [procedimientos](./procedures-collection-adox.md)) que describen el catálogo de esquemas de un origen de datos.  
  
## <a name="remarks"></a>Observaciones  
 Puede modificar el objeto de **Catálogo** agregando o quitando objetos, o modificando los objetos existentes. Es posible que algunos proveedores no admitan todos los objetos de **Catálogo** o que solo admitan ver la información de esquema.  
  
 Con las propiedades y los métodos de un objeto de **Catálogo** , puede:  
  
-   Abra el catálogo estableciendo la propiedad [ActiveConnection](./activeconnection-property-adox.md) en un objeto de [conexión](../ado-api/connection-object-ado.md) ADO o en una cadena de conexión válida.  
  
-   Cree un nuevo catálogo con el método [Create](./create-method-adox.md) .  
  
-   Determine los propietarios de los objetos de un **Catálogo** con los métodos [GetObjectOwner](./getobjectowner-method-adox.md) y [SetObjectOwner](./setobjectowner-method.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Catalog](./catalog-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad ActiveConnection de catálogo (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Ejemplo de propiedades Command y CommandText (VB)](./command-and-commandtext-properties-example-vb.md)   
 [Ejemplo de propiedad de tipo de tabla método de cierre de conexión (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Ejemplo del método Create (VB)](./create-method-example-vb.md)   
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Ejemplo de propiedad de comando, colección de parámetros (VB)](./parameters-collection-command-property-example-vb.md)   
 [Ejemplo de propiedad ParentCatalog (VB)](./parentcatalog-property-example-vb.md)   
 [Ejemplo de método Append de procedimientos (VB)](./procedures-append-method-example-vb.md)   
 [Ejemplo de método Delete de procedimientos (VB)](./procedures-delete-method-example-vb.md)   
 [Ejemplo de método de actualización de procedimientos (VB)](./procedures-refresh-method-example-vb.md)   
 [Ejemplo de colecciones y campos de vistas (VB)](./views-and-fields-collections-example-vb.md)   
 [Ejemplo del método Append de vistas (VB)](./views-append-method-example-vb.md)   
 [Colección views, ejemplo de propiedad CommandText (VB)](./views-collection-commandtext-property-example-vb.md)   
 [Ejemplo de método Delete de vistas (VB)](./views-delete-method-example-vb.md)   
 [Ejemplo del método de actualización de vistas (VB)](./views-refresh-method-example-vb.md)   
 [Colección de grupos (ADOX)](./groups-collection-adox.md)   
 [Colección Procedures (ADOX)](./procedures-collection-adox.md)   
 [Colección de tablas (ADOX)](./tables-collection-adox.md)   
 [Colección de usuarios (ADOX)](./users-collection-adox.md)   
 [Colección de vistas (ADOX)](./views-collection-adox.md)