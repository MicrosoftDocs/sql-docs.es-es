---
description: ObjectTypeEnum
title: ObjectTypeEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- ObjectTypeEnum
helpviewer_keywords:
- ObjectTypeEnum enumeration [ADOX]
ms.assetid: 3fdecfca-aa91-4596-ad98-610f1b7f840b
author: rothja
ms.author: jroth
ms.openlocfilehash: 9a5ff3f235a01bb84a05b9c4efdff5c1fd15b7c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049895"
---
# <a name="objecttypeenum"></a>ObjectTypeEnum
Especifica el tipo de objeto de base de datos para el que se van a establecer permisos o propiedad.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adPermObjColumn**|2|El objeto es una columna.|  
|**adPermObjDatabase**|3|El objeto es una base de datos.|  
|**adPermObjProcedure**|4|El objeto es un procedimiento.|  
|**adPermObjProviderSpecific**|-1|El objeto es un tipo definido por el proveedor. Se producirá un error si el parámetro *objecttype* es **adPermObjProviderSpecific** y no se proporciona un *ObjectTypeId* .|  
|**adPermObjTable**|1|El objeto es una tabla.|  
|**adPermObjView**|5|El objeto es una vista.|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [GetObjectOwner (método, ADOX)](./getobjectowner-method-adox.md)  
        [GetPermissions (método, ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [SetObjectOwner (método)](./setobjectowner-method.md)  
        [SetPermissions (método, ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::