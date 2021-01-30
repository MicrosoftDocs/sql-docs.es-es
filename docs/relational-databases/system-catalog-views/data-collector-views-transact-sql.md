---
description: Vistas del recopilador de datos (Transact-SQL)
title: Vistas del recopilador de datos (Transact-SQL) | Microsoft Docs
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
- data collector view
- data collector [SQL Server], views
ms.assetid: a005e885-7813-4c7e-b332-b01d9e9d4054
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bf304e44eb5f4374d4fdc1de21d7dc73fee2d72e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184308"
---
# <a name="data-collector-views-transact-sql"></a>Vistas del recopilador de datos (Transact-SQL)
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  El recopilador de datos proporciona las vistas siguientes para mostrar información sobre la configuración del recopilador de datos, como propiedades de tipo de recopilador, conjuntos de recopilación y elementos de conjuntos de recopilación, así como estadísticas de ejecución que se obtienen cuando se ejecuta un conjunto de recopilación. Estas vistas, que están en la base de datos **msdb** , también proporcionan una capa de abstracción para las tablas subyacentes. Esta abstracción mejora la seguridad ya que evita el acceso directo a las tablas, al tiempo que permite cambios en las tablas sin afectar a ninguna aplicación asociada.  

:::row:::
    :::column:::
        [syscollector_collection_items &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)
        
        [syscollector_collector_types &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)
        
        [syscollector_execution_log &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-execution-log-transact-sql.md)
        
        [syscollector_execution_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-execution-stats-transact-sql.md)
    :::column-end:::
    :::column:::
        [syscollector_collection_sets &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md)
        
        [syscollector_config_store &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-config-store-transact-sql.md)
        
        [syscollector_execution_log_full &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/syscollector-execution-log-full-transact-sql.md)
    :::column-end:::
:::row-end:::
  
## <a name="see-also"></a>Consulte también  
 [Recopilación de datos](../../relational-databases/data-collection/data-collection.md)   
 [Procedimientos almacenados del recopilador de datos &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/data-collector-stored-procedures-transact-sql.md)  
  
  
