---
description: sys.filetable_system_defined_objects (Transact-SQL)
title: sys.filetable_system_defined_objects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.filetable_system_defined_objects_TSQL
- filetable_system_defined_objects
- filetable_system_defined_objects_TSQL
- sys.filetable_system_defined_objects
dev_langs:
- TSQL
helpviewer_keywords:
- sys.filetable_system_defined_objects catalog view
ms.assetid: 62022e6b-46f6-495f-b14b-53f41e040361
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 35f0f67da1397fec83d6425be71b55da186b3056
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99193947"
---
# <a name="sysfiletable_system_defined_objects-transact-sql"></a>sys.filetable_system_defined_objects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Muestra una lista de los objetos definidos por el sistema relacionados con objetos FileTable. Contiene una fila por cada objeto definido por el sistema.  
  
 Cuando se crea un objeto FileTable, los objetos relacionados como restricciones e índices se crean al mismo tiempo. No puede modificar ni quitar estos objetos; desaparecen solo cuando se quita el propio objeto FileTable.  
  
 Para más información sobre FileTables, vea [FileTables &#40;SQL Server&#41;](../../relational-databases/blob/filetables-sql-server.md).  
  
|Columna|Tipo de datos|Descripción|  
|------------|---------------|-----------------|  
|**object_id**|**int**|Identificador de objeto del objeto definido por el sistema relacionado con una tabla FileTable.<br /><br /> Hace referencia al objeto de **Sys. Objects**.|  
|**parent_object_id**|**int**|Identificador de objeto de la tabla FileTable primaria.<br /><br /> Hace referencia al objeto de **Sys. Objects**.|  
  
## <a name="see-also"></a>Consulte también  
 [Crear, modificar y quitar FileTables](../../relational-databases/blob/create-alter-and-drop-filetables.md)   
 [Administrar FileTables](../../relational-databases/blob/manage-filetables.md)  
  
  
