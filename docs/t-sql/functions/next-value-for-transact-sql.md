---
description: NEXT VALUE FOR (Transact-SQL)
title: NEXT VALUE FOR (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/19/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- NEXT_VALUE_TSQL
- NEXT
- NEXT VALUE
- NEXT VALUE FOR
- NEXT_TSQL
- NEXT_VALUE_FOR_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- NEXT VALUE FOR function
- sequence number object, NEXT VALUE FOR function
ms.assetid: 92632ed5-9f32-48eb-be28-a5e477ef9076
author: julieMSFT
ms.author: jrasnick
ms.openlocfilehash: 4af385b2d2e8874fe7554fe213c355a97382d520
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99189935"
---
# <a name="next-value-for-transact-sql"></a>NEXT VALUE FOR (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Genera un número de secuencia a partir del objeto de secuencia especificado.  
  
 Para más información sobre cómo crear y usar secuencias, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md). Use [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) para generar una reserva de un intervalo de números de secuencia.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql
NEXT VALUE FOR [ database_name . ] [ schema_name . ]  sequence_name  
   [ OVER (<over_order_by_clause>) ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Argumentos
 *database_name*  
 Nombre de la base de datos que contiene el objeto de secuencia.  
  
 *schema_name*  
 Nombre del esquema que contiene el objeto de secuencia.  
  
 *sequence_name*  
 Nombre del objeto de secuencia que genera el número.  
  
 *over_order_by_clause*  
 Determina el orden en el que se asigna el valor de secuencia a las filas de una partición. Para más información, vea [Cláusula OVER &#40;Transact-SQL&#41;](../../t-sql/queries/select-over-clause-transact-sql.md).  
  
## <a name="return-types"></a>Tipos de valor devuelto  
 Devuelve un número utilizando el tipo de la secuencia.  
  
## <a name="remarks"></a>Comentarios  
 La función **NEXT VALUE FOR** se puede usar en procedimientos almacenados y en desencadenadores.  
  
 Cuando la función **NEXT VALUE FOR** se usa en una consulta o restricción predeterminada, si se usa más de una vez el mismo objeto de secuencia, o si se usa el mismo objeto de secuencia en la instrucción que proporciona los valores y en una restricción predeterminada que se está ejecutando, se devolverá el mismo valor para todas las columnas que hagan referencia a la misma secuencia dentro de una fila del conjunto de resultados.  
  
 La función **NEXT VALUE FOR** es no determinista y solo se permite en contextos donde se define bien el número de valores de secuencia generados. A continuación se encuentra la definición de cuántos valores se utilizarán para cada objeto de secuencia al que se hace referencia en una instrucción determinada:  
  
-   **SELECT**: por cada objeto de secuencia al que se hace referencia, se genera un valor nuevo una vez por fila en el resultado de la instrucción.  
  
-   **INSERT** ... **VALUES**: por cada objeto de secuencia al que se hace referencia, se genera un valor nuevo una vez por cada fila insertada en la instrucción.  
  
-   **UPDATE**: por cada objeto de secuencia al que se hace referencia, se genera un valor nuevo para cada fila que actualiza la instrucción.  
  
-   Instrucciones de procedimiento (como **DECLARE**, **SET**, etc.): por cada objeto de secuencia al que se hace referencia, se genera un valor nuevo para cada instrucción.  
  
## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 La función **NEXT VALUE FOR** no se puede usar en las siguientes situaciones:  
  
-   Cuando una base de datos está en modo de solo lectura.  
  
-   Como argumento a una función con valores de tabla.  
  
-   Como argumento a una función de agregado.  
  
-   En subconsultas que contienen expresiones de tabla comunes y tablas derivadas.  
  
-   En vistas, en funciones definidas por el usuario o en columnas calculadas.  
  
-   En una instrucción que use los operadores **DISTINCT**, **UNION**, **UNION ALL**, **EXCEPT** o **INTERSECT**.  
  
-   En una instrucción que use la cláusula **ORDER BY**, a menos que se use **NEXT VALUE FOR** ... **OVER** (**ORDER BY** ...).  
  
-   En las siguientes cláusulas: **FETCH**, **OVER**, **OUTPUT**, **ON**, **PIVOT**, **UNPIVOT**, **GROUP BY**, **HAVING**, **COMPUTE**, **COMPUTE BY** o **FOR XML**.  
  
-   En expresiones condicionales que usan **CASE**, **CHOOSE**, **COALESCE**, **IIF**, **ISNULL** o **NULLIF**.  
  
-   En una cláusula **VALUES** que no forme parte de una instrucción **INSERT**.  
  
-   En la definición de una restricción CHECK.  
  
-   En la definición de una regla u objeto predeterminado. (Se puede utilizar en una restricción predeterminada).  
  
-   Como valor predeterminado de un tipo de tabla definido por el usuario.  
  
-   En una instrucción que use **TOP**, **OFFSET** o cuando se establece la opción **ROWCOUNT**.  
  
-   En la cláusula **WHERE** de una instrucción.  
  
-   En una instrucción **MERGE** (salvo cuando la función **NEXT VALUE FOR** se use en una restricción predeterminada en la tabla de destino y el valor predeterminado se use en la instrucción **CREATE** de la instrucción **MERGE**).  
  
## <a name="using-a-sequence-object-in-a-default-constraint"></a>Utilizar un objeto de secuencia en una restricción predeterminada  
 Al usar la función **NEXT VALUE FOR** en una restricción predeterminada, se aplican las siguientes reglas:  
  
-   Se puede hacer referencia a un objeto de secuencia único desde restricciones Default en varias tablas.  
  
-   La tabla y el objeto de secuencia deben residir en la misma base de datos.  
  
-   El usuario que agrega la restricción predeterminada debe tener el permiso REFERENCES en el objeto de secuencia.  
  
-   No se puede quitar un objeto de secuencia al que se haga referencia desde una restricción predeterminada antes de quitar la restricción predeterminada.  
  
-   Se devuelve el mismo número de secuencia para todas las columnas de una fila si varias restricciones Default utilizan el mismo objeto de secuencia, o si el mismo objeto de secuencia se utiliza en la instrucción que proporciona los valores y en una restricción Default que se esté ejecutando.  
  
-   Las referencias a la función **NEXT VALUE FOR** de una restricción predeterminada no pueden especificar la cláusula **OVER**.  
  
-   Se puede modificar un objeto de secuencia al que se haga referencia en una restricción predeterminada.  
  
-   En el caso de las instrucciones `INSERT ... SELECT` o `INSERT ... EXEC` en las que los datos que se van a insertar procedan de una consulta con una cláusula **ORDER BY**, los valores que devuelva la función **NEXT VALUE FOR** se generarán en el orden especificado por la cláusula **ORDER BY**.  
  
## <a name="using-a-sequence-object-with-an-over-order-by-clause"></a>Utilizar un objeto de secuencia con una cláusula OVER ORDER BY  
 La función **NEXT VALUE FOR** permite generar valores de secuencia ordenados aplicando la cláusula **OVER** a la llamada a **NEXT VALUE FOR**. Si usa la cláusula **OVER**, el usuario tiene la garantía de que los valores que se devuelven se generan en el orden de la subcláusula **ORDER BY** de la cláusula **OVER**. Al usar la función **NEXT VALUE FOR** con la cláusula **OVER**, se aplican las siguientes reglas adicionales:  
  
-   Si hay varias llamadas a la función **NEXT VALUE FOR** para el mismo generador de secuencias en una única instrucción, todas deben usar la misma definición de la cláusula **OVER**.  
  
-   Si hay varias llamadas a la función **NEXT VALUE FOR** que hacen referencia a distintos generadores de secuencias en una única instrucción, pueden tener distintas definiciones de la cláusula **OVER**.  
  
-   Una cláusula **OVER** aplicada a la función **NEXT VALUE FOR** no admite la subcláusula **PARTITION BY**.  
  
-   Si todas las llamadas a la función **NEXT VALUE FOR** de una instrucción **SELECT** especifican la cláusula **OVER**, se puede usar una cláusula **ORDER BY** en la instrucción **SELECT**.  
  
-   La cláusula **OVER** se admite con la función **NEXT VALUE FOR** si se usa en una instrucción **SELECT** o una instrucción `INSERT ... SELECT ...`. No se puede usar la cláusula **OVER** con la función **NEXT VALUE FOR** en las instrucciones **UPDATE** ni **MERGE**.  
  
-   Si otro proceso está teniendo acceso al objeto de secuencia a la vez, los números devueltos podrían tener lagunas.  
  
## <a name="metadata"></a>Metadatos  
 Para más información sobre las secuencias, consulte la vista de catálogo [sys.sequences](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 Requiere el permiso **UPDATE** en el objeto de secuencia o el esquema de la secuencia. Para obtener un ejemplo de concesión de permisos, vea el ejemplo F más adelante en este tema.  
  
### <a name="ownership-chaining"></a>Encadenamiento de propiedad  
 Los objetos de secuencia son compatibles con el encadenamiento de propiedad. Si el objeto de secuencia tiene el mismo propietario que el procedimiento almacenado, desencadenador o tabla que hace la llamada (y un objeto de secuencia como restricción predeterminada), no se necesita ninguna comprobación de permiso en el objeto de secuencia. Si el objeto de secuencia no pertenece al mismo propietario que el procedimiento almacenado, desencadenador o tabla que hace la llamada, se requiere una comprobación de permiso en el objeto de secuencia.  
  
 Cuando la función **NEXT VALUE FOR** se usa como valor predeterminado en una tabla, los usuarios deben tener el permiso **INSERT** en la tabla y el permiso **UPDATE** en el objeto de secuencia para insertar datos por medio del valor predeterminado.  
  
-   Si la restricción predeterminada tiene el mismo propietario que el objeto de secuencia, no es necesario ningún permiso en el objeto de secuencia cuando se llama a la restricción predeterminada.  
  
-   Si la restricción predeterminada y el objeto de secuencia no son propiedad del mismo usuario, se necesitan permisos en el objeto de secuencia aunque la llamada se haga mediante la restricción predeterminada.  
  
### <a name="audit"></a>Auditoría  
 Para auditar la función **NEXT VALUE FOR** supervise SCHEMA_OBJECT_ACCESS_GROUP.  
  
## <a name="examples"></a>Ejemplos  
 Para ver ejemplos de cómo crear secuencias y cómo usar la función **NEXT VALUE FOR** para generar números de secuencia, vea [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md).  
  
 En los siguientes ejemplos se utiliza una secuencia denominada `CountBy1` en un esquema denominado `Test`. Ejecute la siguiente instrucción para crear la secuencia `Test.CountBy1`. En los ejemplos C y E se utiliza la base de datos [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)], de modo que la secuencia `CountBy1` se cree en esa base de datos.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
CREATE SCHEMA Test;  
GO  
  
CREATE SEQUENCE Test.CountBy1  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
```  
  
### <a name="a-using-a-sequence-in-a-select-statement"></a>A. Usar una secuencia en una instrucción SELECT  
 En el siguiente ejemplo se crea una secuencia denominada `CountBy1` que aumenta en uno cada vez que se utiliza.  
  
```sql  
SELECT NEXT VALUE FOR Test.CountBy1 AS FirstUse;  
SELECT NEXT VALUE FOR Test.CountBy1 AS SecondUse;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
 ```
FirstUse  
1  
  
SecondUse  
2
```  
  
### <a name="b-setting-a-variable-to-the-next-sequence-value"></a>B. Establecer una variable en el siguiente valor de la secuencia  
 En el ejemplo siguiente se muestran tres maneras de establecer una variable en el siguiente valor de un número de secuencia.  
  
```sql  
DECLARE @myvar1 BIGINT = NEXT VALUE FOR Test.CountBy1  
DECLARE @myvar2 BIGINT ;  
DECLARE @myvar3 BIGINT ;  
SET @myvar2 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar3 = NEXT VALUE FOR Test.CountBy1 ;  
SELECT @myvar1 AS myvar1, @myvar2 AS myvar2, @myvar3 AS myvar3 ;  
GO  
```  
  
### <a name="c-using-a-sequence-with-a-ranking-window-function"></a>C. Usar una secuencia con una función de ventana de categoría  
  
```sql  
USE AdventureWorks2012 ;  
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 OVER (ORDER BY LastName) AS ListNumber,  
    FirstName, LastName  
FROM Person.Contact ;  
GO  
```  
  
### <a name="d-using-the-next-value-for-function-in-the-definition-of-a-default-constraint"></a>D. Usar la función NEXT VALUE FOR en la definición de una restricción predeterminada  
 La función **NEXT VALUE FOR** se puede usar en la definición de una restricción predeterminada. Para ver un ejemplo de uso de **NEXT VALUE FOR** en una instrucción **CREATE TABLE**, vea el ejemplo C de [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md). En el siguiente ejemplo se utiliza `ALTER TABLE` para agregar una secuencia como valor predeterminado a una tabla actual.  
  
```sql
CREATE TABLE Test.MyTable  
(  
    IDColumn NVARCHAR(25) PRIMARY KEY,  
    name VARCHAR(25) NOT NULL  
) ;  
GO  
  
CREATE SEQUENCE Test.CounterSeq  
    AS INT  
    START WITH 1  
    INCREMENT BY 1 ;  
GO  
  
ALTER TABLE Test.MyTable  
    ADD   
        DEFAULT N'AdvWorks_' +   
        CAST(NEXT VALUE FOR Test.CounterSeq AS NVARCHAR(20))   
        FOR IDColumn;  
GO  
  
INSERT Test.MyTable (name)  
VALUES ('Larry') ;  
GO  
  
SELECT * FROM Test.MyTable;  
GO  
```  
  
### <a name="e-using-the-next-value-for-function-in-an-insert-statement"></a>E. Usar la función NEXT VALUE FOR en una instrucción INSERT  
 En el siguiente ejemplo se crea una tabla denominada `TestTable` y, a continuación, se utiliza la función `NEXT VALUE FOR` para insertar una fila.  
  
```sql  
CREATE TABLE Test.TestTable  
     (CounterColumn INT PRIMARY KEY,  
    Name NVARCHAR(25) NOT NULL) ;   
GO  
  
INSERT Test.TestTable (CounterColumn,Name)  
    VALUES (NEXT VALUE FOR Test.CountBy1, 'Syed') ;  
GO  
  
SELECT * FROM Test.TestTable;   
GO  
```  
  
### <a name="e-using-the-next-value-for-function-with-select--into"></a>E. Usar la función NEXT VALUE FOR con SELECT ... INTO  
 En el siguiente ejemplo se usa la instrucción `SELECT ... INTO` para crear una tabla denominada `Production.NewLocation` y se usa la función`NEXT VALUE FOR` para numerar cada fila.  
  
```sql  
USE AdventureWorks2012 ;   
GO  
  
SELECT NEXT VALUE FOR Test.CountBy1 AS LocNumber, Name   
    INTO Production.NewLocation  
    FROM Production.Location ;  
GO  
  
SELECT * FROM Production.NewLocation ;  
GO  
```  
  
### <a name="f-granting-permission-to-execute-next-value-for"></a>F. Conceder permiso para ejecutar NEXT VALUE FOR  
 En el siguiente ejemplo se concede el permiso **UPDATE** a un usuario denominado `AdventureWorks\Larry` para ejecutar `NEXT VALUE FOR` con `Test.CounterSeq`.  
  
```sql  
GRANT UPDATE ON OBJECT::Test.CounterSeq TO [AdventureWorks\Larry] ;  
```  
  
## <a name="see-also"></a>Consulte también  
 [CREATE SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-sequence-transact-sql.md)   
 [ALTER SEQUENCE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-sequence-transact-sql.md)   
 [Números de secuencia](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
