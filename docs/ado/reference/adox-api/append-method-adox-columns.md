---
description: Append (método) (columnas ADOX)
title: Append (método) (columnas ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Columns::raw_Append
- Columns::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 7a46d23c-efef-4ec7-815d-cd3ac86787dd
author: rothja
ms.author: jroth
ms.openlocfilehash: 40abf0ae298b469d0814e8dd3da85a8c8865be78
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169599"
---
# <a name="append-method-adox-columns"></a>Append (método) (columnas ADOX)
Agrega un nuevo objeto de [columna](./column-object-adox.md) a la colección de [columnas](./columns-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Columns.Append Column [,Type] [,DefinedSize]  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Columna*  
 Objeto de **columna** que se va a anexar o nombre de la columna que se va a crear y anexar.  
  
 *Type*  
 Opcional. Valor **largo** que especifica el tipo de datos de la columna. El parámetro de *tipo* corresponde a la propiedad [Type](./type-property-column-adox.md) de un objeto **Column** .  
  
 *DefinedSize*  
 Opcional. Valor de **tipo Long** que especifica el tamaño de la columna. El parámetro *DefinedSize* corresponde a la propiedad [DefinedSize](./definedsize-property-adox.md) de un objeto **Column** .  
  
> [!NOTE]
>  Se producirá un error al anexar una **columna** a la colección de **columnas** de un [Índice](./index-object-adox.md) si la **columna** no existe en una [tabla](./table-object-adox.md) que ya esté anexada a la colección de [tablas](./tables-collection-adox.md) .  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de columnas (ADOX)](./columns-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de propiedad Name, métodos Append de columnas y tablas (VB)](./columns-and-tables-append-methods-name-property-example-vb.md)   
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Ejemplo de propiedad ParentCatalog (VB)](./parentcatalog-property-example-vb.md)   
 [Append (método) (grupos ADOX)](./append-method-adox-groups.md)   
 [Append (método) (índices ADOX)](./append-method-adox-indexes.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Append (método) (procedimientos ADOX)](./append-method-adox-procedures.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (usuarios ADOX)](./append-method-adox-users.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)