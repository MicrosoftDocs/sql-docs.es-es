---
description: sp_dbfixedrolepermission (Transact-SQL)
title: sp_dbfixedrolepermission (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dbfixedrolepermission
- sp_dbfixedrolepermission_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dbfixedrolepermission
ms.assetid: b8c30191-f532-49cd-83f3-c271f63ce572
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 78f1381d05c5841ce48e395f8cc1cd477b820e01
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201279"
---
# <a name="sp_dbfixedrolepermission-transact-sql"></a>sp_dbfixedrolepermission (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra los permisos de un rol fijo de base de datos. **sp_dbfixedrolepermission** devuelve la información correcta en [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . El resultado no refleja los cambios en la jerarquía de permisos que se implementaron en [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] . Para obtener más información, vea [roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md#fixed-database-roles), que muestra una lista de roles fijos de base de datos y sus permisos correspondientes.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dbfixedrolepermission [ [ @rolename = ] 'role' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] 'role'` Es el nombre de un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] rol fijo de base de datos válido. *role* es de **tipo sysname y su** valor predeterminado es NULL. Si no se especifica *role* , se muestran los permisos de todos los roles fijos de base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nombre del rol fijo de base de datos|  
|**Permiso**|**nvarchar (70)**|Permisos asociados a **DbFixedRole**|  
  
## <a name="remarks"></a>Observaciones  
 Para mostrar una lista de los roles fijos de base de datos, ejecute **sp_helpdbfixedrole**. En la tabla siguiente se muestran los roles fijos de base de datos.  
  
|Rol fijo de base de datos|Descripción|  
|-------------------------|-----------------|  
|**db_owner**|Propietarios de base de datos|  
|**db_accessadmin**|Administradores de acceso a la base de datos|  
|**db_securityadmin**|Administradores de seguridad de la base de datos|  
|**db_ddladmin**|Administradores del Lenguaje de definición de datos (DDL, Data Definition Language) de base de datos |  
|**db_backupoperator**|Operadores de copia de seguridad de la base de datos|  
|**db_datareader**|Lectores de datos de la base de datos|  
|**db_datawriter**|Escritores de datos de la base de datos|  
|**db_denydatareader**|Lectores de datos denegados de la base de datos|  
|**db_denydatawriter**|Escritores de datos denegados de la base de datos|  
  
 Los miembros del rol fijo de base de datos **db_owner** tienen los permisos de todos los demás roles fijos de base de datos. Para mostrar los permisos de los roles fijos de servidor, ejecute **sp_srvrolepermission**.  
  
 El conjunto de resultados contiene las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] que se pueden ejecutar y otras actividades especiales que pueden realizar los miembros del rol de base de datos.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
## <a name="examples"></a>Ejemplos  
 En la siguiente consulta se devuelven los permisos de todos los roles fijos de base de datos porque no se especifica un rol fijo de base de datos.  
  
```  
EXEC sp_dbfixedrolepermission;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helpdbfixedrole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpdbfixedrole-transact-sql.md)   
 [sp_srvrolepermission &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-srvrolepermission-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
