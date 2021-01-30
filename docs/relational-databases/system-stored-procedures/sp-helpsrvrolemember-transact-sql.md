---
description: sp_helpsrvrolemember (Transact-SQL)
title: sp_helpsrvrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpsrvrolemember
- sp_helpsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpsrvrolemember
ms.assetid: d0714913-8d6b-4de3-b042-3ae9934f839d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: f17a4be7d5e9b1511a840857c933ed240d0e3d31
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99198695"
---
# <a name="sp_helpsrvrolemember-transact-sql"></a>sp_helpsrvrolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los miembros de un rol fijo de servidor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpsrvrolemember [ [ @srvrolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @srvrolename = ] 'role'` Es el nombre de un rol fijo de servidor. *role* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *role*, el conjunto de resultados incluye información sobre todos los roles fijos de servidor.  
  
 *role* puede ser cualquiera de los siguientes valores.  
  
|Rol fijo de servidor|Descripción|  
|-----------------------|-----------------|  
|sysadmin|Administradores del sistema|  
|securityadmin|Administradores de seguridad|  
|serveradmin|Administradores de servidor|  
|setupadmin|Administradores de instalación|  
|processadmin|Administradores de proceso|  
|diskadmin|Administradores de disco|  
|dbcreator|Creadores de bases de datos|  
|bulkadmin|Puede ejecutar instrucciones BULK INSERT|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|ServerRole|**sysname**|Nombre del rol de servidor|  
|MemberName|**sysname**|Nombre de un miembro de ServerRole|  
|MemberSID|**varbinary(85)**|Identificador de seguridad de MemberName|  
  
## <a name="remarks"></a>Observaciones  
 Para presentar los miembros de un rol de base de datos, utilice sp_helprolemember.  
  
 Todos los inicios de sesión son miembros de Public. sp_helpsrvrolemember no reconoce el rol Public porque, internamente, no [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] implementa Public como role.  
  
 Para agregar o quitar miembros de roles de servidor, vea [ALTER Server ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 sp_helpsrvrolemember no toma como argumento un rol de servidor definido por el usuario. Para determinar los miembros de un rol de servidor definido por el usuario, vea los ejemplos de [ALTER Server role &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se enumeran los miembros del rol fijo de servidor `sysadmin`.  
  
```  
EXEC sp_helpsrvrolemember 'sysadmin';  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_helprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
