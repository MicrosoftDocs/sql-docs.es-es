---
description: RightsEnum
title: RightsEnum | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- RightsEnum
helpviewer_keywords:
- RightsEnum enumeration [ADOX]
ms.assetid: 55ee67c7-a583-42aa-849a-78264b4cb614
author: rothja
ms.author: jroth
ms.openlocfilehash: 144e01921ec2c0aea14b3b853e0c9ce4bf8daf5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100053800"
---
# <a name="rightsenum"></a>RightsEnum
Especifica los derechos o permisos para un grupo o usuario en un objeto.  
  
|Constante|Value|Descripción|  
|--------------|-----------|-----------------|  
|**adRightCreate**|16384 (&H4000)|El usuario o el grupo tiene permiso para crear nuevos objetos de este tipo.|  
|**adRightDelete**|65536 (&H10000)|El usuario o el grupo tiene permiso para eliminar datos de un objeto. En el caso de objetos como **las tablas**, el usuario tiene permiso para eliminar los valores de los datos de los registros.|  
|**adRightDrop**|256 (&H100)|El usuario o el grupo tiene permiso para quitar objetos del catálogo. Por ejemplo, **las tablas** se pueden eliminar mediante un comando DROP TABLE de SQL.|  
|**adRightExclusive**|512 (&H200)|El usuario o el grupo tiene permiso para obtener acceso al objeto exclusivamente.|  
|**adRightExecute**|536870912 (&H20000000)|El usuario o el grupo tiene permiso para ejecutar el objeto.|  
|**adRightFull**|268435456 (&H10000000)|El usuario o grupo tiene todos los permisos en el objeto.|  
|**adRightInsert**|32768 (&H8000)|El usuario o el grupo tiene permiso para insertar el objeto. En el caso de objetos como **las tablas**, el usuario tiene permiso para insertar datos en la tabla.|  
|**adRightMaximumAllowed**|33554432 (&H2000000)|El usuario o grupo tiene el número máximo de permisos permitido por el proveedor. Los permisos específicos dependen del proveedor.|  
|**adRightNone**|0|El usuario o grupo no tiene permisos para el objeto.|  
|**adRightRead**|-2147483648 (&H80000000)|El usuario o el grupo tiene permiso para leer el objeto. En el caso de objetos como [las tablas](./table-object-adox.md), el usuario tiene permiso para leer los datos de la tabla.|  
|**adRightReadDesign**|1024 (&H400)|El usuario o el grupo tiene permiso para leer el diseño del objeto.|  
|**adRightReadPermissions**|131072 (&H20000)|El usuario o grupo puede ver, pero no cambiar, los permisos específicos para un objeto en el catálogo.|  
|**adRightReference**|8192 (&H2000)|El usuario o el grupo tiene permiso para hacer referencia al objeto.|  
|**adRightUpdate**|1073741824 (&H40000000)|El usuario o el grupo tiene permiso para actualizar el objeto. En el caso de objetos como **las tablas**, el usuario tiene permiso para actualizar los datos de la tabla.|  
|**adRightWithGrant**|4096 (&H1000)|El usuario o el grupo tiene permiso para conceder permisos en el objeto.|  
|**adRightWriteDesign**|2048 (&H800)|El usuario o el grupo tiene permiso para modificar el diseño del objeto.|  
|**adRightWriteOwner**|524288 (&H80000)|El usuario o el grupo tiene permiso para modificar el propietario del objeto.|  
|**adRightWritePermissions**|262144 (&H40000)|El usuario o grupo puede modificar los permisos específicos para un objeto en el catálogo.|  
  
## <a name="applies-to"></a>Se aplica a  

:::row:::
    :::column:::
        [GetPermissions (método, ADOX)](./getpermissions-method-adox.md)  
    :::column-end:::
    :::column:::
        [SetPermissions (método, ADOX)](./setpermissions-method-adox.md)  
    :::column-end:::
:::row-end:::