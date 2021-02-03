---
description: DEALLOCATE (Transact-SQL)
title: DEALLOCATE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DEALLOCATE
- DEALLOCATE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- locking [SQL Server], cursors
- DEALLOCATE statement
- deallocations [SQL Server]
- deleting cursor references
- removing cursor references
ms.assetid: c75cf73d-0268-4c57-973d-b8a84ff801fa
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2b6ad1892bec2cb627e4565c798506bcea91b530
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208776"
---
# <a name="deallocate-transact-sql"></a>DEALLOCATE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Quita una referencia a un cursor. Cuando se desasigna la última referencia al cursor, [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] libera las estructuras de datos que componen el cursor.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
DEALLOCATE { { [ GLOBAL ] cursor_name } | @cursor_variable_name }  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *cursor_name*  
 Es el nombre de un cursor ya declarado. Si existen un cursor global y otro local denominados *cursor_name*, *cursor_name* hace referencia al cursor global si se especifica `GLOBAL` y al cursor local si no se especifica `GLOBAL`.  
  
 @*cursor_variable_name*  
 Es el nombre de una variable de **cursor**. @*cursor_variable_name* debe ser de tipo **cursor**.  
  
## <a name="remarks"></a>Comentarios  
Las instrucciones que realizan operaciones sobre cursores utilizan un nombre de cursor o una variable de cursor para hacer referencia al cursor. `DEALLOCATE` quita la asociación existente entre un cursor y el nombre del cursor o la variable de cursor. Si un nombre o variable es el último que hace referencia a un cursor, se quita el cursor y se liberan los recursos que utiliza. Los bloqueos de desplazamiento usados para proteger el aislamiento de las capturas se liberan en `DEALLOCATE`. Los bloqueos de transacciones utilizados para proteger las actualizaciones, incluidas las actualizaciones posicionadas creadas a través del cursor, se mantienen hasta el final de la transacción.  
  
La instrucción `DECLARE CURSOR` asigna y asocia un cursor a un nombre de cursor.  
  
```sql  
DECLARE abc SCROLL CURSOR FOR  
SELECT * FROM Person.Person;  
```  
  
Después de asociar un nombre de cursor a un cursor, ningún otro cursor del mismo ámbito (global o local) puede usar el nombre hasta que se haya cancelado la asignación al cursor.  
  
 Una variable de cursor se puede asociar a un cursor mediante uno de estos dos métodos:  
  
-   Por nombre con una instrucción `SET` que asocia un cursor a una variable de cursor.  
  
    ```sql  
    DECLARE @MyCrsrRef CURSOR;  
    SET @MyCrsrRef = abc;  
    ```  
  
-   También se puede crear y asociar un cursor a una variable sin necesidad de definir un nombre de cursor.  
  
    ```sql  
    DECLARE @MyCursor CURSOR;  
    SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Person.Person;  
    ```  
  
 Una instrucción `DEALLOCATE <@cursor_variable_name>` solo quita la referencia de la variable con nombre al cursor. No se desasigna la variable hasta que sale de ámbito al final del proceso por lotes, procedimiento almacenado o desencadenador. Después de una instrucción `DEALLOCATE <@cursor_variable_name>`, se puede asociar la variable a otro cursor mediante la instrucción SET.  
  
```sql  
USE AdventureWorks2012;  
GO  
  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
  
DEALLOCATE @MyCursor;  
  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
    SELECT * FROM Sales.SalesTerritory;  
GO  
```  
  
No es necesario cancelar explícitamente la asignación de una variable de cursor. La asignación de la variable se cancela implícitamente cuando sale de ámbito.  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, cualquier usuario válido tiene permiso para usar `DEALLOCATE`.  
  
## <a name="examples"></a>Ejemplos  
 El siguiente script muestra cómo los cursores se mantienen hasta que se ha desasignado el último nombre o variable que hace referencia a ellos.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Create and open a global named cursor that  
-- is visible outside the batch.  
DECLARE abc CURSOR GLOBAL SCROLL FOR  
    SELECT * FROM Sales.SalesPerson;  
OPEN abc;  
GO  
-- Reference the named cursor with a cursor variable.  
DECLARE @MyCrsrRef1 CURSOR;  
SET @MyCrsrRef1 = abc;  
-- Now deallocate the cursor reference.  
DEALLOCATE @MyCrsrRef1;  
-- Cursor abc still exists.  
FETCH NEXT FROM abc;  
GO  
-- Reference the named cursor again.  
DECLARE @MyCrsrRef2 CURSOR;  
SET @MyCrsrRef2 = abc;  
-- Now deallocate cursor name abc.  
DEALLOCATE abc;  
-- Cursor still exists, referenced by @MyCrsrRef2.  
FETCH NEXT FROM @MyCrsrRef2;  
-- Cursor finally is deallocated when last referencing  
-- variable goes out of scope at the end of the batch.  
GO  
-- Create an unnamed cursor.  
DECLARE @MyCursor CURSOR;  
SET @MyCursor = CURSOR LOCAL SCROLL FOR  
SELECT * FROM Sales.SalesTerritory;  
-- The following statement deallocates the cursor  
-- because no other variables reference it.  
DEALLOCATE @MyCursor;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [Cursores](../../relational-databases/cursors.md)   
 [DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)   
 [FETCH &#40;Transact-SQL&#41;](../../t-sql/language-elements/fetch-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
