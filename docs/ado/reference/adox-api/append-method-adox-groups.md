---
description: Append (método) (grupos ADOX)
title: Append (método) (grupos ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Groups::raw_Append
- Groups::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: 56b94fc6-7ef0-4e4a-82a3-033b94c46036
author: rothja
ms.author: jroth
ms.openlocfilehash: 846475567fb15e549f96094ca55a1881d8888ce5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050536"
---
# <a name="append-method-adox-groups"></a>Append (método) (grupos ADOX)
Agrega un nuevo objeto de [Grupo](./group-object-adox.md) a la colección de [grupos](./groups-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Groups.Append Group  
```  
  
#### <a name="parameters"></a>Parámetros  
 *Grupo*  
 Objeto de **Grupo** que se va a anexar o nombre del grupo que se va a crear y anexar.  
  
## <a name="remarks"></a>Observaciones  
 La colección de **grupos** de un [Catálogo](./catalog-object-adox.md) representa todas las cuentas de grupo del catálogo. La colección de **grupos** para un [usuario](./user-object-adox.md) representa solo el grupo al que pertenece el usuario.  
  
 Se producirá un error si el proveedor no admite la creación de grupos.  
  
> [!NOTE]
>  Antes de anexar un objeto de **Grupo** a la colección **Groups** de un objeto **User** , el objeto **Group** con el mismo [nombre](./name-property-adox.md) que el que se va a anexar ya debe existir en la colección **Groups** del **Catálogo**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de grupos (ADOX)](./groups-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos Append y ChangePassword de grupos y usuarios (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (índices ADOX)](./append-method-adox-indexes.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Append (método) (procedimientos ADOX)](./append-method-adox-procedures.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (usuarios ADOX)](./append-method-adox-users.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)