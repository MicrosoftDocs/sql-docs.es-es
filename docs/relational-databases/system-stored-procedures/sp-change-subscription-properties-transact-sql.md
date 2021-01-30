---
description: sp_change_subscription_properties (Transact-SQL)
title: sp_change_subscription_properties (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_change_subscription_properties_TSQL
- sp_change_subscription_properties
helpviewer_keywords:
- sp_change_subscription_properties
ms.assetid: cf8137f9-f346-4aa1-ae35-91a2d3c16f17
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ed6615b10487744ae5ac5c0775627bea97e29c66
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208295"
---
# <a name="sp_change_subscription_properties-transact-sql"></a>sp_change_subscription_properties (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Actualiza la información de las suscripciones de extracción. Este procedimiento almacenado se ejecuta en el suscriptor de la base de datos de suscripciones.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_change_subscription_properties [ @publisher = ] 'publisher'  
        , [ @publisher_db = ] 'publisher_db'  
        , [ @publication = ] 'publication'  
        , [ @property = ] 'property'  
        , [ @value = ] 'value'  
    [ , [ @publication_type = ] publication_type ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el nombre del publicador. *Publisher* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publisher_db = ] 'publisher_db'` Es el nombre de la base de datos del publicador. *publisher_db* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @property = ] 'property'` Es la propiedad que se va a cambiar. *Property* es de **tipo sysname**.  
  
`[ @value = ] 'value'` Es el nuevo valor de la propiedad. el *valor* es **nvarchar (1000)** y no tiene ningún valor predeterminado.  
  
`[ @publication_type = ] publication_type` Especifica el tipo de replicación de la publicación. *publication_type* es de **tipo int** y puede tener uno de estos valores.  
  
|Value|Tipo de publicación|  
|-----------|----------------------|  
|**0**|Transaccional|  
|**1**|Instantánea|  
|**2**|Merge|  
|NULL (predeterminado)|La replicación determina el tipo de publicación. Puesto que el procedimiento almacenado debe examinar varias tablas, esta opción es más lenta que cuando se suministra el tipo de publicación exacto.|  
  
 En esta tabla se describen las propiedades de los artículos y los valores de esas propiedades.  
  
|Propiedad|Value|Descripción|  
|--------------|-----------|-----------------|  
|**alt_snapshot_folder**||Especifica la ubicación de la carpeta alternativa de la instantánea. Si el valor es NULL, los archivos de instantáneas se toman de la ubicación predeterminada que se especifica en el publicador.|  
|**distrib_job_login**||Inicio de sesión de la cuenta de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows en la que se ejecuta el agente.|  
|**distrib_job_password**||Contraseña de la cuenta de Windows con la que se ejecuta el agente.|  
|**distributor_login**||Inicio de sesión del distribuidor.|  
|**distributor_password**||Contraseña del distribuidor.|  
|**distributor_security_mode**|**1**|Se utiliza la autenticación de Windows para la conexión con el distribuidor.|  
||**0**|Se utiliza la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la conexión con el distribuidor.|  
|**dts_package_name**||Especifica el nombre del paquete de Servicios de transformación de datos (DTS) de SQL Server 2000. Este valor solo puede especificarse si la publicación es transaccional o de instantáneas.|  
|**dts_package_password**||Especifica la contraseña del paquete. *dts_package_password* es de **tipo sysname y su** valor predeterminado es null, lo que especifica que la propiedad de contraseña se dejará sin cambios.<br /><br /> Nota: un paquete DTS debe tener una contraseña.<br /><br /> Este valor solo puede especificarse si la publicación es transaccional o de instantáneas.|  
|**dts_package_location**||La ubicación donde se almacena el paquete DTS. Este valor solo puede especificarse si la publicación es transaccional o de instantáneas.|  
|**dynamic_snapshot_location**||Especifica la ruta de acceso a la carpeta donde se guardan los archivos de instantáneas. Este valor solo puede especificarse si la publicación es de mezcla.|  
|**ftp_address**||Se conserva únicamente por compatibilidad con versiones anteriores.|  
|**ftp_login**||Se conserva únicamente por compatibilidad con versiones anteriores.|  
|**ftp_password**||Se conserva únicamente por compatibilidad con versiones anteriores.|  
|**ftp_port**||Se conserva únicamente por compatibilidad con versiones anteriores.|  
|**hostname**||Nombre del host utilizado al conectarse al publicador.|  
|**internet_login**||Inicio de sesión que utiliza el Agente de mezcla al conectarse al servidor web que hospeda la sincronización web utilizando autenticación básica.|  
|**internet_password**||Contraseña que el Agente de mezcla utiliza cuando se conecta al servidor web que hospeda la sincronización web a través de la autenticación básica.|  
|**internet_security_mode**|**1**|Se utiliza la autenticación de Windows integrada para la sincronización web. Se recomienda utilizar la autenticación básica con sincronización web. Para más información, consulte [Configure Web Synchronization](../../relational-databases/replication/configure-web-synchronization.md).|  
||**0**|Se utiliza la autenticación básica para la sincronización web.<br /><br /> Nota: la sincronización Web requiere una conexión TLS con el servidor Web.|  
|**internet_timeout**||Período de tiempo, en segundos, antes de que expire la solicitud de sincronización Web.|  
|**internet_url**||URL que representa la ubicación de la escucha de replicación para la sincronización web.|  
|**merge_job_login**||Inicio de sesión de la cuenta de Windows con la que se ejecuta el agente.|  
|**merge_job_password**||Contraseña de la cuenta de Windows con la que se ejecuta el agente.|  
|**publisher_login**||Inicio de sesión del publicador. Solo se admite el cambio de *publisher_login* para las suscripciones a publicaciones de combinación.|  
|**publisher_password**||Contraseña del publicador. Solo se admite el cambio de *publisher_password* para las suscripciones a publicaciones de combinación.|  
|**publisher_security_mode**|**1**|Se utiliza la autenticación de Windows para la conexión con el publicador. Solo se admite el cambio de *publisher_security_mode* para las suscripciones a publicaciones de combinación.|  
||**0**|Se utiliza la autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para la conexión con el publicador.|  
|**use_ftp**|**true**|Use FTP en lugar del protocolo regular para recuperar las instantáneas.|  
||**false**|Se utiliza el protocolo habitual para recuperar instantáneas.|  
|**use_web_sync**|**true**|Habilita la sincronización web.|  
||**false**|Deshabilita la sincronización web.|  
|**working_directory**||Nombre del directorio de trabajo utilizado para almacenar temporalmente archivos de datos y de esquema para la publicación cuando se utiliza el protocolo de transferencia de archivos (FTP) para transferir archivos de instantáneas.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_change_subscription_properties** se utiliza en todos los tipos de replicación.  
  
 **sp_change_subscription_properties** se utiliza para las suscripciones de extracción.  
  
 En el caso de los publicadores de Oracle, se omite el valor de *publisher_db* porque Oracle solo permite una base de datos por cada instancia del servidor.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_change_subscription_properties**.  
  
## <a name="see-also"></a>Consulte también  
 [View and Modify Pull Subscription Properties](../../relational-databases/replication/view-and-modify-pull-subscription-properties.md)  (Ver y modificar las propiedades de una suscripción de extracción)  
 [sp_addmergepullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md)   
 [sp_addmergepullsubscription_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)   
 [sp_addpullsubscription &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-transact-sql.md)   
 [sp_addpullsubscription_agent &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
