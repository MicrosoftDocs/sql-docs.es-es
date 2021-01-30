---
description: sp_helprole (Transact-SQL)
title: sp_helprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprole_TSQL
- sp_helprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprole
ms.assetid: b023103f-ccf3-44e2-b418-4be9bdd49f4a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 24eca96d2d3c1b7535607fedb2cd3a04961a29df
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210783"
---
# <a name="sp_helprole-transact-sql"></a>sp_helprole (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve información acerca de los roles de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helprole [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] 'role'` Es el nombre de un rol de la base de datos actual. *role* es de **tipo sysname y su** valor predeterminado es NULL. el *rol* debe existir en la base de datos actual. Si no se especifica *role* , se devuelve información acerca de todos los roles de la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**RoleName**|**sysname**|Nombre del rol en la base de datos actual.|  
|**RoleId**|**smallint**|IDENTIFICADOR de **RoleName**.|  
|**IsAppRole**|**int**|0 = **RoleName** no es un rol de aplicación.<br /><br /> 1 = **RoleName** es un rol de aplicación.|  
  
## <a name="remarks"></a>Observaciones  
 Para ver los permisos asociados con el rol, use **sp_helprotect**. Para ver los miembros de un rol de base de datos, use **sp_helprolemember**.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En la siguiente consulta se devuelven todos los roles de la base de datos actual.  
  
```  
EXEC sp_helprole  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [Roles de nivel de servidor](../../relational-databases/security/authentication-access/server-level-roles.md)   
 [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md)   
 [sp_addapprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addapprole-transact-sql.md)   
 [sp_addrole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrole-transact-sql.md)   
 [sp_droprole &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droprole-transact-sql.md)   
 [sp_helprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [sp_helpsrvrolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
