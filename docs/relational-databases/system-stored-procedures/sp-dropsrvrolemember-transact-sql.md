---
description: sp_dropsrvrolemember (Transact-SQL)
title: sp_dropsrvrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/20/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_dropsrvrolemember
- sp_dropsrvrolemember_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_dropsrvrolemember
ms.assetid: 7be99181-d221-49d0-9cb2-c930d8c044a0
ms.author: vanto
author: VanMSFT
ms.openlocfilehash: d042fa55cf15dac997d7e52f9bd39eb7309fd1b5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158097"
---
# <a name="sp_dropsrvrolemember-transact-sql"></a>sp_dropsrvrolemember (Transact-SQL)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Quita un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o un usuario o grupo de Windows de un rol fijo de servidor.

> [!IMPORTANT]
> [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use [ALTER Server role](../../t-sql/statements/alter-server-role-transact-sql.md) .

![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)

## <a name="syntax"></a>Sintaxis

```
sp_dropsrvrolemember [ @loginame = ] 'login' , [ @rolename = ] 'role'  
```

## <a name="arguments"></a>Argumentos

**[ @loginame =]** '_Inicio de sesión_'  
Es el nombre del inicio de sesión que se va a quitar del rol fijo de servidor. *login* es de **tipo sysname** y no tiene ningún valor predeterminado. debe existir un *Inicio de sesión* .  

**[ @rolename =]** '_rol_'  
Es el nombre de un rol de servidor. *role* es de **tipo sysname y su** valor predeterminado es NULL. *role* debe ser uno de los siguientes valores:  

-   sysadmin  
  
-   securityadmin  
  
-   serveradmin  
  
-   setupadmin  
  
-   processadmin  
  
-   diskadmin  
  
-   dbcreator  
  
-   bulkadmin 
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Solo se puede utilizar sp_dropsrvrolemember para quitar un inicio de sesión de un rol fijo de servidor. Para quitar un miembro de un rol de base de datos, utilice sp_droprolemember .  
  
 El inicio de sesión sa no puede quitarse de ningún rol fijo de servidor.  
  
 sp_dropsrvrolemember no puede ejecutarse en una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol fijo de servidor sysadmin, o tener el permiso ALTER ANY LOGIN para el servidor y ser miembro del rol del que se va a quitar el miembro.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se quita el inicio de sesión `JackO` del rol fijo de servidor `sysadmin`.  
  
```sql
EXEC sp_dropsrvrolemember 'JackO', 'sysadmin';  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
