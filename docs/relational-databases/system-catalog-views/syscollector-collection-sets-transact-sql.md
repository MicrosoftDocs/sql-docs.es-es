---
description: syscollector_collection_sets (Transact-SQL)
title: syscollector_collection_sets (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syscollector_collection_sets_TSQL
- syscollector_collection_sets
dev_langs:
- TSQL
helpviewer_keywords:
- data collector view
- syscollector_collection_sets view
ms.assetid: db0def92-f25b-45da-9709-eab972b33800
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: ffa91193b3c09f64d608c255bb8960b7dfb85823
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199775"
---
# <a name="syscollector_collection_sets-transact-sql"></a>syscollector_collection_sets (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Proporciona información sobre un conjunto de recopilación, incluida la programación, el modo de recopilación y su estado.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|collection_set_id|**int**|Identificador local del conjunto de recopilación. No admite valores NULL.|  
|collection_set_uid|**uniqueidentifier**|Identificador único global del conjunto de recopilación. No admite valores NULL.|  
|name|**nvarchar(4000)**|Nombre del conjunto de recopilación. Acepta valores NULL.|  
|Destino|**nvarchar(max)**|Identifica el destino del conjunto de recopilación. Acepta valores NULL.|  
|is_system|**bit**|Se activa (1) o desactiva (0) para indicar si el conjunto de recopilación se incluyó con el recopilador de datos o si se agregó más tarde por medio de dc_admin. Este podría ser un conjunto de recopilación desarrollado internamente o por terceros. No admite valores NULL.|  
|is_running|**bit**|Indica si el conjunto de recopilación se está ejecutando o no. No admite valores NULL.|  
|collection_mode|**smallint**|Especifica el modo de recopilación para el conjunto de recopilación. No admite valores NULL.<br /><br /> El modo de recopilación es uno de lo siguiente:<br /><br /> 0 - Modo de almacenamiento en caché. La recopilación y la carga de datos están en programaciones independientes.<br /><br /> 1 - Modo sin almacenamiento en caché. La recopilación y la carga de datos están en la misma programación.|  
|proxy_id|**int**|Identifica el proxy que se utiliza para ejecutar el paso de trabajo correspondiente al conjunto de recopilación. Acepta valores NULL.|  
|schedule_uid|**uniqueidentifier**|Proporciona un puntero para la programación del conjunto de recopilación. Acepta valores NULL.|  
|collection_job_id|**uniqueidentifier**|Identifica el trabajo de recopilación. Acepta valores NULL.|  
|upload_job_id|**uniqueidentifier**|Identifica el trabajo de carga de recopilación. Acepta valores NULL.|  
|logging_level|**smallint**|Especifica el nivel de registro (0, 1 ó 2). No admite valores NULL.|  
|days_until_expiration|**smallint**|Número de días que los datos recopilados se guardan en el almacén de administración de datos. No admite valores NULL.|  
|description|**nvarchar(4000)**|Describe el conjunto de recopilación. Acepta valores NULL.|  
|dump_on_any_error|**bit**|Está activado (1) o desactivado (0) para indicar si se debe crear un [!INCLUDE[ssIS](../../includes/ssis-md.md)] archivo de volcado de memoria en cualquier error. No admite valores NULL.|  
|dump_on_codes|**nvarchar(max)**|Contiene la lista de [!INCLUDE[ssIS](../../includes/ssis-md.md)] códigos de error que se usan para desencadenar el archivo de volcado de memoria. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 Requiere SELECT para dc_operator, dc_proxy.  
  
## <a name="remarks"></a>Observaciones  
 El recopilador de datos API solamente permite cambiar o eliminar los conjuntos de recopilación creados por el usuario. Los conjuntos de recopilación que se proporcionan con el sistema no pueden modificarse ni eliminarse. No obstante, es posible habilitar o deshabilitar un conjunto de recopilación del sistema y cambiar su configuración.  
  
## <a name="see-also"></a>Vea también  
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)   
 [Vistas del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-collector-views-transact-sql.md)   
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)  
  
  
