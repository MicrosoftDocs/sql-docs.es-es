---
description: sys.sql_modules (Transact-SQL)
title: sys.sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/06/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.sql_modules_TSQL
- sql_modules
- sql_modules_TSQL
- sys.sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sql_modules catalog view
ms.assetid: 23d3ccd2-f356-4d89-a2cd-bee381243f99
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 58e1f2042f10d1b4f06482fdfb13f07cf5edfa3e
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237707"
---
# <a name="syssql_modules-transact-sql"></a>sys.sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada objeto que es un módulo definido por el lenguaje SQL en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , incluida la función definida por el usuario escalar compilada de forma nativa. Los objetos del tipo P, RF, V, TR, FN, IF, TF y R tienen un módulo SQL asociado. Los valores predeterminados independientes, objetos del tipo D, también incluyen una definición de módulo SQL en esta vista. Para obtener una descripción de estos tipos, vea la columna **Type** de la vista de catálogo [Sys. Objects](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md) .  
  
 Para obtener más información, vea [Funciones escalares definidas por el usuario para OLTP en memoria](../../relational-databases/in-memory-oltp/scalar-user-defined-functions-for-in-memory-oltp.md).  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Id. de objeto del objeto contenedor. Es único en una base de datos.|  
|**definir**|**nvarchar(max)**|Texto SQL que define este módulo. Este valor también se puede obtener mediante la función integrada [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) .<br /><br /> NULL = Cifrado.|  
|**uses_ansi_nulls**|**bit**|Módulo creado con SET ANSI_NULLS ON.<br /><br /> Siempre será = 0 para reglas y valores predeterminados.|  
|**uses_quoted_identifier**|**bit**|Módulo creado con SET QUOTED_IDENTIFIER ON.|  
|**is_schema_bound**|**bit**|El módulo se ha creado con la opción SCHEMABINDING.<br /><br /> Siempre contiene el valor 1 para los procedimientos almacenados generados de forma nativa.|  
|**uses_database_collation**|**bit**|1 = La definición del módulo enlazado a un esquema depende de la intercalación predeterminada de la base de datos para la evaluación correcta; en caso contrario, 0. Este tipo de dependencia impide cambiar la intercalación predeterminada de la base de datos.|  
|**is_recompiled**|**bit**|El procedimiento se ha creado con la opción WITH RECOMPILE.|  
|**null_on_null_input**|**bit**|Módulo declarado para generar una salida NULL en cualquier entrada NULL.|  
|**execute_as_principal_id**|**Int**|Id. de la entidad de seguridad de base de datos EXECUTE AS.<br /><br /> NULL de manera predeterminada o si EXECUTE AS CALLER.<br /><br /> IDENTIFICADOR de la entidad de seguridad especificada si EXECUTe AS SELF o EXECUTe AS \<principal> .<br /><br /> -2 = EXECUTE AS OWNER.|  
|**uses_native_compilation**|**bit**|**Se aplica a**: desde [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] hasta [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)].<br /><br /> 0 = no está compilado de forma nativa<br /><br /> 1 = está compilado de forma nativa<br /><br /> El valor predeterminado es 0.|  
|**is_inlineable**|**bit**|**Válido para** : [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] y versiones posteriores.<br/><br />Indica si el módulo es insertable o no. La inlineity se basa en las condiciones especificadas [aquí](../user-defined-functions/scalar-udf-inlining.md#inlineable-scalar-udfs-requirements).<br /><br /> 0 = no insertable<br /><br /> 1 = es inlineable. <br /><br /> En el caso de las UDF escalares, el valor será 1 si la UDF es insertable y 0 en caso contrario. Siempre contiene un valor de 1 para TVF en línea y 0 para todos los demás tipos de módulos.<br />|  
|**inline_type**|**bit**|**Válido para** : [!INCLUDE[sssql19-md](../../includes/sssql19-md.md)] y versiones posteriores.<br /><br />Indica si la inserción está activada para el módulo actualmente. <br /><br />0 = la inclusión está desactivada<br /><br /> 1 = la inserción está activada.<br /><br /> En el caso de las UDF escalares, el valor será 1 si la inserción está activada (explícita o implícitamente). El valor siempre será 1 para TVF en línea y 0 para otros tipos de módulo.<br />|  

  
## <a name="remarks"></a>Notas  
 La expresión SQL de una restricción predeterminada, objeto de tipo D, se encuentra en la vista de catálogo [Sys.default_constraints](../../relational-databases/system-catalog-views/sys-default-constraints-transact-sql.md) . La expresión SQL de una restricción CHECK, objeto de tipo C, se encuentra en la vista de catálogo [Sys.check_constraints](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md) .  
  
 Esta información también se describe en [sys.dm_db_uncontained_entities &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-uncontained-entities-transact-sql.md).  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se devuelve el nombre, el tipo y la definición de cada módulo de la base de datos actual.  
  
```  
SELECT sm.object_id, OBJECT_NAME(sm.object_id) AS object_name, o.type, o.type_desc, sm.definition  
FROM sys.sql_modules AS sm  
JOIN sys.objects AS o ON sm.object_id = o.object_id  
ORDER BY o.type;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [OLTP en memoria &#40;optimización en memoria&#41;](../../relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
  
