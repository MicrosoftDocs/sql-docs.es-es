---
description: sys.pdw_nodes_tables (Transact-SQL)
title: sys.pdw_nodes_tables (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 473b5d14-171b-4a16-9195-acf36d3f786c
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2dd5f56a6b394d25ad8d1f2aa7e661a2bf21155e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207807"
---
# <a name="syspdw_nodes_tables-transact-sql"></a>sys.pdw_nodes_tables (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Contiene una fila por cada objeto de tabla al que pertenece una entidad de seguridad o en la que se ha concedido algún permiso a la entidad de seguridad.  
  
|Nombre de columna|Tipo de datos|Descripción|Intervalo|  
|-----------------|---------------|-----------------|-----------|  
|\<inherited columns>||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. Objects](../system-catalog-views/sys-objects-transact-sql.md).||  
|lob_data_space_id|**int**||Siempre es 0.|  
|filestream_data_space_id|**int**|IDENTIFICADOR de espacio de datos para un grupo de archivos FILESTREAM o [!INCLUDE[ssInfoNA](../../includes/ssinfona-md.md)]|NULL|  
|max_column_id_used|**int**|IDENTIFICADOR de columna máximo usado por esta tabla.||  
|lock_on_bulk_load|**bit**|La tabla está bloqueada en una carga masiva.|TBD|  
|uses_ansi_nulls|**bit**|La tabla se creó con la opción de base de datos SET ANSI_NULLS establecida en ON.|1|  
|is_replicated|**bit**|1 = la tabla se publica mediante la replicación.|0,1 no se admite la replicación.|  
|has_replication_filter|**bit**|1 = La tabla tiene un filtro de replicación.|0|  
|is_merge_published|**bit**|1 = La tabla se publicó con la replicación de mezcla.|0,1 no compatible.|  
|is_sync_tran_subscribed|**bit**|1 = La tabla se suscribió con una suscripción de actualización inmediata.|0,1 no compatible.|  
|has_unchecked_assembly_data|**bit**|1 = La tabla contiene datos persistentes que dependen de un ensamblado cuya definición cambió durante el último ALTER ASSEMBLY. Se restablecerá en 0 tras la siguiente operación DBCC CHECKDB o DBCC CHECKTABLE correcta.|0,1 no es compatible con CLR.|  
|text_in_row_limit|**int**|0 = la opción text in row no está establecida.|Siempre es 0.|  
|large_value_types_out_of_row|**bit**|1 = Los tipos de valores grandes se guardan fuera de la fila.|Siempre es 0.|  
|is_tracked_by_cdc|**bit**|1 = la tabla está habilitada para la captura de datos modificados|Siempre es 0; no se admite CDC.|  
|lock_escalation|**tinyint**|El valor de la opción LOCK_ESCALATION para la tabla: 2 = AUTO|Siempre es 2.|  
|lock_escalation_desc|**nvarchar(60)**|Una descripción de texto de la opción lock_escalation.|"Siempre" AUTO.|  
|pdw_node_id|**int**|Identificador único de un [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] nodo.|NOT NULL|  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de Azure Synapse Analytics y Almacenamiento de datos paralelos](../../relational-databases/system-catalog-views/sql-data-warehouse-and-parallel-data-warehouse-catalog-views.md)  
  
  
