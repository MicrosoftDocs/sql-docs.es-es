---
description: sys.xml_indexes (Transact-SQL)
title: sys.xml_indexes (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.xml_indexes_TSQL
- xml_indexes_TSQL
- sys.xml_indexes
- xml_indexes
dev_langs:
- TSQL
helpviewer_keywords:
- sys.xml_indexes catalog view
ms.assetid: 3408de72-b067-4fda-b5d5-8e856dfd9db3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 63ce32a74a3173cfd0710e9ed12cb925ababeb74
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183623"
---
# <a name="sysxml_indexes-transact-sql"></a>sys.xml_indexes (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por índice XML.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**\<inherited columns>**||Hereda columnas de [Sys. Indexes](../../relational-databases/system-catalog-views/sys-indexes-transact-sql.md).|  
|**using_xml_index_id**|**int**|NULL = Índice XML principal.<br /><br /> Nonnull = Índice XML secundario.<br /><br /> Nonnull es una referencia de autocombinación al índice XML principal.|  
|**secondary_type**|**char(1)**|Descripción del tipo del índice secundario:<br /><br /> P = Índice XML secundario de PATH<br /><br /> V = Índice XML secundario de VALUE<br /><br /> R = Índice XML secundario de PROPERTY<br /><br /> NULL = Índice XML principal|  
|**secondary_type_desc**|**nvarchar(60)**|Descripción del tipo del índice secundario:<br /><br /> PATH = Índice XML secundario de PATH<br /><br /> VALUE = Índice XML secundario de VALUE<br /><br /> PROPERTY = Índices XML secundarios de PROPERTY.<br /><br /> NULL = Índice XML principal|  
|**xml_index_type**|**tinyint**|Tipo de índice:<br /><br /> 0 = Índice XML principal<br /><br /> 1 = Índice XML secundario<br /><br /> 2 = Índice XML selectivo<br /><br /> 3 = Índice XML selectivo secundario|  
|**xml_index_type_description**|**nvarchar(60)**|Descripción del tipo de índice:<br /><br /> PRIMARY_XML<br /><br /> Índice XML secundario<br /><br /> Índice XML selectivo<br /><br /> Índice XML selectivo secundario|  
|**path_id**|**int**|NULL para todos los índices XML, salvo el índice XML selectivo secundario.<br /><br /> También es el identificador de la ruta de acceso promovida en la que se va a generar el índice XML selectivo secundario. Este valor es el mismo que el de path_id de la vista del sistema sys.selective_xml_index_paths.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)] Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md) (Vistas de catálogo de objetos [Transact-SQL])  
  
  
