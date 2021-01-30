---
description: sys.dm_pdw_wait_stats (Transact-SQL)
title: sys.dm_pdw_wait_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: cfb8d905-c34f-44de-9574-dde81e170916
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 962841e0c8b40a2cd0443cc2d75eb2b11c21a4ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99138565"
---
# <a name="sysdm_pdw_wait_stats-transact-sql"></a>sys.dm_pdw_wait_stats (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene información relacionada con el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Estado del sistema operativo relacionado con las instancias que se ejecutan en los distintos nodos. Para obtener una lista de los tipos de esperas y su descripción, vea [Sys.dm_os_wait_stats](https://msdn.microsoft.com/library/ms179984\(v=sql.120\).aspx).  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|**pdw_node_id**|**int**|IDENTIFICADOR del nodo al que hace referencia esta entrada.||  
|**wait_name**|**nvarchar(255)**|Nombre del tipo de espera.||  
|**max_wait_time**|**bigint**|Tiempo de espera máximo de este tipo de espera.||  
|**request_count**|**bigint**|Número de esperas pendientes de este tipo de espera.||  
|**signal_time**|**bigint**|Diferencia entre el momento en que se indicó el subproceso en espera y el momento en que empezó a ejecutarse.||  
|**completed_count**|**bigint**|Número total de esperas de este tipo completadas desde el último reinicio del servidor.||  
|**wait_time**|**bigint**|Tiempo de espera total para este tipo de espera en millisecons. Inclusivo de signal_time.||  
  
## <a name="see-also"></a>Consulte también  
 [Azure Synapse Analytics y vistas de administración dinámica de almacenamiento de datos paralelos &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-and-parallel-data-warehouse-dynamic-management-views.md)   
 [sys.dm_pdw_waits &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql.md)  
  
  
