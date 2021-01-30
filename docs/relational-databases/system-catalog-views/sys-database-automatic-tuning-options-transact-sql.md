---
title: sys.database_automatic_tuning_options (Transact-SQL) | Microsoft Docs
description: Obtenga información sobre cómo ver las opciones de ajuste automático en un SQL Database. Consulte permisos necesarios y ver recursos adicionales disponibles.
ms.custom: ''
ms.date: 07/20/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- database_automatic_tuning_options_tsql
- database_automatic_tuning_options
- sys.database_automatic_tuning_options_tsql
- sys.database_automatic_tuning_options
dev_langs:
- TSQL
helpviewer_keywords:
- database_automatic_tuning_options catalog view
- sys.database_automatic_tuning_options catalog view
ms.assetid: 16b47d55-8019-41ff-ad34-1e0112178067
author: jovanpop-msft
ms.author: jovanpop
monikerRange: =azuresqldb-current||>=sql-server-2017||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 0dbedd65c5f98016e4e001fd863845079da68578
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210381"
---
# <a name="sysdatabase_automatic_tuning_options-transact-sql"></a>Sys. Database \_ \_ (tuning_options automática) (TRANSACT-SQL)
[!INCLUDE[sqlserver2017-asdb](../../includes/applies-to-version/sqlserver2017-asdb.md)]

  Devuelve las opciones de ajuste automático de esta base de datos.  

|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**nvarchar(128)**|Nombre de la opción de ajuste automático. Consulte [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md) para ver las opciones disponibles.|  
|**desired_state**|**smallint**|Indica el modo de operación deseado para la opción de ajuste automático, establecido explícitamente por el usuario.<br />0 = OFF<br />1 = ON|  
|**desired_state_desc**|**nvarchar(60)**|Descripción textual del modo de operación deseado de la opción de ajuste automático.<br />Apagado<br />ACTIVAR|  
|**actual_state**|**smallint**|Indica el modo de operación de la opción de ajuste automático.<br />0 = OFF<br />1 = ON|  
|**actual_state_desc**|**nvarchar(60)**|Descripción textual del modo de operación real de la opción de ajuste automático.<br />Apagado<br />ACTIVAR|  
|**reason**|**smallint**|Indica por qué los Estados real y deseado son diferentes.<br />2 = DESHABILITADO<br />11 = QUERY_STORE_OFF<br />12 = QUERY_STORE_READ_ONLY<br />13 = NOT_SUPPORTED|   
|**reason_desc**|**nvarchar(60)**|Descripción textual de la razón por la que los Estados real y deseado son diferentes.<br />Disabled = la opción está deshabilitada por el sistema<br />QUERY_STORE_OFF = Almacén de consultas está desactivado<br />QUERY_STORE_READ_ONLY = Almacén de consultas está en modo de solo lectura<br />NOT_SUPPORTED = solo disponible en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Enterprise Edition| 
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [Ajuste automático](../../relational-databases/automatic-tuning/automatic-tuning.md)   
 [ALTER DATABASE SET AUTOMATIC_TUNING &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md)   
 [sys.database_query_store_options &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-database-query-store-options-transact-sql.md)   
 [sys.dm_db_tuning_recommendations &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-db-tuning-recommendations-transact-sql.md)   
 
