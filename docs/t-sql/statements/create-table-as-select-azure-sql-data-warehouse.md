---
description: CREATE TABLE AS SELECT (Azure Synapse Analytics)
title: CREATE TABLE AS SELECT (Azure Synapse Analytics) | Microsoft Docs
ms.custom: ''
ms.date: 10/07/2016
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: d1e08f88-64ef-4001-8a66-372249df2533
author: julieMSFT
ms.author: jrasnick
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 6af11a4ffc62f8311f31cf378c9a65f67ae58a2b
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186552"
---
# <a name="create-table-as-select-azure-synapse-analytics"></a>CREATE TABLE AS SELECT (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

CREATE TABLE AS SELECT (CTAS) es una de las características más importantes de T-SQL disponibles. Se trata de una operación de ejecución completamente en paralelo que crea una tabla basada en la salida de una instrucción SELECT. CTAS es la forma más sencilla y rápida de crear una copia de una tabla.   
 
 Por ejemplo, use CTAS para lo siguiente:  
  
-   Volver a crear una tabla con una columna de distribución de hash diferente.
-   Volver a crear una tabla como replicada.   
-   Crear un índice de almacén de columnas en solo algunas columnas de la tabla.  
-   Consultar o importar datos externos.  

> [!NOTE]  
> Dado que CTAS se agrega a las funcionalidades para crear tablas, en este tema se procura no repetir el tema CREATE TABLE. En su lugar, se describen las diferencias entre las instrucciones CTAS y CREATE TABLE. Para obtener todos los detalles sobre CREATE TABLE, vea la instrucción [CREATE TABLE (Azure Synapse Analytics)](./create-table-azure-sql-data-warehouse.md). 
  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
<a name="syntax-bk"></a>

## <a name="syntax"></a>Sintaxis   

```syntaxsql
CREATE TABLE { database_name.schema_name.table_name | schema_name.table_name | table_name }
    [ ( column_name [ ,...n ] ) ]  
    WITH ( 
      <distribution_option> -- required
      [ , <table_option> [ ,...n ] ]    
    )  
    AS <select_statement>  
    OPTION <query_hint> 
[;]  

<distribution_option> ::=
    { 
        DISTRIBUTION = HASH ( distribution_column_name ) 
      | DISTRIBUTION = ROUND_ROBIN 
      | DISTRIBUTION = REPLICATE
    }   

<table_option> ::= 
    {   
        CLUSTERED COLUMNSTORE INDEX --default for Synapse Analytics 
      | CLUSTERED COLUMNSTORE INDEX ORDER (column[,...n])
      | HEAP --default for Parallel Data Warehouse   
      | CLUSTERED INDEX ( { index_column_name [ ASC | DESC ] } [ ,...n ] ) --default is ASC 
    }  
      | PARTITION ( partition_column_name RANGE [ LEFT | RIGHT ] --default is LEFT  
        FOR VALUES ( [ boundary_value [,...n] ] ) ) 
  
<select_statement> ::=  
    [ WITH <common_table_expression> [ ,...n ] ]  
    SELECT select_criteria  

<query_hint> ::=
    {
        MAXDOP 
    }
```  

<a name="arguments-bk"></a>
  
## <a name="arguments"></a>Argumentos  
Para más información, vea la sección [Argumentos](./create-table-azure-sql-data-warehouse.md#Arguments) de CREATE TABLE.  

<a name="column-options-bk"></a>

### <a name="column-options"></a>Opciones de columna
`column_name` [ ,...`n` ]   
 Los nombres de columna no admiten las [opciones de columna](./create-table-azure-sql-data-warehouse.md#ColumnOptions) mencionadas en CREATE TABLE.  En su lugar, puede proporcionar una lista opcional de uno o más nombres de columna para la nueva tabla. Las columnas de la nueva tabla usarán los nombres que especifique. Al especificar nombres de columna, el número de columnas de la lista de columnas debe coincidir con el número de columnas de los resultados de selección. Si no especifica nombres de columna, la nueva tabla de destino usará los nombres de columna de los resultados de la instrucción de selección. 
  
 No se pueden especificar otras opciones de columna, como tipos de datos, intercalación o la nulabilidad. Cada uno de estos atributos se deriva de los resultados de la instrucción `SELECT`. Aun así, puede usar la instrucción SELECT para cambiar los atributos. Para obtener un ejemplo, vea [Usar CTAS para cambiar los atributos de columna](#ctas-change-column-attributes-bk).   

<a name="table-distribution-options-bk"></a>

### <a name="table-distribution-options"></a>Opciones de distribución de tabla

`DISTRIBUTION` = `HASH` ( *distribution_column_name* ) | ROUND_ROBIN | REPLICATE      
La instrucción CTAS requiere una opción de distribución y no tiene valores predeterminados. Es diferente de CREATE TABLE, que tiene valores predeterminados. 

Para obtener más información y entender cómo elegir la mejor columna de distribución, vea la sección [Opciones de distribución de tabla](./create-table-azure-sql-data-warehouse.md#TableDistributionOptions) en CREATE TABLE. 

<a name="table-partition-options-bk"></a>

### <a name="table-partition-options"></a>Opciones de partición de tabla
La instrucción CTAS crea una tabla sin particiones de forma predeterminada, incluso si la tabla de origen tiene particiones. Para crear una tabla con particiones con la instrucción CTAS, debe especificar la opción de partición. 

Para más información, vea la sección [Opciones de partición de tabla](./create-table-azure-sql-data-warehouse.md#TablePartitionOptions) en CREATE TABLE.

<a name="select-options-bk"></a>

### <a name="select-statement"></a>Select, instrucción
La instrucción de selección es la diferencia fundamental entre CTAS y CREATE TABLE.  

 `WITH` *common_table_expression*  
 Especifica un conjunto de resultados temporal con nombre, conocido como expresión de tabla común (CTE). Para más información, vea [WITH common_table_expression &#40;Transact-SQL&#41;](../../t-sql/queries/with-common-table-expression-transact-sql.md).  
  
 `SELECT` *select_criteria*  
 Rellena la nueva tabla con los resultados de una instrucción SELECT. *select_criteria* es el cuerpo de la instrucción SELECT que determina qué datos se copian en la nueva tabla. Para más información sobre las instrucciones SELECT, vea [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md).  
 
### <a name="query-hint"></a>Sugerencia de consulta
Los usuarios pueden establecer MAXDOP en un valor entero para controlar el grado máximo de paralelismo.  Cuando MAXDOP se establece en 1, la consulta se ejecuta en un único subproceso.

 
<a name="permissions-bk"></a>  
  
## <a name="permissions"></a>Permisos  
CTAS requiere el permiso `SELECT` en todos los objetos a los que hace referencia en *select_criteria*.

Para conocer los permisos para crear una tabla, vea [Permisos](./create-table-azure-sql-data-warehouse.md#Permissions) en CREATE TABLE. 
  
<a name="general-remarks-bk"></a>
  
## <a name="general-remarks"></a>Notas generales
Para más información, vea [Notas generales](./create-table-azure-sql-data-warehouse.md#GeneralRemarks) en CREATE TABLE.

<a name="limitations-bk"></a>

## <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  

Se puede crear un índice de almacén de columnas agrupado y ordenado en columnas de cualquier tipo de datos que se admita en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], excepto en columnas de cadena.  

[SET ROWCOUNT &#40;Transact-SQL&#41;](../../t-sql/statements/set-rowcount-transact-sql.md) no tiene ningún efecto en CTAS. Para lograr un comportamiento similar, use [TOP &#40;Transact-SQL&#41;](../../t-sql/queries/top-transact-sql.md).  
 
Para más información, vea [Limitaciones y restricciones](./create-table-azure-sql-data-warehouse.md#LimitationsRestrictions) en CREATE TABLE.

<a name="locking-behavior-bk"></a>
  
## <a name="locking-behavior"></a>Comportamiento del bloqueo  
 Para más información, vea [Comportamiento de bloqueo](./create-table-azure-sql-data-warehouse.md#LockingBehavior) en CREATE TABLE.
 
<a name="performance-bk"></a>
 
 ## <a name="performance"></a>Rendimiento 

Para una tabla distribuida de hash, puede usar CTAS para seleccionar una columna de distribución diferente a fin de obtener un mejor rendimiento de las combinaciones y las agregaciones. Si su objetivo no es seleccionar una columna de distribución diferente, tendrá el mejor rendimiento de CTAS si especifica la misma columna de distribución, ya que de este modo evitará que se vuelvan a distribuir las filas. 

Si usa CTAS para crear una tabla y el rendimiento no es un factor importante, puede especificar `ROUND_ROBIN` para evitar tener que elegir una columna de distribución.

Para evitar el movimiento de datos en las consultas posteriores, puede especificar `REPLICATE` a costa de que aumente el almacenamiento para cargar una copia completa de la tabla en cada nodo de ejecución.  


<a name="examples-copy-table-bk"></a>

## <a name="examples-for-copying-a-table"></a>Ejemplos para copiar una tabla

<a name="ctas-copy-table-bk"></a>

### <a name="a-use-ctas-to-copy-a-table"></a>A. Uso de CTAS para copiar una tabla 
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

Quizás uno de los usos más comunes de `CTAS` es crear una copia de una tabla para poder cambiar el DDL. Si, por ejemplo, creó inicialmente la tabla como `ROUND_ROBIN` y ahora quiere cambiarla a una tabla distribuida en una columna, cambiaría la columna de distribución con `CTAS`. `CTAS` también se puede usar para cambiar los tipos de particiones, indexación o columna.

Supongamos que ha creado esta tabla con el tipo de distribución predeterminado de `ROUND_ROBIN` distribuido, ya que no se ha especificado ninguna columna de distribución en `CREATE TABLE`.

```sql
CREATE TABLE FactInternetSales
(
    ProductKey INT NOT NULL,
    OrderDateKey INT NOT NULL,
    DueDateKey INT NOT NULL,
    ShipDateKey INT NOT NULL,
    CustomerKey INT NOT NULL,
    PromotionKey INT NOT NULL,
    CurrencyKey INT NOT NULL,
    SalesTerritoryKey INT NOT NULL,
    SalesOrderNumber NVARCHAR(20) NOT NULL,
    SalesOrderLineNumber TINYINT NOT NULL,
    RevisionNumber TINYINT NOT NULL,
    OrderQuantity SMALLINT NOT NULL,
    UnitPrice MONEY NOT NULL,
    ExtendedAmount MONEY NOT NULL,
    UnitPriceDiscountPct FLOAT NOT NULL,
    DiscountAmount FLOAT NOT NULL,
    ProductStandardCost MONEY NOT NULL,
    TotalProductCost MONEY NOT NULL,
    SalesAmount MONEY NOT NULL,
    TaxAmt MONEY NOT NULL,
    Freight MONEY NOT NULL,
    CarrierTrackingNumber NVARCHAR(25),
    CustomerPONumber NVARCHAR(25)
);
```

Ahora quiere crear una copia de esta tabla con un índice de almacén de columnas agrupado para poder aprovechar el rendimiento de las tablas del almacén de columnas agrupado. También quiere distribuir esta tabla en ProductKey, ya que prevé que se producirán combinaciones con esta columna y le interesa impedir el movimiento de datos durante las combinaciones en ProductKey. Por último, también quiere agregar la creación de particiones en OrderDateKey para poder eliminar rápidamente datos antiguos mediante la anulación de particiones antiguas. A continuación se muestra la instrucción CTAS que copiaría la tabla antigua en una nueva tabla.

```sql
CREATE TABLE FactInternetSales_new
WITH
(
    CLUSTERED COLUMNSTORE INDEX,
    DISTRIBUTION = HASH(ProductKey),
    PARTITION
    (
        OrderDateKey RANGE RIGHT FOR VALUES
        (
        20000101,20010101,20020101,20030101,20040101,20050101,20060101,20070101,20080101,20090101,
        20100101,20110101,20120101,20130101,20140101,20150101,20160101,20170101,20180101,20190101,
        20200101,20210101,20220101,20230101,20240101,20250101,20260101,20270101,20280101,20290101
        )
    )
)
AS SELECT * FROM FactInternetSales;
```

Por último, puede cambiar el nombre de las tablas para intercambiar la tabla nueva y, después, anular la tabla antigua.

```sql
RENAME OBJECT FactInternetSales TO FactInternetSales_old;
RENAME OBJECT FactInternetSales_new TO FactInternetSales;

DROP TABLE FactInternetSales_old;
```

<a name="examples-column-bk"></a>

## <a name="examples-for-column-options"></a>Ejemplos de opciones de columna

<a name="ctas-change-column-attributes-bk"></a>

### <a name="b-use-ctas-to-change-column-attributes"></a>B. Usar CTAS para cambiar los atributos de columna 
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En este ejemplo se usa CTAS para cambiar los tipos de datos, la nulabilidad y la intercalación de varias columnas en la tabla DimCustomer2.  
  
```sql  
-- Original table 
CREATE TABLE [dbo].[DimCustomer2] (  
    [CustomerKey] INT NOT NULL,  
    [GeographyKey] INT NULL,  
    [CustomerAlternateKey] nvarchar(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL  
)  
WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = HASH([CustomerKey]));  
  
-- CTAS example to change data types, nullability, and column collations  
CREATE TABLE test  
WITH (HEAP, DISTRIBUTION = ROUND_ROBIN)  
AS  
SELECT  
    CustomerKey AS CustomerKeyNoChange,  
    CustomerKey*1 AS CustomerKeyChangeNullable,  
    CAST(CustomerKey AS DECIMAL(10,2)) AS CustomerKeyChangeDataTypeNullable,  
    ISNULL(CAST(CustomerKey AS DECIMAL(10,2)),0) AS CustomerKeyChangeDataTypeNotNullable,  
    GeographyKey AS GeographyKeyNoChange,  
    ISNULL(GeographyKey,0) AS GeographyKeyChangeNotNullable,  
    CustomerAlternateKey AS CustomerAlternateKeyNoChange,  
    CASE WHEN CustomerAlternateKey = CustomerAlternateKey 
        THEN CustomerAlternateKey END AS CustomerAlternateKeyNullable,  
    CustomerAlternateKey COLLATE Latin1_General_CS_AS_KS_WS AS CustomerAlternateKeyChangeCollation  
FROM [dbo].[DimCustomer2]  
  
-- Resulting table 
CREATE TABLE [dbo].[test] (
    [CustomerKeyNoChange] INT NOT NULL, 
    [CustomerKeyChangeNullable] INT NULL, 
    [CustomerKeyChangeDataTypeNullable] DECIMAL(10, 2) NULL, 
    [CustomerKeyChangeDataTypeNotNullable] DECIMAL(10, 2) NOT NULL, 
    [GeographyKeyNoChange] INT NULL, 
    [GeographyKeyChangeNotNullable] INT NOT NULL, 
    [CustomerAlternateKeyNoChange] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NOT NULL, 
    [CustomerAlternateKeyNullable] NVARCHAR(15) COLLATE SQL_Latin1_General_CP1_CI_AS NULL, 
    [CustomerAlternateKeyChangeCollation] NVARCHAR(15) COLLATE Latin1_General_CS_AS_KS_WS NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN);
```
 
Como paso final, puede usar [RENAME &#40;Transact-SQL&#41;](../../t-sql/statements/rename-transact-sql.md) para cambiar los nombres de tabla. Esto hace que DimCustomer2 sea la nueva tabla.

```sql
RENAME OBJECT DimCustomer2 TO DimCustomer2_old;
RENAME OBJECT test TO DimCustomer2;

DROP TABLE DimCustomer2_old;
```

<a name="examples-table-distribution-bk"></a>

## <a name="examples-for-table-distribution"></a>Ejemplos de distribución de la tabla

<a name="ctas-change-distribution-method-bk"></a>

### <a name="c-use-ctas-to-change-the-distribution-method-for-a-table"></a>C. Usar CTAS para cambiar el método de distribución de una tabla
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En este sencillo ejemplo se muestra cómo cambiar el método de distribución de una tabla. Para mostrar la mecánica de cómo se hace, cambia una tabla distribuida de hash a round robin y, después, cambia la tabla round robin de nuevo a distribuida de hash. La tabla final coincide con la tabla original. 

En la mayoría de los casos no es necesario convertir una tabla distribuida de hash en una tabla round robin. A menudo, deberá cambiar una tabla round robin a una tabla distribuida de hash. Por ejemplo, es posible que cargue inicialmente una tabla nueva como round robin y, después, la mueva a una tabla distribuida de hash para obtener un mejor rendimiento de combinación.

En este ejemplo se usa la base de datos de ejemplo AdventureWorksDW. Para cargar la versión [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], vea [Carga de datos de ejemplo en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)]](https://azure.microsoft.com/documentation/articles/sql-data-warehouse-load-sample-databases/).
 
```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a round-robin table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  
Después, cámbiela de nuevo a una tabla distribuida de hash.

```sql
-- You just made DimSalesTerritory a round-robin table.
-- Change it back to the original hash-distributed table. 
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH(SalesTerritoryKey) 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
<a name="ctas-change-to-replicated-bk"></a>

### <a name="d-use-ctas-to-convert-a-table-to-a-replicated-table"></a>D. Usar CTAS para convertir una tabla en una tabla replicada  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En este ejemplo se convierten tablas round robin o distribuidas de hash en una tabla replicada. En este ejemplo en concreto, el método anterior de cambiar el tipo de distribución va un paso más allá.  Dado que DimSalesTerritory es una dimensión y es probable que sea una tabla menor, puede volver a crear la tabla como replicada para evitar el movimiento de datos al combinarla con otras tablas. 

```sql
-- DimSalesTerritory is hash-distributed.
-- Copy it to a replicated table.
CREATE TABLE [dbo].[myTable]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE 
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]; 

-- Switch table names

RENAME OBJECT [dbo].[DimSalesTerritory] to [DimSalesTerritory_old];
RENAME OBJECT [dbo].[myTable] TO [DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```
 
### <a name="e-use-ctas-to-create-a-table-with-fewer-columns"></a>E. Usar CTAS para crear una tabla con menos columnas
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

En el ejemplo siguiente se crea una tabla distribuida round robin denominada `myTable (c, ln)`. La nueva tabla solo tiene dos columnas. Usa los alias de columna en la instrucción SELECT para los nombres de las columnas.  
  
```sql  
CREATE TABLE myTable  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN  
  )  
AS SELECT CustomerKey AS c, LastName AS ln  
    FROM dimCustomer; 
```  

<a name="examples-query-hints-bk"></a>

## <a name="examples-for-query-hints"></a>Ejemplos de sugerencias de consulta

<a name="ctas-query-hint-bk"></a>

### <a name="f-use-a-query-hint-with-create-table-as-select-ctas"></a>F. Usar una sugerencia de consulta con CREATE TABLE AS SELECT (CTAS)  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]
  
Esta consulta muestra la sintaxis básica para usar una sugerencia de combinación de consulta con la instrucción CTAS. Una vez enviada la consulta, [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] aplica la estrategia de combinación hash al generar el plan de consulta para cada distribución individual. Para más información sobre la sugerencia de consulta de combinación hash, vea [Cláusula OPTION &#40;Transact-SQL&#41;](../../t-sql/queries/option-clause-transact-sql.md).  
  
```sql  
CREATE TABLE dbo.FactInternetSalesNew  
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = ROUND_ROBIN   
  )  
AS SELECT T1.* FROM dbo.FactInternetSales T1 JOIN dbo.DimCustomer T2  
ON ( T1.CustomerKey = T2.CustomerKey )  
OPTION ( HASH JOIN );  
```  

<a name="examples-external-tables"></a>

## <a name="examples-for-external-tables"></a>Ejemplos de tablas externas

<a name="ctas-azure-blob-storage-bk"></a>

### <a name="g-use-ctas-to-import-data-from-azure-blob-storage"></a>G. Usar CTAS para importar datos de Azure Blob Storage  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Para importar datos de una tabla externa, basta con que use CREATE TABLE AS SELECT para hacer una selección en la tabla externa. La sintaxis para seleccionar datos de una tabla externa en [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] es igual que la sintaxis para seleccionar datos de una tabla normal.  
  
 En el ejemplo siguiente se define una tabla externa en los datos de una cuenta de Azure Blob Storage. Después, se usa CREATE TABLE AS SELECT para hacer una selección en la tabla externa. De este modo se importan los datos de los archivos delimitados de texto de Azure Blob Storage y se almacenan los datos en una nueva tabla de [!INCLUDE[ssSDW](../../includes/sssdw-md.md)].  
  
```sql  
--Use your own processes to create the text-delimited files on Azure blob storage.  
--Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION='/logs/clickstream/2015/',  
    DATA_SOURCE = MyAzureStorage,  
    FILE_FORMAT = TextFileFormat)  
;  
  
--Use CREATE TABLE AS SELECT to import the Azure blob storage data into a new   
--Synapse Analytics table called ClickStreamData  
CREATE TABLE ClickStreamData   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;  
```  

<a name="ctas-import-Hadoop-bk"></a>
  
### <a name="h-use-ctas-to-import-hadoop-data-from-an-external-table"></a>H. Usar CTAS para importar datos de Hadoop de una tabla externa  
Se aplica a: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
Para importar datos de una tabla externa, basta con que use CREATE TABLE AS SELECT para hacer una selección en la tabla externa. La sintaxis para seleccionar datos de una tabla externa en [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] es igual que la sintaxis para seleccionar datos de una tabla normal.  
  
 En el ejemplo siguiente se define una tabla externa en un clúster de Hadoop. Después, se usa CREATE TABLE AS SELECT para hacer una selección en la tabla externa. De este modo se importan los datos de los archivos delimitados de texto de Hadoop y se almacenan los datos en una nueva tabla de [!INCLUDE[ssPDW](../../includes/sspdw-md.md)].  
  
```sql  
-- Create the external table called ClickStream.  
CREATE EXTERNAL TABLE ClickStreamExt (   
    url VARCHAR(50),  
    event_date DATE,  
    user_IP VARCHAR(50)  
)  
WITH (  
    LOCATION = 'hdfs://MyHadoop:5000/tpch1GB/employee.tbl',  
    FORMAT_OPTIONS ( FIELD_TERMINATOR = '|')  
)  
;  
  
-- Use your own processes to create the Hadoop text-delimited files 
-- on the Hadoop Cluster.  
  
-- Use CREATE TABLE AS SELECT to import the Hadoop data into a new 
-- table called ClickStreamPDW  
CREATE TABLE ClickStreamPDW   
WITH  
  (  
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = HASH (user_IP)  
  )  
AS SELECT * FROM ClickStreamExt  
;   
```  
 
<a name="examples-workarounds-bk"></a>
 
## <a name="examples-using-ctas-to-replace-sql-server-code"></a>Ejemplos del uso de CTAS para reemplazar código de SQL Server

Use CTAS para hallar soluciones temporales para algunas características no admitidas. Además de permitirle ejecutar el código en el almacén de datos, la reescritura del código existente para usar CTAS suele mejorar el rendimiento. Esto se debe a su diseño totalmente paralelizado. 

> [!NOTE]
> Intente dar prioridad a CTAS. Lo más recomendable es resolver los problemas mediante `CTAS`, incluso si como resultado debe escribir más datos.
>

<a name="ctas-replace-select-into-bk"></a>

### <a name="i-use-ctas-instead-of-selectinto"></a>I. Usar CTAS en lugar de SELECT..EN  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]

El código de SQL Server suele usar SELECT..INTO para rellenar una tabla con los resultados de una instrucción SELECT. Este es un ejemplo de una instrucción SELECT..INTO de SQL Server.

```sql
SELECT *
INTO    #tmp_fct
FROM    [dbo].[FactInternetSales]
```

Esta sintaxis no se admite en [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] ni en Almacenamiento de datos paralelos. En este ejemplo se muestra cómo reescribir la anterior instrucción SELECT..INTO como una instrucción CTAS. Puede elegir cualquiera de las opciones DISTRIBUTION que se describen en la sintaxis CTAS. En este ejemplo se usa el método de distribución ROUND_ROBIN.

```sql
CREATE TABLE #tmp_fct
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
AS
SELECT  *
FROM    [dbo].[FactInternetSales]
;
```

<a name="ctas-replace-implicit-joins-bk"></a>

### <a name="j-use-ctas-and-implicit-joins-to-replace-ansi-joins-in-the-from-clause-of-an-update-statement"></a>J. Usar CTAS y combinaciones implícitas para reemplazar combinaciones ANSI en la cláusula `FROM` de una instrucción `UPDATE`  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Es posible que se encuentre con una actualización compleja que combina más de dos tablas mediante la sintaxis de combinación de ANSI para llevar a cabo UPDATE o DELETE.

Imagine que tiene que actualizar esta tabla:

```sql
CREATE TABLE [dbo].[AnnualCategorySales]
(   [EnglishProductCategoryName]    NVARCHAR(50)    NOT NULL
,   [CalendarYear]          SMALLINT    NOT NULL
,   [TotalSalesAmount]      MONEY       NOT NULL
)
WITH
(
    DISTRIBUTION = ROUND_ROBIN
)
;
```

La consulta original podría haber tenido un aspecto como este:

```sql
UPDATE  acs
SET     [TotalSalesAmount] = [fis].[TotalSalesAmount]
FROM    [dbo].[AnnualCategorySales]     AS acs
JOIN    (
        SELECT  [EnglishProductCategoryName]
        ,       [CalendarYear]
        ,       SUM([SalesAmount])              AS [TotalSalesAmount]
        FROM    [dbo].[FactInternetSales]       AS s
        JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
        JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
        JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
        JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
        WHERE   [CalendarYear] = 2004
        GROUP BY
                [EnglishProductCategoryName]
        ,       [CalendarYear]
        ) AS fis
ON  [acs].[EnglishProductCategoryName]  = [fis].[EnglishProductCategoryName]
AND [acs].[CalendarYear]                = [fis].[CalendarYear]
;
```

Dado que [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] no admite combinaciones ANSI en la cláusula `FROM` de una instrucción `UPDATE`, no se puede usar este código de SQL Server sin cambiarlo ligeramente.

Puede usar una combinación de una instrucción `CTAS` y una combinación implícita para reemplazar este código:

```sql
-- Create an interim table
CREATE TABLE CTAS_acs
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT  ISNULL(CAST([EnglishProductCategoryName] AS NVARCHAR(50)),0)    AS [EnglishProductCategoryName]
,       ISNULL(CAST([CalendarYear] AS SMALLINT),0)                      AS [CalendarYear]
,       ISNULL(CAST(SUM([SalesAmount]) AS MONEY),0)                     AS [TotalSalesAmount]
FROM    [dbo].[FactInternetSales]       AS s
JOIN    [dbo].[DimDate]                 AS d    ON s.[OrderDateKey]             = d.[DateKey]
JOIN    [dbo].[DimProduct]              AS p    ON s.[ProductKey]               = p.[ProductKey]
JOIN    [dbo].[DimProductSubCategory]   AS u    ON p.[ProductSubcategoryKey]    = u.[ProductSubcategoryKey]
JOIN    [dbo].[DimProductCategory]      AS c    ON u.[ProductCategoryKey]       = c.[ProductCategoryKey]
WHERE   [CalendarYear] = 2004
GROUP BY
        [EnglishProductCategoryName]
,       [CalendarYear]
;

-- Use an implicit join to perform the update
UPDATE  AnnualCategorySales
SET     AnnualCategorySales.TotalSalesAmount = CTAS_ACS.TotalSalesAmount
FROM    CTAS_acs
WHERE   CTAS_acs.[EnglishProductCategoryName] = AnnualCategorySales.[EnglishProductCategoryName]
AND     CTAS_acs.[CalendarYear]               = AnnualCategorySales.[CalendarYear]
;

--Drop the interim table
DROP TABLE CTAS_acs
;
```

<a name="ctas-replace-ansi-joins-bk"></a>

### <a name="k-use-ctas-to-specify-which-data-to-keep-instead-of-using-ansi-joins-in-the-from-clause-of-a-delete-statement"></a>K. Usar CTAS para especificar qué datos se deben conservar en lugar de usar combinaciones ANSI en la cláusula FROM de una instrucción DELETE  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

A veces, lo más recomendable para eliminar datos es usar `CTAS`. En lugar de eliminar los datos, simplemente seleccione los datos que quiere conservar. Esto es especialmente aconsejable en el caso de las instrucciones `DELETE` que usan sintaxis de combinación ANSI, ya que [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] no admite combinaciones ANSI en la cláusula `FROM` de una instrucción `DELETE`.

A continuación se muestra un ejemplo de una instrucción DELETE convertida:

```sql
CREATE TABLE dbo.DimProduct_upsert
WITH
(   Distribution=HASH(ProductKey)
,   CLUSTERED INDEX (ProductKey)
)
AS -- Select Data you wish to keep
SELECT     p.ProductKey
,          p.EnglishProductName
,          p.Color
FROM       dbo.DimProduct p
RIGHT JOIN dbo.stg_DimProduct s
ON         p.ProductKey = s.ProductKey
;

RENAME OBJECT dbo.DimProduct        TO DimProduct_old;
RENAME OBJECT dbo.DimProduct_upsert TO DimProduct;
```

<a name="ctas-simplify-merge-bk"></a>

### <a name="l-use-ctas-to-simplify-merge-statements"></a>L. Usar CTAS para simplificar las instrucciones de fusión  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Las instrucciones de fusión se pueden sustituir, al menos en parte, mediante `CTAS`. Es posible consolidar `INSERT` y `UPDATE` en una única instrucción. Los registros eliminados deben cerrarse en una segunda instrucción.

A continuación se muestra un ejemplo de `UPSERT`:

```sql
CREATE TABLE dbo.[DimProduct_upsert]
WITH
(   DISTRIBUTION = HASH([ProductKey])
,   CLUSTERED INDEX ([ProductKey])
)
AS
-- New rows and new versions of rows
SELECT      s.[ProductKey]
,           s.[EnglishProductName]
,           s.[Color]
FROM      dbo.[stg_DimProduct] AS s
UNION ALL  
-- Keep rows that are not being touched
SELECT      p.[ProductKey]
,           p.[EnglishProductName]
,           p.[Color]
FROM      dbo.[DimProduct] AS p
WHERE NOT EXISTS
(   SELECT  *
    FROM    [dbo].[stg_DimProduct] s
    WHERE   s.[ProductKey] = p.[ProductKey]
)
;

RENAME OBJECT dbo.[DimProduct]          TO [DimProduct_old];
RENAME OBJECT dbo.[DimProduct_upsert]  TO [DimProduct];

```

<a name="ctas-data-type-and-nullability-bk"></a>

### <a name="m-explicitly-state-data-type-and-nullability-of-output"></a>M. establezca explícitamente el tipo de datos y la nulabilidad de salida  
Se aplica a: [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)] y [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  

Al migrar código de SQL Server a [!INCLUDE[ssSDW](../../includes/sssdwfull-md.md)], puede encontrarse con este tipo de patrón de codificación:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE result
(result DECIMAL(7,2) NOT NULL
)
WITH (DISTRIBUTION = ROUND_ROBIN)

INSERT INTO result
SELECT @d*@f
;
```

Instintivamente pensará que debe migrar este código a una instrucción CTAS, y tendría razón. Lo que pasa es que hay un problema oculto.

El código siguiente NO produce el mismo resultado:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455
;

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT @d*@f as result
;
```

Observe que la columna "result" traslada el tipo de datos y los valores de nulabilidad de la expresión. Esto puede provocar sutiles variaciones en los valores si no tiene cuidado.

Pruebe por ejemplo lo siguiente:

```sql
SELECT result,result*@d
from result
;

SELECT result,result*@d
from ctas_r
;
```

El valor almacenado para el resultado es diferente. Dado que el valor almacenado en la columna de resultados se usa en otras expresiones, el error es todavía mayor.

![Resultados de CREATE TABLE AS SELECT](../../t-sql/statements/media/create-table-as-select-results.png)

Esto es especialmente importante para las migraciones de datos. Aunque podría decirse que la segunda consulta es más precisa, hay un problema. Los datos serían diferentes en comparación con el sistema de origen, lo que produce problemas de integridad en la migración. Este es uno de los pocos casos en los que la respuesta "incorrecta" es en realidad la correcta.

El motivo por el que vemos esta discrepancia entre los dos resultados se debe a la conversión de tipos implícita. En el primer ejemplo, la tabla establece la definición de columna. Cuando se inserta la fila, se realiza una conversión de tipos implícita. En el segundo ejemplo no hay ninguna conversión de tipos implícita, ya que la expresión define el tipo de datos de la columna. Observe también que la columna del segundo ejemplo se ha definido como una columna que acepta valores NULL, al contrario que en el primer ejemplo. Al crear la tabla en el primer ejemplo, se definió explícitamente la nulabilidad de la columna. En el segundo ejemplo, esto le correspondía a la expresión, lo que conllevaba una definición NULL de forma predeterminada.  

Para resolver estos problemas, debe establecer explícitamente la conversión de tipos y la nulabilidad en la parte `SELECT` de la instrucción `CTAS`. No pueden establecer estas propiedades en la parte de creación de la tabla.

En el ejemplo siguiente se muestra cómo corregir el código:

```sql
DECLARE @d DECIMAL(7,2) = 85.455
,       @f FLOAT(24)    = 85.455

CREATE TABLE ctas_r
WITH (DISTRIBUTION = ROUND_ROBIN)
AS
SELECT ISNULL(CAST(@d*@f AS DECIMAL(7,2)),0) as result
```

Tenga en cuenta lo siguiente:
- Podían haberse usado CAST o CONVERT.
- Se usa ISNULL para forzar la nulabilidad, no COALESCE.
- ISNULL es la función más externa.
- La segunda parte de ISNULL es una constante, es decir, 0.

> [!NOTE]
> Para que la nulabilidad se establezca correctamente, es fundamental usar `ISNULL`, no `COALESCE`. `COALESCE` no es una función determinista, por lo que el resultado de la expresión siempre aceptará valores NULL. `ISNULL` es diferente, ya que es determinista. Por lo tanto, cuando la segunda parte de la función `ISNULL` es una constante o un literal, el valor resultante será NOT NULL.

Esta sugerencia no solo es útil para garantizar la integridad de los cálculos, sino que también es importante para la modificación de particiones de tabla. Imagine que ha definido como hecho esta tabla:

```sql
CREATE TABLE [dbo].[Sales]
(
    [date]      INT     NOT NULL
,   [product]   INT     NOT NULL
,   [store]     INT     NOT NULL
,   [quantity]  INT     NOT NULL
,   [price]     MONEY   NOT NULL
,   [amount]    MONEY   NOT NULL
)
WITH
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101,20020101
                    ,20030101,20040101,20050101
                    )
                )
)
;
```

Pero el campo de valor es una expresión calculada, no forma parte del origen de datos.

Para crear el conjunto de datos con particiones, tal vez haría algo como esto:

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   [quantity]*[price]  AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create')
;
```

La consulta se ejecutaría perfectamente. El problema se produce al intentar realizar la modificación de particiones. Las definiciones de tabla no coinciden. Para que las definiciones de tabla coincidan, es necesario modificar la instrucción CTAS.

```sql
CREATE TABLE [dbo].[Sales_in]
WITH    
(   DISTRIBUTION = HASH([product])
,   PARTITION   (   [date] RANGE RIGHT FOR VALUES
                    (20000101,20010101
                    )
                )
)
AS
SELECT
    [date]    
,   [product]
,   [store]
,   [quantity]
,   [price]   
,   ISNULL(CAST([quantity]*[price] AS MONEY),0) AS [amount]
FROM [stg].[source]
OPTION (LABEL = 'CTAS : Partition IN table : Create');
```

Por lo tanto, puede ver que la coherencia de los tipos y el mantenimiento de las propiedades de nulabilidad en una instrucción CTAS es un procedimiento recomendado de ingeniería. Ayuda a mantener la integridad de los cálculos y también garantiza que la modificación de particiones sea posible.

### <a name="n-create-an-ordered-clustered-columnstore-index-with-maxdop-1"></a>Hora Creación de un índice ordenado de almacén de columnas agrupado con MAXDOP 1  
```sql
CREATE TABLE Table1 WITH (DISTRIBUTION = HASH(c1), CLUSTERED COLUMNSTORE INDEX ORDER(c1) )
AS SELECT * FROM ExampleTable
OPTION (MAXDOP 1);
```

 
## <a name="see-also"></a>Consulte también  
 [CREATE EXTERNAL DATA SOURCE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-data-source-transact-sql.md)   
 [CREATE EXTERNAL FILE FORMAT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-file-format-transact-sql.md)   
 [CREATE EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-transact-sql.md)   
 [CREATE EXTERNAL TABLE AS SELECT &#40;Transact-SQL&#41;](../../t-sql/statements/create-external-table-as-select-transact-sql.md)   
 [CREATE TABLE &#40;Azure Synapse Analytics&#41;](../../t-sql/statements/create-table-azure-sql-data-warehouse.md) [DROP TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-table-transact-sql.md)   
 [DROP EXTERNAL TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-external-table-transact-sql.md)   
 [ALTER TABLE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-table-transact-sql.md)   
 [ALTER EXTERNAL TABLE &#40;Transact-SQL&#41;](./create-external-table-transact-sql.md)  
