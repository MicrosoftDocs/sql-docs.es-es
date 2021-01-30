---
description: sys.dm_xe_object_columns (Transact-SQL)
title: sys.dm_xe_object_columns (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_xe_object_columns
- sys.dm_xe_object_columns_TSQL
- dm_xe_object_columns_TSQL
- dm_xe_object_columns
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_xe_object_columns dynamic management view
- extended events [SQL Server], views
ms.assetid: d96a14f3-4284-45ff-b1fe-4858e540a013
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 3bc82f0d9018620fc64fb12e6c8864dd15358ebf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99129427"
---
# <a name="sysdm_xe_object_columns-transact-sql"></a>sys.dm_xe_object_columns (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Devuelve la información del esquema para todos los objetos.  
  
> [!NOTE]  
>  Los objetos de eventos ofrecen esquemas fijos para los datos de solo lectura y de escritura.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|name|**nvarchar(256)**|El nombre de la columna. Name es único en el objeto. No admite valores NULL.|  
|column_id|**int**|Identificador de la columna. column_id es único dentro del objeto cuando se usa con column_type. No admite valores NULL.|  
|object_name|**nvarchar(256)**|Nombre del objeto al que pertenece esta columna. Hay una relación de varios a uno con sys.dm_xe_objects. ID. No acepta valores NULL.|  
|object_package_guid|**uniqueidentifier**|GUID del paquete que contiene el objeto. No admite valores NULL.|  
|type_name|**nvarchar(256)**|Nombre del tipo de esta columna. No admite valores NULL.|  
|type_package_guid|**uniqueidentifier**|GUID del paquete que contiene el tipo de datos de la columna. No admite valores NULL.|  
|column_type|**nvarchar(60)**|Indica cómo se utiliza esta columna. No admite valores NULL. column_type puede ser una de las siguientes:<br /><br /> readonly. La columna contiene un valor estático que no se puede cambiar.<br /><br /> del modelo. La columna contiene datos en tiempo de ejecución expuestos por el objeto.<br /><br /> customizable. La columna contiene un valor que puede cambiarse.<br /><br /> Nota: al cambiar este valor se puede modificar el comportamiento del objeto.|  
|column_value|**nvarchar(256)**|Muestra los valores estáticos asociados con la columna de objetos. Acepta valores NULL.|  
|capabilities|**int**|Un mapa de bits que describe las capacidades de la columna. Acepta valores NULL.|  
|capabilities_desc|**nvarchar(256)**|Una descripción de las capacidades de esta columna de objetos. Este valor puede ser uno de los siguientes:<br /><br /> Mandatory. Se debe establecer el valor al enlazar el objeto primario a una sesión de eventos.<br /><br /> Acepta valores NULL.|  
|description|**nvarchar (a.**|Descripción de esta columna de objetos. Acepta valores NULL.|  
  
## <a name="permissions"></a>Permisos  
 es necesario contar con el permiso VIEW SERVER STATE en el servidor.  
  
### <a name="relationship-cardinalities"></a>Cardinalidades de relación  
  
|From|En|Relación|  
|----------|--------|------------------|  
|sys.dm_xe_object_columns.object_name, sys.dm_xe_object_columns.object_package_guid|sys.dm_xe_objects.name,<br /><br /> sys.dm_xe_objects.package_guid|Varios a uno|  
|sys.dm_xe_object_columns.type_name<br /><br /> sys.dm_xe_object_columns.type_package_guid|sys.dm_xe_objects.name<br /><br /> sys.dm_xe_objects.package_guid|Varios a uno|  
  
## <a name="see-also"></a>Consulte también  
 [Funciones y vistas de administración dinámica &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)  
  
  

