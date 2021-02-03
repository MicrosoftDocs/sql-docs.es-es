---
description: DROP SERVER ROLE (Transact-SQL)
title: DROP SERVER ROLE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DROP SERVER ROLE
- DROP_SERVER_ROLE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- SERVER ROLE, DROP
- DROP SERVER ROLE statement
ms.assetid: a2a1e6e6-e40c-4d6a-81be-d197b80bf226
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9993ef678f2c8526e0ce9aa9daa1b7ac1d1e82e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160206"
---
# <a name="drop-server-role-transact-sql"></a>DROP SERVER ROLE (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2012-xxxx-xxxx-pdw-md](../../includes/tsql-appliesto-ss2012-xxxx-xxxx-pdw-md.md)]

  Quita un rol de servidor definido por el usuario.  
  
 Los roles de servidor definidos por el usuario son nuevos en [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
DROP SERVER ROLE role_name  
```  
  
## <a name="arguments"></a>Argumentos  
 *role_name*  
 Especifica el rol de servidor definido por el usuario que se va a quitar del servidor.  
  
## <a name="remarks"></a>Observaciones  
 Los roles de servidor definidos por el usuario que tienen elementos protegibles no se pueden quitar del servidor. Para quitar un rol de servidor definido por el usuario que tiene elementos protegibles, primero debe transferir la propiedad de esos elementos protegibles o eliminarlos.  
  
 Los roles de servidor definidos por el usuario que tienen miembros no se pueden quitar. Para quitar un rol de servidor definido por el usuario que tenga miembros, primero debe quitar los miembros del rol mediante [ALTER SERVER ROLE](../../t-sql/statements/alter-server-role-transact-sql.md).  
  
 Los roles fijos de servidor no se pueden quitar.  
  
 Puede obtener información sobre la pertenencia a roles mediante una consulta de la vista de catálogo [sys.server_role_members](../../relational-databases/system-catalog-views/sys-server-role-members-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL en el rol de servidor o el permiso ALTER ANY SERVER ROLE.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-to-drop-a-server-role"></a>A. Para quitar un rol de servidor  
 En el siguiente ejemplo se quita el rol de servidor `purchasing`.  
  
```sql  
DROP SERVER ROLE purchasing;  
GO  
```  
  
### <a name="b-to-view-role-membership"></a>B. Para ver la pertenencia al rol  
 Para ver la pertenencia al rol, use la página **Rol de servidor (Miembros)** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o ejecute la siguiente consulta:  
  
```sql  
SELECT SRM.role_principal_id, SP.name AS Role_Name,   
SRM.member_principal_id, SP2.name  AS Member_Name  
FROM sys.server_role_members AS SRM  
JOIN sys.server_principals AS SP  
    ON SRM.Role_principal_id = SP.principal_id  
JOIN sys.server_principals AS SP2   
    ON SRM.member_principal_id = SP2.principal_id  
ORDER BY  SP.name,  SP2.name  
```  
  
### <a name="c-to-view-role-membership"></a>C. Para ver la pertenencia al rol  
 Para determinar si un rol de servidor es propietario de otro rol de servidor, ejecute la siguiente consulta:  
  
```sql  
SELECT SP1.name AS RoleOwner, SP2.name AS Server_Role  
FROM sys.server_principals AS SP1  
JOIN sys.server_principals AS SP2  
    ON SP1.principal_id = SP2.owning_principal_id   
ORDER BY SP1.name ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-role-transact-sql.md)   
 [CREATE ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-role-transact-sql.md)   
 [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md)   
 [DROP ROLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-role-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)   
 [sys.database_role_members &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-role-members-transact-sql.md)   
 [sys.database_principals &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md)  
  
  
