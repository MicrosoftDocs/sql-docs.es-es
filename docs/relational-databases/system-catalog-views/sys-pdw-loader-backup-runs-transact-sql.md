---
description: sys.pdw_loader_backup_runs (Transact-SQL)
title: sys.pdw_loader_backup_runs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 2b72034c-6a11-46b9-a76c-7a88b2bea360
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 12fcde5ded873393a6273a43772b86376f5df607
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208372"
---
# <a name="syspdw_loader_backup_runs-transact-sql"></a>sys.pdw_loader_backup_runs (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información acerca de las operaciones de copia de seguridad y restauración en curso y completadas en y [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] sobre las operaciones de copia de seguridad, restauración y carga continuadas en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] . La información se conserva entre reinicios del sistema.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|run_id|**int**|Identificador único para una copia de seguridad, restauración o ejecución de carga específica.<br /><br /> Clave para esta vista.||  
|name|**nvarchar(255)**|Null para la carga. Nombre opcional para la copia de seguridad o restauración.||  
|submit_time|**datetime**|Hora a la que se envió la solicitud.||  
|start_time|**datetime**|Hora en que se inició la operación.||  
|end_time|**datetime**|Hora de finalización de la operación, error o cancelada.||  
|total_elapsed_time|**int**|Tiempo total transcurrido entre el start_time y la hora actual, o entre start_time y end_time para las ejecuciones completadas, canceladas o con errores.|Si total_elapsed_time supera el valor máximo de un entero (24,8 días en milisegundos), se producirá un error de materialización debido al desbordamiento.<br /><br /> El valor máximo en milisegundos es equivalente a 24,8 días.|  
|operation_type|**nvarchar (16)**|Tipo de carga.|' BACKUP ', ' LOAD ', ' RESTORE '|  
|mode|**nvarchar (16)**|Modo en el tipo de ejecución.|Por operation_type = **copia de seguridad**<br />**DIFFERENTIAL**<br />**FULL**<br /><br /> Por operation_type = **Load**<br />**ANEXAR**<br />**VOLVER A cargar**<br />**UPSERT**<br /><br /> Por operation_type = **restore**<br />**DATABASE**<br />**HEADER_ONLY**|  
|database_name|**nvarchar(255)**|Nombre de la base de datos que es el contexto de esta operación.||  
|table_name|**nvarchar(255)**|[!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]||  
|Principal_id|**int**|IDENTIFICADOR del usuario que solicita la operación.||  
|session_id|**nvarchar(32)**|IDENTIFICADOR de la sesión que realiza la operación.|Vea session_id en [sys.dm_pdw_exec_sessions &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md).|  
|request_id|**nvarchar(32)**|IDENTIFICADOR de la solicitud que realiza la operación. En el caso de las cargas, esta es la solicitud actual o última asociada a esta carga.|Vea request_id en [sys.dm_pdw_exec_requests &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql.md).|  
|status|**nvarchar (16)**|Estado de la ejecución.|' CANCELADO ', ' COMPLETED ', ' FAILED ', ' QUEUED ', ' RUNNING '|  
|progreso|**int**|Porcentaje completado.|De 0 a 100|  
|.|**nvarchar(4000)**|Texto completo del comando enviado por el usuario.|Se truncará si tiene más de 4000 caracteres (recuento de espacios).|  
|rows_processed|**bigint**|Número de filas procesadas como parte de esta operación.||  
|rows_rejected|**bigint**|Número de filas rechazadas como parte de esta operación.||  
|rows_inserted|**bigint**|Número de filas insertadas en las tablas de base de datos como parte de esta operación.||  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
