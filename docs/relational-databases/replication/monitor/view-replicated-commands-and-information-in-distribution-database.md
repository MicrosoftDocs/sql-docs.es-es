---
title: Consulta de los comandos replicados e información en una base de datos de distribución
description: Obtenga información sobre cómo ver comandos replicados y otra información relacionada con la replicación en la base de datos de distribución para SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- sp_browsereplcmds
- transactional replication, monitoring
- distribution databases [SQL Server replication], viewing replicated commands
- viewing replicated commands
ms.assetid: 9c20acec-8fab-4483-b9c1-dfe3768f85dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 222c902eae5b38a634f382ac0d393dc741db61fe
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463236"
---
# <a name="view-replicated-commands-and-information-in-distribution-database"></a>Ver comandos replicados e información en una base de datos de distribución
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  Al utilizar la replicación transaccional, los comandos de transacción están almacenados en la base de datos de distribución hasta que el Agente de distribución los propaga a todos los suscriptores o un Agente de distribución del suscriptor extrae los cambios. Estos comandos pendientes en la base de datos de distribución se pueden ver utilizando mediante programación los procedimientos almacenados de replicación. Para obtener más información, consulte [Replication Stored Procedures &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md) (Procedimientos almacenados de replicación [Transact-SQL]).  
  
### <a name="to-view-replicated-commands-from-all-transactional-publications-in-the-distribution-database"></a>Para ver los comandos replicados de todas las publicaciones transaccionales en la base de datos de distribución  
  
1.  En cualquier base de datos de distribución, ejecute [sp_browsereplcmds](../../../relational-databases/system-stored-procedures/sp-browsereplcmds-transact-sql.md).  
  
### <a name="to-view-replicated-commands-in-the-distribution-database-from-a-specific-article-or-from-a-specific-database-published-using-transactional-replication"></a>Para ver los comandos replicados en la base de datos de distribución de un artículo concreto o de una base de datos concreta publicada utilizando replicación transaccional  
  
1.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md). Especifique `@publication` y `@article`. Anote el valor de **article** del conjunto de resultados.  
  
2.  En cualquier base de datos de distribución, ejecute [sp_browsereplcmds](../../../relational-databases/system-stored-procedures/sp-browsereplcmds-transact-sql.md). (Opcional) Especifique el id. del artículo del paso 2 para `@article_id`. (Opcional) Especifique el id. de la base de datos de publicación para `@publisher_database_id`, que se puede obtener de la columna **database_id** de la vista de catálogo [sys.databases](../../../relational-databases/system-catalog-views/sys-databases-transact-sql.md).  
  
## <a name="see-also"></a>Consulte también  
 [Supervisar la replicación mediante programación](../../../relational-databases/replication/monitor/programmatically-monitor-replication.md)  
  
  
