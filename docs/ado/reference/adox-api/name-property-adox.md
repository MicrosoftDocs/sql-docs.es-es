---
description: Name (propiedad, ADOX)
title: Name (propiedad, ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Table::PutName
- _Table::GetName
- _Key::Name
- _Key::get_Name
- _Column::GetName
- _Index::Name
- _Index::put_Name
- _Column::PutName
- _Key::put_Name
- _Table::put_Name
- _User25::PutName
- _Index::get_Name
- _Column::get_Name
- _Group25::Name
- _Group25::get_Name
- _Column::Name
- _User25::get_Name
- _Table::Name
- _Group25::GetName
- _Index::PutName
- _Column::put_Name
- _Key::GetName
- _Table::get_Name
- _User25::Name
- _User25::put_Name
- _Index::GetName
- _User25::GetName
helpviewer_keywords:
- Name property [ADOX]
ms.assetid: 81b92baf-b6b9-4f4e-9f33-4503795518cd
author: rothja
ms.author: jroth
ms.openlocfilehash: 51373d1eee3613366c39bd3d18a9f479a0d046c6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053999"
---
# <a name="name-property-adox"></a>Name (propiedad, ADOX)
Indica el nombre del objeto.  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece o devuelve un valor de **cadena** .  
  
## <a name="remarks"></a>Observaciones  
 Los nombres no tienen que ser únicos dentro de una colección.  
  
 La propiedad **Name** es de lectura y escritura en objetos de [columna](./column-object-adox.md), [grupo](./group-object-adox.md), [clave](./key-object-adox.md), [Índice](./index-object-adox.md), [tabla](./table-object-adox.md)y [usuario](./user-object-adox.md) . La propiedad **Name** es de solo lectura en los objetos de [Catálogo](./catalog-object-adox.md), [procedimiento](./procedure-object-adox.md)y [vista](./view-object-adox.md) .  
  
 En el caso de objetos de lectura/escritura (objetos de **columnas**, **grupos**, **claves**, **índices**, **tablas** y **usuarios** ), el valor predeterminado es una cadena vacía ("").  
  
> [!NOTE]
>  En el caso de las claves, esta propiedad es de solo lectura en objetos de **clave** ya anexados a una colección. En el caso de las tablas, esta propiedad es de solo lectura para los objetos de **tabla** ya anexados a una colección.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Column (ADOX)](./column-object-adox.md)  
        [Objeto Group (ADOX)](./group-object-adox.md)  
        [Objeto Index (ADOX)](./index-object-adox.md)  
    :::column-end:::
    :::column:::
        [Objeto Key (ADOX)](./key-object-adox.md)  
        [Objeto Procedure (ADOX)](./procedure-object-adox.md)  
        [Objeto Property (ADO)](../ado-api/property-object-ado.md)  
    :::column-end:::
    :::column:::
        [Objeto Table (ADOX)](./table-object-adox.md)  
        [Objeto User (ADOX)](./user-object-adox.md)  
        [Objeto View (ADOX)](./view-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad Name, métodos Append de columnas y tablas (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Ejemplo de propiedad ParentCatalog (VB)](./parentcatalog-property-example-vb.md)