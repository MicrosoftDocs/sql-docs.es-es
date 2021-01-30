---
description: sys.fulltext_semantic_language_statistics_database (Transact-SQL)
title: sys.fulltext_semantic_language_statistics_database (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database_TSQL
- fulltext_semantic_language_statistics_database
- sys.fulltext_semantic_language_statistics_database
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_semantic_language_statistics_database catalog view
ms.assetid: 32e95614-ed88-4068-8c37-1e21544717bc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: ec9f9811504ed9c58c8a37510a46a1857540584f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193872"
---
# <a name="sysfulltext_semantic_language_statistics_database-transact-sql"></a>sys.fulltext_semantic_language_statistics_database (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila sobre la base de datos de estadísticas semánticas de lenguaje instalada en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Puede consultar esta vista para encontrar el componente de estadísticas semánticas de lenguaje necesario para el procesamiento semántico.  
   
  
||||  
|-|-|-|  
|**Nombre de la columna**|**Type**|**Descripción**|  
|**database_id**|**int**|Id. de la base de datos, único en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].|  
|**register_date**|**datetime**|Fecha en que la base de datos se registró para el procesamiento semántico.|  
|**registered_by**|**int**|Identificador de la entidad de seguridad de servidor que registró la base de datos para el procesamiento semántico.|  
|**version**|**nvarchar(128)**|La última información de versión específica de la base de datos de estadísticas semánticas de lenguaje.|  
  
## <a name="general-remarks"></a>Notas generales  
 Para obtener más información, vea [Instalar y configurar la búsqueda semántica](../../relational-databases/search/install-and-configure-semantic-search.md).  
  
## <a name="metadata"></a>Metadatos  
 Para obtener información acerca de los idiomas que se admiten para la indización semántica, consulte la vista de catálogo [sys.fulltext_semantic_languages &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-fulltext-semantic-languages-transact-sql.md).  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 La visibilidad de los metadatos en las vistas de catálogo se limita a los elementos protegibles y que son propiedad de un usuario o sobre los que el usuario tiene algún permiso.  
  
## <a name="examples"></a>Ejemplos  
 En el ejemplo siguiente se muestra cómo consultar **Sys.fulltext_semantic_language_statistics_database** para obtener información sobre la base de datos de estadísticas semánticas de lenguaje registrada en la instancia actual de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
```  
SELECT * FROM sys.fulltext_semantic_language_statistics_database;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [Instalar y configurar la búsqueda semántica](../../relational-databases/search/install-and-configure-semantic-search.md)  
  
  
