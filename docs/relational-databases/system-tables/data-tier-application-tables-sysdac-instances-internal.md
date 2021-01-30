---
description: 'Tablas de aplicación de capa de datos: sysdac_instances_internal'
title: sysdac_instances_internal (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sysdac_instances_internal_TSQL
- sysdac_instances_internal
dev_langs:
- TSQL
helpviewer_keywords:
- sysdac_instances_internal
ms.assetid: d2d52cc4-3463-431a-b779-6fbfdeee1dfc
author: cawrites
ms.author: chadam
ms.openlocfilehash: 043c3a116799d8ab76b02b982c2a5ce3ec19fa83
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99187919"
---
# <a name="data-tier-application-tables---sysdac_instances_internal"></a>Tablas de aplicación de capa de datos: sysdac_instances_internal
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una fila para cada instancia de aplicación de capa de datos (DAC) implementada en una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Esta tabla se almacena en el esquema DBO de la base de datos msdb.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|instance_id|**uniqueidentifier**|Identificador de la instancia de DAC.|  
|nombre_instancia|**sysname**|Nombre de la instancia de DAC especificada cuando se implementó la instancia.|  
|type_name|**sysname**|Nombre de la DAC especificada cuando se creó el paquete DAC.|  
|type_version|**nvarchar (64)**|Versión de la DAC especificada cuando se creó el paquete DAC.|  
|description|**nvarchar(4000)**|Descripción de la DAC escrita cuando se creó el paquete DAC.|  
|type_stream|**varbinary(max)**|Flujo de bits que contiene la representación codificada de los objetos lógicos, como tablas y vistas, contenidos en la DAC.|  
|date_created|**datetime**|Fecha y hora en que se creó la instancia de DAC.|  
|created_by|**sysname**|Inicio de sesión que creó la instancia de DAC.|  
  
## <a name="remarks"></a>Observaciones  
 El acceso de solo lectura a esta vista está disponible para todos los usuarios con permisos para conectarse a la base de datos maestra.  
  
## <a name="permissions"></a>Permisos  
 Requiere la pertenencia al rol fijo de servidor sysadmin.  
  
## <a name="see-also"></a>Consulte también  
 [Aplicaciones de capa de datos](../../relational-databases/data-tier-applications/data-tier-applications.md)   
 [dbo.sysdac_instances &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/data-tier-application-views-dbo-sysdac-instances.md)  
  
  
