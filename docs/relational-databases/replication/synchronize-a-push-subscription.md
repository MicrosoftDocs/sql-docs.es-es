---
title: Sincronizar una suscripción de inserción | Microsoft Docs
description: Obtenga información sobre cómo sincronizar una suscripción de inserción en SQL Server mediante SQL Server Management Studio, agentes de replicación o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- synchronization [SQL Server replication], push subscriptions
- subscriptions [SQL Server replication], push
- push subscriptions [SQL Server replication], synchronizing
ms.assetid: 0cfa7ae5-91d3-4a4f-9edf-a852d45783b5
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 14240f40dcd00f439c0aa5f368ebe4f331641414
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250576"
---
# <a name="synchronize-a-push-subscription"></a>Sincronizar una suscripción de inserción
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  En este tema se describe cómo sincronizar una suscripción de inserción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [agentes de replicación](../../relational-databases/replication/agents/replication-agents-overview.md)o Replication Management Objects (RMO).  
  
[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]

##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 El Agente de distribución (para las instantáneas y la replicación transaccional) o el Agente de mezcla (para la replicación de mezcla) sincronizan las suscripciones. Los agentes pueden ejecutarse continuamente, a petición o según una programación. Para más información sobre la configuración de las programaciones de sincronización, vea [Especificar programaciones de sincronización](../../relational-databases/replication/specify-synchronization-schedules.md).  
  
 Sincronice una suscripción a petición de las carpetas **Publicaciones locales** y **Suscripciones locales** de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y la pestaña **Todas las suscripciones** del Monitor de replicación. Las suscripciones a publicaciones de Oracle no se pueden sincronizar a petición desde el suscriptor. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-publisher"></a>Para sincronizar una suscripción de inserción a petición en Management Studio (en el publicador)  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación en la que desea sincronizar las suscripciones.  
  
4.  Haga clic con el botón derecho en la suscripción que desea sincronizar y, a continuación, haga clic en **Ver estado de sincronización**.  
  
5.  En el cuadro de diálogo **Ver estado de sincronización - \<Subscriber>:\<SubscriptionDatabase>** haga clic en **Iniciar**. Cuando se completa la sincronización, se muestra el mensaje **Sincronización completada**.  
  
6.  Haga clic en **Cerrar**.  

#### <a name="to-synchronize-a-push-subscription-on-demand-in-management-studio-at-the-subscriber"></a>Para sincronizar una suscripción de inserción a petición en Management Studio (en el suscriptor)  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Suscripciones locales**.  
  
3.  Haga clic con el botón derecho en la suscripción que desea sincronizar y, a continuación, haga clic en **Ver estado de sincronización**.  
  
4.  Se muestra un mensaje acerca del establecimiento de una conexión con el distribuidor. Haga clic en **OK**.  
  
5.  En el cuadro de diálogo **Ver estado de sincronización - \<Subscriber>:\<SubscriptionDatabase>** haga clic en **Iniciar**. Cuando se completa la sincronización, se muestra el mensaje **Sincronización completada**.  
  
6.  Haga clic en **Cerrar**.  
  
#### <a name="to-synchronize-a-push-subscription-on-demand-in-replication-monitor"></a>Para sincronizar una suscripción de inserción a petición en el Monitor de replicación  
  
1.  En el Monitor de replicación, expanda un grupo de publicador en el panel izquierdo, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Todas las suscripciones** .  
  
3.  Haga clic con el botón secundario en la suscripción que desea sincronizar y, a continuación, haga clic en **Iniciar sincronización**.  
  
4.  Para ver el progreso de la sincronización, haga clic con el botón secundario en la suscripción y, a continuación, haga clic en **Ver detalles**.  
  
##  <a name="using-replication-agents"></a><a name="ReplProg"></a> Usar agentes de replicación  
 Se pueden sincronizar las suscripciones de inserción mediante programación y a petición invocando el archivo ejecutable de agente de replicación adecuado del símbolo del sistema. El archivo ejecutable de agente de replicación que se invoca dependerá del tipo de publicación a la que pertenece la suscripción de inserción.  
  
#### <a name="to-start-the-distribution-agent-to-synchronize-a-push-subscription-to-a-transactional-publication"></a>Para iniciar el Agente de distribución para sincronizar una suscripción de inserción con una publicación transaccional  
  
1.  Desde el símbolo del sistema o en un archivo por lotes, ejecute **distrib.exe**. Especifique los argumentos de la línea de comandos siguientes:  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     Si está usando la autenticación de SQL Server, también debe especificar los argumentos siguientes:  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
#### <a name="to-start-the-merge-agent-to-synchronize-a-push-subscription-to-a-merge-publication"></a>Para iniciar el Agente de mezcla para sincronizar una suscripción de inserción con una publicación de combinación  
  
1.  Desde el símbolo del sistema o en un archivo por lotes, ejecute **replmerg.exe**. Especifique los argumentos de la línea de comandos siguientes:  
  
    -   **-Publisher**  
  
    -   **-PublisherDB**  
  
    -   **-Publication**  
  
    -   **-Distributor**  
  
    -   **-Subscriber**  
  
    -   **-SubscriberDB**  
  
    -   **-SubscriptionType = 0**  
  
     Si está usando la autenticación de SQL Server, también debe especificar los argumentos siguientes:  
  
    -   **-DistributorLogin**  
  
    -   **-DistributorPassword**  
  
    -   **-DistributorSecurityMode = 0**  
  
    -   **-PublisherLogin**  
  
    -   **-PublisherPassword**  
  
    -   **-PublisherSecurityMode = 0**  
  
    -   **-SubscriberLogin**  
  
    -   **-SubscriberPassword**  
  
    -   **-SubscriberSecurityMode = 0**  
  
        > [!IMPORTANT]  
        >  [!INCLUDE[ssNoteWinAuthentication](../../includes/ssnotewinauthentication-md.md)]  
  
###  <a name="examples-replication-agents"></a><a name="TsqlExample"></a> Ejemplos (agentes de replicación)  
 El ejemplo siguiente inicia el Agente de distribución para sincronizar una suscripción de inserción.  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksProductsTran  
  
REM -- Start the Distribution Agent with four subscription streams.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\DISTRIB.EXE" -Subscriber %Subscriber%   
-SubscriberDB %SubscriptionDB% -SubscriberSecurityMode 1 -Publication %Publication%   
-Publisher %Publisher% -PublisherDB %PublicationDB% -Distributor %Publisher%   
-DistributorSecurityMode 1 -Continuous -SubscriptionType 0 -SubscriptionStreams 4  
  
```  
  
 El ejemplo siguiente inicia el Agente de mezcla para sincronizar una suscripción de inserción.  
  
```  
  
REM -- Declare the variables.  
SET Publisher=%instancename%  
SET Subscriber=%instancename%  
SET PublicationDB=AdventureWorks2012  
SET SubscriptionDB=AdventureWorks2012Replica   
SET Publication=AdvWorksSalesOrdersMerge  
  
REM -- Start the Merge Agent.  
REM -- The following command must be supplied without line breaks.  
"C:\Program Files\Microsoft SQL Server\120\COM\REPLMERG.EXE"  -Publisher %Publisher%   
-Subscriber  %Subscriber%  -Distributor %Publisher% -PublisherDB  %PublicationDB%   
-SubscriberDB %SubscriptionDB% -Publication %Publication% -PublisherSecurityMode 1   
-OutputVerboseLevel 3  -Output -SubscriberSecurityMode 1  -SubscriptionType 0   
-DistributorSecurityMode 1  
  
```  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede sincronizar suscripciones de inserción mediante programación utilizando Replication Management Objects (RMO) y el acceso de código administrado a las funcionalidades del agente de replicación. Las clases que se usan para crear una suscripción de inserción dependen del tipo de publicación al que pertenece la suscripción.  
  
> [!NOTE]
>  Si desea iniciar una sincronización que se ejecute de forma autónoma sin afectar a la aplicación, inicie el agente asincrónicamente. Sin embargo, si desea supervisar el resultado de la sincronización y recibir las devoluciones de llamada del agente durante el proceso de sincronización (por ejemplo, si desea mostrar una barra de progreso), debe iniciar el agente sincrónicamente. Para los suscriptores de [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[ssExpressEd2005](../../includes/ssexpressed2005-md.md)], debe iniciar el agente sincrónicamente.  
  
#### <a name="to-synchronize-a-push-subscription-to-a-snapshot-or-transactional-publication"></a>Para sincronizar una suscripción de inserción a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription> y establezca las propiedades siguientes:  
  
    -   El nombre de la base de datos de publicación para <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
    -   El nombre de la publicación a la que pertenece la suscripción para <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.  
  
    -   El nombre de la base de datos de suscripciones para <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
    -   El nombre del Suscriptor para <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.  
  
    -   La conexión creada en el paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades de suscripción restantes. Si este método devuelve **false**, compruebe que la suscripción existe.  
  
4.  Inicie el Agente de distribución en el Distribuidor de una de las maneras siguientes:  
  
    -   Llame al método <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizeWithJob%2A> en la instancia de <xref:Microsoft.SqlServer.Replication.TransSubscription> del paso 2. Este método inicia el Agente de distribución de forma asincrónica y controla inmediatamente las devoluciones a la aplicación mientras se está ejecutando el trabajo del agente. No puede llamar a este método si la suscripción se creó con un valor **false** para <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>.  
  
    -   Obtenga una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent> de la propiedad <xref:Microsoft.SqlServer.Replication.TransSubscription.SynchronizationAgent%2A> y llame al método <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Synchronize%2A>. Este método inicia el agente sincrónicamente y controla el resto con el trabajo del agente en ejecución. Durante la ejecución sincrónica, puede administrar el evento <xref:Microsoft.SqlServer.Replication.TransSynchronizationAgent.Status> mientras el agente se está ejecutando.  
  
#### <a name="to-synchronize-a-push-subscription-to-a-merge-publication"></a>Para sincronizar una suscripción de inserción con una publicación de combinación  
  
1.  Cree una conexión al distribuidor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription> y establezca las propiedades siguientes:  
  
    -   El nombre de la base de datos de publicación para <xref:Microsoft.SqlServer.Replication.Subscription.DatabaseName%2A>.  
  
    -   El nombre de la publicación a la que pertenece la suscripción para <xref:Microsoft.SqlServer.Replication.Subscription.PublicationName%2A>.  
  
    -   El nombre de la base de datos de suscripciones para <xref:Microsoft.SqlServer.Replication.Subscription.SubscriptionDBName%2A>.  
  
    -   El nombre del Suscriptor para <xref:Microsoft.SqlServer.Replication.Subscription.SubscriberName%2A>.  
  
    -   La conexión creada en el paso 1 para <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A>.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades de suscripción restantes. Si este método devuelve **false**, compruebe que la suscripción existe.  
  
4.  Inicie el Agente de mezcla en el Distribuidor de una de las maneras siguientes:  
  
    -   Llame al método <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizeWithJob%2A> en la instancia de <xref:Microsoft.SqlServer.Replication.MergeSubscription> del paso 2. Este método inicia el Agente de mezcla de forma asincrónica y controla inmediatamente las devoluciones a la aplicación mientras se está ejecutando el trabajo del agente. No puede llamar a este método si la suscripción se creó con un valor **false** para <xref:Microsoft.SqlServer.Replication.Subscription.CreateSyncAgentByDefault%2A>.  
  
    -   Obtenga una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent> de la propiedad <xref:Microsoft.SqlServer.Replication.MergeSubscription.SynchronizationAgent%2A> y llame al método <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Synchronize%2A>. Este método inicia el Agente de mezcla sincrónicamente y controla el resto con el trabajo del agente en ejecución. Durante la ejecución sincrónica, puede administrar el evento <xref:Microsoft.SqlServer.Replication.MergeSynchronizationAgent.Status> mientras el agente se está ejecutando.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 Este ejemplo sincroniza una suscripción de inserción con una publicación transaccional, en la que el agente se inicia forma asincrónica con el trabajo del agente.  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub_withjob)]  
  
 Este ejemplo sincroniza una suscripción de inserción con una publicación transaccional, en la que el agente se inicia sincrónicamente.  
  
 [!code-cs[HowTo#rmo_SyncTranPushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_synctranpushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncTranPushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_synctranpushsub)]  
  
 Este ejemplo sincroniza una suscripción de inserción con una publicación de combinación, en la que el agente se inicia forma asincrónica con el trabajo del agente.  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub_withjob)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub_WithJob](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub_withjob)]  
  
 Este ejemplo sincroniza una suscripción de inserción con una publicación de combinación, en la que el agente se inicia sincrónicamente.  
  
 [!code-cs[HowTo#rmo_SyncMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_syncmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_SyncMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_syncmergepushsub)]  
  
## <a name="see-also"></a>Consulte también  
 [Replication Management Objects Concepts](../../relational-databases/replication/concepts/replication-management-objects-concepts.md)   
 [Sincronizar datos](../../relational-databases/replication/synchronize-data.md)   
 [Procedimientos recomendados de seguridad de replicación](../../relational-databases/replication/security/replication-security-best-practices.md)  
  
  
