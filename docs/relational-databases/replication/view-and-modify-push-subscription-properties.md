---
description: Ver y modificar las propiedades de una suscripción de inserción
title: Ver y modificar las propiedades de una suscripción de inserción | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- viewing replication properties
- push subscriptions [SQL Server replication], properties
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], modifying
- modifying replication properties, push subscriptions
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 801d2995-7aa5-4626-906e-c8190758ec71
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 28abdcd83484be06b135252da8bc9af53b98448b
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250552"
---
# <a name="view-and-modify-push-subscription-properties"></a>Ver y modificar las propiedades de una suscripción de inserción
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo ver y modificar las propiedades de una suscripción de inserción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Vea y modifique las propiedades de suscripción de inserción del publicador en:  
  
-   Cuadro de diálogo **Propiedades de suscripción: \<Publisher>: \<PublicationDatabase>** , que está disponible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
-   La pestaña **Todas las suscripciones** en el Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-management-studio"></a>Ver y modificar las propiedades de suscripción de inserción en Management Studio  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación apropiada, haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  Modifique las propiedades si es necesario y, a continuación, haga clic en **Aceptar**.  
  
#### <a name="to-view-and-modify-push-subscription-properties-in-replication-monitor"></a>Para ver y modificar las propiedades de suscripción de inserción en el Monitor de replicación  
  
1.  Expanda un grupo de publicador en el panel izquierdo del Monitor de replicación, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Todas las suscripciones** .  
  
3.  Haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  Modifique las propiedades si es necesario y, a continuación, haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Se puede modificar las suscripciones de inserción y tener acceso a sus propiedades mediante programación utilizando procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que corresponda la suscripción.  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para ver las propiedades de una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber** y el valor **all** para **\@article**.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), especificando **\@subscriber**.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para modificar las propiedades de una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_changesubscriber](../../relational-databases/system-stored-procedures/sp-changesubscriber-transact-sql.md), y especifique **\@subscriber** y los parámetros de las propiedades del suscriptor que se vayan a cambiar.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_changesubscription](../../relational-databases/system-stored-procedures/sp-changesubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber**, **\@destination_db**, el valor **all** para **\@article**, la propiedad de suscripción que se va a cambiar como **\@property** y el nuevo valor como **\@value**. Esto cambia la configuración de seguridad para la suscripción de inserción.  
  
3.  (Opcional) Para cambiar las propiedades del paquete de Servicios de transformación de datos (DTS) de una suscripción, ejecute [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md) en la base de datos de suscripciones del suscriptor. Especifique el identificador del trabajo del Agente de distribución para **\@jobid** y las siguientes propiedades del paquete DTS:  
  
    -   **\@dts_package_name**  
  
    -   **\@dts_package_password**  
  
    -   **\@dts_package_location**  
  
     De esta forma se cambian las propiedades del paquete DTS de una suscripción.  
  
    > [!NOTE]  
    >  El Id del trabajo se puede obtener ejecutando [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Para ver o las propiedades de una suscripción de inserción a una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md). Especifique **\@publication** y **\@subscriber**.  
  
2.  En el publicador, ejecute [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md) y especifique **\@subscriber**.  
  
#### <a name="to-change-the-properties-of-a-push-subscription-to-a-merge-publication"></a>Para cambiar las propiedades de una suscripción de inserción a una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_changemergesubscription](../../relational-databases/system-stored-procedures/sp-changemergesubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber**, **\@subscriber_db**, la propiedad de suscripción que se va a cambiar como **\@property** y el nuevo valor como **\@value**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Las clases RMO que usa para ver o modificar las propiedades de suscripción de inserción dependen del tipo de publicación a la que se suscribe la suscripción de inserción.  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para ver o modificar propiedades de una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Establezca la conexión <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.TransSubscription> que se puedan establecer y, a continuación, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Opcional) Para ver los nuevos valores, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> para recargar las propiedades de la suscripción.  
  
#### <a name="to-view-or-modify-properties-of-a-push-subscription-to-a-merge-publication"></a>Para ver o modificar las propiedades de una suscripción de inserción a una publicación de combinación  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>y <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A> .  
  
4.  Establezca la conexión <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de suscripción del paso 3 se definieron incorrectamente, o bien que la suscripción no existe.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.MergeSubscription> que se puedan establecer y, a continuación, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Opcional) Para ver los nuevos valores, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> para recargar las propiedades de la suscripción.  
  
## <a name="see-also"></a>Consulte también  
 [Visualización de información y realización de tareas mediante el Monitor de replicación](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
