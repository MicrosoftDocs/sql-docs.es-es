---
description: sys.pdw_nodes_columns (Transact-SQL)
title: sys.pdw_nodes_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 268c77b7-1d71-4197-a2ed-5e2b2b8fc260
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 5e785b80629043772474eaa31a2cae5f93c742e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99186514"
---
# <a name="syspdw_nodes_columns-transact-sql"></a>sys.pdw_nodes_columns (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Muestra las columnas para las tablas definidas por el usuario y las vistas definidas por el usuario.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|object_id|**int**|Identificador del objeto al que pertenece esta columna.||  
|name|**sysname**|Nombre de la columna. Único en el objeto.||  
|column_id|**int**|Identificador de la columna. Único en el objeto.||  
|system_type_id|**tinyint**|Id. del tipo de sistema de la columna.||  
|user_type_id|**int**|Id. del tipo de la columna, tal como lo ha definido el usuario.||  
|max_length|**smallint**|Longitud máxima de la columna, en bytes.|Incluye-1 (no válido) para los tipos de columna no admitidos.|  
|Precisión|**tinyint**|Precisión de la columna, si está basada en números; de lo contrario, es 0.||  
|scale|**tinyint**|La escala de la columna se basa en valores numéricos; en caso contrario, es 0.||  
|collation_name|**sysname**|Nombre de la intercalación de la columna, si está basada en caracteres; de lo contrario, es NULL.||  
|is_nullable|**bit**|1 = La columna acepta valores NULL.||  
|is_ansi_padded|**bit**|1 = La columna utiliza el comportamiento ANSI_PADDING ON si es de tipo character, binary o variant.|Siempre es 0.|  
|is_rowguidcol|**bit**|1 = La columna se ha declarado como ROWGUIDCOL.|Siempre es 0.|  
|is_identity|**bit**|1 = la columna tiene valores de identidad.|Siempre es 0.|  
|is_computed|**bit**|1 = La columna es una columna calculada.|Siempre es 0.|  
|is_filestream|**bit**|1 = La columna es una columna FILESTREAM.|Siempre es 0.|  
|is_replicated|**bit**|1 = La columna está replicada.|Siempre es 0.|  
|is_non_sql_subscribed|**bit**|1 = la columna tiene un suscriptor que no es de SQL.|Siempre es 0.|  
|is_merge_published|**bit**|1 = La columna está publicada para mezcla.|Siempre es 0.|  
|is_dts_replicated|**bit**|1 = la columna se replica mediante SSIS.|Siempre es 0.|  
|is_xml_document|**bit**|1 = El contenido es un documento XML completo.|Siempre es 0.|  
|xml_collection_id|**int**|0 = No es una colección de esquemas XML.|Siempre es 0.|  
|default_object_id|**int**|IDENTIFICADOR del objeto predeterminado; 0 = no hay ningún valor predeterminado.|Siempre es 0.|  
|rule_object_id|**int**|IDENTIFICADOR de la regla independiente enlazada a la columna. <br />0 = No hay ninguna regla independiente.|Siempre es 0.|  
|is_sparse|**bit**|1 = La columna es una columna dispersa.|Siempre es 0.|  
|is_column_set|**bit**|1 = La columna es un conjunto de columnas.|Siempre es 0.|  
|pdw_node_id|**int**|Identificador único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|NOT NULL|  
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso CONTROL SERVER.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)  
  
  
