---
description: sys.system_sql_modules (Transact-SQL)
title: sys.system_sql_modules (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- system_sql_modules_TSQL
- sys.system_sql_modules
- sys.system_sql_modules_TSQL
- system_sql_modules
dev_langs:
- TSQL
helpviewer_keywords:
- sys.system_sql_modules catalog view
ms.assetid: ad3548bc-4780-4821-b962-b421d52daed9
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 28a554a677c619a0c31c069759487dfa7913635b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190622"
---
# <a name="syssystem_sql_modules-transact-sql"></a>sys.system_sql_modules (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada objeto del sistema que contiene un módulo definido de lenguaje SQL. Los objetos del sistema de tipo FN, IF, P, PC, TF y V tienen un módulo SQL asociado. Para identificar el objeto contenedor, puede combinar esta vista con [sys.system_objects](../../relational-databases/system-catalog-views/sys-system-objects-transact-sql.md).  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Número de identificación del objeto contenedor, único en la base de datos.|  
|**definir**|**nvarchar(max)**|Texto SQL que define este módulo.|  
|**uses_ansi_nulls**|**bit**|1 = El módulo se ha creado con la opción de base de datos SET ANSI_NULLS en ON.<br /><br /> Siempre devuelve 1.|  
|**uses_quoted_identifier**|**bit**|1 = El módulo se ha creado con SET QUOTED_IDENTIFIER en ON.<br /><br /> Siempre devuelve 1.|  
|**is_schema_bound**|**bit**|0 = El módulo no se ha creado con la opción SCHEMABINDING.<br /><br /> Siempre devuelve 0.|  
|**uses_database_collation**|**bit**|0 = El módulo no depende de la intercalación predeterminada de la base de datos.<br /><br /> Siempre devuelve 0.|  
|**is_recompiled**|**bit**|0 = El procedimiento no se ha creado con la opción WITH RECOMPILE.<br /><br /> Siempre devuelve 0.|  
|**null_on_null_input**|**bit**|0 = El módulo no se ha creado para generar una salida NULL en cualquier entrada NULL.<br /><br /> Siempre devuelve 0.|  
|**execute_as_principal_id**|**int**|Siempre devuelve NULL.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.all_sql_modules &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-sql-modules-transact-sql.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md) (Vistas de catálogo de objetos [Transact-SQL])  
  
  
