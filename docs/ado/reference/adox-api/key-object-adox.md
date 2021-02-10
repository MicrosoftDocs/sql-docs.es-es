---
description: Objeto Key (ADOX)
title: Objeto Key (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Key
helpviewer_keywords:
- Key object [ADOX]
ms.assetid: 55f116fe-4d56-4892-bffe-0cdd6fc727c9
author: rothja
ms.author: jroth
ms.openlocfilehash: b0e2cdd3dcb1d6d0772e1b7508bee8063089a4e3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100054080"
---
# <a name="key-object-adox"></a>Objeto Key (ADOX)
Representa un campo de clave principal, externa o única de una tabla de base de datos.  
  
## <a name="remarks"></a>Observaciones  
 En el código siguiente se crea una nueva **clave**:  
  
```  
Dim obj As New Key  
```  
  
 Con las propiedades y las colecciones de un objeto de **clave** , puede:  
  
-   Identifique la clave con la propiedad [Name](./name-property-adox.md) .  
  
-   Determine si la clave es principal, externa o única con la propiedad [Type](./type-property-key-adox.md) .  
  
-   Obtenga acceso a las columnas de base de datos de la clave con la colección de [columnas](./columns-collection-adox.md) .  
  
-   Especifique el nombre de la tabla relacionada con la propiedad [RelatedTable](./relatedtable-property-adox.md) .  
  
-   Determine la acción realizada al eliminar o actualizar una clave principal con las propiedades [DeleteRule](./deleterule-property-adox.md) y [UpdateRule](./updaterule-property-adox.md) .  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto Key](./key-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Colección de claves (ADOX)](./keys-collection-adox.md)