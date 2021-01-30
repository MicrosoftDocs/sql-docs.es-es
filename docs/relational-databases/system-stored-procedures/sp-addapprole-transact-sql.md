---
description: sp_addapprole (Transact-SQL)
title: sp_addapprole (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addapprole_TSQL
- sp_addapprole
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addapprole
ms.assetid: 24200295-9a54-4cab-9922-fb2e88632721
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 5a48cfb80bab8e9a754cbcb8186de8cb2955f096
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182858"
---
# <a name="sp_addapprole-transact-sql"></a>sp_addapprole (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Agrega un rol de aplicación a la base de datos actual.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use [crear rol de aplicación](../../t-sql/statements/create-application-role-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_addapprole [ @rolename = ] 'role' , [ @password = ] 'password'  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @rolename = ] 'role'` Es el nombre del nuevo rol de aplicación. *role* es de **tipo sysname** y no tiene ningún valor predeterminado. *role* debe ser un identificador válido y no puede existir en la base de datos actual.  
  
 Los nombres de roles de aplicación pueden contener entre 1 y 128 caracteres, y pueden incluir letras, símbolos y números. Los nombres de rol no pueden contener una barra diagonal inversa ( \\ ) ni ser null ni una cadena vacía (' ').  
  
`[ @password = ] 'password'` Es la contraseña requerida para activar el rol de aplicación. *password* es de **tipo sysname** y no tiene ningún valor predeterminado. la *contraseña* no puede ser null.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], los usuarios (y roles) no se distinguían completamente de los esquemas. A partir de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], los esquemas son totalmente distintos de los roles. Esta nueva arquitectura se refleja en el comportamiento de CREATE APPLICATION ROLE. Esta instrucción reemplaza **sp_addapprole**.  
  
 Para mantener la compatibilidad con versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , **sp_addapprole** hará lo siguiente:  
  
-   Si no existe un esquema con el mismo nombre que el rol de aplicación, se creará dicho esquema. El nuevo esquema será propiedad del rol de aplicación y será el esquema predeterminado del rol de aplicación.  
  
-   Si ya existe un esquema con el mismo nombre que el rol de aplicación, el procedimiento generará un error.  
  
-   **Sp_addapprole** no comprueba la complejidad de la contraseña. La complejidad de la contraseña la comprueba CREATE APPLICATION ROLE.  
  
 La *contraseña* del parámetro se almacena como un hash unidireccional.  
  
 La **sp_addapprole** procedimiento almacenado no se puede ejecutar desde una transacción definida por el usuario.  
  
> [!IMPORTANT]  
>  **SqlClient** no admite la opción de **cifrado** ODBC de Microsoft. Cuando pueda, pida a los usuarios que escriban las credenciales del rol de aplicación en tiempo de ejecución. No guarde las credenciales en un archivo. Si debe conservar las credenciales, cífrelas con las funciones CryptoAPI.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso ALTER ANY APPLICATION ROLE en la base de datos. Si aún no existe un esquema con el mismo nombre de esquema y propietario que el nuevo rol, también se requiere el permiso CREATE SCHEMA en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se agrega el nuevo rol `SalesApp` de aplicación con la contraseña `x97898jLJfcooFUYLKm387gf3` a la base de datos actual.  
  
```  
EXEC sp_addapprole 'SalesApp', 'x97898jLJfcooFUYLKm387gf3' ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE APPLICATION ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-application-role-transact-sql.md)  
  
  
