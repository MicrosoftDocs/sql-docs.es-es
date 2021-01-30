---
description: TABLES (Transact-SQL)
title: TABLAS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- TABLES_TSQL
- TABLES
dev_langs:
- TSQL
helpviewer_keywords:
- TABLES view
- INFORMATION_SCHEMA.TABLES view
ms.assetid: 723a9e63-8f6e-4d6e-b570-468cfaf03201
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 094ce38d1612baf3c26463e6c9f92eb223c48474
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185493"
---
# <a name="tables-transact-sql"></a>TABLES (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila por cada tabla o vista de la base de datos actual para la que el usuario actual tiene permisos.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Calificador de tabla.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nombre de esquema que contiene la tabla.<br /><br /> Importante solo la forma confiable de encontrar el esquema de un objeto consiste en consultar la vista de catálogo sys. Objects. <strong> \* \* \* \* </strong> Las vistas INFORMATION_SCHEMA podrían estar incompletas, ya que no se actualizan para todas las características nuevas.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla o vista.|  
|**TABLE_TYPE**|**VARCHAR (** 10 **)**|Tipo de tabla. Puede ser VIEW o BASE TABLE.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)  
  
