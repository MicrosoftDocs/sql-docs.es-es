---
description: sys.selective_xml_index_paths (Transact-SQL)
title: sys.selective_xml_index_paths (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- xml_schema_attributes_TSQL
- xml_schema_attributes
- sys.xml_schema_attributes_TSQL
- sys.xml_schema_attributes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_schema_attributes catalog view
ms.assetid: 07a73d71-ec3e-4894-947a-5859ca62c606
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 4a768ded6ef89a54f35e7ce30a64bc3dd6e277ac
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182475"
---
# <a name="sysselective_xml_index_paths-transact-sql"></a>sys.selective_xml_index_paths (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  A partir de [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 1, cada fila de sys.selective_xml_index_paths representa una ruta de acceso promocionada para un índice xml selectivo concreto.  
  
 Si crea un índice xml selectivo en xmlcol o la tabla T con la instrucción siguiente,  
  
```  
CREATE SELECTIVE XML INDEX sxi1 ON T(xmlcol)   
FOR ( path1 = '/a/b/c' AS XQUERY 'xs:string',  
      path2 = '/a/b/d' AS XQUERY 'xs:double'  
    )  
```  
  
 Habrá dos filas nuevas en sys.selective_xml_index_paths correspondientes al índice sxi1.  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador de la tabla con la columna XML.|  
|**id_de_índice**|**int**|Identificador único del índice xml selectivo.|  
|**path_id**|**int**|Identificador de la ruta de acceso XML promocionado.|  
|**path**|**nvarchar(4000)**|Ruta de acceso promocionada. Por ejemplo, '/a/b/c/d/e'.|  
|**name**|**sysname**|Nombre de ruta de acceso.|  
|**path_type**|**tinyint**|0 = XQUERY<br /><br /> 1 = SQL|  
|**path_type_desc**|**sysname**|Basado en **path_type** valor ' XQUERY ' o ' SQL '.|  
|**xml_component_id**|**int**|Id. único del componente del esquema XML en la base de datos.|  
|**xquery_type_description**|**nvarchar(4000)**|Nombre del tipo xsd especificado.|  
|**is_xquery_type_inferred**|**bit**|1 = el tipo se infiere.|  
|**xquery_max_length**|**smallint**|Longitud máxima (en carácter de tipo xsd).|  
|**is_xquery_max_length_inferred**|**bit**|1 = la longitud máxima se infiere.|  
|**is_node**|**bit**|0 = la sugerencia de node() no está presente.<br /><br /> 1 = se aplica la sugerencia de optimización de node().|  
|**system_type_id**|**tinyint**|Id. del tipo de sistema de la columna.|  
|**user_type_id**|**tinyint**|Identificador del tipo de usuario de la columna.|  
|**max_length**|**smallint**|Longitud máxima (en bytes) del tipo.<br /><br /> -1 = El tipo de datos de las columnas es varchar(max), nvarchar(max), varbinary(max) o xml.|  
|**precisión**|**tinyint**|Precisión máxima del tipo si está basado en numerales. De lo contrario, es 0.|  
|**scale**|**tinyint**|Escala máxima del tipo si está basado en numerales. De lo contrario, es 0.|  
|**collation_name**|**sysname**|Nombre de la intercalación del tipo si está basado en caracteres. En caso contrario, NULL.|  
|**is_singleton**|**bit**|0 = la sugerencia SINGLETON no está presente.<br /><br /> 1 = Se aplica la sugerencia de optimización SINGLETON.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Esquemas XML &#40;las vistas de catálogo del sistema de tipos XML&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/xml-schemas-xml-type-system-catalog-views-transact-sql.md)  
  
  
