---
description: DENY (permisos de servidor de Transact-SQL)
title: DENY (permisos de servidor de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], servers
- denying permissions [SQL Server], servers
- servers [SQL Server], permissions
- DENY statement, servers
ms.assetid: 68d6b2a9-c36f-465a-9cd2-01d43a667e99
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 906e0a6d860b288a2f3eeda199564e54f61650e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177702"
---
# <a name="deny-server-permissions-transact-sql"></a>DENY (permisos de servidor de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Deniega permisos en un servidor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS <grantor_principal> ]   
  
<grantee_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
  
<grantor_principal> ::= SQL_Server_login   
    | SQL_Server_login_mapped_to_Windows_login  
    | SQL_Server_login_mapped_to_Windows_group  
    | SQL_Server_login_mapped_to_certificate  
    | SQL_Server_login_mapped_to_asymmetric_key  
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede denegar en un servidor. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 CASCADE  
 Indica que el permiso se deniega para la entidad de seguridad especificada y para el resto de entidades de seguridad a las que ésta concedió el permiso. Es obligatorio cuando la entidad de seguridad tiene el permiso con GRANT OPTION. 
  
 TO \<server_principal>  
 Especifica la entidad de seguridad de la que se va a denegar el permiso.  
  
 AS \<grantor_principal>.  
 Especifica la entidad de seguridad de la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso.
Use la cláusula AS de la entidad de seguridad para indicar que la entidad de seguridad registrada como el denegador del permiso debe ser una entidad de seguridad distinta de la persona que ejecuta la instrucción. Por ejemplo, suponga que la usuaria María tiene el principal_id 12 y el usuario Raúl tiene el principal_id 15. María ejecuta `DENY SELECT ON OBJECT::X TO Steven WITH GRANT OPTION AS Raul;`. Ahora bien, la tabla sys.database_permissions indicará que grantor_principal_id de la instrucción DENY fue 15 (Raul), aunque la instrucción realmente la ejecutó el usuario 13 (María).
  
El uso de AS en esta instrucción no implica la capacidad de suplantar a otro usuario.    
  
 *SQL_Server_login*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_mapped_to_Windows_login*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a un inicio de sesión de Windows.  
  
 *SQL_Server_login_mapped_to_Windows_group*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a un grupo de Windows.  
  
 *SQL_Server_login_mapped_to_certificate*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a un certificado.  
  
 *SQL_Server_login_mapped_to_asymmetric_key*  
 Especifica un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a una clave asimétrica.  
  
 *server_role*  
 Especifica un rol de servidor.  
  
## <a name="remarks"></a>Observaciones  
 Los permisos del ámbito del servidor solamente pueden denegarse si la base de datos actual es maestra.  
  
 Puede ver información acerca de los permisos del servidor en la vista de catálogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), mientras que la información acerca de las entidades de seguridad de servidor puede verse en la vista de catálogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Encontrará información sobre la pertenencia de roles de servidor en la vista de catálogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un servidor ocupa el nivel más alto en la jerarquía de permisos. En la siguiente tabla se muestran los permisos más específicos y limitados que pueden denegarse en un servidor.  
  
|Permiso de servidor|Implícito en el permiso de servidor|  
|-----------------------|----------------------------------|  
|ADMINISTER BULK OPERATIONS|CONTROL SERVER|  
|ALTER ANY AVAILABILITY GROUP<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER ANY CONNECTION|CONTROL SERVER|  
|ALTER ANY CREDENTIAL|CONTROL SERVER|  
|ALTER ANY DATABASE|CONTROL SERVER|  
|ALTER ANY ENDPOINT|CONTROL SERVER|  
|ALTER ANY EVENT NOTIFICATION|CONTROL SERVER|  
|ALTER ANY EVENT SESSION|CONTROL SERVER|  
|ALTER ANY LINKED SERVER|CONTROL SERVER|  
|ALTER ANY LOGIN|CONTROL SERVER|  
|ALTER ANY SERVER AUDIT|CONTROL SERVER|  
|ALTER ANY SERVER ROLE<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|ALTER RESOURCES|CONTROL SERVER|  
|ALTER SERVER STATE|CONTROL SERVER|  
|ALTER SETTINGS|CONTROL SERVER|  
|ALTER TRACE|CONTROL SERVER|  
|AUTHENTICATE SERVER|CONTROL SERVER|  
|CONNECT ANY DATABASE<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|CONNECT SQL|CONTROL SERVER|  
|CONTROL SERVER|CONTROL SERVER|  
|CREATE ANY DATABASE|ALTER ANY DATABASE|  
|Crear grupo de disponibilidad<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY AVAILABILITY GROUP|  
|CREATE DDL EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|CREATE ENDPOINT|ALTER ANY ENDPOINT|  
|CREATE SERVER ROLE<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|ALTER ANY SERVER ROLE|  
|CREATE TRACE EVENT NOTIFICATION|ALTER ANY EVENT NOTIFICATION|  
|EXTERNAL ACCESS ASSEMBLY|CONTROL SERVER|  
|IMPERSONATE ANY LOGIN<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SELECT ALL USER SECURABLES<br /><br /> **Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] a través de la [versión actual](/troubleshoot/sql/general/determine-version-edition-update-level)).|CONTROL SERVER|  
|SHUTDOWN|CONTROL SERVER|  
|UNSAFE ASSEMBLY|CONTROL SERVER|  
|VIEW ANY DATABASE|VIEW ANY DEFINITION|  
|VIEW ANY DEFINITION|CONTROL SERVER|  
|VIEW SERVER STATE|ALTER SERVER STATE|  
  
 Los tres permisos de servidor siguientes se agregaron en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].  
  
 Permiso **CONNECT ANY DATABASE**  
 Conceda **CONNECT ANY DATABASE** a un inicio de sesión que debe conectarse a todas las bases de datos que existen actualmente y a todas las bases de datos que puedan crearse en futuro. No concede ningún permiso en ninguna base de datos más allá de conexión. Combine con **SELECT ALL USER SECURABLES** o con **VIEW SERVER STATE** para permitir que un proceso de auditoría vea todos los datos o todos los estados de base de datos en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Permiso **IMPERSONATE ANY LOGIN**  
 Cuando se concede, permite que un proceso de nivel intermedio suplante la cuenta de los clientes que se conecten a él, a medida que se conecta a las bases de datos. Cuando se deniega, se puede impedir que un inicio de sesión con un alto nivel de privilegios suplante a otros inicios de sesión. Por ejemplo, es posible bloquear un inicio de sesión con el permiso **CONTROL SERVER** para impedir que suplante a otros inicios de sesión.  
  
 Permiso **SELECT ALL USER SECURABLES**  
 Cuando se concede, un inicio de sesión como un auditor puede ver los datos de todas las bases de datos a las que el usuario puede conectarse. Cuando se deniega, impide el acceso a objetos a menos que estén en el esquema **sys**.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER o la propiedad del elemento protegible. Si utiliza la cláusula AS, la entidad de seguridad especificada debe ser propietaria del elemento protegible para el que se deniegan los permisos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-denying-connect-sql-permission-to-a-sql-server-login-and-principals-to-which-the-login-has-regranted-it"></a>A. Denegar el permiso CONNECT SQL para un inicio de sesión de SQL Server y las entidades de seguridad para las que el inicio de sesión ha vuelto a concederlo  
 En el siguiente ejemplo se deniega el permiso `CONNECT SQL` al inicio de sesión `Annika` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y a las entidades de seguridad a las que ha concedido el permiso.  
  
```sql  
USE master;  
DENY CONNECT SQL TO Annika CASCADE;  
GO  
```  
  
### <a name="b-denying-create-endpoint-permission-to-a-sql-server-login-using-the-as-option"></a>B. Denegar el permiso CREATE ENDPOINT a un inicio de sesión de SQL Server utilizando la opción AS  
 En el siguiente ejemplo se deniega el permiso `CREATE ENDPOINT` al usuario `ArifS`. En el ejemplo se utiliza la opción `AS` para especificar `MandarP` como la entidad de seguridad desde la que la entidad de seguridad que ejecuta la consulta deriva la autoridad para hacerlo.  
  
```sql  
USE master;  
DENY CREATE ENDPOINT TO ArifS AS MandarP;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY (permisos de servidor de Transact-SQL)](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE Server Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)  [REVOKE (permisos de servidor de Transact-SQL)]  
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
