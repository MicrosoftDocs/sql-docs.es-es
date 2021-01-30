---
title: sys.pdw_permanent_table_mappings (Transact-SQL)
ms.custom: ''
ms.date: 07/24/2020
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
author: mstehrani
ms.author: emtehran
monikerRange: = azure-sqldw-latest
ms.openlocfilehash: 1258a6131ccd0ad2280066faef6ac966bb0fb4d5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212058"
---
# <a name="syspdw_permanent_table_mappings-transact-sql"></a>sys.pdw_permanent_table_mappings (Transact-SQL)
[!INCLUDE [applies-to-version/asa](../../includes/applies-to-version/asa.md)]

  Vincula las tablas de usuario permanentes a los nombres de objeto interno **object_id**.  
  
> [!NOTE]
> **Sys.pdw_permanent_table_mappings** contiene las asignaciones a las tablas permanentes y no incluye las asignaciones de tablas temporales o externas.

|Nombre de columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|physical_name|**nvarchar (36)**|Nombre físico de la tabla.<br /><br /> **physical_name** y **object_id** forman la clave de esta vista.||  
|object_id|**int**|IDENTIFICADOR de objeto de la tabla. Vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).<br /><br /> **physical_name** y **object_id** forman la clave de esta vista.||  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.pdw_index_mappings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql.md)  
  
  
