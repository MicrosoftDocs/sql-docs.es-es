---
description: REVERT (Transact-SQL)
title: REVERT (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- REVERT_TSQL
- REVERT
dev_langs:
- TSQL
helpviewer_keywords:
- REVERT statement
- context switching [SQL Server], reverting
- reverting execution context
- REVERT WITH COOKIE statement
- execution context [SQL Server]
- COOKIE clause
ms.assetid: 4688b17a-dfd1-4f03-8db4-273a401f879f
author: VanMSFT
ms.author: vanto
monikerRange: = azuresqldb-current || = azuresqldb-mi-current || >= sql-server-2016 || >= sql-server-linux-2017||=azure-sqldw-latest
ms.openlocfilehash: f8966a7c1204ec61f2271d4cd44a559a593473a2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99209977"
---
# <a name="revert-transact-sql"></a>REVERT (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]   

  Vuelve a cambiar el contexto de ejecución al autor de llamada de la última instrucción EXECUTE AS.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
REVERT  
    [ WITH COOKIE = @varbinary_variable ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 WITH COOKIE = @*varbinary_variable*  
 Especifica la cookie que se creó en la instrucción independiente [EXECUTE AS](../../t-sql/statements/execute-as-transact-sql.md) correspondiente. *\@varbinary_variable* es **varbinary(100)** .  
  
## <a name="remarks"></a>Observaciones  
 REVERT se puede especificar en un módulo, por ejemplo, en un procedimiento almacenado o una función definida por el usuario, o como una instrucción independiente. Cuando se especifica en un módulo, REVERT solo se aplica a las instrucciones EXECUTE AS definidas en el módulo. Por ejemplo, el siguiente procedimiento almacenado emite una instrucción `EXECUTE AS` seguida de una instrucción `REVERT`.  
  
```sql  
CREATE PROCEDURE dbo.usp_myproc   
  WITH EXECUTE AS CALLER  
AS   
    SELECT SUSER_NAME(), USER_NAME();  
    EXECUTE AS USER = 'guest';  
    SELECT SUSER_NAME(), USER_NAME();  
    REVERT;  
    SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
 Suponga que, como se muestra en el siguiente ejemplo, en la sesión en la que se ejecuta el procedimiento almacenado, el contexto de ejecución de la sesión se cambia explícitamente a `login1`.  
  
```sql 
  -- Sets the execution context of the session to 'login1'.  
EXECUTE AS LOGIN = 'login1';  
GO  
EXECUTE dbo.usp_myproc;   
```  
  
 La instrucción `REVERT` definida en `usp_myproc` cambia el contexto de ejecución establecido en el módulo, pero esto no afecta al contexto de ejecución establecido fuera del módulo. Es decir, el contexto de ejecución de la sesión permanece establecido en `login1`.  
  
 Cuando se especifica como una instrucción independiente, REVERT se aplica a las instrucciones EXECUTE AS definidas en un lote o sesión. REVERT no tiene efecto si la instrucción EXECUTE AS correspondiente contiene la cláusula WITH NO REVERT. En este caso, el contexto de ejecución permanece efectivo hasta que se quita la sesión.  
  
## <a name="using-revert-with-cookie"></a>Usar REVERT WITH COOKIE  
 La instrucción EXECUTE AS que se usa para establecer el contexto de ejecución de una sesión puede incluir la cláusula opcional WITH NO REVERT COOKIE = @*variable_varbinary*. Cuando se ejecuta esta instrucción, [!INCLUDE[ssDE](../../includes/ssde-md.md)] pasa la cookie a @*variable_varbinary*. El contexto de ejecución que establece esa instrucción se puede revertir al contexto anterior solo si la llamada a la instrucción REVERT WITH COOKIE = @*varbinary_variable* contiene el valor *\@varbinary_variable* correcto.  
  
 Este mecanismo es útil en un entorno donde se utiliza la agrupación de conexiones. La agrupación de conexiones es el mantenimiento de un grupo de base de datos que reutilizan las aplicaciones entre varios usuarios finales. Puesto que el valor pasado a *\@varbinary_variable* solo lo conoce el llamador de la instrucción EXECUTE AS (en este caso, la aplicación), el llamador puede garantizar que el usuario final que llama a la aplicación no puede cambiar el contexto de ejecución que establece. Después de volver el contexto de ejecución, la aplicación puede cambiar el contexto a otra entidad de seguridad.  
  
## <a name="permissions"></a>Permisos  
 No se requieren permisos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-execute-as-and-revert-to-switch-context"></a>A. Usar EXECUTE AS y REVERT para cambiar el contexto  
 En el siguiente ejemplo se crea una pila de contextos de ejecución usando varias entidades de seguridad. La instrucción REVERT se utiliza a continuación para restablecer el contexto de ejecución al llamador anterior. La instrucción REVERT se ejecuta varias veces subiendo la pila hasta que el contexto de ejecución se establece en el llamador original.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create two temporary principals.  
CREATE LOGIN login1 WITH PASSWORD = 'J345#$)thb';  
CREATE LOGIN login2 WITH PASSWORD = 'Uor80$23b';  
GO  
CREATE USER user1 FOR LOGIN login1;  
CREATE USER user2 FOR LOGIN login2;  
GO  
-- Give IMPERSONATE permissions on user2 to user1  
-- so that user1 can successfully set the execution context to user2.  
GRANT IMPERSONATE ON USER:: user2 TO user1;  
GO  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- Set the execution context to login1.   
EXECUTE AS LOGIN = 'login1';  
-- Verify that the execution context is now login1.  
SELECT SUSER_NAME(), USER_NAME();  
-- Login1 sets the execution context to login2.  
EXECUTE AS USER = 'user2';  
-- Display current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
-- The execution context stack now has three principals: the originating caller, login1, and login2.  
-- The following REVERT statements will reset the execution context to the previous context.  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
REVERT;  
-- Display the current execution context.  
SELECT SUSER_NAME(), USER_NAME();  
  
-- Remove the temporary principals.  
DROP LOGIN login1;  
DROP LOGIN login2;  
DROP USER user1;  
DROP USER user2;  
GO  
```  
  
### <a name="b-using-the-with-cookie-clause"></a>B. Usar la cláusula WITH COOKIE  
 En el ejemplo siguiente se establece el contexto de ejecución de una sesión en un usuario determinado y se especifica la cláusula WITH NO REVERT COOKIE = @*variable_varbinary*. La instrucción `REVERT` debe especificar el valor pasado a la variable `@cookie` en la instrucción `EXECUTE AS` para volver correctamente el contexto al llamador. Para ejecutar este ejemplo, deben existir el inicio de sesión `login1` y el usuario `user1` creados en el ejemplo A.  
  
```sql 
DECLARE @cookie VARBINARY(100);  
EXECUTE AS USER = 'user1' WITH COOKIE INTO @cookie;  
-- Store the cookie somewhere safe in your application.  
-- Verify the context switch.  
SELECT SUSER_NAME(), USER_NAME();  
--Display the cookie value.  
SELECT @cookie;  
GO  
-- Use the cookie in the REVERT statement.  
DECLARE @cookie VARBINARY(100);  
-- Set the cookie value to the one from the SELECT @cookie statement.  
SET @cookie = <value from the SELECT @cookie statement>;  
REVERT WITH COOKIE = @cookie;  
-- Verify the context switch reverted.  
SELECT SUSER_NAME(), USER_NAME();  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [EXECUTE AS &#40;Transact-SQL&#41;](../../t-sql/statements/execute-as-transact-sql.md)   
 [EXECUTE AS &#40;cláusula de Transact-SQL&#41;](../../t-sql/statements/execute-as-clause-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SUSER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/suser-name-transact-sql.md)   
 [USER_NAME &#40;Transact-SQL&#41;](../../t-sql/functions/user-name-transact-sql.md)   
 [CREATE LOGIN &#40;Transact-SQL&#41;](../../t-sql/statements/create-login-transact-sql.md)   
 [CREATE USER &#40;Transact-SQL&#41;](../../t-sql/statements/create-user-transact-sql.md)  
  
  
