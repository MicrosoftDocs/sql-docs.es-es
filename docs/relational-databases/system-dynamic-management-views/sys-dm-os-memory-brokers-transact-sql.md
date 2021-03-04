---
description: sys.dm_os_memory_brokers (Transact-SQL)
title: sys.dm_os_memory_brokers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_os_memory_brokers
- dm_os_memory_brokers_TSQL
- sys.dm_os_memory_brokers_TSQL
- dm_os_memory_brokers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_brokers dynamic management view
ms.assetid: 48dd6ad9-0d36-4370-8a12-4921d0df4b86
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 40f661b1b7f8470f643612e8a1725b33884786cf
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839173"
---
# <a name="sysdm_os_memory_brokers-transact-sql"></a>sys.dm_os_memory_brokers (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Las asignaciones internas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan el administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El seguimiento de la diferencia entre los contadores de memoria de proceso de **Sys.dm_os_process_memory** y los contadores internos puede indicar el uso de memoria de componentes externos en el [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] espacio de memoria.  
  
 Los agentes de memoria distribuyen equitativamente las asignaciones de memoria entre varios componentes dentro de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], en función del uso actual y previsto. Los agentes de memoria no realizan las asignaciones. Solo realizan el seguimiento de las asignaciones para calcular la distribución.  
  
 La tabla siguiente proporciona información sobre los agentes de memoria.  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_memory_brokers**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**pool_id**|**int**|Id. del grupo de recursos de servidor si está asociado a un grupo del regulador de recursos.|  
|**memory_broker_type**|**nvarchar(60)**|Tipo de agente de memoria. Actualmente hay tres tipos de agentes de memoria en, que se [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enumeran a continuación con sus descripciones.<br /><br /> **MEMORYBROKER_FOR_CACHE** : memoria asignada para su uso por parte de objetos almacenados en caché (no caché del grupo de búferes).<br /><br /> **MEMORYBROKER_FOR_STEAL** : memoria que se ha robado del grupo de búferes. Esta memoria no está disponible para ser reutilizada por otros componentes hasta que el propietario actual la libere.<br /><br /> **MEMORYBROKER_FOR_RESERVE** : memoria reservada para un uso futuro por parte de las solicitudes que se están ejecutando actualmente.|  
|**allocations_kb**|**bigint**|La cantidad de memoria, en kilobytes (KB) asignada a este tipo de agente.|  
|**allocations_kb_per_sec**|**bigint**|La tasa de asignaciones de memoria en kilobytes (KB) por segundo. Este valor puede ser negativo para las cancelaciones de asignación de memoria.|  
|**predicted_allocations_kb**|**bigint**|La cantidad prevista de memoria asignada por el agente. Depende del modelo de uso de la memoria.|  
|**target_allocations_kb**|**bigint**|La cantidad recomendada de memoria asignada, en kilobytes (KB), depende de la configuración actual y del modelo de uso de la memoria. El agente debería aumentar o disminuir hasta este número.|  
|**future_allocations_kb**|**bigint**|El número previsto de asignaciones, en kilobytes (KB), que se realizarán en los segundos siguientes.|  
|**overall_limit_kb**|**bigint**|Cantidad máxima de memoria, en kilobytes (KB), que el agente puede asignar.|  
|**last_notification**|**nvarchar(60)**|Recomendación del uso de memoria, que depende de la configuración actual y del modelo de uso. Los valores válidos son los siguientes:<br /><br /> grow<br /><br /> shrink<br /><br /> Estable|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos  

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="see-also"></a>Consulte también  

  [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
