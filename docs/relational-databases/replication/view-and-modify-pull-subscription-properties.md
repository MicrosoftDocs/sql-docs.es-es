---
description: Ver y modificar las propiedades de una suscripción de extracción
title: Ver y modificar las propiedades de una suscripción de extracción | Microsoft Docs
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying subscriptions
- viewing replication properties
- modifying replication properties, pull subscriptions
- pull subscriptions [SQL Server replication], modifying
- subscriptions [SQL Server replication], pull
- pull subscriptions [SQL Server replication], properties
- modifying subscriptions, SQL Server Management Studio
ms.assetid: 1601e54f-86f0-49e8-b023-87a5d1def033
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 91e312326e1d45ad4c5d747133baaa7c2c25e5d5
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250554"
---
# <a name="view-and-modify-pull-subscription-properties"></a>Ver y modificar las propiedades de una suscripción de extracción
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo ver y modificar las propiedades de una suscripción de extracción en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Para ver y modificar las propiedades de una suscripción de extracción con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Vea las propiedades de las suscripciones de extracción del publicador o el suscriptor en el cuadro de diálogo **Propiedades de suscripción - \<Publisher>: \<PublicationDatabase>** que está disponible en [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. En el suscriptor se pueden ver más propiedades y éstas se pueden modificar. También se pueden ver propiedades del publicador en la pestaña **Todas las suscripciones** , que está disponible en el Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-management-studio"></a>Para ver las propiedades de las suscripciones de extracción en el publicador en Management Studio  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Expanda la publicación apropiada, haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  Vea las propiedades y, a continuación, haga clic en **Aceptar**.  

#### <a name="to-view-and-modify-pull-subscription-properties-from-the-subscriber-in-management-studio"></a>Para ver y modificar las propiedades de las suscripciones de extracción en el publicador en Management Studio  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, la carpeta **Suscripciones locales**.  
  
3.  Haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  Modifique las propiedades si es necesario y, a continuación, haga clic en **Aceptar**.  
  
#### <a name="to-view-pull-subscription-properties-from-the-publisher-in-replication-monitor"></a>Para ver las propiedades de las suscripciones de extracción en el publicador en el Monitor de replicación  
  
1.  Expanda un grupo de publicador en el panel izquierdo del Monitor de replicación, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Todas las suscripciones** .  
  
3.  Haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  Vea las propiedades y, a continuación, haga clic en **Aceptar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Se pueden modificar las suscripciones de extracción y tener acceso a sus propiedades mediante programación usando procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que corresponda la suscripción.  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Para ver las propiedades de una suscripción de extracción a una publicación transaccional o de instantáneas  
  
1.  En el suscriptor, ejecute [sp_helppullsubscription](../../relational-databases/system-stored-procedures/sp-helppullsubscription-transact-sql.md). Especifique `@publisher`, `@publisher_db`y `@publication`. Esto devuelve información sobre la suscripción que está almacenada en tablas del sistema en el Suscriptor.  
  
2.  En el Suscriptor, ejecute [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Especifique `@publisher`, `@publisher_db`, `@publication`y uno de los valores siguientes para `@publication_type`:  
  
    -   **0** : la suscripción pertenece a una publicación transaccional.  
  
    -   **1** : la suscripción pertenece a una publicación de instantáneas.  
  
3.  En el Publicador, ejecute [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md). Especifique `@publication` y `@subscriber`.  
  
4.  En el publicador, ejecute [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), y especifique `@subscriber`. Presenta información acerca del suscriptor.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Para modificar las propiedades de una suscripción de extracción a una publicación transaccional o de instantáneas  
  
1.  En el suscriptor, ejecute [sp_change_subscription_properties](../../relational-databases/system-stored-procedures/sp-change-subscription-properties-transact-sql.md) y especifique `@publisher`, `@publisher_db`, `@publication`, un valor de **0** (transaccional) o de **1** (instantánea) para `@publication_type`, la propiedad de suscripción que se va a cambiar como `@property` y el nuevo valor como `@value`.  
  
2.  (Opcional) En el suscriptor de la base de datos de suscripciones, ejecute [sp_changesubscriptiondtsinfo](../../relational-databases/system-stored-procedures/sp-changesubscriptiondtsinfo-transact-sql.md). Especifique el identificador del trabajo del Agente de distribución para `@jobid` y las siguientes propiedades del paquete de los Servicios de transformación de datos (DTS):  
  
    -   `@dts_package_name`  
  
    -   `dts_package_password`  
  
    -   `@dts_package_location`  
  
     De esta forma se cambian las propiedades del paquete DTS de una suscripción.  
  
    > [!NOTE]  
    >  El Id del trabajo se puede obtener ejecutando [sp_helpsubscription](../../relational-databases/system-stored-procedures/sp-helpsubscription-transact-sql.md).  
  
#### <a name="to-view-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Para ver o las propiedades de una suscripción de extracción a una publicación de combinación  
  
1.  En el suscriptor, ejecute [sp_helpmergepullsubscription](../../relational-databases/system-stored-procedures/sp-helpmergepullsubscription-transact-sql.md). Especifique `@publisher`, `@publisher_db`y `@publication`.  
  
2.  En el Suscriptor, ejecute [sp_helpsubscription_properties](../../relational-databases/system-stored-procedures/sp-helpsubscription-properties-transact-sql.md). Especifique `@publisher`, `@publisher_db`, `@publication` y un valor de 2 para `@publication_type`.  
  
3.  En el Publicador, ejecute [sp_helpmergesubscription](../../relational-databases/system-stored-procedures/sp-helpmergesubscription-transact-sql.md) para mostrar información de la suscripción. Para devolver información sobre una suscripción concreta, debe especificar `@publication`, `@subscriber` y un valor de **pull** para @subscription_type.  
  
4.  En el publicador, ejecute [sp_helpsubscriberinfo](../../relational-databases/system-stored-procedures/sp-helpsubscriberinfo-transact-sql.md), y especifique `@subscriber`. Presenta información acerca del suscriptor.  
  
#### <a name="to-change-the-properties-of-a-pull-subscription-to-a-merge-publication"></a>Para cambiar las propiedades de una suscripción de extracción a una publicación de combinación  
  
1.  En el suscriptor, ejecute [sp_changemergepullsubscription](../../relational-databases/system-stored-procedures/sp-changemergepullsubscription-transact-sql.md). Especifique `@publication`, `@publisher`, `@publisher_db`, la propiedad de suscripción que se va a cambiar como `@property` y el nuevo valor como `@value`.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Las clases RMO que usa para ver o modificar las propiedades de suscripción de extracción dependen del tipo de publicación a la que se suscribe la suscripción de extracción.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-snapshot-or-transactional-publication"></a>Para ver o modificar propiedades de una suscripción de extracción a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPullSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, se definieron incorrectamente las propiedades de suscripción en el paso 3, o bien la suscripción no existe en el servidor.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.TransPullSubscription> que se puedan establecer y, a continuación, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Opcional) Para ver los nuevos valores, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> para recargar las propiedades del artículo.  
  
8.  Cierre todas las conexiones.  
  
#### <a name="to-view-or-modify-properties-of-a-pull-subscription-to-a-merge-publication"></a>Para ver o modificar las propiedades de una suscripción de extracción a una publicación de combinación  
  
1.  Cree una conexión al suscriptor mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection>.  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePullSubscription>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.DatabaseName%2A>, <xref:Microsoft.SqlServer.Replication.PullSubscription.PublisherName%2A>y <xref:Microsoft.SqlServer.Replication.PullSubscription.PublicationDBName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, se definieron incorrectamente las propiedades de suscripción en el paso 3, o bien la suscripción no existe en el servidor.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.MergePullSubscription> que se puedan establecer y, a continuación, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  (Opcional) Para ver los nuevos valores, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.Refresh%2A> para recargar las propiedades del artículo.  
  
8.  Cierre todas las conexiones.  
  
## <a name="see-also"></a>Consulte también  
 [Visualización de información y realización de tareas mediante el Monitor de replicación](../../relational-databases/replication/monitor/view-information-and-perform-tasks-replication-monitor.md)   
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Suscribirse a publicaciones](../../relational-databases/replication/subscribe-to-publications.md)  
  
  
