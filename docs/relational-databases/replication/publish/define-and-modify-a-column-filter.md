---
description: Definir y modificar un filtro de columna
title: Definición y modificación de un filtro de columna | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], column
- modifying filters, column
- modifying filters
- column filters [SQL Server replication]
ms.assetid: d7c3186a-9a8c-45d8-ab34-05beec4c26dd
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e266e188719bd5ed3d84adcf81d49aa175f22872
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076763"
---
# <a name="define-and-modify-a-column-filter"></a>Definir y modificar un filtro de columna
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo definir y modificar un filtro de columna en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
-   **Para definir y modificar un filtro de columna con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Algunas columnas no se pueden filtrar. Para obtener más información, vea [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md). Si modifica un filtro de columna después de que las suscripciones se hayan inicializado, deberá generar una nueva instantánea y reinicializar todas las suscripciones después de efectuar el cambio. Para obtener más información sobre los requisitos para los cambios de propiedad, consulte [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Defina los filtros de columna en la página **Artículos** del Asistente para nueva publicación. Para obtener más información sobre cómo usar el Asistente para nueva publicación, vea [Crear una publicación](../../../relational-databases/replication/publish/create-a-publication.md).  
  
 Defina y modifique los filtros de columna en la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre cómo cambiar las propiedades de la publicación, vea [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-column-filter"></a>Para definir un filtro de columna  
  
1.  En la página **Artículos** del Asistente para nueva publicación, expanda la tabla que se va a filtrar en el panel **Objetos para publicar** .  
  
2.  Desactive la casilla situada junto a cada columna que desee filtrar.  
  
#### <a name="to-modify-column-filtering"></a>Para modificar filtros de columna  
  
1.  En la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , expanda la tabla que se va a filtrar en el panel **Objetos que se van a publicar**.  
  
2.  Desactive la casilla situada junto a cada columna que desee filtrar y asegúrese de que activa las casillas de las columnas que se deben incluir en el artículo.  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Al crear los artículos de la tabla, puede definir qué columnas desea incluir en el artículo y cambiar las columnas una vez definido el artículo. Puede crear y modificar columnas filtradas mediante programación usando los procedimientos almacenados de replicación.  
  
> [!NOTE]  
>  Los procedimientos siguientes suponen que la tabla subyacente no ha cambiado. Para obtener información sobre la replicación de los cambios del lenguaje de definición de datos (DDL) a tablas publicadas, vea [Realizar cambios de esquema en bases de datos de publicaciones](../../../relational-databases/replication/publish/make-schema-changes-on-publication-databases.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Para definir un filtro de columna para un artículo publicado en una instantánea o publicación transaccional  
  
1.  Defina el artículo que se va a filtrar. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md). Esto define las columnas que se van a incluir o quitar del artículo.  
  
    -   Si se publican solo unas columnas de una tabla con muchas columnas, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una vez para cada columna que se está agregando. Especifique el nombre de la columna en **\@column** y un valor de **add** en **\@operation**.  
  
    -   Si se publica la mayoría de las columnas en una tabla con muchas columnas, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) y especifique un valor de **null** para **\@column** y un valor de **add** en **\@operation** para agregar todas las columnas. Después, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md), una vez para cada columna que se va a excluir, y especifique un valor de **drop** para **\@operation** y el nombre de la columna excluida en **\@column**.  
  
3.  En la base de datos de publicación del publicador, ejecute [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Especifique el nombre de la publicación para **\@publication** y el nombre del artículo filtrado para **\@article**. Esto crea los objetos de sincronización para el artículo filtrado.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Para cambiar un filtro de columna para que incluya las columnas adicionales para un artículo publicado en una instantánea o publicación transaccional  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) para cada columna que se está agregando. Especifique el nombre de la columna en **\@column** y un valor de **add** en **\@operation**.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Especifique el nombre de la publicación para **\@publication** y el nombre del artículo filtrado para **\@article**. Si la publicación tiene suscripciones existentes, especifique un valor de **1** para **\@change_active**. Esto vuelve a crear los objetos de sincronización para el artículo filtrado.  
  
3.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada.  
  
4.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-snapshot-or-transactional-publication"></a>Para cambiar un filtro de columna para quitar columnas para un artículo publicado en una instantánea o publicación transaccional  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_articlecolumn](../../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) para cada columna que se está quitando. Especifique el nombre de la columna para **\@column** y un valor de **drop** para **\@operation**.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_articleview](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Especifique el nombre de la publicación para **\@publication** y el nombre del artículo filtrado para **\@article**. Si la publicación tiene suscripciones existentes, especifique un valor de **1** para **\@change_active**. Esto vuelve a crear los objetos de sincronización para el artículo filtrado.  
  
3.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada.  
  
4.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-column-filter-for-an-article-published-in-a-merge-publication"></a>Para definir un filtro de columna para un artículo publicado en una publicación de combinación  
  
1.  Defina el artículo que se va a filtrar. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md). Esto define las columnas que se van a incluir o quitar del artículo.  
  
    -   Si se publican solo unas columnas de una tabla con muchas columnas, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) una vez para cada columna que se está agregando. Especifique el nombre de la columna en **\@column** y un valor de **add** en **\@operation**.  
  
    -   Si se publica la mayoría de las columnas en una tabla con muchas columnas, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) y especifique un valor de **null** para **\@column** y un valor de **add** en **\@operation** para agregar todas las columnas. Después, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md), una vez para cada columna que se va a excluir, y especifique un valor de **drop** para **\@operation** y el nombre de la columna excluida para **\@column**.  
  
#### <a name="to-change-a-column-filter-to-include-additional-columns-for-an-article-published-in-a-merge-publication"></a>Para cambiar un filtro de columna para que incluya columnas adicionales para un artículo publicado en una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) para cada columna que se está agregando. Especifique el nombre de la columna para **\@column**, un valor de **add** para **\@operation** y un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
2.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada.  
  
3.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-change-a-column-filter-to-remove-columns-for-an-article-published-in-a-merge-publication"></a>Para cambiar un filtro de columna para quitar columnas para un artículo publicado en una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_mergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-mergearticlecolumn-transact-sql.md) para cada columna que se está quitando. Especifique el nombre de la columna para **\@column**, un valor de **drop** para **\@operation** y un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
2.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada.  
  
3.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En este ejemplo de replicación transaccional, la columna `DaysToManufacture` se quita de un artículo basado en la tabla `Product` .  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_1.sql)]  
  
 En este ejemplo de replicación de mezcla, la columna `CreditCardApprovalCode` se quita de un artículo basado en la tabla `SalesOrderHeader` .  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-colu_2.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrar datos publicados para la replicación de mezcla](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
