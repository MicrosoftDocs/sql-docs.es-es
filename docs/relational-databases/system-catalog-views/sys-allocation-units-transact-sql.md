---
description: sys.allocation_units (Transact-SQL)
title: sys.allocation_units (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.allocation_units_TSQL
- sys.allocation_units
- allocation_units_TSQL
- allocation_units
dev_langs:
- TSQL
helpviewer_keywords:
- sys.allocation_units catalog view
ms.assetid: ec9de780-68fd-4551-b70b-2d3ab3709b3e
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 59fc26433dcf98c88cd0172b8a25ef288622547e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210085"
---
# <a name="sysallocation_units-transact-sql"></a>sys.allocation_units (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  Contiene una fila por cada unidad de asignación de la base de datos.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|allocation_unit_id|**bigint**|Id. de la unidad de asignación. Es único en una base de datos.|  
|tipo|**tinyint**|Tipo de unidad de asignación:<br /><br /> 0 = Quitado<br /><br /> 1 = Datos de fila (todos los tipos de datos, excepto datos LOB)<br /><br /> 2 = datos de objetos grandes (LOB) (**Text**, **ntext**, **Image**, **XML**, tipos de valores grandes y tipos definidos por el usuario CLR)<br /><br /> 3 = Datos de desbordamiento de fila|  
|type_desc|**nvarchar(60)**|Descripción del tipo de unidad de asignación:<br /><br /> **REDUCIDO**<br /><br /> **IN_ROW_DATA**<br /><br /> **LOB_DATA**<br /><br /> **ROW_OVERFLOW_DATA**|  
|container_id|**bigint**|Id. del contenedor de almacenamiento asociado con la unidad de asignación.<br /><br /> Si type = 1 ó 3, container_id = sys.partitions.hobt_id.<br /><br /> Si type = 2, entonces container_id = sys.partitions.partition_id.<br /><br /> 0 = Unidad de asignación marcada para eliminación diferida|  
|data_space_id|**int**|Id. del grupo de archivos donde reside la unidad de asignación.|  
|total_pages|**bigint**|Número total de páginas asignadas o reservadas por esta unidad de asignación.|  
|used_pages|**bigint**|Número total de páginas que actualmente están en uso.|  
|data_pages|**bigint**|Número de páginas usadas que tienen:<br /><br /> Datos de fila<br /><br /> Datos LOB<br /><br /> Datos de desbordamiento de fila<br /><br /> <br /><br /> Tenga en cuenta que el valor devuelto excluye las páginas de índice internas y las páginas de administración de asignación.|  
  
> [!NOTE]  
>  Al quitar o volver a generar índices grandes, o al quitar o truncar tablas grandes, el [!INCLUDE[ssDE](../../includes/ssde-md.md)] difiere las cancelaciones de asignación de páginas, así como sus bloqueos asociados, hasta que se confirma la transacción. Las operaciones de eliminación diferidas no liberan inmediatamente el espacio asignado. Por tanto, los valores devueltos por sys.allocation_units inmediatamente después de quitar o truncar un objeto grande pueden no reflejar el espacio en disco real que está disponible.  
  
## <a name="permissions"></a>Permisos  
 Debe pertenecer al rol **public** . Para obtener más información, consulte [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md).  
  
## <a name="see-also"></a>Consulte también  
 [sys.partitions &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-partitions-transact-sql.md)   
 [Object Catalog Views &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/object-catalog-views-transact-sql.md)  (Vistas de catálogo de objetos [Transact-SQL])  
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
