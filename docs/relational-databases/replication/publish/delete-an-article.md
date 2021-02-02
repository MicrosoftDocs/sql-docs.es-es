---
description: Eliminar un artículo
title: Eliminación de una artículo | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- articles [SQL Server replication], dropping
- sp_droparticle
- sp_dropmergearticle
- deleting articles
- removing articles
- dropping articles
ms.assetid: 185b58fc-38c0-4abe-822e-6ec20066c863
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 12ec412078e893c8c527f977d35eecebf06f8f93
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076713"
---
# <a name="delete-an-article"></a>Eliminar un artículo
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo eliminar un artículo en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[tsql](../../../includes/tsql-md.md)] o Replication Management Objects (RMO). Para obtener información sobre las condiciones en que se pueden quitar artículos, y si la eliminación de un artículo requiere una nueva instantánea o la reinicialización de las suscripciones, vea [Agregar y quitar artículos de publicaciones existentes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
 **En este tema**  
  
-   **Para eliminar un artículo, mediante:**  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Los artículos pueden eliminarse mediante programación con procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que pertenece el artículo.  
  
#### <a name="to-delete-an-article-from-a-snapshot-or-transactional-publication"></a>Para eliminar un artículo de una publicación de instantáneas o transaccional  
  
1.  Ejecute [sp_droparticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md) para eliminar un artículo, especificado por **\@article**, de una publicación, especificada por **\@publication**. Especifique un valor de **1** para **\@force_invalidate_snapshot**.  
  
2.  (Opcional) Para quitar completamente el objeto publicado de la base de datos, ejecute el comando `DROP <objectname>` en el publicador de la base de datos de publicación.  

#### <a name="to-delete-an-article-from-a-merge-publication"></a>Para eliminar un artículo de una publicación de combinación  
  
1.  Ejecute [sp_dropmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergearticle-transact-sql.md) para eliminar un artículo, especificado por **\@article**, de una publicación, especificada por **\@publication**. Si es necesario, especifique un valor de **1** para **\@force_invalidate_snapshot** y un valor de **1** para **\@force_reinit_subscription**.  
  
2.  (Opcional) Para quitar completamente el objeto publicado de la base de datos, ejecute el comando `DROP <objectname>` en el publicador de la base de datos de publicación.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En el siguiente ejemplo se elimina un artículo de una publicación transaccional Como este cambio invalida la instantánea existente, se especifica un valor de **1** para el parámetro **\@force_invalidate_snapshot**.  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article AS sysname;  
SET @publication = N'AdvWorksProductTran';   
SET @article = N'Product';   
  
-- Drop the transactional article.  
USE [AdventureWorks]  
EXEC sp_droparticle   
  @publication = @publication,   
  @article = @article,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
 El ejemplo siguiente elimina dos artículos de una publicación de combinación. Como estos cambios invalidan la instantánea existente, se especifica un valor de **1** para el parámetro **\@force_invalidate_snapshot**.  
  
```  
DECLARE @publication AS sysname;  
DECLARE @article1 AS sysname;  
DECLARE @article2 AS sysname;  
SET @publication = N'AdvWorksSalesOrdersMerge';  
SET @article1 = N'SalesOrderDetail';   
SET @article2 = N'SalesOrderHeader';   
  
-- Remove articles from a merge publication.  
USE [AdventureWorks]  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article1,  
  @force_invalidate_snapshot = 1;  
EXEC sp_dropmergearticle   
  @publication = @publication,   
  @article = @article2,  
  @force_invalidate_snapshot = 1;  
GO  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede eliminar artículos mediante programación utilizando Replication Management Objects (RMO). Las clases RMO que usa para eliminar un artículo dependen del tipo de publicación al que pertenece el artículo.  
  
#### <a name="to-delete-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>Para eliminar un artículo que pertenece a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransArticle>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>y <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que el artículo existe. Si el valor de esta propiedad es **false**, se definieron incorrectamente las propiedades de artículo en el paso 3, o bien el artículo no existe.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> .  
  
7.  Cierre todas las conexiones.  
  
#### <a name="to-delete-an-article-that-belongs-to-a-merge-publication"></a>Para eliminar un artículo que pertenece a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>y <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que el artículo existe. Si el valor de esta propiedad es **false**, se definieron incorrectamente las propiedades de artículo en el paso 3, o bien el artículo no existe.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Article.Remove%2A> .  
  
7.  Cierre todas las conexiones.  
  
## <a name="see-also"></a>Consulte también  
 [Agregar y quitar artículos de publicaciones existentes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md)   
 [Conceptos de procedimientos almacenados del sistema de replicación](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)  
  
  
