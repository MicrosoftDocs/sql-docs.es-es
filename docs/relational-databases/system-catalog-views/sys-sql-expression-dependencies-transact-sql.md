---
description: sys.sql_expression_dependencies (Transact-SQL)
title: sys.sql_expression_dependencies (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_expression_dependencies
- sql_expression_dependencies_TSQL
- sql_expression_dependencies
- sys.sql_expression_dependencies_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_expression_dependencies catalog view
ms.assetid: 78a218e4-bf99-4a6a-acbf-ff82425a5946
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 34c88c909239e7f9d9fcb12e5b30677708d9cfbc
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210327"
---
# <a name="syssql_expression_dependencies-transact-sql"></a>sys.sql_expression_dependencies (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  Contiene una fila para cada dependencia por nombre en una entidad definida por el usuario en la base de datos actual. Esto incluye las dependencias entre las funciones escalares definidas por el usuario y los módulos que se compilan de forma nativa [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se crea una dependencia entre dos entidades cuando una entidad, denominada entidad a la *que se hace referencia*, aparece por nombre en una expresión SQL persistente de otra entidad, denominada *entidad de referencia*. Por ejemplo, si en la definición de una vista se hace referencia a una tabla, la vista, como entidad que hace la referencia, depende de la tabla, la entidad a la que se hace referencia. Si desapareciera la tabla, la vista sería inservible.  
  
 Para obtener más información, vea [Funciones escalares definidas por el usuario para OLTP en memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
 Puede utilizar esta vista de catálogo para notificar información de dependencia de las entidades siguientes:  
  
-   Entidades enlazadas a un esquema.  
  
-   Entidades no enlazadas a un esquema.  
  
-   Entidades entre servidores y entre bases de datos. Se notifican los nombres de entidad; en cambio, no se resuelve el identificador de la entidad.  
  
-   Dependencias de nivel de columna en entidades enlazadas a un esquema. Las dependencias de nivel de columna para objetos no enlazados a un esquema pueden devolverse mediante [Sys.dm_sql_referenced_entities](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md).  
  
-   Desencadenadores DDL de nivel de servidor en el contexto de la base de datos maestra.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|referencing_id|**int**|Identificador de la entidad que hace la referencia. No admite valores NULL.|  
|referencing_minor_id|**int**|Identificador de la columna cuando la entidad de referencia es una columna; en caso contrario, es 0. No admite valores NULL.|  
|referencing_class|**tinyint**|Clase de la entidad que hace la referencia.<br /><br /> 1 = Objeto o columna<br /><br /> 12 = Desencadenador DDL de base de datos<br /><br /> 13 = Desencadenador DDL de servidor<br /><br /> No admite valores NULL.|  
|referencing_class_desc|**nvarchar(60)**|Descripción de la clase de la entidad que hace la referencia.<br /><br /> OBJECT_OR_COLUMN<br /><br /> DATABASE_DDL_TRIGGER<br /><br /> SERVER_DDL_TRIGGER<br /><br /> No admite valores NULL.|  
|is_schema_bound_reference|**bit**|1 = La entidad a la que se hace referencia está enlazada a un esquema.<br /><br /> 0 = La entidad a la que se hace referencia no está enlazada a un esquema.<br /><br /> No admite valores NULL.|  
|referenced_class|**tinyint**|Clase de la entidad a la que se hace referencia.<br /><br /> 1 = Objeto o columna<br /><br /> 6 = Tipo<br /><br /> 10 = Colección de esquemas XML<br /><br /> 21 = Función de partición<br /><br /> No admite valores NULL.|  
|referenced_class_desc|**nvarchar(60)**|Descripción de la clase de la entidad a la que se hace referencia.<br /><br /> OBJECT_OR_COLUMN<br /><br /> TYPE<br /><br /> XML_SCHEMA_COLLECTION<br /><br /> PARTITION_FUNCTION<br /><br /> No admite valores NULL.|  
|referenced_server_name|**sysname**|Nombre del servidor de la entidad a la que se hace referencia.<br /><br /> Esta columna se rellena para las dependencias entre servidores especificadas con un nombre de cuatro partes válido. Para obtener información sobre los nombres de varias partes, vea [convenciones de sintaxis de Transact-sql &#40;&#41;de Transact-SQL ](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).<br /><br /> NULL para las entidades no enlazadas a un esquema a las que se hizo referencia sin especificar un nombre de cuatro partes.<br /><br /> NULL para las entidades enlazadas a esquema porque deben estar en la misma base de datos y, por tanto, solo se pueden definir mediante un nombre de dos partes (*Schema. Object*).|  
|referenced_database_name|**sysname**|Nombre de la base de datos de la entidad a la que se hace referencia.<br /><br /> Esta columna se rellena para las referencias entre bases de datos o entre servidores especificadas con un nombre válido de tres o cuatro partes.<br /><br /> NULL para las referencias no enlazadas a esquema especificadas con un nombre de una o dos partes.<br /><br /> NULL para las entidades enlazadas a esquema porque deben estar en la misma base de datos y, por tanto, solo se pueden definir mediante un nombre de dos partes (*Schema. Object*).|  
|referenced_schema_name|**sysname**|Esquema al que pertenece la entidad a la que se hace referencia.<br /><br /> NULL para las referencias no enlazadas a esquema en las que se hacía referencia a la entidad sin especificar el nombre del esquema.<br /><br /> Nunca es NULL para las referencias enlazadas a un esquema porque las entidades enlazadas a un esquema deben definirse y hacerse referencia con un nombre de dos partes.|  
|referenced_entity_name|**sysname**|Nombre de la entidad a la que se hace referencia. No admite valores NULL.|  
|referenced_id|**int**|Identificador de la entidad a la que se hace referencia. El valor de esta columna nunca es NULL para las referencias enlazadas a esquema. El valor de esta columna siempre es NULL para las referencias entre bases de datos y entre servidores.<br /><br /> NULL para las referencias dentro de la base de datos si no se puede determinar el identificador. Para las referencias no enlazadas a un esquema, no se puede resolver el identificador en los casos siguientes:<br /><br /> La entidad a la que se hace referencia no existe en la base de datos.<br /><br /> El esquema de la entidad a la que se hace referencia depende del esquema del autor de la llamada y se resuelve en tiempo de ejecución. En este caso, is_caller_dependent se establece en 1.|  
|referenced_minor_id|**int**|Identificador de la columna a la que se hace referencia cuando la entidad que hace la referencia es una columna; en caso contrario, es 0. No admite valores NULL.<br /><br /> Una entidad a la que se hace referencia es una columna cuando una columna se identifica mediante un nombre en la entidad de referencia o cuando la entidad primaria se usa en una instrucción SELECT *.|  
|is_caller_dependent|**bit**|Indica que el enlace de esquema de la entidad a la que se hace referencia se realiza en tiempo de ejecución; por consiguiente, la resolución del identificador de la entidad depende del esquema del autor de la llamada. Esto se produce cuando la entidad a la que se hace referencia es un procedimiento almacenado, un procedimiento almacenado extendido o una función definida por el usuario no enlazada a un esquema llamada en una instrucción EXECUTE.<br /><br /> 1 = la entidad a la que se hace referencia es dependiente del autor de la llamada y se resuelve en tiempo de ejecución. En este caso, referenced_id es NULL.<br /><br /> 0 = El identificador de la entidad a la que se hace referencia no es dependiente del autor de la llamada.<br /><br /> Es siempre 0 para las referencias enlazadas a esquema y para las referencias entre bases de datos o entre servidores que especifican explícitamente un nombre de esquema. Por ejemplo, una referencia a una entidad con el formato `EXEC MyDatabase.MySchema.MyProc` no es dependiente del autor de la llamada. Sin embargo, una referencia con el formato `EXEC MyDatabase..MyProc` es dependiente del autor de la llamada.|  
|is_ambiguous|**bit**|Indica que la referencia es ambigua y se puede resolver en tiempo de ejecución en una función definida por el usuario, un tipo definido por el usuario (UDT) o una referencia XQuery a una columna de tipo **XML**.<br /><br /> Por ejemplo, suponga que la instrucción `SELECT Sales.GetOrder() FROM Sales.MySales` está definida en un procedimiento almacenado. Hasta que no se ejecute el procedimiento almacenado, no se sabrá si `Sales.GetOrder()` es una función definida por el usuario en el esquema `Sales` o en la columna con nombre `Sales` de tipo UDT con un método denominado `GetOrder()`.<br /><br /> 1 = La referencia es ambigua.<br /><br /> 0 = La referencia no es ambigua o la entidad puede enlazarse correctamente cuando se llama a la vista.<br /><br /> Siempre es 0 para las referencias enlazadas a un esquema.|  
  
## <a name="remarks"></a>Observaciones  
 La tabla siguiente enumera los tipos de entidades para las que se crea y mantiene la información de dependencia. La información de dependencia no se crea ni mantiene para reglas, valores predeterminados, tablas temporales, procedimientos almacenados temporales u objetos del sistema.  

> [!NOTE]
> Azure Synapse Analytics y almacenamiento de datos paralelos admiten tablas, vistas, estadísticas filtradas y tipos de entidad de procedimientos almacenados de Transact-SQL de esta lista.  La información de dependencia se crea y se mantiene solo para las tablas, las vistas y las estadísticas filtradas.  
  
|Tipo de entidad|Entidad que hace la referencia|Entidad a la que se hace referencia|  
|-----------------|------------------------|-----------------------|  
|Tabla|Sí*|Sí|  
|Ver|Sí|Sí|  
|Índice filtrado|Sí**|No|  
|Estadísticas filtradas|Sí**|No|  
|[!INCLUDE[tsql](../../includes/tsql-md.md)] procedimiento almacenado * * _|Sí|Sí|  
|procedimiento almacenado CLR|No|Sí|  
|Función definida por el usuario de [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sí|Sí|  
|Función CLR definida por el usuario|No|Sí|  
|Desencadenador CLR (DML y DDL)|No|No|  
|Desencadenador DML de [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sí|No|  
|Desencadenador DDL de nivel de base de datos de [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sí|No|  
|Desencadenador DDL de nivel de servidor de [!INCLUDE[tsql](../../includes/tsql-md.md)]|Sí|No|  
|Procedimientos almacenados extendidos|No|Sí|  
|Cola|No|Sí|  
|Synonym (Sinónimo)|No|Sí|  
|Tipo (tipo CLR y alias definido por el usuario)|No|Sí|  
|Colección de esquemas XML|No|Sí|  
|Función de partición|No|Sí|  
  
 \_ Se realiza un seguimiento de una tabla como una entidad de referencia solo cuando hace referencia a un [!INCLUDE[tsql](../../includes/tsql-md.md)] módulo, un tipo definido por el usuario o una colección de esquemas XML en la definición de una columna calculada, una restricción check o una restricción default.  
  
 ** Se realiza el seguimiento de cada una de las columnas usadas en el predicado de filtro como una entidad de referencia.  
  
 No se realiza el seguimiento de los procedimientos almacenados numerados con un valor entero mayor que 1 como una entidad que hace referencia o a la que se hace referencia.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso VIEW DEFINITION en la base de datos y el permiso SELECT en sys.sql_expression_dependencies para la base de datos. De forma predeterminada, solo se permite el permiso SELECT a los miembros del rol fijo de base de datos db_owner. Si se conceden los permisos SELECT y VIEW DEFINITION a otro usuario, el receptor puede ver todas las dependencias de la base de datos.  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-entities-that-are-referenced-by-another-entity"></a>A. Devolver las entidades a las que otra entidad hace referencia  
 El ejemplo siguiente devuelve las tablas y columnas a las que se hace referencia en la vista `Production.vProductAndDescription`. La vista depende de las entidades (tablas y columnas) devueltas en `referenced_entity_name` y las columnas `referenced_column_name`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
GO  
  
```  
  
### <a name="b-returning-entities-that-reference-another-entity"></a>B. Devolver las entidades que hacen referencia a otra entidad  
 El ejemplo siguiente devuelve las entidades que hacen referencia a la tabla `Production.Product`. Las entidades devueltas en la columna `referencing_entity_name` dependen de la tabla `Product`.  
  
```  
USE AdventureWorks2012;  
GO  
SELECT OBJECT_SCHEMA_NAME ( referencing_id ) AS referencing_schema_name,  
    OBJECT_NAME(referencing_id) AS referencing_entity_name,   
    o.type_desc AS referencing_desciption,   
    COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
    referencing_class_desc, referenced_class_desc,  
    referenced_server_name, referenced_database_name, referenced_schema_name,  
    referenced_entity_name,   
    COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
    is_caller_dependent, is_ambiguous  
FROM sys.sql_expression_dependencies AS sed  
INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
WHERE referenced_id = OBJECT_ID(N'Production.Product');  
GO  
  
```  
  
### <a name="c-returning-cross-database-dependencies"></a>C. Devolver las dependencias entre bases de datos  
 El siguiente ejemplo devuelve todas las dependencias entre bases de datos. El ejemplo crea primero la base de datos `db1` y dos procedimientos almacenados que hacen referencia a tablas en las bases de datos `db2` y `db3`. A continuación, se consulta la tabla `sys.sql_expression_dependencies` para crear informes de las dependencias entre bases de datos entre los procedimientos y las tablas. Observe que NULL se devuelve en la columna `referenced_schema_name` para la entidad a la que se hace referencia `t3` porque no se especificó un nombre de esquema para esa entidad en la definición del procedimiento.  
  
```  
CREATE DATABASE db1;  
GO  
USE db1;  
GO  
CREATE PROCEDURE p1 AS SELECT _ FROM db2.s1.t1;  
GO  
CREATE PROCEDURE p2 AS  
    UPDATE db3..t3  
    SET c1 = c1 + 1;  
GO  
SELECT OBJECT_NAME (referencing_id),referenced_database_name,   
    referenced_schema_name, referenced_entity_name  
FROM sys.sql_expression_dependencies  
WHERE referenced_database_name IS NOT NULL;  
GO  
USE master;  
GO  
DROP DATABASE db1;  
GO  
  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.dm_sql_referenced_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referenced-entities-transact-sql.md)   
 [sys.dm_sql_referencing_entities &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-sql-referencing-entities-transact-sql.md)  
  
  
