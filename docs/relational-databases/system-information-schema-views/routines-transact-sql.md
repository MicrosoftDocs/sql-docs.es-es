---
description: ROUTINES (Transact-SQL)
title: RUTINAs (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- ROUTINES_TSQL
- ROUTINES
dev_langs:
- TSQL
helpviewer_keywords:
- ROUTINES view
- INFORMATION_SCHEMA.ROUTINES view
ms.assetid: c75561b2-c9a1-48a1-9afa-a5896b6454cf
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 25ea15fd105733e04dd571dc925df394b1520d47
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192086"
---
# <a name="routines-transact-sql"></a>ROUTINES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada procedimiento almacenado y función a los que puede tener acceso el usuario actual en la base de datos actual. Las columnas que describen el valor devuelto solo se aplican a funciones. Para procedimientos almacenados, estas columnas serán NULL.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de INFORMATION_SCHEMA. *view_name*.  
  
> [!NOTE]  
>  La columna ROUTINE_DEFINITION contiene las instrucciones de origen que crearon la función o el procedimiento almacenado. Estas instrucciones de origen sirven, probablemente, para contener los retornos de carro incrustados. Si devuelve esta columna a una aplicación que muestra los resultados en formato de texto, los retornos de carro incrustados en los resultados de ROUTINE_DEFINITION pueden afectar al formato del conjunto de resultados general. Si selecciona la columna ROUTINE_DEFINITION, debe tener en cuenta los retornos de carro incrustados; por ejemplo, devolviendo el conjunto de resultados en una cuadrícula o devolviendo ROUTINE_DEFINITION a su propio cuadro de texto.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|SPECIFIC_CATALOG|**nvarchar (** 128 **)**|Nombre específico del catálogo. Este nombre es el mismo que ROUTINE_CATALOG.|  
|SPECIFIC_SCHEMA|**nvarchar (** 128 **)**|Nombre específico del esquema.<br /><br /> Importante no use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. **\* \* \* \*** INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|SPECIFIC_NAME|**nvarchar (** 128 **)**|Nombre específico del catálogo. Este nombre es el mismo que ROUTINE_NAME.|  
|ROUTINE_CATALOG|**nvarchar (** 128 **)**|Nombre del catálogo de la función.|  
|ROUTINE_SCHEMA|**nvarchar (** 128 **)**|Nombre del esquema que contiene esta función.<br /><br /> Importante no use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. **\* \* \* \*** INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|ROUTINE_NAME|**nvarchar (** 128 **)**|El nombre de la función.|  
|ROUTINE_TYPE|**nvarchar (** 20 **)**|Devuelve PROCEDURE para los procedimientos almacenados y FUNCTION para las funciones.|  
|MODULE_CATALOG|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|MODULE_SCHEMA|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|MODULE_NAME|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|UDT_CATALOG|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|UDT_SCHEMA|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|UDT_NAME|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|DATA_TYPE|**nvarchar (** 128 **)**|Tipo de datos del valor devuelto de la función. Devuelve **TABLE** si es una función con valores de tabla.|  
|CHARACTER_MAXIMUM_LENGTH|**int**|Longitud máxima en caracteres, si el tipo devuelto es un tipo de caracteres.<br /><br /> -1 para **XML** y datos de tipo de valor grande.|  
|CHARACTER_OCTET_LENGTH|**int**|Longitud máxima en bytes, si el tipo devuelto es un tipo de caracteres.<br /><br /> -1 para **XML** y datos de tipo de valor grande.|  
|COLLATION_CATALOG|**nvarchar (** 128 **)**|Siempre devuelve NULL.|  
|COLLATION_SCHEMA|**nvarchar (** 128 **)**|Siempre devuelve NULL.|  
|COLLATION_NAME|**nvarchar (** 128 **)**|Nombre de la intercalación del valor devuelto. Para tipos que no son de caracteres devuelve NULL.|  
|CHARACTER_SET_CATALOG|**nvarchar (** 128 **)**|Siempre devuelve NULL.|  
|CHARACTER_SET_SCHEMA|**nvarchar (** 128 **)**|Siempre devuelve NULL.|  
|CHARACTER_SET_NAME|**nvarchar (** 128 **)**|Nombre del juego de caracteres del valor devuelto. Para tipos que no son de caracteres devuelve NULL.|  
|NUMERIC_PRECISION|**smallint**|Precisión numérica del valor devuelto. En el caso de los tipos no numéricos, devuelve NULL.|  
|NUMERIC_PRECISION_RADIX|**smallint**|Base de la precisión numérica del valor devuelto. Para tipos que no son numéricos, devuelve NULL.|  
|NUMERIC_SCALE|**smallint**|Escala del valor devuelto. Para tipos que no son numéricos, devuelve NULL.|  
|DATETIME_PRECISION|**smallint**|Precisión fraccionaria de un segundo si el valor devuelto es de tipo **DateTime**. De lo contrario, devuelve NULL.|  
|INTERVAL_TYPE|**nvarchar (** 30 **)**|NULL. Reservado para un uso futuro.|  
|INTERVAL_PRECISION|**smallint**|NULL. Reservado para un uso futuro.|  
|TYPE_UDT_CATALOG|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|TYPE_UDT_SCHEMA|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|TYPE_UDT_NAME|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|SCOPE_CATALOG|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|SCOPE_SCHEMA|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|SCOPE_NAME|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|MAXIMUM_CARDINALITY|**bigint**|NULL. Reservado para un uso futuro.|  
|DTD_IDENTIFIER|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|ROUTINE_BODY|**nvarchar (** 30 **)**|Devuelve SQL para una función [!INCLUDE[tsql](../../includes/tsql-md.md)] y EXTERNAL para una función escrita externamente.<br /><br /> Las funciones son siempre SQL.|  
|ROUTINE_DEFINITION|**nvarchar (** 4000 **)**|Devuelve los primeros 4000 caracteres del texto de definición de la función o del procedimiento almacenado, si la función o el procedimiento almacenado no están cifrados. De lo contrario, devuelve NULL.<br /><br /> Para asegurarse de que obtiene la definición completa, consulte la función [OBJECT_DEFINITION](../../t-sql/functions/object-definition-transact-sql.md) o la columna Definition en la vista de catálogo [Sys.sql_modules](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md) .|  
|EXTERNAL_NAME|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|EXTERNAL_LANGUAGE|**nvarchar (** 30 **)**|NULL. Reservado para un uso futuro.|  
|PARAMETER_STYLE|**nvarchar (** 30 **)**|NULL. Reservado para un uso futuro.|  
|IS_DETERMINISTIC|**nvarchar (** 10 **)**|Devuelve YES si la rutina es determinista.<br /><br /> Devuelve NO si la rutina no es determinista.<br /><br /> Siempre devuelve NO para procedimientos almacenados.|  
|SQL_DATA_ACCESS|**nvarchar (** 30 **)**|Devuelve uno de los valores siguientes:<br /><br /> NONE = la función no contiene SQL.<br /><br /> CONTAINS = la función posiblemente contenga SQL.<br /><br /> READS = la función posiblemente lea datos SQL.<br /><br /> MODIFIES = la función posiblemente modifique datos SQL.<br /><br /> Devuelve READS para todas las funciones y MODIFIES para todos los procedimientos almacenados.|  
|IS_NULL_CALL|**nvarchar (** 10 **)**|Indica si se llamará a la rutina si alguno de sus argumentos es NULL.|  
|SQL_PATH|**nvarchar (** 128 **)**|NULL. Reservado para un uso futuro.|  
|SCHEMA_LEVEL_ROUTINE|**nvarchar (** 10 **)**|Devuelve YES si es una función de nivel de esquema o NO si no lo es.<br /><br /> Siempre devuelve YES.|  
|MAX_DYNAMIC_RESULT_SETS|**smallint**|Número máximo de conjuntos de resultados dinámicos devueltos por la rutina.<br /><br /> Devuelve 0 si funciona.|  
|IS_USER_DEFINED_CAST|**nvarchar (** 10 **)**|Devuelve YES si es una función de conversión definida por el usuario y NO si no lo es.<br /><br /> Siempre devuelve NO.|  
|IS_IMPLICITLY_INVOCABLE|**nvarchar (** 10 **)**|Devuelve YES si la rutina se puede invocar implícitamente y NO si no se puede.<br /><br /> Siempre devuelve NO.|  
|CREATED|**datetime**|Hora a la que se creó la rutina.|  
|LAST_ALTERED|**datetime**|La última vez que se modificó la función.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.procedures &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-procedures-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)  
  
