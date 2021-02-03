---
title: Reinicialización de una suscripción | Microsoft Docs
description: Obtenga información sobre cómo reinicializar una suscripción en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- initializing subscriptions [SQL Server replication], reinitializing
- subscriptions [SQL Server replication], reinitializing
- reinitializing subscriptions
ms.assetid: ca3625c5-c62e-4ab7-9829-d511f838e385
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 03d97ad323c5e83e7cd0a5de0b09da6bad6b1a22
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250749"
---
# <a name="reinitialize-a-subscription"></a>Reinicializar una suscripción
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo reinicializar una suscripción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO). Las suscripciones individuales se pueden marcar para reinicialización de manera que se aplique una nueva instantánea durante la siguiente sincronización.  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]


  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Reinicializar una suscripción es un proceso con dos partes:  
  
1.  Una o todas las suscripciones a una publicación se *marcan* para reinicializarse. Las suscripciones se marcan para reinicialización en el cuadro de diálogo **Reinicializar suscripciones**, disponible en la carpeta **Publicaciones locales** y en la carpeta **Suscripciones locales** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. También puede marcar suscripciones desde la pestaña **Todas las suscripciones** y el nodo de publicaciones del Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md). Al marcar una suscripción para reinicialización, tiene las siguientes opciones:  
  
     **Utilizar la instantánea actual**  
     Seleccione esta opción para aplicar la instantánea actual al suscriptor la próxima vez que se ejecute el Agente de distribución o el Agente de mezcla. Si no hay ninguna instantánea válida disponible, esta opción no puede seleccionarse.  
  
     **Utilizar una instantánea nueva**  
     Seleccione esta opción para reinicializar la suscripción con una instantánea nueva. La instantánea solo puede aplicarse al suscriptor después de que la haya generado el Agente de instantáneas. Si el Agente de instantáneas está configurado para ejecutarse de acuerdo con una programación, la suscripción no se reinicializará hasta después de la siguiente ejecución programada del Agente de instantáneas. Seleccione **Generar la nueva instantánea ahora** para iniciar el Agente de instantáneas de forma inmediata.  
  
     **Cargar los cambios no sincronizados antes de reinicializar**  
     Solo replicación de mezcla. Seleccione esta opción para cargar cambios pendientes de la base de datos de suscripciones antes de que se sobrescriba el suscriptor con una instantánea.  
  
     Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
2.  Una suscripción se reinicializa la siguiente vez que se sincroniza: el Agente de distribución (para la replicación transaccional) o el Agente de mezcla (para la replicación de mezcla) aplica la instantánea más reciente a cada suscriptor que tiene una suscripción marcada para reinicialización. Para obtener más información acerca de cómo sincronizar suscripciones, vea [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md) y [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-management-studio-at-the-publisher"></a>Para marcar una sola suscripción de extracción o de inserción para reinicializarla en Management Studio (en el publicador)  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación que tiene la suscripción que desea reinicializar.  
  
4.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Reinicializar**.  
  
5.  En el cuadro de diálogo **Reinicializar suscripciones** , seleccione las opciones y, a continuación, haga clic en **Marcar para reinicializar**.  
  
#### <a name="to-mark-a-single-pull-subscription-for-reinitialization-in-management-studio-at-the-subscriber"></a>Para marcar una sola suscripción de extracción para reinicializarla en Management Studio (en el suscriptor)  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Suscripciones locales**.  
  
3.  Haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Reinicializar**.  
  
4.  En el cuadro de diálogo de confirmación que se muestra, haga clic en **Sí**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-management-studio"></a>Para marcar todas las suscripciones para reinicializarlas en Management Studio  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Haga clic con el botón secundario en la publicación que tiene las suscripciones que desea reinicializar y, a continuación, haga clic en **Reinicializar todas las suscripciones**.  
  
4.  En el cuadro de diálogo **Reinicializar suscripciones** , seleccione las opciones y, a continuación, haga clic en **Marcar para reinicializar**.  
  
#### <a name="to-mark-a-single-push-or-pull-subscription-for-reinitialization-in-replication-monitor"></a>Para marcar una sola suscripción de inserción o de extracción para reinicializarla en el Monitor de replicación  
  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Todas las suscripciones** .  
  
3.  Haga clic con el botón secundario en la suscripción que desea reinicializar y, a continuación, haga clic en **Reinicializar suscripción**.  
  
4.  En el cuadro de diálogo **Reinicializar suscripciones** , seleccione las opciones y, a continuación, haga clic en **Marcar para reinicializar**.  
  
#### <a name="to-mark-all-subscriptions-for-reinitialization-in-replication-monitor"></a>Para marcar todas las suscripciones para reinicializarlas en el Monitor de replicación  
  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo y, a continuación, expanda un publicador.  
  
2.  Haga clic con el botón secundario en la publicación que tiene las suscripciones que desea reinicializar y, a continuación, haga clic en **Reinicializar todas las suscripciones**.  
  
3.  En el cuadro de diálogo **Reinicializar suscripciones** , seleccione las opciones y, a continuación, haga clic en **Marcar para reinicializar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las suscripciones pueden reinicializarse mediante programación con procedimientos almacenados de replicación. El procedimiento almacenado que se usa depende del tipo de suscripción (inserción o extracción) y el tipo de publicación a la que pertenece la suscripción.  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Para reinicializar una suscripción de extracción a una publicación transaccional  
  
1.  En la base de datos de suscripciones del suscriptor, ejecute [sp_reinitpullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitpullsubscription-transact-sql.md). Especifique **\@publisher**, **\@publisher_db** y **\@publication**. Esto marca la suscripción para reinicializarla la próxima vez que se ejecute el Agente de distribución.  
  
2.  (Opcional) Inicie el Agente de distribución en el suscriptor para sincronizar la suscripción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Para reinicializar una suscripción de inserción a una publicación transaccional  
  
1.  En el publicador, ejecute [sp_reinitsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitsubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber** y **\@destination_db**. Esto marca la suscripción para reinicializarla la próxima vez que se ejecute el Agente de distribución.  
  
2.  (Opcional) Inicie el Agente de distribución en el distribuidor para sincronizar la suscripción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Para reinicializar una suscripción de extracción a una publicación de combinación  
  
1.  En la base de datos de suscripciones del suscriptor, ejecute [sp_reinitmergepullsubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergepullsubscription-transact-sql.md). Especifique **\@publisher**, **\@publisher_db** y **\@publication**. Para cargar los cambios del Suscriptor antes de que se produzca la reinicialización, especifique un valor de **true** para **\@upload_first**. Esto marca la suscripción para reinicializarla la próxima vez que se ejecute el Agente de mezcla.  
  
    > [!IMPORTANT]  
    >  Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
2.  (Opcional) Inicie el Agente de mezcla en el suscriptor para sincronizar la suscripción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Para reinicializar una suscripción de inserción a una publicación de combinación  
  
1.  En el publicador, ejecute [sp_reinitmergesubscription &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-reinitmergesubscription-transact-sql.md). Especifique **\@publication**, **\@subscriber** y **\@subscriber_db**. Para cargar los cambios del Suscriptor antes de que se produzca la reinicialización, especifique un valor de **true** para **\@upload_first**. Esto marca la suscripción para reinicializarla la próxima vez que se ejecute el Agente de distribución.  
  
    > [!IMPORTANT]  
    >  Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
2.  (Opcional) Inicie el Agente de mezcla en el distribuidor para sincronizar la suscripción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-set-the-reinitialization-policy-when-creating-a-new-merge-publication"></a>Para establecer la directiva de reinicialización al crear una nueva publicación de combinación  
  
1.  En el publicador de la base de datos de publicación, ejecute [sp_addmergepublication](../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md), y especifique uno de los valores siguientes para **\@automatic_reinitialization_policy**:  
  
    -   **1** : los cambios se cargan en el suscriptor antes de que una suscripción se reinicialice automáticamente según lo requiera un cambio a la publicación.  
  
    -   **0** : los cambios en el suscriptor se descartan cuando una suscripción se reinicializa automáticamente según lo requiera un cambio a la publicación.  
  
    > [!IMPORTANT]  
    >  Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
     Para obtener más información, vea [Crear una suscripción](../../relational-databases/replication/publish/create-a-publication.md).  
  
#### <a name="to-change-the-reinitialization-policy-for-an-existing-merge-publication"></a>Para cambiar la directiva de reinicialización para una publicación de combinación existente  
  
1.  En el publicador de la base de datos de publicación, ejecute [sp_changemergepublication](../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md), y especifique **automatic_reinitialization_policy** para **\@property** y uno de los valores siguientes para **\@value**:  
  
    -   **1** : los cambios se cargan en el suscriptor antes de que una suscripción se reinicialice automáticamente según lo requiera un cambio a la publicación.  
  
    -   **0** : los cambios en el suscriptor se descartan cuando una suscripción se reinicializa automáticamente según lo requiera un cambio a la publicación.  
  
    > [!IMPORTANT]  
    >  Si se agrega, quita o modifica un filtro con parámetros, los cambios pendientes en el suscriptor no se pueden cargar en el publicador durante la reinicialización. Si desea cargar los cambios pendientes, sincronice todas las suscripciones antes de cambiar el filtro.  
  
     Para más información, consulte [View and Modify Publication Properties](../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Las suscripciones individuales se pueden marcar para reinicialización de manera que, durante la siguiente sincronización, se aplique una nueva instantánea. Las suscripciones se pueden reinicializar mediante programación usando Replication Management Objects (RMO). Las clases RMO que usa dependen del tipo de publicación a la que pertenece la suscripción y del tipo de suscripción (es decir, una suscripción de inserción o de extracción).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-transactional-publication"></a>Para reinicializar una suscripción de extracción a una publicación transaccional  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPullSubscription> y establezca <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>y la conexión del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto.  
  
    > [!NOTE]  
    >  Si este método devuelve **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o que la suscripción de extracción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.TransPullSubscription.Reinitialize%2A> . Este método marca la suscripción para la reinicialización.  
  
5.  Sincronice la suscripción de extracción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-transactional-publication"></a>Para reinicializar una suscripción de inserción a una publicación transaccional  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription> y establezca <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>y la conexión del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto.  
  
    > [!NOTE]  
    >  Si este método devuelve **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o que la suscripción de inserción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.TransSubscription.Reinitialize%2A> . Este método marca la suscripción para la reinicialización.  
  
5.  Sincronice la suscripción de inserción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
#### <a name="to-reinitialize-a-pull-subscription-to-a-merge-publication"></a>Para reinicializar una suscripción de extracción a una publicación de combinación  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePullSubscription> y establezca <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A>y la conexión del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto.  
  
    > [!NOTE]  
    >  Si este método devuelve **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o que la suscripción de extracción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.MergePullSubscription.Reinitialize%2A> . Pase un valor de **true** para cargar cambios en el Suscriptor antes de la reinicialización o un valor de **false** para reinicializar y perder cualquier cambio pendiente en el Suscriptor. Este método marca la suscripción para la reinicialización.  
  
    > [!NOTE]  
    >  No se pueden cargar los cambios si expira la suscripción. Para más información, consulte [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Sincronice la suscripción de extracción. Para obtener más información, consulte [Synchronize a Pull Subscription](../../relational-databases/replication/synchronize-a-pull-subscription.md).  
  
#### <a name="to-reinitialize-a-push-subscription-to-a-merge-publication"></a>Para reinicializar una suscripción de inserción a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription> y establezca <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>, <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>y la conexión del paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto.  
  
    > [!NOTE]  
    >  Si este método devuelve **false**, significa que las propiedades de suscripción del paso 2 se definieron incorrectamente o que la suscripción de inserción no existe.  
  
4.  Llame al método <xref:Microsoft.SqlServer.Replication.MergeSubscription.Reinitialize%2A> . Pase un valor de **true** para cargar cambios en el Suscriptor antes de la reinicialización o un valor de **false** para reinicializar y perder cualquier cambio pendiente en el Suscriptor. Este método marca la suscripción para la reinicialización.  
  
    > [!NOTE]  
    >  No se pueden cargar los cambios si expira la suscripción. Para más información, consulte [Set the Expiration Period for Subscriptions](../../relational-databases/replication/publish/set-the-expiration-period-for-subscriptions.md).  
  
5.  Sincronice la suscripción de inserción. Para obtener más información, consulte [Synchronize a Push Subscription](../../relational-databases/replication/synchronize-a-push-subscription.md).  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 En este ejemplo reinicializa una suscripción de extracción para una publicación transaccional.  
  
 [!code-cs[HowTo#rmo_ReinitTranPullSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinittranpullsub)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitTranPullSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinittranpullsub)]  
  
 Este ejemplo reinicializa una suscripción de extracción a una publicación de combinación después de cargar primero los cambios pendientes en el Suscriptor.  
  
 [!code-cs[HowTo#rmo_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_reinitmergepullsub_withupload)]  
  
 [!code-vb[HowTo#rmo_vb_ReinitMergePullSub_WithUpload](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_reinitmergepullsub_withupload)]  
  
## <a name="see-also"></a>Consulte también  
 [Reinicializar suscripciones](../../relational-databases/replication/reinitialize-subscriptions.md)   
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
