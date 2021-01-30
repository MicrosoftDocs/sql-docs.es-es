---
description: sp_revokelogin (Transact-SQL)
title: sp_revokelogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_revokelogin_TSQL
- sp_revokelogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_revokelogin
ms.assetid: cb1ab102-1ae0-4811-9144-9a8121ef2d7e
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 130ba750ed2e6dfadac111578ee9c42c6578c313
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161339"
---
# <a name="sp_revokelogin-transact-sql"></a>sp_revokelogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Quita las entradas de inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para un usuario o grupo de Windows creado mediante Create login, **sp_grantlogin** o **sp_denylogin**.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [Drop login](../../t-sql/statements/drop-login-transact-sql.md) en su lugar.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_revokelogin [ @loginame= ] 'login'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @loginame = ] 'login'` Es el nombre del usuario o grupo de Windows. *login* es de **tipo sysname** y no tiene ningún valor predeterminado. el *Inicio de sesión* puede ser cualquier grupo o nombre de usuario de Windows existente con el formato nombre de *equipo* \\ *usuario o dominio* \\ *usuario*.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_revokelogin** deshabilita las conexiones con la cuenta especificada por el parámetro *login* . Los usuarios de Windows que tienen acceso a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] por medio de su pertenencia a un grupo de Windows, pueden seguir conectándose como parte del grupo aunque se les deniegue el acceso individual. Del mismo modo, si el parámetro *login* especifica el nombre de un grupo de Windows, los miembros de ese grupo a los que se haya concedido acceso por separado a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seguirán pudiendo conectarse.  
  
 Por ejemplo, si el usuario de Windows **ADVWORKS\john** es miembro del grupo de Windows **ADVWORKS\Admins**, y **sp_revokelogin** revoca el acceso de `ADVWORKS\john` :  
  
```  
sp_revokelogin [ADVWORKS\john]  
```  
  
 El usuario **ADVWORKS\john** todavía puede conectarse si se ha concedido acceso a **ADVWORKS\Admins** a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Del mismo modo, si se ha revocado el acceso al grupo de Windows **ADVWORKS\Admins** , pero se le concede el acceso a **ADVWORKS\john** , **ADVWORKS\john** todavía puede conectarse.  
  
 Utilice **sp_denylogin** para impedir explícitamente que los usuarios se conecten a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , independientemente de su pertenencia a grupos de Windows.  
  
 **sp_revokelogin** no se puede ejecutar en una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY LOGIN en el servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se quitan las entradas de inicio de sesión del usuario de Windows `Corporate\MollyA` .  
  
```  
EXEC sp_revokelogin 'Corporate\MollyA';  
```  
  
 Or  
  
```  
EXEC sp_revokelogin [Corporate\MollyA];  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [DROP LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/drop-login-transact-sql.md)   
 [sp_denylogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-denylogin-transact-sql.md)   
 [sp_droplogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplogin-transact-sql.md)   
 [sp_grantlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantlogin-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
