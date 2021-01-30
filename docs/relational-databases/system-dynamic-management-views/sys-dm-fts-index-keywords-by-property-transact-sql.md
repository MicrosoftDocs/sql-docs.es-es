---
description: sys.dm_fts_index_keywords_by_property (Transact-SQL)
title: sys.dm_fts_index_keywords_by_property (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_fts_index_keywords_by_property
- dm_fts_index_keywords_by_property_TSQL
- sys.dm_fts_index_keywords_by_property
- sys.dm_fts_index_keywords_by_property_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], troubleshooting
- search property lists [SQL Server], viewing keywords by property
- full-text search [SQL Server], viewing keywords
- sys.dm_fts_index_keywords_by_property dynamic management view
ms.assetid: fa41e052-a79a-4194-9b1a-2885f7828500
author: pmasl
ms.author: pelopes
ms.openlocfilehash: 9052c7d64ab78c2e6eca1388ed08903365b6e129
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196255"
---
# <a name="sysdm_fts_index_keywords_by_property-transact-sql"></a>sys.dm_fts_index_keywords_by_property (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve todo el contenido relacionado con propiedades en el índice de texto completo de una tabla determinada. Esto incluye todos los datos que pertenecen a cualquier propiedad registrados por la lista de propiedades de búsqueda asociada a ese índice de texto completo.  
  
 sys.dm_fts_index_keywords_by_property es una función de administración dinámica que permite ver qué propiedades registradas ha emitido IFilters en el momento del índice, así como el contenido exacto de cada una de las propiedades de cada documento indizado.  
  
 **Para ver todo el contenido en el nivel de documento (incluido el contenido relacionado con las propiedades)**  
  
-   [sys.dm_fts_index_keywords_by_document &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
  
 **Para ver información de nivel superior sobre un índice de texto completo**  
  
-   [sys.dm_fts_index_keywords &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
  
> [!NOTE]  
>  Para obtener información sobre las listas de propiedades de búsqueda, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sys.dm_fts_index_keywords_by_property  
(   
    DB_ID('database_name'),   
OBJECT_ID('table_name')   
)  
```  
  
## <a name="arguments"></a>Argumentos  
 db_id ('*database_name*')  
 Una llamada a la función [DB_ID ()](../../t-sql/functions/db-id-transact-sql.md) . Esta función acepta un nombre de base de datos y devuelve el identificador de base de datos, que sys.dm_fts_index_keywords_by_property usa para encontrar la base de datos especificada. Si el parámetro *database_name* se omite, se devuelve el identificador de base de datos actual.  
  
 object_id ('*TABLE_NAME*')  
 Una llamada a la función [OBJECT_ID ()](../../t-sql/functions/object-id-transact-sql.md) . Esta función acepta un nombre de tabla y devuelve el identificador de la tabla que contiene el índice de texto completo que se va a inspeccionar.  
  
## <a name="table-returned"></a>Tabla devuelta  
  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|palabra clave|**nvarchar(4000)**|Representación hexadecimal de la palabra clave que se almacena dentro del índice de texto completo.<br /><br /> Nota: OxFF representa el carácter especial que indica el final de un archivo o conjunto de archivos.|  
|display_term|**nvarchar(4000)**|Formato legible de la palabra clave. Este formato se deriva del formato interno que se almacena en el índice de texto completo.<br /><br /> Nota: OxFF representa el carácter especial que indica el final de un archivo o conjunto de archivos.|  
|column_id|**int**|Identificador de la columna en que la palabra clave actual forma parte del índice de texto completo.|  
|document_id|**int**|Identificador del documento o fila en que el término actual se indizó con texto completo. Este identificador corresponde al valor de clave de texto completo de ese documento o fila.|  
|property_id|**int**|IDENTIFICADOR de propiedad interno de la propiedad de búsqueda en el índice de texto completo de la tabla que especificó en el parámetro OBJECT_ID ('*TABLE_NAME*').<br /><br /> Cuando una propiedad determinada se agrega a una lista de propiedades de búsqueda, el servicio Motor de búsqueda de texto completo registra la propiedad y le asigna un identificador de propiedad interno que es específico de esa lista de propiedades. El identificador de propiedad interno, que es un entero, es único para una lista de propiedades de búsqueda determinada. Si una propiedad determinada se registra para varias listas de propiedades de búsqueda, se puede asignar un identificador de propiedad interno diferente para cada lista de propiedades de búsqueda.<br /><br /> Nota: el identificador de propiedad interno es distinto del identificador entero de la propiedad que se especifica al agregar la propiedad a la lista de propiedades de búsqueda. Para obtener más información, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).<br /><br /> Para ver la asociación entre property_id y el nombre de la propiedad:<br />                    [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)|  
  
## <a name="remarks"></a>Observaciones  
 Esta vista de administración dinámica puede responder preguntas como las siguientes:  
  
-   ¿Qué contenido se almacena en una determinada propiedad de un DocID específico?  
  
-   ¿Qué frecuencia tiene una determinada propiedad entre los documentos indizados?  
  
-   ¿Qué documentos contienen realmente una propiedad determinada? Todo esto resulta útil si al consultar una determinada propiedad de búsqueda, no se devuelve ninguno de los documentos que esperaba localizar.  
  
 Cuando la columna de clave de texto completo es un tipo de datos entero, como se recomienda, document_id se asigna directamente al valor de clave de texto completo de la tabla base.  
  
 Por el contrario, cuando la columna de clave de texto completo utiliza un tipo de datos que no es entero, document_id no representa la clave de texto completo en la tabla base. En este caso, para identificar la fila de la tabla base devuelta por dm_fts_index_keywords_by_property, debe combinar esta vista con los resultados devueltos por [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md). Para poder combinarlos, debe almacenar la salida del procedimiento almacenado en una tabla temporal. A continuación, puede combinar el document_id columna de dm_fts_index_keywords_by_property con la columna DocId que devuelve este procedimiento almacenado. Tenga en cuenta que una columna de **marca** de tiempo no puede recibir valores en el momento de la inserción, porque los genera automáticamente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por lo tanto, la columna **timestamp** debe convertirse en columnas **varbinary (8)** . En el ejemplo siguiente se muestran estos pasos. En este ejemplo, *table_id* es el identificador de la tabla, *database_name* es el nombre de la base de datos y *TABLE_NAME* es el nombre de la tabla.  
  
```  
USE database_name;  
GO  
CREATE TABLE #MyTempTable   
   (  
      docid INT PRIMARY KEY ,  
      [key] INT NOT NULL  
   );  
DECLARE @db_id int = db_id(N'database_name');  
DECLARE @table_id int = OBJECT_ID(N'table_name');  
INSERT INTO #MyTempTable EXEC sp_fulltext_keymappings @table_id;  
SELECT * FROM sys.dm_fts_index_keywords_by_property   
   ( @db_id, @table_id ) kbd  
   INNER JOIN #MyTempTable tt ON tt.[docid]=kbd.document_id;  
GO  
  
```  
  
## <a name="permissions"></a>Permisos  
 Necesita el permiso SELECT en las columnas cubiertas por el índice de texto completo y permisos CREATE FULLTEXT CATALOG.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se devuelven las palabras clave de la propiedad `Author` en el índice de texto completo de la tabla `Production.Document` de la base de datos de ejemplo `AdventureWorks`. En el ejemplo se utiliza el alias `KWBPOP` para la tabla devuelta por **Sys.dm_fts_index_keywords_by_property**. En el ejemplo se usan combinaciones internas para combinar columnas de [Sys.registered_search_properties](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md) y [Sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md).  
  
```  
-- Once the full-text index is configured to support property searching  
-- on the Author property, return any keywords indexed for this property.  
USE AdventureWorks2012;  
GO   
SELECT KWBPOP.* FROM   
   sys.dm_fts_index_keywords_by_property( DB_ID(),   
   object_id('Production.Document') ) AS KWBPOP  
   INNER JOIN  
      sys.registered_search_properties AS RSP ON(   
         (KWBPOP.property_id = RSP.property_id)   
         AND (RSP.property_name = 'Author') )  
   INNER JOIN  
      sys.fulltext_indexes AS FTI ON(   
         (FTI.[object_id] = object_id('Production.Document'))   
         AND (RSP.property_list_id = FTI.property_list_id) );  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)   
 [Mejorar el rendimiento de los índices de Full-Text](../../relational-databases/search/improve-the-performance-of-full-text-indexes.md)   
 [sp_fulltext_keymappings &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)   
 [sys.dm_fts_index_keywords_by_document &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)   
 [sys.dm_fts_index_keywords &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)   
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)   
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)   
 [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md)  
  
  
