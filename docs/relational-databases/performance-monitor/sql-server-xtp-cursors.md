---
title: Cursores XTP de SQL Server | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento Cursores XTP de SQL Server, que contiene contadores relacionados con los cursores internos del motor OLTP en memoria.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 84bf4654-3ef7-4d7f-a269-c8bb4ed4acad
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 39c491efc7b0001b7f6b44bda127e7b128c31851
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505534"
---
# <a name="sql-server-xtp-cursors"></a>Cursores SQL Server XTP
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  El objeto de rendimiento Cursores XTP de SQL Server contiene contadores relacionados con los cursores internos del motor OLTP en memoria. Los cursores son los bloques de creación de bajo nivel que usa el motor OLTP en memoria para procesar consultas de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Por tanto, el usuario no tiene normalmente control directo sobre ellos.  
  
 En esta tabla se describen los contadores de **Cursores XTP** .  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Eliminaciones de cursor/s**|Número de eliminaciones de cursor (en promedio), por segundo.|  
|**Inserciones de cursor/s**|Número de inserciones de cursor (en promedio), por segundo.|  
|**Recorridos de cursor iniciados/s**|Número de recorridos de cursor iniciados (en promedio), por segundo.|  
|**Infracciones de restricciones UNIQUE de cursor/s**|Número de infracciones de restricciones UNIQUE (en promedio), por segundo.|  
|**Actualizaciones de cursor/s**|Número de actualizaciones de cursor (en promedio), por segundo.|  
|**Conflictos de escritura de cursor/s**|Número de conflictos de escritura contra escritura en la misma versión de fila (en promedio), por segundo.|  
|**Reintentos de recorrido de esquinas sucias/s (emitidos por el usuario)**|Número de reintentos de recorrido debido a conflictos de escritura durante los rastreos de esquinas sucias emitidos por un recorrido de tabla completo del usuario (en promedio), por segundo. Es un contador de nivel muy bajo, no está pensado para uso de los clientes.|  
|**Filas expiradas quitadas/s**|Número de filas expiradas quitadas mediante cursores (en promedio), por segundo.|  
|**Filas expiradas tocadas/s**|Número de filas expiradas tocadas mediante cursores (en promedio), por segundo.|  
|**Filas devueltas/s**|Número de filas devueltas mediante cursores (en promedio), por segundo.|  
|**Filas tocadas/s**|Número de filas tocadas mediante cursores (en promedio), por segundo.|  
|**Filas tocadas eliminadas provisionalmente/s**|Número de filas que van a expirar tocadas mediante cursores (en promedio), por segundo. Una fila va a expirar si la transacción que la eliminó permanece activa (es decir, no se confirma o anula).|  
  
## <a name="see-also"></a>Consulte también  
 [Contadores de rendimiento de XTP &#40;OLTP en memoria&#41;](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
