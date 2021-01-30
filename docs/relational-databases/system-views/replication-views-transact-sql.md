---
title: Vistas de replicación (Transact-SQL) | Microsoft Docs
description: Las vistas de replicación contienen información que utiliza la replicación en SQL Server. Las vistas facilitan el acceso a los datos de las tablas del sistema de replicación.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
dev_langs:
- TSQL
helpviewer_keywords:
- distribution databases [SQL Server replication], system views
- metadata [SQL Server], views
- views [SQL Server], replication
- replication views [SQL Server]
- publications [SQL Server replication], system views
- articles [SQL Server replication], system views
- replication metadata [SQL Server]
- subscriptions [SQL Server replication], system views
- system views [SQL Server], replication
ms.assetid: 93e5056d-0d93-4a48-ba33-72762eb995d8
author: stevestein
ms.author: sstein
ms.openlocfilehash: 64fcd7c57c57dec9978695b52db1af2cc2c8ebba
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160358"
---
# <a name="replication-views-transact-sql"></a>Vistas de replicación (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Estas vistas contienen información que utiliza la replicación en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Las vistas permiten un acceso más sencillo a los datos de [las tablas del sistema de replicación](../../relational-databases/system-tables/replication-tables-transact-sql.md). Las vistas se crean en una base de datos de usuario cuando ésta está habilitada como una base de datos de publicaciones o suscripciones.  Todos los objetos de replicación se quitan de las bases de datos de usuario cuando se quita la base de datos de una topología de replicación. El método preferido para obtener acceso a los metadatos de replicación es mediante [procedimientos almacenados de replicación](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md).  
  
> [!IMPORTANT]  
>  Ningún usuario debe modificar directamente las vistas del sistema.  
  
## <a name="replication-views"></a>Vistas de replicación  
 A continuación, se presenta una lista de las vistas del sistema utilizadas por la replicación, agrupadas por base de datos.  
  
### <a name="replication-views-in-the-msdb-database"></a>Vistas de replicación de la base de datos msdb  

:::row:::
    :::column:::
        [MSdatatype_mappings &#40;&#41;de Transact-SQL ](../../relational-databases/system-views/msdatatype-mappings-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysdatatypemappings &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysdatatypemappings-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-distribution-database"></a>Vistas de replicación de la base de datos de distribución  

:::row:::
    :::column:::
        [IHextendedArticleView &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihextendedarticleview-transact-sql.md)

        [IHextendedSubscriptionView &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihextendedsubscriptionview-transact-sql.md)

        [IHsyscolumns &#40;Transact-SQL&#41;](../../relational-databases/system-views/ihsyscolumns-transact-sql.md)

        [MSdistribution_status &#40;&#41;de Transact-SQL ](../../relational-databases/system-views/msdistribution-status-transact-sql.md)

        [sysarticlecolumns &#40;vista del sistema&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticlecolumns-system-view-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysarticles &#40;vista del sistema&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md)

        [sysextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysextendedarticlesview-transact-sql.md)

        [syspublications &#40;Vista del sistema&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syspublications-system-view-transact-sql.md)

        [syssubscriptions &#40;Vista del sistema&#41; &#40;Transact-SQL&#41;](../../relational-databases/system-views/syssubscriptions-system-view-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-publication-database"></a>Vistas de replicación de la base de datos de publicaciones  

:::row:::
    :::column:::
        [sysmergeextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergeextendedarticlesview-transact-sql.md)

        [systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
    :::column:::
        [sysmergepartitioninfoview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergepartitioninfoview-transact-sql.md)
    :::column-end:::
:::row-end:::

### <a name="replication-views-in-the-subscription-database"></a>Vistas de replicación de la base de datos de suscripciones  

:::row:::
    :::column:::
        [sysmergeextendedarticlesview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergeextendedarticlesview-transact-sql.md)

        [sysmergepartitioninfoview &#40;Transact-SQL&#41;](../../relational-databases/system-views/sysmergepartitioninfoview-transact-sql.md)
    :::column-end:::
    :::column:::
        [systranschemas &#40;Transact-SQL&#41;](../../relational-databases/system-views/systranschemas-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)  
  
  
