---
description: GetPermissions (método, ADOX)
title: GetPermissions (método) (ADOX) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _User25::GetPermissions
- _Group25::raw_GetPermissions
- _Group25::GetPermissions
- _User25::raw_GetPermissions
helpviewer_keywords:
- GetPermissions method [ADOX]
ms.assetid: df201c1f-c76a-465d-98f0-83b7fc36e6e3
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a90b95cb6ca9d020ce841524caa498743b8b1d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100050085"
---
# <a name="getpermissions-method-adox"></a>GetPermissions (método, ADOX)
Devuelve los permisos de un [Grupo](./group-object-adox.md) o [usuario](./user-object-adox.md) en un contenedor de objetos o objetos.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
ReturnValue=GroupOrUser.GetPermissions(Name, ObjectType    [,ObjectTypeId])  
```  
  
## <a name="return-value"></a>Valor devuelto  
 Devuelve un valor de **tipo Long** que especifica una máscara de caracteres que contiene los permisos que el grupo o el usuario tiene en el objeto. Este valor puede ser una o varias de las constantes [RightsEnum](./rightsenum.md) .  
  
#### <a name="parameters"></a>Parámetros  
 *Nombre*  
 Valor **Variant** que especifica el nombre del objeto para el que se van a establecer los permisos. Establezca *nombre* en un valor nulo si desea obtener los permisos para el contenedor de objetos.  
  
 *ObjectType*  
 Un valor **Long** que puede ser una de las constantes [ObjectTypeEnum](./objecttypeenum.md) , que especifica el tipo del objeto para el que se van a obtener permisos.  
  
 *ObjectTypeId*  
 Opcional. Valor **Variant** que especifica el GUID de un tipo de objeto de proveedor no definido por la especificación de OLE DB. Este parámetro es necesario si *objecttype* está establecido en **adPermObjProviderSpecific**; de lo contrario, no se utiliza.  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [Objeto Group (ADOX)](./group-object-adox.md)  
    :::column-end:::
    :::column:::
        [Objeto User (ADOX)](./user-object-adox.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Ejemplo de métodos GetPermissions y SetPermissions (VB)](./getpermissions-and-setpermissions-methods-example-vb.md)   
 [Name (propiedad, ADOX)](./name-property-adox.md)   
 [SetPermissions (método, ADOX)](./setpermissions-method-adox.md)