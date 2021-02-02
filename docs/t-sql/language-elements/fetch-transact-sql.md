---
description: FETCH (Transact-SQL)
title: FETCH (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- FETCH
- FETCH_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- FETCH statement
- cursors [SQL Server], fetching
- Transact-SQL cursors, fetching and scrolling
- retrieving rows
- fetching [SQL Server]
- SCROLL option
- row fetching [SQL Server]
ms.assetid: 5d68dac2-f91b-4342-bb4e-209ee132665f
author: cawrites
ms.author: chadam
ms.openlocfilehash: da86d5cdd7889383c370f9eb9a8a6b6322100c9d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99115311"
---
# <a name="fetch-transact-sql"></a>FETCH (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Recupera una fila específica de un cursor de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
FETCH   
          [ [ NEXT | PRIOR | FIRST | LAST   
                    | ABSOLUTE { n | @nvar }   
                    | RELATIVE { n | @nvar }   
               ]   
               FROM   
          ]   
{ { [ GLOBAL ] cursor_name } | @cursor_variable_name }   
[ INTO @variable_name [ ,...n ] ]   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 NEXT  
 Devuelve la fila de resultados inmediatamente posterior a la fila actual, y aumenta la fila actual a la fila devuelta. Si `FETCH NEXT` es la primera operación de captura en un cursor, se devuelve la primera fila del conjunto de resultados. `NEXT` es la opción predeterminada para la captura de cursores.  
  
 PRIOR  
 Devuelve la fila de resultados inmediatamente anterior a la fila actual, y reduce la fila actual a la fila devuelta. Si `FETCH PRIOR` es la primera operación de captura en un cursor, no se devuelve ninguna fila y el cursor queda colocado delante de la primera fila.  
  
 FIRST  
 Devuelve la primera fila del cursor y la convierte en la fila actual.  
  
 LAST  
 Devuelve la última fila del cursor y la convierte en la fila actual.  
  
 ABSOLUTE { *n*| \@*nvar*}  
 Si *n* o \@*nvar* es positivo, se devuelve la fila *n* desde el principio del cursor y la fila devuelta se convierte en la nueva fila actual. Si *n* o \@*nvar* es negativo, se devuelve la fila *n* antes del final del cursor y la fila devuelta se convierte en la nueva fila actual. Si *n* o \@*nvar* es 0, no se devuelve ninguna fila. *n* debe ser una constante de tipo entero y \@*nvar* debe ser **smallint**, **tinyint** o **int**.  
  
 RELATIVE { *n*| \@*nvar*}  
 Si *n* o \@*nvar* es positivo, se devuelve la fila *n* posterior a la fila actual y la fila devuelta se convierte en la nueva fila actual. Si *n* o \@*nvar* es negativo, se devuelve la fila *n* anterior a la fila actual y la fila devuelta se convierte en la nueva fila actual. Si *n* o \@*nvar* es 0, se devuelve la fila actual. Si `FETCH RELATIVE` se especifica con *n* o \@*nvar* se establece en números negativos o 0 en la primera operación de captura que se realiza en un cursor, no se devuelven filas. *n* debe ser una constante de tipo entero y \@*nvar* debe ser **smallint**, **tinyint** o **int**.  
  
 GLOBAL  
 Especifica que *cursor_name* hace referencia a un cursor global.  
  
 *cursor_name*  
 Es el nombre del cursor abierto desde el que se debe realizar la captura. Si hay un cursor global y otro local denominados *cursor_name*, *cursor_name* hace referencia al cursor global si se especifica GLOBAL y al cursor local si no se especifica GLOBAL.  
  
 \@*cursor_variable_name*  
 Es el nombre de una variable de cursor que hace referencia al cursor abierto desde el que se va efectuar la captura.  
  
 INTO \@*nombre_de_variable*[ ,...*n*]  
 Permite colocar en variables locales los datos de las columnas de una captura. Todas las variables de la lista, de izquierda a derecha, están asociadas a las columnas correspondientes del conjunto de resultados del cursor. El tipo de datos de cada variable tiene que coincidir o ser compatible con la conversión implícita del tipo de datos de la columna correspondiente del conjunto de resultados. El número de variables debe coincidir con el número de columnas de la lista de selección del cursor.  
  
## <a name="remarks"></a>Comentarios  
 Si la opción `SCROLL` no se especifica con una instrucción `DECLARE CURSOR` de estilo ISO, `NEXT` es la única opción de `FETCH` admitida. Si `SCROLL` se especifica con un `DECLARE CURSOR` de estilo ISO, se admiten todas las opciones de `FETCH`.  
  
 Cuando se utilizan las extensiones de cursor DECLARE de [!INCLUDE[tsql](../../includes/tsql-md.md)], se aplican las reglas siguientes:  
  
-   Si se especifica `FORWARD_ONLY` o `FAST_FORWARD`, `NEXT` es la única opción de `FETCH` admitida.  
  
-   Si no se especifican las opciones `DYNAMIC`, `FORWARD_ONLY` o `FAST_FORWARD`, y se especifica una de las opciones `KEYSET`, `STATIC` o `SCROLL`, se admitirán todas las opciones de `FETCH`.  
  
-   Los cursores `DYNAMIC SCROLL` admiten todas las opciones de `FETCH`, excepto `ABSOLUTE`.  
  
 La función `@@FETCH_STATUS` informa del estado de la última instrucción `FETCH`. La misma información queda grabada en la columna fetch_status del cursor devuelto por sp_describe_cursor. Esta información de estado tiene que usarse para determinar la validez de los datos devueltos por una instrucción `FETCH` antes de intentar realizar cualquier operación con esos datos. Para más información, vea [@@FETCH_STATUS &#40;Transact-SQL&#41;](../../t-sql/functions/fetch-status-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 De forma predeterminada, cualquier usuario válido tiene permiso para usar `FETCH`.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-using-fetch-in-a-simple-cursor"></a>A. Utilizar FETCH en un cursor sencillo  
 En este ejemplo se declara un cursor sencillo para las filas de la tabla `Person.Person` cuyo apellido empiece por `B` y se utiliza `FETCH NEXT` para recorrer las filas paso a paso. Las instrucciones `FETCH` devuelven el valor de la columna especificada en `DECLARE CURSOR` como un conjunto de resultados de una sola fila.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch.  
FETCH NEXT FROM contact_cursor;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="b-using-fetch-to-store-values-in-variables"></a>B. Utilizar FETCH para almacenar valores en variables  
 El siguiente ejemplo es similar al ejemplo A, con la diferencia de que la salida de las instrucciones `FETCH` se almacena en variables locales en lugar de devolverse directamente al cliente. La instrucción `PRINT` combina las variables en una misma cadena y la devuelve al cliente.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Declare the variables to store the values returned by FETCH.  
DECLARE @LastName VARCHAR(50), @FirstName VARCHAR(50);  
  
DECLARE contact_cursor CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
WHERE LastName LIKE 'B%'  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Perform the first fetch and store the values in variables.  
-- Note: The variables are in the same order as the columns  
-- in the SELECT statement.   
  
FETCH NEXT FROM contact_cursor  
INTO @LastName, @FirstName;  
  
-- Check @@FETCH_STATUS to see if there are any more rows to fetch.  
WHILE @@FETCH_STATUS = 0  
BEGIN  
  
   -- Concatenate and display the current values in the variables.  
   PRINT 'Contact Name: ' + @FirstName + ' ' +  @LastName  
  
   -- This is executed as long as the previous fetch succeeds.  
   FETCH NEXT FROM contact_cursor  
   INTO @LastName, @FirstName;  
END  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
### <a name="c-declaring-a-scroll-cursor-and-using-the-other-fetch-options"></a>C. Declarar un cursor SCROLL y utilizar el resto de las opciones de FETCH  
 En el siguiente ejemplo se crea un cursor `SCROLL` y se usan todas las funciones de desplazamiento mediante las opciones `LAST`, `PRIOR`, `RELATIVE` y `ABSOLUTE`.  
  
```sql  
USE AdventureWorks2012;  
GO  
-- Execute the SELECT statement alone to show the   
-- full result set that is used by the cursor.  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
-- Declare the cursor.  
DECLARE contact_cursor SCROLL CURSOR FOR  
SELECT LastName, FirstName FROM Person.Person  
ORDER BY LastName, FirstName;  
  
OPEN contact_cursor;  
  
-- Fetch the last row in the cursor.  
FETCH LAST FROM contact_cursor;  
  
-- Fetch the row immediately prior to the current row in the cursor.  
FETCH PRIOR FROM contact_cursor;  
  
-- Fetch the second row in the cursor.  
FETCH ABSOLUTE 2 FROM contact_cursor;  
  
-- Fetch the row that is three rows after the current row.  
FETCH RELATIVE 3 FROM contact_cursor;  
  
-- Fetch the row that is two rows prior to the current row.  
FETCH RELATIVE -2 FROM contact_cursor;  
  
CLOSE contact_cursor;  
DEALLOCATE contact_cursor;  
GO  
```  
  
## <a name="see-also"></a>Vea también  
 [CLOSE &#40;Transact-SQL&#41;](../../t-sql/language-elements/close-transact-sql.md)   
 [DEALLOCATE &#40;Transact-SQL&#41;](../../t-sql/language-elements/deallocate-transact-sql.md)   
 [DECLARE CURSOR &#40;Transact-SQL&#41;](../../t-sql/language-elements/declare-cursor-transact-sql.md)   
 [OPEN &#40;Transact-SQL&#41;](../../t-sql/language-elements/open-transact-sql.md)  
  
  
