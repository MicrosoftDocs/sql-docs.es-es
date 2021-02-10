---
description: SetObjectOwner (método)
title: Método SetObjectOwner | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Catalog::SetObjectOwner
- _Catalog::raw_SetObjectOwner
helpviewer_keywords:
- SetObjectOwner method [ADOX]
ms.assetid: e5170a37-9d6e-43db-bfb6-9b6631fa3048
author: rothja
ms.author: jroth
ms.openlocfilehash: 2d75966ce8d5c1f9fddea9c09ff254eca9106e62
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053810"
---
# <a name="setobjectowner-method"></a>SetObjectOwner (método)
Especifica el propietario de un objeto de un [Catálogo](./catalog-object-adox.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
Catalog.SetObjectOwner ObjectName, ObjectType, OwnerName [,ObjectTypeId]  
```  
  
#### <a name="parameters"></a>Parámetros  
 *ObjectName*  
 Valor de **cadena** que especifica el nombre del objeto para el que se va a especificar el propietario.  
  
 *ObjectType*  
 Un valor **Long** que puede ser una de las constantes [ObjectTypeEnum](./objecttypeenum.md) que especifica el tipo de propietario.  
  
 *OwnerName*  
 Valor de **cadena** que especifica el [nombre](./name-property-adox.md) del [usuario](./user-object-adox.md) o [Grupo](./group-object-adox.md) que va a poseer el objeto.  
  
 *ObjectTypeId*  
 Opcional. Valor **Variant** que especifica el GUID de un tipo de objeto de proveedor que no está definido por la especificación OLE DB. Este parámetro es necesario si *objecttype* está establecido en **adPermObjProviderSpecific**; de lo contrario, no se utiliza.  
  
## <a name="remarks"></a>Observaciones  
 Se producirá un error si el proveedor no admite la especificación de propietarios de objetos.  
  
## <a name="applies-to"></a>Se aplica a  
 [Objeto Catalog (ADOX)](./catalog-object-adox.md)  
  
## <a name="see-also"></a>Consulte también  
 [Ejemplo de los métodos GetObjectOwner y SetObjectOwner (VB)](./getobjectowner-and-setobjectowner-methods-example-vb.md)   
 [GetObjectOwner (método, ADOX)](./getobjectowner-method-adox.md)