---
description: sp_addrolemember (Transact-SQL)
title: sp_addrolemember (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addrolemember_TSQL
- sp_addrolemember
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addrolemember
ms.assetid: a583c087-bdb3-46d2-b9e5-3921b3e6d10b
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9b0049c38a2e94f250dd3e6dc5a350a03ef89c4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210065"
---
# <a name="sp_addrolemember-transact-sql"></a>sp_addrolemember (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Agrega un usuario de base de datos, un rol de base de datos, un inicio de sesión de Windows o un grupo de Windows a un rol de base de datos en la base de datos actual.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, utilice [ALTER role](../../t-sql/statements/alter-role-transact-sql.md) .  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
sp_addrolemember [ @rolename = ] 'role', [ @membername = ] 'security_account'  
```    

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]
  
## <a name="arguments"></a>Argumentos  
 [ @rolename =] '*rol*'  
 Es el nombre del rol en la base de datos actual. *role* es un **sysname** y no tiene ningún valor predeterminado.  
  
 [ @membername =] '*security_account*'  
 Es la cuenta de seguridad que se va a agregar al rol. *security_account* es un **sysname** y no tiene ningún valor predeterminado. *security_account* puede ser un usuario de base de datos, un rol de base de datos, un inicio de sesión de Windows o un grupo de Windows.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="remarks"></a>Observaciones  
 Un miembro agregado a un rol mediante sp_addrolemember hereda los permisos del rol. Si el nuevo miembro es una entidad de seguridad de nivel de Windows sin un usuario de base de datos correspondiente, se creará un usuario de base de datos pero no se asignará completamente al inicio de sesión. Compruebe siempre que el inicio de sesión existe y tiene acceso a la base de datos.  
  
 Un rol no puede incluirse como un miembro. Este tipo de definiciones "circulares" no es válido, incluso cuando la pertenencia solo esté implícita indirectamente en uno o varios miembros intermedios.  
  
 sp_addrolemember no puede Agregar un rol fijo de base de datos, un rol fijo de servidor o DBO a un rol.
  
 Utilice sp_addrolemember únicamente para agregar un miembro a un rol de base de datos. Para agregar un miembro a un rol de servidor, use [sp_addsrvrolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Para agregar miembros a los roles flexibles de la base de datos, se debe cumplir una de las condiciones siguientes:  
  
-   Pertenencia al rol fijo de base de datos db_securityadmin o db_owner.  
  
-   Se debe pertenecer al rol propietario del rol.  
  
-   El permiso **ALTER any role** o el permiso **ALTER** en el rol.  
  
 Para poder agregar miembros a los roles fijos de base de datos es necesario pertenecer al rol fijo de base de datos db_owner.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-adding-a-windows-login"></a>A. Agregar un inicio de sesión de Windows  
 En el ejemplo siguiente se agrega el inicio de sesión de Windows `Contoso\Mary5` a la `AdventureWorks2012` base de datos como usuario `Mary5` . A continuación, se agrega el usuario `Mary5` al rol `Production`.  
  
> [!NOTE]  
>  Dado que `Contoso\Mary5` se conoce como el usuario `Mary5` de la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)], es preciso especificar el nombre de usuario `Mary5`. Se producirá un error en la instrucción a menos que un inicio de sesión `Contoso\Mary5` exista. Pruebe a usar un inicio de sesión de su dominio.  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE USER Mary5 FOR LOGIN [Contoso\Mary5] ;  
GO  
```  
  
### <a name="b-adding-a-database-user"></a>B. Agregar un usuario de base de datos  
 En el siguiente ejemplo se agrega el usuario de base de datos `Mary5` al rol de la base de datos `Production` en la base de datos actual.  
  
```sql  
EXEC sp_addrolemember 'Production', 'Mary5';  
```  
  
## <a name="examples-sspdw"></a>Ejemplos: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-adding-a-windows-login"></a>C. Agregar un inicio de sesión de Windows  
 En el ejemplo siguiente se agrega el inicio de sesión `LoginMary` a la `AdventureWorks2008R2` base de datos como usuario `UserMary` . A continuación, se agrega el usuario `UserMary` al rol `Production`.  
  
> [!NOTE]  
>  Dado que el inicio `LoginMary` de sesión se conoce como el usuario `UserMary` de base de datos en la base de [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] datos, `UserMary` se debe especificar el nombre de usuario. Se producirá un error en la instrucción a menos que un inicio de sesión `Mary5` exista. Los inicios de sesión y los usuarios suelen tener el mismo nombre. En este ejemplo se usan nombres diferentes para diferenciar las acciones que afectan al inicio de sesión y al usuario.  
  
```sql  
-- Uses AdventureWorks  
  
CREATE USER UserMary FOR LOGIN LoginMary ;  
GO  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
### <a name="d-adding-a-database-user"></a>D. Agregar un usuario de base de datos  
 En el siguiente ejemplo se agrega el usuario de base de datos `UserMary` al rol de la base de datos `Production` en la base de datos actual.  
  
```sql  
EXEC sp_addrolemember 'Production', 'UserMary'  
```  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)   
 [sp_addsrvrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addsrvrolemember-transact-sql.md)   
 [sp_droprolemember &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)   
 [sp_grantdbaccess &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-grantdbaccess-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Roles de nivel de base de datos](../../relational-databases/security/authentication-access/database-level-roles.md)  
  
  
