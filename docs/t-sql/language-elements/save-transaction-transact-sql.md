---
description: SAVE TRANSACTION (Transact-SQL)
title: SAVE TRANSACTION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- SAVE
- SAVE_TSQL
- SAVE_TRANSACTION_TSQL
- SAVE TRANSACTION
dev_langs:
- TSQL
helpviewer_keywords:
- rolling back transactions, SAVE TRANSACTION
- SAVE TRANSACTION statement
- transactions [SQL Server], rolling back
- marking transactions [SQL Server]
- savepoints [SQL Server]
- marked transactions [SQL Server], SAVE TRANSACTION statement
- duplicate savepoints
ms.assetid: b953c3f1-f96d-42f1-95a2-30e314292b35
author: cawrites
ms.author: chadam
ms.openlocfilehash: 28fa4c2ef9ea1c023a6860076477913af2d5a022
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194312"
---
# <a name="save-transaction-transact-sql"></a>SAVE TRANSACTION (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Establece un punto de retorno dentro de una transacción.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  

 ## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
SAVE { TRAN | TRANSACTION } { savepoint_name | @savepoint_variable }  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *savepoint_name*  
 Es el nombre asignado al punto de retorno. Los nombres de los puntos de retorno deben ajustarse a las reglas para los identificadores, aunque están limitados a 32 caracteres. *savepoint_name* siempre distingue mayúsculas de minúsculas, incluso cuando la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no distingue mayúsculas de minúsculas.  
  
 @*savepoint_variable*  
 Es el nombre de una variable definida por el usuario que contiene un nombre de punto de retorno válido. La variable debe declararse con un tipo de datos **char**, **varchar**, **nchar** o **nvarchar**. Es posible pasar más de 32 caracteres a la variable, aunque solo se utilizarán los primeros 32.  
  
## <a name="remarks"></a>Observaciones  
 Un usuario puede establecer un punto de retorno, o marcador, dentro de una transacción. El punto de retorno define una ubicación a la que la transacción puede volver si se cancela parte de la transacción de forma condicional. Si se revierte una transacción hasta un punto de retorno, se debe continuar hasta su finalización con más instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] si es necesario y una instrucción COMMIT TRANSACTION o se debe cancelar completamente al revertir la transacción hasta su inicio. Para cancelar una transacción completa, use el formato ROLLBACK TRANSACTION *transaction_name*. Se deshacen todas las instrucciones o procedimientos de la transacción.  
  
 En una transacción se permiten nombres de puntos de retorno duplicados, pero una instrucción ROLLBACK TRANSACTION que especifique el nombre de un punto de retorno solo revertirá la transacción hasta la instrucción SAVE TRANSACTION más reciente que también utilice ese nombre.  
  
 No se admite SAVE TRANSACTION en transacciones distribuidas iniciadas explícitamente con BEGIN DISTRIBUTED TRANSACTION u originadas a partir de una transacción local.  
  
> [!IMPORTANT]  
>  Una instrucción ROLLBACK TRANSACTION que especifica un savepoint_name libera todos los bloqueos adquiridos más allá del punto de retorno, a excepción de las extensiones y las conversiones. Estos bloqueos no se liberan y no vuelven a convertirse a su modo de bloqueo previo.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestra el uso de un punto de retorno de una transacción para revertir únicamente las modificaciones realizadas por un procedimiento almacenado si una transacción activa se inicia antes de la ejecución del procedimiento almacenado.  
  
```sql  
USE AdventureWorks2012;  
GO  
IF EXISTS (SELECT name FROM sys.objects  
           WHERE name = N'SaveTranExample')  
    DROP PROCEDURE SaveTranExample;  
GO  
CREATE PROCEDURE SaveTranExample  
    @InputCandidateID INT  
AS  
    -- Detect whether the procedure was called  
    -- from an active transaction and save  
    -- that for later use.  
    -- In the procedure, @TranCounter = 0  
    -- means there was no active transaction  
    -- and the procedure started one.  
    -- @TranCounter > 0 means an active  
    -- transaction was started before the   
    -- procedure was called.  
    DECLARE @TranCounter INT;  
    SET @TranCounter = @@TRANCOUNT;  
    IF @TranCounter > 0  
        -- Procedure called when there is  
        -- an active transaction.  
        -- Create a savepoint to be able  
        -- to roll back only the work done  
        -- in the procedure if there is an  
        -- error.  
        SAVE TRANSACTION ProcedureSave;  
    ELSE  
        -- Procedure must start its own  
        -- transaction.  
        BEGIN TRANSACTION;  
    -- Modify database.  
    BEGIN TRY  
        DELETE HumanResources.JobCandidate  
            WHERE JobCandidateID = @InputCandidateID;  
        -- Get here if no errors; must commit  
        -- any transaction started in the  
        -- procedure, but not commit a transaction  
        -- started before the transaction was called.  
        IF @TranCounter = 0  
            -- @TranCounter = 0 means no transaction was  
            -- started before the procedure was called.  
            -- The procedure must commit the transaction  
            -- it started.  
            COMMIT TRANSACTION;  
    END TRY  
    BEGIN CATCH  
        -- An error occurred; must determine  
        -- which type of rollback will roll  
        -- back only the work done in the  
        -- procedure.  
        IF @TranCounter = 0  
            -- Transaction started in procedure.  
            -- Roll back complete transaction.  
            ROLLBACK TRANSACTION;  
        ELSE  
            -- Transaction started before procedure  
            -- called, do not roll back modifications  
            -- made before the procedure was called.  
            IF XACT_STATE() <> -1  
                -- If the transaction is still valid, just  
                -- roll back to the savepoint set at the  
                -- start of the stored procedure.  
                ROLLBACK TRANSACTION ProcedureSave;  
                -- If the transaction is uncommitable, a  
                -- rollback to the savepoint is not allowed  
                -- because the savepoint rollback writes to  
                -- the log. Just return to the caller, which  
                -- should roll back the outer transaction.  
  
        -- After the appropriate rollback, echo error  
        -- information to the caller.  
        DECLARE @ErrorMessage NVARCHAR(4000);  
        DECLARE @ErrorSeverity INT;  
        DECLARE @ErrorState INT;  
  
        SELECT @ErrorMessage = ERROR_MESSAGE();  
        SELECT @ErrorSeverity = ERROR_SEVERITY();  
        SELECT @ErrorState = ERROR_STATE();  
  
        RAISERROR (@ErrorMessage, -- Message text.  
                   @ErrorSeverity, -- Severity.  
                   @ErrorState -- State.  
                   );  
    END CATCH  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [BEGIN TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-transaction-transact-sql.md)   
 [COMMIT TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-transaction-transact-sql.md)   
 [COMMIT WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/commit-work-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [ROLLBACK TRANSACTION &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-transaction-transact-sql.md)   
 [ROLLBACK WORK &#40;Transact-SQL&#41;](../../t-sql/language-elements/rollback-work-transact-sql.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)  
  
  
