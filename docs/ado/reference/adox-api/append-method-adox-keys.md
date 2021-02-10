---
description: Append (método) (claves ADOX)
title: Append (método) (claves ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Keys::Append
- Keys::raw_Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 215a5391-f422-42ec-99ea-4e6fbb5d3d64
author: rothja
ms.author: jroth
ms.openlocfilehash: 573bfb24f68896f647f4c0d9ff5867ee1d38e5fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050516"
---
# <a name="append-method-adox-keys"></a>Append (método) (claves ADOX)
Agrega un nuevo objeto [clave](./key-object-adox.md) a la colección de [claves](./keys-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Keys.Append Key [,KeyType] [,Column] [,RelatedTable] [,RelatedColumn]  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Key*  
 Objeto de **clave** que se va a anexar o el nombre de la clave que se va a crear y anexar.  
  
 *KeyType*  
 Opcional. Valor **largo** que especifica el tipo de clave. El parámetro *clave* corresponde a la propiedad [Type](./type-property-key-adox.md) de un objeto **key** .  
  
 *Columna*  
 Opcional. Valor de **cadena** que especifica el nombre de la columna que se va a indizar. El parámetro *Columns* corresponde al valor de la propiedad [Name](./name-property-adox.md) de un objeto [Column](./column-object-adox.md) .  
  
 *RelatedTable*  
 Opcional. Valor de **cadena** que especifica el nombre de la tabla relacionada. El parámetro *RelatedTable* corresponde al valor de la propiedad **Name** de un objeto [TABLE](./table-object-adox.md) .  
  
 *RelatedColumn*  
 Opcional. Valor de **cadena** que especifica el nombre de la columna relacionada para una clave externa. El parámetro *RelatedColumn* corresponde al valor de la propiedad **Name** de un objeto [Column](./column-object-adox.md) .  
  
## <a name="remarks"></a>Observaciones  
 El parámetro *Columns* puede tomar el nombre de una columna o una matriz de nombres de columna.  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de claves (ADOX)](./keys-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de las propiedades de método Append, tipo de clave, RelatedColumn, RelatedTable y UpdateRule (VB)](./keys-append-method-key-type-relatedcolumn-relatedtable-example-vb.md)   
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (grupos ADOX)](./append-method-adox-groups.md)   
 [Append (método) (índices ADOX)](./append-method-adox-indexes.md)   
 [Append (método) (procedimientos ADOX)](./append-method-adox-procedures.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (usuarios ADOX)](./append-method-adox-users.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)