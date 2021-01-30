---
description: sp_helpsubscription_properties (Transact-SQL)
title: sp_helpsubscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpsubscription_properties
- sp_helpsubscription_properties_TSQL
helpviewer_keywords:
- sp_helpsubscription_properties
ms.assetid: 7a76a645-97eb-47ac-b3ea-e2d75012cbed
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 681a72a604af765311bb10833cc17842819e97f1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99103290"
---
# <a name="sp_helpsubscription_properties-transact-sql"></a>sp_helpsubscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Recupera información de seguridad de la tabla [MSsubscription_properties](../../relational-databases/system-tables/mssubscription-properties-transact-sql.md) . Este procedimiento almacenado se ejecuta en el suscriptor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpsubscription_properties [ [ @publisher = ] 'publisher' ]  
    [ , [ @publisher_db =] 'publisher_db' ]   
    [ , [ @publication =] 'publication' ]  
    [ , [ @publication_type = ] publication_type ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información sobre todos los publicadores.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información sobre todas las bases de datos del publicador.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname y su** valor predeterminado es **%** , que devuelve información sobre todas las publicaciones.  
  
`[ @publication_type = ] publication_type` Es el tipo de publicación. *publication_type* es de **tipo int** y su valor predeterminado es NULL. Si se proporciona, *publication_type* debe ser uno de los siguientes valores:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**0**|Publicación transaccional|  
|**1**|Publicación de instantáneas|  
|**2**|Publicación de combinación|  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**publisher**|**sysname**|Nombre del publicador.|  
|**publisher_db**|**sysname**|Nombre de la base de datos del publicador.|  
|**publicaciones**|**sysname**|Nombre de la publicación.|  
|**publication_type**|**int**|Tipo de publicación:<br /><br /> **0** = transaccional<br /><br /> **1** = instantánea<br /><br /> **2** = fusionar mediante combinación|  
|**publisher_login**|**sysname**|Id. de inicio de sesión utilizado en el publicador para la Autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**publisher_password**|**nvarchar (524)**|Contraseña utilizada en el publicador para la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (cifrada).|  
|**publisher_security_mode**|**int**|Modo de seguridad utilizado en el publicador.<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows|  
|**distribuidor**|**sysname**|Nombre del distribuidor.|  
|**distributor_login**|**sysname**|Inicio de sesión del distribuidor.|  
|**distributor_password**|**nvarchar (524)**|Contraseña del distribuidor (cifrada).|  
|**distributor_security_mode**|**int**|Modo de seguridad utilizado en el distribuidor:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows|  
|**ftp_address**|**sysname**|Se conserva únicamente por compatibilidad con versiones anteriores. Dirección de red del servicio de protocolo de transferencia de archivos (FTP) para el distribuidor.|  
|**ftp_port**|**int**|Se conserva únicamente por compatibilidad con versiones anteriores. Número de puerto del servicio FTP para el distribuidor.|  
|**ftp_login**|**sysname**|Se conserva únicamente por compatibilidad con versiones anteriores. Nombre de usuario que se utiliza para conectar con el servicio FTP.|  
|**ftp_password**|**nvarchar (524)**|Se conserva únicamente por compatibilidad con versiones anteriores. Contraseña de usuario que se utiliza para conectar con el servicio FTP.|  
|**alt_snapshot_folder**|**nvarchar(255)**|Especifica la ubicación de la carpeta alternativa de la instantánea.|  
|**working_directory**|**nvarchar(255)**|Nombre del directorio de trabajo utilizado para almacenar archivos de datos y de esquema.|  
|**use_ftp**|**bit**|Especifica que se utilizará FTP en lugar del protocolo regular para recuperar las instantáneas. Si es **1**, se utiliza FTP.|  
|**dts_package_name**|**sysame**|Especifica el nombre del paquete de Servicios de transformación de datos (DTS).|  
|**dts_package_password**|**nvarchar (524)**|Especifica la contraseña del paquete, si procede.|  
|**dts_package_location**|**int**|La ubicación donde se almacena el paquete DTS.<br /><br /> **0** = la ubicación del paquete está en el distribuidor.<br /><br /> **1** = la ubicación del paquete está en el suscriptor.|  
|**offload_agent**|**bit**|Especifica si el agente puede activarse de forma remota. Si es **0**, no se puede activar el agente de forma remota.|  
|**offload_server**|**sysname**|Especifica el nombre de red del servidor utilizado para la activación remota.|  
|**dynamic_snapshot_location**|**nvarchar(255)**|Especifica la ruta de acceso a la carpeta donde se guardan los archivos de instantáneas.|  
|**use_web_sync**|**bit**|Especifica si la suscripción se puede sincronizar a través de HTTPS, donde un valor de **1** significa que esta característica está habilitada.|  
|**internet_url**|**nvarchar(260)**|URL que representa la ubicación de la escucha de replicación para la sincronización web.|  
|**internet_login**|**nvarchar(128)**|Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor web que hospeda la sincronización web utilizando autenticación básica.|  
|**internet_password**|**nvarchar (524)**|Contraseña para el Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor web que hospeda la sincronización web utilizando autenticación básica.|  
|**internet_security_mode**|**int**|El modo de autenticación utilizado al conectarse al servidor Web que hospeda la sincronización Web, donde un valor de **1** significa autenticación de Windows y un valor de **0** significa autenticación básica.|  
|**internet_timeout**|**int**|Período de tiempo, en segundos, antes de que expire la solicitud de sincronización Web.|  
|**hostname**|**nvarchar(128)**|Especifica el valor de HOST_NAME() cuando se utiliza esta función en el filtro de fila con parámetros de la cláusula WHERE.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpsubscription_properties** se utiliza en la replicación de instantáneas, la replicación transaccional y la replicación de mezcla.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_helpsubscription_properties**.  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
