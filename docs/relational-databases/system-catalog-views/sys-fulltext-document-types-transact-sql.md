---
description: sys.fulltext_document_types (Transact-SQL)
title: sys.fulltext_document_types (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.fulltext_document_types_TSQL
- fulltext_document_types
- fulltext_document_types_TSQL
- sys.fulltext_document_types
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fulltext_document_types catalog view
ms.assetid: 156fcfa4-7304-4a5c-b96f-1c3e061e5df0
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 209e7585bfae7009c55447416c150b6a352a1111
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193897"
---
# <a name="sysfulltext_document_types-transact-sql"></a>sys.fulltext_document_types (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Devuelve una fila por cada tipo de documento disponible para operaciones de indización de texto completo. Cada fila representa la interfaz IFilter registrada en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**document_type**|**sysname**|Extensión de archivo del tipo de documento admitido.<br /><br /> Este valor se puede utilizar para identificar el filtro que se utilizará durante la indización de texto completo de las columnas de tipo **varbinary (Max)** o **Image**.|  
|**class_id**|**uniqueidentifier**|GUID de la clase de IFilter que admite la extensión de archivo.|  
|**path**|**nvarchar(260)**|Ruta de acceso al archivo DLL de IFilter. La ruta de acceso solo es visible para los miembros del rol fijo de servidor **serveradmin** .|  
|**version**|**sysname**|Versión del archivo DLL de IFilter.|  
|**le**|**sysname**|Nombre del fabricante de IFilter.<br /><br /> Nota: solo [!INCLUDE[msCoName](../../includes/msconame-md.md)] se admiten en los documentos con el fabricante como [!INCLUDE[ssSDS](../../includes/sssds-md.md)] .|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
## <a name="see-also"></a>Consulte también  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
