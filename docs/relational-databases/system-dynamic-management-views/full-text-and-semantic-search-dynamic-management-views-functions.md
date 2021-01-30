---
description: 'Vistas de administración dinámica de Full-Text y semántica: funciones'
title: 'Vistas de administración dinámica de Full-Text y semántica: funciones | Microsoft Docs'
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- dynamic management objects [SQL Server], full-text search
- full-text search [SQL Server], dynamic management views
ms.assetid: 199dbd5a-29f6-4ef0-8e65-86e32c0aaa3a
author: pmasl
ms.author: pelopes
ms.openlocfilehash: cd9e072fa2c4aafceaad2d4aad57eb9dcc4bb38b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201355"
---
# <a name="full-text-and-semantic-search-dynamic-management-views---functions"></a>Vistas de administración dinámica de Full-Text y semántica: funciones
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En esta sección se incluyen las siguientes funciones y vistas de administración dinámica que están relacionadas con la búsqueda de texto completo y la búsqueda semántica.  
  
## <a name="full-text-search-dynamic-management-views-and-functions"></a>Funciones y vistas de administración dinámica relacionadas con la búsqueda de texto completo  
 [sys.dm_fts_active_catalogs &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-active-catalogs-transact-sql.md)  
 Devuelve información de catálogos de texto completo que tienen alguna actividad de rellenado en progreso en el servidor.  
  
 [sys.dm_fts_fdhosts](../../relational-databases/system-dynamic-management-views/sys-dm-fts-fdhosts-transact-sql.md)  
 Devuelve información sobre la actividad actual de los host de demonio de filtro en la instancia de servidor.  
  
 [sys.dm_fts_index_keywords](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-transact-sql.md)  
 Devuelve información sobre el contenido de un índice de texto completo para la tabla especificada.  
  
 [sys.dm_fts_index_keywords_by_document](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-document-transact-sql.md)  
 Devuelve información sobre el contenido de nivel de documento de un índice de texto completo para la tabla especificada. Una palabra clave determinada puede aparecer en varios documentos.  
  
 [sys.dm_fts_index_keywords_by_property](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-keywords-by-property-transact-sql.md)  
 Devuelve todo el contenido relacionado con propiedades en el índice de texto completo de una tabla determinada. Esto incluye todos los datos que pertenecen a cualquier propiedad registrados por la lista de propiedades de búsqueda asociada a ese índice de texto completo.  
  
 sys.dm_fts_index_keywords_position_by_document  
 Devuelve la posición de las palabras clave de un documento.  
  
 [sys.dm_fts_index_population](../../relational-databases/system-dynamic-management-views/sys-dm-fts-index-population-transact-sql.md)  
 Devuelve información acerca de los rellenados de índices de texto completo actualmente en progreso.  
  
 [sys.dm_fts_memory_buffers](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-buffers-transact-sql.md)  
 Devuelve información acerca de los búferes de memoria que pertenecen a un bloque de memoria específico que se utiliza como parte de un rastreo de texto completo o un intervalo de rastreo de texto completo.  
  
 [sys.dm_fts_memory_pools](../../relational-databases/system-dynamic-management-views/sys-dm-fts-memory-pools-transact-sql.md)  
 Devuelve información acerca de los bloques de memoria compartida disponibles para el recopilador de texto completo en un rastreo de texto completo o un intervalo de rastreo de texto completo.  
  
 [sys.dm_fts_outstanding_batches](../../relational-databases/system-dynamic-management-views/sys-dm-fts-outstanding-batches-transact-sql.md)  
 Devuelve información acerca de cada lote de indización de texto completo.  
  
 [sys.dm_fts_parser](../../relational-databases/system-dynamic-management-views/sys-dm-fts-parser-transact-sql.md)  
 Devuelve el resultado de la tokenización final después de aplicar una combinación de separador de palabras, diccionario de sinónimos y listas de palabras irrelevantes determinados a la entrada de una cadena de consulta. El resultado es equivalente al producido si la cadena de consulta determinada especificada se emitió en el motor de búsqueda de texto completo.  
  
 [sys.dm_fts_population_ranges](../../relational-databases/system-dynamic-management-views/sys-dm-fts-population-ranges-transact-sql.md)  
 Devuelve información acerca de los intervalos específicos relacionados con un rellenado de índices de texto completo actualmente en progreso.  
  
## <a name="semantic-search-dynamic-management-views-and-functions"></a>Funciones y vistas de administración dinámica de búsqueda semántica  
 [sys.dm_fts_semantic_similarity_population &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-fts-semantic-similarity-population-transact-sql.md)  
 Devuelve una fila de información de estado sobre el rellenado del índice de similitud de documentos para cada índice de similitud en cada tabla que tiene un índice semántico asociado.  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)  
  
