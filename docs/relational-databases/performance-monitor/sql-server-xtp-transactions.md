---
title: Transacciones XTP de SQL Server | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento Transacciones XTP de SQL Server, que contiene contadores relacionados con las transacciones de OLTP en memoria de SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: 443d67e4-1c7f-41d7-b18d-2d657f58c22a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 84d00aa0fc9f9677af96604788dd374e8d08e92d
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505460"
---
# <a name="sql-server-xtp-transactions"></a>Transacciones XTP de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  El objeto de rendimiento Transacciones XTP de SQL Server contiene contadores relacionados con las transacciones de OLTP en memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 En esta tabla se describen los contadores de **Transacciones XTP de SQL Server** .  
  
|Contador|Descripción|  
|-------------|-----------------|  
|**Anulaciones en cascada/s**|Número de transacciones revertidas debido a una reversión de dependencia de confirmación (en promedio), por segundo.|  
|**Dependencias de confirmación realizadas/s**|Número de dependencias de confirmación realizadas por transacciones (en promedio), por segundo.|  
|**Transacciones de solo lectura preparadas/s**|Número de transacciones de solo lectura que se prepararon para el procesamiento de confirmaciones, por segundo.|  
|**Actualizaciones de punto de retorno/s**|Número de veces que se "actualizó" un punto de retorno (en promedio), por segundo. Una actualización de punto de retorno es cuando un punto de retorno existente se restablece al momento actual de la duración de la transacción.|  
|**Reversiones de punto de retorno/s**|Número de veces que se revertió una transacción a un punto de retorno (en promedio), por segundo.|  
|**Puntos de retorno creados/s**|Número de puntos de retorno creados (en promedio), por segundo.|  
|**Errores de validación de transacciones/s**|Número de transacciones que produjeron error en el procesamiento de validación (en promedio), por segundo.|  
|**Transacciones anuladas por el usuario/s**|Número de transacciones que el usuario anuló (en promedio), por segundo.|  
|**Transacciones anuladas/s**|Número de transacciones anuladas (tanto por el usuario como por el sistema, en promedio), por segundo.|  
|**Transacciones creadas/s**|Número de transacciones creadas en el sistema (en promedio), por segundo.<br /><br /> Las transacciones XTP se cuentan de manera distinta que las transacciones basadas en disco (según se refleja en Bases de datos: Transacciones/seg). Por ejemplo, Transacciones creadas/seg cuenta las transacciones de solo lectura, mientras que Bases de datos: Transacciones/seg no.|  
  
## <a name="see-also"></a>Consulte también  
 [Contadores de rendimiento de XTP &#40;OLTP en memoria&#41;](../../relational-databases/performance-monitor/sql-server-xtp-in-memory-oltp-performance-counters.md)  
  
  
