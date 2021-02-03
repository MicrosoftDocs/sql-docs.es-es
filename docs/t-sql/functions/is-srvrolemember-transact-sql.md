---
description: IS_SRVROLEMEMBER (Transact-SQL)
title: IS_SRVROLEMEMBER (Transact-SQL)
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- IS_SRVROLEMEMBER_TSQL
- IS_SRVROLEMEMBER
dev_langs:
- TSQL
helpviewer_keywords:
- roles [SQL Server], members
- IS_SRVROLEMEMBER function
- members [SQL Server], verifying
ms.assetid: 3241a44a-6958-415b-b8b7-2a1207c36ab3
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 10d6a11fccad8f4905015024f6d90a3aadb2dac4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165092"
---
# <a name="is_srvrolemember-transact-sql"></a>IS_SRVROLEMEMBER (Transact-SQL)

[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Indica si en el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es miembro del rol de servidor especificado.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
IS_SRVROLEMEMBER ( 'role' [ , 'login' ] )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **'** *role* **'**  
 Es el nombre del rol de servidor que se va comprobar. *role* es **sysname**.  
  
 Los valores válidos para *role* son los roles de servidor definidos por el usuario y los siguientes roles fijos de servidor:  

- sysadmin
- serveradmin
- dbcreator
- setupadmin  
- bulkadmin
- securityadmin  
- diskadmin
- público  
- processadmin
  
 **'** *login* **'**  
 Es el nombre del inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se va a comprobar. *login* es de tipo **sysname** y su valor predeterminado es NULL. Si no se especifica ningún valor, el resultado se basa en el contexto de ejecución actual. Si el parámetro contiene la palabra NULL, se devolverá NULL.  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 **int**  
  
|Valor devuelto|Descripción|  
|------------------|-----------------|  
|0|*login* no es miembro del grupo *role*.<br /><br /> En [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)], esta instrucción siempre devuelve 0.|  
|1|*login* es miembro del grupo *role*.|  
|NULL|*role* o *login* no son válidos o no tienen permiso para ver la pertenencia a roles.|  
  
## <a name="remarks"></a>Observaciones  
 Use IS_SRVROLEMEMBER para determinar si el usuario actual puede realizar una acción que necesite los permisos del rol de servidor.  
  
 Si se especifica un inicio de sesión de Windows, como Contoso\María5, para *login*, **IS_SRVROLEMEMBER** devuelve **NULL**, a menos que se haya concedido o denegado el acceso directo a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al inicio de sesión.  
  
 Si no se proporciona el parámetro *login* opcional y *login* es un inicio de sesión de dominio de Windows, puede ser un miembro del rol fijo de servidor mediante la pertenencia a un grupo de Windows. Para resolver estas pertenencias indirectas, IS_SRVROLEMEMBER solicita al controlador de dominio información sobre la pertenencia a grupos de Windows. Si no se puede tener acceso al controlador de dominio o este no responde, **IS_ROLEMEMBER** devuelve información sobre la pertenencia a roles teniendo en cuenta únicamente al usuario y sus grupos locales. Si el usuario especificado no es el usuario actual, el valor devuelto por IS_SRVROLEMEMBER podría diferir de la última actualización de datos del autenticador (por ejemplo, Active Directory) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Si se proporciona el parámetro de inicio de sesión opcional, el inicio de sesión de Windows que se consulta se debe encontrar en sys.server_principals o IS_SRVROLEMEMBER devolverá NULL. Esto indica que el inicio de sesión no es válido.  
  
 Cuando el parámetro de inicio de sesión es un inicio de sesión del dominio o está basado en un grupo de Windows y el controlador de dominio no es accesible, se producirá un error en las llamadas a IS_SRVROLEMEMBER y podrían devolverse datos incorrectos o incompletos.  
  
 Si el controlador de dominio no está disponible, la llamada a IS_SRVROLEMEMBER devolverá la información precisa cuando se puede autenticar el principio de Windows localmente, como una cuenta de Windows local o un inicio de sesión [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **IS_SRVROLEMEMBER** devuelve siempre 0 cuando se usa un grupo de Windows como el argumento de inicio de sesión y este grupo de Windows es un miembro de otro grupo de Windows que, a su vez, es miembro del rol de servidor especificado.  
  
 El valor de Control de cuentas de usuario (UAC) también puede provocar que se devuelvan resultados diferentes. Esto dependería de si el usuario tuvo acceso al servidor como un miembro del grupo de Windows o como un usuario específico de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Esta función evalúa la pertenencia al rol, no el permiso subyacente. Por ejemplo, el rol fijo de servidor **sysadmin** tiene el permiso **CONTROL SERVER**. Si el usuario tiene el permiso **CONTROL SERVER** pero pertenece al rol, esta función informará correctamente de que el usuario no es miembro del rol **sysadmin**, aunque tenga los mismos permisos.  
  
## <a name="related-functions"></a>Funciones relacionadas  
 Para determinar si el usuario actual es miembro del grupo de Windows o del rol de base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificados, use [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md). Para determinar si un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es miembro de un rol de base de datos, use [IS_ROLEMEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-rolemember-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW DEFINITION en el rol de servidor.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se indica si el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el usuario actual es miembro del rol fijo de servidor `sysadmin`.  
  
```sql  
IF IS_SRVROLEMEMBER ('sysadmin') = 1  
   print 'Current user''s login is a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') = 0  
   print 'Current user''s login is NOT a member of the sysadmin role'  
ELSE IF IS_SRVROLEMEMBER ('sysadmin') IS NULL  
   print 'ERROR: The server role specified is not valid.';  
```  
  
 En este ejemplo se indica si el inicio de sesión de dominio Pat es miembro del rol fijo de servidor **diskadmin**.  
  
```sql  
SELECT IS_SRVROLEMEMBER('diskadmin', 'Contoso\Pat');  
```  
  
## <a name="see-also"></a>Consulte también  
 [IS_MEMBER &#40;Transact-SQL&#41;](../../t-sql/functions/is-member-transact-sql.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)  
  
  
