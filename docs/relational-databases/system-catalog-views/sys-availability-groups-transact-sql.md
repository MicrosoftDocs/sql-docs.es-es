---
description: sys.availability_groups (Transact-SQL)
title: sys.availability_groups (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.availability_groups_TSQL
- availability_groups_TSQL
- sys.availability_groups
- availability_groups
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_groups catalog view
ms.assetid: da7fa55f-c008-45d9-bcfc-3513b02d9e71
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: cc784f8bbd811ad50f2fcb1b64bd3f5bdf8cd2d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338915"
---
# <a name="sysavailability_groups-transact-sql"></a>sys.availability_groups (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila para cada grupo de disponibilidad para el que la instancia local de SQL Server hospeda una réplica de disponibilidad. Cada fila contiene una copia almacenada en caché de los metadatos del grupo de disponibilidad.  
   
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**group_id**|**uniqueidentifier**|Identificador único (GUID) del grupo de disponibilidad.|  
|**name**|**sysname**|Nombre del grupo de disponibilidad. Es un nombre definido por el usuario que debe ser único dentro del clúster de conmutación por error de Windows Server (WSFC).|  
|**resource_id**|**nvarchar(40)**|Id. del recurso del clúster WSFC.|  
|**resource_group_id**|**nvarchar(40)**|Id. del grupo de recursos del clúster WSFC del grupo de disponibilidad.|  
|**failure_condition_level**|**int**|Nivel de condición de error definido por el usuario en el que se debe desencadenar una conmutación automática por error, uno de los valores enteros que se muestran en la tabla inmediatamente debajo de esta tabla.<br /><br /> Los niveles de condición de error (1-5) abarcan desde el nivel menos restrictivo (1) al más restrictivo (5). Un nivel de condición dado abarca todos los niveles menos restrictivos. Así pues, el nivel de condición más estricto (el nivel 5) incluye los cuatro niveles de condición menos restrictivos (1-4), el nivel 4 incluye los niveles 1-3, y así sucesivamente.<br /><br /> Para cambiar este valor, use la opción FAILURE_CONDITION_LEVEL de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**health_check_timeout**|**int**|Tiempo de espera (en milisegundos) para que el procedimiento almacenado del sistema [sp_server_diagnostics](../../relational-databases/system-stored-procedures/sp-server-diagnostics-transact-sql.md) devuelva información de estado del servidor, antes de que se asuma que la instancia del servidor es lenta o no responde. El valor predeterminado es 30000 milisegundos (30 segundos).<br /><br /> Para cambiar este valor, use la opción HEALTH_CHECK_TIMEOUT de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**automated_backup_preference**|**tinyint**|Ubicación preferida para realizar copias de seguridad en las bases de datos de disponibilidad en este grupo de disponibilidad. A continuación se muestran los valores posibles y sus descripciones.<br /><br /> <br /><br /> 0: principal. Las copias de seguridad deben realizarse siempre en la réplica principal.<br /><br /> 1: solo secundaria. Es preferible realizar copias de seguridad en una réplica secundaria.<br /><br /> 2: preferir secundario. Es preferible realizar copias de seguridad en una réplica secundaria, pero se pueden realizar en la réplica principal si no hay ninguna réplica secundaria disponible a tal efecto. Éste es el comportamiento predeterminado.<br /><br /> 3: cualquier réplica. No se establecen preferencias sobre si las copias de seguridad se deben realizar en la réplica principal o en una secundaria.<br /><br /> <br /><br /> Para más información, consulte [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**automated_backup_preference_desc**|**nvarchar(60)**|Descripción de **automated_backup_preference**, uno de los siguientes:<br /><br /> PRIMARY<br /><br /> SECONDARY_ONLY<br /><br /> SECONDARY<br /><br /> Ninguno|  
|**version**|**smallint**|La versión de los metadatos del grupo de disponibilidad almacenados en el clúster de conmutación por error de Windows. Este número de versión se incrementa cuando se agregan nuevas características.|  
|**basic_features**|**bit**|Especifica si se trata de un grupo de disponibilidad básico. Para obtener más información, vea [Grupos de disponibilidad básica &#40;grupos de disponibilidad AlwaysOn&#41;](../../database-engine/availability-groups/windows/basic-availability-groups-always-on-availability-groups.md).|  
|**dtc_support**|**bit**|Especifica si se ha habilitado la compatibilidad con DTC para este grupo de disponibilidad. La opción **DTC_SUPPORT** de **Crear grupo de disponibilidad** controla esta configuración.|  
|**db_failover**|**bit**|Especifica si el grupo de disponibilidad admite la conmutación por error para las condiciones de mantenimiento de la base de datos. La opción **DB_FAILOVER** de **Crear grupo de disponibilidad** controla esta configuración.|  
|**is_distributed**|**bit**|Especifica si se trata de un grupo de disponibilidad distribuido. Para obtener más información, vea [Distributed Availability Groups &#40;Always On Availability Groups&#41; (Grupos de disponibilidad distribuida &#40;grupos de disponibilidad AlwaysOn&#41;)](../../database-engine/availability-groups/windows/distributed-availability-groups.md).|
|**cluster_type**|**tinyint**|0: clúster de conmutación por error de Windows Server <br/><br/>1: clúster externo (por ejemplo, Linux Pacemaker)<br/><br/>2: ninguno|
|**cluster_type_desc**|**nvarchar(60)**|Descripción de texto de tipo de clúster|
|**required_synchronized_secondaries_to_commit**|**int**| El número de réplicas secundarias que deben estar en un estado sincronizado para que se complete una confirmación|
|**sequence_number**|**bigint**|Identifica la secuencia de configuración del grupo de disponibilidad. Aumenta incrementalmente cada vez que la réplica principal del grupo de disponibilidad actualiza la configuración del grupo.|
|**is_contained**|**bit**|1: instancia maestra de clúster de Big Data configurada para alta disponibilidad. <br/><br/> 0: todos los demás.|
  
## <a name="failure-condition-level--values"></a>Valores de nivel de condición de error  
 En la tabla siguiente se describen los posibles niveles de condición de error de la columna **failure_condition_level** .  
  
|Value|Condición de error|  
|-----------|-----------------------|  
|1|Especifica que se debe iniciar una conmutación por error automática en los casos siguientes:<br /><br /> <br /><br /> -El [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] servicio está inactivo.<br /><br /> -La concesión del grupo de disponibilidad para conectarse al clúster de conmutación por error de WSFC expira porque no se recibe ninguna confirmación de la instancia del servidor. Para más información, vea [Cómo funciona: tiempo de espera de concesión de Always On de SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/how-it-works-sql-server-alwayson-lease-timeout/ba-p/317268).|  
|2|Especifica que se debe iniciar una conmutación por error automática en los casos siguientes:<br /><br /> <br /><br /> -La instancia de no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se conecta al clúster y se supera el umbral de **health_check_timeout** especificado por el usuario del grupo de disponibilidad.<br /><br /> -La réplica de disponibilidad tiene un estado de error.|  
|3|Especifica que se debe iniciar una conmutación automática por error en caso de errores internos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] graves, como bloqueos por subproceso huérfanos, infracciones graves de acceso de escritura o un volcado excesivo.<br /><br /> Este es el valor predeterminado.|  
|4|Especifica que se debe iniciar una conmutación automática por error en caso de errores internos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] moderados, tales como una condición persistente de memoria insuficiente en el grupo de recursos de servidor interno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|5|Especifica que se debe iniciar una conmutación por error automática en el caso de condiciones de error designadas, incluidas las siguientes:<br /><br /> <br /><br /> -Agotamiento de los subprocesos de trabajo del motor de SQL.<br /><br /> -Detección de un interbloqueo no resuelto.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW ANY DEFINITION en la instancia de servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.availability_replicas &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-replicas-transact-sql.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
