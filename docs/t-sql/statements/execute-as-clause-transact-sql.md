---
description: EXECUTE AS (cláusula de Transact-SQL)
title: EXECUTE AS (cláusula de Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- AS
- AS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- permission sets [SQL Server]
- queues [SQL Server]
- stored procedures [SQL Server], executing
- user-defined functions [SQL Server], execution context
- EXECUTE AS
- triggers [SQL Server], execution context
- execution context [SQL Server]
- switching execution context
- functions [SQL Server], execution context
ms.assetid: bd517aa3-f06e-4356-87d8-70de5df4494a
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: f36b2dd484e1308e8b7ee71d19d8f1f6315c3f59
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159150"
---
# <a name="execute-as-clause-transact-sql"></a>EXECUTE AS (cláusula de Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede definir el contexto de ejecución de los siguientes módulos definidos por el usuario: funciones (excepto funciones con valores de tabla insertadas), procedimientos, colas y desencadenadores.  
  
 Al especificar el contexto en el que se ejecuta el módulo, puede controlar qué cuenta de usuario usa [!INCLUDE[ssDE](../../includes/ssde-md.md)] para validar permisos en objetos a los que el módulo hace referencia. De esta forma se dispone de mayor control y flexibilidad para administrar los permisos a lo largo de la cadena de objetos que existe entre los módulos definidos por el usuario y los objetos a los que esos módulos hacen referencia. Los permisos deben concederse a usuarios únicamente del propio módulo, sin tener que concederles permisos explícitos para los objetos referenciados. Solo el usuario que ejecuta el módulo debe tener permisos en los objetos a los que tiene acceso el módulo.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
-- SQL Server Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }   
  
DDL Triggers with Server Scope and logon triggers  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'login_name' }   
  
Queues  
{ EXEC | EXECUTE } AS { SELF | OWNER | 'user_name' }   
```  
  
```syntaxsql
-- Azure SQL Database Syntax  
Functions (except inline table-valued functions), Stored Procedures, and DML Triggers  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | OWNER | 'user_name' }   
  
DDL Triggers with Database Scope  
  
{ EXEC | EXECUTE } AS { CALLER | SELF | 'user_name' }  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 **CALLER**  
 Especifica que las instrucciones dentro del módulo se ejecutan en el contexto del llamador del módulo. El usuario que ejecuta el módulo debe tener los permisos adecuados no solo en el propio módulo, sino también en los objetos de la base de datos a los que el módulo hace referencia.  
  
 CALLER es el valor predeterminado para todos los módulos excepto las colas, y tiene el mismo comportamiento que [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
 CALLER no se puede especificar en una instrucción CREATE QUEUE o ALTER QUEUE.  
  
 **SELF**  
 EXECUTE AS SELF es equivalente a EXECUTE AS *user_name*, donde el usuario especificado es la persona que crea o modifica el módulo. El identificador de usuario real de la persona que crea o modifica los módulos se almacena en la columna **execute_as_principal_id** en las vistas de catálogo **sys.sql_modules** o **sys.service_queues**.  
  
 SELF es el valor predeterminado para colas.  
  
> [!NOTE]  
>  Para cambiar el identificador de usuario de **execute_as_principal_id** en la vista de catálogo **sys.service_queues**, debe especificar de forma explícita el valor de EXECUTE AS en la instrucción ALTER QUEUE.  
  
 OWNER  
 Especifica que las instrucciones dentro del módulo se ejecutan en el contexto del propietario actual del módulo. Si el módulo no tiene un propietario especificado, se usa el propietario del esquema del módulo. OWNER no se puede especificar para los desencadenadores DDL o logon.  
  
> [!IMPORTANT]  
>  OWNER debe asignarse a una cuenta singleton y no puede ser un rol o un grupo.  
  
 **'** *user_name* **'**  
 Especifica que las instrucciones dentro del módulo se ejecutan en el contexto del usuario especificado en *user_name*. Los permisos para los objetos dentro del módulo se comprueban con *user_name*. No se puede especificar *user_name* para desencadenadores DDL con el ámbito de servidor ni para desencadenadores de inicio de sesión. Use *login_name* en su lugar.  
  
 *user_name* debe existir en la base de datos actual y debe ser una cuenta singleton. *user_name* no puede ser un grupo, rol, certificado, clave o cuenta integrada, como NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
 El identificador de usuario del contexto de ejecución se almacena en metadatos y se puede ver en la columna **execute_as_principal_id** en la vista de catálogo **sys.sql_modules** o **sys.assembly_modules**.  
  
 **'** *login_name* **'**  
 Especifica que las instrucciones dentro del módulo se ejecutan en el contexto del usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] especificado en *login_name*. Los permisos para los objetos que haya dentro del módulo se comprueban con *login_name*. *login_name* se puede especificar únicamente para desencadenadores DDL con el ámbito de servidor o para desencadenadores de inicio de sesión.  
  
 *login_name* no puede ser un grupo, rol, certificado, clave o cuenta integrada, como NT AUTHORITY\LocalService, NT AUTHORITY\NetworkService o NT AUTHORITY\LocalSystem.  
  
## <a name="remarks"></a>Comentarios  
 Como [!INCLUDE[ssDE](../../includes/ssde-md.md)] evalúa los permisos en los objetos a los que el módulo hace referencia, depende de la cadena de propiedad que existe entre los objetos que llaman y los objetos a los que se hace referencia. En versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la cadena de propiedad era el único método disponible para evitar tener que conceder al usuario que llama acceso a todos los objetos a los que se hace referencia.  
  
 La cadena de propiedad tiene las siguientes limitaciones:  
  
-   Solo se aplica a instrucciones DML: SELECT, INSERT, UPDATE y DELETE.  
  
-   Los propietarios de los objetos que llaman y llamados deben ser los mismos.  
  
-   No se aplica a consultas dinámicas dentro del módulo.  
  
 Independientemente del contexto de ejecución especificado en el módulo, siempre se aplican las siguientes acciones:  
  
-   Cuando se ejecuta el módulo, [!INCLUDE[ssDE](../../includes/ssde-md.md)] comprueba primero que el usuario que ejecuta el módulo tiene el permiso EXECUTE en el módulo.  
  
-   Las reglas de la cadena de propiedad siguen aplicándose. Esto significa que si los propietarios de los objetos que llaman y llamados son los mismos, no se comprueban los permisos en los objetos subyacentes.  
  
 Cuando un usuario ejecuta un módulo que se ha especificado para ejecutar en un contexto distinto de CALLER, se comprueba el permiso del usuario para ejecutar el módulo, pero también se realizan comprobaciones adicionales de permisos en objetos a los que tiene acceso el módulo contra la cuenta de usuario especificada en la cláusula EXECUTE AS. El usuario que ejecuta el módulo suplanta al usuario especificado.  
  
 El contexto especificado en la cláusula EXECUTE AS del módulo solo es válido durante la ejecución del módulo. El contexto vuelve al llamador cuando finaliza la ejecución del módulo.  
  
## <a name="specifying-a-user-or-login-name"></a>Especificar un nombre de inicio de sesión o usuario  
 Un usuario de base de datos o un inicio de sesión de servidor especificado en la cláusula EXECUTE AS de un módulo no puede quitarse hasta que el módulo se haya modificado para ejecutarse en otro contexto.  
  
 El nombre de inicio de sesión o usuario especificado en la cláusula EXECUTE AS debe existir como una entidad de seguridad en **sys.database_principals** o **sys.server_principals** respectivamente, o la operación de creación o modificación del módulo generará errores. Además, el usuario que crea o modifica el módulo debe tener permisos IMPERSONATE en la entidad de seguridad.  
  
 Si el usuario tiene acceso implícito a la base de datos o instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] porque pertenece a un grupo de Windows, el usuario especificado en la cláusula EXECUTE AS se crea de forma implícita cuando se crea el módulo y se cumple uno de los siguientes requisitos:  
  
-   El inicio de sesión o usuario especificado es miembro del rol fijo de servidor **sysadmin**.  
  
-   El usuario que crea el módulo tiene permiso para crear entidades de seguridad.  
  
 Cuando no se cumple ninguno de esos requisitos, la operación de creación del módulo genera errores.  
  
> [!IMPORTANT]  
>  Si el servicio (MSSQLSERVER) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está ejecutándose como una cuenta local (servicio local o cuenta de usuario local), no tendrá privilegios para obtener la pertenencia a grupo de una cuenta de dominio de Windows especificada en la cláusula EXECUTE AS. Esto generará errores en la ejecución del módulo.  
  
 Por ejemplo, supongamos las siguientes condiciones:  
  
-   El grupo **CompanyDomain\SQLUsers** tiene acceso a la base de datos **Sales**.  
  
-   **CompanyDomain\SqlUser1** es un miembro de **SQLUsers** y, por tanto, tiene acceso a la base de datos **Sales**.  
  
-   El usuario que crea o modifica el módulo tiene permisos para crear entidades de seguridad.  
  
 Cuando se ejecute la siguiente instrucción `CREATE PROCEDURE`, el `CompanyDomain\SqlUser1` se crea de forma implícita como una entidad de seguridad de la base de datos en la base de datos `Sales`.  
  
```sql  
USE Sales;  
GO  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'CompanyDomain\SqlUser1'  
AS  
SELECT user_name();  
GO  
```  
  
## <a name="using-execute-as-caller-stand-alone-statement"></a>Usar la instrucción independiente EXECUTE AS CALLER  
 Use la instrucción independiente EXECUTE AS CALLER dentro de un módulo para establecer el contexto de ejecución al llamador del módulo.  
  
 Suponga que `SqlUser2` llama al siguiente procedimiento almacenado.  
  
```sql  
CREATE PROCEDURE dbo.usp_Demo  
WITH EXECUTE AS 'SqlUser1'  
AS  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
EXECUTE AS CALLER;  
SELECT user_name(); -- Shows execution context is set to SqlUser2, the caller of the module.  
REVERT;  
SELECT user_name(); -- Shows execution context is set to SqlUser1.  
GO  
```  
  
## <a name="using-execute-as-to-define-custom-permission-sets"></a>Usar EXECUTE AS para definir conjuntos de permisos personalizados  
 Cuando desee definir conjuntos de permisos personalizados puede ser muy útil especificar un contexto de ejecución para un módulo. Por ejemplo, algunas acciones, como TRUNCATE TABLE, no tienen permisos que se puedan conceder. Al incorporar la instrucción TRUNCATE TABLE en un módulo y especificar que ese módulo se ejecute con un usuario que tiene permisos para modificar la tabla, puede ampliar los permisos para truncar la tabla al usuario a quien ha concedido permisos EXECUTE para el módulo.  
  
 Para ver la definición del módulo con el contexto de ejecución especificado, use la vista de catálogo [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md).  
  
## <a name="best-practice"></a>Procedimiento recomendado  
 Especifique un inicio de sesión o usuario que tenga al menos los privilegios requeridos para realizar las operaciones definidas en el módulo. Por ejemplo, no especifique una cuenta de propietario de base de datos a menos que tenga los permisos requeridos.  
  
## <a name="permissions"></a>Permisos  
 Para ejecutar un módulo especificado con EXECUTE AS, el llamador debe tener permisos EXECUTE en el módulo.  
  
 Para ejecutar un módulo CLR especificado con EXECUTE AS que tiene acceso a recursos en otra base de datos o servidor, la base de datos o el servidor de destino deben confiar en el autenticador de la base de datos que ha originado el módulo (la base de datos de origen).  
  
 Para especificar la cláusula EXECUTE AS cuando cree o modifique un módulo, debe tener permisos IMPERSONATE en la entidad de seguridad especificada y, además, permisos para crear el módulo. Siempre puede suplantarse a usted mismo. Cuando no se especifica ningún contexto de ejecución o se especifica EXECUTE AS CALLER, no se requieren permisos IMPERSONATE.  
  
 Para especificar un *login_name* o *user_name* que tenga acceso implícito a la base de datos porque pertenece a un grupo de Windows, debe tener permisos CONTROL en la base de datos.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se crea un procedimiento almacenado en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)] y se asigna el contexto de ejecución a `OWNER`.  
  
```sql  
CREATE PROCEDURE HumanResources.uspEmployeesInDepartment   
@DeptValue int  
WITH EXECUTE AS OWNER  
AS  
    SET NOCOUNT ON;  
    SELECT e.BusinessEntityID, c.LastName, c.FirstName, e.JobTitle  
    FROM Person.Person AS c   
    INNER JOIN HumanResources.Employee AS e  
        ON c.BusinessEntityID = e.BusinessEntityID  
    INNER JOIN HumanResources.EmployeeDepartmentHistory AS edh  
        ON e.BusinessEntityID = edh.BusinessEntityID  
    WHERE edh.DepartmentID = @DeptValue  
    ORDER BY c.LastName, c.FirstName;  
GO  
  
-- Execute the stored procedure by specifying department 5.  
EXECUTE HumanResources.uspEmployeesInDepartment 5;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [sys.assembly_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-assembly-modules-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.service_queues &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-service-queues-transact-sql.md)   
 [REVERT &#40;Transact-SQL&#41;](../../t-sql/statements/revert-transact-sql.md)   
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)  
  
  
