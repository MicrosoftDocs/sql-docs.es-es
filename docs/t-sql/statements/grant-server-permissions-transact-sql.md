---
description: GRANT (permisos de servidor de Transact-SQL)
title: GRANT (permisos de servidor de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- GRANT statement, servers
- permissions [SQL Server], servers
- servers [SQL Server], permissions
- granting permissions [SQL Server], servers
ms.assetid: 7e880a5a-3bdc-491f-a167-7a9ed338be7f
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 436ede592487a7f70a01a5ef72f5b359b250ac4a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161184"
---
# <a name="grant-server-permissions-transact-sql"></a>GRANT (permisos de servidor de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede permisos en un servidor. 
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
GRANT permission [ ,...n ]   
    TO <grantee_principal> [ ,...n ] [ WITH GRANT OPTION ]  
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
 Especifica un permiso que se puede conceder para un servidor. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 TO \<grantee_principal> Especifica la entidad de seguridad para la que se concede el permiso.  
  
 AS \<grantor_principal> Especifica la entidad de seguridad desde la que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso.  
  
 WITH GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
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
 Especifica un rol de servidor definido por el usuario.  
  
## <a name="remarks"></a>Observaciones  
 Los permisos del ámbito del servidor solamente pueden concederse si la base de datos actual es maestra.  
  
 Puede ver información acerca de los permisos del servidor en la vista de catálogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md), mientras que la información acerca de las entidades de seguridad de servidor puede verse en la vista de catálogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md). Encontrará información sobre la pertenencia de roles de servidor en la vista de catálogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
 Un servidor ocupa el nivel más alto en la jerarquía de permisos. En la siguiente tabla se muestran los permisos más específicos y limitados que pueden concederse para un servidor.  
  
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
 El otorgante de permisos (o la entidad de seguridad especificada con la opción AS) debe tener asignado el mismo permiso con GRANT OPTION o un permiso superior que implique el permiso que se va a conceder. Los miembros del rol fijo de servidor sysadmin pueden conceder cualquier permiso.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-granting-a-permission-to-a-login"></a>A. Conceder un permiso a un inicio de sesión  
 En el siguiente ejemplo se concede el permiso `CONTROL SERVER` al inicio de sesión `TerryEminhizer` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
```sql  
USE master;  
GRANT CONTROL SERVER TO TerryEminhizer;  
GO  
```  
  
### <a name="b-granting-a-permission-that-has-grant-permission"></a>B. Conceder un permiso que dispone del permiso GRANT  
 En el siguiente ejemplo se concede `ALTER ANY EVENT NOTIFICATION` al inicio de sesión `JanethEsteves` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con el derecho para conceder este permiso a otro inicio de sesión.  
  
```sql  
USE master;  
GRANT ALTER ANY EVENT NOTIFICATION TO JanethEsteves WITH GRANT OPTION;  
GO  
```  
  
### <a name="c-granting-a-permission-to-a-server-role"></a>C. Conceder un permiso a un rol de servidor  
 En el ejemplo siguiente se crean dos roles de servidor denominados `ITDevAdmin` e `ITDevelopers`. Concede el permiso `ALTER ANY DATABASE` al rol de servidor definido por el usuario `ITDevAdmin` incluida la opción `WITH GRANT` de manera que el rol de servidor `ITDevAdmin` pueda reasignar el permiso `ALTER ANY DATABASE`. A continuación, el ejemplo concede a `ITDevelopers` el permiso para utilizar el permiso `ALTER ANY DATABASE` del rol de servidor `ITDevAdmin`.  
  
```sql  
USE master;  
CREATE SERVER ROLE ITDevAdmin ;  
CREATE SERVER ROLE ITDevelopers ;  
GRANT ALTER ANY DATABASE TO ITDevAdmin WITH GRANT OPTION ;  
GRANT ALTER ANY DATABASE TO ITDevelopers AS ITDevAdmin ;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-transact-sql.md)   
 [DENY &#40;Transact-SQL&#41;](../../t-sql/statements/deny-transact-sql.md)   
 [DENY &#40;permisos de servidor de Transact-SQL&#41;](../../t-sql/statements/deny-server-permissions-transact-sql.md)   
 [REVOKE Server Permissions &#40;Transact-SQL&#41;](../../t-sql/statements/revoke-server-permissions-transact-sql.md)  [REVOKE (permisos de servidor de Transact-SQL)]  
 [Jerarquía de permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-hierarchy-database-engine.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [sys.fn_builtin_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-builtin-permissions-transact-sql.md)   
 [sys.fn_my_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-my-permissions-transact-sql.md)   
 [HAS_PERMS_BY_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/has-perms-by-name-transact-sql.md)  
  
