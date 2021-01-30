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
ms.openlocfilehash: c5ca59cb5900643202c2d541072c48b275b5b5df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169136"
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