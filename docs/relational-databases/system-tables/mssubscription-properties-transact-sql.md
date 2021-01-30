---
description: MSsubscription_properties (Transact-SQL)
title: MSsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- MSsubscription_properties
- MSsubscription_properties_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- MSsubscription_properties system table
ms.assetid: f96fc1ae-b798-4b05-82a7-564ae6ef23b8
author: cawrites
ms.author: chadam
ms.openlocfilehash: 570bfade322ef3b3747619cf2a0779a3cf6ec392
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99102950"
---
# <a name="mssubscription_properties-transact-sql"></a>MSsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla **MSsubscription_properties** contiene filas para la información de parámetros necesaria para ejecutar agentes de replicación en el suscriptor. Esta tabla se almacena en la base de datos de suscripciones en el Suscriptor para una suscripción de extracción o en la base de datos de distribución en el Distribuidor para una suscripción de inserción.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|El nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publication_type**|**int**|Tipo de publicación:<br /><br /> **0** = transaccional.<br /><br /> **2** = fusionar mediante combinación.|  
|**publisher_login**|**sysname**|Identificador de inicio de sesión utilizado en el publicador para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Contraseña (cifrada) utilizada en el publicador para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_security_mode**|**int**|Modo de seguridad aplicado en el publicador:<br /><br /> **0**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server la autenticación.<br /><br /> **1**  =  [!INCLUDE[msCoName](../../includes/msconame-md.md)] autenticación de Windows.<br /><br /> **2** = los desencadenadores de sincronización utilizan una entrada **sysservers** estática para realizar una llamada a procedimiento remoto (RPC) y el *publicador* debe definirse en la tabla **sysservers** como un servidor remoto o un servidor vinculado.|  
|**distribuidor**|**sysname**|Nombre del distribuidor.|  
|**distributor_login**|**sysname**|Id. de inicio de sesión utilizado en el distribuidor para la autenticación de SQL Server.|  
|**distributor_password**|**nvarchar (524)**|Contraseña (cifrada) utilizada en el distribuidor para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**distributor_security_mode**|**int**|Modo de seguridad aplicado en el distribuidor:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.<br /><br /> **1** = autenticación de Windows.|  
|**ftp_address**|**sysname**|Dirección de red del servicio FTP (Protocolo de transferencia de archivos) del distribuidor.|  
|**ftp_port**|**int**|El número de puerto del servicio FTP para el distribuidor.|  
|**ftp_login**|**sysname**|El nombre de usuario utilizado para conectarse al servicio FTP.|  
|**ftp_password**|**nvarchar (524)**|La contraseña de usuario u. User usada para conectarse al servicio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|**working_directory**|**nvarchar(255)**|Nombre del directorio de trabajo utilizado para almacenar los archivos de datos y de esquema.|  
|**use_ftp**|**bit**|Especifica que se utilizará FTP en lugar del protocolo regular para recuperar las instantáneas. Si es **1**, se utiliza FTP.|  
|**dts_package_name**|**sysname**|Especifica el nombre del paquete de Servicios de transformación de datos (DTS).|  
|**dts_package_password**|**nvarchar (524)**|Especifica la contraseña del paquete.|  
|**dts_package_location**|**int**|Ubicación donde se almacena el paquete DTS.|  
|**enabled_for_syncmgr**|**bit**|Especifica si la suscripción se puede sincronizar mediante el Administrador de sincronización de [!INCLUDE[msCoName](../../includes/msconame-md.md)].<br /><br /> **0** = la suscripción no está registrada en el administrador de sincronización.<br /><br /> **1** = la suscripción está registrada en el administrador de sincronización y se puede sincronizar sin iniciar [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] .|  
|**offload_agent**|**bit**|Especifica si el agente puede activarse de manera remota. Si es **0**, no se puede activar el agente de forma remota.|  
|**offload_server**|**sysname**|Especifica el nombre de red del servidor utilizado para la activación remota.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Especifica la ruta de acceso a la carpeta donde se guardan los archivos de instantáneas.|  
|**use_web_sync**|**bit**|Especifica si la suscripción puede sincronizarse mediante HTTP. Un valor de **1** significa que esta característica está habilitada.|  
|**internet_url**|**nvarchar(260)**|Dirección URL que representa la ubicación de la escucha de replicación de la sincronización web.|  
|**internet_login**|**sysname**|Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor Web que hospeda la sincronización Web mediante la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación de.|  
|**internet_password**|**nvarchar (524)**|Contraseña del inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor Web que hospeda la sincronización Web mediante la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación de.|  
|**internet_security_mode**|**int**|El modo de autenticación utilizado al conectarse al servidor Web que hospeda la sincronización Web, donde un valor de **1** significa autenticación de Windows y un valor de **0** significa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**internet_timeout**|**int**|Tiempo que transcurre, en segundos, hasta que expira una solicitud de sincronización web.|  
|**hostname**|**sysname**|Especifica el valor de **host_name** cuando esta función se utiliza en la cláusula **Where** de un filtro de combinación o una relación de registros lógicos.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_helppullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md)   
 [sp_helpsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md)   
 [sp_helpsubscription_properties &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md)  
  
  
