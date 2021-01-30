---
description: sys.servers (Transact-SQL)
title: Sys. servers (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- servers_TSQL
- sys.servers_TSQL
- servers
- sys.servers
dev_langs:
- TSQL
helpviewer_keywords:
- sys.servers catalog view
ms.assetid: 4e774ed9-4e83-4726-9f1d-8efde8f9feff
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 7261cde3df2e1aa31e7a7ab0d5ccbd5f8e8d3e54
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161402"
---
# <a name="sysservers-transact-sql"></a>sys.servers (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Contiene una fila por cada servidor vinculado o remoto registrado, y una fila para el servidor local que tiene **server_id** = 0.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**server_id**|**int**|Id. local del servidor vinculado.|  
|**name**|**sysname**|Cuando **server_id** = 0, el valor devuelto es el nombre del servidor.<br /><br /> Cuando **server_id** > 0, el valor devuelto es el nombre local del servidor vinculado.|  
|**product**|**sysname**|Nombre de producto del servidor vinculado. Un valor de "SQL Server" indica otra instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**presta**|**sysname**|Nombre del proveedor OLE DB para la conexión con el servidor vinculado.<br /><br />A partir de [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] , el valor "SQLNCLI" se asigna al [controlador de Microsoft OLE DB para SQL Server (MSOLEDBSQL) de](../../connect/oledb/oledb-driver-for-sql-server.md) forma predeterminada. En versiones anteriores, el valor "SQLNCLI" se asigna al [proveedor de OLE DB de SQL Server Native Client (SQLNCLI11)](../../relational-databases/native-client/sql-server-native-client.md).|  
|**data_source**|**nvarchar(4000)**|Propiedad de conexión del origen de datos OLE DB.|  
|**ubicación**|**nvarchar(4000)**|Propiedad de conexión de la ubicación OLE DB. Es NULL si no hay ninguna.|  
|**provider_string**|**nvarchar(4000)**|Propiedad de conexión de la cadena del proveedor OLE DB.<br /><br /> Es NULL a menos que el autor de la llamada tenga el `ALTER ANY LINKED SERVER` permiso.|  
|**catalog**|**sysname**|OLE DB propiedad de conexión del catálogo. Es NULL si no hay ninguna.|  
|**connect_timeout**|**int**|Tiempo de espera de conexión, en segundos. Es 0 si no hay ninguno.|  
|**query_timeout**|**int**|Tiempo de espera de la consulta, en segundos. Es 0 si no hay ninguno.|  
|**is_linked**|**bit**|0 = es un servidor de estilo antiguo agregado mediante **sp_addserver**, con diferentes comportamientos de RPC y transacciones distribuidas.<br /><br /> 1 = Servidor vinculado estándar.|  
|**is_remote_login_enabled**|**bit**|Se establece la opción de RPC para habilitar los inicios de sesión remotos entrantes en este servidor.|  
|**is_rpc_out_enabled**|**bit**|Se habilitan las RPC salientes (desde este servidor).|  
|**is_data_access_enabled**|**bit**|El servidor está habilitado para las consultas distribuidas.|  
|**is_collation_compatible**|**bit**|Si no se dispone de información sobre la intercalación, se da por supuesto que la intercalación de datos remotos es compatible con los datos locales.|  
|**uses_remote_collation**|**bit**|Si es 1, se utiliza la intercalación notificada por el servidor remoto; en caso contrario, se utiliza la intercalación especificada en la columna siguiente.|  
|**collation_name**|**sysname**|Nombre de la intercalación que se va a utilizar, o NULL si solo se usa la local.|  
|**lazy_schema_validation**|**bit**|Si es 1, no se comprueba la validación del esquema al iniciar la consulta.|  
|**is_system**|**bit**|Únicamente el sistema interno puede tener acceso a este servidor.|  
|**is_publisher**|**bit**|El servidor es un publicador de replicación.|  
|**is_subscriber**|**bit**|El servidor es un suscriptor de replicación.|  
|**is_distributor**|**bit**|El servidor es un distribuidor de replicación.|  
|**is_nonsql_subscriber**|**bit**|El servidor es un suscriptor de replicación que no es de SQL Server.|  
|**is_remote_proc_transaction_promotion_enabled**|**bit**|Si es 1, al llamar a un procedimiento remoto almacenado se inicia una transacción distribuida y se da de alta en MS DTC. Para obtener más información, vea [sp_serveroption &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-serveroption-transact-sql.md).|  
|**modify_date**|**datetime**|Fecha en que cambió por última vez la información del servidor.|  
|**is_rda_server**|**bit**|**Se aplica a:** A partir de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] .<br /><br />El servidor es habilitado para el archivo de datos remotos (habilitado para Stretch). Para obtener más información, consulte [enable Stretch Database on The Server](../../sql-server/stretch-database/enable-stretch-database-for-a-database.md#EnableTSQLServer).|
  
## <a name="permissions"></a>Permisos  
 El valor de **provider_string** siempre es null, a menos que el autor de la llamada tenga el permiso ALTER any Linked Server.  
  
 No se necesitan permisos para ver el servidor local (**server_id** = 0).  
  
 Cuando se crea un servidor vinculado o remoto, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea una asignación de inicio de sesión predeterminada para el rol de servidor **público** . La asignación de inicio de sesión predeterminada significa que todos los inicios de sesión pueden ver todos los servidores vinculados y remotos. Para restringir la visibilidad de estos servidores, quite la asignación de inicio de sesión predeterminada ejecutando [sp_droplinkedsrvlogin](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md) y especificando null para el parámetro *locallogin* .  
  
 Si se elimina la asignación de inicio de sesión predeterminada, solo los usuarios que se hayan agregado de forma explícita como un inicio de sesión vinculado o remoto podrán ver los servidores vinculados o remotos para los que tienen un inicio de sesión.  Los siguientes permisos son necesarios para ver todos los servidores vinculados y remotos después de la asignación de inicio de sesión predeterminada:  
  
- `ALTER ANY LINKED SERVER` o `ALTER ANY LOGIN ON SERVER`  
- Pertenencia a los roles fijos de servidor **setupadmin** o **sysadmin**  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Servidores vinculados vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedsrvlogin-transact-sql.md)   
 [sp_addremotelogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)  
  
