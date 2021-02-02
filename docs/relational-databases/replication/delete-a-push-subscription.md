---
title: Eliminación de una suscripción de inserción | Microsoft Docs
description: Obtenga información sobre cómo eliminar una suscripción de inserción en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing subscriptions
- push subscriptions [SQL Server replication], deleting
- deleting subscriptions
- subscriptions [SQL Server replication], push
ms.assetid: 3c4847e2-aed9-4488-b45d-8164422bdb10
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 958919536049d4a2c023ee7b00a02f54daebed00
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076407"
---
# <a name="delete-a-push-subscription"></a>Eliminar una suscripción de inserción
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo eliminar una suscripción de inserción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Para eliminar una suscripción de inserción con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Elimine una suscripción de inserción en el publicador (desde la carpeta **Publicaciones locales** en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]) o el suscriptor (desde la carpeta **Suscripciones locales** ). Al eliminar una suscripción no se quitan los objetos ni los datos de la suscripción, y deben quitarse manualmente.  
  
#### <a name="to-delete-a-push-subscription-at-the-publisher"></a>Para eliminar una suscripción de inserción en el publicador  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación asociada con la suscripción que desea eliminar.  
  
4.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Eliminar**.  
  
5.  En el cuadro de diálogo de confirmación, seleccione si desea conectarse al suscriptor para eliminar la información de suscripciones. Si desactiva la casilla **Conectar a suscriptor** , debe conectar al suscriptor posteriormente para eliminar la información.  

#### <a name="to-delete-a-push-subscription-at-the-subscriber"></a>Para eliminar una suscripción de inserción en el suscriptor  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Suscripciones locales**.  
  
3.  Haga clic con el botón secundario en la suscripción que desea eliminar y, a continuación, haga clic en **Eliminar**.  
  
4.  En el cuadro de diálogo de confirmación, seleccione si desea conectarse al publicador para eliminar la información de suscripciones. Si desactiva la casilla **Conectar al publicador** , deberá conectarse al publicador más adelante para eliminar la información.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las suscripciones de inserción pueden eliminarse mediante programación con procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que corresponda la suscripción.  
  
#### <a name="to-delete-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para eliminar una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  (Opcional) en la base de datos de publicación del publicador, ejecute [sp_dropsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropsubscription-transact-sql.md). Especifique **\@publication** y **\@subscriber**. Especifique un valor de **all** para **\@article**. (Opcional) Si no se puede acceder al Distribuidor, especifique un valor de **1** para **\@ignore_distributor** con el fin de eliminar la suscripción sin quitar los objetos relacionados en el Distribuidor.  
  
2.  (Opcional) En la base de datos de suscripciones del suscriptor, ejecute [sp_subscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) para quitar los metadatos de replicación restantes en la base de datos de suscripciones.  
  
#### <a name="to-delete-a-push-subscription-to-a-merge-publication"></a>Para eliminar una suscripción de inserción a una publicación de combinación  
  
1.  En el publicador, ejecute [sp_dropmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-dropmergesubscription-transact-sql.md) y especifique **\@publication**, **\@subscriber** y **\@subscriber_db**. (Opcional) Si no se puede acceder al Distribuidor, especifique un valor de **1** para **\@ignore_distributor** con el fin de eliminar la suscripción sin quitar los objetos relacionados en el Distribuidor.  
  
2.  En la base de datos de suscripciones del suscriptor, ejecute [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md). Especifique **\@publisher**, **\@publisher_db** y **\@publication**. Esto quita los metadatos de mezcla de la base de datos de suscripciones.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En este ejemplo se elimina una suscripción de inserción a una publicación transaccional.  
  
 [!code-sql[HowTo#sp_droptransubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-push-subscription_1.sql)]  
  
 En este ejemplo se elimina una suscripción de inserción a una publicación de combinación.  
  
 [!code-sql[HowTo#sp_dropmergesubscription](../../relational-databases/replication/codesnippet/tsql/delete-a-push-subscription_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Las clases RMO que se usan para eliminar una suscripción de inserción dependen del tipo de publicación a la se suscribe dicha suscripción.  
  
#### <a name="to-delete-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para eliminar una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A> .  
  
4.  Establezca la conexión <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la suscripción existe. Si el valor de esta propiedad es **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o bien, que la suscripción no existe.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Subscription.Remove%2A> .  
  
#### <a name="to-delete-a-push-subscription-to-a-merge-publication"></a>Para eliminar una suscripción de inserción a una publicación de combinación  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A> .  
  
4.  Establezca la conexión <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la suscripción existe. Si el valor de esta propiedad es **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o bien, que la suscripción no existe.  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Subscription.Remove%2A> .  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 Las suscripciones de inserción se pueden eliminar mediante programación utilizando Replication Management Objects (RMO).  
  
 [!code-cs[HowTo#rmo_DropTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpushsub)]  
  
## <a name="see-also"></a>Consulte también  
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
