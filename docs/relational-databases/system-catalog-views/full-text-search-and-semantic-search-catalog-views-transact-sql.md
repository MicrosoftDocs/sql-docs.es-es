---
description: Vistas de catálogo relacionadas con la búsqueda de texto completo y la búsqueda semántica (Transact-SQL)
title: Full-Text vistas de catálogo de búsqueda semántica (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- catalog views [SQL Server], full-text search
- full-text search [SQL Server], catalog views
- full-text indexes [SQL Server], catalog views
ms.assetid: b08ad2fd-e3d8-458f-96f1-678217e0f419
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 90a7bcde4caa40158bf55314c84054bb94a8426f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194619"
---
# <a name="full-text-search-and-semantic-search-catalog-views-transact-sql"></a>Vistas de catálogo relacionadas con la búsqueda de texto completo y la búsqueda semántica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  En esta sección se describen las vistas de catálogo que proporcionan información sobre índices de texto completo e índices semánticos.  
  
## <a name="full-text-search-catalog-views"></a>Vistas de catálogo de búsqueda de texto completo  
 [sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md)  
 Contiene una fila para cada catálogo de texto completo.  
  
 [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md)  
 Devuelve una fila por cada tipo de documento disponible para operaciones de indización de texto completo. Cada fila representa la interfaz **IFilter** registrada en la instancia de SQL Server.  
  
 [sys.fulltext_index_catalog_usages](../../relational-databases/system-catalog-views/sys-fulltext-index-catalog-usages-transact-sql.md)  
 Devuelve una fila por cada referencia de catálogo de texto completo a índice de texto completo.  
  
 [sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
 Contiene una fila para cada columna que forma parte de un índice de texto completo.  
  
 [sys.fulltext_index_fragments](../../relational-databases/system-catalog-views/sys-fulltext-index-fragments-transact-sql.md)  
 Contiene una fila para cada fragmento de índice de texto completo de cada tabla que contenga uno.  
  
 [sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
 Contiene una fila por índice de texto completo de un objeto tabular.  
  
 [sys.fulltext_languages](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)  
 Contiene una fila por idioma cuyos separadores de palabras estén registrados en SQL Server. Cada fila muestra el LCID y el nombre del idioma.  
  
 [sys.fulltext_stoplists](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)  
 Contiene una fila por cada lista de palabras irrelevantes de texto completo de la base de datos.  
  
 [sys.fulltext_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)  
 Contiene una fila por cada palabra irrelevante de todas las listas de palabras irrelevantes de la base de datos.  
  
 [sys.fulltext_system_stopwords](../../relational-databases/system-catalog-views/sys-fulltext-system-stopwords-transact-sql.md)  
 Proporciona acceso a la lista de palabras irrelevantes del sistema.  
  
 [sys.registered_search_properties &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-properties-transact-sql.md)  
 Contiene una fila para cada propiedad de búsqueda que cualquier lista de propiedades de búsqueda contiene en la base de datos actual.  
  
 [sys.registered_search_property_lists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-registered-search-property-lists-transact-sql.md)  
 Contiene una fila para cada lista de propiedades de búsqueda de la base de datos actual.  
  
## <a name="semantic-search-catalog-views"></a>Vistas de catálogo de búsqueda semántica  
 [sys.fulltext_semantic_language_statistics_database &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-language-statistics-database-transact-sql.md)  
 Devuelve una fila sobre la base de datos de estadísticas semánticas de lenguaje instalada en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [sys.fulltext_semantic_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md)  
 Devuelve una fila por cada idioma cuyo modelo de estadísticas se registra con la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cuando se registra un modelo de idioma, el idioma se habilita para la indización semántica.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas del sistema &#40;Transact-SQL&#41;](../../t-sql/language-reference.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)  
  
