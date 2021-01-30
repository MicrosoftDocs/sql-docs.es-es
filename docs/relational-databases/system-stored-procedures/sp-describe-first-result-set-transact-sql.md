---
description: sp_describe_first_result_set (Transact-SQL)
title: sp_describe_first_result_set (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_first_result_set
- sp_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_first_result_set
ms.assetid: f2355a75-3a8e-43e6-96ad-4f41038f6d22
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 8e9a77fb68a5642b3ba5fac7aac9ae0d9c31c899
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200810"
---
# <a name="sp_describe_first_result_set-transact-sql"></a>sp_describe_first_result_set (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve los metadatos para el primer conjunto de resultados posible del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote. Devuelve un conjunto de resultados vacío si el lote no devuelve resultados. Genera un error si [!INCLUDE[ssDE](../../includes/ssde-md.md)] no puede determinar los metadatos de la primera consulta que se ejecutará realizando un análisis estático. La vista de administración dinámica [sys.dm_exec_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) devuelve la misma información.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_describe_first_result_set [ @tsql = ] N'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' ]   
    [ , [ @browse_information_mode = ] <tinyint> ] ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ \@tsql = ] 'Transact-SQL_batch'` Una o más [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones. *Transact-SQL_batch* puede ser **nvarchar (**_n_*_)_* o **nvarchar (Max)**.  
  
`[ \@params = ] N'parameters'`\@params proporciona una cadena de declaración para los parámetros del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote, que es similar a sp_executesql. Los parámetros pueden ser **nvarchar (n)** o **nvarchar (Max)**.  
  
 Es una cadena que contiene las definiciones de todos los parámetros que se han incrustado en el [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. La cadena debe ser una constante Unicode o una variable Unicode. Cada definición de parámetro se compone de un nombre de parámetro y un tipo de datos. *n* es un marcador de posición que indica definiciones de parámetros adicionales. Todos los parámetros especificados en la instrucción deben definirse en \@ params. Si la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote de la instrucción no contiene parámetros, \@ no es necesario realizar ningún parámetro. NULL es el valor predeterminado para este parámetro.  
  
`[ \@browse_information_mode = ] tinyint` Especifica si se devuelven columnas de clave adicionales e información de la tabla de origen. Si está establecido en 1, cada consulta se analiza como si incluyera una opción FOR BROWSE. Se devuelven las columnas de clave adicionales e información de la tabla de origen.  
  
-   Si se establece en 0, no se devuelve información.  
  
-   Si está establecido en 1, cada consulta se analiza como si incluyera una opción FOR BROWSE. Esto devolverá los nombres de tabla base como información de la columna de origen.  
  
-   Si se establece en 2, cada consulta se analiza como si se fuera a usar en la preparación o ejecución de un cursor. Esto devolverá los nombres de vista como información de la columna de origen.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **sp_describe_first_result_set** devuelve siempre el estado cero en caso de éxito. Si el procedimiento produce un error y se llama al procedimiento como un RPC, el estado de retorno es rellenado por el tipo de error descrito en la error_type columna de sys.dm_exec_describe_first_result_set. Si se llama al procedimiento desde [!INCLUDE[tsql](../../includes/tsql-md.md)], el valor devuelto siempre es cero, incluso cuando se produce un error.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 Estos metadatos comunes se devuelven como un conjunto de resultados con una única fila por cada columna de los metadatos de los resultados. Cada fila describe el tipo y la nulabilidad de la columna en el formato descrito en la siguiente sección. Si la primera instrucción no existe en cada una de las rutas de acceso de control, se devuelve un conjunto de resultados con cero filas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit NOT NULL**|Indica que la columna es una columna adicional agregada para examinar el propósito de la información y que no aparece realmente en el conjunto de resultados.|  
|**column_ordinal**|**int NOT NULL**|Contiene la posición ordinal de la columna en el conjunto de resultados. La posición de la primera columna se especificará como 1.|  
|**name**|**sysname NULL**|Contiene el nombre de la columna si se puede determinar uno. De lo contrario, contendrá NULL.|  
|**is_nullable**|**bit NOT NULL**|Contiene el valor 1 si la columna permite valores NULL, 0 si la columna no permite valores NULL y 1 si no se puede determinar si la columna permite valores NULL.|  
|**system_type_id**|**int NOT NULL**|Contiene el system_type_id del tipo de datos de la columna tal y como se especifica en sys. types. En el caso de los tipos de CLR, aunque la columna system_type_name devuelva NULL, esta columna devolverá el valor 240.|  
|**system_type_name**|**nvarchar (256) NULL**|Contiene el nombre y los argumentos (como length, precision y scale) especificados para el tipo de datos de la columna. Si el tipo de datos es un tipo de alias definido por el usuario, el tipo de sistema subyacente se especifica aquí. Si es un tipo definido por el usuario de CLR, NULL se devuelve en esta columna.|  
|**max_length**|**smallint NOT NULL**|Longitud máxima de la columna, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor **max_length** será 16 o el valor establecido por **sp_tableoption ' Text in row '**.|  
|**precisión**|**tinyint NOT NULL**|Precisión de la columna, si está basada en números. De lo contrario, devuelve 0.|  
|**scale**|**tinyint NOT NULL**|La escala de la columna se basa en valores numéricos. De lo contrario, devuelve 0.|  
|**collation_name**|**sysname NULL**|Nombre de la intercalación de la columna, si está basada en caracteres. De lo contrario, devuelve NULL.|  
|**user_type_id**|**int NULL**|Para los tipos de alias y CLR, contiene el user_type_id del tipo de datos de la columna tal y como se especifica en sys.types. De lo contrario, es NULL.|  
|**user_type_database**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre de la base de datos en la que se define el tipo. De lo contrario, es NULL.|  
|**user_type_schema**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre del esquema en el que se define el tipo. De lo contrario, es NULL.|  
|**user_type_name**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre del tipo. De lo contrario, es NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Para los tipos CLR, devuelve el nombre del ensamblado y la clase que definen el tipo. De lo contrario, es NULL.|  
|**xml_collection_id**|**int NULL**|Contiene el xml_collection_id del tipo de datos de la columna tal y como se especifica en sys.columns. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_database**|**sysname NULL**|Contiene la base de datos en la que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_schema**|**sysname NULL**|Contiene el esquema en el que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_name**|**sysname NULL**|Contiene el nombre de la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**is_xml_document**|**bit NOT NULL**|Devuelve 1 si el tipo de datos devuelto es XML y se garantiza que ese tipo es un documento XML completo (incluido un nodo raíz), en lugar de un fragmento XML. De lo contrario, devuelve 0.|  
|**is_case_sensitive**|**bit NOT NULL**|Devuelve 1 si la columna es un tipo de cadena que distingue entre mayúsculas y minúsculas, y 0 si no lo es.|  
|**is_fixed_length_clr_type**|**bit NOT NULL**|Devuelve 1 si la columna es de un tipo CLR de longitud fija y 0 de lo contrario.|  
|**source_server**|**sysname**|Nombre del servidor de origen que devuelve la columna en este resultado (si se origina desde un servidor remoto). El nombre se proporciona tal y como aparece en sys. servers. Devuelve NULL si la columna se origina en el servidor local o si no se puede determinar en qué servidor se origina. Solo se rellena si se solicita buscar información.|  
|**source_database**|**sysname**|Nombre de la base de datos de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la base de datos. Solo se rellena si se solicita buscar información.|  
|**source_schema**|**sysname**|Nombre del esquema de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar el esquema. Solo se rellena si se solicita buscar información.|  
|**source_table**|**sysname**|Nombre de la tabla de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la tabla. Solo se rellena si se solicita buscar información.|  
|**source_column**|**sysname**|Nombre de la columna de origen que devuelve la columna de resultado. Devuelve NULL si no se puede determinar la columna. Solo se rellena si se solicita buscar información.|  
|**is_identity_column**|**bit NULL**|Devuelve 1 si la columna es una columna de identidad y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna es una columna de identidad.|  
|**is_part_of_unique_key**|**bit NULL**|Devuelve 1 si la columna forma parte de un índice único (que incluye una restricción única y principal) y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna forma parte de un índice único. Solo se rellena si se solicita buscar información.|  
|**is_updateable**|**bit NULL**|Devuelve 1 si la columna es actualizable y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna se puede actualizar.|  
|**is_computed_column**|**bit NULL**|Devuelve 1 si la columna es una columna calculada y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna es una columna calculada.|  
|**is_sparse_column_set**|**bit NULL**|Devuelve 1 si la columna es una columna dispersa y 0 si no lo es. Devuelve NULL si no se puede determinar que la columna forma parte de un conjunto de columnas dispersas.|  
|**ordinal_in_order_by_list**|**smallint NULL**|Posición de esta columna en la lista ORDER BY. Devuelve NULL si la columna no aparece en la lista ORDER BY o si la lista ORDER BY no se puede determinar de forma única.|  
|**order_by_list_length**|**smallint NULL**|Longitud de la lista de ORDER BY. Devuelve NULL si no hay ninguna lista ORDER BY o si no se puede determinar la lista ORDER BY singularmente. Tenga en cuenta que este valor será el mismo para todas las filas devueltas por **sp_describe_first_result_set.**|  
|**order_by_is_descending**|**smallint NULL**|Si ordinal_in_order_by_list no es NULL, la columna **order_by_is_descending** notifica la dirección de la cláusula ORDER BY para esta columna. De lo contrario, notifica NULL.|  
|**tds_type_id**|**int NOT NULL**|Para uso interno.|  
|**tds_length**|**int NOT NULL**|Para uso interno.|  
|**tds_collation_id**|**int NULL**|Para uso interno.|  
|**tds_collation_sort_id**|**tinyint NULL**|Para uso interno.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_describe_first_result_set** garantiza que, si el procedimiento devuelve los metadatos del primer conjunto de resultados de un lote hipotético a y, si ese lote (a) se ejecuta posteriormente, el lote (1) generará un error en tiempo de optimización. (2) genera un error en tiempo de ejecución, (3) no devuelve ningún conjunto de resultados o (4) devuelve un primer conjunto de resultados con los mismos metadatos descritos por **sp_describe_first_result_set**.  
  
 El nombre, la nulabilidad y el tipo de datos pueden diferir. Si **sp_describe_first_result_set** devuelve un conjunto de resultados vacío, la garantía es que la ejecución del lote devolverá conjuntos sin resultados.  
  
 Esta garantía presupone que no hay ningún cambio de esquema relevante en el servidor. Los cambios de esquema relevantes en el servidor no incluyen la creación de tablas temporales o variables de tabla en el lote A entre el momento en que se llama a **sp_describe_first_result_set** y el momento en que se devuelve el conjunto de resultados durante la ejecución, incluidos los cambios de esquema realizados por el lote B.  
  
 **sp_describe_first_result_set** devuelve un error en cualquiera de los casos siguientes.  
  
-   Si el TSQL de entrada \@ no es un [!INCLUDE[tsql](../../includes/tsql-md.md)] lote válido. La validez se determina mediante el análisis y el análisis del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote. Los errores causados por el lote durante la optimización de la consulta o durante la ejecución no se tienen en cuenta al determinar si el [!INCLUDE[tsql](../../includes/tsql-md.md)] lote es válido.  
  
-   Si \@ params no es NULL y contiene una cadena que no es una cadena de declaración sintácticamente válida para los parámetros, o si contiene una cadena que declara cualquier parámetro más de una vez.  
  
-   Si el lote de entrada [!INCLUDE[tsql](../../includes/tsql-md.md)] declara una variable local con el mismo nombre que un parámetro declarado en \@ params.  
  
-   Si la instrucción utiliza una tabla temporal.  
  
-   La consulta incluye la creación de una tabla permanente que se consulta.  
  
 Si todas las demás comprobaciones se realizan correctamente, se tienen en cuenta todas las rutas de flujo de control posibles incluidas en el lote de entrada. Esto tiene en cuenta todas las instrucciones de flujo de control (GOTO, IF/ELSE, WHILE, y [!INCLUDE[tsql](../../includes/tsql-md.md)] bloques try/catch, así como los procedimientos, lotes dinámicos [!INCLUDE[tsql](../../includes/tsql-md.md)] o desencadenadores que se invocan desde el lote de entrada mediante una instrucción exec, una instrucción DDL que hace que se activen los desencadenadores DDL o una instrucción DML que hace que los desencadenadores se activen en una tabla de destino o en una tabla que se modifica debido a la acción en cascada en una restricción En el caso de que haya numerosas rutas de acceso de control posibles, los algoritmos se detienen en algún punto.  
  
 Para cada ruta de acceso de flujo de control, la primera instrucción (si existe) que devuelve un conjunto de resultados viene determinada por **sp_describe_first_result_set**.  
  
 Cuando en el lote se encuentran varias instrucciones que podrían ser las primeras, sus resultados pueden diferir en el número de columnas, el nombre de las columnas, la nulabilidad y el tipo de datos. A continuación se describe con más detalle cómo se administrar estas diferencias:  
  
-   Si el número de columnas difiere, se genera un error y no se devuelve ningún resultado.  
  
-   Si difiere el nombre de columna, el valor devuelto se establece en NULL.  
  
-   Si difiere la nulabilidad, la nulabilidad devuelta permitirá valores NULL.  
  
-   Si difiere el tipo de datos, se generará un error y no se devolverán resultados salvo en los casos siguientes:  
  
    -   **VARCHAR (a)** a **VARCHAR (a ')** donde ' > a.  
  
    -   **VARCHAR (a)** a **VARCHAR (Max)**  
  
    -   **nvarchar (a)** a **nvarchar (a ')** donde ' > a.  
  
    -   **nvarchar (a)** a **nvarchar (Max)**  
  
    -   **varbinary (a)** a **varbinary (a ')** donde ' > a.  
  
    -   **varbinary (a)** a **varbinary (Max)**  
  
 **sp_describe_first_result_set** no admite la recursividad indirecta.  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso para ejecutar el \@ argumento TSQL.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="typical-examples"></a>Ejemplos habituales  
  
#### <a name="a-simple-example"></a>A. Ejemplo sencillo  
 En el ejemplo siguiente se describe el conjunto de resultados devuelto a partir de una consulta única.  
  
```  
sp_describe_first_result_set @tsql = N'SELECT object_id, name, type_desc FROM sys.indexes'  
```  
  
 En el ejemplo siguiente se muestra el conjunto de resultados devuelto por una única consulta que contiene un parámetro.  
  
```  
sp_describe_first_result_set @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes   
WHERE object_id = @id1'  
, @params = N'@id1 int'  
```  
  
#### <a name="b-browse-mode-examples"></a>B. Ejemplos de modo de exploración  
 En los tres ejemplos siguientes se muestra la diferencia clave entre los diferentes modos de información de exploración. En los resultados de la consulta solo se incluyen las columnas relevantes.  
  
 El ejemplo que usa 0 indica que no se devuelve información alguna.  
  
```sql  
CREATE TABLE dbo.t (a int PRIMARY KEY, b1 int);  
GO  
CREATE VIEW dbo.v AS SELECT b1 AS b2 FROM dbo.t;  
GO  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM dbo.v', null, 0;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|NULL|NULL|NULL|NULL|  
  
 El ejemplo que usa 1 indica que se devuelve información como si incluyera una opción FOR BROWSE en la consulta.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 1  
  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|b3|dbo|t|B1|0|  
|1|2|a|dbo|t|a|1|  
  
 El ejemplo que usa 2 indica que se analiza como si se estuviera preparando un cursor.  
  
```sql  
EXEC sp_describe_first_result_set N'SELECT b2 AS b3 FROM v', null, 2  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
|is_hidden|column_ordinal|name|source_schema|source_table|source_column|is_part_of_unique_key|  
|----------------|---------------------|----------|--------------------|-------------------|--------------------|-------------------------------|  
|0|1|B3|dbo|v|B2|0|  
|1|2|ROWSTAT|NULL|NULL|NULL|0|  
  
### <a name="examples-of-problems"></a>Ejemplos de problemas  
 En todos los ejemplos siguientes se usan dos tablas. Ejecute las siguientes instrucciones para crear las tablas de ejemplo.  
  
```  
CREATE TABLE dbo.t1 (a int NULL, b varchar(10) NULL, c nvarchar(10) NULL);  
CREATE TABLE dbo.t2 (a smallint NOT NULL, d varchar(20) NOT NULL, e int NOT NULL);  
```  
  
#### <a name="error-because-the-number-of-columns-differ"></a>Error porque difiere el número de columnas  
 En este ejemplo, difiere el número de columnas de los primeros conjuntos de resultados posibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a, b FROM t1;  
SELECT * FROM t; -- Ignored, not a possible first result set.'  
  
```  
  
#### <a name="error-because-the-data-types-differ"></a>Error porque difieren los tipos de datos  
 Los tipos de columnas difieren en los primeros conjuntos de resultados posibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT a FROM t1;  
ELSE  
    SELECT a FROM t2;  
```  
  
 Resultado: error, tipos no coincidentes (**int** frente a **smallint**).  
  
#### <a name="column-name-cannot-be-determined"></a>El nombre de columna no se puede determinar  
 Las columnas de los primeros conjuntos de resultados posibles difieren en la longitud del mismo tipo de longitud variable, la nulabilidad y los nombres de columna.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d FROM t2; '  
```  
  
 Resultado: \<Unknown Column Name> **VARCHAR (20) null**  
  
#### <a name="column-name-forced-to-be-identical-through-aliasing"></a>Se exige que el nombre de columna sea idéntico en todos los alias  
 Igual que el caso anterior, pero las columnas tienen el mismo nombre en todos los alias de columna.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT d AS b FROM t2;'  
```  
  
 Resultado: b **VARCHAR (20) null**  
  
#### <a name="error-because-column-types-cannot-be-matched"></a>Error porque los tipos de columna no coinciden  
 Los tipos de columnas difieren en los primeros conjuntos de resultados posibles.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    SELECT b FROM t1;  
ELSE  
    SELECT c FROM t1;'  
```  
  
 Resultado: error, tipos no coincidentes (**VARCHAR (10)** frente a **nvarchar (10)**).  
  
#### <a name="result-set-can-return-an-error"></a>El conjunto de resultados puede devolver un error  
 El primer conjunto de resultados es un error o un conjunto de resultados.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RAISERROR(''Some Error'', 16, 1);  
  
ELSE  
    SELECT a FROM t1;  
SELECT e FROM t2; -- Ignored, not a possible first result set.;'  
```  
  
 Resultado: **intNULL**  
  
#### <a name="some-code-paths-return-no-results"></a>Algunas rutas de acceso del código no devuelven resultados  
 El primer conjunto de resultados es NULL o un conjunto de resultados.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
IF(1=1)  
    RETURN;  
SELECT a FROM t1;'  
```  
  
 Resultado: **intNULL**  
  
#### <a name="result-from-dynamic-sql"></a>Resultado de SQL dinámico  
 El primer conjunto de resultados es un SQL dinámico que se puede detectar porque es una cadena literal.  
  
```  
sp_describe_first_result_set @tsql =   
N'EXEC(N''SELECT a FROM t1'');'  
```  
  
 Resultado: un **valor int null**  
  
#### <a name="result-failure-from-dynamic-sql"></a>Error al obtener resultados de SQL dinámico  
 El primer conjunto de resultados no está definido debido a un SQL dinámico.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL); '  
```  
  
 Resultado: Error. El resultado no se puede detectar debido a un SQL dinámico.  
  
#### <a name="result-set-specified-by-user"></a>Conjunto de resultados especificado por el usuario  
 El usuario especifica manualmente el primer conjunto de resultados.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
DECLARE @SQL NVARCHAR(max);  
SET @SQL = N''SELECT a FROM t1 WHERE 1 = 1 '';  
IF(1=1)  
    SET @SQL += N'' AND e > 10 '';  
EXEC(@SQL)  
    WITH RESULT SETS(  
        (Column1 BIGINT NOT NULL)  
    ); '  
```  
  
 Resultado: Column1 **BIGINT not null**  
  
#### <a name="error-caused-by-a-ambiguous-result-set"></a>Error generado por un conjunto de resultados ambiguo  
 En este ejemplo se da por supuesto que otro usuario llamado user1 tiene una tabla denominada T1 en el esquema predeterminado S1 con columnas (un **int no NULL**).  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT * FROM t1;'  
, @params = N'@p int'  
```  
  
 Resultado: Error. T1 puede ser DBO. T1 o S1. T1, cada uno con un número diferente de columnas.  
  
#### <a name="result-even-with-ambiguous-result-set"></a>Resultado incluso con el conjunto de resultados ambiguo  
 Use las mismas suposiciones que en el ejemplo anterior.  
  
```  
sp_describe_first_result_set @tsql =   
N'  
    IF(@p > 0)  
    EXECUTE AS USER = ''user1'';  
    SELECT a FROM t1;'  
```  
  
 Resultado: un **valor int null** porque tanto DBO. T1. a como S1. T1. a tienen un tipo **int** y una nulabilidad diferente.  
  
## <a name="see-also"></a>Consulte también  
 [sp_describe_undeclared_parameters &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
 
