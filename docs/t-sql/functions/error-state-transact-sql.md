---
description: ERROR_STATE (Transact-SQL)
title: ERROR_STATE (Transact-SQL)
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ERROR_STATE_TSQL
- ERROR_STATE
dev_langs:
- TSQL
helpviewer_keywords:
- messages [SQL Server], state
- ERROR_STATE function
- errors [SQL Server], state
- TRY...CATCH [SQL Server]
- CATCH block
- states [SQL Server], error numbers
ms.assetid: 6059af00-83fe-409f-ab7c-daad111bc671
author: cawrites
ms.author: chadam
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 9cef172dff8e103a8ece7a8d704cdd7b009c08d4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99158747"
---
# <a name="error_state-transact-sql"></a>ERROR_STATE (Transact-SQL)

[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Devuelve el número de estado del error que provocó que se ejecutara el bloque CATCH de una construcción TRY…CATCH.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
ERROR_STATE ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devuelto
 **int**  
  
## <a name="return-value"></a>Valor devuelto  
 Cuando se llama en un bloque CATCH, devuelve el número de estado del mensaje de error que provocó que el bloque CATCH se ejecutara.  
  
 Devuelve NULL si se le llama desde fuera del ámbito del bloque CATCH.  
  
## <a name="remarks"></a>Observaciones  
 Algunos mensajes de error se pueden generar en varios puntos del código de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Por ejemplo, el error "1105" se puede generar bajo diferentes condiciones. Cada condición específica que genera el error asigna un código de estado único.  
  
 Cuando se ven las bases de datos de problemas conocidos, como [!INCLUDE[msCoName](../../includes/msconame-md.md)] Knowledge Base, puede utilizar el número de estado para determinar si el problema registrado puede ser el mismo que el error que ha encontrado. Por ejemplo, si un artículo de Knowledge Base trata un mensaje de error 1105 con un estado de 2 y el mensaje de error 1105 que recibió tiene un estado de 3, probablemente su error tiene un origen distinto del registrado en el artículo.  
  
 Un ingeniero de soporte técnico de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] también puede utilizar el código de estado de un error para encontrar la ubicación en el código de origen donde se ha producido ese error, que puede proporcionar ideas adicionales sobre cómo diagnosticar el problema.  
  
 ERROR_STATE puede llamarse en cualquier lugar del ámbito de un bloque CATCH.  
  
 ERROR_STATE devuelve el estado de error independientemente de las veces que se ejecute o de dónde se ejecute en el ámbito del bloque CATCH. Esto contrasta con funciones como @@ERROR, que solo devuelve el número de error en la instrucción inmediatamente posterior a la que causa un error, o en la primera instrucción de un bloque CATCH.  
  
 En los bloques CATCH anidados, ERROR_STATE devuelve el estado de error específico del ámbito del bloque CATCH en el que se hace referencia al mismo. Por ejemplo, el bloque CATCH de una construcción TRY...CATCH externa podría tener una construcción TRY...CATCH anidada. En el bloque CATCH anidado, ERROR_STATE devuelve el estado del error que invocó el bloque CATCH anidado. Si ERROR_STATE se ejecuta en el bloque CATCH externo, devuelve el estado del error que invocó ese bloque CATCH.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-error_state-in-a-catch-block"></a>A. Utilizar ERROR_STATE en un bloque CATCH  
 En este ejemplo de código se muestra una instrucción `SELECT` que genera un error de división por cero. Se devuelve el estado del error.  
  
```sql  
BEGIN TRY  
    -- Generate a divide by zero error  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT ERROR_STATE() AS ErrorState;  
END CATCH;  
GO  
```  
  
### <a name="b-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>B. Utilizar ERROR_STATE en un bloque CATCH con otras herramientas de control de errores  
 En este ejemplo de código se muestra una instrucción `SELECT` que genera un error de división por cero. Además del estado de error, se devuelve otra información relacionada con el error.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_LINE() AS ErrorLine,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="c-using-error_state-in-a-catch-block-with-other-error-handling-tools"></a>C. Utilizar ERROR_STATE en un bloque CATCH con otras herramientas de control de errores  
 En este ejemplo de código se muestra una instrucción `SELECT` que genera un error de división por cero. Además del estado de error, se devuelve otra información relacionada con el error.  
  
```sql  
BEGIN TRY  
    -- Generate a divide-by-zero error.  
    SELECT 1/0;  
END TRY  
BEGIN CATCH  
    SELECT  
        ERROR_NUMBER() AS ErrorNumber,  
        ERROR_SEVERITY() AS ErrorSeverity,  
        ERROR_STATE() AS ErrorState,  
        ERROR_PROCEDURE() AS ErrorProcedure,  
        ERROR_MESSAGE() AS ErrorMessage;  
END CATCH;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.messages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/messages-for-errors-catalog-views-sys-messages.md)   
 [TRY...CATCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/try-catch-transact-sql.md)   
 [ERROR_LINE &#40;Transact-SQL&#41;](../../t-sql/functions/error-line-transact-sql.md)   
 [ERROR_MESSAGE &#40;Transact-SQL&#41;](../../t-sql/functions/error-message-transact-sql.md)   
 [ERROR_NUMBER &#40;Transact-SQL&#41;](../../t-sql/functions/error-number-transact-sql.md)   
 [ERROR_PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/functions/error-procedure-transact-sql.md)   
 [ERROR_SEVERITY &#40;Transact-SQL&#41;](../../t-sql/functions/error-severity-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../../t-sql/language-elements/raiserror-transact-sql.md)   
 [@@ERROR &#40;Transact-SQL&#41;](../../t-sql/functions/error-transact-sql.md)    
 [Referencia de errores y eventos &#40;Motor de base de datos&#41;](../../relational-databases/errors-events/errors-and-events-reference-database-engine.md)     
  
    

