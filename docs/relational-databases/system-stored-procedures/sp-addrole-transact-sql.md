---
description: sp_addrole (Transact-SQL)
title: sp_addrole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addrole
- sp_addrole_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrole
ms.assetid: e8a21642-8440-419a-8585-93d3d9d44f00
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c6217f78d47e193c8583caca0925f2d4b587c439
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200165"
---
# <a name="sp_addrole-transact-sql"></a>sp_addrole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un rol de base de datos nuevo en la base de datos actual.  
  
> [!IMPORTANT]
>  **sp_addrole** se incluye por compatibilidad con versiones anteriores de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y es posible que no se admita en una versión futura. En su lugar, use [crear rol](../../t-sql/statements/create-role-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addrole [ @rolename = ] 'role' [ , [ @ownername = ] 'owner' ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] 'role'` Es el nombre del nuevo rol de base de datos. *role* es un **sysname** y no tiene ningún valor predeterminado. *role* debe ser un identificador (ID.) válido y no debe existir en la base de datos actual.  
  
`[ @ownername = ] 'owner'` Es el propietario del nuevo rol de base de datos. *Owner* es un **tipo sysname** y su valor predeterminado es el usuario que se está ejecutando actualmente. *Owner* debe ser un usuario de base de datos o un rol de base de datos en la base de datos actual.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Los nombres de los roles de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pueden contener de 1 a 128 caracteres, incluidos letras, símbolos y números. Los nombres de los roles de base de datos no pueden: contener un carácter de barra diagonal inversa ( \\ ), ser null o una cadena vacía (**' '**).  
  
 Después de agregar un rol de base de datos, use [sp_addrolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md) para agregar entidades de seguridad al rol. Cuando se utilizan las instrucciones GRANT, DENY o REVOKE para aplicar permisos al rol de base de datos, los miembros de este rol heredan estos permisos como si se aplicaran directamente a sus cuentas.  
  
> [!NOTE]  
>  No se pueden crear roles de servidor nuevos. Los roles solo pueden crearse en las bases de datos.  
  
 no se puede usar **sp_addrole** dentro de una transacción definida por el usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CREATE ROLE en la base de datos. Si crea un esquema, es necesario el permiso CREATE SCHEMA en la base de datos. Si se especifica *Owner* como usuario o grupo, se requiere Impersonate en ese usuario o grupo. Si se especifica *Owner* como rol, se requiere el permiso ALTER en ese rol o en un miembro de ese rol. Si se especifica owner como rol de aplicación, es necesario el permiso ALTER en ese rol de aplicación.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega un nuevo rol llamado `Managers` a la base de datos actual.  
  
```  
EXEC sp_addrole 'Managers';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)  
  
  
