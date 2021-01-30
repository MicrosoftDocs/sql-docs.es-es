---
title: sys.dm_exec_describe_first_result_set (Transact-SQL)
description: sys.dm_exec_describe_first_result_set (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_describe_first_result_set
- sys.dm_exec_describe_first_result_set_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set catalog view
ms.assetid: 6ea88346-0bdb-4f0e-9f1f-4d85e3487d23
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 06/10/2016
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2a9c52669618f25d685016db93de99f01bd095ab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99106611"
---
# <a name="sysdm_exec_describe_first_result_set-transact-sql"></a>sys.dm_exec_describe_first_result_set (Transact-SQL)

[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

Esta función de administración dinámica toma una [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción como parámetro y describe los metadatos del primer conjunto de resultados de la instrucción.  
  
 **Sys.dm_exec_describe_first_result_set** tiene la misma definición del conjunto de resultados que [Sys.dm_exec_describe_first_result_set_for_object &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md) y es similar a [SP_DESCRIBE_FIRST_RESULT_SET &#40;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  

 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_exec_describe_first_result_set(@tsql, @params, @include_browse_information)  
```  
  
## <a name="arguments"></a>Argumentos  
 *\@tsql*  
 Una o varias instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)]. *Transact-SQL_batch* puede ser **nvarchar (**_n_*_)_* o **nvarchar (Max)**.  
  
 *\@params*  
 \@params proporciona una cadena de declaración para los parámetros del [!INCLUDE[tsql](../../includes/tsql-md.md)] lote, similar a sp_executesql. Los parámetros pueden ser **nvarchar (n)** o **nvarchar (Max)**.  
  
 Es una cadena que contiene las definiciones de todos los parámetros que se han incrustado en el [!INCLUDE[tsql](../../includes/tsql-md.md)] *_batch*. La cadena debe ser una constante Unicode o una variable Unicode. Cada definición de parámetro se compone de un nombre de parámetro y un tipo de datos. *n* es un marcador de posición que indica definiciones de parámetros adicionales. Todos los parámetros especificados en stmt deben definirse en \@ params. Si la [!INCLUDE[tsql](../../includes/tsql-md.md)] instrucción o el lote de la instrucción no contiene parámetros, \@ no es necesario realizar ningún parámetro. NULL es el valor predeterminado para este parámetro.  
  
 *\@include_browse_information*  
 Si está establecido en 1, cada consulta se analiza como si tuviera una opción FOR BROWSE en la consulta. Se devuelven las columnas de clave adicionales e información de la tabla de origen.  
  
## <a name="table-returned"></a>Tabla devuelta  
 Estos metadatos comunes se devuelven como un conjunto de resultados. Una fila para cada columna de los metadatos de resultados describe el tipo y la nulabilidad de la columna en el formato que se muestra en la siguiente tabla. Si la primera instrucción no existe en cada una de las rutas de acceso de control, se devuelve un conjunto de resultados con cero filas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Especifica que la columna es una columna adicional agregada con la finalidad de buscar información que realmente no aparece en el conjunto de resultados.|  
|**column_ordinal**|**int**|Contiene la posición ordinal de la columna en el conjunto de resultados. La posición de la primera columna se especificará como 1.|  
|**name**|**sysname**|Contiene el nombre de la columna si se puede determinar uno. Si no, contendrá NULL.|  
|**is_nullable**|**bit**|Contiene los valores siguientes:<br /><br /> El valor 1 si la columna permite valores NULL.<br /><br /> El valor 0 si la columna no permite valores NULL.<br /><br /> El valor 1 si no se puede determinar que la columna admite valores NULL.|  
|**system_type_id**|**int**|Contiene el system_type_id del tipo de datos de la columna tal y como se especifica en sys. types. En el caso de los tipos de CLR, aunque la columna system_type_name devuelva NULL, esta columna devolverá el valor 240.|  
|**system_type_name**|**nvarchar(256)**|Contiene el nombre y los argumentos (como length, precision y scale) especificados para el tipo de datos de la columna.<br /><br /> Si el tipo de datos es un tipo de alias definido por el usuario, el tipo de sistema subyacente se especifica aquí.<br /><br /> Si es un tipo de datos definido por el usuario de CLR, en esta columna se devuelve NULL.|  
|**max_length**|**smallint**|Longitud máxima de la columna, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor **max_length** será 16 o el valor establecido por **sp_tableoption ' Text in row '**.|  
|**precisión**|**tinyint**|Precisión de la columna, si está basada en números. De lo contrario, devuelve 0.|  
|**scale**|**tinyint**|La escala de la columna se basa en valores numéricos. De lo contrario, devuelve 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación de la columna, si está basada en caracteres. De lo contrario, devuelve NULL.|  
|**user_type_id**|**int**|Para los tipos de alias y CLR, contiene el user_type_id del tipo de datos de la columna tal y como se especifica en sys.types. De lo contrario, es NULL.|  
|**user_type_database**|**sysname**|Para los tipos de alias y CLR, contiene el nombre de la base de datos en la que se define el tipo. De lo contrario, es NULL.|  
|**user_type_schema**|**sysname**|Para los tipos de alias y CLR, contiene el nombre del esquema en el que se define el tipo. De lo contrario, es NULL.|  
|**user_type_name**|**sysname**|Para los tipos de alias y CLR, contiene el nombre del tipo. De lo contrario, es NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Para los tipos CLR, devuelve el nombre del ensamblado y la clase que definen el tipo. De lo contrario, es NULL.|  
|**xml_collection_id**|**int**|Contiene el xml_collection_id del tipo de datos de la columna tal y como se especifica en sys.columns. Esta columna devuelve NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_database**|**sysname**|Contiene la base de datos en la que se define la colección de esquema XML asociado a este tipo. Esta columna devuelve NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_schema**|**sysname**|Contiene el esquema en el que se define la colección de esquema XML asociado a este tipo. Esta columna devuelve NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_name**|**sysname**|Contiene el nombre de la colección de esquema XML asociado a este tipo. Esta columna devuelve NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**is_xml_document**|**bit**|Devuelve 1 si el tipo de datos devuelto es XML y se garantiza que ese tipo es un documento XML completo (incluido un nodo raíz), en lugar de un fragmento XML. De lo contrario, devuelve 0.|  
|**is_case_sensitive**|**bit**|Devuelve 1 si la columna es de un tipo de cadena con distinción entre mayúsculas y minúsculas. Devuelve 0, en caso contrario.|  
|**is_fixed_length_clr_type**|**bit**|Devuelve 1 si la columna es de un tipo CLR de longitud fija. Devuelve 0, en caso contrario.|  
|**source_server**|**sysname**|Nombre del servidor de origen (si se origina desde un servidor remoto). El nombre se proporciona tal y como aparece en sys. servers. Devuelve NULL si la columna se origina en el servidor local o si no se puede determinar en qué servidor se origina. Solo se rellena si se solicita buscar información.|  
|**source_database**|**sysname**|Nombre de la base de datos de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la base de datos. Solo se rellena si se solicita buscar información.|  
|**source_schema**|**sysname**|Nombre del esquema de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar el esquema. Solo se rellena si se solicita buscar información.|  
|**source_table**|**sysname**|Nombre de la tabla de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la tabla. Solo se rellena si se solicita buscar información.|  
|**source_column**|**sysname**|Nombre de la columna de origen que devuelve la columna de resultado. Devuelve NULL si no se puede determinar la columna. Solo se rellena si se solicita buscar información.|  
|**is_identity_column**|**bit**|Devuelve 1 si la columna es una columna de identidad y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna es una columna de identidad.|  
|**is_part_of_unique_key**|**bit**|Devuelve 1 si la columna forma parte de un índice único (incluidas las restricciones única y principal) y 0 en caso contrario. Devuelve NULL si no se puede determinar que la columna forma parte de un índice único. Solo se rellena si se solicita buscar información.|  
|**is_updateable**|**bit**|Devuelve 1 si la columna es actualizable y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna se puede actualizar.|  
|**is_computed_column**|**bit**|Devuelve 1 si la columna es una columna calculada y 0 de lo contrario. Devuelve NULL si no se puede determinar si la columna es una columna calculada.|  
|**is_sparse_column_set**|**bit**|Devuelve 1 si la columna es una columna dispersa y 0 si no lo es. Devuelve NULL si no se puede determinar que la columna forma parte de un conjunto de columnas dispersas.|  
|**ordinal_in_order_by_list**|**smallint**|La posición de esta columna está en la lista ORDER BY. Devuelve NULL si no aparece en la lista ORDER BY o si la lista ORDER BY no se puede determinar de forma inequívoca.|  
|**order_by_list_length**|**smallint**|Longitud de la lista de ORDER BY. Devuelve NULL si no hay ninguna lista ORDER BY o si no se puede determinar la lista ORDER BY singularmente. Observe que este valor será el mismo para todas las filas devueltas por sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Si ordinal_in_order_by_list no es NULL, la columna **order_by_is_descending** notifica la dirección de la cláusula ORDER BY para esta columna. De lo contrario, notifica NULL.|  
|**error_number**|**int**|Contiene el número de error devuelto por la función. Si no se produjo ningún error, la columna contendrá NULL.|  
|**error_severity**|**int**|Contiene la gravedad devuelta por la función. Si no se produjo ningún error, la columna contendrá NULL.|  
|**error_state**|**int**|Contiene el mensaje de estado que la función devuelve. Si no se produjo ningún error, la columna contendrá NULL.|  
|**error_message**|**nvarchar (4096)**|Contiene el mensaje que devuelve la función. Si no se produjo ningún error, la columna contendrá NULL.|  
|**error_type**|**int**|Contiene un entero que representa el error que se va a devolver. Se asigna a error_type_desc. Vea la lista bajo las notas.|  
|**error_type_desc**|**nvarchar(60)**|Contiene una cadena corta en mayúsculas que representa el error que se va a devolver. Se asigna a error_type. Vea la lista bajo las notas.|  
  
## <a name="remarks"></a>Observaciones  
 Esta función utiliza el mismo algoritmo que **sp_describe_first_result_set**. Para obtener más información, vea [sp_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 En la tabla siguiente se muestran los tipos de error y su descripción.  
  
|error_type|error_type|Descripción|  
|-----------------|-----------------|-----------------|  
|1|VARIOS|Todos los errores que no se han descrito.|  
|2|SINTAXIS|Se produjo un error de sintaxis en el lote.|  
|3|CONFLICTING_RESULTS|El resultado no se pudo determinar debido a un conflicto entre las dos primeras instrucciones posibles.|  
|4|DYNAMIC_SQL|El resultado no se pudo determinar debido al SQL dinámico que podría devolver el primer resultado.|  
|5|CLR_PROCEDURE|El resultado no se pudo determinar porque un procedimiento almacenado CLR podría devolver el primer resultado.|  
|6|CLR_TRIGGER|El resultado no se pudo determinar porque un desencadenador CLR podría devolver el primer resultado.|  
|7|EXTENDED_PROCEDURE|El resultado no se pudo determinar porque un procedimiento almacenado extendido podría devolver el primer resultado.|  
|8|UNDECLARED_PARAMETER|No se pudo determinar el resultado porque el tipo de datos de una o varias de las columnas del conjunto de resultados depende potencialmente de un parámetro no declarado.|  
|9|RECURSION|El resultado no se pudo determinar porque el lote contiene una instrucción recursiva.|  
|10|TEMPORARY_TABLE|El resultado no se pudo determinar porque el lote contiene una tabla temporal que no es admitida por **sp_describe_first_result_set**.|  
|11|UNSUPPORTED_STATEMENT|El resultado no se pudo determinar porque el lote contiene una instrucción que no es admitida por **sp_describe_first_result_set** (por ejemplo, FETCH, REVERT, etc.).|  
|12|OBJECT_TYPE_NOT_SUPPORTED|\@No se admite la object_id pasada a la función (es decir, no un procedimiento almacenado)|  
|13|OBJECT_DOES_NOT_EXIST|\@No se encontró la object_id pasada a la función en el catálogo del sistema.|  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso para ejecutar el \@ argumento TSQL.  
  
## <a name="examples"></a>Ejemplos  
 Los ejemplos adicionales del tema [sp_describe_first_result_set &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md) se pueden adaptar para usar **Sys.dm_exec_describe_first_result_set**.  
  
### <a name="a-returning-information-about-a-single-transact-sql-statement"></a>A. Devolver información acerca de una sola instrucción de Transact-SQL  
 El código siguiente devuelve información acerca de los resultados de una instrucción de [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT * FROM sys.dm_exec_describe_first_result_set  
(N'SELECT object_id, name, type_desc FROM sys.indexes', null, 0) ;  
```  
  
### <a name="b-returning-information-about-a-procedure"></a>B. Devolver información acerca de un procedimiento  
 En el ejemplo siguiente se crea un procedimiento almacenado denominado pr_TestProc que devuelve dos conjuntos de resultados. En el ejemplo se muestra que **sys.dm_exec_describe_first_result_set** devuelve información acerca del primer conjunto de resultados del procedimiento.  
  
```  
USE AdventureWorks2012;  
GO  
  
CREATE PROC Production.TestProc  
AS  
SELECT Name, ProductID, Color FROM Production.Product ;  
SELECT Name, SafetyStockLevel, SellStartDate FROM Production.Product ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set  
('Production.TestProc', NULL, 0) ;  
```  
  
### <a name="c-returning-metadata-from-a-batch-that-contains-multiple-statements"></a>C. Devolver los metadatos de un lote que contiene varias instrucciones  
 El ejemplo siguiente devuelve un lote que contiene dos instrucciones de [!INCLUDE[tsql](../../includes/tsql-md.md)]. El conjunto de resultados describe el primer conjunto de resultados devuelto.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set(  
N'SELECT CustomerID, TerritoryID, AccountNumber FROM Sales.Customer WHERE CustomerID = @CustomerID;  
SELECT * FROM Sales.SalesOrderHeader;',  
N'@CustomerID int', 0) AS a;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set_for_object &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-for-object-transact-sql.md)  
  
  
