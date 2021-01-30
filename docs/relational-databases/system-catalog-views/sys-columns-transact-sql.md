---
description: sys.columns (Transact-SQL)
title: Sys. Columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 11/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.columns_TSQL
- sys.columns
- columns_TSQL
- columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.columns catalog view
ms.assetid: 323ac9ea-fc52-4b8c-8a7e-e0e44f8ed86c
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0c82800b53596af6ab281c73183e9dbb772cf208
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200912"
---
# <a name="syscolumns-transact-sql"></a>sys.columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila para cada columna de un objeto que incluye columnas, como vistas o tablas. La siguiente lista incluye tipos de objetos que contienen columnas:  
  
-   Funciones de ensamblado con valores de tabla (FT)  
  
-   Funciones SQL con valores de tabla insertados (IF)  
  
-   Tablas internas (IT)  
  
-   Tablas del sistema (S)  
  
-   Funciones SQL con valores de tabla (TF)  
  
-   Tablas de usuario (U)  
  
-   Vistas (V)  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|object_id|**int**|Identificador del objeto al que pertenece esta columna.|  
|name|**sysname**|Nombre de la columna. Es único en el objeto.|  
|column_id|**int**|Identificador de la columna. Es único en el objeto.<br /><br /> Los Id. de columna no tienen que ser secuenciales.|  
|system_type_id|**tinyint**|Id. del tipo de sistema de la columna.|  
|user_type_id|**int**|Id. del tipo de la columna, tal como lo ha definido el usuario.<br /><br /> Para devolver el nombre del tipo, únase a la vista de catálogo [Sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) en esta columna.|  
|max_length|**smallint**|Longitud máxima de la columna, en bytes.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor max_length será 16 o el valor establecido por sp_tableoption ' Text in row '.|  
|Precisión|**tinyint**|Precisión de la columna, si está basada en números; de lo contrario, es 0.|  
|scale|**tinyint**|La escala de la columna se basa en valores numéricos; en caso contrario, es 0.|  
|collation_name|**sysname**|Nombre de la intercalación de la columna, si está basada en caracteres; de lo contrario, es NULL.|  
|is_nullable|**bit**|1 = La columna acepta valores NULL.|  
|is_ansi_padded|**bit**|1 = La columna utiliza el comportamiento ANSI_PADDING ON si es de tipo character, binary o variant.<br /><br /> 0 = La columna no es de tipo character, binary o variant.|  
|is_rowguidcol|**bit**|1 = La columna se ha declarado como ROWGUIDCOL.|  
|is_identity|**bit**|1 = La columna tiene valores de identidad.|  
|is_computed|**bit**|1 = La columna es una columna calculada.|  
|is_filestream|**bit**|1 = La columna es una columna FILESTREAM.|  
|is_replicated|**bit**|1 = La columna está replicada.|  
|is_non_sql_subscribed|**bit**|1 = La columna tiene un suscriptor que no es de SQL Server.|  
|is_merge_published|**bit**|1 = La columna está publicada para mezcla.|  
|is_dts_replicated|**bit**|1 = La columna se replica con [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|is_xml_document|**bit**|1 = El contenido es un documento XML completo.<br /><br /> 0 = el contenido es un fragmento de documento o el tipo de datos de la columna no es **XML**.|  
|xml_collection_id|**int**|Es distinto de cero si el tipo de datos de la columna es **XML** y se escribe el XML. El valor será el ID. de la colección que contiene el espacio de nombres del esquema XML de validación de la columna.<br /><br /> 0 = No es una colección de esquemas XML.|  
|default_object_id|**int**|IDENTIFICADOR del objeto predeterminado, independientemente de si se trata de un objeto independiente [Sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)o una restricción de nivel de columna insertada y predeterminada. La columna parent_object_id de un objeto predeterminado de nivel de columna insertada es una referencia a la propia tabla.<br /><br /> 0 = No hay un valor predeterminado.|  
|rule_object_id|**int**|Id. de la regla independiente enlazada a la columna mediante sys.sp_bindrule.<br /><br /> 0 = No hay ninguna regla independiente. Para las restricciones CHECK de nivel de columna, vea [sys.check_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = La columna es una columna dispersa. Para obtener más información, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = La columna es un conjunto de columnas. Para obtener más información, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|  
|generated_always_type|**tinyint**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Identifica Cuándo se genera el valor de la columna (siempre será 0 para las columnas de las tablas del sistema):<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END<br /><br /> Para obtener más información, vea [tablas temporales &#40;bases de datos relacionales&#41;](../../relational-databases/tables/temporal-tables.md).|  
|generated_always_type_desc|**nvarchar(60)**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Descripción textual del `generated_always_type` valor de (siempre NOT_APPLICABLE para las columnas de las tablas del sistema) <br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END|  
|encryption_type|**int**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Tipo de cifrado:<br /><br /> 1 = cifrado determinista<br /><br /> 2 = cifrado aleatorio|  
|encryption_type_desc|**nvarchar (64)**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Descripción del tipo de cifrado:<br /><br /> ALEATORIO<br /><br /> DETERMINISTIC|  
|encryption_algorithm_name|**sysname**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Nombre del algoritmo de cifrado.<br /><br /> Solo se admite AEAD_AES_256_CBC_HMAC_SHA_512.|  
|column_encryption_key_id|**int**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> IDENTIFICADOR de CEK.|  
|column_encryption_key_database_name|**sysname**|**Se aplica a**: [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)] y versiones posteriores, [!INCLUDE[ssSDW_md](../../includes/sssds-md.md)]<br /><br /> Nombre de la base de datos en la que existe la clave de cifrado de columna si es diferente de la base de datos de la columna. ES NULL si la clave existe en la misma base de datos que la columna.|  
|is_hidden|**bit**|**Se aplica a**: [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Indica si la columna está oculta:<br /><br /> 0 = columna visible normal, no oculta<br /><br /> 1 = columna oculta|  
|is_masked|**bit**|**Se aplica a**: [!INCLUDE[ssCurrentLong](../../includes/sscurrent-md.md)] y versiones posteriores, [!INCLUDE[ssSDS_md](../../includes/sssds-md.md)]<br /><br /> Indica si la columna está enmascarada por un enmascaramiento dinámico de datos:<br /><br /> 0 = columna normal, no enmascarada<br /><br /> 1 = la columna está enmascarada|  


 
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)  
  
