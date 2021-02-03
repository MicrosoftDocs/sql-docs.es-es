---
description: THROW (Transact-SQL)
title: THROW (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- THROW_TSQL
- THROW
dev_langs:
- TSQL
helpviewer_keywords:
- THROW statement
ms.assetid: 43661b89-8f13-4480-ad53-70306cbb14c5
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a9be1b4243009bbc4ca99fe9bdf9bfcd94d72c45
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182315"
---
# <a name="throw-transact-sql"></a>THROW (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Genera una excepción y transfiere la ejecución a un bloque CATCH de una construcción TRY…CATCH en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
THROW [ { error_number | @local_variable },  
        { message | @local_variable },  
        { state | @local_variable } ]   
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *error_number*  
 Es una constante o una variable que representan la excepción. *error_number* es de tipo **int** y debe ser mayor o igual que 50000 y menor o igual que 2147483647.  
  
 *message*  
 Es una cadena o una variable que describe la excepción. *message* es **nvarchar (2048)** .  
  
 *state*  
 Es una constante o una variable comprendida entre 0 y 255 que indica el estado que se ha de asociar al mensaje. *state* es **tinyint**.  
  
## <a name="remarks"></a>Observaciones  
 La instrucción anterior a la instrucción THROW debe ir seguida del terminador de instrucción punto y coma (;).  
  
 Si una construcción TRY…CATCH no está disponible, el lote de instrucciones se finaliza. Se establecen el número de línea y el procedimiento donde se produce la excepción. La gravedad se establece en 16.  
  
 Si la instrucción THROW se especifica sin parámetros, debe aparecer dentro de un bloque CATCH. Esto hace que se produzca la excepción detectada. Cualquier error que se produzca en una instrucción THROW hace que el lote de instrucciones se finalice.  
  
 % es un carácter reservado en el texto del mensaje de una instrucción THROW y debe ser de escape. Duplique el carácter % para devolver % como parte del texto del mensaje, por ejemplo "el aumento supera un 15 %% el valor original".  
  
## <a name="differences-between-raiserror-and-throw"></a>Diferencias entre RAISERROR y THROW  
 En la siguiente tabla se enumeran algunas diferencias entre las instrucciones THROW y RAISERROR.  
  
|RAISERROR, instrucción|Instrucción THROW|  
|-------------------------|---------------------|  
|Si se pasa *msg_id* a RAISERROR, el identificador se debe definir en sys.messages.|El parámetro *error_number* no tiene que definirse en sys.messages.|  
|El parámetro *msg_str* puede contener estilos de formato de **printf**.|El parámetro *message* no acepta el formato de estilo de **printf**.|  
|El parámetro *severity* especifica la gravedad de la excepción.|No hay ningún parámetro *severity*. Cuando THROW se usa para iniciar la excepción, la gravedad siempre se establece en 16, pero cuando se usa para volver a producir una excepción existente, la gravedad se establecerá en el nivel de gravedad de la excepción en cuestión.|  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-throw-to-raise-an-exception"></a>A. Usar THROW para generar una excepción  
 En el siguiente ejemplo se muestra cómo utilizar la instrucción `THROW` para producir una excepción.  
  
```sql  
THROW 51000, 'The record does not exist.', 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 51000, Level 16, State 1, Line 1  
  
 The record does not exist.
 ```  
  
### <a name="b-using-throw-to-raise-an-exception-again"></a>B. Usar THROW para generar de nuevo una excepción  
 El siguiente ejemplo muestra cómo utilizar la instrucción `THROW` para generar de nuevo la última excepción THROWN.  
  
```sql  
USE tempdb;  
GO  
CREATE TABLE dbo.TestRethrow  
(    ID INT PRIMARY KEY  
);  
BEGIN TRY  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
--  Force error 2627, Violation of PRIMARY KEY constraint to be raised.  
    INSERT dbo.TestRethrow(ID) VALUES(1);  
END TRY  
BEGIN CATCH  
  
    PRINT 'In catch block.';  
    THROW;  
END CATCH;  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 In catch block. 
 Msg 2627, Level 14, State 1, Line 1  
 Violation of PRIMARY KEY constraint 'PK__TestReth__3214EC272E3BD7D3'. Cannot insert duplicate key in object 'dbo.TestRethrow'.  
 The statement has been terminated.
 ```  
  
### <a name="c-using-formatmessage-with-throw"></a>C. Usar FORMATMESSAGE con THROW  
 En el ejemplo siguiente se muestra cómo usar la función `FORMATMESSAGE` con `THROW` para producir un mensaje de error personalizado. En el ejemplo se crea primero un mensaje de error definido por el usuario mediante `sp_addmessage`. Como la instrucción THROW no permite parámetros de sustitución en el parámetro *message* de la manera en que lo hace RAISERROR, la función FORMATMESSAGE se emplea para pasar los tres valores de parámetro esperados por el mensaje de error 60000.  
  
```sql  
EXEC sys.sp_addmessage  
     @msgnum   = 60000  
    ,@severity = 16  
    ,@msgtext  = N'This is a test message with one numeric parameter (%d), one string parameter (%s), and another string parameter (%s).'  
    ,@lang = 'us_english';   
GO  
  
DECLARE @msg NVARCHAR(2048) = FORMATMESSAGE(60000, 500, N'First string', N'second string');   
  
THROW 60000, @msg, 1;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Msg 60000, Level 16, State 1, Line 2  
 This is a test message with one numeric parameter (500), one string parameter (First string), and another string parameter (second string).
 ```  
  
## <a name="see-also"></a>Consulte también  
 [FORMATMESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/formatmessage-transact-sql.md)   
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
  
  

