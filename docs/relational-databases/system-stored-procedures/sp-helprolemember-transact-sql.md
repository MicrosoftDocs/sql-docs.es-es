---
description: sp_helprolemember (Transact-SQL)
title: sp_helprolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helprolemember_TSQL
- sp_helprolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helprolemember
ms.assetid: 42797510-aa5d-4564-85ac-27418419af9c
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 223d0fe0a72ed72f312e1aba5402ee9c14e9db63
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195091"
---
# <a name="sp_helprolemember-transact-sql"></a>sp_helprolemember (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve información acerca de los miembros directos de un rol de la base de datos actual.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helprolemember [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] ' role '` Es el nombre de un rol de la base de datos actual. *role* es de **tipo sysname y su** valor predeterminado es NULL. el *rol* debe existir en la base de datos actual. Si no se especifica *role* , se devuelven todos los roles que contienen al menos un miembro de la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DbRole**|**sysname**|Nombre del rol en la base de datos actual.|  
|**NombreDeMiembro**|**sysname**|Nombre de un miembro de **DbRole.**|  
|**MemberSID**|**varbinary(85)**|Identificador de seguridad de **memberName.**|  
  
## <a name="remarks"></a>Observaciones  
 Si la base de datos contiene roles anidados, **memberName** puede ser el nombre de un rol. **sp_helprolemember** no muestra la pertenencia obtenida a través de roles anidados. Por ejemplo, si User1 es miembro de Role1 y Role1 es miembro de Role2, `EXEC sp_helprolemember 'Role2'`, devolverá Role1, pero no los miembros de Role1 (User1 en este ejemplo). Para devolver pertenencias anidadas, debe ejecutar **sp_helprolemember** repetidas veces para cada rol anidado.  
  
 Utilice **sp_helpsrvrolemember** para mostrar los miembros de un rol fijo de servidor.  
  
 Use [IS_ROLEMEMBER &#40;&#41;de Transact-SQL ](../../t-sql/functions/is-rolemember-transact-sql.md) para comprobar la pertenencia al rol de un usuario especificado.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestran los miembros del rol `Sales`.  
  
```  
EXEC sp_helprolemember 'Sales';  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helpsrvrolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpsrvrolemember-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
