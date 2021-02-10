---
description: Colección de tablas (ADOX)
title: Colección de tablas (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Catalog::Tables
- Tables
helpviewer_keywords:
- Tables collection [ADOX]
ms.assetid: 38d750e7-f3fb-426e-b4b4-55eea4f1a654
author: rothja
ms.author: jroth
ms.openlocfilehash: 5d95bec64e9751d8663c34d1bf9d9d301c163eb9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053476"
---
# <a name="tables-collection-adox"></a>Colección de tablas (ADOX)
Contiene todos los objetos de [tabla](./table-object-adox.md) de un catálogo.  
  
## <a name="remarks"></a>Observaciones  
 El método [Append](./append-method-adox-tables.md) de una colección **tables** es único para ADOX. Puede:  
  
-   Agregue una nueva tabla a la colección con el método **Append** .  
  
 Las propiedades y los métodos restantes son estándar para las colecciones de ADO. Puede:  
  
-   Obtener acceso a una tabla de la colección con la propiedad [Item](../ado-api/item-property-ado.md) .  
  
-   Devuelve el número de tablas contenidas en la colección con la propiedad [Count](../ado-api/count-property-ado.md) .  
  
-   Quite una tabla de la colección con el método [Delete](./delete-method-adox-collections.md) .  
  
-   Actualice los objetos de la colección para que reflejen el esquema de la base de datos actual con el método [Refresh](../ado-api/refresh-method-ado.md) .  
  
 Algunos proveedores pueden devolver otros objetos de esquema, como una vista, en la colección de **tablas** . Por lo tanto, algunas colecciones ADOX pueden contener varias referencias al mismo objeto. Si elimina el objeto de una colección, el cambio no será visible en otra colección que haga referencia al objeto eliminado hasta que se llame al método **Refresh** en la colección. Por ejemplo, con el proveedor de OLE DB para Microsoft Jet, las vistas se devuelven con la colección **tables** . Si quita una vista, debe actualizar la colección de **tablas** antes de que la colección refleje el cambio.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Eventos, métodos y propiedades de la colección de tablas](./tables-collection-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad ActiveConnection de catálogo (VB)](./catalog-activeconnection-property-example-vb.md)   
 [Ejemplo de propiedad Name, métodos Append de columnas y tablas (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Ejemplo de propiedad de tipo de tabla método de cierre de conexión (VB)](./connection-close-method-table-type-property-example-vb.md)   
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)   
 [Objeto Table (ADOX)](./table-object-adox.md)