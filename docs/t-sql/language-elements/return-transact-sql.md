---
description: RETURN (Transact-SQL)
title: RETURN (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- RETURN_TSQL
- RETURN
dev_langs:
- TSQL
helpviewer_keywords:
- unconditionally exiting program
- stored procedures [SQL Server], exiting
- batches [SQL Server], exiting
- statement blocks [SQL Server]
- queries [SQL Server], exiting
- exiting queries [SQL Server]
- exiting procedures [SQL Server]
- RETURN statement
ms.assetid: 1d9c8247-fd89-4544-be9c-01c95b745db0
author: cawrites
ms.author: chadam
ms.openlocfilehash: aec79684b2d0325381c3a1d70b976e4281007f61
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194316"
---
# <a name="return-transact-sql"></a>RETURN (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Sale incondicionalmente de una consulta o procedimiento. RETURN es inmediata y completa, y se puede utilizar en cualquier punto para salir de un procedimiento, lote o bloque de instrucciones. Las instrucciones que siguen a RETURN no se ejecutan.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
RETURN [ integer_expression ]   
```  
  
## <a name="arguments"></a>Argumentos  
 *integer_expression*  
 Es el valor entero que se devuelve. Los procedimientos almacenados pueden devolver un valor entero al procedimiento que realiza la llamada o a una aplicación.  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>Tipos de valor devueltos
 Devuelve opcionalmente **int**.  
  
> [!NOTE]  
>  A menos que se documente de otra manera, todos los procedimientos almacenados del sistema devuelven el valor 0. Esto indica que son correctos y un valor distinto de cero indica que se ha producido un error.  
  
## <a name="remarks"></a>Observaciones  
 Cuando se utiliza con un procedimiento almacenado, RETURN no puede devolver un valor NULL. Si un procedimiento intenta devolver un valor NULL (por ejemplo, al utilizar RETURN @status cuando @status es NULL), se genera un mensaje de advertencia y se devuelve un valor 0.  
  
 El valor de estado devuelto se puede incluir en las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] del lote o procedimiento que ha ejecutado el procedimiento actual, pero se debe escribir de la forma siguiente: `EXECUTE @return_status = <procedure_name>`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-from-a-procedure"></a>A. Devolver desde un procedimiento  
 En el siguiente ejemplo se muestra que si no se proporciona ningún nombre de usuario como parámetro al ejecutar `findjobs`, `RETURN` provoca la salida del procedimiento tras enviar un mensaje a la pantalla del usuario. Si se especifica un nombre de usuario, se obtienen de las tablas del sistema adecuadas los nombres de todos los objetos creados por este usuario en la base de datos actual.  
  
```sql  
CREATE PROCEDURE findjobs @nm sysname = NULL  
AS   
IF @nm IS NULL  
    BEGIN  
        PRINT 'You must give a user name'  
        RETURN  
    END  
ELSE  
    BEGIN  
        SELECT o.name, o.id, o.uid  
        FROM sysobjects o INNER JOIN master..syslogins l  
            ON o.uid = l.sid  
        WHERE l.name = @nm  
    END;  
```  
  
### <a name="b-returning-status-codes"></a>B. Devolver códigos de estado  
 En el siguiente ejemplo se comprueba el estado del Id. de un contacto especificado. Si el estado es Washington (`WA`), se devuelve un estado de `1`. En caso contrario, se devuelve `2` para cualquier otra condición (un valor distinto de `WA` para `StateProvince` o `ContactID` que no coincida con una fila).  
  
```sql  
USE AdventureWorks2012;  
GO  
CREATE PROCEDURE checkstate @param VARCHAR(11)  
AS  
IF (SELECT StateProvince FROM Person.vAdditionalContactInfo WHERE ContactID = @param) = 'WA'  
    RETURN 1  
ELSE  
    RETURN 2;  
GO  
```  
  
 En los siguientes ejemplos se muestra el estado devuelto al ejecutar `checkstate`. En el primer ejemplo se muestra un contacto en Washington, en el segundo ejemplo un contacto distinto de Washington y en el tercer ejemplo un contacto no válido. Se debe declarar la variable local `@return_status` para poder utilizarla.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '2';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status 
  
 ------------- 
  
 1
 ```  
  
 Ejecute la consulta de nuevo con un número de contacto diferente.  
  
```sql  
DECLARE @return_status INT;  
EXEC @return_status = checkstate '6';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
 Ejecute la consulta de nuevo con otro número de contacto.  
  
```sql  
DECLARE @return_status INT  
EXEC @return_status = checkstate '12345678901';  
SELECT 'Return Status' = @return_status;  
GO  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
 Return Status  
 -------------  
  
 2
 ```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-procedure-transact-sql.md)   
 [CREATE PROCEDURE &#40;Transact-SQL&#41;](../../t-sql/statements/create-procedure-transact-sql.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)   
 [THROW &#40;Transact-SQL&#41;](../../t-sql/language-elements/throw-transact-sql.md)  
  
  
