---
description: sys.edge_constraints (Transact-SQL)
title: sys.edge_constraints (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/17/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.edge_constraints
- edge_constraints
- SQL Graph
- edge_constraints_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.edge_constraints catalog view
ms.assetid: 0f782d2f-7126-46ab-85b7-bcba44862231
author: shkale-msft
ms.author: shkale
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e0c07810f26bcb125ac0fc699c8c943a52ca819b
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464963"
---
# <a name="sysedge_constraints-transact-sql"></a>sys.edge_constraints (Transact-SQL)
[!INCLUDE[sqlserver2019](../../includes/applies-to-version/sqlserver2019.md)]

Contiene una fila por cada objeto que es una restricción perimetral. 
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<Columns inherited from sys.objects>**||Para obtener una lista de las columnas que hereda esta vista, vea [Sys. objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).|  
|**is_disabled**|**bit**|1 = la restricción perimetral está dessangrada.<br /><br /> 0 = la restricción perimetral está habilitada.|  
|**is_not_trusted**|**bit**|1 = el sistema no ha comprobado la restricción perimetral.<br /><br /> 0 = el sistema ha comprobado la restricción perimetral.|  
|**delete_referential_action**|**tinyint**|Acción referencial que se definió en esta restricción perimetral.<br /><br />0 = ninguna acción.|  
|**delete_referential_action_desc**|**nvarchar(60)**|Descripción de la acción referencial que se definió en esta restricción perimetral.<br /><br />NO_ACTION|  
|**is_system_named**|**bit**|1 = el sistema generó el nombre de la restricción perimetral.<br /><br />0 = el usuario proporcionó el nombre de la restricción perimetral.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Consultar las preguntas más frecuentes (P+F) del catálogo del sistema de SQL Server](../../relational-databases/system-catalog-views/querying-the-sql-server-system-catalog-faq.yml)  
  
  
