---
description: sys.dm_hadr_auto_page_repair (Transact-SQL)
title: sys.dm_hadr_auto_page_repair (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/05/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_hadr_auto_page_repair_TSQL
- sys.dm_hadr_auto_page_repair
- sys.dm_hadr_auto_page_repair_TSQL
- dm_hadr_auto_page_repair
dev_langs:
- TSQL
helpviewer_keywords:
- Availability Groups [SQL Server], monitoring
- automatic page repair
- sys.dm_hadr_auto_page_repair dynamic management view
ms.assetid: d7840adf-4a1b-41ac-bc94-102c07ad1c79
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: b7ec1c79c7d3142909750ebbc525729fa75b82c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354183"
---
# <a name="sysdm_hadr_auto_page_repair-transact-sql"></a>sys.dm_hadr_auto_page_repair (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve una fila por cada intento de reparación de página automática en cualquier base de datos de disponibilidad en una réplica de disponibilidad hospedada para cualquier grupo de disponibilidad por la instancia de servidor. Esta vista contiene las filas para los últimos intentos de reparación de página automática en una base de datos principal o secundaria determinada, con un máximo de 100 filas por base de datos. En cuanto una base de datos alcanza el máximo, la fila del siguiente intento de reparación de página automática reemplazará una de las entradas existentes.
  
  En la tabla siguiente se define el significado de las diversas columnas:  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Identificador la base de datos a la que corresponde esta fila.|  
|**file_id**|**int**|Identificador del archivo en el que se encuentra la página.|  
|**page_id**|**bigint**|Identificador de la página en el archivo.|  
|**error_type**|**int**|Tipo del error. Los valores pueden ser:<br /><br /> **-** 1 = todos los errores de hardware 823<br /><br /> 1 = Errores 824 distintos de una suma de comprobación errónea o una página rasgada (por ejemplo, un identificador de página incorrecto)<br /><br /> 2 = Suma de comprobación incorrecta<br /><br /> 3 = Página rasgada|  
|**page_status**|**int**|El estado del intento de reparación de la página:<br /><br /> 2 = En cola para la solicitud del socio.<br /><br /> 3 = Solicitud enviada al socio.<br /><br /> 4 = la página se reparó correctamente.<br /><br /> 5 = no se pudo reparar la página durante el último intento o la reparación de página automática intentará reparar la página de nuevo.|  
|**modification_time**|**datetime**|Hora del último cambio en el estado de la página.|  
  
## <a name="security"></a>Seguridad  
  
### <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
## <a name="see-also"></a>Consulte también  
 [Reparación de página automática &#40;grupos de disponibilidad: creación de reflejo de la base de datos&#41;](../../sql-server/failover-clusters/automatic-page-repair-availability-groups-database-mirroring.md)   
 [suspect_pages &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/suspect-pages-transact-sql.md)   
 [Administrar la tabla suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  

