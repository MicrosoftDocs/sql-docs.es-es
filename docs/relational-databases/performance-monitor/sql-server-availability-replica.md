---
title: SQL Server, réplica de disponibilidad | Microsoft Docs
description: Obtenga información sobre el objeto de rendimiento SQLServer:Availability Replica, que contiene contadores de rendimiento de las réplicas de disponibilidad en los grupos de disponibilidad AlwaysOn.
ms.custom: ''
ms.date: 08/25/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- performance counters [SQL Server], AlwaysOn Availability Groups
- SQLServer:Availability Replica
- Availability Groups [SQL Server], performance counters
ms.assetid: e402f996-c1fb-484a-b804-45c49972f2e0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 5e31767cec60df160d371efb342ecc411f1b5cc4
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049267"
---
# <a name="sql-server-availability-replica"></a>SQL Server, réplica de disponibilidad

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto de rendimiento **SQLServer:Availability Replica** contiene contadores de rendimiento que proporcionan información acerca de las réplicas de disponibilidad en los grupos de disponibilidad AlwaysOn en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)]. Todos los contadores de rendimiento de las réplicas de disponibilidad se aplican tanto a las réplicas principales como a las réplicas secundarias, y los contadores de envío/recepción reflejan la réplica local. En general, la réplica principal envía la mayor parte de los datos y las réplicas secundarias reciben los datos. Sin embargo, las réplicas secundarias envían mensajes de confirmación de reconocimiento y otro tráfico de fondo a las réplicas principales. Observe que en una réplica de disponibilidad dada, algunos contadores mostrarán el valor cero, dependiendo del rol actual (principal o secundario) de la réplica local.  
  
|Nombre del contador|Descripción|  
|------------------|-----------------|  
|**Bytes recibidos de la réplica/s**|**En SQL Server 2012 y 2014:** número real de bytes (comprimidos) recibidos de la réplica de disponibilidad por segundo (sincrónica o asincrónica). Las actualizaciones de estado y ping generará tráfico de red incluso en las bases de datos sin actualizaciones de usuario. <BR/> <BR/> **SQL Server 2016 y versiones posteriores:** número real de bytes recibidos (comprimidos para los casos asincrónicos, sin comprimir para los casos sincrónicos) de la réplica de disponibilidad por segundo.|  
|**Bytes enviados a la réplica/s**|**En SQL Server 2012 y 2014:** número real de bytes enviados (comprimidos) por segundo a través de la red a la réplica de disponibilidad remota (sincrónica o asincrónica). La compresión está habilitada de forma predeterminada tanto para réplicas sincrónicas como asincrónicas. <BR/> <BR/> **SQL Server 2016 y versiones posteriores:** Número de bytes enviados a la réplica de disponibilidad remota por segundo. Antes de la compresión de la réplica asincrónica. (Número real de bytes para la réplica sincrónica sin compresión).|  
|**Bytes enviados al transporte/s**|**En SQL Server 2012 y 2014:** número real de bytes enviados por segundo (comprimidos) a través de la red a la réplica de disponibilidad remota (sincrónica o asincrónica). La compresión está habilitada de forma predeterminada tanto para réplicas sincrónicas como asincrónicas. <BR/> <BR/> **SQL Server 2016 y versiones posteriores:** número de bytes enviados a la réplica de disponibilidad remota por segundo antes de la compresión para la réplica asincrónica. (Número real de bytes para la réplica sincrónica sin compresión).|  
|**Tiempo de control de flujo (ms/s)**|Tiempo en milisegundos que los mensajes de flujo de registro esperaron el control de flujo de envío en el último segundo.|  
|**Control de flujo/s**|Número de veces que se inició el control de flujo en el último segundo. **Tiempo de control de flujo (ms/s)** dividido entre **Control de flujo por segundo** es el tiempo medio por espera.|  
|**Recepciones de la réplica/s**|Número de mensajes de AlwaysOn recibidos de la réplica por segundo.|  
|**Mensajes reenviados/s**|Número de mensajes de AlwaysOn reenviados en el último segundo.|  
|**Envíos a la réplica/s**|Número de mensajes de AlwaysOn enviados a esta réplica de disponibilidad por segundo.|  
|**Envíos a transporte/s**|Número real de mensajes de AlwaysOn enviados por segundo a través de la red en la réplica de disponibilidad remota. En la réplica principal, se trata del número de mensajes enviados a la réplica secundaria. En la réplica secundaria, se trata del número de mensajes enviados a la réplica principal.|  
  
## <a name="see-also"></a>Consulte también 
 
 [Supervisar el uso de recursos &#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)   
 [SQL Server, réplica de base de datos](../../relational-databases/performance-monitor/sql-server-database-replica.md)   
 [Grupos de disponibilidad Always On (SQL Server)](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)  
  
  
