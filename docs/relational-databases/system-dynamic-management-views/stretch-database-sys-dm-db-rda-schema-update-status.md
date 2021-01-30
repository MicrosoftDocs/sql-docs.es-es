---
title: sys.dm_db_rda_schema_update_status (Transact-SQL) | Microsoft Docs
description: Obtenga información acerca de cómo sys.dm_db_rda_schema_update_status contiene una fila para cada tarea de actualización de esquema para el archivo de datos remoto de cada tabla habilitada para stretch en la base de datos.
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: stored-procedures
ms.topic: reference
f1_keywords:
- sys.dm_db_rda_schema_update_status
- sys.dm_db_rda_schema_update_status_TSQL
- dm_db_rda_schema_update_status
- dm_db_rda_schema_update_status_TSQL
helpviewer_keywords:
- sys.dm_db_rda_schema_update_status dynamic management view
ms.assetid: 364e3caa-a7c6-4be5-a029-0b19da34de3e
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 955f06ba0a7ad991ffe979fc927dfabe6e5a8dd0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202417"
---
# <a name="stretch-database---sysdm_db_rda_schema_update_status"></a>Stretch Database sys.dm_db_rda_schema_update_status
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Contiene una fila por cada tarea de actualización de esquema para el archivo de datos remoto de cada tabla habilitada para stretch en la base de datos actual. Las tareas se identifican por sus identificadores de tarea.  
  
 **dm_db_rda_schema_update_status** está en el ámbito del contexto de la base de datos actual. Asegúrese de que se encuentra en el contexto de la base de datos de la tabla habilitada para stretch para la que desea ver el estado de actualización del esquema.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**table_id**|**int**|IDENTIFICADOR de la tabla habilitada para Stretch local cuyo esquema de archivo de datos remotos se está actualizando.|  
|**database_id**|**int**|IDENTIFICADOR de la base de datos que contiene la tabla habilitada para Stretch local.|  
|**task_id**|**bigint**|El ID. de la tarea de actualización de esquema del archivo de datos remoto.|  
|**task_type**|**int**|El tipo de la tarea de actualización de esquema del archivo de datos remoto.|  
|**task_type_desc**|**nvarchar**|Descripción del tipo de la tarea de actualización de esquema del archivo de datos remoto.|  
|**task_state**|**int**|El estado de la tarea de actualización de esquema del archivo de datos remoto.|  
|**task_state_des**|**nvarchar**|La descripción del estado de la tarea de actualización de esquema del archivo de datos remoto.|  
|**start_time_utc**|**datetime**|Hora UTC a la que se inició la actualización del esquema del archivo de datos remotos.|  
|**end_time_utc**|**datetime**|Hora UTC a la que finalizó la actualización del esquema del archivo de datos remotos.|  
|**error_number**|**int**|Si se produce un error en la actualización del esquema del archivo de datos remoto, el número de error del error que se produjo; de lo contrario, es NULL.|  
|**error_severity**|**int**|Si se produce un error en la actualización del esquema del archivo de datos remoto, la gravedad del error que se produjo; de lo contrario, es NULL.|  
|**error_state**|**int**|Si se produce un error en la actualización del esquema del archivo de datos remoto, el estado del error que se produjo; de lo contrario, es NULL. El error_state indica la condición o la ubicación donde se produjo el error.|  
  
## <a name="see-also"></a>Consulte también  
 [Stretch Database](../../sql-server/stretch-database/stretch-database.md)  
  
  
