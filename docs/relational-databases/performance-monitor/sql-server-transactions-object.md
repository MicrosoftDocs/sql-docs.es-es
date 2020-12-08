---
title: Transactions (objeto de SQL Server) | Microsoft Docs
description: Obtenga información sobre el objeto Transactions, que proporciona contadores para supervisar las transacciones activas en el Motor de base de datos y los efectos de las transacciones en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Transactions
- Transactions object
ms.assetid: 85240267-78fd-476a-9ef6-010d6cf32dd8
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 2f86c13a6f4cc6672b457653f6d923b63245fbc7
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505551"
---
# <a name="sql-server-transactions-object"></a>Transactions (objeto de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El objeto **Transactions** de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proporciona contadores para supervisar el número de transacciones activas en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] y las consecuencias de estas transacciones en recursos, como el almacén de versiones de fila de aislamiento de instantáneas en **tempdb**. Las transacciones son unidades de trabajo lógicas; un conjunto de operaciones que deben ser todas correctas o se deben borrar de una base de datos para mantener la integridad lógica de los datos. Todas las modificaciones de datos en bases de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se realizan en transacciones.  
  
 Cuando una base de datos se establece para permitir el nivel de aislamiento de instantáneas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe mantener un registro de las modificaciones realizadas en cada fila de una base de datos. Cada vez que se modifica una fila, se registra una copia de la fila anterior a la modificación en un almacén de versiones de fila en **tempdb**. Muchos de los contadores del objeto **Transactions** se pueden utilizar para supervisar el tamaño y la tasa de crecimiento del almacén de versiones de fila en **tempdb**.  
  
 Los contadores del objeto **Transactions** informan de todas las transacciones en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 Esta tabla describe los contadores de **SQLServer:Transactions** .  
  
|Contadores de SQLServer:Transactions|Descripción|  
|--------------------------------------|-----------------|  
|**Espacio disponible en tempdb (KB)**|Cantidad de espacio (en kilobytes) disponible en **tempdb**. Debe haber suficiente espacio disponible para incluir el almacén de versiones de nivel de aislamiento de instantáneas y todos los objetos temporales nuevos creados en esta instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].|  
|**Tiempo mayor de ejecución de transacción**|Tiempo (en segundos) transcurrido desde el inicio de la transacción que ha estado activa durante más tiempo que las demás transacciones actuales. Este contador solo muestra actividad cuando la base de datos está en el nivel de aislamiento de instantánea de lectura confirmada activa. No registra ninguna actividad si la base de datos está en cualquier otro nivel de aislamiento.|  
|**Transacciones de versión que no son instantáneas**|Número de transacciones activas actualmente que no utilizan el nivel de aislamiento de instantáneas y que han realizado modificaciones en los datos que, a su vez, han generado versiones de filas en el almacén de versiones de **tempdb** .|  
|**Transacciones de instantáneas**|Número de transacciones activas actualmente que utilizan el nivel de aislamiento de instantáneas.<br /><br /> Nota: El contador de objetos **Transacciones de instantáneas** responde cuando se produce el primer acceso a los datos, no cuando se emite la instrucción `BEGIN TRANSACTION`.|  
|**Transactions**|Número de transacciones activas actualmente de todos los tipos.|  
|**Frecuencia de conflictos de actualización**|Porcentaje de transacciones que utilizan el nivel de aislamiento de instantáneas y que han experimentado conflictos de actualización durante el último segundo. Un conflicto de actualización se produce cuando una transacción de nivel de aislamiento de instantáneas intenta modificar una fila ya modificada por otra transacción y que no se había confirmado al iniciar la transacción de nivel de aislamiento de instantáneas.|  
|**Base de frecuencia de conflictos de actualización**|Solo para uso interno.|
|**Transacciones de instantáneas de actualización**|Número de transacciones activas actualmente que utilizan el nivel de aislamiento de instantáneas y que tienen datos modificados.|  
|**Velocidad de limpieza de versión (KB/seg.)**|Velocidad (en kilobytes por segundo) a la que las versiones de filas se quitan del almacén de versiones de aislamiento de instantáneas en **tempdb**.|  
|**Velocidad de generación de versión (KB/seg.)**|Velocidad (en kilobytes por segundo) a la que se agregan nuevas versiones de fila al almacén de versiones de aislamiento de instantáneas en **tempdb**.|  
|**Tamaño de almacén de versión (KB)**|Cantidad de espacio (en kilobytes) en **tempdb** que se usa para almacenar las versiones de fila de nivel de aislamiento de instantáneas.|  
|**Recuento de unidad de almacén de versión**|Número de unidades de asignación activa en el almacén de versiones de aislamiento de instantáneas de **tempdb**.|  
|**Creación de unidad de almacén de versión**|Número de unidades de asignación creadas en el almacén de aislamiento de instantáneas desde que se inició la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
|**Truncamiento de unidad de almacén de versión**|Número de unidades de asignación quitadas del almacén de aislamiento de instantáneas desde que se inició la instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)] .|  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar el uso de recursos&#40;Monitor de sistema&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
