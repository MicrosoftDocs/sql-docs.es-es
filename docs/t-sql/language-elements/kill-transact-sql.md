---
description: KILL (Transact-SQL)
title: KILL (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- KILL_TSQL
- KILL
dev_langs:
- TSQL
helpviewer_keywords:
- WITH STATUSONLY option
- terminating distributed transactions
- distributed transactions [SQL Server], terminating
- in-doubt transactions
- IDs [SQL Server], user processes
- ending distributed transactions [SQL Server]
- stopping distributed transactions
- session IDs [SQL Server]
- system process termination [SQL Server]
- stopping processes
- ending processes [SQL Server]
- UOW [SQL Server]
- orphaned transactions [SQL Server]
- unit of work [SQL Server]
- process termination [SQL Server]
- KILL statement
- terminating process
ms.assetid: 071cf260-c794-4b45-adc0-0e64097938c0
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c91a736847cee951b638978551d162a846bb930
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205461"
---
# <a name="kill-transact-sql"></a>KILL (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Termina un proceso de usuario basado en el identificador de sesión o la unidad de trabajo (UOW). Si el identificador de sesión o UOW especificado tiene que deshacer muchas operaciones, puede que la instrucción KILL tarde en completarse. El proceso tarda más tiempo en completarse especialmente cuando implique revertir una transacción larga.  
  
KILL termina una conexión normal, que detiene internamente las transacciones asociadas al identificador de sesión especificado. A veces, podría estar en uso el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC). Si MS DTC está en uso, también puede utilizar la instrucción para terminar transacciones distribuidas huérfanas y dudosas.  
  
![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server  
  
KILL { session ID [ WITH STATUSONLY ] | UOW [ WITH STATUSONLY | COMMIT | ROLLBACK ] }    

```  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
KILL 'session_id'  
[;]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos

_Identificador de sesión_   
Es el identificador de sesión del proceso que se va a terminar. `session_id` es un entero único (**int**) asignado a cada conexión de usuario en el momento de realizarla. El valor del Id. de sesión asociado con la conexión mientras ésta dure. Cuando la conexión finaliza, el valor entero se libera y se puede volver a asignar a una nueva conexión.  

La consulta siguiente puede ayudarle a identificar el valor `session_id` que quiere terminar:  

 ```sql  
 SELECT conn.session_id, host_name, program_name,
     nt_domain, login_name, connect_time, last_request_end_time 
FROM sys.dm_exec_sessions AS sess
JOIN sys.dm_exec_connections AS conn
    ON sess.session_id = conn.session_id;

```  


_UOW_   
Identifica el identificador de unidad de trabajo (UOW) de las transacciones distribuidas. _UOW_ es un GUID que puede obtenerse desde la columna request_owner_guid de la vista de administración dinámica `sys.dm_tran_locks`. _UOW_ también puede obtenerse desde el registro de errores o a través del monitor MS DTC. Para obtener más información sobre cómo supervisar transacciones distribuidas, consulte la documentación de MS DTC.  
  
Use KILL \<UOW> para detener transacciones distribuidas sin resolver. Estas transacciones no están asociadas a ningún identificador de sesión real, sino que se asocian artificialmente al identificador de sesión = "-2". Este identificador de sesión facilita la identificación de transacciones sin resolver consultando la columna del identificador de sesión en las vistas de administración dinámica `sys.dm_tran_locks`, ` sys.dm_exec_sessions` o `sys.dm_exec_requests`.  

_WITH STATUSONLY_   
Se utiliza para generar un informe de progreso para un argumento _UOW_ o `session_id` especificado que se está revirtiendo a causa de una instrucción KILL anterior. KILL WITH STATUSONLY no termina la UOW o el identificador de sesión ni lo revierte. El comando solo muestra el progreso actual de la reversión.

_WITH COMMIT_   
Se utiliza para terminar una transacción distribuida sin resolver con confirmación. Solo se aplica a las transacciones distribuidas. Debe especificar una _UOW_ para utilizar esta opción.  Vea [Transacciones distribuidas](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions) para más información.

_WITH ROLLBACK_   
Se utiliza para terminar una transacción distribuida sin resolver con reversión. Solo se aplica a las transacciones distribuidas. Debe especificar una _UOW_ para utilizar esta opción.  Vea [Transacciones distribuidas](../../database-engine/availability-groups/windows/configure-availability-group-for-distributed-transactions.md#manage-unresolved-transactions) para más información.


  
## <a name="remarks"></a>Observaciones  
KILL se utiliza normalmente para terminar un proceso que está bloqueando otros procesos importantes mediante bloqueos. KILL también se puede usar para detener un proceso que está ejecutando una consulta que usa los recursos necesarios del sistema. Los procesos del sistema y los procesos que ejecutan un procedimiento almacenado extendido no se pueden terminar.  
  
Use KILL con cuidado, especialmente cuando se estén ejecutando procesos críticos. No puede eliminar sus propios procesos. Tampoco debe eliminar los siguientes procesos:  
  
-   AWAITING COMMAND  
-   CHECKPOINT SLEEP  
-   LAZY WRITER  
-   LOCK MONITOR  
-   SIGNAL HANDLER  
  
Use @@SPID para mostrar el valor session ID de la sesión actual.  
  
Para obtener un informe de los valores de los identificadores de sesión activos, consulte la columna session_id de las vistas de administración dinámica sys.dm_tran_locks, sys.dm_exec_sessions y sys.dm_exec_requests. También puede ver la columna SPID que el procedimiento almacenado del sistema sp_who devuelve. Si se está realizando la reversión de un SPID determinado, la columna cmd del conjunto de resultados sp_who para ese SPID indica KILLED/ROLLBACK.  
  
Cuando una conexión específica tiene un bloqueo aplicado a un recurso de base de datos y bloquea el progreso de otra conexión, el identificador de sesión de la conexión que bloquea se muestra en la columna `blocking_session_id` de `sys.dm_exec_requests` o en la columna `blk` devuelta por `sp_who`.  
  
El comando KILL puede utilizarse para resolver transacciones distribuidas dudosas. Estas transacciones son transacciones distribuidas sin resolver que se producen a causa de reinicios no planeados del servidor de la base de datos o del coordinador de MS DTC. Para obtener más información sobre transacciones dudosas, vea la sección "Confirmación en dos fases" de [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
## <a name="using-with-statusonly"></a>Usar WITH STATUSONLY  
KILL WITH STATUSONLY genera un informe si el identificador de sesión o UOW se revierte a causa de una instrucción KILL _session ID_|_UOW_ anterior. El informe de progreso indica la cantidad de operaciones de reversión completadas, expresada en porcentaje, y la estimación de tiempo restante, expresada en segundos. El informe lo indica de la siguiente forma:  
  
`Spid|UOW <xxx>: Transaction rollback in progress. Estimated rollback completion: <yy>% Estimated time left: <zz> seconds`  
  
Si la reversión del identificador de sesión o UOW ha finalizado antes de que se ejecute la instrucción KILL _session ID_|_UOW_ WITH STATUSONLY, KILL _session ID_|_UOW_ WITH STATUSONLY devuelve el siguiente error:  
  
```
"Msg 6120, Level 16, State 1, Line 1"  
"Status report cannot be obtained. Rollback operation for Process ID <session ID> is not in progress."
```  
Este error también se produce si no se revierte el parámetro identificador de sesión o UOW.


Se puede obtener el mismo informe de estado si se repite la misma instrucción KILL _session ID_|_UOW_ sin la opción WITH STATUSONLY. Sin embargo, no se recomienda repetir la opción de este modo. Si repite una instrucción KILL _session ID_, el nuevo proceso podría terminar si la reversión finaliza y el identificador de sesión se reasigna a una nueva tarea antes de que la nueva instrucción KILL se ejecute. Impida que el nuevo proceso se detenga especificando WITH STATUSONLY.  
  
## <a name="permissions"></a>Permisos  
**[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]:** Requiere el permiso ALTER ANY CONNECTION. ALTER ANY CONNECTION se incluye con la pertenencia los roles fijos de servidor sysadmin o processadmin.  
  
**[!INCLUDE[ssSDS](../../includes/sssds-md.md)]:** Requiere el permiso KILL DATABASE CONNECTION. El inicio de sesión de entidad de seguridad a nivel de servidor tiene KILL DATABASE CONNECTION.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-kill-to-stop-a-session"></a>A. Uso de KILL para detener una sesión  
 En el siguiente ejemplo se muestra cómo detener el identificador de sesión `53`.  
  
```sql  
KILL 53;  
GO  
```  
  
### <a name="b-using-kill-session-id-with-statusonly-to-obtain-a-progress-report"></a>B. Usar KILL Id. de sesión WITH STATUSONLY para obtener un informe de progreso  
En el siguiente ejemplo se genera un estado del proceso de revertir para el Id. de sesión específico.  
  
```sql  
KILL 54;  
KILL 54 WITH STATUSONLY;  
GO  
  
--This is the progress report.  
spid 54: Transaction rollback in progress. Estimated rollback completion: 80% Estimated time left: 10 seconds.  
```  
  
### <a name="c-using-kill-to-stop-an-orphaned-distributed-transaction"></a>C. Uso de KILL para detener una transacción distribuida huérfana  
En el siguiente ejemplo se muestra cómo detener una transacción distribuida huérfana (identificador de sesión = -2) con un valor *UOW* de `D5499C66-E398-45CA-BF7E-DC9C194B48CF`.  
  
```sql  
KILL 'D5499C66-E398-45CA-BF7E-DC9C194B48CF';  
```  

  
## <a name="see-also"></a>Consulte también  
[KILL STATS JOB &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-stats-job-transact-sql.md)   
[KILL QUERY NOTIFICATION SUBSCRIPTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/kill-query-notification-subscription-transact-sql.md)   
[Funciones integradas &#40;Transact-SQL&#41;](~/t-sql/functions/functions.md)   
[SHUTDOWN &#40;Transact-SQL&#41;](../../t-sql/language-elements/shutdown-transact-sql.md)   
[@@SPID &#40;Transact-SQL&#41;](../../t-sql/functions/spid-transact-sql.md)   
[sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
[sys.dm_exec_sessions &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sessions-transact-sql.md)   
[sys.dm_tran_locks &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-tran-locks-transact-sql.md)   
[sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
[sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
  
