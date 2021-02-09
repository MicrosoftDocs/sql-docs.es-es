---
description: catalog.effective_object_permissions (base de datos de SSISDB)
title: catalog.effective_object_permissions (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- catalog.effective_object_permissions views [Integration Services]
- effective_object_permissions view [Integration Services]
ms.assetid: e70c4ce9-79f5-44df-ac75-6c29b6e38776
author: chugugrace
ms.author: chugu
ms.openlocfilehash: ffe8d9eef7322fce02977527aa625755b6e8c0c8
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835281"
---
# <a name="catalogeffective_object_permissions-ssisdb-database"></a>catalog.effective_object_permissions (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra los permisos efectivos de la entidad de seguridad actual para todos los objetos del catálogo [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)].  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_type|**smallint**|Tipo del objeto protegible. Entre los tipos de objetos protegibles se incluyen carpeta (`1`), proyecto (`2`), entorno (`3`) y operación (`4`).|  
|object_id|**bigint**|Identificador único (Id.) o clave principal del objeto protegido.|  
|permission_type|**smallint**|Tipo de permiso.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra los tipos de permisos enumerados en la siguiente tabla:  
  
|Valor de permission_type|Nombre del permiso|Descripción del permiso|Tipos de objetos aplicables|  
|----------------------------|---------------------|----------------------------|-----------------------------|  
|`1`|READ|Permite a la entidad de seguridad leer información que se considera parte del objeto, como las propiedades. No permite a la entidad de seguridad enumerar o leer el contenido de otros objetos contenidos dentro del objeto.|Carpeta, proyecto, entorno, operación|  
|`2`|MODIFY|Permite a la entidad de seguridad modificar información que se considera parte del objeto, como las propiedades. No permite a la entidad de seguridad modificar otros objetos contenidos dentro del objeto.|Carpeta, proyecto, entorno, operación|  
|`3`|Ejecute|Permite a la entidad de seguridad ejecutar todos los paquetes del proyecto.|Project|  
|`4`|MANAGE_PERMISSIONS|Permite a la entidad de seguridad asignar permisos a los objetos.|Carpeta, proyecto, entorno, operación|  
|`100`|CREATE_OBJECTS|Permite a la entidad de seguridad crear objetos en la carpeta.|Carpeta|  
|`101`|READ_OBJECTS|Permite a la entidad de seguridad leer todos los objetos de la carpeta.|Carpeta|  
|`102`|MODIFY_OBJECTS|Permite a la entidad de seguridad modificar todos los objetos de la carpeta.|Carpeta|  
|`103`|EXECUTE_OBJECTS|Permite a la entidad de seguridad ejecutar todos los paquetes de todos los proyectos de la carpeta.|Carpeta|  
|`104`|MANAGE_OBJECT_PERMISSIONS|Permite a la entidad de seguridad administrar los permisos de todos los objetos de la carpeta.|Carpeta|  
  
 Solo se evalúan los objetos en los que el autor de llamada tiene los permisos. Los permisos se calculan basándose en lo siguiente:  
  
-   Permisos explícitos  
  
-   Permisos heredados  
  
-   La pertenencia de la entidad de seguridad en roles  
  
-   La pertenencia de la entidad de seguridad en grupos  
  
## <a name="permissions"></a>Permisos  
 Los usuarios pueden ver los permisos efectivos solo de ellos mismos y de los roles de los que son miembro.  
  
  
