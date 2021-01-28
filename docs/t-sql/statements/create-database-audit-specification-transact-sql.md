---
title: CREATE DATABASE AUDIT SPECIFICATION
description: Crea un objeto de especificación de auditoría de base de datos mediante la característica SQL Server Audit.
titleSuffix: SQL Server (Transact-SQL)
ms.custom: seo-lt-2019
ms.date: 01/03/2020
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE DATABASE AUDIT
- DATABASE_AUDIT_SPECIFICATION_TSQL
- DATABASE AUDIT SPECIFICATION
- CREATE_DATABASE_AUDIT_SPECIFICATION_TSQL
- CREATE_DATABASE_AUDIT_TSQL
- CREATE DATABASE AUDIT SPECIFICATION
dev_langs:
- TSQL
helpviewer_keywords:
- database audit specification
- CREATE DATABASE AUDIT SPECIFICATION statement
ms.assetid: 0544da48-0ca3-4a01-ba4c-940e23dc315b
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 8d1f8288a0e09c3382c9c31420fb1da6083292f8
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783504"
---
# <a name="create-database-audit-specification-transact-sql"></a>CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea un objeto de especificación de auditoría de base de datos mediante la característica de auditoría de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE DATABASE AUDIT SPECIFICATION audit_specification_name  
{  
    FOR SERVER AUDIT audit_name   
        [ { ADD ( { <audit_action_specification> | audit_action_group_name } )   
      } [, ...n] ]  
    [ WITH ( STATE = { ON | OFF } ) ]  
}  
[ ; ]  
<audit_action_specification>::=  
{  
      action [ ,...n ]ON [ class :: ] securable BY principal [ ,...n ]  
}  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *audit_specification_name*  
 Es el nombre de la especificación de auditoría.  
  
 *audit_name*  
 Es el nombre de la auditoría a la que se aplica esta especificación.  
  
 *audit_action_specification*  
 Es la especificación de acciones de elementos protegibles mediante entidades de seguridad que se deben grabar en la auditoría.  
  
 *action*  
 Es el nombre de una o varias acciones auditables en el nivel de base de datos. Para ver una lista de acciones de auditoría, vea [Grupos de acciones y acciones de SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *audit_action_group_name*  
 Es el nombre de uno o varios grupos de acciones auditables en el nivel de base de datos. Para ver una lista de grupos de acciones de auditoría, vea [Grupos de acciones y acciones de SQL Server Audit](../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
 *class*  
 Es el nombre de clase (si procede) en el objeto protegible.  
  
 *securable*  
 Es la tabla, vista u otro objeto protegible de la base de datos al que se debe aplicar la acción de auditoría o el grupo de acciones de auditoría. Para más información, consulte [Securables](../../relational-databases/security/securables.md).  
  
 *principal*  
 Es el nombre de la entidad de seguridad de base de datos a la que se debe aplicar la acción de auditoría o el grupo de acciones de auditoría. Para auditar todas las entidades de seguridad de base de datos use la entidad de seguridad de base de datos `public`. Para más información, vea [Entidades de seguridad &#40;motor de base de datos&#41;](../../relational-databases/security/authentication-access/principals-database-engine.md).  
  
 WITH ( STATE = { ON | OFF } )  
 Habilita o deshabilita la recopilación de registros por parte de la auditoría para esta especificación de auditoría.  
  
## <a name="remarks"></a>Observaciones  
 Las especificaciones de auditoría de base de datos son objetos no protegibles que residen en una base de datos determinada. Cuando se crea una especificación de auditoría de servidor de base de datos, está en un estado deshabilitado.  
  
## <a name="permissions"></a>Permisos  
 Los usuarios con el permiso `ALTER ANY DATABASE AUDIT` pueden crear especificaciones de auditoría de base de datos y enlazarlas a cualquier auditoría.  
  
 Después de crear una especificación de auditoría de base de datos, podrán verla los usuarios con el permiso `CONTROL SERVER`, o bien la cuenta `sysadmin`.  
  
## <a name="examples"></a>Ejemplos

### <a name="a-audit-select-and-insert-on-a-table-for-any-database-principal"></a>A. Operaciones SELECT e INSERT de auditoría en una tabla para cualquier entidad de seguridad de base de datos 
 En el siguiente ejemplo se crea una auditoría de servidor denominada `Payrole_Security_Audit` y una especificación de auditoría de base de datos denominada `Payrole_Security_Audit` que audita las instrucciones `SELECT` y `INSERT` por el usuario `dbo`, para la tabla `HumanResources.EmployeePayHistory` de la base de datos `AdventureWorks2012`.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.  
CREATE SERVER AUDIT Payrole_Security_Audit  
    TO FILE ( FILEPATH =   
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ;  
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT Payrole_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Pay_Tables  
FOR SERVER AUDIT Payrole_Security_Audit  
ADD (SELECT , INSERT  
     ON HumanResources.EmployeePayHistory BY dbo )  
WITH (STATE = ON) ;  
GO  
``` 

### <a name="b-audit-any-dml-insert-update-or-delete-on-_all_-objects-in-the-_sales_-schema-for-a-specific-database-role"></a>B. Auditoría de cualquier DML (INSERT, UPDATE o DELETE) en _todos_ los objetos del esquema _sales_ para un rol de base de datos específico  
 En el ejemplo siguiente se crea una auditoría de servidor denominada `DataModification_Security_Audit` y después una especificación de auditoría de base de datos denominada `Audit_Data_Modification_On_All_Sales_Tables` que audita las instrucciones `INSERT`, `UPDATE` y `DELETE` por los usuarios en un nuevo rol de base de datos `SalesUK`, para todos los objetos del esquema `Sales` de la base de datos `AdventureWorks2012`.  
  
```sql  
USE master ;  
GO  
-- Create the server audit.
-- Change the path to a path that the SQLServer Service has access to. 
CREATE SERVER AUDIT DataModification_Security_Audit  
    TO FILE ( FILEPATH = 
'C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\DATA' ) ; 
GO  
-- Enable the server audit.  
ALTER SERVER AUDIT DataModification_Security_Audit   
WITH (STATE = ON) ;  
GO  
-- Move to the target database.  
USE AdventureWorks2012 ;  
GO  
CREATE ROLE SalesUK
GO
-- Create the database audit specification.  
CREATE DATABASE AUDIT SPECIFICATION Audit_Data_Modification_On_All_Sales_Tables  
FOR SERVER AUDIT DataModification_Security_Audit  
ADD ( INSERT, UPDATE, DELETE  
     ON Schema::Sales BY SalesUK )  
WITH (STATE = ON) ;    
GO  
```  


## <a name="see-also"></a>Consulte también  
 [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md)   
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION (Transact-SQL)](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
 [ALTER DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-audit-specification-transact-sql.md)   
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
  
  
