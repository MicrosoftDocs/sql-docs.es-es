---
description: sp_who (Transact-SQL)
title: sp_who (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_who_TSQL
- sp_who
dev_langs:
- TSQL
helpviewer_keywords:
- sp_who
ms.assetid: 132dfb08-fa79-422e-97d4-b2c4579c6ac5
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: db60af487d1ee7b7475b0b4d6ec1d8f67decb1ca
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201801"
---
# <a name="sp_who-transact-sql"></a>sp_who (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información sobre los usuarios, las sesiones y los procesos actuales en una instancia de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] . La información se puede filtrar para obtener solo los procesos que están activos, que pertenecen a un usuario específico o que pertenecen a una sesión específica.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_who [ [ @loginame = ] 'login' | session ID | 'ACTIVE' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @loginame = ] 'login' | session ID | 'ACTIVE'` Se utiliza para filtrar el conjunto de resultados.  
  
 *login* es de **tipo sysname** que identifica los procesos que pertenecen a un inicio de sesión determinado.  
  
 el ID. de *sesión* es un número de identificación de sesión que pertenece a la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instancia. el ID. de *sesión* es **smallint**.  
  
 **Active** excluye las sesiones que están esperando el siguiente comando del usuario.  
  
 Si no se indica ningún valor, el procedimiento muestra todas las sesiones que pertenecen a la instancia.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o 1 (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_who** devuelve un conjunto de resultados con la siguiente información.  
  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|**spid**|**smallint**|Id. de sesión.|  
|**ECID**|**smallint**|Id. de contexto de ejecución de un subproceso determinado, asociado con un Id. de sesión específico.<br /><br /> ECID = {0, 1, 2, 3,... *n*}, donde 0 siempre representa el subproceso principal o primario, y {1, 2, 3,... *n*} representan los subprocesos.|  
|**status**|**NCHAR (30)**|Estado del proceso. Los valores posibles son:<br /><br /> **inactivo**. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está restableciendo la sesión.<br /><br /> en **ejecución**. La sesión está ejecutando uno o varios lotes. Si Conjuntos de resultados activos múltiples (MARS) está habilitado, una sesión puede ejecutar varios lotes. Para obtener más información, vea [Usar conjuntos de resultados activos múltiples &#40;MARS&#41;](../../relational-databases/native-client/features/using-multiple-active-result-sets-mars.md).<br /><br /> **en segundo plano**. La sesión está ejecutando una tarea en segundo plano, como una detección de interbloqueos.<br /><br /> **revertir**. La sesión está realizando una reversión de una transacción.<br /><br /> **pendiente**. La sesión está esperando que un subproceso de trabajo esté disponible.<br /><br /> **ejecutable**. La tarea de la sesión está en la cola de ejecutables de un programador mientras espera obtener un cuanto de tiempo.<br /><br /> **spinloop**. La tarea de la sesión está esperando que se libere un bloqueo por bucle.<br /><br /> **suspendido**. La sesión está esperando a que finalice un evento, como una entrada o salida.|  
|**loginame**|**nchar(128)**|Nombre de inicio de sesión asociado al proceso específico.|  
|**hostname**|**nchar(128)**|Nombre del host o equipo de cada proceso.|  
|**blk**|**Char (5)**|Id. de sesión del proceso de bloqueo, si existe. De lo contrario, esta columna tiene el valor cero.<br /><br /> Cuando una transacción huérfana distribuida bloquea una transacción asociada con un Id. de sesión determinado, esta columna devolverá '-2' para la transacción huérfana de bloqueo.|  
|**nombrebd**|**nchar(128)**|Base de datos utilizada por el proceso.|  
|**cmd**|**nchar(16)**|Comando de [!INCLUDE[ssDE](../../includes/ssde-md.md)] (instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)], proceso de [!INCLUDE[ssDE](../../includes/ssde-md.md)] interno, etc.) que se ejecuta para el proceso. En SQL Server 2019, el tipo de datos ha cambiado a **NCHAR (26)**.|  
|**id_de_solicitud**|**int**|Id. de las solicitudes que se ejecutan en una sesión específica.|  
  
 En el caso de procesamiento paralelo, se crean subprocesos secundarios para el identificador de sesión específico. El subproceso principal se indica como `spid = <xxx>` y `ecid =0`. Los demás subprocesos tienen el mismo `spid = <xxx>` , pero con **ECID** > 0.  
  
## <a name="remarks"></a>Observaciones  
 Un proceso de bloqueo, que puede tener un bloqueo exclusivo, es el que contiene recursos que otro proceso necesita.  
  
 A todas las transacciones distribuidas huérfanas se les asigna como identificador de sesión el valor '-2'. Las transacciones distribuidas huérfanas son transacciones distribuidas que no están asociadas con ningún identificador de sesión. Para obtener más información, vea [Usar transacciones marcadas para recuperar bases de datos relacionadas sistemáticamente &#40;modelo de recuperación completa&#41;](../../relational-databases/backup-restore/use-marked-transactions-to-recover-related-databases-consistently.md).  
  
 Consulte la columna **is_user_process** de sys.dm_exec_sessions para separar los procesos del sistema de los procesos de usuario.  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso VIEW SERVER STATE en el servidor para ver todas las sesiones en ejecución en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. De lo contrario, el usuario solo ve la sesión actual.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-listing-all-current-processes"></a>A. Mostrar la lista de todos los procesos actuales  
 En el ejemplo siguiente se utiliza `sp_who` sin parámetros para informar de todos los usuarios actuales.  
  
```  
USE master;  
GO  
EXEC sp_who;  
GO  
```  
  
### <a name="b-listing-a-specific-users-process"></a>B. Mostrar un proceso de un usuario específico  
 En el ejemplo siguiente se muestra cómo ver información acerca de un usuario actual a partir de su nombre de inicio de sesión.  
  
```  
USE master;  
GO  
EXEC sp_who 'janetl';  
GO  
```  
  
### <a name="c-displaying-all-active-processes"></a>C. Mostrar todos los procesos activos  
  
```  
USE master;  
GO  
EXEC sp_who 'active';  
GO  
```  
  
### <a name="d-displaying-a-specific-process-identified-by-a-session-id"></a>D. Mostrar un proceso específico identificado mediante un Id. de sesión  
  
```  
USE master;  
GO  
EXEC sp_who '10' --specifies the process_id;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_lock &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-lock-transact-sql.md)   
 [sys.sysprocesos &#40;Transact-SQL&#41;](../../relational-databases/system-compatibility-views/sys-sysprocesses-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
