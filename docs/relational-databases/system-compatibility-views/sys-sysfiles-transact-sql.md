---
description: sys.sysfiles (Transact-SQL)
title: Archivos sys.sys(Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysfiles
- sys.sysfiles_TSQL
- sys.sysfiles
- sysfiles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysfiles system table
- sys.sysfiles compatibility view
ms.assetid: 3b47f38d-1cff-404d-89d3-9342c451c802
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 34ec2c47be129384cca6be84894251a74cb996f4
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201376"
---
# <a name="syssysfiles-transact-sql"></a>sys.sysfiles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada archivo de una base de datos.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**ID**|**smallint**|Número de identificación del archivo, único para cada base de datos.|  
|**GROUPID**|**smallint**|Número de identificación del grupo de archivos.|  
|**size**|**int**|Tamaño de archivo, en páginas de 8 KB.|  
|**tamañomáximo**|**int**|Tamaño máximo de archivo, en páginas de 8 KB.<br /><br /> 0 = Sin aumento de tamaño.<br /><br /> -1 = El archivo crece hasta que el disco esté lleno.<br /><br /> 268435456 = El archivo de registro aumentará de tamaño hasta un tamaño máximo de 2 TB.<br /><br /> Nota: las bases de datos que se actualizan con un tamaño de archivo de registro ilimitado informarán de-1 para el tamaño máximo del archivo de registro.|  
|**crezca**|**int**|Tamaño de aumento de la base de datos. Puede ser el número de páginas o el porcentaje del tamaño del archivo, en función del valor de **status**.<br /><br /> 0 = Sin aumento de tamaño.|  
|**status**|**int**|Bits de estado para el valor de **crecimiento** en megabytes (MB) o kilobytes (KB).<br /><br /> 0x2 = Archivo de disco.<br /><br /> 0x40 = Archivo de registro.<br /><br /> 0x100000 = Crecimiento. Este valor es un porcentaje y no el número de páginas.|  
|**rendimiento**|**int**|Reservado.|  
|**name**|**sysname**|Nombre lógico del archivo.|  
|**filename**|**nvarchar(260)**|Nombre del dispositivo físico. Incluye la ruta de acceso completa al archivo.|  
  
## <a name="see-also"></a>Consulte también  
 [Asignar tablas del sistema a vistas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Vistas de compatibilidad &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
