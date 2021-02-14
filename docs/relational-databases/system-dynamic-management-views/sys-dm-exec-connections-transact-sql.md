---
description: sys.dm_exec_connections (Transact-SQL)
title: sys.dm_exec_connections (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_exec_connections_TSQL
- sys.dm_exec_connections_TSQL
- sys.dm_exec_connections
- dm_exec_connections
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_connections dynamic management view
ms.assetid: 6bd46fe1-417d-452d-a9e6-5375ee8690d8
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 7b9e9018c6da47efcdbf0a7e42564b77586446c0
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343006"
---
# <a name="sysdm_exec_connections-transact-sql"></a>sys.dm_exec_connections (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de las conexiones establecidas con esta instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y los detalles de cada conexión. Devuelve información de conexión del servidor para SQL Server. Devuelve la información de conexión de la base de datos actual para SQL Database.  
  
> [!NOTE]
> Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use [Sys.dm_pdw_exec_connections &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-connections-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|session_id|**int**|Identifica la sesión asociada a esta conexión. Acepta valores NULL.|  
|most_recent_session_id|**int**|Representa el Id. de sesión de la solicitud más reciente asociada a esta conexión. (Las conexiones SOAP pueden ser reutilizadas por otra sesión). Acepta valores NULL.|  
|connect_time|**datetime**|Marca de tiempo en que se estableció la conexión. No admite valores NULL.|  
|net_transport|**nvarchar(40)**|Siempre devuelve una **sesión** cuando una conexión tiene habilitados los conjuntos de resultados activos múltiples (Mars).<br /><br /> **Nota:** Describe el protocolo de transporte físico utilizado por esta conexión. No admite valores NULL.|  
|protocol_type|**nvarchar(40)**|Especifica el tipo de protocolo de la carga. Actualmente, distingue entre TDS (TSQL) y SOAP. Acepta valores NULL.|  
|protocol_version|**int**|Versión del protocolo de acceso a datos asociado a esta conexión. Acepta valores NULL.|  
|endpoint_id|**int**|Identificador que describe el tipo de conexión. Este endpoint_id se puede utilizar para realizar consultas en la vista sys.endpoints. Acepta valores NULL.|  
|encrypt_option|**nvarchar(40)**|Valor booleano que describe si se ha habilitado el cifrado para esta conexión. No admite valores NULL.|  
|auth_scheme|**nvarchar(40)**|Especifica el esquema de autenticación [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]/Windows usado en esta conexión. No admite valores NULL.|  
|node_affinity|**smallint**|Identifica el nodo de memoria con el que esta conexión tiene afinidad. No admite valores NULL.|  
|num_reads|**int**|Número de lecturas de bytes que se han producido en esta conexión. Acepta valores NULL.|  
|num_writes|**int**|Número de escrituras de bytes que se han producido a través de esta conexión. Acepta valores NULL.|  
|last_read|**datetime**|Marca de tiempo de la última operación de lectura realizada en esta conexión. Acepta valores NULL.|  
|last_write|**datetime**|Marca de tiempo de la última operación de escritura realizada en esta conexión. No acepta valores NULL.|  
|net_packet_size|**int**|Tamaño del paquete de red utilizado para la transferencia de información y datos. Acepta valores NULL.|  
|client_net_address|**varchar(48)**|Dirección de host del cliente que se conecta a este servidor. Acepta valores NULL.<br /><br /> Antes de V12 en [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta columna siempre devuelve NULL.|  
|client_tcp_port|**int**|Número de puerto del equipo cliente asociado a esta conexión. Acepta valores NULL.<br /><br /> En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta columna siempre devuelve NULL.|  
|local_net_address|**varchar(48)**|Representa la dirección IP del servidor que es el destino de esta conexión. Solo está disponible para las conexiones que utilicen el proveedor de transporte TCP. Acepta valores NULL.<br /><br /> En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta columna siempre devuelve NULL.|  
|local_tcp_port|**int**|Representa el puerto TCP del servidor de destino de esta conexión, si se trata de una conexión que utiliza el transporte TCP. Acepta valores NULL.<br /><br /> En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta columna siempre devuelve NULL.|  
|connection_id|**uniqueidentifier**|Identifica cada conexión de manera única. No admite valores NULL.|  
|parent_connection_id|**uniqueidentifier**|Identifica la conexión principal utilizada por la sesión MARS. Acepta valores NULL.|  
|most_recent_sql_handle|**varbinary (64)**|Identificador SQL de la última solicitud ejecutada en esta conexión. La columna most_recent_sql_handle siempre está sincronizada con la columna most_recent_session_id. Acepta valores NULL.|  
|pdw_node_id|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  
  
## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En SQL Database objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de [Administrador de Azure Active Directory](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los objetivos del servicio SQL Database, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   

## <a name="physical-joins"></a>Combinaciones físicas  
 ![Combinaciones de sys.dm_exec_connections](../../relational-databases/system-dynamic-management-views/media/join-dm-exec-connections-1.gif "Combinaciones de sys.dm_exec_connections")  
  
## <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
| Primer elemento | Segundo elemento | Relación |
| --------------| -------------- | ------------ |  
|dm_exec_sessions.session_id|dm_exec_connections.session_id|Uno a uno|  
|dm_exec_requests.connection_id|dm_exec_connections.connection_id|Varios a uno|  
|dm_broker_connections.connection_id|dm_exec_connections.connection_id|Uno a uno|  
  
## <a name="examples"></a>Ejemplos  
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

 [Funciones y vistas de administración dinámica relacionadas con ejecuciones &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  


