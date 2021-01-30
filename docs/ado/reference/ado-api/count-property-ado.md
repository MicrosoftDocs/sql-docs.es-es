---
description: Count (propiedad, ADO)
title: Propiedad Count (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Collection::Count
helpviewer_keywords:
- Count property [ADO]
ms.assetid: da9ccd1f-d402-41a2-940c-45556fc5340d
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a99489ca105a3661fb17c7ea8a62ee831e899ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167668"
---
# <a name="count-property-ado"></a>Count (propiedad, ADO)
Indica el número de objetos de una colección.  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor **Long** .  
  
## <a name="remarks"></a>Observaciones  
 Utilice la propiedad **Count** para determinar el número de objetos que hay en una colección determinada.  
  
 Dado que la numeración de los miembros de una colección comienza con cero, siempre debe codificar los bucles empezando por el miembro cero y terminando con el valor de la propiedad **Count** menos 1. Si usa Microsoft Visual Basic y desea crear un bucle a través de los miembros de una colección sin comprobar la propiedad Count, utilice el **parámetro** **for each... Comando siguiente** .  
  
 Si la propiedad **Count** es cero, no hay ningún objeto en la colección.  
  
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
 [Ejemplo de la propiedad Count (VB)](./count-property-example-vb.md)   
 [Ejemplo de la propiedad Count (VC + +)](./count-property-example-vc.md)   
 [Actualizar (método, ADO)](./refresh-method-ado.md)