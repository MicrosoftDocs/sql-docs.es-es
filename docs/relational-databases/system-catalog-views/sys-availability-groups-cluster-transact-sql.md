---
description: sys.availability_groups_cluster (Transact-SQL)
title: sys.availability_groups_cluster (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_cluster
- availability_groups_cluster
- availability_groups_cluster_TSQL
- sys.availability_groups_cluster_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups_cluster catalog view
- Availability Groups [SQL Server], WSFC clusters
ms.assetid: d0f4683f-cdf0-4227-8b68-720ffe58f158
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 22e1b7354ff2e343b86530766e6fe040383a2920
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341370"
---
# <a name="sysavailability_groups_cluster-transact-sql"></a>sys.availability_groups_cluster (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada grupo de disponibilidad AlwaysOn en los clústeres de conmutación por error de Windows Server (WSFC). Cada fila contiene los metadatos del grupo de disponibilidad del clúster de WSFC.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador único (GUID) del grupo de disponibilidad.|  
|**name**|**sysname**|Nombre del grupo de disponibilidad. Es un nombre definido por el usuario que debe ser único dentro del clúster de conmutación por error de Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|Id. del recurso del clúster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|Id. del grupo de recursos del clúster WSFC del grupo de disponibilidad.|  
|**failure_condition_level**|**int**|Nivel de condición de error definido por el usuario bajo el cual debe activarse una conmutación por error automática; puede tener uno de los valores enteros siguientes:<br /><br /> 1: especifica que se debe iniciar una conmutación automática por error cuando se produce cualquiera de las siguientes situaciones: <br />-El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio está inactivo.<br />-La concesión del grupo de disponibilidad para conectarse al clúster de conmutación por error de WSFC expira porque no se recibe ninguna confirmación de la instancia del servidor. Para más información, vea [Cómo funciona: tiempo de espera de concesión de Always On de SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).<br /><br /> 2: especifica que se debe iniciar una conmutación automática por error cuando se produce cualquiera de las siguientes situaciones:  <br />-La instancia de no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta al clúster y se supera el umbral de **health_check_timeout** especificado por el usuario del grupo de disponibilidad. <br />-La réplica de disponibilidad tiene un estado de error. <br />3: especifica que se debe iniciar una conmutación automática por error en caso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errores internos críticos, como bloqueos por subproceso huérfanos, infracciones de acceso de escritura graves o demasiados volcados. Este es el valor predeterminado. <br />4: especifica que se debe iniciar una conmutación automática por error en caso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] errores internos moderados, como una condición de falta de memoria persistente en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] grupo de recursos internos.<br />5: especifica que se debe iniciar una conmutación automática por error en cualquier condición de error calificada, como:<br />-Agotamiento de los subprocesos de trabajo del motor de SQL. <br />-Detección de un interbloqueo no resuelto.<br /><br /> Los niveles de condición de error (1-5) abarcan desde el nivel menos restrictivo (1) al más restrictivo (5). Un nivel de condición dado abarca todos los niveles menos restrictivos. Así pues, el nivel de condición más estricto (el nivel 5) incluye los cuatro niveles de condición menos restrictivos (1-4), el nivel 4 incluye los niveles 1-3, y así sucesivamente.<br /><br /> Para cambiar este valor, use la opción FAILURE_CONDITION_LEVEL de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Tiempo de espera (en milisegundos) para que el procedimiento almacenado del sistema [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) devuelva información de estado del servidor, antes de que se asuma que la instancia del servidor es lenta o no responde. El valor predeterminado es 30000 milisegundos (30 segundos).<br /><br /> Para cambiar este valor, use la opción HEALTH_CHECK_TIMEOUT de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Ubicación preferida para realizar copias de seguridad en las bases de datos de disponibilidad en este grupo de disponibilidad. Uno de los siguientes valores:<br /><br /> 0: principal. Las copias de seguridad deben realizarse siempre en la réplica principal.<br />1: solo secundaria. Es preferible realizar copias de seguridad en una réplica secundaria.<br />2: preferir secundario. Es preferible realizar copias de seguridad en una réplica secundaria, pero se pueden realizar en la réplica principal si no hay ninguna réplica secundaria disponible a tal efecto. Este es el comportamiento predeterminado.<br />3: cualquier réplica. No se establecen preferencias sobre si las copias de seguridad se deben realizar en la réplica principal o en una secundaria.<br /><br /> Para más información, consulte [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Descripción de **automated_backup_preference**, uno de los siguientes:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Ninguno|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW ANY DEFINITION en la instancia de servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
