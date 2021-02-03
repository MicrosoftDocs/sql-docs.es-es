---
description: GRANT (permisos de entidad de seguridad de servidor de Transact-SQL)
title: GRANT (permisos de entidad de seguridad de servidor de Transact-SQL) | Microsoft Docs
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
- impersonate [SQL Server], granting
- granting permissions [SQL Server], logins
- permissions [SQL Server], impersonate
- permissions [SQL Server], logins
- GRANT statement, impersonation
- GRANT statement, logins
- logins [SQL Server], granting access
- granting permissions [SQL Server], impersonation
ms.assetid: 4cbed281-5e1e-4d8b-b410-4c18a6cd0205
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 708c37c1575869b14460daabaff1ed05e446465a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99161180"
---
# <a name="grant-server-principal-permissions-transact-sql"></a>GRANT (permisos de entidad de seguridad de servidor de Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Concede permisos para un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
GRANT permission [ ,...n ] }   
    ON   
    { [ LOGIN :: SQL_Server_login ]  
      | [ SERVER ROLE :: server_role ] }   
    TO <server_principal> [ ,...n ]  
    [ WITH GRANT OPTION ]  
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
 Especifica un permiso que puede concederse para un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de permisos, vea la sección Comentarios que se muestra posteriormente en este tema.  
  
 LOGIN **::** *SQL_Server_login*  
 Especifica el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el que se va a conceder el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 SERVER ROLE **::** *server_role*  
 Especifica el rol del servidor definido por el usuario en el que se concede el permiso. El calificador de ámbito ( **::** ) es obligatorio.  
  
 TO \<server_principal> Especifica el rol de servidor o el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el que se va a conceder el permiso.  
  
 *SQL_Server_login*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 *SQL_Server_login_from_Windows_login*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde un inicio de sesión de Windows.  
  
 *SQL_Server_login_from_certificate*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a un certificado.  
  
 *SQL_Server_login_from_AsymKey*  
 Especifica el nombre de un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] asignado a una clave asimétrica.  
  
 *server_role*  
 Especifica el nombre de un rol de servidor definido por el usuario.  
  
 WITH GRANT OPTION  
 Indica que la entidad de seguridad también podrá conceder el permiso especificado a otras entidades de seguridad.  
  
 AS *SQL_Server_login*  
 Especifica el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] del que la entidad de seguridad que ejecuta esta consulta deriva su derecho de conceder el permiso.  
  
## <a name="remarks"></a>Observaciones  
 Los permisos del ámbito del servidor solamente pueden concederse si la base de datos actual es maestra.  
  
 Encontrará información sobre los permisos de servidor en la vista de catálogo [sys.server_permissions](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md). Encontrará información sobre las entidades de seguridad de servidor en la vista de catálogo [sys.server_principals](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md).  
  
 Los roles de servidor y los inicios de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] son protegibles en el nivel de servidor. La mayoría de permisos limitados y específicos que se pueden conceder para un rol de servidor o inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se muestran en la siguiente tabla, junto con permisos más generales que los incluyen por implicación.  
  
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
  
### <a name="a-granting-impersonate-permission-on-a-login"></a>A. Conceder el permiso IMPERSONATE para un inicio de sesión  
 En el siguiente ejemplo se concede el permiso `IMPERSONATE` para el inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]`WanidaBenshoof` a un inicio de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] creado desde el usuario de Windows `AdvWorks\YoonM`.  
  
```sql  
USE master;  
GRANT IMPERSONATE ON LOGIN::WanidaBenshoof to [AdvWorks\YoonM];  
GO  
```  
  
### <a name="b-granting-view-definition-permission-with-grant-option"></a>B. Conceder el permiso VIEW DEFINITION con GRANT OPTION  
 En el siguiente ejemplo se concede `VIEW DEFINITION` para el inicio de sesión `EricKurjan` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el inicio de sesión de `RMeyyappan` de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] con `GRANT OPTION`.  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON LOGIN::EricKurjan TO RMeyyappan   
    WITH GRANT OPTION;  
GO   
```  
  
### <a name="c-granting-view-definition-permission-on-a-server-role"></a>C. Conceder el permiso VIEW DEFINITION en un rol de servidor  
 En el siguiente ejemplo se concede el permiso `VIEW DEFINITION` en el rol de servidor `Sales` al rol de servidor `Auditors`.  
  
```sql  
USE master;  
GRANT VIEW DEFINITION ON SERVER ROLE::Sales TO Auditors ;  
GO   
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.server_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-principals-transact-sql.md)   
 [sys.server_permissions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-permissions-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [Permisos &#40;motor de base de datos&#41;](../../relational-databases/security/permissions-database-engine.md)   
 [Funciones de seguridad &#40;Transact-SQL&#41;](../../t-sql/functions/security-functions-transact-sql.md)   
 [Procedimientos almacenados de seguridad &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/security-stored-procedures-transact-sql.md)  
  
  

