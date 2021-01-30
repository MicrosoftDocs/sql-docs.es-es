---
description: sys.dm_exec_sessions (Transact-SQL)
title: sys.dm_exec_sessions (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/03/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_sessions_TSQL
- sys.dm_exec_sessions
- dm_exec_sessions
- sys.dm_exec_sessions_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_sessions dynamic management view
ms.assetid: 2b7e8e0c-eea0-431e-819f-8ccd12ec8cfa
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c9cfe8d7d41a56b216fdb5598173ac252ca989a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192955"
---
# <a name="sysdm_exec_sessions-transact-sql"></a>sys.dm_exec_sessions (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada sesión autenticada en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. sys.dm_exec_sessions es una vista de ámbito de servidor que muestra información acerca de todas las conexiones de usuario activas y las tareas internas. Esta información incluye la versión del cliente, el nombre del programa cliente, la hora de inicio de sesión del cliente, el usuario de inicio de sesión, la configuración de la sesión actual, etcétera. Use sys.dm_exec_sessions para ver, primero, la carga del sistema actual e identificar una sesión de interés y, después, obtener más información acerca de esa sesión con otras vistas o funciones de administración dinámica.  
  
 Las vistas de administración dinámica sys.dm_exec_connections, sys.dm_exec_sessions y sys.dm_exec_requests se asignan a la tabla del sistema [sys.sysprocesses](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md) .  
  
> **Nota:** Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_exec_sessions**.  
  
|Nombre de la columna|Tipo de datos|Descripción e información específica de la versión|  
|-----------------|---------------|-----------------|  
|session_id|**smallint**|Identifica la sesión asociada a cada conexión principal activa. No admite valores NULL.|  
|login_time|**datetime**|Hora en que se estableció la sesión. No admite valores NULL. Las sesiones que no hayan completado el inicio de sesión en el momento en que se consulta esta DMV se muestran con un tiempo de inicio de sesión de `1900-01-01` .|  
|host_name|**nvarchar(128)**|Nombre de la estación de trabajo cliente específica de una sesión. El valor es NULL para las sesiones internas. Acepta valores NULL.<br /><br /> **Nota de seguridad:** La aplicación cliente proporciona el nombre de la estación de trabajo y puede proporcionar datos inexactos. No confíe en HOST_NAME como característica de seguridad.|  
|program_name|**nvarchar(128)**|Nombre del programa cliente que inició la sesión. El valor es NULL para las sesiones internas. Acepta valores NULL.|  
|host_process_id|**int**|Identificador de proceso del programa cliente que inició la sesión. El valor es NULL para las sesiones internas. Acepta valores NULL.|  
|client_version|**int**|Versión del protocolo TDS de la interfaz utilizada por el cliente para conectarse al servidor. El valor es NULL para las sesiones internas. Acepta valores NULL.|  
|client_interface_name|**nvarchar(32)**|Nombre de la biblioteca o el controlador que el cliente usa para comunicarse con el servidor. El valor es NULL para las sesiones internas. Acepta valores NULL.|  
|security_id|**varbinary(85)**|Identificador de seguridad de Microsoft Windows asociado al inicio de sesión. No admite valores NULL.|  
|login_name|**nvarchar(128)**|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se está ejecutando la sesión. Para saber qué nombre de inicio de sesión original ha creado la sesión, vea original_login_name. Puede ser un nombre de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Inicio de sesión autenticado o un nombre de usuario de dominio autenticado de Windows. No admite valores NULL.|  
|nt_domain|**nvarchar(128)**|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Dominio de Windows para el cliente si la sesión utiliza la autenticación de Windows o una conexión de confianza. Este valor es NULL para las sesiones internas y los usuarios que no son del dominio. Acepta valores NULL.|  
|nt_user_name|**nvarchar(128)**|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Nombre de usuario de Windows para el cliente si la sesión utiliza la autenticación de Windows o una conexión de confianza. Este valor es NULL para las sesiones internas y los usuarios que no son del dominio. Acepta valores NULL.|  
|status|**nvarchar(30)**|Estado de la sesión. Valores posibles:<br /><br /> **Running**: está ejecutando una o varias solicitudes actualmente<br /><br /> **Sleeping**: no está ejecutando solicitudes actualmente<br /><br /> **Inactivo** : la sesión se ha restablecido debido a la agrupación de conexiones y ahora está en estado de inicio de sesión.<br /><br /> **Preconnect**: la sesión está en el clasificador del Regulador de recursos.<br /><br /> No admite valores NULL.|  
|context_info|**varbinary(128)**|Valor CONTEXT_INFO para la sesión. El usuario establece la información de contexto mediante la instrucción [set CONTEXT_INFO](../../t-sql/statements/set-context-info-transact-sql.md) . Acepta valores NULL.|  
|cpu_time|**int**|Tiempo de CPU, en milisegundos, utilizado por esta sesión. No admite valores NULL.|  
|memory_usage|**int**|Número de páginas de memoria de 8 KB utilizadas por esta sesión. No admite valores NULL.|  
|total_scheduled_time|**int**|Tiempo total, en milisegundos, para el que se programó la ejecución de la sesión (sus solicitudes). No admite valores NULL.|  
|total_elapsed_time|**int**|Tiempo transcurrido, en milisegundos, desde que se estableció la sesión. No admite valores NULL.|  
|endpoint_id|**int**|Identificador del extremo asociado a la sesión. No admite valores NULL.|  
|last_request_start_time|**datetime**|Hora a la que comenzó la última solicitud de la sesión. Se incluye la solicitud que se ejecuta actualmente. No admite valores NULL.|  
|last_request_end_time|**datetime**|Hora a la que se realizó por última vez una solicitud de la sesión. Acepta valores NULL.|  
|Lecturas|**bigint**|Número de lecturas realizadas, por solicitudes de esta sesión, durante esta sesión. No admite valores NULL.|  
|Escrituras|**bigint**|Número de escrituras realizadas, por solicitudes de esta sesión, durante esta sesión. No admite valores NULL.|  
|logical_reads|**bigint**|Número de lecturas lógicas realizadas en la sesión. No admite valores NULL.|  
|is_user_process|**bit**|0 si es una sesión de sistema; De lo contrario, es 1. No admite valores NULL.|  
|text_size|**int**|Valor de TEXTSIZE para la sesión. No admite valores NULL.|  
|language|**nvarchar(128)**|Valor de LANGUAGE para la sesión. Acepta valores NULL.|  
|date_format|**nvarchar (3)**|Valor de DATEFORMAT para la sesión. Acepta valores NULL.|  
|date_first|**smallint**|Valor de DATEFIRST para la sesión. No admite valores NULL.|  
|quoted_identifier|**bit**|Valor de QUOTED_IDENTIFIER para la sesión. No admite valores NULL.|  
|arithabort|**bit**|Valor de ARITHABORT para la sesión. No admite valores NULL.|  
|ansi_null_dflt_on|**bit**|Valor de ANSI_NULL_DFLT_ON para la sesión. No admite valores NULL.|  
|ansi_defaults|**bit**|Valor de ANSI_DEFAULTS para la sesión. No admite valores NULL.|  
|ansi_warnings|**bit**|Valor de ANSI_WARNINGS para la sesión. No admite valores NULL.|  
|ansi_padding|**bit**|Valor de ANSI_PADDING para la sesión. No admite valores NULL.|  
|ansi_nulls|**bit**|Valor de ANSI_NULLS para la sesión. No admite valores NULL.|  
|concat_null_yields_null|**bit**|Valor de CONCAT_NULL_YIELDS_NULL para la sesión. No admite valores NULL.|  
|transaction_isolation_level|**smallint**|Nivel de aislamiento de transacción de la sesión.<br /><br /> 0 = Unspecified<br /><br /> 1 = ReadUncommitted<br /><br /> 2 = ReadCommitted<br /><br /> 3 = RepeatableRead<br /><br /> 4 = Serializable<br /><br /> 5 = Snapshot<br /><br /> No admite valores NULL.|  
|lock_timeout|**int**|Valor de LOCK_TIMEOUT para la sesión. El valor se expresa en milisegundos. No admite valores NULL.|  
|deadlock_priority|**int**|Valor de DEADLOCK_PRIORITY para la sesión. No admite valores NULL.|  
|row_count|**bigint**|Número de filas devueltas en la sesión hasta este momento. No admite valores NULL.|  
|prev_error|**int**|Identificador del último error devuelto en la sesión. No admite valores NULL.|  
|original_security_id|**varbinary(85)**|Identificador de seguridad de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows asociado al original_login_name. No admite valores NULL.|  
|original_login_name|**nvarchar(128)**|Nombre de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que el cliente usó para crear esta sesión. Puede ser un nombre de inicio de sesión autenticado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un nombre de usuario de dominio autenticado en Windows o un usuario de base de datos independiente. Tenga en cuenta que, después de la conexión inicial, la sesión puede haber pasado por muchos cambios de contexto implícitos o explícitos. Por ejemplo, si se usa [Execute as](../../t-sql/statements/execute-as-transact-sql.md) . No admite valores NULL.|  
|last_successful_logon|**datetime**|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Hora del último inicio de sesión correcto para original_login_name con anterioridad al inicio de sesión actual.|  
|last_unsuccessful_logon|**datetime**|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Hora del último intento de inicio de sesión incorrecto para original_login_name con anterioridad al inicio de sesión actual.|  
|unsuccessful_logons|**bigint**|**Válido para** : [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores.<br /><br /> Número de intentos de inicio de sesión incorrectos para original_login_name entre last_successful_logon y login_time.|  
|group_id|**int**|Identificador del grupo de cargas de trabajo al que pertenece esta sesión. No admite valores NULL.|  
|database_id|**smallint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Identificador de la base de datos actual para cada sesión.|  
|authenticating_database_id|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Identificador de la base de datos que autentica la entidad de seguridad. Para los inicios de sesión, el valor será 0. Para los usuarios de base de datos independiente, el valor será el identificador de base de datos de la base de datos independiente.|  
|open_transaction_count|**int**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Número de transacciones abiertas por sesión.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
|page_server_reads|**bigint**|**Se aplica a**: hiperescala Azure SQL Database<br /><br /> Número de lecturas del servidor de páginas realizadas, por solicitudes de esta sesión, durante esta sesión. No admite valores NULL.|  
  
## <a name="permissions"></a>Permisos  
Todos pueden ver su propia información de sesión.  
**[!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] :** Requiere el `VIEW SERVER STATE` permiso en [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] para ver todas las sesiones en el servidor.  
**[!INCLUDE[ssSDS_md](../../includes/sssds-md.md)] :** Requiere `VIEW DATABASE STATE` para ver todas las conexiones a la base de datos actual. `VIEW DATABASE STATE` no se puede conceder en la `master` base de datos. 
  
  
## <a name="remarks"></a>Observaciones  
 Cuando está habilitada la opción de configuración del servidor **Common Criteria Compliance Enabled** , las estadísticas de inicio de sesión se muestran en las siguientes columnas.  
  
-   last_successful_logon  
  
-   last_unsuccessful_logon  
  
-   unsuccessful_logons  
  
 Si esta opción no está habilitada, estas columnas devuelven valores NULL. Para obtener más información sobre cómo establecer esta opción de configuración del servidor, vea [Common Criteria Compliance Enabled (opción de configuración del servidor](../../database-engine/configure-windows/common-criteria-compliance-enabled-server-configuration-option.md)).  
 
 
 Las conexiones de administrador en Azure SQL Database verán una fila por cada sesión autenticada. Las sesiones de "SA" que aparecen en el conjunto de resultados no afectan a la cuota de usuario de las sesiones. Las conexiones que no son de administrador solo verán información relacionada con sus sesiones de usuario de base de datos.
 
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|En/aplicar|Relación|  
|----------|--------|---------------|------------------|  
|sys.dm_exec_sessions|[sys.dm_exec_requests](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)|session_id|Uno a ninguno o uno a varios|  
|sys.dm_exec_sessions|[sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/sys-dm-exec-connections-transact-sql.md)|session_id|Uno a ninguno o uno a varios|  
|sys.dm_exec_sessions|[sys.dm_tran_session_transactions](../../relational-databases/system-dynamic-management-views/sys-dm-tran-session-transactions-transact-sql.md)|session_id|Uno a ninguno o uno a varios|  
|sys.dm_exec_sessions|[Sys.dm_exec_cursors](../../relational-databases/system-dynamic-management-views/sys-dm-exec-cursors-transact-sql.md)(session_ID &#124; 0)|session_id CROSS APPLY<br /><br /> OUTER APPLY|Uno a ninguno o uno a varios|  
|sys.dm_exec_sessions|[sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md)|session_id|Uno a uno|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-finding-users-that-are-connected-to-the-server"></a>A. Buscar usuarios conectados al servidor  
 En el ejemplo siguiente se buscan los usuarios que están conectados al servidor y se devuelve el número de sesiones de cada usuario.  
  
```sql  
SELECT login_name ,COUNT(session_id) AS session_count   
FROM sys.dm_exec_sessions   
GROUP BY login_name;  
```  
  
### <a name="b-finding-long-running-cursors"></a>B. Buscar cursores de ejecución prolongada  
 En el ejemplo siguiente se buscan los cursores que se han abierto durante más de un período concreto, quién los creó y en qué sesión están.  
  
```sql  
USE master;  
GO  
SELECT creation_time ,cursor_id   
    ,name ,c.session_id ,login_name   
FROM sys.dm_exec_cursors(0) AS c   
JOIN sys.dm_exec_sessions AS s   
   ON c.session_id = s.session_id   
WHERE DATEDIFF(mi, c.creation_time, GETDATE()) > 5;  
```  
  
### <a name="c-finding-idle-sessions-that-have-open-transactions"></a>C. Buscar sesiones inactivas que tienen transacciones abiertas  
 En el ejemplo siguiente se buscan sesiones que tienen transacciones abiertas y están inactivas. Una sesión inactiva es aquélla que no tiene ninguna solicitud en ejecución.  
  
```sql  
SELECT s.*   
FROM sys.dm_exec_sessions AS s  
WHERE EXISTS   
    (  
    SELECT *   
    FROM sys.dm_tran_session_transactions AS t  
    WHERE t.session_id = s.session_id  
    )  
    AND NOT EXISTS   
    (  
    SELECT *   
    FROM sys.dm_exec_requests AS r  
    WHERE r.session_id = s.session_id  
    );  
```  
  
### <a name="d-finding-information-about-a-queries-own-connection"></a>D. Buscar información sobre una conexión propietaria de consultas  
 Consulta típica para recopilar información sobre una conexión solo para consultas.  
  
```sql  
SELECT   
    c.session_id, c.net_transport, c.encrypt_option,   
    c.auth_scheme, s.host_name, s.program_name,   
    s.client_interface_name, s.login_name, s.nt_domain,   
    s.nt_user_name, s.original_login_name, c.connect_time,   
    s.login_time   
FROM sys.dm_exec_connections AS c  
JOIN sys.dm_exec_sessions AS s  
    ON c.session_id = s.session_id  
WHERE c.session_id = @@SPID;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  



