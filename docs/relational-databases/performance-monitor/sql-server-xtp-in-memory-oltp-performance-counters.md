---
title: Contadores de rendimiento de XTP (OLTP en memoria)
description: SQL Server ofrece objetos y contadores que el Monitor de rendimiento puede usar para supervisar la actividad de OLTP en memoria.
ms.custom: seo-dt-2019
ms.date: 04/06/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
ms.assetid: fe3cbaf4-65f4-44c5-acc6-7b735cda0c5d
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4c6e97837d26e680f7f94bcdc60132127a11afe4
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237720"
---
# <a name="sql-server-xtp-in-memory-oltp-performance-counters"></a>Contadores de rendimiento de XTP (OLTP en memoria) de SQL Server
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ofrece objetos y contadores que el Monitor de rendimiento puede usar para supervisar la actividad de OLTP en memoria. Los objetos y los contadores se comparten entre todas las instancias de una versión concreta de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el equipo, a partir de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Antes, los nombres de los objetos y los contadores contenían *XTP*, como en **Cursores XTP**. A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], los nombres siguen este patrón:  
  
-   **Cursores XTP** *\<version>* de **SQL Server**  
  
 Para *\<version>* , el valor es similar a 2016.  
  
##  <a name="sql-server-xtp-performance-objects"></a><a name="SQLServerPOs"></a> Objetos de rendimiento de SQL Server XTP  
 En la tabla siguiente se describen los objetos de rendimiento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Objeto de rendimiento|Descripción|  
|------------------------|-----------------|  
|[Cursores SQL Server XTP](../../relational-databases/performance-monitor/sql-server-xtp-cursors.md)|El objeto de rendimiento Cursores XTP de SQL Server contiene contadores relacionados con los cursores internos del motor OLTP en memoria. Los cursores son los bloques de creación de bajo nivel que usa el motor OLTP en memoria para procesar consultas de [!INCLUDE[tsql](../../includes/tsql-md.md)] . Por tanto, el usuario no tiene normalmente control directo sobre ellos.|  
|[Bases de datos SQL Server XTP](../../relational-databases/performance-monitor/sql-server-xtp-databases.md)|El objeto de rendimiento SQL Server XTP Databases contiene contadores relacionados con las bases de datos del motor OLTP en memoria.|  
|[Recolección de elementos no utilizados de XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-garbage-collection.md)|El objeto de rendimiento Recolección de elementos no utilizados de XTP de SQL Server contiene contadores relacionados con el recolector de elementos no utilizados del motor OLTP en memoria.|  
|[SQL Server 2016 XTP IO Governor](../../relational-databases/performance-monitor/sql-server-xtp-io-governor.md)|El objeto de rendimiento SQL Server XTP IO Governor contiene contadores relacionados con el regulador de frecuencia de E/S OLPT en memoria.|
|[Procesador fantasma de XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-phantom-processor.md)|El objeto de rendimiento Procesador fantasma de XTP de SQL Server contiene contadores relacionados con el subsistema de procesamiento fantasma del motor de OLTP en memoria. Este componente es responsable de detectar filas fantasma en transacciones que se ejecutan en el nivel de aislamiento SERIALIZABLE.|  
|[XTP Storage de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-storage.md)|El objeto de rendimiento Almacenamiento de XTP de SQL Server contiene contadores relacionados con el almacenamiento OLTP en memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Registro de transacciones XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transaction-log.md)|El objeto de rendimiento Registro de transacciones XTP de SQL Server contiene contadores relacionados con el registro de transacciones OLTP en memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|[Transacciones XTP de SQL Server](../../relational-databases/performance-monitor/sql-server-xtp-transactions.md)|El objeto de rendimiento Transacciones XTP de SQL Server contiene contadores relacionados con las transacciones del motor OLTP en memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
  
  
