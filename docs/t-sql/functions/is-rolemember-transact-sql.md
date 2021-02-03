---
description: IS_ROLEMEMBER (Transact-SQL)
title: IS_ROLEMEMBER (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_ROLEMEMBER
- IS_ROLEMEMBER_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_ROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 73efa688-ae91-4014-98bc-1cabe47321f7
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: fe767dab30645db9f34090e6182f60329f049b74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207994"
---
# <a name="is_rolemember-transact-sql"></a>IS_ROLEMEMBER (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Indica si una entidad de seguridad de base de datos especificada es miembro del rol de base de datos especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IS_ROLEMEMBER ( 'role' [ , 'database_principal' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *role* **'**  
 Nombre del rol de base de datos que se va comprobar. *role* es **sysname**.  
  
 **'** *database_principal* **'**  
 Nombre del usuario de la base de datos, rol de base de datos o rol de aplicación que se va a comprobar. *database_principal* es **sysname**, con un valor predeterminado NULL. Si no se especifica ningún valor, el resultado se basa en el contexto de ejecución actual. Si el parámetro contiene la palabra NULL, se devolverá NULL.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|0|*database_principal* no es miembro de *role*.|  
|1|*database_principal* es miembro de *role*.|  
|NULL|*database_principal* o *role* no es válido o no tiene permiso para ver la pertenencia a roles.|  
  
## <a name="remarks"></a>Comentarios  
 Utilice IS_ROLEMEMBER para determinar si el usuario actual puede realizar una acción que necesite los permisos del rol de base de datos.  
  
 Si *database_principal* está basado en un inicio de sesión de Windows, como Contoso\Mary5, IS_ROLEMEMBER devuelve NULL, a menos que se haya concedido o denegado a *database_principal* el acceso directo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si el parámetro *database_principal* opcional no se proporciona y si *database_principal* se basa en un inicio de sesión de dominio de Windows, puede ser miembro de un rol de base de datos mediante la pertenencia a un grupo de Windows. Para resolver estas pertenencias indirectas, IS_ROLEMEMBER solicita al controlador de dominio información sobre la pertenencia a grupos de Windows. Si no se puede tener acceso al controlador de dominio o no responde, IS_ROLEMEMBER devuelve información sobre la pertenencia a roles teniendo en cuenta únicamente al usuario y sus grupos locales. Si el usuario especificado no es el usuario actual, el valor devuelto por IS_ROLEMEMBER podría diferir de la última actualización de datos del autenticador (por ejemplo, Active Directory) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si se proporciona el parámetro *database_principal* opcional, la entidad de seguridad de base de datos que se está consultando debe estar presente en ys.database_principals o IS_ROLEMEMBER devolverá NULL. Esto indica que *database_principal* no es válido en esta base de datos.  
  
 Cuando el parámetro *database_principal* se basa en un inicio de sesión del dominio o en un grupo de Windows y no se puede acceder al controlador de dominio, se produce un error en las llamadas a IS_ROLEMEMBER y podrían devolverse datos incorrectos o incompletos.  
  
 Si el controlador de dominio no está disponible, la llamada a IS_ROLEMEMBER devolverá información precisa cuando se pueda autenticar localmente la entidad de seguridad de Windows, como una cuenta de Windows local o un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_ROLEMEMBER** siempre devuelve 0 cuando se usa un grupo de Windows como el argumento de entidad de seguridad de Windows y este grupo de Windows es un miembro de otro grupo de Windows que, a su vez, es miembro del rol de base de datos especificado.  
  
 El Control de cuentas de usuario (UAC) de [!INCLUDE[wiprlhext](../../includes/wiprlhext-md.md)] y Windows Server 2008 también podrían devolver resultados diferentes. Esto dependería de si el usuario tuvo acceso al servidor como un miembro del grupo de Windows o como un usuario específico de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Esta función evalúa la pertenencia al rol, no el permiso subyacente. Por ejemplo, el rol fijo de base de datos **db_owner** tiene el permiso **CONTROL DATABASE**. Si el usuario tiene el permiso **CONTROL DATABASE** pero no es miembro del rol, esta función informará correctamente de que el usuario no es miembro del rol **db_owner**, aunque tenga los mismos permisos.  
  
## <a name="related-functions"></a>Funciones relacionadas  
 Para determinar si el usuario actual es miembro del grupo de Windows o del rol de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificados, use [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Para determinar si un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es miembro de un rol de servidor, use [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Se necesita el permiso VIEW DEFINITION en el rol de base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se indica si el usuario actual es miembro del rol fijo de base de datos `db_datareader`.  
  
```sql  
IF IS_ROLEMEMBER ('db_datareader') = 1  
   print 'Current user is a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') = 0  
   print 'Current user is NOT a member of the db_datareader role'  
ELSE IF IS_ROLEMEMBER ('db_datareader') IS NULL  
   print 'ERROR: The database role specified is not valid.';  
```  
  
## <a name="see-also"></a>Vea también  
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [CREATE SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-role-transact-sql.md)   
 [ALTER SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-role-transact-sql.md)   
 [DROP SERVER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-role-transact-sql.md)   
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-srvrolemember-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
