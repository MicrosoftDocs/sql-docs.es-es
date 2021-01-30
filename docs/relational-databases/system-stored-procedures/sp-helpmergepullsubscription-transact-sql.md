---
description: sp_helpmergepullsubscription (Transact-SQL)
title: sp_helpmergepullsubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpmergepullsubscription
- sp_helpmergepullsubscription_TSQL
helpviewer_keywords:
- sp_helpmergepullsubscription
ms.assetid: 6f3125f3-0dfa-40bd-b725-8aa1591234f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 91266e8d825fd669979e925753804230fd62027c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160596"
---
# <a name="sp_helpmergepullsubscription-transact-sql"></a>sp_helpmergepullsubscription (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de las suscripciones de extracción que existen en un suscriptor. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpmergepullsubscription [ [ @publication=] 'publication']  
    [ , [ @publisher=] 'publisher']  
    [ , [ @publisher_db=] 'publisher_db']  
    [ , [ @subscription_type=] 'subscription_type']  
```  
  
## <a name="argument"></a>Argumento  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** . Si la *publicación* es **%** , se devuelve información acerca de todas las publicaciones y suscripciones de combinación en la base de datos actual.  
  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @subscription_type = ] 'subscription_type'` Indica si se van a mostrar las suscripciones de extracción. *subscription_type* es de tipo **nvarchar (10)** y su valor predeterminado es **' pull '**. Los valores válidos son **' Inserte '**, **' pull '** o **' both '**.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**subscription_name**|**nvarchar(1000)**|Nombre de la suscripción.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publisher**|**sysname**|Nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**suscriptor**|**sysname**|Nombre del suscriptor.|  
|**subscription_db**|**sysname**|Nombre de la base de datos de suscripciones.|  
|**status**|**int**|Estado de la suscripción:<br /><br /> **0** = suscripción inactiva<br /><br /> **1** = suscripción activa<br /><br /> **2** = suscripción eliminada<br /><br /> **3** = suscripción desasociada<br /><br /> **4** = suscripción adjunta<br /><br /> **5** = la suscripción se ha marcado para reinicializarla con carga<br /><br /> **6** = error al adjuntar la suscripción<br /><br /> **7** = suscripción restaurada desde la copia de seguridad|  
|**subscriber_type**|**int**|Tipo de suscriptor:<br /><br /> **1** = global<br /><br /> **2** = local<br /><br /> **3** = anónimo|  
|**subscription_type**|**int**|Tipo de suscripción:<br /><br /> **0** = inserciones<br /><br /> **1** = extracción<br /><br /> **2** = anónimo|  
|**priority**|**Float (8)**|Prioridad de la suscripción. El valor debe ser menor que **100,00**.|  
|**sync_type**|**tinyint**|Tipo de sincronización de suscripción:<br /><br /> **1** = automática<br /><br /> **2** = no se usa la instantánea.|  
|**description**|**nvarchar(255)**|Breve descripción de la suscripción de extracción.|  
|**merge_jobid**|**binario (16)**|Id. de trabajo del Agente de mezcla.|  
|**enabled_for_syncmgr**|**int**|Indica si la suscripción se puede sincronizar mediante el Administrador de sincronización de [!INCLUDE[msCoName](../../includes/msconame-md.md)].|  
|**last_updated**|**nvarchar (26)**|Hora a la que el Agente de mezcla sincronizó correctamente la suscripción por última vez.|  
|**publisher_login**|**sysname**|Nombre de inicio de sesión del publicador.|  
|**publisher_password**|**sysname**|Contraseña del publicador.|  
|**publisher_security_mode**|**int**|Especifica el modo de seguridad del publicador:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows|  
|**distribuidor**|**sysname**|Nombre del distribuidor.|  
|**distributor_login**|**sysname**|Nombre de inicio de sesión del distribuidor.|  
|**distributor_password**|**sysname**|Contraseña del distribuidor.|  
|**distributor_security_mode**|**int**|Especifica el modo de seguridad del distribuidor:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows|  
|**ftp_address**|**sysname**|Disponible únicamente por compatibilidad con versiones anteriores. Es la dirección de red del servicio de protocolo de transferencia de archivos (FTP) para el distribuidor.|  
|**ftp_port**|**int**|Disponible únicamente por compatibilidad con versiones anteriores. Es el número de puerto del servicio FTP para el distribuidor.|  
|**ftp_login**|**sysname**|Disponible únicamente por compatibilidad con versiones anteriores. Es el nombre de usuario utilizado para conectarse al servicio FTP.|  
|**ftp_password**|**sysname**|Disponible únicamente por compatibilidad con versiones anteriores. Es la contraseña del usuario que se utiliza para conectarse al servicio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Ubicación donde se almacena la carpeta de instantáneas si no es la ubicación predeterminada o es complementaria a ésta.|  
|**working_directory**|**nvarchar(255)**|Ruta de acceso completa al directorio donde se transfieren los archivos de instantáneas por medio de FTP, cuando se especifica esa opción.|  
|**use_ftp**|**bit**|La suscripción se suscribe a la publicación a través de Internet y se configuran las propiedades de direccionamiento FTP. Si es **0**, la suscripción no utiliza FTP. Si es **1**, la suscripción utiliza FTP.|  
|**offload_agent**|**bit**|Especifica si el agente se puede activar y ejecutar de manera remota. Si es **0**, no se puede activar el agente de forma remota.|  
|**offload_server**|**sysname**|Nombre del servidor utilizado para la activación remota.|  
|**use_interactive_resolver**|**int**|Devuelve si se utiliza o no el solucionador interactivo durante la reconciliación. Si es **0**, no se utiliza el solucionador interactivo.|  
|**Subid**|**uniqueidentifier**|Id. del suscriptor.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Ruta de acceso de la carpeta donde se guardan los archivos de instantáneas.|  
|**last_sync_status**|**int**|Estado de sincronización:<br /><br /> **1** = iniciando<br /><br /> **2** = correcto<br /><br /> **3** = en curso<br /><br /> **4** = inactivo<br /><br /> **5** = reintentando después de un error anterior<br /><br /> **6** = error<br /><br /> **7** = error de validación<br /><br /> **8** = validación superada<br /><br /> **9** = se solicitó un cierre|  
|**last_sync_summary**|**sysname**|Descripción de los últimos resultados de la sincronización.|  
|**use_web_sync**|**bit**|Especifica si la suscripción se puede sincronizar a través de HTTPS, donde un valor de **1** significa que esta característica está habilitada.|  
|**internet_url**|**nvarchar(260)**|URL que representa la ubicación de la escucha de replicación para la sincronización web.|  
|**internet_login**|**nvarchar(128)**|Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor web que hospeda la sincronización web utilizando autenticación básica.|  
|**internet_password**|**nvarchar (524)**|Contraseña para el Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor web que hospeda la sincronización web utilizando autenticación básica.|  
|**internet_security_mode**|**int**|Modo de autenticación utilizado al conectarse al servidor web que hospeda la sincronización web. Un valor de **1** significa autenticación de Windows y un valor de **0** significa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación.|  
|**internet_timeout**|**int**|Período de tiempo, en segundos, antes de que expire la solicitud de sincronización Web.|  
|**hostname**|**nvarchar(128)**|Especifica un valor sobrecargado para [host_name](../../t-sql/functions/host-name-transact-sql.md) cuando esta función se utiliza en la cláusula WHERE de un filtro de fila con parámetros.|  
|**job_login**|**nvarchar(512)**|Es la cuenta de Windows con la que se ejecuta el agente de mezcla, que se devuelve con el formato *dominio* \\ *nombreDeUsuario*.|  
|**job_password**|**sysname**|Por motivos de seguridad, siempre se devuelve un valor de " **\*\*\*\*\*\*\*\*\*\*** ".|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpmergepullsubscription** se utiliza en la replicación de mezcla. En el conjunto de resultados, la fecha devuelta en **LAST_UPDATED** tiene el formato *AAAAMMDD hh: mm: SS. FFF*.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** y del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpmergepullsubscription**.  
  
## <a name="see-also"></a>Consulte también  
 [sp_addmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_changemergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md)   
 [sp_dropmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
