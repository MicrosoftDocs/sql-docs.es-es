---
description: Eliminar una suscripción de extracción
title: Eliminación de una suscripción de extracción | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing subscriptions
- deleting subscriptions
- pull subscriptions [SQL Server replication], deleting
- subscriptions [SQL Server replication], pull
ms.assetid: 997c0b8e-d8d9-4eed-85b1-6baa1f8594ce
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 1da4312a4eb0f25b82b820ef9004bb9c64e5fff7
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076397"
---
# <a name="delete-a-pull-subscription"></a>Eliminar una suscripción de extracción
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo eliminar una suscripción de extracción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Para eliminar una suscripción de extracción con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Elimine una suscripción de extracción en el publicador (desde la carpeta **Publicaciones locales** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]) o en el suscriptor (desde la carpeta **Suscripciones locales** ). Al eliminar una suscripción no se quitan los objetos ni los datos de la suscripción, y deben quitarse manualmente.  
  
#### <a name="to-delete-a-pull-subscription-at-the-publisher"></a>Para eliminar una suscripción de extracción en el publicador  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación asociada con la suscripción que desea eliminar.  
  
4.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Eliminar**.  
  
5.  En el cuadro de diálogo de confirmación, seleccione si desea conectarse al suscriptor para eliminar la información de suscripciones. Si desactiva la casilla **Conectar a suscriptor** , deberá conectarse al suscriptor más adelante para eliminar la información.  
  
#### <a name="to-delete-a-pull-subscription-at-the-subscriber"></a>Para eliminar una suscripción de extracción en el suscriptor  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Suscripciones locales**.  
  
3.  Haga clic con el botón secundario en la suscripción que desea eliminar y, a continuación, haga clic en **Eliminar**.  
  
4.  En el cuadro de diálogo de confirmación, seleccione si desea conectarse al publicador para eliminar la información de suscripciones. Si desactiva la casilla **Conectar al publicador** , deberá conectarse al publicador más adelante para eliminar la información.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las suscripciones de extracción pueden eliminarse mediante programación con procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que corresponda la suscripción.  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Para eliminar una suscripción de extracción a una publicación transaccional o de instantáneas  
  
1.  En la base de datos de suscripciones del suscriptor, ejecute [sp_droppullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droppullsubscription-transact-sql.md). Especifique **\@publication**, **\@publisher** y **\@publisher_db**.  
  
2.  (Opcional) en la base de datos de publicación del publicador, ejecute [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). Especifique **\@publication** y **\@subscriber**. Especifique un valor de **all** para **\@article**. (Opcional) Si no se puede acceder al Distribuidor, especifique un valor de **1** para **\@ignore_distributor** con el fin de eliminar la suscripción sin quitar los objetos relacionados en el Distribuidor.  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>Para eliminar una suscripción de extracción a una publicación de combinación  
  
1.  En la base de datos de suscripciones del suscriptor, ejecute [sp_dropmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergepullsubscription-transact-sql.md). Especifique **\@publication**, **\@publisher** y **\@publisher_db**.  
  
2.  En el publicador de la base de datos de publicaciones, ejecute [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber** y **\@subscriber_db**. Especifique un valor **pull** para **\@subscription_type**. (Opcional) Si no se puede acceder al Distribuidor, especifique un valor de **1** para **\@ignore_distributor** con el fin de eliminar la suscripción sin quitar los objetos relacionados en el Distribuidor.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 El siguiente ejemplo elimina una suscripción de extracción en una publicación transaccional. El primer lote se ejecuta en el Suscriptor y el segundo lote se ejecuta en el Publicador.  
  
 [!code-sql[HowTo#sp_droptranpullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_1.sql)]  
  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_2.sql)]  
  
 El siguiente ejemplo elimina una suscripción de extracción en una publicación de combinación. El primer lote se ejecuta en el Suscriptor y el segundo lote se ejecuta en el Publicador.  
  
 [!code-sql[HowTo#sp_dropmergepullsubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_3.sql)]  
  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-pull-subscription_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Las suscripciones de extracción se pueden eliminar mediante programación utilizando Replication Management Objects (RMO). Las clases RMO que se usan para eliminar una suscripción de extracción dependen del tipo de publicación a la se suscribe dicha suscripción.  
  
#### <a name="to-delete-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Para eliminar una suscripción de extracción a una publicación transaccional o de instantáneas  
  
1.  Cree conexiones tanto al Suscriptor como al Publicador con la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPullSubscription> y establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> . Use la conexión del Suscriptor del paso 1 para establecer la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
3.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la suscripción existe. Si el valor de esta propiedad es **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o bien, que la suscripción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> .  
  
5.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication> con la conexión de publicador del paso 1. Especifique <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> y <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, las propiedades especificadas en el paso 5 son incorrectas o la publicación no existe en el servidor.  
  
7.  Llame al método <xref:Microsoft.SqlServer.Replication.TransPublication.RemovePullSubscription%2A> . Especifique el nombre del Suscriptor y la base de datos de suscripciones para los parámetros *subscriber* y *subscriberDB* .  
  
#### <a name="to-delete-a-pull-subscription-to-a-merge-publication"></a>Para eliminar una suscripción de extracción a una publicación de combinación  
  
1.  Cree conexiones tanto al Suscriptor como al Publicador con la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePullSubscription> y establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> . Use la conexión del paso 1 para establecer la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
3.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la suscripción existe. Si el valor de esta propiedad es **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o bien, que la suscripción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.PullSubscription.Remove%2A> .  
  
5.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> con la conexión de publicador del paso 1. Especifique <xref:Microsoft.SqlServer.Replication.Publication.Name%2A>, <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> y <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, las propiedades especificadas en el paso 5 son incorrectas o la publicación no existe en el servidor.  
  
7.  Llame al método <xref:Microsoft.SqlServer.Replication.MergePublication.RemovePullSubscription%2A> . Especifique el nombre del Suscriptor y la base de datos de suscripciones para los parámetros *subscriber* y *subscriberDB* .  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 Este ejemplo elimina una suscripción de extracción a una publicación transaccional y quita el registro de suscripciones en el Publicador.  
  
 [!code-cs[HowTo#rmo_DropTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpullsub)]  
  
 Este ejemplo elimina una suscripción de extracción a una publicación de combinación y quita el registro de suscripciones en el Publicador.  
  
 [!code-cs[HowTo#rmo_DropMergePullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepullsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepullsub)]  
  
## <a name="see-also"></a>Consulte también  
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
