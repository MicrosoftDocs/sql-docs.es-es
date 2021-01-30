---
description: CONSTRAINT_TABLE_USAGE (Transact-SQL)
title: CONSTRAINT_TABLE_USAGE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- CONSTRAINT_TABLE_USAGE_TSQL
- CONSTRAINT_TABLE_USAGE
dev_langs:
- TSQL
helpviewer_keywords:
- CONSTRAINT_TABLE_USAGE view
- INFORMATION_SCHEMA.CONSTRAINT_TABLE_USAGE view
ms.assetid: 5770b696-6c04-4d5c-a8db-9eb92022fa42
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f5caf4072d3fcd3fd955ff97e1533bb4bd85b377
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179807"
---
# <a name="constraint_table_usage-transact-sql"></a>CONSTRAINT_TABLE_USAGE (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada tabla de la base de datos actual que tiene una restricción definida en ella. Esta vista de esquema de información devuelve información acerca de los objetos para los que el usuario actual tiene permisos.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Calificador de tabla.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nombre de esquema que contiene la tabla.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla.|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Calificador de la restricción.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene la restricción.<br /><br /> **&#42;&#42; importante &#42;&#42;** No use vistas INFORMATION_SCHEMA para determinar el esquema de un objeto. INFORMATION_SCHEMA vistas solo representan un subconjunto de los metadatos de un objeto. La única manera confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects.|  
|**CONSTRAINT_NAME**|**sysname**|Nombre de la restricción.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.check_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)   
 [sys.key_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.foreign_keys &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-foreign-keys-transact-sql.md)  
  
