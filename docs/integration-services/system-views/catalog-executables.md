---
description: catalog.executables
title: catalog.executables | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
ms.assetid: bae22d0c-e190-426f-a074-c1d1170e8dd8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 3aff9256e3f8468642177a24975489ebf7ebd646
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835332"
---
# <a name="catalogexecutables"></a>catalog.executables 

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  En esta vista se muestra una fila para cada ejecutable en la ejecución especificada.  
  
 Un ejecutable es una tarea o un contenedor que se agrega al flujo de control de un paquete.  
  
|Nombre de la columna|**Tipo de datos**|Descripción|  
|-----------------|-------------------|-----------------|  
|executable_id|**bigint**|El identificador único para el ejecutable.|  
|execution_id|**bigint**|Identificador único de la instancia de ejecución.|  
|executable_name|**nvarchar(4000)**|El nombre del ejecutable.|  
|executable_guid|**nvarchar(38)**|El GUID del ejecutable.|  
|package_name|**nvarchar(260)**|Nombre del paquete.|  
|package_path|**nvarchar(max)**|La ruta de acceso del paquete.|  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Cuando se dispone de permiso para realizar una operación en el servidor, también se dispone de permiso para ver información sobre la operación. Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
## <a name="remarks"></a>Observaciones  
  
