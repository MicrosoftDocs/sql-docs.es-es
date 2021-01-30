---
description: suspect_pages (Transact-SQL)
title: suspect_pages (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- suspect_page_table
- suspect_page_table_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- suspect_pages system table
- suspect pages [SQL Server]
ms.assetid: 119c8d62-eea8-44fb-bf72-de469c838c50
author: cawrites
ms.author: chadam
ms.openlocfilehash: b7c25bf9c155afbe2e0517bea417c3955aa37d2f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191136"
---
# <a name="suspect_pages-transact-sql"></a>suspect_pages (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por página en la que se produjo un error pequeño 823 o un error 824. Se hace una lista de páginas en esta tabla porque se sospecha que estén dañadas, pero es posible que, en realidad, sean correctas. Cuando se repara una página sospechosa, su estado se actualiza en el **event_type** columna.  
  
 La tabla siguiente, que tiene un límite de 1.000 filas, se almacena en la base de datos **msdb** .  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**database_id**|**int**|Id. de la base de datos a la que se aplica esta página.|  
|**file_id**|**int**|Id. del archivo en la base de datos.|  
|**page_id**|**bigint**|Id. de la página sospechosa. Cada página incluye un valor de 32 bits de Id. de página que identifica su ubicación en la base de datos. El **page_id** es el desplazamiento en el archivo de datos de la página de 8 KB. Los Id. de página son únicos en un archivo.|  
|**event_type**|**int**|Tipo de error; puede ser uno de los siguientes:<br /><br /> 1 = Un error 823 que produce una página sospechosa (como un error de disco) o un error 824 distinto de una suma de comprobación incorrecta o una página rasgada (como un Id. de página no válido).<br /><br /> 2 = Suma de comprobación incorrecta.<br /><br /> 3 = Página rasgada.<br /><br /> 4 = Restaurada (la página se restauró después de marcarse como incorrecta).<br /><br /> 5 = Reparada (DBCC ha reparado la página).<br /><br /> 7 = Desasignada por DBCC.|  
|**error_count**|**int**|Número de veces que ha ocurrido el error.|  
|**last_update_date**|**datetime**|Marca de fecha y hora de la última actualización.|  
  
## <a name="permissions"></a>Permisos  
 Cualquier persona con acceso a **msdb** puede leer los datos de la tabla **suspect_pages** . Cualquier persona con el permiso UPDATE en la tabla suspect_pages puede actualizar sus registros. Los miembros del rol fijo de base de datos **db_owner** de **msdb** o el rol fijo de servidor **sysadmin** pueden insertar, actualizar y eliminar registros.  
  
## <a name="see-also"></a>Consulte también  
 [Restaurar páginas &#40;SQL Server&#41;](../../relational-databases/backup-restore/restore-pages-sql-server.md)   
 [Clase de eventos Database Suspect Data Page](../../relational-databases/event-classes/database-suspect-data-page-event-class.md)   
 [Tablas del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-tables/system-tables-transact-sql.md)   
 [Administrar la tabla suspect_pages &#40;SQL Server&#41;](../../relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
  
