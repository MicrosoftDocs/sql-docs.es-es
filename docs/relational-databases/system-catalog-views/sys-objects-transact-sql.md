---
description: sys.objects (Transact-SQL)
title: sys.objects (Transact-SQL)
ms.custom: ''
ms.date: 01/22/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.objects_TSQL
- objects
- sys.objects
- objects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.objects catalog view
- table-valued parameters, sys.objects catalog view
- user-defined table types [SQL Server]
- table types [SQL Server]
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2b622fc53dce99f624299c9c7fc6e0bede6fade3
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102465053"
---
# <a name="sysobjects-transact-sql"></a>sys.objects (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila para cada objeto de ámbito de esquema definido por el usuario que se crea en una base de datos, incluida la función definida por el usuario escalar compilada de forma nativa.  
  
 Para obtener más información, vea [Funciones escalares definidas por el usuario para OLTP en memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
> [!NOTE]  
>  sys.objects no muestra los desencadenadores DDL, porque no tienen el ámbito de esquema. Todos los desencadenadores, tanto DML como DDL, se encuentran en [Sys. Triggers](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md). Sys. Triggers admite una mezcla de reglas de ámbito de nombres para los distintos tipos de desencadenadores.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**sysname**|Nombre de objeto.|  
|object_id|**int**|Número de identificación del objeto. Es único en una base de datos.|  
|principal_id|**int**|Identificador del propietario individual, si es diferente del propietario del esquema. De forma predeterminada, los objetos contenidos en el esquema pertenecen al propietario del esquema. No obstante, es posible especificar un propietario alternativo mediante la instrucción ALTER AUTHORIZATION para cambiar la propiedad.<br /><br /> Es NULL si no existe ningún propietario individual alternativo.<br /><br /> Es NULL si el tipo de objeto es uno de los siguientes:<br /><br /> C = restricción CHECK<br /><br /> D = DEFAULT (restricción o independiente)<br /><br /> F = Restricción FOREIGN KEY<br /><br /> PK = Restricción PRIMARY KEY<br /><br /> R = Regla (estilo antiguo, independiente)<br /><br /> TA = Desencadenador de ensamblado (integración CLR)<br /><br /> TR = Desencadenador SQL<br /><br /> UQ = Restricción UNIQUE<br /><br /> EC = restricción perimetral |  
|schema_id|**int**|IDENTIFICADOR del esquema en el que se encuentra el objeto.<br /><br /> Los objetos de sistema con ámbito de esquema siempre están contenidos en los esquemas sys o INFORMATION_SCHEMA.|  
|parent_object_id|**int**|Identificador del objeto al que pertenece este objeto.<br /><br /> 0 = No es un objeto secundario.|  
|type|**char(2)**|Tipo de objeto:<br /><br /> AF = Función de agregado (CLR)<br /><br /> C = restricción CHECK<br /><br /> D = DEFAULT (restricción o independiente)<br /><br /> F = Restricción FOREIGN KEY<br /><br /> FN = Función escalar de SQL<br /><br /> FS = Función escalar del ensamblado (CLR)<br /><br /> FT = Función con valores de tabla de ensamblado (CLR)<br /><br /> IF = Función SQL insertada con valores de tabla<br /><br /> IT = tabla interna<br /><br /> P = Procedimiento almacenado de SQL<br /><br /> PC = Procedimiento almacenado del ensamblado (CLR)<br /><br /> PG = Guía de plan<br /><br /> PK = Restricción PRIMARY KEY<br /><br /> R = Regla (estilo antiguo, independiente)<br /><br /> RF = Procedimiento de filtro de replicación<br /><br /> S = Tabla base del sistema<br /><br /> SN = Sinónimo<br /><br /> SO = Objeto de secuencia<br /><br /> U = Tabla (definida por el usuario)<br /><br /> V = Vista<br /><br /> EC = restricción perimetral <br /><br /> <br /><br /> **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> SQ = Cola de servicio<br /><br /> TA = Desencadenador DML del ensamblado (CLR)<br /><br /> TF = Función con valores de tabla SQL<br /><br /> TR = Desencadenador DML de SQL<br /><br /> TT = Tipo de tabla<br /><br /> UQ = Restricción UNIQUE<br /><br /> X = Procedimiento almacenado extendido<br /><br /> <br /><br /> **Se aplica a**: [!INCLUDE[sssql14](../../includes/sssql14-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .<br /><br /> ST = STATS_TREE<br /><br /> <br /><br /> **Se aplica a**: [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] , [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] .<br /><br /> ET = tabla externa|  
|type_desc|**nvarchar(60)**|Descripción del tipo de objeto:<br /><br /> AGGREGATE_FUNCTION<br /><br /> CHECK_CONSTRAINT<br /><br /> CLR_SCALAR_FUNCTION<br /><br /> CLR_STORED_PROCEDURE<br /><br /> CLR_TABLE_VALUED_FUNCTION<br /><br /> CLR_TRIGGER<br /><br /> DEFAULT_CONSTRAINT<br /><br /> EXTENDED_STORED_PROCEDURE<br /><br /> FOREIGN_KEY_CONSTRAINT<br /><br /> INTERNAL_TABLE<br /><br /> PLAN_GUIDE<br /><br /> PRIMARY_KEY_CONSTRAINT<br /><br /> REPLICATION_FILTER_PROCEDURE<br /><br /> RULE<br /><br /> SEQUENCE_OBJECT<br /><br /> <br /><br /> **Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br />  SERVICE_QUEUE<br /><br /> SQL_INLINE_TABLE_VALUED_FUNCTION<br /><br /> SQL_SCALAR_FUNCTION<br /><br /> SQL_STORED_PROCEDURE<br /><br /> SQL_TABLE_VALUED_FUNCTION<br /><br /> SQL_TRIGGER<br /><br /> SYNONYM<br /><br /> SYSTEM_TABLE<br /><br /> TABLE_TYPE<br /><br /> UNIQUE_CONSTRAINT<br /><br /> USER_TABLE<br /><br /> VIEW|  
|create_date|**datetime**|Fecha de creación del objeto.|  
|modify_date|**datetime**|Fecha en que se modificó el objeto por última vez con una instrucción ALTER. Si el objeto es una tabla o una vista, modify_date también cambia cuando se crea o modifica un índice en la tabla o vista.|  
|is_ms_shipped|**bit**|Un componente interno de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] crea el objeto.|  
|is_published|**bit**|El objeto se publica.|  
|is_schema_published|**bit**|Solo se ha publicado el esquema del objeto.|  
  
## <a name="remarks"></a>Observaciones  
 Puede aplicar las funciones integradas [OBJECT_ID](../../t-sql/functions/object-id-transact-sql.md), [OBJECT_NAME](../../t-sql/functions/object-name-transact-sql.md)y [OBJECTPROPERTY](../../t-sql/functions/objectproperty-transact-sql.md)() a los objetos que se muestran en sys. Objects.  
  
 Hay una versión de esta vista con el mismo esquema, denominada [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md), que muestra los objetos del sistema. Hay otra vista llamada [Sys.ALL_OBJECTS](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md) que muestra los objetos de usuario y del sistema. Las tres vistas de catálogo tienen la misma estructura.  
  
 En esta versión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un índice extendido, por ejemplo un índice XML o espacial, se considera como una tabla interna en sys.objects (type = IT y type_desc = INTERNAL_TABLE). En un índice extendido:  
  
-   name es el nombre interno de la tabla de índice.  
  
-   parent_object_id es el object_id de la tabla base.  
  
-   Las columnas is_ms_shipped, is_published y is_schema_published están establecidas en 0.  

**Vistas del sistema útiles relacionadas**  
Los subconjuntos de los objetos se pueden ver mediante las vistas del sistema para un tipo específico de objeto, como:  
- [sys.tables](sys-tables-transact-sql.md)  
- [sys.views](sys-views-transact-sql.md)  
- [sys.procedures](sys-procedures-transact-sql.md)  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
  
### <a name="a-returning-all-the-objects-that-have-been-modified-in-the-last-n-days"></a>A. Devolver todos los objetos modificados en los últimos N días  
 Antes de ejecutar la consulta siguiente, reemplace `<database_name>` y `<n_days>` por valores válidos.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS object_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE modify_date > GETDATE() - <n_days>  
ORDER BY modify_date;  
GO  
```  
  
### <a name="b-returning-the-parameters-for-a-specified-stored-procedure-or-function"></a>B. Devolver los parámetros de un procedimiento almacenado o función determinados  
 Antes de ejecutar la consulta siguiente, reemplace `<database_name>` y `<schema_name.object_name>` por nombres válidos.  
  
```sql  
USE <database_name>;  
GO  
SELECT SCHEMA_NAME(schema_id) AS schema_name  
    ,o.name AS object_name  
    ,o.type_desc  
    ,p.parameter_id  
    ,p.name AS parameter_name  
    ,TYPE_NAME(p.user_type_id) AS parameter_type  
    ,p.max_length  
    ,p.precision  
    ,p.scale  
    ,p.is_output  
FROM sys.objects AS o  
INNER JOIN sys.parameters AS p ON o.object_id = p.object_id  
WHERE o.object_id = OBJECT_ID('<schema_name.object_name>')  
ORDER BY schema_name, object_name, p.parameter_id;  
GO  
```  
  
### <a name="c-returning-all-the-user-defined-functions-in-a-database"></a>C. Devolver todas las funciones definidas por el usuario de una base de datos  
 Antes de ejecutar la consulta siguiente, reemplace `<database_name>` por un nombre de base de datos válido.  
  
```sql  
USE <database_name>;  
GO  
SELECT name AS function_name   
  ,SCHEMA_NAME(schema_id) AS schema_name  
  ,type_desc  
  ,create_date  
  ,modify_date  
FROM sys.objects  
WHERE type_desc LIKE '%FUNCTION%';  
GO  
```  
  
### <a name="d-returning-the-owner-of-each-object-in-a-schema"></a>D. Devolver el propietario de cada objeto de un esquema.  
 Antes de ejecutar la consulta siguiente, reemplace todas las apariciones de `<database_name>` y `<schema_name>` por nombres válidos.  
  
```sql  
USE <database_name>;  
GO  
SELECT 'OBJECT' AS entity_type  
    ,USER_NAME(OBJECTPROPERTY(object_id, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.objects WHERE SCHEMA_NAME(schema_id) = '<schema_name>'  
UNION   
SELECT 'TYPE' AS entity_type  
    ,USER_NAME(TYPEPROPERTY(SCHEMA_NAME(schema_id) + '.' + name, 'OwnerId')) AS owner_name  
    ,name   
FROM sys.types WHERE SCHEMA_NAME(schema_id) = '<schema_name>'   
UNION  
SELECT 'XML SCHEMA COLLECTION' AS entity_type   
    ,COALESCE(USER_NAME(xsc.principal_id),USER_NAME(s.principal_id)) AS owner_name  
    ,xsc.name   
FROM sys.xml_schema_collections AS xsc JOIN sys.schemas AS s  
    ON s.schema_id = xsc.schema_id  
WHERE s.name = '<schema_name>';  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.all_objects &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-objects-transact-sql.md)   
 [sys.system_objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md)   
 [sys.triggers &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-triggers-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [sys.internal_tables &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-internal-tables-transact-sql.md)  
  
  
