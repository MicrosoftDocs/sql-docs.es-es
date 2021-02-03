---
description: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
title: ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_DATABASE_AUDIT_SPECIFICATION_TSQL
- ALTER DATABASE AUDIT SPECIFICATION
- ALTER_DATABASE_AUDIT_TSQL
- ALTER DATABASE AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER DATABASE AUDIT SPECIFICATION statement
ms.assetid: 85f4e7e6-a330-4de0-9048-64f386ccc314
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 088c1503fd306386ca85f36be97b78dcf480e7ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195932"
---
# <a name="alter-database-audit-specification-transact-sql"></a>ALTER DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifica un objeto de especificación de auditoría de base de datos usando la característica Auditoría de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    [ FOR SERVER AUDIT audit_name ]  
    [ { { ADD | DROP } (   
           { <audit_action_specification> | audit_action_group_name }   
                )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      <action_specification>[ ,...n ] ON [ class :: ] securable   
     BY principal [ ,...n ]   
}  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *audit_specification_name*  
 Nombre de la especificación de auditoría.  
  
 *audit_name*  
 Nombre de la auditoría a la que se aplica esta especificación.  
  
 *audit_action_specification*  
 Nombre de una o varias acciones de auditoría de nivel de base de datos. Para ver una lista de grupos de acciones de auditoría, vea [Grupos de acciones y acciones de SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Nombre de uno o varios grupos de acciones de auditoría de nivel de base de datos. Para ver una lista de grupos de acciones de auditoría, vea [Grupos de acciones y acciones de SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Nombre de clase (si procede) en el objeto protegible.  
  
 *securable*  
 Tabla, vista u otro objeto protegible de la base de datos al que se debe aplicar la acción de auditoría o el grupo de acciones de auditoría. Para más información, consulte [Securables](../../relational-databases/security/securables.md).  
  
 *column*  
 Nombre de columna (si procede) en el objeto protegible.  
  
 *principal*  
 Nombre de la entidad de seguridad de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a la que se debe aplicar la acción de auditoría o el grupo de acciones de auditoría. Para más información, vea [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH **(** STATE **=** { ON | OFF } **)**  
 Habilita o deshabilita la recopilación de registros por parte de la auditoría para esta especificación de auditoría. Los cambios de estado de la especificación de auditoría se deben realizar fuera de una transacción de usuario y no puede haber otros cambios en la misma instrucción cuando la transición es de ON a OFF.  
  
## <a name="remarks"></a>Observaciones  
 Las especificaciones de auditoría de base de datos son objetos no protegibles que residen en una base de datos determinada. Para poder realizar cambios en una especificación de auditoría de base de datos, es necesario establecer su estado en OFF. Si se ejecuta ALTER DATABASE AUDIT SPECIFICATION cuando una auditoría está habilitada con opciones distintas de STATE=OFF, aparecerá un mensaje de error. Para obtener más información, consulte [tempdb Database](../../relational-databases/databases/tempdb-database.md).  
  
## <a name="permissions"></a>Permisos  
 Los usuarios con el permiso ALTER ANY DATABASE AUDIT pueden modificar las especificaciones de auditoría de base de datos y enlazarlas a cualquier auditoría.  
  
 Después de crearse una especificación de auditoría de base de datos, podrá ser vista por entidades de seguridad que cuenten con los permisos CONTROL SERVER o ALTER ANY DATABASE AUDIT, así como la cuenta sysadmin, o por entidades de seguridad que tengan acceso explícito a la auditoría.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se modifica una especificación de auditoría de base de datos denominada `HIPAA_Audit_DB_Specification` en la que se auditan las instrucciones `SELECT` emitidas por el usuario `dbo` para una auditoría de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] denominada `HIPAA_Audit`.  
  
```sql  
ALTER DATABASE AUDIT SPECIFICATION HIPAA_Audit_DB_Specification  
FOR SERVER AUDIT HIPAA_Audit  
    ADD (SELECT  
         ON OBJECT::dbo.Table1  
         BY dbo)  
    WITH (STATE = ON);  
GO  
```  
  
 Para ver un ejemplo completo de cómo crear una auditoría, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [DROP DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-database-audit-specification-transact-sql.md)   
 [ALTER AUTHORIZATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-authorization-transact-sql.md)   
 [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md)   
 [sys.server_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audits-transact-sql.md)   
 [sys.server_file_audits &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-file-audits-transact-sql.md)   
 [sys.server_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specifications-transact-sql.md)   
 [sys.server_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-server-audit-specification-details-transact-sql.md)   
 [sys.database_audit_specifications &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specifications-transact-sql.md)   
 [sys.database_audit_specification_details &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-audit-specification-details-transact-sql.md)   
 [sys.dm_server_audit_status &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-server-audit-status-transact-sql.md)   
 [sys.dm_audit_actions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-actions-transact-sql.md)   
 [Crear una auditoría de servidor y una especificación de auditoría de servidor](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
