---
description: Objeto User (ADOX)
title: Objeto user (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- User
helpviewer_keywords:
- User object [ADOX]
ms.assetid: f68e32ce-ef7c-407d-bdb5-d280947ae0e2
author: rothja
ms.author: jroth
ms.openlocfilehash: e37c09d4793a96056ab5d8003bf022b1d250b744
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049685"
---
# <a name="user-object-adox"></a>Objeto User (ADOX)
Representa una cuenta de usuario que tiene permisos de acceso en una base de datos protegida.  
  
## <a name="remarks"></a>Observaciones  
 La colección de [usuarios](./users-collection-adox.md) de un [Catálogo](./catalog-object-adox.md) representa todos los usuarios del catálogo. La colección de **usuarios** de un [Grupo](./group-object-adox.md) solo representa a los usuarios del grupo específico.  
  
 Con las propiedades, colecciones y métodos de un objeto de **usuario** , puede:  
  
-   Identifique al usuario con la propiedad [Name](./name-property-adox.md) .  
  
-   Cambiar la contraseña de un usuario con el método [ChangePassword](./changepassword-method-adox.md) .  
  
-   Determine si un usuario tiene permisos de lectura, escritura o eliminación con los métodos [GetPermissions](./getpermissions-method-adox.md) y [SetPermissions](./setpermissions-method-adox.md) .  
  
-   Acceder a los grupos a los que pertenece un usuario con la colección de [grupos](./groups-collection-adox.md) .  
  
-   Obtener acceso a las propiedades específicas del proveedor con la colección [Properties](../ado-api/properties-collection-ado.md) .  
  
-   Determinar el [ParentCatalog](./parentcatalog-property-adox.md) de un usuario.  
  
 Esta sección contiene el siguiente tema.  
  
-   [Propiedades, métodos y eventos del objeto User](./user-object-properties-methods-and-events.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos GetPermissions y SetPermissions (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Colección de grupos (ADOX)](./groups-collection-adox.md)   
 [Colección de usuarios (ADOX)](./users-collection-adox.md)