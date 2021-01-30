---
description: MSmerge_sessions (Transact-SQL)
title: MSmerge_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSmerge_sessions
- MSmerge_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSmerge_sessions system table
ms.assetid: 09ada8fc-c148-4379-9524-7826b1b0216c
author: cawrites
ms.author: chadam
ms.openlocfilehash: af57f9ae760c09fa14bffe42ceba987734b7c5ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200287"
---
# <a name="msmerge_sessions-transact-sql"></a>MSmerge_sessions (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSmerge_sessions** contiene filas de historial con los resultados de las sesiones de trabajos de agente de mezcla anteriores. Cada vez que se ejecuta el Agente de mezcla, se agrega una nueva fila a esta tabla. Esta tabla se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**session_id**|**int**|Id. de la sesión de trabajo del Agente de mezcla.|  
|**agent_id**|**int**|El Id. del Agente de mezcla.|  
|**start_time**|**datetime**|Hora a la que comenzó la ejecución del trabajo.|  
|**end_time**|**datetime**|Hora a la que terminó la ejecución del trabajo.|  
|**duration**|**int**|La duración acumulada, en segundos, de esta sesión de trabajo.|  
|**delivery_time**|**int**|Número de segundos que se ha tardado en aplicar un lote de cambios.|  
|**upload_time**|**int**|Número de segundos que se ha tardado en cargar los cambios en el publicador.|  
|**download_time**|**int**|Número de segundos que se ha tardado en descargar los cambios en el suscriptor.|  
|**delivery_rate**|**float**|Promedio de comandos realizados por segundo.|  
|**time_remaining**|**int**|Número estimado de segundos que quedan en una sesión activa.|  
|**percent_complete**|**decimal**|Porcentaje estimado de los cambios totales que ya se han realizado en una sesión activa.|  
|**upload_inserts**|**int**|Número de inserciones aplicadas en el publicador.|  
|**upload_updates**|**int**|Número de actualizaciones aplicadas en el publicador.|  
|**upload_deletes**|**int**|Número de eliminaciones aplicadas en el publicador.|  
|**upload_conflicts**|**int**|Número de conflictos que se han producido al aplicar los cambios en el publicador.|  
|**upload_conflicts_resolved**|**int**|Número de conflictos que se han producido al aplicar los cambios en el publicador y que se han resuelto.|  
|**upload_rows_retried**|**int**|Número de filas que se han reintentado al cargarse en el publicador.|  
|**download_inserts**|**int**|Número de inserciones aplicadas en el suscriptor.|  
|**download_updates**|**int**|Número de actualizaciones aplicadas en el suscriptor.|  
|**download_deletes**|**int**|Número de eliminaciones aplicadas en el suscriptor.|  
|**download_conflicts**|**int**|Número de conflictos que se han producido al aplicar los cambios en el suscriptor.|  
|**download_conflicts_resolved**|**int**|Número de conflictos que se han producido al aplicar los cambios en el suscriptor y que se han resuelto.|  
|**download_rows_retried**|**int**|Número de filas que se han reintentado al descargarse en el suscriptor.|  
|**schema_changes**|**int**|Número de cambios de esquema aplicados en la sesión.|  
|**metadata_rows_cleanedup**|**int**|Número de filas de metadatos limpiados en la sesión.|  
|**runstatus**|**int**|Estado de ejecución:<br /><br /> **1** = Inicio.<br /><br /> **2** = correcto.<br /><br /> **3** = en curso.<br /><br /> **4** = inactivo.<br /><br /> **5** = Reintentar.<br /><br /> **6** = error.|  
|**estimated_upload_changes**|**int**|Número estimado de cambios que es necesario aplicar en el publicador.|  
|**estimated_download_changes**|**int**|Número estimado de cambios que es necesario aplicar en el suscriptor.|  
|**connection_type**|**int**|Conexión utilizada en la carga:<br /><br /> **1** = red de área local (LAN).<br /><br /> **2** = conexión de acceso telefónico a redes.<br /><br /> **3** = sincronización Web.|  
|**timestamp**|**timestamp**|La columna de marca de tiempo de esta tabla.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
