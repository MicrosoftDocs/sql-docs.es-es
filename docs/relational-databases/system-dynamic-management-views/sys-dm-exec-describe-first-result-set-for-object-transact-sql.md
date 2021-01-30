---
description: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
title: sys.dm_exec_describe_first_result_set_for_object (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_exec_describe_first_result_set_for_object_TSQL
- sys.dm_exec_describe_first_result_set_for_object
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_exec_describe_first_result_set_for_object catalog view
ms.assetid: 63b0fde7-95d7-4ad7-a219-a9feacf1bd89
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c03f5feb44cdc530c0ccc2e4c89973538c1b8cde
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160081"
---
# <a name="sysdm_exec_describe_first_result_set_for_object-transact-sql"></a>sys.dm_exec_describe_first_result_set_for_object (Transact-SQL)
[!INCLUDE [SQL Server Azure SQL Database ](../../includes/applies-to-version/sql-asdb.md)]

  Esta función de administración dinámica toma @object_id como parámetro y describe los metadatos del primer resultado para el módulo con ese identificador. El @object_id especificado puede ser el identificador de un [!INCLUDE[tsql](../../includes/tsql-md.md)] procedimiento almacenado o un [!INCLUDE[tsql](../../includes/tsql-md.md)] desencadenador. Si es el identificador de cualquier otro objeto (como una vista, tabla, función o procedimiento CLR), se especificará un error en las columnas de error del resultado.  
  
 **Sys.dm_exec_describe_first_result_set_for_object** tiene la misma definición del conjunto de resultados que [Sys.dm_exec_describe_first_result_set &#40;transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md) y es similar a [SP_DESCRIBE_FIRST_RESULT_SET &#40;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md).  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_exec_describe_first_result_set_for_object   
    ( @object_id , @include_browse_information )  
```  
  
## <a name="arguments"></a>Argumentos  
 *\@object_id*  
 @object_idDe un [!INCLUDE[tsql](../../includes/tsql-md.md)] procedimiento almacenado o un [!INCLUDE[tsql](../../includes/tsql-md.md)] desencadenador. @object_id es de tipo **int**.  
  
 *\@include_browse_information*  
 @include_browse_information es de tipo **bit**. Si está establecido en 1, cada consulta se analiza como si tuviera una opción FOR BROWSE en la consulta. Devuelve las columnas de clave adicionales e información de la tabla de origen.  
  
## <a name="table-returned"></a>Tabla devuelta  
 Estos metadatos comunes se devuelven como un conjunto de resultados con una única fila por cada columna de los metadatos de los resultados. Cada fila describe el tipo y la nulabilidad de la columna en el formato descrito en la siguiente sección. Si la primera instrucción no existe en cada una de las rutas de acceso de control, se devuelve un conjunto de resultados con cero filas.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**is_hidden**|**bit**|Especifica si la columna es una columna adicional agregada con la finalidad de buscar información que realmente no aparece en el conjunto de resultados.|  
|**column_ordinal**|**int**|Contiene la posición ordinal de la columna en el conjunto de resultados. La posición de la primera columna se especificará como 1.|  
|**name**|**sysname**|Contiene el nombre de la columna si se puede determinar uno. De lo contrario, es NULL.|  
|**is_nullable**|**bit**|Contiene el valor 1 si la columna permite valores NULL, 0 si la columna no permite valores NULL y 1 si no se puede determinar que la columna permite  valores NULL.|  
|**system_type_id**|**int**|Contiene el system_type_id del tipo de datos de la columna tal y como se especifica en sys. types. En el caso de los tipos de CLR, aunque la columna system_type_name devuelva NULL, esta columna devolverá el valor 240.|  
|**system_type_name**|**nvarchar(256)**|Contiene el nombre del tipo de datos. Incluye los argumentos (como length, precision y scale) especificados para el tipo de datos de la columna. Si el tipo de datos es un tipo de alias definido por el usuario, el tipo de sistema subyacente se especifica aquí. Si es un tipo definido por el usuario de CLR, NULL se devuelve en esta columna.|  
|**max_length**|**smallint**|Longitud máxima de la columna, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor **max_length** será 16 o el valor establecido por **sp_tableoption ' Text in row '**.|  
|**precisión**|**tinyint**|Precisión de la columna, si está basada en números. De lo contrario, devuelve 0.|  
|**scale**|**tinyint**|La escala de la columna se basa en valores numéricos. De lo contrario, devuelve 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación de la columna, si está basada en caracteres. De lo contrario, devuelve NULL.|  
|**user_type_id**|**int**|Para los tipos de alias y CLR, contiene el user_type_id del tipo de datos de la columna tal y como se especifica en sys.types. De lo contrario, es NULL.|  
|**user_type_database**|**sysname**|Para los tipos de alias y CLR, contiene el nombre de la base de datos en la que se define el tipo. De lo contrario, es NULL.|  
|**user_type_schema**|**sysname**|Para los tipos de alias y CLR, contiene el nombre del esquema en el que se define el tipo. De lo contrario, es NULL.|  
|**user_type_name**|**sysname**|Para los tipos de alias y CLR, contiene el nombre del tipo. De lo contrario, es NULL.|  
|**assembly_qualified_type_name**|**nvarchar(4000)**|Para los tipos CLR, devuelve el nombre del ensamblado y la clase que definen el tipo. De lo contrario, es NULL.|  
|**xml_collection_id**|**int**|Contiene el xml_collection_id del tipo de datos de la columna tal y como se especifica en sys.columns. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_database**|**sysname**|Contiene la base de datos en la que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_schema**|**sysname**|Contiene el esquema en el que se define la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**xml_collection_name**|**sysname**|Contiene el nombre de la colección de esquema XML asociado a este tipo. Esta columna devolverá NULL si el tipo devuelto no está asociado a una colección de esquema XML.|  
|**is_xml_document**|**bit**|Devuelve 1 si el tipo de datos devuelto es XML y se garantiza que ese tipo es un documento XML completo (incluido un nodo raíz), en lugar de un fragmento XML. De lo contrario, devuelve 0.|  
|**is_case_sensitive**|**bit**|Devuelve 1 si la columna es de un tipo de cadena con distinción entre mayúsculas y minúsculas, y 0 en caso contrario.|  
|**is_fixed_length_clr_type**|**bit**|Devuelve 1 si la columna es de un tipo CLR de longitud fija y 0 en caso contrario.|  
|**source_server**|**sysname**|Nombre del servidor de origen que devuelve la columna en este resultado (si se origina desde un servidor remoto). El nombre se proporciona tal y como aparece en sys. servers.  Devuelve NULL si la columna se origina en el servidor local o si no se puede determinar en qué servidor se origina. Solo se rellena si se solicita buscar información.|  
|**source_database**|**sysname**|Nombre de la base de datos de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la base de datos. Solo se rellena si se solicita buscar información.|  
|**source_schema**|**sysname**|Nombre del esquema de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar el esquema. Solo se rellena si se solicita buscar información.|  
|**source_table**|**sysname**|Nombre de la tabla de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la tabla. Solo se rellena si se solicita buscar información.|  
|**source_column**|**sysname**|Nombre de la columna de origen que devuelve la columna en este resultado. Devuelve NULL si no se puede determinar la columna. Solo se rellena si se solicita buscar información.|  
|**is_identity_column**|**bit**|Devuelve 1 si la columna es una columna de identidad y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna es una columna de identidad.|  
|**is_part_of_unique_key**|**bit**|Devuelve 1 si la columna forma parte de un índice único (que incluye una restricción única y principal) y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna forma parte de un índice único. Solo se rellena si se solicita buscar información.|  
|**is_updateable**|**bit**|Devuelve 1 si la columna es actualizable y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna se puede actualizar.|  
|**is_computed_column**|**bit**|Devuelve 1 si la columna es una columna calculada y 0 de lo contrario. Devuelve NULL si no se puede determinar que la columna es una columna calculada.|  
|**is_sparse_column_set**|**bit**|Devuelve 1 si la columna es una columna dispersa y 0 si no lo es. Devuelve NULL si no se puede determinar que la columna forma parte de un conjunto de columnas dispersas.|  
|**ordinal_in_order_by_list**|**smallint**|La posición de esta columna en la lista ORDER BY devuelve NULL si la columna no aparece en la lista ORDER BY o si no se puede determinar la lista ORDER BY de modo exclusivo.|  
|**order_by_list_length**|**smallint**|Longitud de la lista de ORDER BY. Devuelve NULL si no hay ninguna lista ORDER BY o si no se puede determinar la lista ORDER BY singularmente. Observe que este valor será el mismo para todas las filas devueltas por sp_describe_first_result_set.|  
|**order_by_is_descending**|**smallint NULL**|Si ordinal_in_order_by_list no es NULL, la columna **order_by_is_descending** notifica la dirección de la cláusula ORDER BY para esta columna. De lo contrario, notifica NULL.|  
|**error_number**|**int**|Contiene el número de error devuelto por la función. Contiene NULL si no se produjo ningún error en la columna.|  
|**error_severity**|**int**|Contiene la gravedad devuelta por la función. Contiene NULL si no se produjo ningún error en la columna.|  
|**error_state**|**int**|Contiene el mensaje de estado que devuelve la función. Si no se produjo ningún error, la columna contendrá NULL.|  
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
|12|OBJECT_ID_NOT_SUPPORTED|@object_idNo se admite la pasada a la función (es decir, no es un procedimiento almacenado)|  
|13|OBJECT_ID_DOES_NOT_EXIST|No se ha @object_id pasado a la función en el catálogo del sistema.|  
  
## <a name="permissions"></a>Permisos  
 Requiere permiso para ejecutar el @tsql argumento.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-metadata-with-and-without-browse-information"></a>A. Devolver los metadatos con y sin información de exploración  
 En el ejemplo siguiente se crea un procedimiento almacenado denominado TestProc2 que devuelve dos conjuntos de resultados. A continuación, el ejemplo demuestra que **sys.dm_exec_describe_first_result_set** devuelve información acerca del primer conjunto de resultados en el procedimiento, con y sin la información de exploración.  
  
```  
CREATE PROC TestProc2  
AS  
SELECT object_id, name FROM sys.objects ;  
SELECT name, schema_id, create_date FROM sys.objects ;  
GO  
  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 0) ;  
SELECT * FROM sys.dm_exec_describe_first_result_set_for_object(OBJECT_ID('TestProc2'), 1) ;  
GO  
```  
  
### <a name="b-combining-the-sysdm_exec_describe_first_result_set_for_object-function-and-a-table-or-view"></a>B. Combinar la función sys.dm_exec_describe_first_result_set_for_object y una tabla o vista  
 En el ejemplo siguiente se usa la vista de catálogo del sistema sys. Procedures y la función **Sys.dm_exec_describe_first_result_set_for_object** para mostrar los metadatos de los conjuntos de resultados de todos los procedimientos almacenados en la [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] base de datos.  
  
```  
USE AdventureWorks2012;  
GO  
  
SELECT p.name, r.*   
FROM sys.procedures AS p  
CROSS APPLY sys.dm_exec_describe_first_result_set_for_object(p.object_id, 0) AS r;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [sp_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-first-result-set-transact-sql.md)   
 [sp_describe_undeclared_parameters &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-describe-undeclared-parameters-transact-sql.md)   
 [sys.dm_exec_describe_first_result_set &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-describe-first-result-set-transact-sql.md)  
  
  
