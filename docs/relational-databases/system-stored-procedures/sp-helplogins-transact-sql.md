---
description: sp_helplogins (Transact-SQL)
title: sp_helplogins (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helplogins_TSQL
- sp_helplogins
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helplogins
ms.assetid: f9ad3767-5b9f-420d-8922-b637811404f7
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b58e74a30ddd86fc2edbfc8db553ddf9449181db
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183123"
---
# <a name="sp_helplogins-transact-sql"></a>sp_helplogins (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información acerca de inicios de sesión y sus usuarios asociados en cada base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helplogins [ [ @LoginNamePattern = ] 'login' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @LoginNamePattern = ] 'login'` Es un nombre de inicio de sesión. *login* es de tipo **sysname** y su valor predeterminado es NULL. el *Inicio de sesión* debe existir si se especifica. Si no se especifica *login* , se devuelve información acerca de todos los inicios de sesión.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 El primer informe contiene información acerca de cada inicio de sesión especificado, tal como se muestra en la tabla siguiente.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nombre de inicio de sesión.|  
|**SID**|**varbinary(85)**|Identificador de seguridad (SID) del inicio de sesión.|  
|**DefDBName**|**sysname**|Base de datos predeterminada que **LoginName** usa al conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**DefLangName**|**sysname**|Idioma predeterminado usado por **LoginName**.|  
|**Auser**|**Char (5)**|Sí = **LoginName** tiene un nombre de usuario asociado en una base de datos.<br /><br /> No = **LoginName** no tiene un nombre de usuario asociado.|  
|**ARemote**|**Char (7)**|Sí = **LoginName** tiene asociado un inicio de sesión remoto.<br /><br /> No = **LoginName** no tiene un inicio de sesión asociado.|  
  
 El segundo informe contiene información sobre los usuarios asignados a cada inicio de sesión y las pertenencias a roles del inicio de sesión como se muestra en la tabla siguiente.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**LoginName**|**sysname**|Nombre de inicio de sesión.|  
|**Nombrebd**|**sysname**|Base de datos predeterminada que **LoginName** usa al conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|**UserName**|**sysname**|Cuenta de usuario a la que **LoginName** está asignada en **dbname** y los roles de los que **LoginName** es miembro en **dbname**.|  
|**UserOrAlias**|**Char (8)**|MemberOf = **username** es un rol.<br /><br /> User = **username** es una cuenta de usuario.|  
  
## <a name="remarks"></a>Observaciones  
 Antes de quitar un inicio de sesión, utilice **sp_helplogins** para identificar las cuentas de usuario que se asignan al inicio de sesión.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor **securityadmin** .  
  
 Para identificar todas las cuentas de usuario asignadas a un inicio de sesión determinado, **sp_helplogins** debe comprobar todas las bases de datos del servidor. Por lo tanto, en todas las bases de datos del servidor se tiene que dar, como mínimo, una de las condiciones siguientes:  
  
-   El usuario que ejecuta **sp_helplogins** tiene permiso para obtener acceso a la base de datos.  
  
-   La cuenta de usuario **invitado** está habilitada en la base de datos.  
  
 Si **sp_helplogins** no puede tener acceso a una base de datos, **sp_helplogins** devolverá toda la información que pueda y mostrará el mensaje de error 15622.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se proporciona información sobre el inicio de sesión `John`.  
  
```  
EXEC sp_helplogins 'John';  
GO  
  
LoginName SID                        DefDBName DefLangName AUser ARemote   
--------- -------------------------- --------- ----------- ----- -------   
John      0x23B348613497D11190C100C  master    us_english  yes   no  
  
(1 row(s) affected)  
  
LoginName   DBName   UserName   UserOrAlias   
---------   ------   --------   -----------   
John        pubs     John       User          
  
(1 row(s) affected)  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_helpdb &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdb-transact-sql.md)   
 [sp_helpuser &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpuser-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
