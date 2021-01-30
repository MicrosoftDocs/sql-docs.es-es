---
description: sysarticlecolumns (vista del sistema) (Transact-SQL)
title: sysarticlecolumns (vista del sistema) (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysarticlecolumns
- sysarticlecolumns_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysarticlecolumns view
ms.assetid: a8dd8d13-c827-45c4-87ba-802725301382
author: stevestein
ms.author: sstein
ms.openlocfilehash: 1de338795532eb470db25a4510f75dc3d6bf4239
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160347"
---
# <a name="sysarticlecolumns-system-view-transact-sql"></a>sysarticlecolumns (vista del sistema) (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysarticlecolumns** expone información adicional sobre las columnas de los artículos publicados. Esta vista se almacena en la base de datos de distribución.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**artid**|**int**|Identifica un artículo.|  
|**colid**|**int**|Identifica una columna de un artículo.|  
|**is_udt**|**int**|Indica si la columna es una columna de un tipo de datos definido por el usuario (UDT). Un valor de **1** indica una columna UDT.|  
|**is_xml**|**int**|Es si la columna es una columna **XML** . Un valor de **1** indica una columna **XML** .|  
|**is_max**|**int**|Es si la columna es una columna de tipo de datos de valor grande (**VARCHAR (Max)**, **nvarchar (Max)** o **varbinary (Max)**). Un valor de **1** indica una columna de valores grandes.|  
  
## <a name="see-also"></a>Consulte también  
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sysarticlecolumns &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticlecolumns-transact-sql.md)  
  
  
