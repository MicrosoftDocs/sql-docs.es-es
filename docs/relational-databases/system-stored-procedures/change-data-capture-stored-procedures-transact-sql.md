---
description: Procedimientos almacenados de captura de datos modificados (Transact-SQL)
title: Procedimientos almacenados de captura de datos modificados (Transact-SQL) | Microsoft Docs
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
- system stored procedures [SQL Server], change data capture
- change data capture [SQL Server], stored procedures
ms.assetid: 7da7068d-6388-465a-b708-a2f27ded1efe
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 1b0794e4c35038a16f09a41098cad3b7384783a1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203759"
---
# <a name="change-data-capture-stored-procedures-transact-sql"></a>Procedimientos almacenados de captura de datos modificados (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La captura de datos modificados hace que el registro histórico de la actividad del Lenguaje de manipulación de datos (DML) que se produjo en las tablas habilitadas esté disponible en un formato relacional apropiado. Los procedimientos almacenados siguientes se utilizan para configurar la captura de datos modificados, administrar los trabajos del Agente de captura de datos modificados y proporcionar los metadatos actuales para cambiar los consumidores de datos.  

:::row:::
    :::column:::
        [sys.sp_cdc_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-add-job-transact-sql.md)

        [sys.sp_cdc_change_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-change-job-transact-sql.md)

        [sys.sp_cdc_cleanup_change_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-cleanup-change-table-transact-sql.md)

        [sys.sp_cdc_disable_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-db-transact-sql.md)

        [sys.sp_cdc_disable_table &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-disable-table-transact-sql.md)

        [sys.sp_cdc_drop_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-drop-job-transact-sql.md)

        [sys.sp_cdc_enable_db &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-db-transact-sql.md)

        [sys.sp_cdc_enable_table &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-enable-table-transact-sql.md)
    :::column-end:::
    :::column:::
        [sys.sp_cdc_generate_wrapper_function &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-generate-wrapper-function-transact-sql.md)

        [sys.sp_cdc_get_captured_columns &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-captured-columns-transact-sql.md)

        [sys.sp_cdc_get_ddl_history &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-get-ddl-history-transact-sql.md)

        [sys.sp_cdc_help_change_data_capture &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-change-data-capture-transact-sql.md)

        [sys.sp_cdc_help_jobs &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-help-jobs-transact-sql.md)

        [sys.sp_cdc_scan &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-scan-transact-sql.md)

        [sys.sp_cdc_start_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-start-job-transact-sql.md)

        [sys.sp_cdc_stop_job &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sys-sp-cdc-stop-job-transact-sql.md)
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>Consulte también  
 [Tablas de captura de datos de cambio &#40;Transact-SQL&#41;](../../relational-databases/system-tables/change-data-capture-tables-transact-sql.md)  
  
  
