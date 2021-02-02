---
description: Ver y modificar las propiedades de un artículo
title: Ver y modificar las propiedades de un artículo | Microsoft Docs
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
- sp_changearticle
- sp_helparticle
- viewing replication properties
- sp_changemergearticle
- sp_helpmergearticle
- modifying replication properties, articles
- articles [SQL Server replication], modifying
- articles [SQL Server replication], properties
ms.assetid: e71831fa-3d39-4e4a-9706-4d3a497082cc
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 8901c5e21f257d9b84e625c7f59a7d13c899bd1b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076983"
---
# <a name="view-and-modify-article-properties"></a>Ver y modificar las propiedades de un artículo
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo ver y modificar las propiedades del artículo en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para ver y modificar las propiedades del artículo con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Algunas propiedades no se pueden modificar después de crearlas, y otras no se pueden modificar si existen suscripciones a la publicación. Las propiedades que no se pueden modificar se muestran como de solo lectura.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Una vez creada una publicación, algunos cambios de las propiedades requieren una nueva instantánea. Si una publicación tiene suscripciones, algunos cambios también requieren reinicializar todas las suscripciones. Para más información, vea [Change Publication and Article Properties](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos) y [Agregar y quitar artículos de publicaciones existentes](../../../relational-databases/replication/publish/add-articles-to-and-drop-articles-from-existing-publications.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Vea y modifique las propiedades del artículo en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , que está disponible en [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] y el Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
-   La página **General** incluye el nombre y la descripción de la publicación, el nombre de la base de datos, el tipo de publicación y los valores de expiración de la suscripción.  
  
-   La página **Artículos** corresponde a la página **Artículos** del Asistente para nueva publicación. Utilice esta página para agregar y eliminar artículos, y para cambiar propiedades y el filtro de columna de artículos.  
  
-   La página **Filtrar filas** corresponde a la página **Filtrar filas de tabla** del Asistente para nueva publicación. Utilice esta página para agregar, editar y eliminar filtros de fila estáticos de todos los tipos de publicaciones, y para agregar, editar y eliminar filtros de fila con parámetros y filtros de combinación de publicaciones de combinación.  
  
-   La página **Instantánea** permite especificar el formato y la ubicación de la instantánea, si la instantánea debe comprimirse y si los scripts deben ejecutarse antes y después de aplicar la instantánea.  
  
-   La página **Instantánea de FTP** (en publicaciones de instantáneas y transaccionales, y publicaciones de combinación en publicadores que ejecutan versiones anteriores a SQL Server 2005) permite especificar si los suscriptores pueden descargar archivos de instantáneas mediante el Protocolo de transferencia de archivos (FTP).  
  
-   La página **Instantánea de FTP e Internet** (en publicaciones de combinación de publicadores que ejecutan SQL Server 2005 o posterior) permite especificar si los suscriptores pueden descargar archivos de instantáneas mediante FTP y si los suscriptores pueden sincronizar suscripciones mediante HTTPS.  
  
-   La página **Opciones de suscripción** permite establecer diversas opciones que se aplican a todas las suscripciones. Las opciones varían según el tipo de publicación.  
  
-   La página **Lista de acceso a la publicación** permite especificar qué inicios de sesión y grupos pueden tener acceso a una publicación.  
  
-   La página **Seguridad del agente** le permite tener acceso a la configuración para las cuentas con las que se ejecutan los agentes siguientes y realizan conexiones a los equipos en una topología de replicación: el Agente de instantáneas para todas las publicaciones, el Agente de registro del LOG para todas las publicaciones transaccionales y el Agente de lectura de cola para las publicaciones transaccionales que permiten las suscripciones de actualización en cola.  
  
-   La página **Particiones de datos** (en publicaciones de combinación de publicadores que ejecutan SQL Server 2005 o posterior) permite especificar si los suscriptores a publicaciones con filtros con parámetros pueden solicitar una instantánea si no está disponible. También permite generar instantáneas de una o varias particiones, solo una vez o varias veces.  
  
#### <a name="to-view-and-modify-article-properties"></a>Para ver y modificar propiedades de un artículo  
  
1.  En la página **Artículos** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un artículo y luego haga clic en **Propiedades del artículo**.  
  
2.  Seleccione a qué propiedades de artículos se deben aplicar los cambios:  
  
    -   Haga clic en **Establecer propiedades del artículo de \<ObjectType> resaltado** para iniciar el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** ; los cambios de propiedad realizados en este cuadro de diálogo solo se aplican al objeto que está resaltado en el panel de objetos de la página **Artículos**.  
  
    -   Haga clic en **Establecer propiedades de todos los artículos de \<ObjectType>** para iniciar el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** . Los cambios de propiedad realizados en este cuadro de diálogo se aplican a todos los objetos de ese tipo en el panel de objetos de la página **Artículos**, incluidos los que todavía no se hayan seleccionado para la publicación.  
  
        > [!NOTE]  
        >  Los cambios de propiedades realizados en el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** reemplazan los que se hicieran anteriormente en el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** . Por ejemplo, si desea establecer varios valores predeterminados para todos los artículos de un tipo de objeto, pero solamente desea establecer algunas propiedades para objetos individuales, establezca primero los valores predeterminados para todos los artículos. A continuación, establezca las propiedades de los objetos individuales.  
  
3.  Modifique las propiedades si es necesario y, a continuación, haga clic en **Aceptar**.  
  
4.  Haga clic en **Aceptar** en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** .  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Se pueden modificar los artículos y devolver sus propiedades mediante programación con los procedimientos almacenados de replicación. Los procedimientos almacenados que se usen dependerán del tipo de publicación a la que pertenece el artículo.  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>Para ver las propiedades de un artículo que pertenece a una publicación transaccional o de instantáneas  
  
1.  Ejecute [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md) y especifique el nombre de la publicación para el parámetro `@publication` y el nombre de artículo para el parámetro `@article`. Si no especifica `@article`, se devolverá información para todos los artículos de la publicación.  
  
2.  Ejecute [sp_helparticlecolumns](../../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md) para que los artículos de tabla muestren todas las columnas disponibles en la tabla base.  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-snapshot-or-transactional-publication"></a>Para modificar las propiedades de un artículo que pertenece a una publicación transaccional o de instantáneas  
  
1.  Ejecute [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md) y especifique la propiedad de artículo que se va a cambiar en el parámetro `@property` y el nuevo valor de esta propiedad en el parámetro `@value`.  
  
    > [!NOTE]  
    >  Si el cambio requiere la generación de una nueva instantánea, también debe especificar un valor de `1` para `@force_invalidate_snapshot`, y si el cambio requiere que se reinicialicen los suscriptores, debe especificar un valor de `1` para `@force_reinit_subscription`. Para más información sobre las propiedades que, cuando se cambian, necesitan una nueva instantánea o una reinicialización, [Change Publication and Article Properties](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
#### <a name="to-view-the-properties-of-an-article-belonging-to-a-merge-publication"></a>Para ver las propiedades de un artículo que pertenece a una publicación de combinación  
  
1.  Ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) y especifique el nombre de la publicación para el parámetro `@publication` y el nombre de artículo para el parámetro `@article`. Si no especifica estos parámetros, se devolverá información para todos los artículos de una publicación o en el publicador.  
  
2.  Ejecute [sp_helpmergearticlecolumn](../../../relational-databases/system-stored-procedures/sp-helpmergearticlecolumn-transact-sql.md) para que los artículos de tabla muestren todas las columnas disponibles en la tabla base.  
  
#### <a name="to-modify-the-properties-of-an-article-belonging-to-a-merge-publication"></a>Para modificar las propiedades de un artículo que pertenece a una publicación de combinación  
  
1.  Ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) y especifique la propiedad de artículo que se va a cambiar en el parámetro `@property` y el nuevo valor de esta propiedad en el parámetro `@value`.  
  
    > [!NOTE]  
    >  Si el cambio requiere la generación de una nueva instantánea, también debe especificar un valor de `1` para `@force_invalidate_snapshot`, y si el cambio requiere que se reinicialicen los suscriptores, debe especificar un valor de `1` para `@force_reinit_subscription`. Para más información sobre las propiedades que, cuando se cambian, necesitan una nueva instantánea o una reinicialización, [Change Publication and Article Properties](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 Este ejemplo de replicación transaccional devuelve las propiedades del artículo publicado.  
  
 [!code-sql[HowTo#sp_helptranarticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_1.sql)]  
  
 Este ejemplo de replicación transaccional cambia las opciones de esquema del artículo publicado.  
  
 [!code-sql[HowTo#sp_changetranarticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_2.sql)]  
  
 Este ejemplo de replicación de mezcla devuelve las propiedades del artículo publicado.  
  
 [!code-sql[HowTo#sp_helpmergearticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_3.sql)]  
  
 Este ejemplo de replicación de mezcla cambia los valores de detección de conflictos para un artículo publicado.  
  
 [!code-sql[HowTo#sp_changemergearticle](../../../relational-databases/replication/codesnippet/tsql/view-and-modify-article-_4.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 Puede modificar los artículos y obtener acceso mediante programación a sus propiedades utilizando Replication Management Objects (RMO). Las clases RMO que usa para ver o modificar las propiedades de artículo dependen del tipo de publicación a la que pertenece el artículo.  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-snapshot-or-transactional-publication"></a>Para ver o modificar propiedades de un artículo que pertenece a una publicación transaccional o de instantáneas  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransArticle>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>y <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, se definieron incorrectamente las propiedades de artículo en el paso 3, o bien el artículo no existe.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.TransArticle> que se pueden establecer.  
  
7.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
#### <a name="to-view-or-modify-properties-of-an-article-that-belongs-to-a-merge-publication"></a>Para ver o modificar propiedades de un artículo que pertenece a una publicación de combinación  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeArticle>.  
  
3.  Establezca las propiedades <xref:Microsoft.SqlServer.Replication.Article.Name%2A>, <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>y <xref:Microsoft.SqlServer.Replication.Article.DatabaseName%2A> .  
  
4.  Establezca la conexión del paso 1 para la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> .  
  
5.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, se definieron incorrectamente las propiedades de artículo en el paso 3, o bien el artículo no existe.  
  
6.  (Opcional) Para cambiar las propiedades, establezca un nuevo valor para una de las propiedades <xref:Microsoft.SqlServer.Replication.MergeArticle> que se pueden establecer.  
  
7.  (Opcional) Si especificara un valor de **true** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A>, llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.CommitPropertyChanges%2A> para confirmar los cambios en el servidor. Si especificó el valor **false** para <xref:Microsoft.SqlServer.Replication.ReplicationObject.CachePropertyChanges%2A> (predeterminado), los cambios se envían inmediatamente al servidor.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Ejemplo (RMO)  
 Este ejemplo cambia un artículo de mezcla para especificar el controlador de lógica de negocios que usa el artículo.  
  
 [!code-cs[HowTo#rmo_ChangeMergeArticle_BLH](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_changemergearticle_blh)]  
  
 [!code-vb[HowTo#rmo_vb_ChangeMergeArticle_BLH](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_changemergearticle_blh)]  
  
## <a name="see-also"></a>Consulte también  
 [Implement a Business Logic Handler for a Merge Article](../../../relational-databases/replication/implement-a-business-logic-handler-for-a-merge-article.md)  (Implementar un controlador de lógica de negocios para un artículo de mezcla)  
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Replicación de mezcla avanzada: detección y resolución de conflictos](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-detection-and-resolution.md)  
  
  
