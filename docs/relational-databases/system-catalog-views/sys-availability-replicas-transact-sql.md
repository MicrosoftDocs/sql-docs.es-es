---
description: sys.availability_replicas (Transact-SQL)
title: sys.availability_replicas (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- availability_replicas_TSQL
- availability_replicas
- sys.availability_replicas
- sys.availability_replicas_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- sys.availability_replicas catalog view
ms.assetid: 0a06e9b6-a1e4-4293-867b-5c3f5a8ff62c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b78d5e65fe9f6c60e94ddbf241c9eabfed332f0c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338922"
---
# <a name="sysavailability_replicas-transact-sql"></a>sys.availability_replicas (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Devuelve una fila para cada una de las réplicas de disponibilidad pertenecientes a un grupo de disponibilidad AlwaysOn del clúster de conmutación por error de WSFC.  
  
Si la instancia del servidor local no puede comunicar con el clúster de conmutación por error de WSFC, debido por ejemplo a que el clúster está inactivo o se ha perdido el quorum, solo se devuelven las filas de las réplicas de disponibilidad locales. Estas filas contendrán solamente las columnas de datos que están almacenadas localmente en caché en metadatos.  
  
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**replica_id**|**uniqueidentifier**|Identificador único de la réplica.|  
|**group_id**|**uniqueidentifier**|Identificador único del grupo de disponibilidad al que pertenece la réplica.|  
|**replica_metadata_id**|**int**|Identificador del objeto de metadatos local correspondiente a las réplicas de disponibilidad en el motor de base de datos.|  
|**replica_server_name**|**nvarchar(256)**|Nombre de servidor de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que hospeda esta réplica y, para una instancia no predeterminada, su nombre de instancia.|  
|**owner_sid**|**varbinary(85)**|Identificador de seguridad (SID) registrado en esta instancia de servidor para el propietario externo de esta réplica de disponibilidad.<br /><br /> NULL para las réplicas de disponibilidad no locales.|  
|**endpoint_url**|**nvarchar(128)**|Representación en forma de cadena de la base de datos definida por el usuario que crea un reflejo del extremo usado por las conexiones entre las réplicas principal y secundaria para la sincronización de datos. Para obtener más información sobre la sintaxis de las direcciones URL del punto de conexión, vea [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).<br /><br /> NULL = No se puede comunicar con el clúster de conmutación por error de WSFC.<br /><br /> Para cambiar este punto de conexión, use la opción ENDPOINT_URL de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**availability_mode**|**tinyint**|Modo de disponibilidad de la réplica, que puede ser alguno de los siguientes:<br /><br /> 0 &#124; confirmación asincrónica. La réplica principal puede confirmar transacciones sin esperar a que la réplica secundaria escriba el registro en disco.<br /><br /> 1 &#124; confirmación sincrónica. La réplica principal espera para confirmar una determinada transacción hasta que la réplica secundaria escribe la transacción en el disco.<br /><br />4 &#124; solo configuración. La réplica principal envía los metadatos de configuración del grupo de disponibilidad a la réplica de forma sincrónica. Los datos de usuario no se transmiten a la réplica. Disponible en SQL Server 2017 CU1 y versiones posteriores.<br /><br /> Para obtener más información, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).|  
|**availability_mode_desc**|**nvarchar(60)**|Descripción del **\_ modo de disponibilidad**, uno de los siguientes:<br /><br /> confirmación ASINCRÓNICA \_<br /><br /> confirmación sincrónica \_<br /><br /> \_solo configuración<br /><br /> Para cambiar el modo de disponibilidad de una réplica de disponibilidad, use la opción AVAILABILITY_MODE de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .<br/><br>No se puede cambiar el modo de disponibilidad de una réplica a \_ solo configuración. No se puede cambiar una \_ réplica de solo configuración a una réplica principal o secundaria. |  
|**modo de conmutación por error \_**|**tinyint**|El [modo de conmutación por error](../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md) de la réplica de disponibilidad, uno de los siguientes:<br /><br /> 0 &#124; conmutación automática por error. La réplica es un posible objetivo de las conmutaciones por error automáticas.  La conmutación automática por error solo se admite si el modo de disponibilidad está establecido en confirmación sincrónica (**\_ modo de disponibilidad** = 1) y la réplica de disponibilidad está sincronizada actualmente.<br /><br /> 1 &#124; conmutación por error manual. La conmutación por error a una réplica secundaria establecida en conmutación por error manual la debe iniciar manualmente el administrador de la base de datos. El tipo de conmutación por error que se realice dependerá de si se ha sincronizado la réplica secundaria, del modo siguiente:<br /><br /> Si la réplica de disponibilidad no se ha sincronizado o se está sincronizando todavía, solo se puede realizar una conmutación por error forzada (con la posibilidad de que se pierdan datos).<br /><br /> Si el modo de disponibilidad se establece en confirmación sincrónica (**\_ modo de disponibilidad** = 1) y la réplica de disponibilidad está sincronizada actualmente, se puede producir una conmutación por error manual sin pérdida de datos.<br /><br /> Para ver un resumen del estado de sincronización de base de datos de cada base de datos de disponibilidad en una réplica de disponibilidad, use las columnas **de estado de sincronización y de \_** **\_ \_ DESC** de sincronización de la vista de administración dinámica [Sys.dm_hadr_availability_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-availability-replica-states-transact-sql.md) . El resumen tiene en cuenta el estado de sincronización de todas las bases de datos de disponibilidad y el modo de disponibilidad de sus réplicas de disponibilidad.<br /><br /> **Nota:** Para ver el estado de sincronización de una determinada base de datos de disponibilidad, consulte las columnas **\_ Estado** de sincronización y estado de **sincronización \_** de la vista de administración dinámica [Sys.dm_hadr_database_replica_states](../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .|  
|**modo de conmutación por error \_ \_ DESC**|**nvarchar(60)**|Descripción del **\_ modo de conmutación por error**, uno de los siguientes:<br /><br /> MANUAL<br /><br /> AUTOMATIC<br /><br /> Para cambiar el modo de conmutación por error, use la \_ opción modo de conmutación por error de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**tiempo de espera de sesión \_**|**int**|Período de tiempo de espera, en segundos. El período de tiempo de espera es el tiempo máximo que la réplica espera hasta recibir un mensaje de otra réplica antes de considerar que se ha producido un error en la conexión entre la réplica principal y secundaria. El tiempo de espera de la sesión detecta si las réplicas secundarias están conectadas a la réplica principal.<br /><br /> Al detectar una conexión con errores con una réplica secundaria, la réplica principal considera que la réplica secundaria no está \_ sincronizada. Al detectar un error en la conexión con la réplica principal, la réplica secundaria intenta volver a conectarse.<br /><br /> **Nota:** Los tiempos de espera de la sesión no producen conmutaciones por error automáticas.<br /><br /> Para cambiar este valor, use la opción SESSION_TIMEOUT de la instrucción [ALTER Availability Group](../../t-sql/statements/alter-availability-group-transact-sql.md) [!INCLUDE[tsql](../../includes/tsql-md.md)] .|  
|**rol principal- \_ \_ permitir \_ conexiones**|**tinyint**|Si la disponibilidad permite todas las conexiones o solamente conexiones de lectura/escritura, que puede tener uno de los siguientes valores:<br /><br /> 2 = Todas (predeterminado)<br /><br /> 3 = Lectura/escritura|  
|**\_rol principal \_ permitir \_ conexiones \_ DESC**|**nvarchar(60)**|Descripción del **\_ rol principal \_ permitir \_ conexiones**, una de las siguientes:<br /><br /> ALL<br /><br /> LECTURA y \_ escritura|  
|**\_rol secundario \_ permitir \_ conexiones**|**tinyint**|Si una réplica de disponibilidad que está realizando el rol secundario (es decir, una réplica secundaria) puede aceptar conexiones de clientes; puede tener uno de los valores siguientes:<br /><br /> 0 = No. No se permiten conexiones directas con las bases de datos de la réplica secundaria y las bases de datos no están disponible para acceso de lectura. Esta es la configuración predeterminada.<br /><br /> 1 = Solo lectura. Solo se permiten conexiones de solo lectura a las bases de datos de la réplica secundaria. Todas las bases de datos de la réplica están disponibles para acceso de lectura.<br /><br /> 2 = Todas. Se permiten todas las conexiones con las bases de datos de la réplica secundaria para acceso de solo lectura.<br /><br /> Para más información, consulte [Secundarias activas: réplicas secundarias legibles &#40;grupos de disponibilidad AlwaysOn&#41;](../../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).|  
|**secondary_role_allow_connections_desc**|**nvarchar(60)**|Descripción de **secondary_role_allow_connections**, uno de los siguientes:<br /><br /> No<br /><br /> READ_ONLY<br /><br /> ALL|  
|**create_date**|**datetime**|Fecha en que se creó la réplica.<br /><br /> NULL = La réplica no está en esta instancia de servidor.|  
|**modify_date**|**datetime**|Fecha de la última modificación de la réplica.<br /><br /> NULL = La réplica no está en esta instancia de servidor.|  
|**backup_priority**|**int**|Representa la prioridad definida por el usuario para realizar copias de seguridad en esta réplica en relación con las otras réplicas del mismo grupo de disponibilidad. El valor es un número entero en el intervalo de 0..100.<br /><br /> Para más información, consulte [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad Always On&#41;](../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).|  
|**read_only_routing_url**|**nvarchar(256)**|Extremo de conectividad (URL) de la réplica de disponibilidad de solo lectura. Para obtener más información, vea [Configurar el enrutamiento de solo lectura para un grupo de disponibilidad &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/configure-read-only-routing-for-an-availability-group-sql-server.md).|  
|**seeding_mode**|**tinyint**|Uno de los valores siguientes: </br></br> 0: automático </br></br> 1: manual|
|**seeding_mode_desc**|**nvarchar(60)**|Describe el modo de propagación. </br></br> AUTOMATIC </br></br>MANUAL|
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW ANY DEFINITION en la instancia de servidor.  
  
## <a name="see-also"></a>Consulte también  
 [sys.availability_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-availability-groups-transact-sql.md)   
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)  
  
  
