---
description: Tablas de Agente SQL Server (Transact-SQL)
title: Tablas de Agente SQL Server (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- SQL Server Agent, system tables
- system tables [SQL Server], SQL Server Agent
ms.assetid: 6cb39bfd-079e-4be4-9c42-2fa234c65ce1
author: cawrites
ms.author: chadam
ms.openlocfilehash: f606e046f09254c3beb9c933a04640eea78990ee
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191124"
---
# <a name="sql-server-agent-tables-transact-sql"></a>Tablas de Agente SQL Server (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En los temas de esta sección se describen las tablas del sistema que almacenan la información utilizada por el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Todas las tablas están en el esquema dbo de la base de datos msdb.  
  
## <a name="in-this-section"></a>En esta sección  
 [dbo.sysalerts](../../relational-databases/system-tables/dbo-sysalerts-transact-sql.md)  
 Contiene una fila por cada alerta.  
  
 [dbo.syscategories](../../relational-databases/system-tables/dbo-syscategories-transact-sql.md)  
 Contiene las categorías que usa [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] para organizar los trabajos, las alertas y los operadores.  
  
 [dbo.sysdownloadlist](../../relational-databases/system-tables/dbo-sysdownloadlist-transact-sql.md)  
 Contiene la cola de instrucciones de descarga para todos los servidores de destino.  
  
 [dbo.sysjobactivity](../../relational-databases/system-tables/dbo-sysjobactivity-transact-sql.md)  
 Contiene información sobre la actividad y el estado de los trabajos actuales del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobhistory](../../relational-databases/system-tables/dbo-sysjobhistory-transact-sql.md)  
 Contiene información acerca de la ejecución de los trabajos programados por el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobs](../../relational-databases/system-tables/dbo-sysjobs-transact-sql.md)  
 Almacena la información de cada trabajo programado que debe ejecutar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobschedules](../../relational-databases/system-tables/dbo-sysjobschedules-transact-sql.md)  
 Contiene información de programación para los trabajos que va a ejecutar el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] agente  
  
 [dbo.sysjobservers](../../relational-databases/system-tables/dbo-sysjobservers-transact-sql.md)  
 Almacena la asociación o relación de un trabajo determinado con uno o más servidores de destino.  
  
 [dbo.sysjobsteps](../../relational-databases/system-tables/dbo-sysjobsteps-transact-sql.md)  
 Contiene la información de cada paso de un trabajo que debe ejecutar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysjobstepslogs](../../relational-databases/system-tables/dbo-sysjobstepslogs-transact-sql.md)  
 Contiene información sobre los registros de pasos de trabajo.  
  
 [dbo.sysnotifications](../../relational-databases/system-tables/dbo-sysnotifications-transact-sql.md)  
 Contiene una fila por cada notificación.  
  
 [dbo.sysoperators](../../relational-databases/system-tables/dbo-sysoperators-transact-sql.md)  
 Contiene una fila por cada operador del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxies](../../relational-databases/system-tables/dbo-sysproxies-transact-sql.md)  
 Contiene información sobre las cuentas de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxylogin](../../relational-databases/system-tables/dbo-sysproxylogin-transact-sql.md)  
 Registra qué inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] están asociados a cada cuenta de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.sysproxysubsystem](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Registra qué subsistema del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se utiliza en cada cuenta de proxy.  
  
 [dbo.sysschedules](../../relational-databases/system-tables/dbo-sysschedules-transact-sql.md)  
 Contiene información sobre las programaciones de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssessions](../../relational-databases/system-tables/dbo-syssessions-transact-sql.md)  
 Contiene la fecha de inicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para cada sesión del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Se crea una sesión cada vez que se inicia el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [dbo.syssubsystems](../../relational-databases/system-tables/dbo-sysproxysubsystem-transact-sql.md)  
 Contiene información sobre todos los subsistemas proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] disponibles.  
  
 [dbo.systargetservergroupmembers](../../relational-databases/system-tables/dbo-systargetservergroupmembers-transact-sql.md)  
 Registra los servidores de destino dados de alta actualmente en este grupo multiservidor.  
  
 [dbo.systargetservergroups](../../relational-databases/system-tables/dbo-systargetservergroups-transact-sql.md)  
 Registra los grupos de servidor de destino dados de alta actualmente en este entorno multiservidor.  
  
 [dbo.systargetservers](../../relational-databases/system-tables/dbo-systargetservers-transact-sql.md)  
 Registra los servidores de destino que están datos de alta actualmente en este dominio de operación multiservidor.  
  
 [dbo.systaskids](../../relational-databases/system-tables/dbo-systaskids-transact-sql.md)  
 Contiene una asignación de las tareas creadas en versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a trabajos de [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] de la versión actual.  
  
  
