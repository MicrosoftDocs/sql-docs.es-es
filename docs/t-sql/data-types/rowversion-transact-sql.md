---
description: rowversion (Transact-SQL)
title: rowversion (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/22/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- timestamp_TSQL
- timestamp
dev_langs:
- TSQL
helpviewer_keywords:
- rowversion data type
- size [SQL Server], rowversion
- row version-stamping [SQL Server]
- rowversion columns
- version-stamping table rows
- unique rowversion numbers
- unique timestamp numbers
- timestamp data type
- timestamp columns
- size [SQL Server], timestamp
ms.assetid: 65c9cf0e-3e8a-45f8-87b3-3460d96afb0b
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 47117ba5b13d7b494174576d76cca9a5472f0c9e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187834"
---
# <a name="rowversion-transact-sql"></a>rowversion (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Es un tipo de datos que expone números binarios únicos generados automáticamente en una base de datos. **rowversion** se suele usar como mecanismo para marcar la versión de las filas de la tabla. El tamaño de almacenamiento es de 8 bytes. El tipo de datos **rowversion** es simplemente un número que se incrementa y no conserva una fecha o una hora. Para registrar una fecha o una hora, use un tipo de datos **datetime2**.
  
## <a name="remarks"></a>Comentarios  
Cada base de datos tiene un contador que se incrementa por cada operación de inserción o actualización que se lleva a cabo en una tabla que contiene una columna **rowversion** en la base de datos. Este contador es la versión de fila (rowversion) de la base de datos. Realiza un seguimiento de una hora relativa de una base de datos, no una hora real que pueda asociarse con un reloj. Una tabla solo puede tener una columna **rowversion**. Cada vez que se modifica o inserta una fila con una columna **rowversion**, el valor rowversion de la base de datos incrementado se inserta en la columna **rowversion**. Esta propiedad hace que una columna **rowversion** sea un mal candidato para claves, especialmente claves principales. Cualquier actualización de la fila hace que cambie el valor rowversion, con lo que cambia el valor de la clave. Si la columna está en una clave principal, el valor de la clave principal antigua deja de ser válido, así como las claves externas que hacen referencia al valor antiguo. Si se hace referencia a la tabla en un cursor dinámico, todas las actualizaciones cambian la posición de las filas en el cursor. Si la columna es una clave de índice, todas las actualizaciones de la fila de datos también generan actualizaciones del índice.  El valor de **rowversion** se incrementa con cualquier instrucción de actualización, incluso si ningún valor de fila cambia. Por ejemplo, si un valor de columna es 5 y una instrucción de actualización establece el valor en 5, esta acción se considera una actualización, aun cuando no hay cambio alguno, y **rowversion** aumenta.
  
**timestamp** es el sinónimo del tipo de datos **rowversion** y está sujeto al comportamiento de los sinónimos de tipos de datos. En las instrucciones DDL, use **rowversion** en lugar de **timestamp** siempre que sea posible. Para más información, vea [Sinónimos de tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-synonyms-transact-sql.md).
  
El tipo de datos  **timestamp** de [!INCLUDE[tsql](../../includes/tsql-md.md)] es distinto del tipo de datos **timestamp** definido en el estándar ISO.
  
> [!NOTE]  
>  La sintaxis de **timestamp** está en desuso. [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)]  
  
En una instrucción CREATE TABLE o ALTER TABLE, no tiene que especificar ningún nombre de columna para el tipo de datos **timestamp**, por ejemplo:
  
```sql
CREATE TABLE ExampleTable (PriKey int PRIMARY KEY, timestamp);  
```  
  
Si no especifica un nombre de columna, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] genera el nombre de columna **timestamp**; el sinónimo de **rowversion**, en cambio, no sigue este comportamiento. Cuando usa **rowversion**, debe especificar un nombre de columna, por ejemplo:
  
```sql
CREATE TABLE ExampleTable2 (PriKey int PRIMARY KEY, VerCol rowversion) ;  
```  
  
> [!NOTE]  
>  Se pueden generar valores **rowversion** duplicados con la instrucción SELECT INTO en la que una columna **rowversion** está en la lista SELECT. No se recomienda usar **rowversion** de esta manera.  
  
Una columna **rowversion** que no admite valores NULL equivale semánticamente a una columna **binary(8)**. Una columna **rowversion** que admite valores NULL equivale semánticamente a una columna **varbinary(8)**.
  
Puede usar la columna **rowversion** de una fila para saber fácilmente si se ha ejecutado una instrucción de actualización en la fila desde la última vez que se ha leído. Si se ejecuta una instrucción de actualización en la fila, el valor rowversion se actualiza. Si no se ejecuta ninguna instrucción de actualización en la fila, el valor rowversion es el mismo que el de la lectura anterior. Para devolver el valor rowversion actual de una base de datos, use [@@DBTS](../../t-sql/functions/dbts-transact-sql.md).
  
Puede agregar una columna **rowversion** a una tabla para ayudar a mantener la integridad de la base de datos cuando varios usuarios actualizan filas al mismo tiempo. También puede que desee conocer cuántas filas y qué filas se actualizaron sin volver a consultar la tabla.
  
Por ejemplo, suponga que crea una tabla denominada `MyTest`. Rellena algunos datos en la tabla ejecutando las siguientes instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)].
  
```sql
CREATE TABLE MyTest (myKey int PRIMARY KEY  
    ,myValue int, RV rowversion);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest (myKey, myValue) VALUES (2, 0);  
GO  
```  
  
A continuación, puede utilizar las instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)] de ejemplo siguientes para implementar el control de simultaneidad optimista en la tabla `MyTest` durante la actualización. El script usa `<myRv>` para representar el valor **rowversion** desde la última vez que se ha leído la fila. Reemplace el valor por el valor **rowversion** real. Un ejemplo de un valor **rowversion** real es `0x00000000000007D3`.
  
```sql
DECLARE @t TABLE (myKey int);  
UPDATE MyTest  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND RV = <myRv>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  


Las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] de ejemplo también se pueden incluir en una transacción. Al consultar la variable `@t` en el ámbito de la transacción, puede recuperar la columna `myKey` actualizada de la tabla sin consultar de nuevo la tabla `MyTest`.

En el ejemplo siguiente se usa la sintaxis de **timestamp**. Reemplace `<myTS>` por un valor **timestamp** real.

```sql
CREATE TABLE MyTest2 (myKey int PRIMARY KEY  
    ,myValue int, TS timestamp);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (1, 0);  
GO   
INSERT INTO MyTest2 (myKey, myValue) VALUES (2, 0);  
GO  
DECLARE @t TABLE (myKey int);  
UPDATE MyTest2  
SET myValue = 2  
    OUTPUT inserted.myKey INTO @t(myKey)   
WHERE myKey = 1   
    AND TS = <myTS>;  
IF (SELECT COUNT(*) FROM @t) = 0  
    BEGIN  
        RAISERROR ('error changing row with myKey = %d'  
            ,16 -- Severity.  
            ,1 -- State   
            ,1) -- myKey that was changed   
    END;  
```  
  
## <a name="see-also"></a>Consulte también
[ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)  
[CAST y CONVERT &#40;Transact-SQL&#41;](../../t-sql/functions/cast-and-convert-transact-sql.md)  
[CREATE TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-table-transact-sql.md)  
[Tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
[DECLARE @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-local-variable-transact-sql.md)  
[DELETE &#40;Transact-SQL&#41;](../../t-sql/statements/delete-transact-sql.md)  
[INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/insert-transact-sql.md)  
[MIN_ACTIVE_ROWVERSION &#40;Transact-SQL&#41;](../../t-sql/functions/min-active-rowversion-transact-sql.md)  
[SET @local_variable &#40;Transact-SQL&#41;](../../t-sql/language-elements/set-local-variable-transact-sql.md)  
[UPDATE &#40;Transact-SQL&#41;](../../t-sql/queries/update-transact-sql.md)
  
  
