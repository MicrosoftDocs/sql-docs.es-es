---
description: sp_executesql (Transact-SQL)
title: sp_executesql (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_executesql
- sp_executesql_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_executesql
- dynamic SQL
ms.assetid: a8d68d72-0f4d-4ecb-ae86-1235b962f646
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 333bc510861d769a8b9dbb22272918e65982ff2a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99188021"
---
# <a name="sp_executesql-transact-sql"></a>sp_executesql (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Ejecuta una instrucción o lote [!INCLUDE[tsql](../../includes/tsql-md.md)] que puede volver a utilizarse muchas veces o uno que se ha generado de forma dinámica. La instrucción o el lote [!INCLUDE[tsql](../../includes/tsql-md.md)] puede contener parámetros incrustados.  
  
> [!IMPORTANT]  
>  Las instrucciones [!INCLUDE[tsql](../../includes/tsql-md.md)] compiladas en tiempo de ejecución pueden exponer a las aplicaciones a ataques malintencionados.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```syntaxsql  
-- Syntax for SQL Server, Azure SQL Database, Azure Synapse Analytics, Parallel Data Warehouse  
  
sp_executesql [ @stmt = ] statement  
[   
  { , [ @params = ] N'@parameter_name data_type [ OUT | OUTPUT ][ ,...n ]' }   
     { , [ @param1 = ] 'value1' [ ,...n ] }  
]  
```  
  
## <a name="arguments"></a>Argumentos  
 [ \@ stmt =] ( *instrucción* )  
 Es una cadena Unicode que contiene una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o un lote. \@stmt debe ser una constante Unicode o una variable Unicode. No se permite utilizar expresiones Unicode más complejas, como una concatenación de dos cadenas con el operador +. Las constantes de caracteres no están permitidas. Si se especifica una constante Unicode, debe ir precedida de **N**. Por ejemplo, la constante Unicode **N ' sp_who '** es válida, pero la constante de caracteres **' sp_who '** no lo es. El tamaño de la cadena solo está limitado por la memoria disponible en el servidor de bases de datos. En los servidores de 64 bits, el tamaño de la cadena está limitado a 2 GB, el tamaño máximo de **nvarchar (Max)**.  
  
> [!NOTE]  
>  \@stmt puede contener parámetros que tengan el mismo formato que un nombre de variable, por ejemplo: `N'SELECT * FROM HumanResources.Employee WHERE EmployeeID = @IDParameter'`  
  
 Cada parámetro incluido en \@ stmt debe tener una entrada correspondiente en la \@ lista de definición de parámetros params y en la lista de valores de parámetro.  
  
 [ \@ params =] N ' \@ *parameter_name* *data_type* [,... *n* ] '  
 Es una cadena que contiene las definiciones de todos los parámetros que se han incrustado en \@ stmt. La cadena debe ser una constante Unicode o una variable Unicode. Cada definición de parámetro se compone de un nombre de parámetro y un tipo de datos. *n* es un marcador de posición que indica definiciones de parámetros adicionales. Todos los parámetros especificados en \@ stmt deben definirse en \@ params. Si la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote de \@ stmt no contiene parámetros, \@ no es necesario realizar ningún parámetro. El valor predeterminado de este parámetro es NULL.  
  
 [ \@ parám1 =] '*value1*'  
 Es un valor para el primer parámetro definido en la cadena de parámetros. El valor puede ser una constante Unicode o una variable Unicode. Debe haber un valor de parámetro proporcionado para cada parámetro incluido en \@ stmt. Los valores no son necesarios cuando la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote en \@ stmt no tiene parámetros.  
  
 [ OUT | OUTPUT ]  
 Indica que se trata de un parámetro de salida. los parámetros **Text**, **ntext** e **Image** se pueden usar como parámetros OUTPUT, a menos que el procedimiento sea un procedimiento Common Language Runtime (CLR). Un parámetro de salida que utilice la palabra clave OUTPUT puede ser un marcador de posición de cursor, a menos que el procedimiento sea un procedimiento CLR.  
  
 *n*  
 Es un marcador de posición para los valores de los parámetros adicionales. Los valores solo pueden ser constantes o variables. Los valores no pueden ser expresiones más complejas como funciones ni expresiones generadas mediante operadores.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 0 (correcto) o distinto de cero (error)  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Devuelve los conjuntos de resultados de todas las instrucciones SQL integradas en la cadena SQL.  
  
## <a name="remarks"></a>Observaciones  
 los parámetros de sp_executesql deben especificarse en el orden específico, tal y como se describe en la sección "sintaxis", anteriormente en este tema. Si los parámetros se escriben desordenados, se producirá un mensaje de error.  
  
 sp_executesql tiene el mismo comportamiento que EXECUTE en cuanto a los lotes, el ámbito de los nombres y el contexto de las bases de datos. La [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote del \@ parámetro sp_executesql stmt no se compila hasta que se ejecuta la instrucción sp_executesql. El contenido de \@ stmt se compila y se ejecuta como un plan de ejecución independiente del plan de ejecución del lote que llamó a sp_executesql. El lote de sp_executesql no puede hacer referencia a las variables declaradas en el lote que llama a sp_executesql. Los cursores o las variables locales del lote de sp_executesql no son visibles para el lote que llama a sp_executesql. Los cambios en el contexto de base de datos solo se mantienen hasta el final de la instrucción sp_executesql.  
  
 sp_executesql puede utilizarse como alternativa a los procedimientos almacenados para ejecutar varias veces una instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] si la única variación es que cambian los valores de los parámetros de la instrucción. Al permanecer constante la propia instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] y variar solo los valores de los parámetros, es probable que el optimizador de consultas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vuelva a utilizar el plan de ejecución que genera para la primera ejecución.  
  
> [!NOTE]  
>  Para mejorar el rendimiento, utilice nombres de objeto completos en la cadena de la instrucción.  
  
 sp_executesql permite establecer los valores de los parámetros independientemente de la cadena [!INCLUDE[tsql](../../includes/tsql-md.md)], como se muestra en el siguiente ejemplo.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
  
/* Build the SQL string one time.*/  
SET @SQLString =  
     N'SELECT BusinessEntityID, NationalIDNumber, JobTitle, LoginID  
       FROM AdventureWorks2012.HumanResources.Employee   
       WHERE BusinessEntityID = @BusinessEntityID';  
SET @ParmDefinition = N'@BusinessEntityID tinyint';  
/* Execute the string with the first parameter value. */  
SET @IntVariable = 197;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
/* Execute the same string with the second parameter value. */  
SET @IntVariable = 109;  
EXECUTE sp_executesql @SQLString, @ParmDefinition,  
                      @BusinessEntityID = @IntVariable;  
```  
  
 Los parámetros de salida también se pueden utilizar con sp_executesql. En el ejemplo siguiente, se recupera un puesto de trabajo de la tabla `AdventureWorks2012.HumanResources.Employee` y se devuelve en el parámetro de salida `@max_title`.  
  
```sql  
DECLARE @IntVariable INT;  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @max_title VARCHAR(30);  
  
SET @IntVariable = 197;  
SET @SQLString = N'SELECT @max_titleOUT = max(JobTitle)   
   FROM AdventureWorks2012.HumanResources.Employee  
   WHERE BusinessEntityID = @level';  
SET @ParmDefinition = N'@level TINYINT, @max_titleOUT VARCHAR(30) OUTPUT';  
  
EXECUTE sp_executesql @SQLString, @ParmDefinition, @level = @IntVariable, @max_titleOUT=@max_title OUTPUT;  
SELECT @max_title;  
```  
  
 La posibilidad de sustituir los parámetros de sp_executesql ofrece las siguientes ventajas con respecto al uso de la instrucción EXECUTE para ejecutar una cadena:  
  
-   Debido a que el texto real de la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de la cadena de sp_executesql no cambia entre ejecuciones, es posible que el optimizador de consultas utilice la instrucción [!INCLUDE[tsql](../../includes/tsql-md.md)] de la segunda ejecución con el plan de ejecución generado en la primera. De este modo, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no tiene que compilar la segunda instrucción.  
  
-   La cadena de [!INCLUDE[tsql](../../includes/tsql-md.md)] solo se genera una vez.  
  
-   El parámetro de tipo integer se especifica en su formato nativo. No es necesaria la conversión a Unicode.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol public.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-executing-a-simple-select-statement"></a>A. Ejecutar una instrucción SELECT simple  
 En el siguiente ejemplo se crea y se ejecuta una instrucción `SELECT` simple que contiene un parámetro incrustado denominado `@level`.  
  
```sql  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorks2012.HumanResources.Employee   
          WHERE BusinessEntityID = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
### <a name="b-executing-a-dynamically-built-string"></a>B. Ejecutar una cadena generada de forma dinámica  
 En el siguiente ejemplo se muestra el uso de `sp_executesql` para ejecutar una cadena generada de forma dinámica. El procedimiento almacenado de ejemplo se utiliza para insertar datos en un conjunto de tablas empleado para dividir los datos de ventas de un año. Hay una tabla por cada mes del año, que tiene el formato siguiente:  
  
```sql  
CREATE TABLE May1998Sales  
    (OrderID INT PRIMARY KEY,  
    CustomerID INT NOT NULL,  
    OrderDate  DATETIME NULL  
        CHECK (DATEPART(yy, OrderDate) = 1998),  
    OrderMonth INT  
        CHECK (OrderMonth = 5),  
    DeliveryDate DATETIME NULL,  
        CHECK (DATEPART(mm, OrderDate) = OrderMonth)  
    )  
```  
  
 Este procedimiento almacenado de ejemplo genera y ejecuta de forma dinámica una instrucción `INSERT` para insertar pedidos nuevos en la tabla que corresponda. En el ejemplo se utiliza la fecha de pedido para crear el nombre de la tabla que debe contener los datos y, a continuación, incorpora ese nombre a una instrucción `INSERT`.  
  
> [!NOTE]  
>  Éste es un ejemplo simple de sp_executesql. El ejemplo no contiene comprobación de errores ni incluye comprobaciones de reglas de negocios como, por ejemplo, garantizar que los números de pedido no se repitan en otras tablas.  
  
```sql  
CREATE PROCEDURE InsertSales @PrmOrderID INT, @PrmCustomerID INT,  
                 @PrmOrderDate DATETIME, @PrmDeliveryDate DATETIME  
AS  
DECLARE @InsertString NVARCHAR(500)  
DECLARE @OrderMonth INT  
  
-- Build the INSERT statement.  
SET @InsertString = 'INSERT INTO ' +  
       /* Build the name of the table. */  
       SUBSTRING( DATENAME(mm, @PrmOrderDate), 1, 3) +  
       CAST(DATEPART(yy, @PrmOrderDate) AS CHAR(4) ) +  
       'Sales' +  
       /* Build a VALUES clause. */  
       ' VALUES (@InsOrderID, @InsCustID, @InsOrdDate,' +  
       ' @InsOrdMonth, @InsDelDate)'  
  
/* Set the value to use for the order month because  
   functions are not allowed in the sp_executesql parameter  
   list. */  
SET @OrderMonth = DATEPART(mm, @PrmOrderDate)  
  
EXEC sp_executesql @InsertString,  
     N'@InsOrderID INT, @InsCustID INT, @InsOrdDate DATETIME,  
       @InsOrdMonth INT, @InsDelDate DATETIME',  
     @PrmOrderID, @PrmCustomerID, @PrmOrderDate,  
     @OrderMonth, @PrmDeliveryDate  
  
GO  
```  
  
 En este procedimiento, el uso de sp_executesql resulta más eficaz que el de EXECUTE para ejecutar una cadena. Cuando se usa sp_executesql, solo hay doce versiones de la cadena INSERT generada, una por cada tabla mensual. Con EXECUTE, cada cadena INSERT es única, ya que los valores de los parámetros son distintos. Aunque ambos métodos generan el mismo número de lotes, la semejanza de las cadenas INSERT que genera sp_executesql hace más probable que el optimizador de consultas vuelva a utilizar los planes de ejecución.  
  
### <a name="c-using-the-output-parameter"></a>C. Utilizar el parámetro OUTPUT  
 En el ejemplo siguiente se utiliza un `OUTPUT` parámetro para almacenar el conjunto de resultados generado por la `SELECT` instrucción en el `@SQLString` parámetro. `SELECT` A continuación, se ejecutan dos instrucciones que utilizan el valor del `OUTPUT` parámetro.  
  
```sql  
USE AdventureWorks2012;  
GO  
DECLARE @SQLString NVARCHAR(500);  
DECLARE @ParmDefinition NVARCHAR(500);  
DECLARE @SalesOrderNumber NVARCHAR(25);  
DECLARE @IntVariable INT;  
SET @SQLString = N'SELECT @SalesOrderOUT = MAX(SalesOrderNumber)  
    FROM Sales.SalesOrderHeader  
    WHERE CustomerID = @CustomerID';  
SET @ParmDefinition = N'@CustomerID INT,  
    @SalesOrderOUT NVARCHAR(25) OUTPUT';  
SET @IntVariable = 22276;  
EXECUTE sp_executesql  
    @SQLString  
    ,@ParmDefinition  
    ,@CustomerID = @IntVariable  
    ,@SalesOrderOUT = @SalesOrderNumber OUTPUT;  
-- This SELECT statement returns the value of the OUTPUT parameter.  
SELECT @SalesOrderNumber;  
-- This SELECT statement uses the value of the OUTPUT parameter in  
-- the WHERE clause.  
SELECT OrderDate, TotalDue  
FROM Sales.SalesOrderHeader  
WHERE SalesOrderNumber = @SalesOrderNumber;  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Ejemplos: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="d-executing-a-simple-select-statement"></a>D. Ejecutar una instrucción SELECT simple  
 En el siguiente ejemplo se crea y se ejecuta una instrucción `SELECT` simple que contiene un parámetro incrustado denominado `@level`.  
  
```sql  
-- Uses AdventureWorks  
  
EXECUTE sp_executesql   
          N'SELECT * FROM AdventureWorksPDW2012.dbo.DimEmployee   
          WHERE EmployeeKey = @level',  
          N'@level TINYINT',  
          @level = 109;  
```  
  
## <a name="see-also"></a>Consulte también  
 [EXECUTE &#40;Transact-SQL&#41;](../../t-sql/language-elements/execute-transact-sql.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
