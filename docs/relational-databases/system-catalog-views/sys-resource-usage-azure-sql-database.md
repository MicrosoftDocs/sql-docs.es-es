---
description: sys.resource_usage (Azure SQL Database)
title: sys.resource_usage (Azure SQL Database) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-database
ms.reviewer: ''
ms.topic: reference
f1_keywords:
- sys.resource_usage_TSQL
- resource_usage
- sys.resource_usage
- resource_usage_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- resource_usage
- sys.resource_usage
ms.assetid: b90147a3-fd8e-408e-961d-5c7000e068ad
author: julieMSFT
ms.author: jrasnick
monikerRange: = azuresqldb-current
ms.openlocfilehash: 1c9ef43ee6834a890359ea46bcab7ff388ec7d4f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193006"
---
# <a name="sysresource_usage-azure-sql-database"></a>sys.resource_usage (Azure SQL Database)
[!INCLUDE[Azure SQL Database Azure SQL Managed Instance](../../includes/applies-to-version/asdb-asdbmi.md)]

    
> [!IMPORTANT]
>  Esta característica se encuentra en un estado de vista previa. No dependa de la implementación específica de esta característica, ya que podría modificarse o quitarse en una versión futura.  
> 
>  Mientras se encuentra en estado de vista previa, el equipo de operaciones de [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] podría activar y desactivar la recopilación de datos para esta DMV:  
> 
>  -   Si está activada, la DMV devuelve los datos actuales tal como están agregados.  
> -   Si está desactivada, la DMV devuelve datos históricos, que podrían estar desusados.  
  
 Proporciona un resumen por horas de los datos de uso de recursos de las bases de datos de usuario del servidor actual. Los datos históricos se conservan durante 90 días.  
  
 Para cada base de datos de usuario, existe una fila para cada hora de forma continua. Aparece una fila incluso si la base de datos está inactiva durante esa hora, y el valor de usage_in_seconds para esa base de datos será 0. El uso del almacenamiento y la información de SKU se acumulan durante esa hora de forma apropiada.  
  
|Columnas|Tipo de datos|Descripción|  
|-------------|---------------|-----------------|  
|time|**datetime**|Hora (UTC) en incrementos de una hora.|  
|database_name|**nvarchar**|Nombre de la base de datos de usuario.|  
|sku|**nvarchar**|Nombre de la SKU. Los posibles valores son los siguientes:<br /><br /> Web<br /><br /> Business<br /><br /> Básico<br /><br /> Estándar<br /><br /> Premium|  
|usage_in_seconds|**int**|Suma del tiempo de CPU utilizado durante esa hora.<br /><br /> Nota: esta columna está en desuso para V11 y no se aplica a V12. **El valor siempre se establece en 0.**|  
|storage_in_megabytes|**decimal**|Tamaño de almacenamiento máximo para la hora, incluidos los datos, los índices, los procedimientos almacenados y los metadatos de la base de datos.|  
  
## <a name="permissions"></a>Permisos  
 Esta vista está disponible para todos los roles de usuario con permisos para conectarse a la base de datos **maestra** virtual.  
  
  
