---
description: SortOrder (propiedad, ADOX)
title: SortOrder (propiedad, ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Column::SortOrder
- _Column::PutSortOrder
- _Column::put_SortOrder
- _Column::get_SortOrder
- _Column::GetSortOrder
helpviewer_keywords:
- SortOrder property [ADOX]
ms.assetid: 04510b19-9cb2-4895-b23b-f7790123eb04
author: rothja
ms.author: jroth
ms.openlocfilehash: ff50a29404b063ee8f7eee2d88d93c2ac2cbcf6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169197"
---
# <a name="sortorder-property-adox"></a>SortOrder (propiedad, ADOX)
Indica la secuencia de ordenación de la columna (solo columnas de índice).  
  
## <a name="settings-and-return-values"></a>Configuración y valores devueltos  
 Establece y devuelve un valor **Long** que puede ser una de las constantes [SortOrderEnum](./sortorderenum.md) . El valor predeterminado es **adSortAscending**.  
  
## <a name="remarks"></a>Observaciones  
 Esta propiedad solo se aplica a los objetos de [columna](./column-object-adox.md) de la colección [Columns](./columns-collection-adox.md) de un [Índice](./index-object-adox.md).  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Column (ADOX)](./column-object-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de la propiedad SortOrder (VB)](./sortorder-property-example-vb.md)   
 [Colección de columnas (ADOX)](./columns-collection-adox.md)   
 [Objeto Index (ADOX)](./index-object-adox.md)