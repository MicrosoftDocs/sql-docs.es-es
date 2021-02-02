---
description: sys.sysprocesses (Transact-SQL)
title: Procesos de sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysprocesses_TSQL
- sys.sysprocesses_TSQL
- sys.sysprocesses
- sysprocesses
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysprocesses compatibility view
- sysprocesses system table
ms.assetid: 60a36d36-54b3-4bd6-9cac-702205a21b16
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8cf9c34360cfabff16dd4823738168b9611a90ba
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235874"
---
# <a name="syssysprocesses-transact-sql"></a>sys.sysprocesses (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene información sobre los procesos que se ejecutan en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Estos procesos pueden ser procesos del cliente o procesos del sistema. Para obtener acceso a sysprocesses, debe estar en el contexto de la base de datos maestra o utilizar el nombre de tres partes master.dbo.sysprocesses.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|spid|**smallint**|Identificador de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|kpid|**smallint**|Identificador de subproceso de Windows.|  
|blocked|**smallint**|Id. de la sesión que bloquea la solicitud. Si esta columna es NULL, la solicitud no está bloqueada o la información de la sesión de bloqueo no está disponible (o no puede ser identificada).<br /><br /> -2 = El recurso de bloqueo es propiedad de una transacción distribuida huérfana.<br /><br /> -3 = El recurso de bloqueo es propiedad de una transacción de recuperación diferida.<br /><br /> -4 = No se pudo determinar el Id. de sesión del propietario del bloqueo temporal a causa de transiciones internas de estado del bloqueo temporal.|  
|waittype|**binario (2)**|Reservado.|  
|waittime|**bigint**|Tiempo de espera actual (en milisegundos).<br /><br /> 0 = El proceso no está en espera.|  
|lastwaittype|**NCHAR (32)**|Cadena que indica el nombre del último tipo de espera, o del actual.|  
|waitresource|**NCHAR (256)**|Representación textual de un recurso de bloqueo.|  
|dbid|**smallint**|Identificador de la base de datos que el proceso utiliza actualmente.|  
|uid|**smallint**|Identificador del usuario que ha ejecutado el comando. Produce un desbordamiento o devuelve NULL si el número de usuarios y roles es superior a 32.767.|  
|cpu|**int**|Tiempo de CPU acumulado para el proceso. La entrada se actualiza para todos los procesos, independientemente de si la opción SET STATISTICS TIME está establecida en ON o en OFF.|  
|physical_io|**bigint**|Número de lecturas y escrituras en disco acumuladas para el proceso.|  
|memusage|**int**|Número de páginas de la memoria caché de procedimientos que están asignadas actualmente al proceso. Un número negativo indica que el proceso está liberando memoria asignada por otro proceso.|  
|login_time|**datetime**|Hora a la que un proceso de cliente inició una sesión en el servidor.|  
|last_batch|**datetime**|Hora a la que un proceso de cliente ejecutó por última vez una llamada a un procedimiento almacenado remoto o una instrucción EXECUTE.|  
|ecid|**smallint**|Identificador de contexto de ejecución usado para identificar de forma única los subprocesos que operan en nombre de un único proceso.|  
|open_tran|**smallint**|Número de transacciones abiertas para el proceso.|  
|status|**NCHAR (30)**|Estado del identificador de proceso. Los valores posibles son:<br /><br /> **inactivo**  =  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está restableciendo la sesión.<br /><br /> en **ejecución** = la sesión está ejecutando uno o varios lotes. Si Conjuntos de resultados activos múltiples (MARS) está habilitado, una sesión puede ejecutar varios lotes. Para obtener más información, vea [Usar conjuntos de resultados activos múltiples &#40;MARS&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).<br /><br /> **background** = la sesión está ejecutando una tarea en segundo plano, como la detección de interbloqueos.<br /><br /> **Rollback** = la sesión tiene una reversión de la transacción en curso.<br /><br /> **Pending** = la sesión está esperando a que un subproceso de trabajo esté disponible.<br /><br /> **Runnable** = la tarea de la sesión está en la cola de ejecutables de un programador mientras espera obtener un cuanto de tiempo.<br /><br /> **SpinLoop** = la tarea de la sesión está esperando a que se libere una Spinlock.<br /><br /> **suspendido** = la sesión está esperando a que se complete un evento, como e/s.|  
|sid|**binario (86)**|Identificador único global (GUID) del usuario.|  
|hostname|**nchar(128)**|Nombre de la estación de trabajo.|  
|program_name|**nchar(128)**|Nombre del programa de aplicación.|  
|hostprocess|**nchar(10)**|Número de identificación de proceso de la estación de trabajo.|  
|cmd|**NCHAR (52)**|Comando que se está ejecutando actualmente.|  
|nt_domain|**nchar(128)**|Dominio de Windows para el cliente, si se utiliza la autenticación de Windows o una conexión de confianza.|  
|nt_username|**nchar(128)**|Nombre de usuario de Windows del proceso, si se utiliza la autenticación de Windows una conexión de confianza.|  
|net_address|**NCHAR (12)**|Identificador único asignado al adaptador de red de la estación de trabajo de cada usuario. Cuando un usuario inicia una sesión, este identificador se inserta en la columna net_address.|  
|net_library|**NCHAR (12)**|Columna en la que se almacena la biblioteca de red del cliente. Cada proceso de cliente proviene de una conexión de red. Las conexiones de red tienen asociada una biblioteca de red que les permite realizar la conexión.|  
|loginame|**nchar(128)**|Nombre de inicio de sesión.|  
|context_info|**binario (128)**|Datos almacenados en un lote con la instrucción SET CONTEXT_INFO.|  
|sql_handle|**binario (20)**|Representa el objeto o archivo por lotes en ejecución.<br /><br /> **Nota:** Este valor se deriva del lote o la dirección de memoria del objeto. No se calcula mediante el algoritmo hash de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|stmt_start|**int**|Desplazamiento inicial de la instrucción SQL actual para el identificador sql_handle especificado.|  
|stmt_end|**int**|Desplazamiento final de la instrucción SQL actual para el identificador sql_handle especificado.<br /><br /> -1 = La instrucción actual se ejecuta al final de los resultados devueltos por la función fn_get_sql para el identificador sql_handle especificado.|  
|request_id|**int**|IDENTIFICADOR de la solicitud. Se utiliza para identificar solicitudes que se ejecutan en una sesión específica.|
|page_resource |**Binary(8** |**Se aplica a**: [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] <br /><br /> Representación hexadecimal de 8 bytes del recurso de página si la `waitresource` columna contiene una página. |  
  
## <a name="remarks"></a>Notas  
 Si un usuario tiene el permiso VIEW SERVER STATE en el servidor, verá todas las sesiones en ejecución de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; de lo contrario, el usuario solo verá la sesión actual.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica relacionadas con la ejecución &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/execution-related-dynamic-management-views-and-functions-transact-sql.md)   
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
