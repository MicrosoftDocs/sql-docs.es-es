---
title: Eliminación de una publicación | Microsoft Docs
description: Aprenda a eliminar una publicación en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- removing publications
- publications [SQL Server replication], deleting
- articles [SQL Server replication], deleting
- deleting publications
ms.assetid: 408a1360-12ee-4896-ac94-482ae839593b
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: b1a87a8f9c2344b21cd67a24c35ab33615965300
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076733"
---
# <a name="delete-a-publication"></a>Eliminar una publicación
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo eliminar una publicación en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Para eliminar una publicación con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Elimine publicaciones de la carpeta **Publicaciones locales** en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
#### <a name="to-delete-a-publication"></a>Para eliminar una publicación  
  
1.  Conéctese al publicador en [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)]y luego expanda el nodo del servidor.  
  
2.  Expanda la carpeta **Replicación** y, a continuación, expanda la carpeta **Publicaciones locales** .  
  
3.  Haga clic con el botón secundario en la publicación que desea eliminar y, a continuación, haga clic en **Eliminar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las publicaciones pueden eliminarse mediante programación con procedimientos almacenados de replicación. Los procedimientos almacenados que use dependen del tipo de publicación que se elimina.  
  
> [!NOTE]  
>  Al eliminar una publicación, no se quitan los objetos publicados de la base de datos de publicación ni los objetos correspondientes de la base de datos de suscripciones. Si es necesario, use el comando `DROP <object>` para quitar estos objetos manualmente.  
  
#### <a name="to-delete-a-snapshot-or-transactional-publication"></a>Para eliminar una publicación transaccional o de instantáneas  
  
1.  Realice una de las siguientes acciones:  
  
    -   Para eliminar una publicación, ejecute [sp_droppublication](../../../relational-databases/system-stored-procedures/sp-droppublication-transact-sql.md) en la base de datos de publicación del publicador.  
  
    -   Para eliminar todas las publicaciones de una base de datos publicada y quitar todos sus objetos de replicación, ejecute [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) en el publicador. Especifique un valor de **tran** para **\@type**. (Opcional) Si no se puede tener acceso al distribuidor, o bien si el estado de la base de datos es sospechoso o está sin conexión, especifique un valor de **1** para **\@force**. (Opcional) Especifique el nombre de la base de datos para **\@dbname** si [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) no se ejecuta en la base de datos de publicación.  
  
        > [!NOTE]  
        >  Especificar un valor de **1** para **\@force** puede dejar objetos de publicación relacionados con la replicación en la base de datos.  
  
2.  (Opcional) Si esta base de datos no tiene otras publicaciones, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) para deshabilitar la publicación de la base de datos actual mediante la replicación transaccional o de instantáneas.  
  
3.  (Opcional) En la base de datos de suscripciones del suscriptor, ejecute [sp_subscription_cleanup](../../../relational-databases/system-stored-procedures/sp-subscription-cleanup-transact-sql.md) para quitar los metadatos de replicación restantes en la base de datos de suscripciones.  
  
#### <a name="to-delete-a-merge-publication"></a>Para eliminar una publicación de combinación  
  
1.  Realice una de las siguientes acciones:  
  
    -   Para eliminar una publicación, ejecute [sp_dropmergepublication &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-dropmergepublication-transact-sql.md) en la base de datos de publicación del publicador.  
  
    -   Para eliminar todas las publicaciones de una base de datos publicada y quitar todos sus objetos de replicación, ejecute [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) en el publicador. Especifique el valor de **merge** para **\@type**. (Opcional) Si no se puede tener acceso al distribuidor, o bien si el estado de la base de datos es sospechoso o está sin conexión, especifique un valor de **1** para **\@force**. (Opcional) Especifique el nombre de la base de datos para **\@dbname** si [sp_removedbreplication](../../../relational-databases/system-stored-procedures/sp-removedbreplication-transact-sql.md) no se ejecuta en la base de datos de publicación.  
  
        > [!NOTE]  
        >  Especificar un valor de **1** para **\@force** puede dejar objetos de publicación relacionados con la replicación en la base de datos.  
  
2.  (Opcional) Si esta base de datos no tiene otras publicaciones, ejecute [sp_replicationdboption &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-replicationdboption-transact-sql.md) para deshabilitar la publicación de la base de datos actual con la replicación de mezcla.  
  
3.  (Opcional) En la base de datos de suscripciones del suscriptor, ejecute [sp_mergesubscription_cleanup &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-mergesubscription-cleanup-transact-sql.md) para quitar los metadatos de replicación restantes en la base de datos de suscripciones.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 Este ejemplo muestra cómo quitar una publicación transaccional y deshabilitar la publicación transaccional para una base de datos. Este ejemplo supone que se quitaron todas las suscripciones previamente. Para obtener más información, consulte [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) o [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_droppublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_1.sql)]  
  
 Este ejemplo muestra cómo quitar una publicación de combinación y deshabilitar la publicación de combinación para una base de datos. Este ejemplo supone que se quitaron todas las suscripciones previamente. Para obtener más información, consulte [Delete a Pull Subscription](../../../relational-databases/replication/delete-a-pull-subscription.md) o [Delete a Push Subscription](../../../relational-databases/replication/delete-a-push-subscription.md).  
  
 [!code-sql[HowTo#sp_dropmergepublication](../../../relational-databases/replication/codesnippet/tsql/delete-a-publication_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede eliminar publicación mediante programación utilizando Replication Management Objects (RMO). Las clases RMO que utiliza para quitar una publicación dependen del tipo de publicación que quita.  
  
#### <a name="to-remove-a-snapshot-or-transactional-publication"></a>Para quitar una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPublication>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
4.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la publicación existe. Si el valor de esta propiedad es **false**, significa que las propiedades de la publicación del paso 3 se definieron incorrectamente, o bien que la publicación no existe.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Opcional) Si no existe ninguna otra publicación transaccional para esta base de datos, ésta se puede deshabilitar para la publicación transaccional del siguiente modo:  
  
    1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la instancia de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1.  
  
    2.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, confirme que la base de datos existe.  
  
    3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledTransPublishing%2A> en **false**.  
  
    4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Cierre las conexiones.  
  
#### <a name="to-remove-a-merge-publication"></a>Para quitar una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
4.  Compruebe la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.IsExistingObject%2A> para asegurarse de que la publicación existe. Si el valor de esta propiedad es **false**, significa que las propiedades de la publicación del paso 3 se definieron incorrectamente, o bien que la publicación no existe.  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.Publication.Remove%2A> .  
  
6.  (Opcional) Si no existe ninguna otra publicación de combinación para esta base de datos, ésta se puede deshabilitar para la publicación de combinación del siguiente modo:  
  
    1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.ReplicationDatabase>. Establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la instancia de <xref:Microsoft.SqlServer.Management.Common.ServerConnection> del paso 1.  
  
    2.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> . Si este método devuelve **false**, compruebe que la base de datos existe.  
  
    3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.ReplicationDatabase.EnabledMergePublishing%2A> en **false**.  
  
    4.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> .  
  
7.  Cierre las conexiones.  
  
###  <a name="examples-rmo"></a><a name="PShellExample"></a> Ejemplos (RMO)  
 En el siguiente ejemplo se elimina una publicación transaccional Si no existe ninguna otra publicación transaccional para esta base de datos, también se deshabilita la publicación transaccional.  
  
 [!code-cs[HowTo#rmo_DropTranPub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_droptranpub)]  
  
 [!code-vb[HowTo#rmo_vb_DropTranPub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_droptranpub)]  
  
 En el siguiente ejemplo se elimina una publicación de combinación. Si no existe ninguna otra publicación de combinación para esta base de datos, también se deshabilita la publicación de combinación.  
  
 [!code-cs[HowTo#rmo_DropMergePub](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_dropmergepub)]  
  
 [!code-vb[HowTo#rmo_vb_DropMergePub](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_dropmergepub)]  
  
## <a name="see-also"></a>Consulte también  
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)  
  
  
