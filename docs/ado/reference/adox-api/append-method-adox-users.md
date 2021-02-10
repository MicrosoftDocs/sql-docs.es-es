---
description: Append (método) (usuarios ADOX)
title: Append (método) (usuarios ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Users::raw_Append
- Users::Append
helpviewer_keywords:
- Append method [ADOX]
ms.assetid: b80bc5d5-78ca-4f75-956b-2ac658029cc7
author: rothja
ms.author: jroth
ms.openlocfilehash: 6df2a76aa32ceec8e77bf27424345fc6bae52233
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050476"
---
# <a name="append-method-adox-users"></a>Append (método) (usuarios ADOX)
Agrega un nuevo objeto de [usuario](./user-object-adox.md) a la colección de [usuarios](./users-collection-adox.md) .  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Users.Append User[,Password]  
```  
  
#### <a name="parameters"></a>Parámetros  
 *User*  
 Valor de **tipo Variant** que contiene el objeto de **usuario** que se va a anexar o el nombre del usuario que se va a crear y anexar.  
  
 *Contraseña*  
 Opcional. Valor de **cadena** que contiene la contraseña del usuario. El parámetro *password* corresponde al valor especificado por el método [ChangePassword](./changepassword-method-adox.md) de un objeto **User** .  
  
## <a name="remarks"></a>Observaciones  
 La colección de **usuarios** de un [Catálogo](./catalog-object-adox.md) representa todos los usuarios del catálogo. La colección de **usuarios** de un [Grupo](./group-object-adox.md) representa solo los usuarios que tienen una pertenencia al grupo específico.  
  
 Se producirá un error si el proveedor no admite la creación de usuarios.  
  
> [!NOTE]
>  Antes de anexar un objeto de **usuario** a la colección de **usuarios** de un objeto de **Grupo** , ya debe existir un objeto de **usuario** con el mismo [nombre](./name-property-adox.md) que el que se va a anexar en la colección de **usuarios** del **Catálogo**.  
  
## <a name="applies-to"></a>Se aplica a  
 [Colección de usuarios (ADOX)](./users-collection-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos Append y ChangePassword de grupos y usuarios (VB)](./groups-and-users-append-changepassword-methods-example-vb.md)   
 [Append (método) (columnas ADOX)](./append-method-adox-columns.md)   
 [Append (método) (grupos ADOX)](./append-method-adox-groups.md)   
 [Append (método) (índices ADOX)](./append-method-adox-indexes.md)   
 [Append (método) (claves ADOX)](./append-method-adox-keys.md)   
 [Append (método) (procedimientos ADOX)](./append-method-adox-procedures.md)   
 [Append (método) (tablas ADOX)](./append-method-adox-tables.md)   
 [Append (método) (vistas ADOX)](./append-method-adox-views.md)