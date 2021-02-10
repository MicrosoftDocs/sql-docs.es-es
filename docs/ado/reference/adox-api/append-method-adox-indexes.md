---
description: Append (método) (índices ADOX)
title: Append (método) (índices ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Indexes::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 6695769f-275b-4b70-81bd-1a5f7d74926c
author: rothja
ms.author: jroth
ms.openlocfilehash: feb259809798e3260e4e201c4658bfbc1455f168
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050546"
---
# <a name="append-method-adox-indexes"></a>Append (método) (índices ADOX)
Agrega un nuevo objeto [index](./index-object-adox.md) a la colección [Indexes](./indexes-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Indexes.Append Index [,Columns]  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Index*  
 Objeto de **Índice** que se va a anexar o nombre del índice que se va a crear y anexar.  
  
 *Columnas*  
 Opcional. Valor **Variant** que especifica los nombres de las columnas que se van a indizar. El parámetro *Columns* corresponde a los valores de la propiedad [Name](./name-property-adox.md) de un objeto de [columna](./column-object-adox.md) u objetos.  
  
## <a name="remarks"></a>Observaciones  
 El parámetro *Columns* puede tomar el nombre de una columna o una matriz de nombres de columna.  
  
 Se producirá un error si el proveedor no admite la creación de índices.  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de índices (ADOX)](./indexes-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de método Append de índices (VB)](./indexes-append-method-example-vb.md)   
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (grupos ADOX)](./append-method-adox-groups.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Append (método) (procedimientos ADOX)](./append-method-adox-procedures.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (usuarios ADOX)](./append-method-adox-users.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)