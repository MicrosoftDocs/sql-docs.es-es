---
description: sp_helpdistpublisher (Transact-SQL)
title: sp_helpdistpublisher (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helpdistpublisher_TSQL
- sp_helpdistpublisher
helpviewer_keywords:
- sp_helpdistpublisher
ms.assetid: f207c22d-8fb2-4756-8a9d-6c51d6cd3470
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 461a186656cbef0d1a83eecb2a3e58d63ab896ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209309"
---
# <a name="sp_helpdistpublisher-transact-sql"></a>sp_helpdistpublisher (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Devuelve las propiedades de los publicadores que utilizan un distribuidor. Este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpdistpublisher [ [ @publisher=] 'publisher']   
    [ , [ @check_user = ] check_user  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publisher = ] 'publisher'` Es el publicador cuyas propiedades se devuelven. *Publisher* es de **tipo sysname y su** valor predeterminado es **%** .  
  
`[ @check_user = ] check_user` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del publicador.|  
|**distribution_db**|**sysname**|Base de datos de distribución del publicador especificado.|  
|**security_mode**|**int**|Modo de seguridad empleado por los agentes de replicación para conectar al publicador para suscripciones de actualización en cola, o con un publicador que no sea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] autenticación<br /><br /> **1** = autenticación de Windows|  
|**login**|**sysname**|Nombre de inicio de sesión empleado por los agentes de replicación para conectar al publicador para suscripciones de actualización en cola, o con un publicador que no sea de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**password**|**nvarchar (524)**|Contraseña devuelta (en forma de cifrado sencillo). La contraseña es NULL para los usuarios que no sean **sysadmin**.|  
|**active**|**bit**|Indica si un publicador remoto utiliza el servidor local como distribuidor:<br /><br /> **0** = No<br /><br /> **1** = Sí|  
|**working_directory**|**nvarchar(255)**|Nombre del directorio de trabajo.|  
|**trusted**|**bit**|Si se necesita la contraseña cuando un publicador se conecta con el distribuidor. En [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] y versiones posteriores, siempre debe devolver **0**, lo que significa que se necesita la contraseña.|  
|**thirdparty_flag**|**bit**|Indica si la publicación está habilitada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o por una aplicación de terceros:<br /><br /> **0**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , Oracle o el publicador de puerta de enlace de Oracle.<br /><br /> **1** = el publicador se ha integrado con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante una aplicación de otro fabricante.|  
|**publisher_type**|**sysname**|Tipo de publicador; puede ser uno de los siguientes:<br /><br /> **MSSQLSERVER**<br /><br /> **ORACLE**<br /><br /> **ORACLE GATEWAY**|  
|**publisher_data_source**|**nvarchar(4000)**|Nombre del origen de datos OLE DB en el publicador.|  
|**storage_connection_string**|**nvarchar(4000)**|Clave de acceso de almacenamiento para el directorio de trabajo cuando el distribuidor o el publicador en Azure SQL Database.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helpdistpublisher** se utiliza en todos los tipos de replicación.  
  
 **sp_helpdistpublisher** no mostrará el inicio de sesión o la contraseña del publicador en el conjunto de resultados para inicios de sesión que no sean de **sysadmin** .  
  
## <a name="permissions"></a>Permisos  
 Los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **Sp_helpdistpublisher** para cualquier publicador que use el servidor local como distribuidor. Los miembros del rol fijo de base de datos **db_owner** o el rol **replmonitor** en una base de datos de distribución pueden ejecutar **sp_helpdistpublisher** para cualquier publicador que use esa base de datos de distribución. Los usuarios de la lista de acceso a la publicación para una publicación en el *publicador* especificado pueden ejecutar **sp_helpdistpublisher**. Si no se especifica *Publisher* , se devuelve información para todos los publicadores para los que el usuario tiene derechos de acceso.  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del distribuidor y del publicador](../../relational-databases/replication/view-and-modify-distributor-and-publisher-properties.md)   
 [sp_adddistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-adddistpublisher-transact-sql.md)   
 [sp_changedistpublisher &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedistpublisher-transact-sql.md)   
 [sp_dropdistpublisher &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropdistpublisher-transact-sql.md)  
  
  
