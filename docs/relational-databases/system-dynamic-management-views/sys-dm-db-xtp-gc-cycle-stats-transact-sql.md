---
description: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
title: sys.dm_db_xtp_gc_cycle_stats (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/29/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_db_xtp_gc_cycle_stats_TSQL
- dm_db_xtp_gc_cycle_stats
- sys.dm_db_xtp_gc_cycle_stats_TSQL
- sys.dm_db_xtp_gc_cycle_stats
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_db_xtp_gc_cycle_stats dynamic management view
ms.assetid: bbc9704e-158e-4d32-b693-f00dce31cd2f
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea5374390de8b1e07ef796d18062787029e90fa6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160165"
---
# <a name="sysdm_db_xtp_gc_cycle_stats-transact-sql"></a>sys.dm_db_xtp_gc_cycle_stats (Transact-SQL)
[!INCLUDE[sql-asdb-asdbmi](../../includes/applies-to-version/sql-asdb-asdbmi.md)]

  Genera el estado actual de las transacciones confirmadas que han eliminado una o varias filas. El subproceso principal de recolección de elementos no utilizados se reactiva cada minuto o cuando el número de transacciones DML confirmadas supera un umbral interno desde el último ciclo de recolección de elementos no utilizados. Como parte del ciclo de recolección de elementos no utilizados, mueve las transacciones que se han confirmado a una o varias colas asociadas con las generaciones. Las transacciones que han generado versiones obsoletas se agrupan en una unidad de 16 transacciones en 16 generaciones de la manera siguiente:  
  
-   Generación 0: almacena todas las transacciones que se confirmaron antes que la transacción activa más antigua. Las versiones de fila generadas por estas transacciones están disponibles inmediatamente para la recolección de elementos no utilizados.  
  
-   Generaciones 1 a 14: almacena las transacciones cuya marca de tiempo es mayor que la transacción activa más antigua. No se pueden recolectar las versiones de fila no utilizadas. Cada generación puede contener hasta 16 transacciones. En estas generaciones pueden existir un total de 224 (14 * 16) transacciones.  
  
-   Generación 15: las transacciones restantes cuya marca de tiempo es mayor que la transacción activa más antigua van a la generación 15. Al igual que ocurre con la generación 0, no hay ningún límite en cuanto al número de transacciones de la generación 15.  
  
 Cuando hay presión de memoria, el subproceso de recolección de elementos no utilizados actualiza la sugerencia de la transacción activa más antigua de forma absoluta, lo que fuerza la recolección de elementos no utilizados.  
  
 Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
  
|Nombre de la columna|Tipo|Descripción|  
|-----------------|----------|-----------------|  
|cycle_id|**bigint**|Identificador único del ciclo de recolección de elementos no utilizados.|  
|ticks_at_cycle_start|**bigint**|Tics en el momento en el que se inició el ciclo.|  
|ticks_at_cycle_end|**bigint**|Tics en el momento en el que finalizó el ciclo.|  
|base_generation|**bigint**|El valor de generación base actual de la base de datos. Representa la marca de tiempo de la transacción activa más antigua usada para identificar transacciones para la recolección de elementos no utilizados. El identificador de la transacción activa más antigua se actualiza en incrementos de 16. Por ejemplo, si tiene identificadores de transacción como 124, 125, 126... 139, el valor será 124. Cuando agregue otra transacción, por ejemplo 140, el valor será 140.|  
|xacts_copied_to_local|**bigint**|El número de transacciones copiadas de la canalización de transacciones en la matriz de generación de la base de datos.|  
|xacts_in_gen_0- xacts_in_gen_15|**bigint**|Número de transacciones de cada generación.|  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DATABASE STATE en el servidor.  
  
## <a name="usage-scenario"></a>Escenario de uso  
 A continuación se muestra un resultado de ejemplo con un subconjunto de columnas, donde se muestran 27 generaciones:  
  
```  
cycle_id   ticks_at_cycle_start ticks_at_cycle_end   base_generation  xacts_in_gen_0    xacts_in_gen_1  
  
1          123160509            123160509            1                    0                    0  
2          123176822            123176822            1                    0                    1  
3          123236826            123236826            1                    0                    1  
4          123296829            123296829            1                    0                    1  
5          123356832            123356941            129                  0                    0  
6          123357473            123357473            129                  0                    0  
7          123417486            123417486            129                  0                    0  
8          123477489            123477489            129                  0                    0  
9          123537492            123537492            129                  0                    0  
10         123597500            123597500            129                  0                    0  
11         123657504            123657504            129                  0                    0  
12         123717507            123717507            129                  0                    0  
13         123777510            123777510            129                  0                    0  
14         123837513            123837513            129                  0                    0  
15         123897516            123897516            129                  0                    0  
16         123957516            123957516            129                  0                    0  
17         124017516            124017516            129                  0                    0  
18         124077517            124077517            129                  0                    0  
19         124137517            124137517            129                  0                    0  
20         124197518            124197518            129                  0                    0  
21         124257518            124257518            129                  0                    0  
22         124317523            124317523            129                  0                    0  
23         124377526            124377526            129                  0                    0  
24         124437529            124437529            129                  0                    0  
25         124497533            124497533            129                  0                    0  
26         124557536            124557536            129                  0                    0  
27         124617539            124617539            129                  0                    0  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de administración dinámica de tablas optimizadas para memoria &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/memory-optimized-table-dynamic-management-views-transact-sql.md)  
  
  
