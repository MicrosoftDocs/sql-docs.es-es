---
description: VIEWS (Transact-SQL)
title: VISTAS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- VIEWS_TSQL
- VIEWS
dev_langs:
- TSQL
helpviewer_keywords:
- VIEWS view
- INFORMATION_SCHEMA.VIEWS view
ms.assetid: 6119bc94-0b22-45d4-a34b-967afd810a9d
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 618a05cb67728e381a0f1390faa9a98487e6c52f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190086"
---
# <a name="views-transact-sql"></a>VIEWS (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve una fila para las vistas a las que puede tener acceso el usuario actual en la base de datos actual.  
  
 Para recuperar información de estas vistas, especifique el nombre completo de **INFORMATION_SCHEMA.** _view_name_.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**TABLE_CATALOG**|**nvarchar (** 128 **)**|Calificador de la vista.|  
|**TABLE_SCHEMA**|**nvarchar (** 128 **)**|Nombre del esquema que contiene la vista.<br /><br /> **&#42;&#42; importante &#42;&#42;**  única manera confiable de encontrar el esquema de un objeto consiste en consultar la vista de catálogo sys. Objects.|  
|**TABLE_NAME**|**nvarchar (** 128 **)**|Nombre de la vista.|  
|**VIEW_DEFINITION**|**nvarchar (** 4000 **)**|Si la longitud de la definición es mayor que **nvarchar (** 4000 **)**, esta columna es NULL. De lo contrario, esta columna es el texto de la definición de la vista.|  
|**CHECK_OPTION**|**VARCHAR (** 7 **)**|Tipo de WITH CHECK OPTION. Es CASCADE si la vista original se creó con WITH CHECK OPTION. En caso contrario se devuelve NONE.|  
|**IS_UPDATABLE**|**VARCHAR (** 2 **)**|Especifica si la vista se puede actualizar. Siempre devuelve NO.|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de esquema de información &#40;Transact-SQL&#41;](~/relational-databases/system-information-schema-views/system-information-schema-views-transact-sql.md)   
 [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md)   
 [sys.views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-views-transact-sql.md)  
  
