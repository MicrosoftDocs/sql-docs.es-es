---
title: Base de datos XTP de SQL Server | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento SQL Server XTP Databases, que contiene contadores específicos de las bases de datos de OLTP en memoria.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQL Server 2016 XTP Databases
ms.assetid: 488ff55e-173f-43f6-9bdb-67b35e7cebfe
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9b4e75e2033e0e73c592eb0e565038d514f73229
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505517"
---
# <a name="sql-server-xtp-databases"></a>SQL Server XTP Databases
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

El objeto de rendimiento **SQL Server XTP Databases** contiene contadores relacionados con las bases de datos del motor OLTP en memoria.

> [!NOTE]
>  Actualmente, los contadores de SQL Server XTP Databases no son visibles desde sys.dm_os_performance_counters.  Los contadores se pueden ver desde [Monitor del sistema](../../relational-databases/performance/start-system-monitor-windows.md).

En esta tabla se describen los contadores de **SQL Server XTP Databases** .

|Contador|Descripción| 
|-------------|-----------------|  
|**Avg Transaction Segment Large Data Size**|Tamaño promedio de la carga de datos grandes del segmento de transacciones. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**Avg Transaction Segment Size**|Tamaño promedio de la carga del segmento de transacciones. Si este valor llega a cero, se asignan más páginas desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**Flush Thread 256K Queue Depth**|Profundidad de la cola de subproceso de vaciado para solicitudes de E/S de 256 KB.|
|**Flush Thread 4K Queue Depth**|Profundidad de la cola de subproceso de vaciado para solicitudes de E/S de 4 KB.|
|**Flush Thread 64K Queue Depth**|Profundidad de la cola de subproceso de vaciado para solicitudes de E/S de 64 KB.|
|**Flush Thread Frozen IOs/sec (256K)**|El número de solicitudes de E/S de 256 KB que se encontraron durante el procesamiento de páginas de vaciado que se encuentran sobre el umbral de congelación y que, por lo tanto, no se pueden emitir.|
|**Flush Thread Frozen IOs/sec (4K)**|El número de solicitudes de E/S de 4 KB que se encontraron durante el procesamiento de páginas de vaciado que se encuentran sobre el umbral de congelación y que, por lo tanto, no se pueden emitir.|
|**Flush Thread Frozen IOs/sec (64K)**|El número de solicitudes de E/S de 64 KB que se encontraron durante el procesamiento de páginas de vaciado que se encuentran sobre el umbral de congelación y que, por lo tanto, no se pueden emitir.|
|**IoPagePool256K Free List Count**|Número de páginas en la lista disponible de bloque paginado de E/S de 256 KB. Si este valor llega a cero, se asignan más páginas desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**IoPagePool256K Total Allocated**|Número total de páginas asignadas y contenidas por el bloque paginado de E/S de 256 KB desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**IoPagePool4K Free List Count**|Número de páginas en la lista disponible de bloque paginado de E/S de 4 KB. Si este valor llega a cero, se asignan más páginas desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**IoPagePool4K Total Allocated**|Número total de páginas asignadas y contenidas por el bloque paginado de E/S de 4 KB desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**IoPagePool64K Free List Count**|Número de páginas en la lista disponible de bloque paginado de E/S de 64 KB. Si este valor llega a cero, se asignan más páginas desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**IoPagePool64K Total Allocated**|Número total de páginas asignadas y contenidas por el bloque paginado de E/S de 64 KB desde el asignador de backend. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 256K Expand Count**|Número de veces que se extendió un MtLog de 256 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 256K IOs Outstanding**|Número de solicitudes de E/S de 256 KB pendientes que emite MtLog.|
|**MtLog 256K Page Fill %/Page Flushed**|Porcentaje de relleno promedio de cada página MtLog de 256 KB vaciada. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 256K Write Bytes/sec**|Bytes de escritura por segundo en objetos de MtLog de 256 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 4K Expand Count**|Número de veces que se expandió un MtLog de 4 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 4K IOs Outstanding**|Número de solicitudes de E/S de 4 KB pendientes que emite MtLog.|
|**MtLog 4K Page Fill %/Page Flushed**|Porcentaje de relleno promedio de cada página MtLog de 4 KB vaciada. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 4K Write Bytes/sec**|Bytes de escritura por segundo en objetos de MtLog de 4 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 64K Expand Count**|Número de veces que se expandió un MtLog de 64 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 64K IOs Outstanding**|Número de solicitudes de E/S de 64 KB pendientes que emite MtLog.|
|**MtLog 64K Page Fill %/Page Flushed**|Porcentaje de relleno promedio de cada página MtLog de 64 KB vaciada. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**MtLog 64K Write Bytes/sec**|Bytes de escritura por segundo en objetos de MtLog de 64 KB. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**Num Merges**|El número de combinaciones en proceso.|
|**Num Merges/sec**|El número de combinaciones creadas por segundo (en promedio).|
|**Num Serializations**|El número de serializaciones en proceso.|
|**Num Serializations/sec**|El número de serializaciones creadas por segundo (en promedio).|
|**Tail Cache Page Count**|Número de páginas asignadas en la caché de cola. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|
|**Tail Cache Page Count Peak**|Número máximo de páginas asignadas en la caché de cola. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|


## <a name="see-also"></a>Consulte también  
[Contadores de rendimiento de XTP &#40;OLTP en memoria&#41;](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)
