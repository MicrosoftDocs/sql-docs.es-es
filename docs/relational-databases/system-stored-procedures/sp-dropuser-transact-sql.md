---
description: sp_dropuser (Transact-SQL)
title: sp_dropuser (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropuser
- sp_dropuser_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropuser
ms.assetid: e28f18f9-7ecf-4568-89f4-fe5c520df386
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ba5b9eb814f48003d93bf6d7b05c2b1c37c1ea76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99157148"
---
# <a name="sp_dropuser-transact-sql"></a>sp_dropuser (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita un usuario de base de datos de la base de datos actual. **sp_dropuser** proporciona compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [Drop User](../../t-sql/statements/drop-user-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_dropuser [ @name_in_db = ] 'user'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @name_in_db = ] 'user'` Es el nombre del usuario que se va a quitar. el *usuario* es un **sysname** y no tiene ningún valor predeterminado. el *usuario* debe existir en la base de datos actual. Cuando especifique un inicio de sesión de Windows, utilice el nombre por el que se conoce ese inicio de sesión en la base de datos.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_dropuser** ejecuta **sp_revokedbaccess** para quitar al usuario de la base de datos actual.  
  
 Utilice **sp_helpuser** para mostrar una lista de los nombres de usuario que se pueden quitar de la base de datos actual.  
  
 Cuando se quita un usuario de base de datos, también se quitan los alias que el usuario tuviera. Si el usuario posee un esquema vacío con su mismo nombre, se eliminará también el esquema. Si el usuario posee otros elementos protegibles en la base de datos, no se eliminará el usuario. La propiedad de los objetos debe transferirse primero a otra entidad de seguridad. Para obtener más información, vea [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md). Al quitar un usuario de la base de datos se quitan automáticamente los permisos que tiene asociados y, también, se quita el usuario de todos los roles de los que es miembro.  
  
 **sp_dropuser** no se puede usar para quitar el propietario de la base de datos (**DBO**) **INFORMATION_SCHEMA** usuarios o el usuario **invitado** de las bases de datos **maestra** o **tempdb** . En las bases de datos que no son del sistema, `EXEC sp_dropuser 'guest'` revocará el permiso Connect del **invitado** del usuario. Pero no se quitará al usuario.  
  
 **sp_dropuser** no se puede ejecutar en una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY USER en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En este ejemplo se quita el usuario `Albert` de la base de datos actual.  
  
```  
EXEC sp_dropuser 'Albert';  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [DROP USER &#40;Transact-SQL&#41;](../../t-sql/statements/drop-user-transact-sql.md)   
 [sp_revokedbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-revokedbaccess-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
