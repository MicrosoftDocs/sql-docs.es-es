---
description: sys.dm_os_threads (Transact-SQL)
title: sys.dm_os_threads (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/13/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_threads_TSQL
- sys.dm_os_threads
- dm_os_threads
- sys.dm_os_threads_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_threads dynamic management view
ms.assetid: a5052701-edbf-4209-a7cb-afc9e65c41c1
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: bd05ea40dd38053b30e75323e888689fc903f8f1
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837239"
---
# <a name="sysdm_os_threads-transact-sql"></a>sys.dm_os_threads (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una lista de todos los subprocesos del sistema operativo [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se están ejecutando en el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_threads**.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|thread_address|**varbinary(8**|Dirección de memoria (clave principal) del subproceso.|  
|started_by_sqlservr|**bit**|Indica el iniciador del subproceso.<br /><br /> 1 = [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inició el subproceso.<br /><br /> 0 = Otro componente inició el subproceso como un procedimiento almacenado extendido desde [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|os_thread_id|**int**|Identificador del subproceso asignado por el sistema operativo.|  
|status|**int**|Marca del estado interno|  
|instruction_address|**varbinary(8**|Dirección de la instrucción que se está ejecutando en ese momento.|  
|creation_time|**datetime**|Hora a la que se creó este subproceso.|  
|kernel_time|**bigint**|Tiempo del kernel consumido por este subproceso.|  
|usermode_time|**bigint**|Tiempo de usuario consumido por este subproceso.|  
|stack_base_address|**varbinary(8**|Dirección de memoria de la dirección de pila más alta de este subproceso.|  
|stack_end_address|**varbinary(8**|Dirección de memoria de la dirección de pila más baja de este subproceso.|  
|stack_bytes_committed|**int**|Número de bytes confirmados en la pila.|  
|stack_bytes_used|**int**|Número de bytes que se usan de forma activa en el subproceso.|  
|affinity|**bigint**|Máscara de CPU en la que se ejecuta este subproceso. Esto depende del valor configurado por la instrucción **ALTER Server Configuration Set Process Affinity** . Puede ser distinta del programador en caso de afinidad de software.|  
|Priority|**int**|Valor de prioridad de este subproceso.|  
|Configuración regional|**int**|LCID de la configuración regional en memoria caché del subproceso.|  
|Token|**varbinary(8**|Identificador del token de suplantación en caché del subproceso.|  
|is_impersonating|**int**|Indica si este subproceso usa suplantación de Win32.<br /><br /> 1 = El subproceso usa credenciales de seguridad diferentes de las predeterminadas del proceso. Indica que el subproceso suplanta una entidad distinta de la que creó el proceso.|  
|is_waiting_on_loader_lock|**int**|Estado del sistema operativo que indica si el subproceso espera en el bloqueo de carga.|  
|fiber_data|**varbinary(8**|Fibra actual de Win32 que se ejecuta en el subproceso. Esto solo se aplica cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurado para la agrupación ligera.|  
|thread_handle|**varbinary(8**|Solo para uso interno.|  
|event_handle|**varbinary(8**|Solo para uso interno.|  
|scheduler_address|**varbinary(8**|Dirección de memoria del programador asociado con este subproceso. Para obtener más información, vea [sys.dm_os_schedulers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-schedulers-transact-sql.md).|  
|worker_address|**varbinary(8**|Dirección de memoria del trabajador enlazado a este subproceso. Para obtener más información, vea [sys.dm_os_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md).|  
|fiber_context_address|**varbinary(8**|Dirección de contexto de fibra interna. Esto solo se aplica cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está configurado para la agrupación ligera.|  
|self_address|**varbinary(8**|Puntero de comprobaciones de coherencia.|  
|processor_group|**smallint**|**Válido para** : [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] y versiones posteriores.<br /><br /> Identificador de grupo de procesadores.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="notes-on-linux-version"></a>Notas sobre la versión de Linux

Debido a cómo funciona el motor de SQL en Linux, parte de esta información no coincide con los datos de diagnóstico de Linux. Por ejemplo, no `os_thread_id` coincide con el resultado de herramientas como `ps` `top` o procfs (/proc/ `pid` ).  Esto se debe a la capa de abstracción de plataforma (SQLPAL), una capa entre SQL Server componentes y el sistema operativo.

## <a name="examples"></a>Ejemplos  
 Tras el inicio, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] inicia subprocesos y después asocia trabajos a dichos subprocesos. Sin embargo, los componentes externos, como un procedimiento almacenado extendido, pueden iniciar subprocesos bajo el proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no controla estos subprocesos. sys.dm_os_threads puede proporcionar información sobre subprocesos no autorizados que consumen recursos del [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] proceso.  
  
 La siguiente consulta se utiliza para buscar subprocesos de trabajo, junto con el tiempo consumido en la ejecución, que ejecutan subprocesos no iniciados por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]
>  Por concisión, la siguiente consulta utiliza un asterisco (`*`) en la instrucción `SELECT`. Evite utilizar asteriscos (*), especialmente con vistas de catálogo, vistas de administración dinámica y funciones con valores de tabla del sistema. Las actualizaciones y versiones futuras de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden agregar columnas y cambiar el orden de las columnas a estas vistas y funciones. Es posible que estos cambios provoquen errores en las aplicaciones que esperan un determinado orden y número de columnas.  
  
```  
SELECT *  
  FROM sys.dm_os_threads  
  WHERE started_by_sqlservr = 0;  
```  
  
## <a name="see-also"></a>Consulte también  
  [sys.dm_os_workers &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-workers-transact-sql.md)   
 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)  
  
