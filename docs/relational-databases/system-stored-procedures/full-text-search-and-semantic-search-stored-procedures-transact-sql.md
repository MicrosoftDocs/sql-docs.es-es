---
description: Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica (Transact-SQL)
title: Procedimientos almacenados de Full-Text Search y Semantics (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- full-text indexes [SQL Server], stored procedures
- full-text search [SQL Server], stored procedures
- full-text catalogs [SQL Server], stored procedures
- system stored procedures [SQL Server], full-text search
ms.assetid: 0d185a16-2b16-4958-884f-efe675e2e551
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 39e13966219ed8f08c5a5095c505a661e6923a8b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165196"
---
# <a name="full-text-search-and-semantic-search-stored-procedures-transact-sql"></a>Búsqueda de texto completo y procedimientos almacenados de búsqueda semántica (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite los siguientes procedimientos almacenados del sistema que se usan para implementar y consultar índices de texto completo e índices semánticos.  
  
## <a name="full-text-search-stored-procedures"></a>Procedimientos almacenados de la búsqueda de texto completo  
 [sp_fulltext_catalog](../../relational-databases/system-stored-procedures/sp-fulltext-catalog-transact-sql.md)  
 Crea y quita un catálogo de texto completo, e inicia y detiene la acción de indización de un catálogo. Se pueden crear varios catálogos de texto completo en cada base de datos.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Use [CREATE FULLTEXT CATALOG](../../t-sql/statements/create-fulltext-catalog-transact-sql.md), [ALTER fulltext](../../t-sql/statements/alter-fulltext-catalog-transact-sql.md)CATALOG y [Drop FULLTEXT CATALOG](../../t-sql/statements/drop-fulltext-catalog-transact-sql.md) en su lugar.  
  
 [sp_fulltext_column](../../relational-databases/system-stored-procedures/sp-fulltext-column-transact-sql.md)  
 Especifica si una columna concreta de una tabla participa en la indización de texto completo.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] Utilice [ALTER fulltext index](../../t-sql/statements/alter-fulltext-index-transact-sql.md) en su lugar.  
  
 [sp_fulltext_database](../../relational-databases/system-stored-procedures/sp-fulltext-database-transact-sql.md)  
 No tiene ningún efecto en catálogos de texto completo en [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] y versiones posteriores, y solo se admite por razones de compatibilidad con versiones anteriores.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_fulltext_keymappings](../../relational-databases/system-stored-procedures/sp-fulltext-keymappings-transact-sql.md)  
 Devuelve las asignaciones entre los identificadores de documento (**DocId** s) y los valores de clave de texto completo.  
  
 [sp_fulltext_load_thesaurus_file](../../relational-databases/system-stored-procedures/sp-fulltext-load-thesaurus-file-transact-sql.md)  
 Analiza y carga los datos de un archivo de diccionario de sinónimos actualizado correspondiente a un LCID y vuelve a compilar las consultas de texto completo que utilizan el diccionario de sinónimos.  
  
 [sp_fulltext_pendingchanges](../../relational-databases/system-stored-procedures/sp-fulltext-pendingchanges-transact-sql.md)  
 Devuelve cambios sin procesar, como eliminaciones, actualizaciones e inserciones pendientes, de una tabla especificada que utilice el seguimiento de cambios.  
  
 [sp_fulltext_service](../../relational-databases/system-stored-procedures/sp-fulltext-service-transact-sql.md)  
 Cambia las propiedades de servidor de la búsqueda de texto completo para SQL Server.  
  
 [sp_fulltext_table](../../relational-databases/system-stored-procedures/sp-fulltext-table-transact-sql.md)  
 Marca o quita la marca de una tabla para la indización de texto completo.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Use [CREATE FULLTEXT INDEX](../../t-sql/statements/create-fulltext-index-transact-sql.md), [ALTER FULLTEXT INDEX](../../t-sql/statements/alter-fulltext-index-transact-sql.md)y [DROP FULLTEXT INDEX](../../t-sql/statements/drop-fulltext-index-transact-sql.md) en su lugar.  
  
 [sp_help_fulltext_catalog_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalog-components-transact-sql.md)  
 Devuelve una lista con todos los componentes (filtros, separadores de palabras y controladores de protocolo) que se usan en los catálogos de texto completo de la base de datos actual.  
  
 [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)]  
  
 [sp_help_fulltext_catalogs](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-transact-sql.md)  
 Devuelve el identificador, el nombre, el directorio raíz, el estado y el número de tablas indizadas de texto completo del catálogo de texto completo especificado.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
 [sp_help_fulltext_catalogs_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-catalogs-cursor-transact-sql.md)  
 Utiliza un cursor para devolver el identificador, nombre, directorio raíz, estado y número de las tablas indizadas de texto completo para el catálogo de texto completo especificado.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_catalogs](../../relational-databases/system-catalog-views/sys-fulltext-catalogs-transact-sql.md) .  
  
 [sp_help_fulltext_columns](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-transact-sql.md)  
 Devuelve las columnas diseñadas para la indización de texto completo.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) .  
  
 [sp_help_fulltext_columns_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-columns-cursor-transact-sql.md)  
 Utiliza un cursor para devolver las columnas designadas para la indización de texto completo.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_index_columns](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md) .  
  
 [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md)  
 Devuelve información de los separadores de palabras, filtros y controladores de protocolo registrados, además de una lista de identificadores de bases de datos y catálogos de texto completo que han utilizado un componente especificado.  
  
 [sp_help_fulltext_tables](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-transact-sql.md)  
 Devuelve una lista de tablas registradas para la indización de texto completo.  
  
 [sp_help_fulltext_tables_cursor](../../relational-databases/system-stored-procedures/sp-help-fulltext-tables-cursor-transact-sql.md)  
 Devuelve una lista de tablas registradas para la indización de texto completo.  
  
 [!INCLUDE[ssNoteDepFutureAvoid](../../includes/ssnotedepfutureavoid-md.md)] En su lugar, use la vista de catálogo [Sys.fulltext_indexes](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md) .  
  
## <a name="semantic-search-stored-procedures"></a>Procedimientos almacenados de búsqueda semántica  
 [sp_fulltext_semantic_register_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-register-language-statistics-db-transact-sql.md)  
 Registra una base de datos previamente rellenada de estadísticas semánticas de lenguaje en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [sp_fulltext_semantic_unregister_language_statistics_db &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-fulltext-semantic-unregister-language-statistics-db-transact-sql.md)  
 Cancela el registro de una base de datos existente de estadísticas semánticas de lenguaje de la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y elimina los metadatos asociados.  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo de búsqueda de texto completo y búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/full-text-search-and-semantic-search-catalog-views-transact-sql.md)   
 [Funciones y vistas de administración dinámica de la búsqueda de texto completo y la búsqueda semántica &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/full-text-and-semantic-search-dynamic-management-views-functions.md)   
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [Búsqueda de texto completo](../../relational-databases/search/full-text-search.md)  
  
  
