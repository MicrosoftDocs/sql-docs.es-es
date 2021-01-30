---
description: sys.registered_search_property_lists (Transact-SQL)
title: sys.registered_search_property_lists (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- registered_search_property_lists_TSQL
- sys.registered_search_property_lists
- registered_search_property_lists
- sys.registered_search_property_lists_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- full-text search [SQL Server], search property lists
- sys.registered_search_property_lists catalog view
- search property lists [SQL Server], viewing
ms.assetid: 630d4caa-9bea-4cd3-a5b1-01098b0855fc
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
ms.openlocfilehash: 6504ed677747548d940de7858ceade9a6a0ac75a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206862"
---
# <a name="sysregistered_search_property_lists-transact-sql"></a>sys.registered_search_property_lists (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila para cada lista de propiedades de búsqueda de la base de datos actual.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**property_list_id**|**int**|Identificador de la lista de propiedades.|  
|**name**|**sysname**|Nombre de la lista de propiedades.|  
|**create_date**|**datetime**|Feche en que se creó la lista de propiedades.|  
|**modify_date**|**datetime**|Fecha en que se modificó la lista propiedades por última vez mediante una instrucción ALTER.|  
|**principal_id**|**int**|Propietario de la lista de propiedades.|  
  
## <a name="remarks"></a>Observaciones  
 Para obtener más información, vea [Buscar propiedades de documento con listas de propiedades de búsqueda](../../relational-databases/search/search-document-properties-with-search-property-lists.md).  
  
## <a name="permissions"></a>Permisos  
 La visibilidad de los metadatos en las listas de propiedades se limita a aquellas de las que se es propietario o en las que se tenga concedido algún permiso REFERENCE.  
  
> [!NOTE]  
>  El propietario de lista de propiedades de búsqueda puede conceder permisos REFERENCE o CONTROL en la lista. Los usuarios con permiso CONTROL también pueden conceder el permiso REFERENCE a otros usuarios.  
  
## <a name="examples"></a>Ejemplos  
 En el siguiente ejemplo se muestran el identificador y nombre de las listas de propiedades de búsquedas en la base de datos [!INCLUDE[ssSampleDBnormal](../../includes/sssampledbnormal-md.md)].  
  
```  
USE AdventureWorks2012;  
GO  
SELECT property_list_id, name FROM sys.registered_search_property_lists;  
GO  
```  
  
## <a name="see-also"></a>Consulte también  
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)   
 [sys.fulltext_indexes &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-indexes-transact-sql.md)  
  
  
