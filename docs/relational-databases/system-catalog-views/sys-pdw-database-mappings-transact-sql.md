---
description: sys.pdw_database_mappings (Transact-SQL)
title: sys.pdw_database_mappings (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/17/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 4ae2c71e-dd56-41ea-a16b-64936175b459
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016'
ms.openlocfilehash: 52881d6590abd115565e2d35bc109a99460eb72d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180806"
---
# <a name="syspdw_database_mappings-transact-sql"></a>sys.pdw_database_mappings (Transact-SQL)
[!INCLUDE [pdw](../../includes/applies-to-version/pdw.md)]

  Asigna los **database_id** s de las bases de datos al nombre físico que se usa en los nodos de proceso y proporciona el ID. de la **entidad** de seguridad del propietario de la base de datos en el sistema. Únase **Sys.pdw_database_mappings** a **Sys. databases** y **Sys.pdw_nodes_pdw_physical_databases**.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|physical_name|**nvarchar (36)**|Nombre físico de la base de datos en los nodos de proceso.<br /><br /> **physical_name** y **database_id** forman la clave de esta vista.||  
|database_id|**int**|IDENTIFICADOR de objeto de la base de datos. Vea [Sys. databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).<br /><br /> **physical_name** y **database_id** forman la clave de esta vista.||  
  
## <a name="examples-sspdw"></a>Ejemplos: [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 En el ejemplo siguiente se combina sys.pdw_database_mappings con otras tablas del sistema para mostrar cómo se asignan las bases de datos.  
  
```  
SELECT DB.database_id, DB.name, Map.*, Phys.*   
FROM sys.databases AS DB  
JOIN sys.pdw_database_mappings AS Map  
    ON DB.database_id = Map.database_id  
JOIN sys.pdw_nodes_pdw_physical_databases AS Phys  
    ON Map.physical_name = Phys.physical_name  
ORDER BY DB.database_id, Phys.pdw_node_id;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)   
 [sys.pdw_index_mappings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-index-mappings-transact-sql.md)   
 [sys.pdw_table_mappings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-table-mappings-transact-sql.md)   
 [sys.pdw_nodes_pdw_physical_databases &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-pdw-nodes-pdw-physical-databases-transact-sql.md)  
  
  

