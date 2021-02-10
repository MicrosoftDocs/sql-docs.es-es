---
description: Propiedad Item (ADO)
title: Item (propiedad) (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Parameters::GetItem
- Indexes::GetItem
- Parameters::Item
- Tables::Item
- Procedures::Item
- Users::GetItem
- Tables::GetItem
- Procedures::GetItem
- Users::get_Item
- Users::Item
- Views::GetItem
- Groups::Item
- Groups::get_Item
- Columns::Item
- Indexes::Item
- Fields15::GetItem
- Columns::GetItem
- Fields::Item
- Indexes::get_Item
- Columns::get_Item
- Fields15::Item
- Views::get_Item
- Groups::GetItem
- Errors::get_Item
- Fields15::get_Item
- Tables::get_Item
- Views::Item
- Errors::GetItem
- Parameters::get_Item
- Errors::Item
- Procedures::get_Item
helpviewer_keywords:
- Item property [ADO]
ms.assetid: e11484bb-c5c7-42d8-9bb8-21572125d727
author: rothja
ms.author: jroth
ms.openlocfilehash: 3e87e7ae8592afc67b049fb25c4e66392f45eb89
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041905"
---
# <a name="item-property-ado"></a>Propiedad Item (ADO)
Indica un miembro específico de una colección, por nombre o número ordinal.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
Set object = collection.Item ( Index )  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve una referencia de objeto.  
  
## <a name="parameters"></a>Parámetros  
 *Index*  
 Expresión **Variant** que se evalúa como el nombre o el número ordinal de un objeto de una colección.  
  
## <a name="remarks"></a>Observaciones  
 Use la propiedad **Item** para devolver un objeto específico de una colección. Si el **elemento** no encuentra un objeto en la colección correspondiente al argumento de *Índice* , se produce un error. Además, algunas colecciones no admiten objetos con nombre. para estas colecciones, debe utilizar referencias de número ordinal.  
  
 La propiedad **Item** es la propiedad predeterminada para todas las colecciones; por lo tanto, los siguientes formatos de sintaxis son intercambiables:  
  
```  
collection.Item (Index)  
collection (Index)  
```  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Colección Axes (ADO MD)](../ado-md-api/axes-collection-ado-md.md)  
        [Colección de columnas (ADOX)](../adox-api/columns-collection-adox.md)  
        [Colección CubeDefs (ADO MD)](../ado-md-api/cubedefs-collection-ado-md.md)  
        [Colección Dimensions (ADO MD)](../ado-md-api/dimensions-collection-ado-md.md)  
        [Colección de errores (ADO)](./errors-collection-ado.md)  
        [Fields (colección) (ADO)](./fields-collection-ado.md)  
        [Colección de grupos (ADOX)](../adox-api/groups-collection-adox.md)  
    :::column-end:::
    :::column:::
        [Colección Hierarchies (ADO MD)](../ado-md-api/hierarchies-collection-ado-md.md)  
        [Colección de índices (ADOX)](../adox-api/indexes-collection-adox.md)  
        [Colección de claves (ADOX)](../adox-api/keys-collection-adox.md)  
        [Colección de niveles (ADO MD)](../ado-md-api/levels-collection-ado-md.md)  
        [Colección Members (ADO MD)](../ado-md-api/members-collection-ado-md.md)  
        [Colección de parámetros (ADO)](./parameters-collection-ado.md)  
    :::column-end:::
    :::column:::
        [Colección de posiciones (ADO MD)](../ado-md-api/positions-collection-ado-md.md)  
        [Colección de procedimientos (ADOX)](../adox-api/procedures-collection-adox.md)  
        [Colección de propiedades (ADO)](./properties-collection-ado.md)  
        [Colección de tablas (ADOX)](../adox-api/tables-collection-adox.md)  
        [Colección de usuarios (ADOX)](../adox-api/users-collection-adox.md)  
        [Colección de vistas (ADOX)](../adox-api/views-collection-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad Item (VB)](./item-property-example-vb.md)   
 [Ejemplo de la propiedad de elemento (VC ++)](./item-property-example-vc.md)