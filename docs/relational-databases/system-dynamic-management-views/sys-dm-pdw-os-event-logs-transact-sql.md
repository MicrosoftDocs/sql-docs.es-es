---
description: sys.dm_pdw_os_event_logs (Transact-SQL)
title: sys.dm_pdw_os_event_logs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: a0daa8cf-72e2-4349-8be1-d3cc0f9b1e02
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: bfc09d3ebaae6a7e78c327cc0b12d8327c7e6d00
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99140173"
---
# <a name="sysdm_pdw_os_event_logs-transact-sql"></a>sys.dm_pdw_os_event_logs (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Contiene información relacionada con los distintos registros de eventos de Windows en los diferentes nodos.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|pdw_node_id|**int**|Nodo del dispositivo del que procede este registro.<br /><br /> pdw_node_id y log_name forman la clave de esta vista.||  
|log_name|**nvarchar(255)**|Nombre del registro de eventos de Windows.<br /><br /> pdw_node_id y log_name forman la clave de esta vista.||  
|log_source|**nvarchar(255)**|Nombre del origen del registro de eventos de Windows.||  
|event_id|**int**|IDENTIFICADOR del evento. No es único.||  
|event_type|**nvarchar(255)**|Tipo del evento, que identifica la gravedad.|' Información ', ' ADVERTENCIA ', ' error '|  
|event_message|**nvarchar(4000)**|Detalles del evento.||  
|generate_time|**datetime**|Hora en que se creó el evento.||  
|write_time|**datetime**|Hora en que el evento se escribió realmente en el registro.||  
  
 Para obtener información acerca de las filas máximas retenidas en esta vista, consulte la sección de metadatos en el tema [límites de capacidad](/azure/sql-data-warehouse/sql-data-warehouse-service-capacity-limits#metadata) . 
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)  
  
  
