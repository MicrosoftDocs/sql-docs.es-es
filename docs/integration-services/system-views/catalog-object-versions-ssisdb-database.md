---
description: catalog.object_versions (base de datos de SSISDB)
title: catalog.object_versions (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: 2fd8c020-1c77-4702-8e6b-efa6a348daab
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 83ccc09b8a38340939340f214c3ca62fddca6d47
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835178"
---
# <a name="catalogobject_versions-ssisdb-database"></a>catalog.object_versions (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra las versiones de objetos en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. En esta versión, solo las versiones de proyectos se admiten en esta vista.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_version_lsn|**bigint**|Identificador único (ID) de la versión del objeto. No se garantiza que este número sea secuencial.|  
|object_id|**bigint**|Identificador único del objeto.|  
|object_type|**smallint**|Tipo del objeto. Se mostrará un valor `20` para los proyectos.|  
|object_name|**sysname(nvarchar(128))**|El nombre del objeto.|  
|description|**nvarchar(1024)**|Descripción del proyecto.|  
|created_by|**nvarchar(128)**|Nombre del usuario que agregó el objeto al catálogo.|  
|created_time|**datetimeoffset**|Fecha y hora en que el objeto se agregó al catálogo.|  
|restored_by|**nvarchar(128)**|Nombre del usuario que restauró el objeto.|  
|last_restored_time|**datetimeoffset**|Fecha y hora en que el objeto se restauró por última vez.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada versión de un objeto en el catálogo.  
  
## <a name="permissions"></a>Permisos  
 Para ver las filas en esta vista, debe tener uno de los siguientes permisos:  
  
-   Permiso READ en el objeto  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor **sysadmin**  
  
> [!NOTE]  
>  Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
