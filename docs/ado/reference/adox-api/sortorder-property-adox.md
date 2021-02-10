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
ms.openlocfilehash: 939630bea7346d1c4602fb04a6d9ed21a7b3fef7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053516"
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