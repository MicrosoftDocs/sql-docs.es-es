---
description: sys.dm_xtp_system_memory_consumers (Transact-SQL)
title: sys.dm_xtp_system_memory_consumers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_xtp_system_memory_consumers
- sys.dm_xtp_system_memory_consumers_TSQL
- sys.dm_xtp_system_memory_consumers
- dm_xtp_system_memory_consumers_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xtp_system_memory_consumers dynamic management view
ms.assetid: 9eb0dd82-7920-42e0-9e50-7ce6e7ecee8b
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 106b028c5c625df2643fdebc7f0168e25fda30ba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99127279"
---
# <a name="sysdm_xtp_system_memory_consumers-transact-sql"></a>sys.dm_xtp_system_memory_consumers (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Notifica a los consumidores de memoria de nivel de sistema para [!INCLUDE[hek_2](../../includes/hek-2-md.md)]. La memoria para estos consumidores procede del grupo predeterminado (cuando la asignación está en el contexto de un subproceso de usuario) o del grupo interno (si la asignación está en el contexto de un subproceso del sistema).  
  
```  
-- system memory consumers @ instance  
select * from sys.dm_xtp_system_memory_consumers  
```  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|memory_consumer_id|**bigint**|Identificador interno del consumidor de memoria.|  
|memory_consumer_type|**int**|Entero que representa el tipo de consumidor de memoria con uno de los valores siguientes:<br /><br /> 0: no debe mostrarse. Agrega el uso de memoria de dos o varios consumidores.<br /><br /> 1-pista: realiza un seguimiento del consumo de memoria de una traducción del sistema.<br /><br /> 2-VARHEAP: realiza el seguimiento del consumo de memoria para un montón de longitud variable.<br /><br /> 4-Grupo de páginas de e/s: realiza un seguimiento del consumo de memoria de un grupo de páginas del sistema utilizado para operaciones de e/s.|  
|memory_consumer_type_desc|**nvarchar (16)**|Descripción del tipo de consumidor de memoria:<br /><br /> 0: no debe mostrarse.<br /><br /> 1-ALTA<br /><br /> 2: VARHEAP<br /><br /> 4: PGPOOL|  
|memory_consumer_desc|**nvarchar (64)**|Descripción de la instancia del consumidor de memoria:<br /><br /> VARHEAP <br />Montón del sistema. Uso general. Actualmente solo se usa para asignar elementos de trabajo de la recolección de elementos no utilizados.<br />O<br />Montón de lista de direcciones. Lo usan las listas de direcciones cuando el número de elementos contenidos en la lista alcanza un extremo predeterminado (normalmente alrededor de 5.000 elementos).<br /><br /> PGPOOL: para los grupos del sistema de e/s hay tres tamaños distintos: Grupo de páginas de System 4K, grupo de páginas del sistema 64 K y grupo de páginas del sistema 256 K.|  
|lookaside_id|**bigint**|El identificador del proveedor de memoria de direcciones local del subproceso.|  
|pagepool_id|**bigint**|El identificador del subproceso local, proveedor de memoria del grupo de páginas.|  
|allocated_bytes|**bigint**|Número de bytes reservados para el consumidor.|  
|used_bytes|**bigint**|Bytes utilizados por el consumidor. Solo se aplica a los consumidores de memoria varheap.|  
|allocation_count|**int**|Número de asignaciones.|  
|partition_count|**int**|Solo para uso interno.|  
|sizeclass_count|**int**|Solo para uso interno.|  
|min_sizeclass|**int**|Solo para uso interno.|  
|max_sizeclass|**int**|Solo para uso interno.|  
|memory_consumer_address|**varbinary**|Dirección interna del consumidor.|  
  
## <a name="permissions"></a>Permisos  
 Requiere los permisos VIEW SERVER STATE en el servidor.  
  
## <a name="user-scenario"></a>Escenario de usuario  
  
```  
-- system memory consumers @ instance  
selectmemory_consumer_type_desc,   
allocated_bytes/1024 as allocated_bytes_kb,   
used_bytes/1024 as used_bytes_kb, allocation_count  
from sys.dm_xtp_system_memory_consumers  
```  
  
 El resultado muestra todos los consumidores de memoria en el nivel de sistema. Por ejemplo, hay consumidores para el registro de transacciones.  
  
```  
memory_consumer_type_name           memory_consumer_desc                           allocated_bytes_kb   used_bytes_kb        allocation_count  
-------------------------------          ---------------------                          -------------------  --------------        ----------------  
VARHEAP                                  Lookaside heap                                 0                    0                    0  
VARHEAP                                  System heap                                    768                  0                    2  
LOOKASIDE                                GC transaction map entry                       64                   64                   910  
LOOKASIDE                                Redo transaction map entry                     128                  128                  1260  
LOOKASIDE                                Recovery table cache entry                     448                  448                  8192  
LOOKASIDE                                Transaction recent rows                        3264                 3264                 4444  
LOOKASIDE                                Range cursor                                   0                    0                    0  
LOOKASIDE                                Hash cursor                                    3200                 3200                 11070  
LOOKASIDE                                Transaction save-point set entry               0                    0                    0  
LOOKASIDE                                Transaction partially-inserted rows set        704                  704                  1287  
LOOKASIDE                                Transaction constraint set                     576                  576                  1940  
LOOKASIDE                                Transaction save-point set                     0                    0                    0  
LOOKASIDE                                Transaction write set                          704                  704                  672  
LOOKASIDE                                Transaction scan set                           320                  320                  156  
LOOKASIDE                                Transaction read set                           704                  704                  343  
LOOKASIDE                                Transaction                                    4288                 4288                 1459  
PGPOOL                                   System 256K page pool                          5120                 5120                 20  
PGPOOL                                   System 64K page pool                           0                    0                    0  
PGPOOL                                   System 4K page pool                            24                   24                   6  
```  
  
 Para ver la memoria total que consumen los asignadores del sistema:  
  
```  
select sum(allocated_bytes)/(1024*1024) as total_allocated_MB, sum(used_bytes)/(1024*1024) as total_used_MB   
from sys.dm_xtp_system_memory_consumers  
  
total_allocated_MB   total_used_MB  
-------------------- --------------------  
2                    2  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
