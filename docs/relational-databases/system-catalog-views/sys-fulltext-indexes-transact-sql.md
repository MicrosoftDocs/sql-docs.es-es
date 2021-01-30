---
description: sys.fulltext_indexes (Transact-SQL)
title: sys.fulltext_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- fulltext_indexes
- fulltext_indexes_TSQL
- sys.fulltext_indexes_TSQL
- sys.fulltext_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_indexes catalog view
- full-text indexes [SQL Server], properties
ms.assetid: 7fc10fdc-370f-4927-bba0-b76108a7508e
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 46b65b9a9662df54a9945adfc78ca4f38d9343d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193881"
---
# <a name="sysfulltext_indexes-transact-sql"></a>sys.fulltext_indexes (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una fila por índice de texto completo de un objeto tabular.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_id**|**int**|Identificador del objeto al que pertenece este índice de texto completo.|  
|**unique_index_id**|**int**|Identificador del índice único que no es de texto completo correspondiente que se utiliza para relacionar el índice de texto completo con las filas.|  
|**fulltext_catalog_id**|**int**|Identificador del catálogo de texto completo en el que reside el índice de texto completo.|  
|**is_enabled**|**bit**|1 = El índice de texto completo está habilitado actualmente.|  
|**change_tracking_state**|**char(1)**|Estado del seguimiento de cambios.<br /><br /> M = Manual<br /><br /> A = Automático<br /><br /> O = Desactivado|  
|**change_tracking_state_desc**|**nvarchar(60)**|Descripción del estado del seguimiento de cambios.<br /><br /> MANUAL<br /><br /> AUTO<br /><br /> Apagado|  
|**has_crawl_completed**|**bit**|Último rastreo (rellenado) completado por el índice de texto completo.|  
|**crawl_type**|**char(1)**|Tipo de rastreo último o actual.<br /><br /> F = Rastreo completo<br /><br /> I = Rastreo basado en la marca de tiempo incremental<br /><br /> U = Actualización de rastreo, basado en notificaciones<br /><br /> P = Rastreo completo detenido|  
|**crawl_type_desc**|**nvarchar(60)**|Descripción del tipo de rastreo último o actual.<br /><br /> FULL_CRAWL<br /><br /> INCREMENTAL_CRAWL<br /><br /> UPDATE_CRAWL<br /><br /> PAUSED_FULL_CRAWL|  
|**crawl_start_date**|**datetime**|Inicio del rastreo último o actual.<br /><br /> NULL = Ninguno|  
|**crawl_end_date**|**datetime**|Fin del rastreo último o actual.<br /><br /> NULL = Ninguno|  
|**incremental_timestamp**|**Binary(8**|Valor de marca de tiempo que deberá utilizarse para el siguiente rastreo incremental.<br /><br /> NULL = Ninguno|  
|**stoplist_id**|**int**|IDENTIFICADOR de la lista de [palabras irrelevantes](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) asociada a este índice de texto completo.|  
|**data_space_id**|**int**|Grupo de archivos donde reside este índice de texto completo.|  
|**property_list_id**|**int**|Identificador de la lista de propiedades de búsqueda asociada a este índice de texto completo. NULL indica que no hay ninguna lista de propiedades de búsqueda asociada al índice de texto completo. Para obtener más información acerca de esta lista de propiedades de búsqueda, utilice la sys.registered_search_property_lists &#40;vista de catálogo [de Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md) .|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="examples"></a>Ejemplos  
 El ejemplo siguiente usa un índice de texto completo en la tabla `HumanResources.JobCandidate` de la base de datos de ejemplo `AdventureWorks2012`. En el ejemplo se devuelve el identificador de objeto de la tabla, el identificador de la lista de propiedades de búsqueda y el identificador de la lista de palabras irrelevantes usada por el índice de texto completo.  
  
> [!NOTE]  
>  Para obtener el ejemplo de código que crea este índice de texto completo, vea la sección "ejemplos" de [CREATE FULLTEXT index &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md).  
  
```  
USE AdventureWorks2012;  
GO  
SELECT object_id, property_list_id, stoplist_id FROM sys.fulltext_indexes  
    where object_id = object_id('HumanResources.JobCandidate');   
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [sys.fulltext_index_fragments &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)   
 [sys.fulltext_index_columns &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [sys.fulltext_index_catalog_usages &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Crear y administrar índices de texto completo](../../relational-databases/search/create-and-manage-full-text-indexes.md)   
 [DROP FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/drop-fulltext-index-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
