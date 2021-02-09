---
description: catalog.executions (base de datos de SSISDB)
title: catalog.executions (base de datos de SSISDB) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: language-reference
helpviewer_keywords:
- executions view [Integration Services]
- catalog.executions view [Integration Services]
ms.assetid: 879f13b0-331d-4dee-a079-edfaca11ae5b
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a95232db301aa538a166e930c4dde830691adaac
ms.sourcegitcommit: 868c60aa3a76569faedd9b53187e6b3be4997cc9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/08/2021
ms.locfileid: "99835124"
---
# <a name="catalogexecutions-ssisdb-database"></a>catalog.executions (base de datos de SSISDB)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

  Muestra las instancias de ejecución del paquete en el catálogo de [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]. Los paquetes que se ejecutan con la tarea Ejecutar paquete se ejecutan en la misma instancia de ejecución que el paquete primario.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|execution_id|**bigint**|Identificador (id.) único global de la instancia de ejecución.|  
|nombreDeCarpeta|**sysname(nvarchar(128))**|Nombre de la carpeta que contiene el proyecto.|  
|project_name|**sysname(nvarchar(128))**|Nombre del proyecto.|  
|package_name|**nvarchar(260)**|Nombre del primer paquete que se inició durante la ejecución.|  
|reference_id|**bigint**|Entorno al que hace referencia la instancia de ejecución.|  
|reference_type|**char(1)**|Indica si el entorno se puede encontrar en la misma carpeta que el proyecto (referencia relativa) o en una carpeta diferente (referencia absoluta). Cuando el valor es `R`, el entorno se encuentra utilizando una referencia relativa. Cuando el valor es `A`, el entorno se encuentra utilizando una referencia absoluta.|  
|environment_folder_name|**nvarchar(128)**|Nombre de la carpeta que contiene el entorno.|  
|environment_name|**nvarchar(128)**|Nombre del entorno al que se hizo referencia durante la ejecución.|  
|project_lsn|**bigint**|Versión del proyecto correspondiente a la instancia de ejecución. No se garantiza que este número sea secuencial.|  
|executed_as_sid|**varbinary(85)**|SID del usuario que inició la instancia de ejecución.|  
|executed_as_name|**nvarchar(128)**|Nombre de la entidad de seguridad de base de datos que se utilizó para iniciar la instancia de ejecución.|  
|use32bitruntime|**bit**|Indica si el motor en tiempo de ejecución de 32 bits se usa para ejecutar el paquete en un sistema operativo de 64 bits. Si el valor es `1`, se realiza la ejecución con el motor en tiempo de ejecución de 32 bits. Si el valor es `0`, se realiza la ejecución con el motor en tiempo de ejecución de 64 bits.|  
|object_type|**smallint**|Tipo del objeto. El objeto puede ser un proyecto (`20`) o un paquete (`30`).|  
|object_id|**bigint**|Identificador del objeto afectado por la operación.|  
|status|**int**|Estado de la operación. Los valores posibles son creado (`1`), en ejecución (`2`), cancelado (`3`), con errores (`4`), pendiente (`5`), finalizado inesperadamente (`6`), correcto (`7`), deteniendo (`8`) y completado (`9`).|  
|start_time|**datetimeoffset**|Hora a la que se inició la instancia de ejecución.|  
|end_time|**datetimeoffsset**|Hora a la que finalizó la instancia de ejecución.|  
|caller_sid|**varbinary(85)**|Identificador de seguridad (SID) del usuario si se utilizó Autenticación de Windows para iniciar sesión.|  
|caller_name|**nvarchar(128)**|Nombre de la cuenta que realizó la operación.|  
|process_id|**int**|Identificador de proceso del proceso externo, si es aplicable.|  
|stopped_by_sid|**varbinary(85)**|Identificador de seguridad (SID) del usuario que detuvo la instancia de ejecución.|  
|stopped_by_name|**nvarchar(128)**|Nombre del usuario que detuvo la instancia de ejecución.|  
|total_physical_memory_kb|**bigint**|Memoria física total (en megabytes) en el servidor cuando se inició la ejecución.|  
|available_physical_memory_kb|**bigint**|Memoria física disponible (en megabytes) en el servidor cuando se inició la ejecución.|  
|total_page_file_kb|**bigint**|Memoria de página total (en megabytes) en el servidor cuando se inició la ejecución.|  
|available_page_file_kb|**bigint**|Memoria de página disponible (en megabytes) en el servidor cuando se inició la ejecución.|  
|cpu_count|**int**|Número de CPU lógicas en el servidor cuando se inició la ejecución.|  
|server_name|**nvarchar(128)**|Información del servidor Windows y de la instancia asociada a una instancia especificada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|machine_name|**nvarchar(128)**|Nombre del equipo en el que se está ejecutando la instancia del servidor.|  
|dump_id|**uniqueidentifier**|El identificador de un volcado de ejecución.|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista muestra una fila para cada instancia de ejecución del catálogo.  
  
## <a name="permissions"></a>Permisos  
 Esta vista exige uno de los siguientes permisos:  
  
-   Permiso READ en la instancia de ejecución  
  
-   Pertenencia al rol de base de datos de **ssis_admin**  
  
-   Pertenencia al rol de servidor de **sysadmin**  
  
> [!NOTE]  
>  Se aplica la seguridad en el nivel de fila; solo se muestran las filas para las que disponga de permiso para ver.  
  
  
