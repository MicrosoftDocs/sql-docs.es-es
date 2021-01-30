---
description: sp_helpremotelogin (Transact-SQL)
title: sp_helpremotelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpremotelogin_TSQL
- sp_helpremotelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpremotelogin
ms.assetid: 93f50869-2627-4642-899f-8f626f8833f4
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ff291542fee3d10fe94e9ccd628e05c8f9e77f7b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210813"
---
# <a name="sp_helpremotelogin-transact-sql"></a>sp_helpremotelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Presenta información acerca de los inicios de sesión remotos de un servidor remoto específico o acerca de todos los servidores remotos definidos en el servidor local.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepNextDontUse](../../includes/ssnotedepnextdontuse-md.md)] Utilice servidores vinculados y procedimientos almacenados de servidores vinculados en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpremotelogin [ [ @remoteserver = ] 'remoteserver' ]   
     [ , [ @remotename = ] 'remote_name' ]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ @remoteserver **=** ] **'**_ServidorRemoto_*_'_*  
 Es el servidor remoto cuya información de inicio de sesión remoto se va a devolver. *RemoteServer* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *RemoteServer* , se devuelve información acerca de todos los servidores remotos definidos en el servidor local.  
  
 [ @remotename **=** ] **'**_remote_name_*_'_*  
 Es un inicio de sesión remoto específico en el servidor remoto. *remote_name* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *remote_name* , se devuelve información acerca de todos los usuarios remotos definidos para *RemoteServer* .  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|server|**sysname**|Nombre de un servidor remoto definido en el servidor local.|  
|local_user_name|**sysname**|Inicio de sesión del servidor local al que está asociado el inicio de sesión remoto.|  
|remote_user_name|**sysname**|Inicio de sesión del servidor remoto asignado a local_user_name.|  
|opciones|**sysname**|Trusted = El inicio de sesión remoto no necesita contraseña cuando se conecta con el servidor local desde el servidor remoto.<br /><br /> Untrusted (o en blanco) = El inicio de sesión remoto tiene que suministrar una contraseña cuando se conecta con el servidor local desde el servidor remoto.|  
  
## <a name="remarks"></a>Observaciones  
 Utilice sp_helpserver para presentar los nombres de los servidores remotos definidos en el servidor local.  
  
## <a name="permissions"></a>Permisos  
 No se comprueba ningún permiso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-reporting-help-on-a-single-server"></a>A. Presentar información acerca de un solo servidor  
 En el siguiente ejemplo se presenta información acerca de todos los usuarios remotos del servidor remoto `Accounts`.  
  
```  
EXEC sp_helpremotelogin 'Accounts';  
```  
  
### <a name="b-reporting-help-on-all-remote-users"></a>B. Presentar información acerca de todos los usuarios remotos  
 En el siguiente ejemplo se presenta información acerca de todos los usuarios remotos de todos los servidores remotos conocidos por el servidor local.  
  
```  
EXEC sp_helpremotelogin;  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_addremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addremotelogin-transact-sql.md)   
 [sp_dropremotelogin &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dropremotelogin-transact-sql.md)   
 [sp_helpserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpserver-transact-sql.md)   
 [sp_remoteoption &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-remoteoption-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
