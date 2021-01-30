---
description: TABLE_CONSTRAINTS (Transact-SQL)
title: TABLE_CONSTRAINTS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- TABLE_CONSTRAINTS
- TABLE_CONSTRAINTS_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- INFORMATION_SCHEMA.TABLE_CONSTRAINTS view
- TABLE_CONSTRAINTS view
ms.assetid: 687f3284-2849-4853-8a5c-fc936deceae0
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eddfd8e6aa593cde7cfb384d5e0b287095bb3127
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185461"
---
# <a name="table_constraints-transact-sql"></a>TABLE_CONSTRAINTS (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada restricción de tabla de la base de datos actual. Esta vista de esquema de información devuelve información acerca de los objetos para los que el usuario actual tiene permisos.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**CONSTRAINT_CATALOG**|**nvarchar (** 128 **)**|Calificador de la restricción.|  
|**CONSTRAINT_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene la restricción.<br /><br /> Importante solo la forma confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects. <strong> \* \* \* \* </strong>|  
|**CONSTRAINT_NAME**|**sysname**|Nombre de la restricción.|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Calificador de tabla.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nombre de esquema que contiene la tabla.<br /><br /> Importante solo la forma confiable de encontrar el esquema de un objeto es consultar la vista de catálogo sys. Objects. <strong> \* \* \* \* </strong>|  
|**TABLE_NAME**|**sysname**|Nombre de la tabla.|  
|**CONSTRAINT_TYPE**|**VARCHAR (** 11 **)**|Tipo de restricción:<br /><br /> CHECK<br /><br /> UNIQUE<br /><br /> PRIMARY KEY<br /><br /> FOREIGN KEY|  
|**IS_DEFERRABLE**|**VARCHAR (** 2 **)**|Especifica si la comprobación de la restricción se puede aplazar. Siempre devuelve NO.|  
|**INITIALLY_DEFERRED**|**VARCHAR (** 2 **)**|Especifica si la comprobación de la restricción se aplaza inicialmente. Siempre devuelve NO.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md)   
 [sys.key_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-key-constraints-transact-sql.md)   
 [sys.check_constraints &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-check-constraints-transact-sql.md)   
 [sys.tables &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-tables-transact-sql.md)  
  
