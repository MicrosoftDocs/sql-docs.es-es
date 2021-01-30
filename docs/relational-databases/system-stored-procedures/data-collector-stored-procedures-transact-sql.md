---
description: Procedimientos almacenados del recopilador de datos (Transact-SQL)
title: Procedimientos almacenados del recopilador de datos (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- stored procedures [SQL Server], data collector
- system stored procedures [SQL Server], data collector
- data collector [SQL Server], stored procedures
ms.assetid: 9dd2824f-ea55-439b-8cd5-3a81fedb1432
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5bb5a4e980e43c5f2e21a4a72b072a9899f6f81
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199633"
---
# <a name="data-collector-stored-procedures-transact-sql"></a>Procedimientos almacenados del recopilador de datos (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  SQL Server admite los siguientes procedimientos almacenados del sistema que se usan para trabajar con el recopilador de datos y los componentes siguientes: conjuntos de recopilación, elementos de recopilación y tipos de colección.  
  
> [!IMPORTANT]  
>  A diferencia de los procedimientos almacenados normales, los parámetros de los procedimientos del  recopilador de datos deben escribirse de forma precisa y no admiten la conversión automática de tipos de datos. Si no se llama a estos parámetros con los tipos de datos de parámetros de entrada correctos, según se especifica en la descripción del argumento, el procedimiento almacenado devuelve un error.  

:::row:::
    :::column:::
        [sp_syscollector_create_collection_item](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-item-transact-sql.md)

        [sp_syscollector_create_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)

        [sp_syscollector_create_collector_type](../../relational-databases/system-stored-procedures/sp-syscollector-create-collector-type-transact-sql.md)

        [sp_syscollector_delete_collection_item](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-item-transact-sql.md)

        [sp_syscollector_delete_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-set-transact-sql.md)

        [sp_syscollector_delete_collector_type](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collector-type-transact-sql.md)

        [sp_syscollector_delete_execution_log_tree](../../relational-databases/system-stored-procedures/sp-syscollector-delete-execution-log-tree-transact-sql.md)

        [sp_syscollector_disable_collector](../../relational-databases/system-stored-procedures/sp-syscollector-disable-collector-transact-sql.md)

        [sp_syscollector_enable_collector](../../relational-databases/system-stored-procedures/sp-syscollector-enable-collector-transact-sql.md)

        [sp_syscollector_set_cache_directory](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-directory-transact-sql.md)
    :::column-end:::
    :::column:::
        [sp_syscollector_set_cache_window](../../relational-databases/system-stored-procedures/sp-syscollector-set-cache-window-transact-sql.md)

        [sp_syscollector_set_warehouse_database_name](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-database-name-transact-sql.md)

        [sp_syscollector_set_warehouse_instance_name](../../relational-databases/system-stored-procedures/sp-syscollector-set-warehouse-instance-name-transact-sql.md)

        [sp_syscollector_start_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-start-collection-set-transact-sql.md)

        [sp_syscollector_stop_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-stop-collection-set-transact-sql.md)

        [sp_syscollector_run_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-run-collection-set-transact-sql.md)

        [sp_syscollector_update_collection_item](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-item-transact-sql.md)

        [sp_syscollector_update_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-set-transact-sql.md)

        [sp_syscollector_update_collector_type](../../relational-databases/system-stored-procedures/sp-syscollector-update-collector-type-transact-sql.md)

        [sp_syscollector_upload_collection_set](../../relational-databases/system-stored-procedures/sp-syscollector-upload-collection-set-transact-sql.md)
    :::column-end:::
:::row-end:::

 Los procedimientos almacenados siguientes son únicamente para uso interno:  
  
-   sp_syscollector_get_warehouse_connection_string  
  
-   sp_syscollector_set_warehouse_connection_password  
  
-   sp_syscollector_set_warehouse_connection_user  
  
-   sp_syscollector_event_oncollectionbegin  
  
-   sp_syscollector_event_oncollectionend  
  
-   sp_syscollector_event_onpackagebegin  
  
-   sp_syscollector_event_onpackageend  
  
-   sp_syscollector_event_onpackageupdate  
  
-   sp_syscollector_event_onerror  
  
-   sp_syscollector_event_onstatsupdate  
  
## <a name="see-also"></a>Consulte también  
 [Procedimientos almacenados del sistema &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
