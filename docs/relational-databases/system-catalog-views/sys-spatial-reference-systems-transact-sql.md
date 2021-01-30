---
description: sys.spatial_reference_systems (Transact-SQL)
title: sys.spatial_reference_systems (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- spatial_reference_systems_TSQL
- sys.spatial_reference_systems_TSQL
- sys.spatial_reference_systems
- spatial_reference_systems
dev_langs:
- TSQL
helpviewer_keywords:
- sys.spatial_reference_systems catalog view
- spatial_reference_systems
ms.assetid: 3c9bc120-67c3-463f-9e24-29fd623f25a0
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: abcffe652c6dabbed8573a00bb2b9754326587e1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99150352"
---
# <a name="sysspatial_reference_systems-transact-sql"></a>sys.spatial_reference_systems (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Enumera los sistemas de referencia espacial (SRID) que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite.  

  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|spatial_reference_id|**int**|SRID que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] admite.|  
|authority_name|**nvarchar(128)**|Autoridad del SRID.|  
|authorized_spatial_reference_id|**int**|SRID proporcionado por la autoridad llamada en **authority_name**.|  
|well_known_text|**nvarchar(4000)**|Representación WKT del SRID.|  
|unit_of_measure|**nvarchar(128)**|Nombre de la unidad de medida.|  
|unit_conversion_factor|**float**|Longitud de la unidad de medida en metros.|  
  
## <a name="permissions"></a>Permisos  
 [!INCLUDE[ssCatViewPerm](../../includes/sscatviewperm-md.md)]  
  
  
