---
description: sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
title: sys.dm_fts_index_keywords_position_by_document (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document_TSQL
- dm_fts_index_keywords_position_by_document
- sys.dm_fts_index_keywords_position_by_document
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_fts_index_keywords_position_by_document dynamic management view
ms.assetid: 0d70184f-baa2-411b-a32d-a4c5af890edd
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 099574becb70e3812864c4acc7dc0a395f3169b9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211225"
---
# <a name="sysdm_fts_index_keywords_position_by_document-transact-sql"></a>sys.dm_fts_index_keywords_position_by_document (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la información de posición de la palabra clave en los documentos indexados.  
  
## <a name="syntax"></a>Sintaxis  
  
```  
sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Argumentos  
 db_id ('*database_name*')  
 Una llamada a la función [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Esta función acepta un nombre de base de datos y devuelve el identificador de base de datos, que sys.dm_fts_index_keywords_position_by_document usa para encontrar la base de datos especificada.  
  
 object_id ('*TABLE_NAME*')  
 Una llamada a la función [OBJECT_ID ()](../../t-sql/functions/object-id-transact-sql.md) . Esta función acepta un nombre de tabla y devuelve el identificador de la tabla que contiene el índice de texto completo que se va a inspeccionar.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|palabra clave|**varbinary(128)**|Cadena binaria que representa la palabra clave.|  
|display_term|**nvarchar(4000)**|Formato legible de la palabra clave. Este formato se deriva del formato interno que se almacena en el índice de texto completo.|  
|column_id|**int**|Identificador de la columna en que la palabra clave actual forma parte del índice de texto completo.|  
|document_id|**bigint**|Identificador del documento o fila en que el término actual se indizó con texto completo. Este identificador corresponde al valor de clave de texto completo de ese documento o fila.|  
|position|**int**|Posición de la palabra clave en el documento.|  
  
## <a name="remarks"></a>Observaciones  
 Use la DMV para identificar la ubicación de las palabras indizadas en los documentos indexados. Esta DMV se puede usar para solucionar problemas cuando **Sys.dm_fts_index_keywords_by_document** indica que las palabras están en el índice de texto completo, pero cuando se ejecuta una consulta con esas palabras, no se devuelve el documento.  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso SELECT en las columnas cubiertas por el índice de texto completo y permisos CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelven palabras clave del índice de texto completo de la `Production.Document` tabla de la `AdventureWorks` base de datos de ejemplo.  
  
```  
USE AdventureWorks2012;  
GO   
  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
);   
GO  
```  
  
 Puede Agregar un predicado en el otro columns_id como en la siguiente consulta de ejemplo, para aislar aún más las ubicaciones.  
  
```  
SELECT * FROM sys.dm_fts_index_keywords_position_by_document  
(   
    DB_ID('AdventureWorks2012'),  
    OBJECT_ID('AdventureWorks2012.Production.Document')   
)  
WHERE document_id = 7 AND display_term = 'performance';  
```  
  
## <a name="see-also"></a>Consulte también  
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)   
 [Mejorar el rendimiento de los índices de Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [Funciones de búsqueda de texto completo y búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-functions/full-text-search-and-semantic-search-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/full-text-search-and-semantic-search-stored-procedures-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)   
 [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
  
