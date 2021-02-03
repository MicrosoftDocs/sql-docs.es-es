---
description: WAITFOR (Transact-SQL)
title: WAITFOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- WAITFOR
- WAITFOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- TIME option
- delaying executions [SQL Server]
- batches [SQL Server], execution times
- stored procedures [SQL Server], executing
- DELAY
- time [SQL Server], execution delays
- blocking executions
- transactions [SQL Server], execution times
- WAITFOR statement
- timing executions
ms.assetid: 8e896e73-af27-4cae-a725-7a156733f3bd
author: cawrites
ms.author: chadam
ms.openlocfilehash: 51cd5038d472ae4220d15673036dd46295887543
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211062"
---
# <a name="waitfor-transact-sql"></a>WAITFOR (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Bloquea la ejecución de un lote, un procedimiento almacenado o una transacción hasta alcanzar la hora o el intervalo de tiempo transcurrido, o hasta que una instrucción especificada modifique o devuelva al menos una fila.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
  
WAITFOR   
{  
    DELAY 'time_to_pass'   
  | TIME 'time_to_execute'   
  | [ ( receive_statement ) | ( get_conversation_group_statement ) ]   
    [ , TIMEOUT timeout ]  
}  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 DELAY  
 Es el período de tiempo especificado (hasta un máximo de 24 horas) que debe transcurrir antes de la ejecución de un lote, un procedimiento almacenado o una transacción.  
  
 "*tiempo_que_transcurre*"  
 Es el período de tiempo que hay que esperar. *time_to_pass* se puede especificar en un formato de datos de **datetime** o como una variable local. Las fechas no se pueden especificar, por lo que la parte de la fecha del valor **datetime** no se permite. *time_to_pass* tiene el formato hh:mm[[:ss],mss].
  
 TIME  
 Es la hora especificada a la que se ejecuta el lote, el procedimiento almacenado o la transacción.  
  
 "*hora_de_ejecución*"  
 Es la hora a la que termina la instrucción WAITFOR. *time_to_execute* se puede especificar en un formato de datos de **datetime**, o bien se puede especificar como una variable local. Las fechas no se pueden especificar, por lo que la parte de la fecha del valor **datetime** no se permite. *time_to_execute* tiene el formato hh:mm[[:ss],mss] y, opcionalmente, puede incluir la fecha de 01-01-1900.
  
 *instrucción_receive*  
 Es una instrucción RECEIVE válida.  
  
> [!IMPORTANT]  
>  WAITFOR con una *instrucción_receive* solo es aplicable a los mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener más información, vea [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md).  
  
 *instrucción_get_conversation_group*  
 Es una instrucción GET CONVERSATION GROUP válida.  
  
> [!IMPORTANT]  
>  WAITFOR con una *instrucción_get_conversation_group* solo es aplicable a los mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener más información, vea [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
 TIMEOUT *tiempo_de_espera*  
 Especifica el período de tiempo, en milisegundos, que se espera a que llegue un mensaje en la cola.  
  
> [!IMPORTANT]  
>  Solo se puede especificar WAITFOR con TIMEOUT en mensajes de [!INCLUDE[ssSB](../../includes/sssb-md.md)]. Para obtener más información, vea [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md) y [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md).  
  
## <a name="remarks"></a>Comentarios  
 La transacción se está ejecutando mientras se ejecuta la instrucción WAITFOR y no se puede ejecutar ninguna otra solicitud para la misma transacción.  
  
 El retraso de tiempo real puede variar en relación con el tiempo especificado en *time_to_pass*, *time_to_execute* o *timeout*, y depende del nivel de actividad del servidor. El contador de tiempo se inicia cuando se programa el subproceso de la declaración WAITFOR. Si el servidor está ocupado, es posible que no se programe el subproceso inmediatamente; por tanto, el retraso de tiempo puede ser mayor que el tiempo especificado.  
  
 WAITFOR no cambia la semántica de una consulta. Si una consulta no devuelve ninguna fila, WAITFOR esperará indefinidamente o hasta que se alcance el valor de TIMEOUT, si se especifica.  
  
 No se pueden abrir cursores en las instrucciones WAITFOR.  
  
 No se pueden definir vistas en las instrucciones WAITFOR.  
  
 Cuando una consulta supera la opción query wait, se puede completar el argumento de la instrucción WAITFOR sin ejecutarse. Para obtener más información sobre la opción de configuración, vea [Establecer la opción de configuración del servidor Espera de consulta](../../database-engine/configure-windows/configure-the-query-wait-server-configuration-option.md). Para ver los procesos activos y en espera, use [sp_who](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md).  
  
 Cada instrucción WAITFOR tiene un subproceso asociado. Si se especifica un gran número de instrucciones WAITFOR en el mismo servidor, se pueden acumular muchos subprocesos a la espera de que se ejecuten estas instrucciones. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] supervisa el número de subprocesos asociados a las instrucciones WAITFOR y selecciona aleatoriamente algunos de estos subprocesos para salir si el servidor empieza a experimentar un colapso de subprocesos.  
  
 Para crear un interbloqueo, ejecute una consulta con WAITFOR en una transacción que también tenga bloqueos que impidan realizar cambios en el conjunto de filas al que intenta acceder la instrucción WAITFOR. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] identifica estos escenarios y devuelve un conjunto de resultados vacío si existe la posibilidad de un interbloqueo de este tipo.  
  
> [!CAUTION]  
>  La inclusión de WAITFOR ralentizará la finalización del proceso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y puede provocar un mensaje que indique que se ha superado el tiempo de espera en la aplicación. Si es necesario, ajuste el valor de tiempo de espera de la conexión en el nivel de aplicación.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-waitfor-time"></a>A. Usar WAITFOR TIME  
 En el ejemplo siguiente se ejecuta el procedimiento almacenado `sp_update_job` en la base de datos msdb a las 10:20 p.m. (`22:20`).  
  
```sql  
EXECUTE sp_add_job @job_name = 'TestJob';  
BEGIN  
    WAITFOR TIME '22:20';  
    EXECUTE sp_update_job @job_name = 'TestJob',  
        @new_name = 'UpdatedJob';  
END;  
GO  
```  
  
### <a name="b-using-waitfor-delay"></a>B. Usar WAITFOR DELAY  
 En el ejemplo siguiente se ejecuta el procedimiento almacenado después de un retardo de dos horas.  
  
```sql  
BEGIN  
    WAITFOR DELAY '02:00';  
    EXECUTE sp_helpdb;  
END;  
GO  
```  
  
### <a name="c-using-waitfor-delay-with-a-local-variable"></a>C. Usar WAITFOR DELAY con una variable local  
 En el ejemplo siguiente se muestra cómo se puede utilizar una variable local con la opción `WAITFOR DELAY`. Este procedimiento almacenado espera durante un período de tiempo variable y, después, devuelve información al usuario como el número transcurrido de horas, minutos y segundos.  
  
```sql  
IF OBJECT_ID('dbo.TimeDelay_hh_mm_ss','P') IS NOT NULL  
    DROP PROCEDURE dbo.TimeDelay_hh_mm_ss;  
GO  
CREATE PROCEDURE dbo.TimeDelay_hh_mm_ss   
    (  
    @DelayLength char(8)= '00:00:00'  
    )  
AS  
DECLARE @ReturnInfo VARCHAR(255)  
IF ISDATE('2000-01-01 ' + @DelayLength + '.000') = 0  
    BEGIN  
        SELECT @ReturnInfo = 'Invalid time ' + @DelayLength   
        + ',hh:mm:ss, submitted.';  
        -- This PRINT statement is for testing, not use in production.  
        PRINT @ReturnInfo   
        RETURN(1)  
    END  
BEGIN  
    WAITFOR DELAY @DelayLength  
    SELECT @ReturnInfo = 'A total time of ' + @DelayLength + ',   
        hh:mm:ss, has elapsed! Your time is up.'  
    -- This PRINT statement is for testing, not use in production.  
    PRINT @ReturnInfo;  
END;  
GO  
/* This statement executes the dbo.TimeDelay_hh_mm_ss procedure. */  
EXEC TimeDelay_hh_mm_ss '00:00:10';  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 `A total time of 00:00:10, in hh:mm:ss, has elapsed. Your time is up.`  
  
## <a name="see-also"></a>Vea también  
 [Lenguaje de control de flujo &#40;Transact-SQL&#41;](~/t-sql/language-elements/control-of-flow.md)   
 [datetime &#40;Transact-SQL&#41;](../../t-sql/data-types/datetime-transact-sql.md)   
 [sp_who &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-who-transact-sql.md)  
  
