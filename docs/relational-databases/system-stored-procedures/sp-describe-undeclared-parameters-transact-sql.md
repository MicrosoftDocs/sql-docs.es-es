---
description: sp_describe_undeclared_parameters (Transact-SQL)
title: sp_describe_undeclared_parameters (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/24/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_describe_undeclared_parameters
- sp_describe_undeclared_parameters_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_describe_undeclared_parameters
ms.assetid: 6f016da6-dfee-4228-8b0d-7cd8e7d5a354
author: markingmyname
ms.author: maghan
monikerRange: = azuresqldb-current||= azure-sqldw-latest||>= sql-server-2016||>= sql-server-linux-2017
ms.openlocfilehash: 46dd9d66c73337920300162f331ee7a59b1e559f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200768"
---
# <a name="sp_describe_undeclared_parameters-transact-sql"></a>sp_describe_undeclared_parameters (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)] 

  Devuelve un conjunto de resultados que contiene metadatos sobre parámetros no declarados en un [!INCLUDE[tsql](../../includes/tsql-md.md)] lote. Considera cada parámetro que se utiliza en el lote de **\@ tsql** , pero no se declara en los **\@ parámetros**. Se devuelve un conjunto de resultados que contiene una fila para cada parámetro, con la información de tipo deducida para dicho parámetro. El procedimiento devuelve un conjunto de resultados vacío si el lote de entrada de **\@ tsql** no tiene ningún parámetro excepto los declarados en **\@ params**.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```sql
  
sp_describe_undeclared_parameters   
    [ @tsql = ] 'Transact-SQL_batch'   
    [ , [ @params = ] N'parameters' data type ] [, ...n]  
```  

> [!Note] 
> Para usar este procedimiento almacenado en Azure Synapse Analytics en un grupo de SQL dedicado, establezca el nivel de compatibilidad de la base de datos en 20 o superior.   Para no participar, cambie el nivel de compatibilidad de la base de datos a 10.

## <a name="arguments"></a>Argumentos  
`[ \@tsql = ] 'Transact-SQL\_batch'` Una o más [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucciones. *Transact-SQL_batch* puede ser **nvarchar (**_n_**)** o **nvarchar (Max)**.  
  
`[ \@params = ] N'parameters'`\@params proporciona una cadena de declaración para los parámetros del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote, de forma similar a como funciona sp_executesql. *Los parámetros* pueden ser **nvarchar (**_n_**)** o **nvarchar (Max)**.  
  
 Es una cadena que contiene las definiciones de todos los parámetros que se han incrustado en *Transact-SQL_batch*. La cadena debe ser una constante Unicode o una variable Unicode. Cada definición de parámetro se compone de un nombre de parámetro y un tipo de datos. n es un marcador de posición que indica definiciones de parámetros adicionales. Si la instrucción o el lote de Transact-SQL de la instrucción no contiene parámetros, \@ no se requiere ningún parámetro. El valor predeterminado de este parámetro es NULL.  
  
 Datatype  
 El tipo de datos del parámetro.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **sp_describe_undeclared_parameters** siempre devuelve el estado de retorno de cero si se realiza correctamente. Si el procedimiento produce un error y se llama al procedimiento como un RPC, el estado de retorno es rellenado por el tipo de error tal y como se describe en la error_type columna de sys.dm_exec_describe_first_result_set. Si se llama al procedimiento desde [!INCLUDE[tsql](../../includes/tsql-md.md)], el valor devuelto siempre es cero, incluso si se produce un error.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
 **sp_describe_undeclared_parameters** devuelve el siguiente conjunto de resultados.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**parameter_ordinal**|**int NOT NULL**|Contiene la posición ordinal del parámetro en el conjunto de resultados. La posición del primer parámetro se especificará como 1.|  
|**name**|**sysname NOT NULL**|Contiene el nombre del parámetro.|  
|**suggested_system_type_id**|**int NOT NULL**|Contiene el **system_type_id** del tipo de datos del parámetro tal y como se especifica en sys. types.<br /><br /> En el caso de los tipos de CLR, aunque la columna **system_type_name** devuelva NULL, esta columna devolverá el valor 240.|  
|**suggested_system_type_name**|**nvarchar (256) NULL**|Contiene el nombre del tipo de datos. Incluye los argumentos (como length, precision y scale) especificados para el tipo de datos del parámetro. Si el tipo de datos es un tipo de alias definido por el usuario, el tipo de sistema subyacente se especifica aquí. Si es un tipo de datos definido por el usuario de CLR, NULL se devuelve en esta columna. Si no se puede deducir el tipo del parámetro, se devuelve NULL.|  
|**suggested_max_length**|**smallint NOT NULL**|Vea sys. Columns. para **max_length** Descripción de la columna.|  
|**suggested_precision**|**tinyint NOT NULL**|Vea sys. Columns. para obtener la descripción de la columna de precisión.|  
|**suggested_scale**|**tinyint NOT NULL**|Vea sys. Columns. para obtener la descripción de la columna de escala.|  
|**suggested_user_type_id**|**int NULL**|Para los tipos de alias y CLR, contiene el user_type_id del tipo de datos de la columna tal y como se especifica en sys.types. De lo contrario, es NULL.|  
|**suggested_user_type_database**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre de la base de datos en la que se define el tipo. De lo contrario, es NULL.|  
|**suggested_user_type_schema**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre del esquema en el que se define el tipo. De lo contrario, es NULL.|  
|**suggested_user_type_name**|**sysname NULL**|Para los tipos de alias y CLR, contiene el nombre del tipo. De lo contrario, es NULL.|  
|**suggested_assembly_qualified_type_name**|**nvarchar (4000) NULL**|Para los tipos de CLR, devuelve el nombre del ensamblado y la clase que define el tipo. De lo contrario, es NULL.|  
|**suggested_xml_collection_id**|**int NULL**|Contiene el xml_collection_id del tipo de datos del parámetro tal y como se especifica en sys. Columns. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**suggested_xml_collection_database**|**sysname NULL**|Contiene la base de datos en la que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**suggested_xml_collection_schema**|**sysname NULL**|Contiene el esquema en el que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**suggested_xml_collection_name**|**sysname NULL**|Contiene el nombre de la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**suggested_is_xml_document**|**bit NOT NULL**|Devuelve 1 si el tipo que se va a devolver es XML y se garantiza que es un documento XML. De lo contrario, devuelve 0.|  
|**suggested_is_case_sensitive**|**bit NOT NULL**|Devuelve 1 si la columna es de un tipo de cadena con distinción entre mayúsculas y minúsculas, y 0 en caso contrario.|  
|**suggested_is_fixed_length_clr_type**|**bit NOT NULL**|Devuelve 1 si la columna es de un tipo CLR de longitud fija y 0 en caso contrario.|  
|**suggested_is_input**|**bit NOT NULL**|Devuelve 1 si el parámetro se utiliza en cualquier otro lugar que no sea el lado izquierdo de una asignación. De lo contrario, devuelve 0.|  
|**suggested_is_output**|**bit NOT NULL**|Devuelve 1 si el parámetro se utiliza en el lado izquierdo de una asignación o se pasa a un parámetro de salida de un procedimiento almacenado. De lo contrario, devuelve 0.|  
|**formal_parameter_name**|**sysname NULL**|Si el parámetro es un argumento para un procedimiento almacenado o una función definida por el usuario, devuelve el nombre del parámetro formal correspondiente. De lo contrario, devuelve NULL.|  
|**suggested_tds_type_id**|**int NOT NULL**|Para uso interno.|  
|**suggested_tds_length**|**int NOT NULL**|Para uso interno.|  
  
## <a name="remarks"></a>Observaciones  
 **sp_describe_undeclared_parameters** siempre devuelve el estado de retorno de cero.  
  
 El uso más común es cuando se proporciona a una aplicación una instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)] que podría contener parámetros y debe procesarlos de alguna manera. Un ejemplo es una interfaz de usuario (como ODBCTest o RowsetViewer) donde el usuario proporciona una consulta con la sintaxis de parámetros de ODBC. La aplicación debe detectar el número de parámetros dinámicamente y pedir confirmación al usuario para cada uno.  
  
 Otro ejemplo es cuando no hay datos proporcionados por el usuario y una aplicación debe recorrer los parámetros y obtener los datos para ellos desde alguna otra ubicación (como una tabla). En este caso, la aplicación no tiene que pasar toda la información de parámetros a la vez. En su lugar, la aplicación puede obtener toda la información de parámetros del proveedor y obtener el propio dato de la tabla. El código que usa **sp_describe_undeclared_parameters** es más genérico y es menos probable que sea necesario modificarlo Si la estructura de datos cambia más adelante.  
  
 **sp_describe_undeclared_parameters** devuelve un error en cualquiera de los casos siguientes.  
  
-   Si el TSQL de entrada \@ no es un [!INCLUDE[tsql](../../includes/tsql-md.md)] lote válido. La validez se determina mediante el análisis y el análisis del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote. Los errores causados por el lote durante la optimización de la consulta o durante la ejecución no se tienen en cuenta al determinar si el [!INCLUDE[tsql](../../includes/tsql-md.md)] lote es válido.  
  
-   Si \@ params no es NULL y contiene una cadena que no es una cadena de declaración sintácticamente válida para los parámetros, o si contiene una cadena que declara cualquier parámetro más de una vez.  
  
-   Si el lote de entrada [!INCLUDE[tsql](../../includes/tsql-md.md)] declara una variable local con el mismo nombre que un parámetro declarado en \@ params.  
  
- Si la instrucción hace referencia a tablas temporales.

- La consulta incluye la creación de una tabla permanente que se consulta.
  
 Si \@ tsql no tiene ningún parámetro, excepto los declarados en \@ params, el procedimiento devuelve un conjunto de resultados vacío.  
  
## <a name="parameter-selection-algorithm"></a>Algoritmo de selección de parámetros  
 En una consulta con parámetros no declarados, la deducción de su tipo de datos se realiza en tres pasos.  
  
 **Paso 1**  
  
 El primer paso de la deducción del tipo de datos de una consulta con parámetros no declarados es encontrar los tipos de datos de todas las subexpresiones cuyos tipos de datos no dependen de los parámetros no declarados. El tipo se puede determinar para las siguientes expresiones:  
  
-   Columnas, constantes, variables y parámetros declarados.  
  
-   Los resultados de una llamada a una función definida por el usuario (UDF).  
  
-   Una expresión con tipos de datos que no dependen de los parámetros no declarados para todas las entradas.  
  
 Por ejemplo, considere la consulta `SELECT dbo.tbl(@p1) + c1 FROM t1 WHERE c2 = @p2 + 2`. Las expresiones DBO. TBL ( \@ P1) + C1 y C2 tienen tipos de datos, y las expresiones \@ P1 y \@ P2 + 2 no lo hacen.  
  
 Después de este paso, si alguna expresión (excepto una llamada a un UDF) tiene dos argumentos sin tipos de datos, la deducción del tipo da un error. Por ejemplo, todo lo siguiente produce errores:  
  
```sql
SELECT * FROM t1 WHERE @p1 = @p2  
SELECT * FROM t1 WHERE c1 = @p1 + @p2  
SELECT * FROM t1 WHERE @p1 = SUBSTRING(@p2, 2, 3)  
```  
  
 En el siguiente ejemplo no se genera un error:  
  
```sql
SELECT * FROM t1 WHERE @p1 = dbo.tbl(c1, @p2, @p3)  
```
  
 **Paso 2**  
  
 Para un determinado parámetro no declarado \@ p, el algoritmo de deducción de tipos encuentra la expresión más interna e ( \@ p) que contiene \@ p y es una de las siguientes:  
  
-   Un argumento de un operador de asignación o comparación.  
  
-   Un argumento de una función definida por el usuario (incluido un UDF con valor de tabla), procedimiento o método.  
  
-   Argumento de una cláusula **Values** de una instrucción **Insert** .  
  
-   Argumento que se va a convertir en una **conversión** o **conversión**.  
  
 El algoritmo de deducción de tipos encuentra un tipo de datos de destino TT ( \@ p) para E ( \@ p). Los tipos de datos de destino de los ejemplos anteriores son los siguientes:  
  
-   El tipo de datos del otro lado de la comparación o asignación.  
  
-   El tipo de datos declarado del parámetro al que se pasa este argumento.  
  
-   El tipo de datos de la columna en la que se inserta este valor.  
  
-   El tipo de datos al que la instrucción se va a convertir.  
  
 Por ejemplo, considere la consulta `SELECT * FROM t WHERE @p1 = dbo.tbl(@p2 + c1)`. A continuación, E ( \@ P1) = \@ P1, E ( \@ P2) = \@ P2 + C1, TT ( \@ P1) es el tipo de datos devuelto declarado de DBO. tbl y TT ( \@ P2) es el tipo de datos de parámetro declarado para DBO. tbl.  
  
 Si \@ p no está incluido en ninguna expresión enumerada al principio del paso 2, el algoritmo de deducción de tipos determina que E ( \@ p) es la expresión escalar más grande que contiene \@ p y el algoritmo de deducción de tipo no calcula un tipo de datos de destino TT ( \@ p) para E ( \@ p). Por ejemplo, si la consulta está seleccionada `@p + 2` , e ( \@ p) = \@ p + 2 y no hay TT ( \@ p).  
  
 **Paso 3**  
  
 Ahora que \@ se identifican e (p) y TT ( \@ p), el algoritmo de deducción de tipo deduce un tipo de datos para \@ p de una de las dos maneras siguientes:  
  
-   Deducción simple  
  
     Si E ( \@ p) = \@ p y TT ( \@ p) existe, es decir, si \@ p es directamente un argumento de una de las expresiones enumeradas al principio del paso 2, el algoritmo de deducción de tipo deduce el tipo de datos \@ p para que sea TT ( \@ p). Por ejemplo:  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p1 AND @p2 = dbo.tbl(@p3)  
    ```  
  
     El tipo de datos de \@ P1, \@ P2 y \@ P3 será el tipo de datos C1, el tipo de datos devuelto de DBO. tbl y el tipo de datos de parámetro de DBO. tbl, respectivamente.  
  
     Como caso especial, si \@ p es un argumento de un \<, > operador, \<=, or > =, no se aplican las reglas de deducción simples. El algoritmo de deducción de tipo utilizará las reglas de deducción generales explicadas en la sección siguiente. Por ejemplo, si c1 es una columna del tipo de datos char(30), considere las siguientes dos consultas:  
  
    ```sql
    SELECT * FROM t WHERE c1 = @p  
    SELECT * FROM t WHERE c1 > @p  
    ```  
  
     En el primer caso, el algoritmo de deducción de tipo deduce **Char (30)** como el tipo de datos de \@ p según las reglas anteriormente en este tema. En el segundo caso, el algoritmo de deducción de tipo deduce **VARCHAR (8000)** de acuerdo con las reglas de deducción generales de la sección siguiente.  
  
-   Deducción general  
  
     Si la deducción simple no se aplica, los siguientes tipos de datos se consideran para los parámetros no declarados:  
  
    -   Tipos de datos enteros (**bit**, **tinyint**, **smallint**, **int**, **BIGINT**)  
  
    -   Tipos de datos Money (**smallmoney**, **Money**)  
  
    -   Tipos de datos de punto flotante (**float**, **real**)  
  
    -   **Numeric (38, 19)** : no se tienen en cuenta otros tipos de datos numéricos o decimales.  
  
    -   **VARCHAR (8000)**, **VARCHAR (Max)**, **nvarchar (4000)** y **nvarchar (Max)** : otros tipos de datos de cadena (como **Text**, **Char (8000)**, **nvarchar (30)**, etc.) no se consideran.  
  
    -   **varbinary (8000)** y **varbinary (Max)** : otros tipos de datos binarios no se tienen en cuenta (como **Image**, **Binary (8000)**, **varbinary (30)**, etc.).  
  
    -   **Date**, **Time (7)**, **smalldatetime**, **DateTime**, **datetime2 (7)**, **DateTimeOffset (7)** -otros tipos de fecha y hora, como **Time (4)**, no se consideran.  
  
    -   **sql_variant**  
  
    -   **xml**  
  
    -   Tipos definidos por el sistema CLR (**hierarchyid**, **Geometry**, **Geography**)  
  
    -   Tipos definidos por el usuario CLR  
  
### <a name="selection-criteria"></a>Criterios de selección  
 De los tipos de datos candidatos, se rechaza cualesquiera que invalidara la consulta. De los tipos de datos candidatos restantes, el algoritmo de deducción de tipo selecciona uno según las siguientes reglas.  
  
1.  Se selecciona el tipo de datos que produce el menor número de conversiones implícitas en E ( \@ p). Si un tipo de datos determinado genera un tipo de datos para E ( \@ p) que es diferente de TT ( \@ p), el algoritmo de deducción de tipos considera que se trata de una conversión implícita adicional del tipo de datos de E ( \@ p) a TT ( \@ p).  
  
     Por ejemplo:  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_Int + @p  
    ```  
  
     En este caso, E ( \@ p) es Col_Int + \@ p y TT ( \@ p) es de **tipo int**. se elige **int** para \@ p porque no genera ninguna conversión implícita. Cualquier otra opción de tipo de datos genera al menos una conversión implícita.  
  
2.  Si hay varios tipos de datos que coinciden en el número menor de conversiones, se utiliza el tipo de datos con mayor prioridad. Por ejemplo,  
  
    ```sql
    SELECT * FROM t WHERE Col_Int = Col_smallint + @p  
    ```  
  
     En este caso, **int** y **smallint** generan una conversión. Otros tipos de datos generan más de una conversión. Dado que **int** tiene prioridad sobre **smallint**, **int** se utiliza para \@ p. Para obtener más información sobre la prioridad de los tipos de datos, vea prioridad de los [tipos de datos &#40;Transact-SQL&#41;](../../t-sql/data-types/data-type-precedence-transact-sql.md).  
  
     Esta regla solo se aplica si hay una conversión implícita entre cada tipo de datos que coincide según la regla 1 y el tipo de datos con la precedencia máxima. Si no hay ninguna conversión implícita, la deducción del tipo de datos no se puede realizar y genera un error. Por ejemplo, en la consulta `SELECT @p FROM t` , la deducción del tipo de datos produce un error porque cualquier tipo de datos de \@ p sería igualmente bueno. Por ejemplo, no hay ninguna conversión implícita de **int** a **XML**.  
  
3.  Si dos tipos de datos similares se vinculan en la regla 1, por ejemplo, **VARCHAR (8000)** y **VARCHAR (Max)**, se elige el tipo de datos más pequeño (**VARCHAR (8000)**). El mismo principio se aplica a los tipos de datos **nvarchar** y **varbinary** .  
  
4.  Para los fines de la regla 1, el algoritmo de deducción de tipo prefiere ciertas conversiones sobre otras. Las conversiones, en orden de mejor a peor, son:  

    1.  Conversión entre el mismo tipo de datos básico de longitud diferente.  
  
    2.  Conversión entre la versión de longitud fija y de longitud variable de los mismos tipos de datos (por ejemplo, **Char** a **VARCHAR**).  
  
    3.  Conversión entre **null** e **int**.  
  
    4.  Cualquier otra conversión.  
  
 Por ejemplo, para la consulta `SELECT * FROM t WHERE [Col_varchar(30)] > @p` , se elige **VARCHAR (8000)** porque la conversión (a) es mejor. En la consulta `SELECT * FROM t WHERE [Col_char(30)] > @p` , se sigue eligiendo **VARCHAR (8000)** porque produce una conversión de tipo (b) y porque otra opción (como **VARCHAR (4000)**) produciría una conversión de tipo (d).  
  
 Como último ejemplo, dada una consulta `SELECT NULL + @p` , se elige **int** para \@ p porque produce una conversión de tipo (c).  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso para ejecutar el \@ argumento TSQL.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelve información como el tipo de datos esperado para los parámetros `@id` e `@name` no declarados.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR name = @name'  
  
```  
  
 Cuando se proporciona el parámetro `@id` como una referencia `@params`, el parámetro `@id` se omite del conjunto de resultados y solo se describe el parámetro `@name`.  
  
```sql
sp_describe_undeclared_parameters @tsql =   
N'SELECT object_id, name, type_desc   
FROM sys.indexes  
WHERE object_id = @id OR NAME = @name',  
@params = N'@id int'  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)
