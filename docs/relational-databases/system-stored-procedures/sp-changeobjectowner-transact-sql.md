---
description: sp_changeobjectowner (Transact-SQL)
title: sp_changeobjectowner (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_changeobjectowner_TSQL
- sp_changeobjectowner
dev_langs:
- TSQL
helpviewer_keywords:
- sp_changeobjectowner
ms.assetid: 45b3dc1c-1cde-45b7-a248-5195c12973e9
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 482fcbdb2d7c1b79eb0a8b5ed88d1295785b994b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159825"
---
# <a name="sp_changeobjectowner-transact-sql"></a>sp_changeobjectowner (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cambia el propietario de un objeto en la base de datos actual.  
  
> [!IMPORTANT]
>  Este procedimiento almacenado solo funciona con los objetos disponibles en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] . [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Use [ALTER Schema](../../t-sql/statements/alter-schema-transact-sql.md) o [ALTER Authorization](../../t-sql/statements/alter-authorization-transact-sql.md) en su lugar. **sp_changeobjectowner** cambia tanto el esquema como el propietario. Para mantener la compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], este procedimiento almacenado solo cambia los propietarios de los objetos cuando el propietario actual y el nuevo poseen esquemas con el mismo nombre que sus nombres de usuario de base de datos.  
> 
> [!IMPORTANT]
>  Se ha agregado un nuevo requisito a los permisos de este procedimiento almacenado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_changeobjectowner [ @objname = ] 'object' , [ @newowner = ] 'owner'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @objname = ] 'object'` Es el nombre de una tabla, vista, función definida por el usuario o procedimiento almacenado existente en la base de datos actual. el *objeto* es un **nvarchar (776)** y no tiene ningún valor predeterminado. el *objeto* se puede calificar con el propietario del objeto existente, con el formato _existing_owner_**.** _objeto_ si el esquema y su propietario tienen el mismo nombre.  
  
`[ @newowner = ] 'owner_ '` Es el nombre de la cuenta de seguridad que será el nuevo propietario del objeto. *Owner* es de **tipo sysname** y no tiene ningún valor predeterminado. *Owner* debe ser un usuario de base de datos, un rol de servidor, un [!INCLUDE[msCoName](../../includes/msconame-md.md)] Inicio de sesión de Windows o un grupo de Windows válido con acceso a la base de datos actual. Si el nuevo propietario es un usuario o grupo de Windows que no tiene una entidad de seguridad de base de datos correspondiente, se creará un usuario de base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_changeobjectowner** quita todos los permisos existentes del objeto. Tendrá que volver a aplicar los permisos que desea conservar después de ejecutar **sp_changeobjectowner**. Por lo tanto, se recomienda crear un script para los permisos existentes antes de ejecutar **sp_changeobjectowner**. Una vez modificado el propietario del objeto, se puede utilizar el script para volver a aplicar los permisos. Deberá modificar el propietario del objeto en el script de permisos antes de la ejecución.  
  
 Para cambiar el propietario de un elemento protegible, utilice ALTER AUTHORIZATION. Para cambiar un esquema, utilice ALTER SCHEMA.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de base de datos **db_owner** , o la pertenencia al rol fijo de base de datos **db_ddladmin** y el rol fijo de base de datos **db_securityadmin** , y también el permiso control en el objeto.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se cambia el propietario de la `authors` tabla a `Corporate\GeorgeW` .  
  
```  
EXEC sp_changeobjectowner 'authors', 'Corporate\GeorgeW';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER SCHEMA &#40;Transact-SQL&#41;](../../t-sql/statements/alter-schema-transact-sql.md)   
 [ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sp_changedbowner &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changedbowner-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
