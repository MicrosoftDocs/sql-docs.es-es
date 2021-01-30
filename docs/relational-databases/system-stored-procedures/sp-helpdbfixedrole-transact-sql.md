---
description: sp_helpdbfixedrole (Transact-SQL)
title: sp_helpdbfixedrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_helpdbfixedrole
- sp_helpdbfixedrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_helpdbfixedrole
ms.assetid: ad87e9a0-b901-4e37-9950-aa517d680fc3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 817947901ca7f35ba2972004d23ed8d7f85215ed
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176426"
---
# <a name="sp_helpdbfixedrole-transact-sql"></a>sp_helpdbfixedrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la lista de los roles fijos de base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helpdbfixedrole [ [ @rolename = ] 'role' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] 'role'` Es el nombre de un rol fijo de base de datos. *role* es de **tipo sysname y su** valor predeterminado es NULL. Si se especifica *role* , solo se devuelve información sobre ese rol; de lo contrario, se devolverá una lista y una descripción de todos los roles fijos de base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**DbFixedRole**|**sysname**|Nombre del rol fijo de base de datos.|  
|**Descripción**|**nvarchar (70)**|Descripción de **DbFixedRole.**|  
  
## <a name="remarks"></a>Observaciones  
 Los roles fijos de base de datos, como se muestra en la siguiente tabla, se definen de nivel de base de datos y tienen permisos para realizar actividades administrativas específicas en la base de datos. No es posible agregar o quitar roles fijos de base de datos. No es posible cambiar los permisos concedidos a un rol fijo de base de datos.  
  
|Rol fijo de base de datos|Descripción|  
|-------------------------|-----------------|  
|**db_owner**|Propietarios de base de datos|  
|**db_accessadmin**|Administradores de acceso a la base de datos|  
|**db_securityadmin**|Administradores de seguridad de la base de datos|  
|**db_ddladmin**|Administradores de DDL de la base de datos|  
|**db_backupoperator**|Operadores de copia de seguridad de la base de datos|  
|**db_datareader**|Lectores de datos de la base de datos|  
|**db_datawriter**|Escritores de datos de la base de datos|  
|**db_denydatareader**|Lectores de datos denegados de la base de datos|  
|**db_denydatawriter**|Escritores de datos denegados de la base de datos|  
  
 En la siguiente tabla se muestran los procedimientos almacenados que se utilizan para modificar los roles de base de datos.  
  
|Procedimiento almacenado|Acción|  
|----------------------|------------|  
|**sp_addrolemember**|Agrega un usuario de base de datos a un rol fijo de base de datos.|  
|**sp_helprole**|Presenta la lista de los miembros de un rol fijo de base de datos.|  
|**sp_droprolemember**|Quita un miembro de un rol fijo de base de datos.|  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** .  
  
 La información mostrada está sometida a restricciones de acceso a los metadatos. No se mostrarán las entidades en las que la entidad de seguridad no tiene permiso. Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra la lista de todos los roles fijos de base de datos.  
  
```  
EXEC sp_helpdbfixedrole;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sp_dbfixedrolepermission &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-dbfixedrolepermission-transact-sql.md)   
 [sp_droprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_helprole &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprole-transact-sql.md)   
 [sp_helprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helprolemember-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
