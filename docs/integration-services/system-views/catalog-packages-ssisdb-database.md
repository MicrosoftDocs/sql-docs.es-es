---
description: catalog.packages (base de datos de SSISDB)
title: catalog.packages (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- packages view [Integration Services]
- catalog.packages view [Integration Services]
ms.assetid: a634e94d-f492-4dfd-9611-a35f545106a1
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 7d506b377970879c865ed807a4377a0c1bc1241b
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835246"
---
# <a name="catalogpackages-ssisdb-database"></a>catalog.packages (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra los detalles de todos los paquetes que aparecen en el catálogo de **SSISDB**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|package_id|**bigint**|El identificador único (Id.) del paquete.|  
|name|**nvarchar(256)**|El nombre único del paquete.|  
|package_guid|**uniqueidentifier**|El identificador único global (GUID) que identifica el paquete.|  
|description|**nvarchar(1024)**|Descripción opcional del paquete.|  
|package_format_version|**int**|La versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizada para desarrollar el paquete.|  
|version_major|**int**|Versión principal del paquete.|  
|version_minor|**int**|Versión secundaria del paquete.|  
|version_build|**int**|La versión de la compilación del paquete.|  
|version_comments|**nvarchar(1024)**|Comentarios opcionales acerca de la versión del paquete.|  
|version_guid|**uniqueidentifier**|El GUID que identifica de forma exclusiva la versión del paquete.|  
|identificador de proyecto|**bigint**|Identificador único del proyecto.|  
|entry_point|**bit**|El valor de `1` significa que el paquete se puede iniciar directamente. El valor de `0` significa que el paquete debe ser iniciado por otro paquete mediante la tarea Ejecutar paquete. El valor predeterminado es `1`.|  
|validation_status|**char(1)**|El estado de la validación.|  
|last_validation_time|**datetimeoffset(7)**|Hora de la última validación.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada paquete del catálogo.  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en el proyecto correspondiente  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol del servidor de **sysadmin**  
  
> [!NOTE]  
>  Si tiene permiso READ en un proyecto, también tendrá permiso READ en todos los paquetes y referencias de entorno asociados a ese proyecto. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
