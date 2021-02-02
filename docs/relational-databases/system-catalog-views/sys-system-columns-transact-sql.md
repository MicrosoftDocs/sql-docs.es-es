---
description: sys.system_columns (Transact-SQL)
title: sys.system_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- system_columns_TSQL
- system_columns
- sys.system_columns
- sys.system_columns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_columns catalog view
ms.assetid: 4ab1d48a-d57a-4e76-a08c-9627eeaf4588
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 92d65f7307e96cb1261688a5da3443c3d4861f84
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236166"
---
# <a name="syssystem_columns-transact-sql"></a>sys.system_columns (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Contiene una fila para cada columna de objetos de sistema que tienen columnas.  
  
|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador del objeto al que pertenece esta columna.|  
|**name**|**sysname**|Nombre de la columna. Es único en el objeto.|  
|**column_id**|**int**|Identificador de la columna. Es único en el objeto.<br /><br /> Los Id. de columna no tienen que ser secuenciales.|  
|**system_type_id**|**tinyint**|Id. del tipo de sistema de la columna|  
|**user_type_id**|**int**|Id. del tipo de la columna, tal como lo ha definido el usuario.<br /><br /> Para devolver el nombre del tipo, únase a la vista de catálogo [Sys. Types](../../relational-databases/system-catalog-views/sys-types-transact-sql.md) en esta columna.|  
|**max_length**|**smallint**|Longitud máxima (en bytes) de la columna.<br /><br /> -1 = el tipo de datos de la columna es **VARCHAR (Max)**, **nvarchar (Max)**, **varbinary (Max)** o **XML**.<br /><br /> En el caso de las columnas de **texto** , el valor **max_length** será 16 o el valor establecido por **sp_tableoption** ' Text in row '.|  
|**precisión**|**tinyint**|Precisión de la columna, si está basada en números; de lo contrario, es 0.|  
|**scale**|**tinyint**|Escala de la columna, si está basada en números; en caso contrario, es 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación de la columna, si está basada en caracteres; de lo contrario, es NULL.|  
|**is_nullable**|**bit**|1 = La columna acepta valores NULL.|  
|**is_ansi_padded**|**bit**|1 = La columna utiliza el comportamiento ANSI_PADDING ON si es de tipo character, binary o variant.<br /><br /> 0 = La columna no es de tipo character, binary o variant.|  
|**is_rowguidcol**|**bit**|1 = La columna se ha declarado como ROWGUIDCOL.|  
|**is_identity**|**bit**|1 = la columna tiene valores de identidad.|  
|**is_computed**|**bit**|1 = La columna es una columna calculada.|  
|**is_filestream**|**bit**|1 = La columna se ha declarado para utilizar el almacenamiento FILESTREAM.|  
|**is_replicated**|**bit**|1 = La columna está replicada.|  
|**is_non_sql_subscribed**|**bit**|1 = La columna tiene un suscriptor que no es de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**is_merge_published**|**bit**|1 = La columna está publicada para mezcla.|  
|**is_dts_replicated**|**bit**|1 = La columna se replica con [!INCLUDE[ssIS](../../includes/ssis-md.md)].|  
|**is_xml_document**|**bit**|1 = El contenido es un documento XML completo.<br /><br /> 0 = el contenido es un fragmento de documento o el tipo de datos de la columna no es **XML**.|  
|**xml_collection_id**|**int**|Distinto de cero si el tipo de datos de la columna es **XML** y se ha escrito el XML. El valor será el ID. de la colección que contiene el espacio de nombres del esquema XML de validación de la columna.<br /><br /> 0 = No es una colección de esquemas XML.|  
|**default_object_id**|**int**|IDENTIFICADOR del objeto predeterminado, independientemente de si se trata de un [Sys.sp_bindefault](../../relational-databases/system-stored-procedures/sp-bindefault-transact-sql.md)independiente o una restricción predeterminada de nivel de columna insertada. La columna **parent_object_id** de un objeto predeterminado de nivel de columna insertado es una referencia a la propia tabla. O bien, es 0 si no hay valor predeterminado.|  
|**rule_object_id**|**int**|IDENTIFICADOR de la regla independiente enlazada a la columna mediante **Sys.sp_bindrule**.<br /><br /> 0 = No hay ninguna regla independiente.<br /><br /> Para las restricciones CHECK de nivel de columna, vea [sys.check_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md).|  
|is_sparse|**bit**|1 = La columna es una columna dispersa. Para obtener más información, vea [Usar columnas dispersas](../../relational-databases/tables/use-sparse-columns.md).|  
|is_column_set|**bit**|1 = La columna es un conjunto de columnas. Para obtener más información, vea [Usar conjuntos de columnas](../../relational-databases/tables/use-column-sets.md).|  
|generated_always_type|**tinyint**|Valor numérico que representa el tipo de columna:<br /><br /> 0 = NOT_APPLICABLE<br /><br /> 1 = AS_ROW_START<br /><br /> 2 = AS_ROW_END|  
|generated_always_type_desc|**nvarchar(60)**|La descripción de texto del tipo de columna:<br /><br /> NOT_APPLICABLE<br /><br /> AS_ROW_START<br /><br /> AS_ROW_END<br /><br /> **Válido para** : [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.md)   
 [sys.columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-columns-transact-sql.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.computed_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-computed-columns-transact-sql.md)  
  
  
