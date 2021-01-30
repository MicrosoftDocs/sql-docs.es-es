---
description: sys.fulltext_system_stopwords (Transact-SQL)
title: sys.fulltext_system_stopwords (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_system_stopwords_TSQL
- fulltext_system_stopwords
- fulltext_system_stopwords_TSQL
- sys.fulltext_system_stopwords
dev_langs:
- TSQL
helpviewer_keywords:
- stoplists [full-text search]
- sys.fulltext_system_stopwords catalog view
- full-text search [SQL Server], stopwords
- stopwords [full-text search]
ms.assetid: 487de53f-c637-4d78-85f6-fef5e768cd0c
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 28bbeec3e484cd20dbdb24b165b821e07f1952e0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193848"
---
# <a name="sysfulltext_system_stopwords-transact-sql"></a>sys.fulltext_system_stopwords (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Proporciona acceso a la lista de palabras irrelevantes del sistema.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**palabra irrelevante**|**nvarchar (64)**|Término que se considera como coincidencia de palabra irrelevante.|  
|**language_id**|**int**|Identificador de configuración regional (LCID) del idioma. Este LCID se utiliza para la separación de palabras.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [sys.fulltext_stoplists &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stoplists-transact-sql.md)   
 [sys.fulltext_stopwords &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-stopwords-transact-sql.md)   
 [Configurar y administrar palabras irrelevantes y listas de palabras irrelevantes para la búsqueda de texto completo](../../relational-databases/search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md)  
  
  
