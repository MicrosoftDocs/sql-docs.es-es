---
description: DENY (permisos de entidad de seguridad de servidor de Transact-SQL)
title: DENY (permisos de entidad de seguridad de servidor de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/09/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- DENY statement, impersonate
- permissions [SQL Server], impersonate
- impersonate [SQL Server], denying
- DENY statement, logins
- permissions [SQL Server], logins
- denying permissions [SQL Server], logins
- servers [SQL Server], permissions
- logins [SQL Server], denying access
ms.assetid: 859affa7-0567-47d1-9490-57c1abbd619b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8e3ea7c43e664883324f87f031ba47f985ef78bd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177691"
---
# <a name="deny-server-principal-permissions-transact-sql"></a>DENY (permisos de entidad de seguridad de servidor de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Deniega los permisos concedidos para un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DENY permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    TO <server_principal> [ ,...n ]  
    [ CASCADE ]  
    [ AS SQL_Server_login ]   
  
<server_principal> ::=   
    SQL_Server_login  
    | SQL_Server_login_from_Windows_login   
    | SQL_Server_login_from_certificate   
    | SQL_Server_login_from_AsymKey   
    | server_role  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *permission*  
 Especifica un permiso que se puede denegar en un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 LOGIN **::** *SQL_Server_login*  
 Especifica el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que se deniega el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 SERVER ROLE **::** *server_role*  
 Especifica el rol de servidor para el que se deniega el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 TO \<server_principal>  
 Especifica el rol de servidor o el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que se va a conceder el permiso.  
  
 TO *SQL_Server_login*  
 Especifica el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que se va a denegar el permiso.  
  
 *SQL_Server_login*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde un inicio de sesión de Windows.  
  
 *SQL_Server_login_from_certificate*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a un certificado.  
  
 *SQL_Server_login_from_AsymKey*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a una clave asimétrica.  
  
 *server_role*  
 Especifica el nombre de un rol de servidor.  
  
 CASCADE  
 Indica que el permiso que se va a denegar también se denegará a otras entidades de seguridad a las que esta entidad de seguridad ha concedido permisos.  
  
 AS *SQL_Server_login*  
 Especifica el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del que la entidad de seguridad que ejecuta esta consulta deriva su derecho de denegar el permiso.  
  
## <a name="remarks"></a>Observaciones  
 Los permisos del ámbito del servidor solamente pueden denegarse si la base de datos actual es maestra.  
  
 Puede ver la información acerca de los permisos de servidor en la vista de catálogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md). Puede ver la información acerca de las entidades de seguridad de servidor en la vista de catálogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Se produce un error en la instrucción DENY si no se especifica CASCADE al denegar un permiso a una entidad de seguridad a la que se concedió dicho permiso con GRANT OPTION.  
  
 Los roles de servidor y los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son protegibles en el nivel de servidor. La mayoría de permisos limitados y específicos que se pueden denegar para un rol de servidor o un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se muestran en la siguiente tabla, junto con los permisos más generales que los incluyen por implicación.  
  
|Permiso de rol de servidor o de inicio de sesión de SQL Server|Permiso de rol de servidor o inicio de sesión implícito de SQL Server|Implícito en el permiso de servidor|  
|------------------------------------------------|-----------------------------------------------------------|----------------------------------|  
|CONTROL|CONTROL|CONTROL SERVER|  
|IMPERSONATE|CONTROL|CONTROL SERVER|  
|VIEW DEFINITION|CONTROL|VIEW ANY DEFINITION|  
|ALTER|CONTROL|ALTER ANY LOGIN<br /><br /> ALTER ANY SERVER ROLE|  
  
## <a name="permissions"></a>Permisos  
 Para los inicios de sesión, se necesita el permiso CONTROL en el inicio de sesión o el permiso ALTER ANY LOGIN en el servidor.  
  
 Para los roles de servidor, se necesita el permiso CONTROL en el rol de servidor o el permiso ALTER ANY SERVER ROLE en el servidor.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-denying-impersonate-permission-on-a-login"></a>A. Denegar el permiso IMPERSONATE en un inicio de sesión  
 En el siguiente ejemplo se deniega el permiso `IMPERSONATE` para el inicio de sesión `WanidaBenshoof` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde el usuario de Windows `AdvWorks\YoonM`.  
  
```  
USE master;  
DENY IMPERSONATE ON LOGIN::WanidaBenshoof TO [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-denying-view-definition-permission-with-cascade"></a>B. Denegar el permiso VIEW DEFINITION con CASCADE  
 En el siguiente ejemplo se deniega el permiso `VIEW DEFINITION` para el inicio de sesión `EricKurjan` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al inicio de sesión `RMeyyappan` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. La opción `CASCADE` indica que el permiso `VIEW DEFINITION` para `EricKurjan` también se denegará a las entidades de seguridad a las que `RMeyyappan` concedió este permiso.  
  
```sql  
USE master;  
DENY VIEW DEFINITION ON LOGIN::EricKurjan TO RMeyyappan   
    CASCADE;  
GO   
```  
  
### <a name="c-denying-view-definition-permission-on-a-server-role"></a>C. Denegar el permiso VIEW DEFINITION en un rol de servidor  
 En el siguiente ejemplo se deniega el permiso `VIEW DEFINITION` en el rol de servidor `Sales` al rol de servidor `Auditors`.  
  
```sql 
USE master;  
DENY VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [GRANT &#40;permisos de entidad de seguridad de servidor de Transact-SQL&#41;](../../t-sql/statements/grant-server-principal-permissions-transact-sql.md)   
 [REVOKE &#40;permisos de entidad de seguridad de servidor de Transact-SQL&#41;](../../t-sql/statements/revoke-server-principal-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  
