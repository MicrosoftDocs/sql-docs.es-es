---
description: ALTER SERVER AUDIT (Transact-SQL)
title: ALTER SERVER AUDIT  (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/07/2018
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_SERVER_AUDIT_TSQL
- ALTER SERVER AUDIT
dev_langs:
- TSQL
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
- ALTER SERVER AUDIT statement
ms.assetid: 63426d31-7a5c-4378-aa9e-afcf4f64ceb3
author: VanMSFT
ms.author: vanto
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: b85d564e6a1a29ec72eebfb2248ae58775c82b6e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182185"
---
# <a name="alter-server-audit--transact-sql"></a>ALTER SERVER AUDIT (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Modifica un objeto de auditoría de servidor mediante la característica [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Audit. Para obtener más información, vea [SQL Server Audit &#40;motor de base de datos&#41;](../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
ALTER SERVER AUDIT audit_name  
{  
    [ TO { { FILE ( <file_options> [, ...n] ) } | APPLICATION_LOG | SECURITY_LOG } | URL]  
    [ WITH ( <audit_options> [ , ...n] ) ]   
    [ WHERE <predicate_expression> ]  
}  
| REMOVE WHERE  
| MODIFY NAME = new_audit_name  
[ ; ]  
  
<file_options>::=  
{  
      FILEPATH = 'os_file_path'   
    | MAXSIZE = { max_size { MB | GB | TB } | UNLIMITED }   
    | MAX_ROLLOVER_FILES = { integer | UNLIMITED }   
    | MAX_FILES = integer   
    | RESERVE_DISK_SPACE = { ON | OFF }   
}  
  
<audit_options>::=  
{  
      QUEUE_DELAY = integer   
    | ON_FAILURE = { CONTINUE | SHUTDOWN | FAIL_OPERATION }   
    | STATE = = { ON | OFF }   
}  
  
<predicate_expression>::=  
{  
    [NOT ] <predicate_factor>   
    [ { AND | OR } [NOT ] { <predicate_factor> } ]   
    [,...n ]  
}  
  
<predicate_factor>::=   
    event_field_name { = | < > | ! = | > | > = | < | < = } { number | ' string ' }  
```  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 TO { FILE \| APPLICATION_LOG \| SECURITY \|URL}  
 Determina la ubicación del destino de la auditoría. Las opciones son un archivo binario, el registro de la aplicación Windows o el registro de seguridad de Windows.  

> [!IMPORTANT]
> En Azure SQL Managed Instance, la auditoría de SQL funciona en el nivel de servidor y almacena archivos `.xel` en Azure Blob Storage.
  
 FILEPATH **= '** _os\_file\_path_ **'**  
 La ruta de acceso de la pista de auditoría. El nombre de archivo se genera en función del nombre de la auditoría y del GUID de la auditoría.  
  
 MAXSIZE **=** _max\_size_  
 Especifica el tamaño máximo que puede alcanzar el archivo de auditoría. El valor de *max_size* debe ser un entero seguido de **MB**, **GB**, **TB** o **UNLIMITED**. El tamaño mínimo que se puede especificar para *max_size* es 2 **MB** y el máximo, 2 147 483 647 **TB**. Si se especifica **UNLIMITED**, el archivo crecerá hasta que se llene el disco. Si se especifica un valor inferior a 2 MB, se produce el error MSG_MAXSIZE_TOO_SMALL. El valor predeterminado es **UNLIMITED**.  
  
 MAX_ROLLOVER_FILES **=** _integer_ | **UNLIMITED**  
 Especifica el número máximo de archivos que se deben conservar en el sistema de archivos. Si se establece MAX_ROLLOVER_FILES=0, no se impone ningún límite en cuanto al número de archivos de sustitución incremental que se crean. El valor predeterminado es 0. El número máximo de archivos que se pueden especificar es 2.147.483.647.  
  
 MAX_FILES =*integer*  
 Especifica el número máximo de archivos de auditoría que pueden crearse. No realiza la sustitución incremental al primer archivo cuando se alcanza el límite. Cuando se alcanza el límite de MAX_FILES, cualquier acción que ocasione la generación de eventos de auditoría adicionales producirá un error y se mostrará un mensaje.  
**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 RESERVE_DISK_SPACE **=** { ON | OFF }  
 Esta opción preasigna el archivo en el disco al valor de MAXSIZE. Solo se aplica si MAXSIZE no es igual a UNLIMITED. El valor predeterminado es OFF.  
  
 QUEUE_DELAY **=** _integer_  
 Determina el tiempo, en milisegundos, que puede transcurrir antes de exigir que se procesen las acciones de auditoría. El valor 0 indica la entrega sincrónica. El valor mínimo que puede establecerse para la cola es 1000 (1 segundo), que es el valor predeterminado. El máximo es 2.147.483.647 (2.147.483,647 segundos, o 24 días, 20 horas, 31 minutos y 23,647 segundos). Si se especifica un número no válido, se producirá el error MSG_INVALID_QUEUE_DELAY.  
  
 ON_FAILURE **=** { CONTINUE | SHUTDOWN | FAIL_OPERATION}  
 Indica si la escritura de la instancia en el destino se debe suspender, continuar o detener si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede escribir en el registro de auditoría.  
  
 CONTINUE  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Las operaciones de  continúan. Los registros de auditoría no se conservan. La auditoría continúa intentando el registro de eventos y se reanuda si se resuelve la condición de error. La selección de la opción Continuar puede permitir que una actividad no se audite, con lo que se infringirían las directivas de seguridad. Utilice esta opción cuando la operación de continuación del [!INCLUDE[ssDE](../../includes/ssde-md.md)] sea más importante que el mantenimiento de una auditoría completa.  
  
SHUTDOWN  
Obliga a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a apagarse si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede escribir datos en el destino de auditoría por algún motivo. El inicio de sesión que ejecuta la instrucción `ALTER` debe tener el permiso `SHUTDOWN` en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. El comportamiento de apagado persiste aun cuando el permiso `SHUTDOWN` se revoque más adelante del inicio de sesión que ejecuta la instrucción. Si el usuario no tiene este permiso, la instrucción producirá un error y la auditoría no se modificará. Utilice la opción si un error de auditoría puede poner en peligro la seguridad o la integridad del sistema. Para obtener más información, vea [SHUTDOWN](../../t-sql/language-elements/shutdown-transact-sql.md). 
  
 FAIL_OPERATION  
 Las acciones de base de datos producen un error si generan eventos auditados. Las acciones que no generan eventos auditados pueden continuar, pero no pueden producirse eventos auditados. La auditoría continúa intentando el registro de eventos y se reanuda si se resuelve la condición de error. Utilice esta opción si el mantenimiento de una auditoría completa es más importante que el acceso total al [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.   
  
 STATE **=** { ON | OFF }  
 Habilita o deshabilita la recopilación de registros en la auditoría. El cambio de estado de una auditoría que se está ejecutando (de ON a OFF) crea una entrada de auditoría que incluye la auditoría detenida, la entidad de seguridad que la ha detenido y la hora en la que se ha detenido.  
  
 MODIFY NAME = *new_audit_name*  
 Cambia el nombre de la auditoría. No se puede usar con ninguna otra opción.  
  
 predicate_expression  
 Especifica la expresión de predicado usada para determinar si debe procesarse o no un evento. Las expresiones de predicado se limitan a 3.000 caracteres, lo que limita los argumentos de cadena.  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 event_field_name  
 Es el nombre del campo de evento que identifica el origen del predicado. Los campos de auditoría se describen en [sys.fn_get_audit_file &#40;Transact-SQL&#41;](../../relational-databases/system-functions/sys-fn-get-audit-file-transact-sql.md). Todos los campos pueden auditarse excepto `file_name` y `audit_file_offset`.  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 number  
 Es cualquier tipo numérico, incluido el tipo **decimal**. Las limitaciones son la falta de memoria física disponible o un número demasiado grande para ser representado como un entero de 64 bits.  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
 ' string '  
 Una cadena ANSI o Unicode según lo requerido por la comparación de predicado. No se realiza ninguna conversión implícita de tipos de cadena para las funciones de comparación de predicado. Si se pasa el tipo incorrecto se producirá un error.  
 **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.  
  
## <a name="remarks"></a>Observaciones  
 Se debe especificar al menos una de las cláusulas TO, WITH o MODIFY NAME al llamar a ALTER AUDIT.  
  
 Para poder realizar cambios en una auditoría, es necesario establecer su estado en OFF. Si se habilita una auditoría con opciones distintas de STATE=OFF y se ejecuta ALTER AUDIT, aparece un mensaje de error de MSG_NEED_AUDIT_DISABLED.  
  
 Es posible agregar, modificar o quitar especificaciones de auditoría sin detener la auditoría.  
  
 Sin embargo, no se puede cambiar el GUID de una auditoría una vez creada ésta.  
 
 La instrucción **ALTER SERVER AUDIT** no se puede usar dentro de una transacción de usuario.
 
## <a name="permissions"></a>Permisos  
 Para crear, modificar o quitar una entidad de seguridad de auditoría de servidor, se deben tener los permisos ALTER ANY SERVER AUDIT o CONTROL SERVER.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-changing-a-server-audit-name"></a>A. Cambiar el nombre de una auditoría de servidor  
 En el ejemplo siguiente se cambia el nombre de la auditoría de servidor `HIPAA_Audit` a `HIPAA_Audit_Old`.  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
MODIFY NAME = HIPAA_Audit_Old;  
GO  
ALTER SERVER AUDIT HIPAA_Audit_Old  
WITH (STATE = ON);  
GO  
```  
  
### <a name="b-changing-a-server-audit-target"></a>B. Cambiar el destino de una auditoría de servidor  
 En el ejemplo siguiente se cambia la auditoría de servidor denominada `HIPAA_Audit` a un destino de archivo.  
  
```sql  
USE master  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = OFF);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
TO FILE (FILEPATH ='\\SQLPROD_1\Audit\',  
          MAXSIZE = 1000 MB,  
          RESERVE_DISK_SPACE=OFF)  
WITH (QUEUE_DELAY = 1000,  
       ON_FAILURE = CONTINUE);  
GO  
ALTER SERVER AUDIT HIPAA_Audit  
WITH (STATE = ON);  
GO  
```  
  
### <a name="c-changing-a-server-audit-where-clause"></a>C. Cambiar una cláusula WHERE de auditoría de servidor  
 En el ejemplo siguiente se modifica la cláusula WHERE creada en el ejemplo C de [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../t-sql/statements/create-server-audit-transact-sql.md). La nueva cláusula WHERE filtra por el evento definido por el usuario si es 27.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
WHERE user_defined_event_id = 27;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="d-removing-a-where-clause"></a>D. Quitar una cláusula WHERE  
 En el ejemplo siguiente se quita una expresión de predicado de cláusula WHERE.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
REMOVE WHERE;  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = ON);  
GO  
```  
  
### <a name="e-renaming-a-server-audit"></a>E. Cambiar el nombre de una auditoría de servidor  
 En el ejemplo siguiente se cambia el nombre de la auditoría de servidor de `FilterForSensitiveData` a `AuditDataAccess`.  
  
```sql  
ALTER SERVER AUDIT [FilterForSensitiveData] WITH (STATE = OFF)  
GO  
ALTER SERVER AUDIT [FilterForSensitiveData]  
MODIFY NAME = AuditDataAccess;  
GO  
ALTER SERVER AUDIT [AuditDataAccess] WITH (STATE = ON);  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
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
 [Crear una auditoría de servidor y una especificación de auditoría de servidor](../../relational-databases/security/auditing/create-a-server-audit-and-server-audit-specification.md)  
  
  
