---
description: CREATE SERVER AUDIT (Transact-SQL)
title: CREATE SERVER AUDIT (Transact-SQL)
ms.custom: ''
ms.date: 01/07/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE_SERVER_AUDIT_TSQL
- SERVER AUDIT
- SERVER_AUDIT_TSQL
- CREATE SERVER AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- server audit [SQL Server]
- CREATE SERVER AUDIT statement
- audits [SQL Server], creating
ms.assetid: 1c321680-562e-41f1-8eb1-e7fa5ae45cc5
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 9ee8cbb22293995220f6a6cb9126c889a093222d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188477"
---
# <a name="create-server-audit-transact-sql"></a>CREATE SERVER AUDIT (Transact-SQL)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea un objeto de auditoría de servidor utilizando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Para obtener más información, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
CREATE SERVER AUDIT audit_name  
{  
    TO { [ FILE (<file_options> [ , ...n ] ) ] | APPLICATION_LOG | SECURITY_LOG | URL | EXTERNAL_MONITOR }  
    [ WITH ( <audit_options> [ , ...n ] ) ]   
    [ WHERE <predicate_expression> ]  
}  
[ ; ]  
  
<file_options>::=  
{  
        FILEPATH = 'os_file_path'  
    [ , MAXSIZE = { max_size { MB | GB | TB } | UNLIMITED } ]  
    [ , { MAX_ROLLOVER_FILES = { integer | UNLIMITED } } | { MAX_FILES = integer } ]  
    [ , RESERVE_DISK_SPACE = { ON | OFF } ]   
}  
  
<audit_options>::=  
{  
    [   QUEUE_DELAY = integer ]  
    [ , ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION } ]  
    [ , AUDIT_GUID = uniqueidentifier ]  
}  
  
<predicate_expression>::=  
{  
    [NOT ] <predicate_factor>   
    [ { AND | OR } [NOT ] { <predicate_factor> } ]   
    [,...n ]  
}  
  
<predicate_factor>::=   
    event_field_name { = | < > | ! = | > | > = | < | < = | LIKE } { number | ' string ' }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 TO { FILE \| APPLICATION_LOG \| SECURITY_LOG \| URL \| EXTERNAL_MONITOR } Determina la ubicación del destino de auditoría. Las opciones son un archivo binario, el registro de la aplicación Windows o el registro de seguridad de Windows. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede escribir en el registro de seguridad de Windows sin configurar valores adicionales en Windows. Para obtener más información, vea [Escribir eventos de auditoría de SQL Server en el registro de seguridad](../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md).  

> [!IMPORTANT]
> En Azure SQL Managed Instance, la auditoría de SQL funciona en el nivel de servidor. Las ubicaciones solo pueden ser `URL` o `EXTERNAL_MONITOR`.
  
 FILEPATH ='*os_file_path*'  
 La ruta de acceso del registro de auditoría. El nombre de archivo se genera en función del nombre de la auditoría y del GUID de la auditoría.  
  
 MAXSIZE = { *max_size }*  
 Especifica el tamaño máximo que puede alcanzar el archivo de auditoría. El valor de *max_size* debe ser un entero seguido de MB, GB, TB o UNLIMITED. El tamaño mínimo que se puede especificar para *max_size* es 2 MB y el máximo, 2.147.483.647 TB. Si se especifica UNLIMITED, el archivo crecerá hasta que se llene el disco. (0 también indica UNLIMITED). Si se especifica un valor inferior a 2 MB, se produce el error MSG_MAXSIZE_TOO_SMALL. El valor predeterminado es UNLIMITED.  
  
 MAX_ROLLOVER_FILES = *{ integer* | UNLIMITED }  
 Especifica el número máximo de archivos que se deben conservar en el sistema de archivos además del archivo actual. El valor de *MAX_ROLLOVER_FILES* debe ser un entero o UNLIMITED. El valor predeterminado es UNLIMITED. Este parámetro se evalúa siempre que se reinicia la auditoría (lo que puede suceder cuando se reinicia la instancia de [!INCLUDE[ssDE](../../includes/ssde-md.md)] o cuando se desactiva la auditoría y, a continuación, se activa de nuevo) o cuando se necesita un nuevo archivo porque se ha alcanzado el MAXSIZE. Cuando se evalúa *MAX_ROLLOVER_FILES*, si el número de archivos supera la configuración de *MAX_ROLLOVER_FILES*, se elimina el archivo más antiguo. Como resultado, cuando la configuración de *MAX_ROLLOVER_FILES* es 0, se crea un archivo cada vez que se evalúa la configuración de *MAX_ROLLOVER_FILES*. Se elimina solo un archivo automáticamente cuando se evalúa la configuración de *MAX_ROLLOVER_FILES*, de modo que cuando se disminuye el valor de *MAX_ROLLOVER_FILES*, el número de archivos no se reduce a menos que se eliminen manualmente los archivos antiguos. El número máximo de archivos que se pueden especificar es 2.147.483.647.  
  
 MAX_FILES =*integer*  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Especifica el número máximo de archivos de auditoría que pueden crearse. No realiza la sustitución incremental al primer archivo cuando se alcanza el límite. Cuando se alcanza el límite de MAX_FILES, cualquier acción que ocasione la generación de eventos de auditoría adicionales producirá un error y se mostrará un mensaje.  
  
 RESERVE_DISK_SPACE = { ON | OFF }  
 Esta opción preasigna el archivo en el disco al valor de MAXSIZE. Se aplica únicamente si MAXSIZE no es igual a UNLIMITED. El valor predeterminado es OFF.  
  
 QUEUE_DELAY =*integer*  
 Determina el tiempo, en milisegundos, que puede transcurrir antes de exigir que se procesen las acciones de auditoría. El valor 0 indica la entrega sincrónica. El valor mínimo que puede establecerse para la cola es 1000 (1 segundo), que es el valor predeterminado. El máximo es 2.147.483.647 (2.147.483,647 segundos, o 24 días, 20 horas, 31 minutos y 23,647 segundos). Si se especifica un número no válido, se producirá el error MSG_INVALID_QUEUE_DELAY.  
  
 ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION }  
 Indica si la escritura de la instancia en el destino debe suspender, continuar o detener [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si el destino no puede escribir en el registro de auditoría. El valor predeterminado es CONTINUE.  
  
 CONTINUE  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Las operaciones de  continúan. Los registros de auditoría no se conservan. La auditoría continúa intentando el registro de eventos y se reanuda si se resuelve la condición de error. La selección de la opción Continuar puede permitir que una actividad no se audite, con lo que se infringirían las directivas de seguridad. Utilice esta opción cuando la operación de continuación del [!INCLUDE[ssDE](../../includes/ssde-md.md)] sea más importante que el mantenimiento de una auditoría completa.  
  
SHUTDOWN  
Obliga a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a apagarse si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede escribir datos en el destino de auditoría por algún motivo. El inicio de sesión que ejecuta la instrucción `CREATE SERVER AUDIT` debe tener el permiso `SHUTDOWN` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El comportamiento de apagado persiste aun cuando el permiso `SHUTDOWN` se revoque más adelante del inicio de sesión que ejecuta la instrucción. Si el usuario no tiene este permiso, la instrucción producirá un error y la auditoría no se creará. Utilice la opción si un error de auditoría puede poner en peligro la seguridad o la integridad del sistema. Para obtener más información, vea [SHUTDOWN](../../t-sql/language-elements/shutdown-transact-sql.md).  
  
 FAIL_OPERATION  
 Las acciones de base de datos producen un error si generan eventos auditados. Las acciones que no generan eventos auditados pueden continuar, pero no pueden producirse eventos auditados. La auditoría continúa intentando el registro de eventos y se reanuda si se resuelve la condición de error. Utilice esta opción si el mantenimiento de una auditoría completa es más importante que el acceso total al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.

 AUDIT_GUID =*uniqueidentifier*  
 Para que sea compatible con escenarios como la creación de reflejo de la base de datos, una auditoría necesita un GUID específico que coincida con el de la base de datos reflejada. No se puede modificar el GUID una vez creada la auditoría.  
  
 predicate_expression  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Especifica la expresión de predicado usada para determinar si debe procesarse o no un evento. Las expresiones de predicado se limitan a 3.000 caracteres, lo que limita los argumentos de cadena.  
  
 event_field_name  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Es el nombre del campo de evento que identifica el origen del predicado. Los campos de auditoría se describen en [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md). Todos los campos se pueden filtrar, excepto `file_name`, `audit_file_offset` y `event_time`.  

> [!NOTE]  
>  Si bien los campos `action_id` y `class_type` son de tipo **varchar** en sys.fn_get_audit_file, solo se pueden usar con números cuando sean un origen de predicado para el filtrado. Ejecute la siguiente consulta para obtener una lista de los valores que se usarán con `class_type`:  
> ```sql
> SELECT spt.[name], spt.[number]
> FROM   [master].[dbo].[spt_values] spt
> WHERE  spt.[type] = N'EOD'
> ORDER BY spt.[name];
> ```


 number  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Es cualquier tipo numérico, incluido el tipo **decimal**. Las limitaciones son la falta de memoria física disponible o un número demasiado grande para ser representado como un entero de 64 bits.  
  
 ' string '  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 Una cadena ANSI o Unicode según lo requerido por la comparación de predicado. No se realiza ninguna conversión implícita de tipos de cadena para las funciones de comparación de predicado. Si se pasa el tipo incorrecto se producirá un error.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se crea una auditoría de servidor, está en un estado deshabilitado.  
  
 La instrucción CREATE SERVER AUDIT está en el ámbito de una transacción. Si se revierte la transacción, también se revierte la instrucción.  
  
## <a name="permissions"></a>Permisos  
 Para crear, modificar o quitar una auditoría de servidor, las entidades de seguridad deben tener el permiso ALTER ANY SERVER AUDIT o CONTROL SERVER.  
  
 Al guardar información de auditoría en un archivo, para tratar de impedir su alteración, restrinja el acceso a la ubicación del archivo.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-creating-a-server-audit-with-a-file-target"></a>A. Crear una auditoría de servidor con destino a un archivo  
 En el ejemplo siguiente se crea una auditoría de servidor denominada `HIPAA_Audit` con un archivo binario como destino y sin ninguna opción.  
  
```sql  
CREATE SERVER AUDIT HIPAA_Audit  
    TO FILE ( FILEPATH ='\\SQLPROD_1\Audit\' );  
```  
  
### <a name="b-creating-a-server-audit-with-a-windows-application-log-target-with-options"></a>B. Crear una auditoría de servidor con destino al registro de la aplicación Windows y con opciones  
 En el ejemplo siguiente se crea una auditoría de servidor denominada `HIPAA_Audit` con destino al registro de la aplicación Windows. La cola se escribe cada segundo y apaga el motor de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si se produce un error.  
  
```sql  
CREATE SERVER AUDIT HIPAA_Audit  
    TO APPLICATION_LOG  
    WITH ( QUEUE_DELAY = 1000,  ON_FAILURE = SHUTDOWN);  
```  
  
###  <a name="c-creating-a-server-audit-containing-a-where-clause"></a><a name="ExampleWhere"></a> C. Crear un servidor de auditoría que contiene una cláusula WHERE  
 En el ejemplo siguiente se crean una base de datos, un esquema y dos tablas para el ejemplo. La tabla denominada `DataSchema.SensitiveData` contiene datos confidenciales y el acceso a la tabla debe registrarse en la auditoría. La tabla denominada `DataSchema.GeneralData` no contiene datos confidenciales. La especificación de auditoría de base de datos audita el acceso a todos los objetos del esquema `DataSchema`. La auditoría de servidor se crea con una cláusula WHERE que limita la auditoría de servidor solamente a la tabla `SensitiveData`. La auditoría de servidor da por hecho que existe una carpeta de auditoría en `C:\SQLAudit`.  
  
```sql  
CREATE DATABASE TestDB;  
GO  
USE TestDB;  
GO  
CREATE SCHEMA DataSchema;  
GO  
CREATE TABLE DataSchema.GeneralData (ID int PRIMARY KEY, DataField varchar(50) NOT NULL);  
GO  
CREATE TABLE DataSchema.SensitiveData (ID int PRIMARY KEY, DataField varchar(50) NOT NULL);  
GO  
-- Create the server audit in the master database  
USE master;  
GO  
CREATE SERVER AUDIT AuditDataAccess  
    TO FILE ( FILEPATH ='C:\SQLAudit\' )  
    WHERE object_name = 'SensitiveData' ;  
GO  
ALTER SERVER AUDIT AuditDataAccess WITH (STATE = ON);  
GO  
-- Create the database audit specification in the TestDB database  
USE TestDB;  
GO  
CREATE DATABASE AUDIT SPECIFICATION [FilterForSensitiveData]  
FOR SERVER AUDIT [AuditDataAccess]   
ADD (SELECT ON SCHEMA::[DataSchema] BY [public])  
WITH (STATE = ON);  
GO  
-- Trigger the audit event by selecting from tables  
SELECT ID, DataField FROM DataSchema.GeneralData;  
SELECT ID, DataField FROM DataSchema.SensitiveData;  
GO  
-- Check the audit for the filtered content  
SELECT * FROM fn_get_audit_file('C:\SQLAudit\AuditDataAccess_*.sqlaudit',default,default);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-transact-sql.md)   
 [DROP SERVER AUDIT  &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-transact-sql.md)   
 [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-specification-transact-sql.md)   
 [ALTER SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/alter-server-audit-specification-transact-sql.md)   
 [DROP SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/drop-server-audit-specification-transact-sql.md)   
 [CREATE DATABASE AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../t-sql/statements/create-database-audit-specification-transact-sql.md)   
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
 [sys.dm_audit_class_type_map &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-audit-class-type-map-transact-sql.md)   
 [Crear una auditoría de servidor y una especificación de auditoría de servidor](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
