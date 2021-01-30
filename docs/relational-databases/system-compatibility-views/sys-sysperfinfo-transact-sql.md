---
description: sys.sysperfinfo (Transact-SQL)
title: sys.sysperfinfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysperfinfo_TSQL
- sys.sysperfinfo_TSQL
- sys.sysperfinfo
- sysperfinfo
dev_langs:
- TSQL
helpviewer_keywords:
- sys.sysperfinfo compatibility view
- sysperfinfo system table
ms.assetid: e22a81cd-27de-4690-9443-6aad6393bd3c
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f19c8f9f9115ca52746812d7df9891eb6338ad82
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160682"
---
# <a name="syssysperfinfo-transact-sql"></a>sys.sysperfinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] representación de los contadores de rendimiento internos que se pueden mostrar a través del monitor de sistema de Windows.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**object_name**|**nchar(128)**|Nombre de objeto de rendimiento, como **SQLServer: LockManager** o **SQLServer: BufferManager**.|  
|**counter_name**|**nchar(128)**|Nombre del contador de rendimiento dentro del objeto, como **solicitudes de páginas** o **bloqueos solicitados**.|  
|**instance_name**|**nchar(128)**|Instancia con nombre del contador. Por ejemplo, hay contadores que se mantienen para cada tipo de bloqueo, como **tabla**, **Página**, **clave**, etc. El nombre de instancia permite distinguir entre contadores similares.|  
|**cntr_value**|**bigint**|Valor del contador. Con frecuencia, será un nivel o un contador que se incrementa continuamente y que cuenta las veces que se produce el evento de la instancia.|  
|**cntr_type**|**int**|Tipo de contador definido en la arquitectura de rendimiento de Windows.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
