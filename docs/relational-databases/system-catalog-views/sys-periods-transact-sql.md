---
description: Sys. periods (Transact-SQL)
title: Sys. periods (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 25e66ed3-2270-4c5c-9f5a-2c0f165a57ca
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 0e74808f16b7b287544d11d209d4da94d8485b3b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102463878"
---
# <a name="sysperiods-transact-sql"></a>Sys. periods (Transact-SQL)
[!INCLUDE [sqlserver2016](../../includes/applies-to-version/sqlserver2016.md)]

  Devuelve una fila por cada tabla para la que se han definido períodos.  
  
|Encabezado de columna|Tipo de datos|Descripción|  
|-------------------|---------------|-----------------|  
|name|**sysname**|Nombre del período|  
|period_type|**tinyint**|Valor numérico que representa el tipo de punto:<br /><br /> 1 = período de tiempo del sistema|  
|period_type_desc|**nvarchar(60)**|La descripción de texto del tipo de columna:<br /><br /> SYSTEM_TIME_PERIOD|  
|object_id|**int**|Identificador de la tabla que contiene la columna de period_type|  
|start_column_id|**int**|Identificador de la columna que define el límite del período inferior.|  
|end_column_id|**int**|Identificador de la columna que define el límite del período superior.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.all_columns &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-all-columns-transact-sql.md)   
 [sys.system_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-system-columns-transact-sql.md)   
 [Preguntas más frecuentes sobre el catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)   
 [Tablas temporales](../../relational-databases/tables/temporal-tables.md)  
  
