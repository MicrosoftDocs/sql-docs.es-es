---
description: TRY...CATCH (Transact-SQL)
title: TRY...CATCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- BEGIN_TRY_TSQL
- BEGIN_CATCH_TSQL
- TRY
- BEGIN TRY
- TRY_TSQL
- BEGIN CATCH
dev_langs:
- TSQL
helpviewer_keywords:
- BEGIN CATCH statement
- uncommittable transactions
- errors [SQL Server], TRY...CATCH
- TRY block [SQL Server]
- XACT_STATE function
- TRY...CATCH [SQL Server]
- BEGIN TRY statement
- CATCH block
ms.assetid: 248df62a-7334-4bca-8262-235a28f4b07f
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 7e694b4366ac0b0382fb9a29f155bc7fe346653b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176382"
---
# <a name="trycatch-transact-sql"></a>TRY...CATCH (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]


  Implementa un mecanismo de control de errores para [!INCLUDE[tsql](../../includes/tsql-md.md)] que es similar al control de excepciones en los lenguajes [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# y [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C++. Se puede incluir un grupo de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] en un bloque TRY. Si se produce un error en el bloque TRY, el control se transfiere a otro grupo de instrucciones que está incluido en un bloque CATCH.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
BEGIN TRY  
     { sql_statement | statement_block }  
END TRY  
BEGIN CATCH  
     [ { sql_statement | statement_block } ]  
END CATCH  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *sql_statement*  
 Es cualquier instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 *statement_block*  
 Grupo de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] incluidas en un lote o en un bloque BEGIN…END.  
  
## <a name="remarks"></a>Observaciones  
 Una construcción TRY…CATCH detecta todos los errores de ejecución que tienen una gravedad mayor de 10 y que no cierran la conexión de la base de datos.  
  
 Un bloque TRY debe ir seguido inmediatamente por un bloque CATCH asociado. Si se incluye cualquier otra instrucción entre las instrucciones END TRY y BEGIN CATCH se genera un error de sintaxis.  
  
 Una construcción TRY…CATCH no puede abarcar varios lotes. Una construcción TRY…CATCH no puede abarcar varios bloques de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)]. Por ejemplo, una construcción TRY…CATCH no puede abarcar dos bloques BEGIN…END de instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)], ni puede abarcar una construcción IF…ELSE.  
  
 Si no hay errores en el código incluido en un bloque TRY, cuando la última instrucción de este bloque ha terminado de ejecutarse, el control se transfiere a la instrucción inmediatamente posterior a la instrucción END CATCH asociada.
 
 Si hay un error en el código incluido en un bloque TRY, el control se transfiere a la primera instrucción del bloque CATCH asociado. Cuando finaliza el código del bloque CATCH, el control se transfiere a la instrucción inmediatamente posterior a la instrucción END CATCH. 
 
 > [!NOTE] 
 > Si la instrucción END CATCH es la última instrucción de un procedimiento almacenado o desencadenador, el control se devuelve a la instrucción que llamó al procedimiento almacenado o activó el desencadenador. 
 
 Los errores capturados por un bloque CATCH no se devuelven a la aplicación que realiza la llamada. Si es necesario devolver cualquier parte de la información sobre el error a la aplicación, debe hacerlo el código del bloque CATCH a través de mecanismos como los conjuntos de resultados SELECT o las instrucciones RAISERROR y PRINT.  
  
 Las construcciones TRY…CATCH pueden estar anidadas. Un bloque TRY o un bloque CATCH puede contener construcciones TRY…CATCH anidadas. Por ejemplo, un bloque CATCH puede contener una construcción TRY…CATCH insertada para controlar los errores detectados por el código de CATCH.  
  
 Los errores que se encuentren en un bloque CATCH se tratan como los errores generados en otros lugares. Si el bloque CATCH contiene una construcción TRY…CATCH anidada, los errores del bloque TRY anidado transferirán el control al bloque CATCH anidado. Si no hay ninguna construcción TRY…CATCH anidada, el error se devuelve al autor de la llamada.  
  
 Las construcciones TRY…CATCH capturan los errores no controlados de los procedimientos almacenados o desencadenadores ejecutados por el código del bloque TRY. Como alternativa, los procedimientos almacenados o desencadenadores pueden contener sus propias construcciones TRY…CATCH para controlar los errores generados por su código. Por ejemplo, cuando un bloque TRY ejecuta un procedimiento almacenado y se produce un error en éste, el error se puede controlar de las formas siguientes:  
  
-   Si el procedimiento almacenado no contiene su propia construcción TRY…CATCH, el error devuelve el control al bloque CATCH asociado al bloque TRY que contiene la instrucción EXECUTE.  
  
-   Si el procedimiento almacenado contiene una construcción TRY…CATCH, el error transfiere el control al bloque CATCH del procedimiento almacenado. Cuando finaliza el código del bloque CATCH, el control se devuelve a la instrucción inmediatamente posterior a la instrucción EXECUTE que llamó al procedimiento almacenado.  
  
 No se pueden utilizar instrucciones GOTO para entrar en un bloque TRY o CATCH. Estas instrucciones se pueden utilizar para saltar a una etiqueta dentro del mismo bloque TRY o CATCH, o bien para salir de un bloque TRY o CATCH.  
  
 La construcción TRY…CATCH no se puede usar en una función definida por el usuario.  
  
## <a name="retrieving-error-information"></a>Recuperar información sobre errores  
 En el ámbito de un bloque CATCH, se pueden utilizar las siguientes funciones del sistema para obtener información acerca del error que provocó la ejecución del bloque CATCH:  
  
-   [ERROR_NUMBER()](../../t-sql/functions/error-number-transact-sql.md) devuelve el número del error.  
  
-   [ERROR_SEVERITY()](../../t-sql/functions/error-severity-transact-sql.md) devuelve la gravedad.  
  
-   [ERROR_STATE()](../../t-sql/functions/error-state-transact-sql.md) devuelve el número de estado del error.  
  
-   [ERROR_PROCEDURE()](../../t-sql/functions/error-procedure-transact-sql.md) devuelve el nombre del procedimiento almacenado o desencadenador donde se produjo el error.  
  
-   [ERROR_LINE()](../../t-sql/functions/error-line-transact-sql.md) devuelve el número de línea de la rutina que provocó el error.  
  
-   [ERROR_MESSAGE()](../../t-sql/functions/error-message-transact-sql.md) devuelve el texto completo del mensaje de error. El texto incluye los valores proporcionados para los parámetros sustituibles, como las longitudes, nombres de objeto o tiempos.  
  
Estas funciones devuelven NULL si se las llama desde fuera del ámbito del bloque CATCH. Con ellas se puede recuperar información sobre los errores desde cualquier lugar dentro del ámbito del bloque CATCH. Por ejemplo, en el siguiente script se muestra un procedimiento almacenado que contiene funciones de control de errores. Se llama al procedimiento almacenado en el bloque `CATCH` de una construcción `TRY...CATCH` y se devuelve información sobre el error.  
  
```sql  
-- Verify that the stored procedure does not already exist.  
IF OBJECT_ID ( 'usp_GetErrorInfo', 'P' ) IS NOT NULL   
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
SELECT  
    ERROR_NUMBER() AS ErrorNumber  
    ,ERROR_SEVERITY() AS ErrorSeverity  
    ,ERROR_STATE() AS ErrorState  
    ,ERROR_PROCEDURE() AS ErrorProcedure  
    ,ERROR_LINE() AS ErrorLine  
    ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
BEGIN TRY  
    -- Generate divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
END CATCH;   
```  
  
 La función ERROR\_\* funciona también en un bloque `CATCH` de un [procedimiento almacenado compilado de forma nativa](../../relational-databases/in-memory-oltp/a-guide-to-query-processing-for-memory-optimized-tables.md).  
  
## <a name="errors-unaffected-by-a-trycatch-construct"></a>Errores no afectados por una construcción TRY…CATCH  
 Las construcciones TRY…CATCH no detectan las condiciones siguientes:  
  
-   Advertencias o mensajes informativos que tienen una gravedad 10 o inferior.  
  
-   Errores que tienen la gravedad 20 o superior que detienen el procesamiento de las tareas de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] en la sesión. Si se produce un error con una gravedad 20 o superior y no se interrumpe la conexión de la base de datos, TRY…CATCH controlará el error.  
  
-   Atenciones, como solicitudes de interrupción de clientes o conexiones de cliente interrumpidas.  
  
-   Cuando el administrador del sistema finaliza la sesión mediante la instrucción KILL.  
  
Un bloque CATCH no controla los siguientes tipos de errores cuando se producen en el mismo nivel de ejecución que la construcción TRY…CATCH:  
  
-   Errores de compilación, como errores de sintaxis, que impiden la ejecución de un lote.  
  
-   Errores que se producen durante la recompilación de instrucciones, como errores de resolución de nombres de objeto que se producen después de la compilación debido a una resolución de nombres diferida.  
-   Errores de resolución de nombres de objeto   

  
Estos errores se devuelven al nivel de ejecución del lote, procedimiento almacenado o desencadenador.  
  
Si se produce un error durante la compilación o la recompilación de nivel de instrucción en un nivel de ejecución inferior (por ejemplo, al ejecutar sp_executesql o un procedimiento almacenado definido por el usuario) dentro del bloque TRY, el error se producirá en un nivel inferior a la construcción TRY…CATCH y lo controlará el bloque CATCH asociado.  
  
En el ejemplo siguiente se muestra cómo la construcción `SELECT` no captura un error de resolución de nombre de objeto generado por una instrucción `TRY...CATCH`, sino que es el bloque `CATCH` el que lo captura cuando la misma instrucción `SELECT` se ejecuta dentro de un procedimiento almacenado.  
  
```sql  
BEGIN TRY  
    -- Table does not exist; object name resolution  
    -- error not caught.  
    SELECT * FROM NonexistentTable;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
       ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH  
```  
  
 El error no se captura y el control se transfiere fuera de la construcción `TRY...CATCH`, al siguiente nivel superior.  
  
 Al ejecutar la instrucción `SELECT` dentro de un procedimiento almacenado, el error se produce en un nivel inferior al bloque `TRY`. La construcción `TRY...CATCH` controlará el error.  
  
```sql  
-- Verify that the stored procedure does not exist.  
IF OBJECT_ID ( N'usp_ExampleProc', N'P' ) IS NOT NULL   
    DROP PROCEDURE usp_ExampleProc;  
GO  
  
-- Create a stored procedure that will cause an   
-- object resolution error.  
CREATE PROCEDURE usp_ExampleProc  
AS  
    SELECT * FROM NonexistentTable;  
GO  
  
BEGIN TRY  
    EXECUTE usp_ExampleProc;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
```  
  
## <a name="uncommittable-transactions-and-xact_state"></a>Transacciones no confirmables y XACT_STATE  
 Si un error generado en un bloque TRY hace que se invalide el estado de la transacción actual, la transacción se clasifica como una transacción no confirmable. Un error que normalmente termina una transacción fuera de un bloque TRY hace que la transacción no confirmable cuando se produce dentro de un bloque TRY. Una transacción no confirmable solo puede realizar operaciones de lectura o ROLLBACK TRANSACTION. La transacción no puede ejecutar ninguna instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] que genere una operación de escritura o COMMIT TRANSACTION. La función XACT_STATE devuelve el valor -1 si una transacción se ha clasificado como transacción no confirmable. Cuando finaliza el lote, [!INCLUDE[ssDE](../../includes/ssde-md.md)] revierte todas las transacciones activas no confirmables. Si no se envió ningún mensaje de error cuando la transacción entró en un estado no confirmable, cuando finalice la ejecución del lote se enviará un mensaje de error a la aplicación cliente. Esto indica que se ha detectado y revertido una transacción no confirmable.  
  
 Para más información sobre las transacciones no confirmables y la función XACT_STATE, vea [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-trycatch"></a>A. Uso de TRY…CATCH  
 En el siguiente ejemplo se muestra una instrucción `SELECT` que generará un error de división por cero. El error hace que la ejecución salte al bloque `CATCH` asociado.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
### <a name="b-using-trycatch-in-a-transaction"></a>B. Uso de TRY…CATCH en una transacción  
 En este ejemplo se muestra cómo funciona un bloque `TRY...CATCH` dentro de una transacción. La instrucción del bloque `TRY` genera un error por infracción de restricción.  
  
```sql  
BEGIN TRANSACTION;  
  
BEGIN TRY  
    -- Generate a constraint violation error.  
    DELETE FROM Production.Product  
    WHERE ProductID = 980;  
END TRY  
BEGIN CATCH  
    SELECT   
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_LINE() AS ErrorLine  
        ,ERROR_MESSAGE() AS ErrorMessage;  
  
    IF @@TRANCOUNT > 0  
        ROLLBACK TRANSACTION;  
END CATCH;  
  
IF @@TRANCOUNT > 0  
    COMMIT TRANSACTION;  
GO  
```  
  
### <a name="c-using-trycatch-with-xact_state"></a>C. Uso de TRY…CATCH con XACT_STATE  
 En este ejemplo se muestra cómo utilizar la construcción `TRY...CATCH` para controlar los errores que se producen en una transacción. La función `XACT_STATE` determina si la transacción debe confirmarse o revertirse. En este ejemplo `SET XACT_ABORT` es `ON`. Esto hace que la transacción sea no confirmable cuando se produce el error por infracción de restricción.  
  
```sql  
-- Check to see whether this stored procedure exists.  
IF OBJECT_ID (N'usp_GetErrorInfo', N'P') IS NOT NULL  
    DROP PROCEDURE usp_GetErrorInfo;  
GO  
  
-- Create procedure to retrieve error information.  
CREATE PROCEDURE usp_GetErrorInfo  
AS  
    SELECT   
         ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_LINE () AS ErrorLine  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
GO  
  
-- SET XACT_ABORT ON will cause the transaction to be uncommittable  
-- when the constraint violation occurs.   
SET XACT_ABORT ON;  
  
BEGIN TRY  
    BEGIN TRANSACTION;  
        -- A FOREIGN KEY constraint exists on this table. This   
        -- statement will generate a constraint violation error.  
        DELETE FROM Production.Product  
            WHERE ProductID = 980;  
  
    -- If the DELETE statement succeeds, commit the transaction.  
    COMMIT TRANSACTION;  
END TRY  
BEGIN CATCH  
    -- Execute error retrieval routine.  
    EXECUTE usp_GetErrorInfo;  
  
    -- Test XACT_STATE:  
        -- If 1, the transaction is committable.  
        -- If -1, the transaction is uncommittable and should   
        --     be rolled back.  
        -- XACT_STATE = 0 means that there is no transaction and  
        --     a commit or rollback operation would generate an error.  
  
    -- Test whether the transaction is uncommittable.  
    IF (XACT_STATE()) = -1  
    BEGIN  
        PRINT  
            N'The transaction is in an uncommittable state.' +  
            'Rolling back transaction.'  
        ROLLBACK TRANSACTION;  
    END;  
  
    -- Test whether the transaction is committable.
    -- You may want to commit a transaction in a catch block if you want to commit changes to statements that ran prior to the error.
    IF (XACT_STATE()) = 1  
    BEGIN  
        PRINT  
            N'The transaction is committable.' +  
            'Committing transaction.'  
        COMMIT TRANSACTION;     
    END;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-using-trycatch"></a>D. Uso de TRY…CATCH  
 En el siguiente ejemplo se muestra una instrucción `SELECT` que generará un error de división por cero. El error hace que la ejecución salte al bloque `CATCH` asociado.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber  
        ,ERROR_SEVERITY() AS ErrorSeverity  
        ,ERROR_STATE() AS ErrorState  
        ,ERROR_PROCEDURE() AS ErrorProcedure  
        ,ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)   
 [Niveles de gravedad de error del motor de base de datos](../../relational-databases/errors-events/database-engine-error-severities.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [ERROR_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/error-state-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)   
 [GOTO &#40;Transact-SQL&#41;](../../t-sql/language-elements/goto-transact-sql.md)   
 [BEGIN...END &#40;Transact-SQL&#41;](../../t-sql/language-elements/begin-end-transact-sql.md)   
 [XACT_STATE &#40;Transact-SQL&#41;](../../t-sql/functions/xact-state-transact-sql.md)   
 [SET XACT_ABORT &#40;Transact-SQL&#41;](../../t-sql/statements/set-xact-abort-transact-sql.md)  
  
