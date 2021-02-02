---
title: Definición de una relación de registros lógicos entre artículos de tabla de combinación
description: Obtenga información sobre cómo definir la relación de registros lógicos entre las tablas relacionadas utilizadas para los artículos de replicación de combinación.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication logical records [SQL Server replication]
- articles [SQL Server replication], logical records
- logical records [SQL Server replication]
ms.assetid: ff847b3a-c6b0-4eaf-b225-2ffc899c5558
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 397ecaba4cf6b8e56f3dbcb6366dca8121ae345d
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076793"
---
# <a name="define-a-logical-record-relationship-between-merge-table-articles"></a>Definir una relación de registros lógicos entre artículos de tabla de mezcla
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo definir una relación de registros lógicos entre los artículos de tabla de mezcla en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o Replication Management Objects (RMO).  
  
 La replicación de mezcla le permite definir una relación entre filas relacionadas de tablas diferentes. De ese modo, las filas se pueden procesar como una unidad transaccional durante la sincronización. Se puede definir un registro lógico entre dos artículos independientemente de que tengan una relación de filtro de unión o no. Para más información, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
-   **Para definir una relación de registros lógicos entre artículos de tabla de mezcla con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si agrega, modifica o elimina un registro lógico una vez inicializadas las suscripciones a la publicación, deberá generar una instantánea nueva y reinicializar todas las suscripciones después de realizar el cambio. Para obtener más información sobre los requisitos para los cambios de propiedad, consulte [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Defina registros lógicos en el cuadro de diálogo **Agregar combinación**, que está disponible en el Asistente para nueva publicación y en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
 Los registros lógicos se pueden definir en el cuadro de diálogo **Agregar combinación** solamente si se aplican a un filtro de combinación en una publicación de combinación y la publicación cumple los requisitos para utilizar particiones precalculadas. Debe utilizar procedimientos almacenados para definir registros lógicos que no se aplican a los filtros de combinación y para establecer la detección y resolución de conflictos en el nivel de registro lógico.  
  
#### <a name="to-define-a-logical-record-relationship"></a>Para definir una relación de registros lógicos  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un filtro de fila en el panel **Tablas filtradas**.  
  
     Una relación de registros lógicos está asociada con un filtro de combinación, que amplía un filtro de fila. Por tanto, debe definir un filtro de fila antes de poder ampliar el filtro con una combinación y aplicar una relación de registros lógicos. Una vez definido el filtro de combinación, podrá ampliarlo con otro filtro de combinación. Para obtener más información acerca de cómo definir filtros de combinación, vea [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
2.  Haga clic en **Agregar** y, a continuación, en **Agregar combinación para ampliar el filtro seleccionado**.  
  
3.  Defina el filtro de combinación en el cuadro de diálogo **Agregar combinación** y, a continuación, active la casilla **Registro lógico**.  
  
4.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
#### <a name="to-delete-a-logical-record-relationship"></a>Para eliminar una relación de registros lógicos  
  
-   Elimine solamente la relación de registros lógicos o elimine la relación de registros lógicos y el filtro de combinación asociado a la misma.  
  
     Para eliminar solamente la relación de registros lógicos:  
  
    1.  En la página **Filtrar filas** del Asistente para nueva publicación o en la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione el filtro de combinación asociado con la relación de registros lógicos del panel **Tablas filtradas** y haga clic en **Editar**.  
  
    2.  En el cuadro de diálogo **Editar combinación** , desactive la casilla **Registro lógico**.  
  
    3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
     Para eliminar la relación de registros lógicos y el filtro de combinación asociado a la misma:  
  
    -   En la página **Filtrar filas** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un filtro en el panel **Tablas filtradas** y haga clic en **Eliminar**. Si el filtro de combinación que elimina está a su vez ampliado por otras combinaciones, esas combinaciones también se eliminarán.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede especificar mediante programación las relaciones de registros lógicos entre los artículos usando procedimientos almacenados de replicación.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Para definir una relación de registros lógicos sin un filtro de combinación asociado  
  
1.  Si la publicación contiene cualquier artículo que se filtra, ejecute [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md)y tenga en cuenta el valor de **use_partition_groups** en el conjunto de resultados.  
  
    -   Si el valor es **1**, las particiones precalculadas ya se están usando.  
  
    -   Si el valor es **0**, ejecute [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique un valor de **use_partition_groups** para **\@property** y un valor de **true** para **\@value**.  
  
        > [!NOTE]  
        >  Si la publicación no admite las particiones precalculadas, no se pueden usar registros lógicos. Para obtener más información, vea la sección Requisitos para utilizar particiones precalculadas de [Optimizar el rendimiento de los filtros con parámetros con particiones calculadas previamente](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   Si el valor es NULL, el Agente de instantáneas tiene que seguir ejecutándose para generar la instantánea inicial para la publicación.  
  
2.  Si no existen los artículos que comprenderán el registro lógico, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) en el Publicador de la base de datos de publicación. Especifique una de las siguientes opciones de detección y resolución de conflictos para el registro lógico:  
  
    -   Para detectar y solucionar conflictos que se producen dentro de las filas relacionadas en el registro lógico, especifique un valor de **true** para **\@logical_record_level_conflict_detection** y **\@logical_record_level_conflict_resolution**.  
  
    -   Para usar la resolución y detección de conflictos estándar en el nivel de filas o columnas, especifique un valor de **false** para **\@logical_record_level_conflict_detection** y **\@logical_record_level_conflict_resolution**, que es el valor predeterminado.  
  
3.  Repita el paso 2 para cada artículo que comprenderá el registro lógico. Debe usar la misma opción de detección y resolución de conflictos para cada artículo del registro lógico. Para más información, consulte [Detectar y solucionar conflictos en registros lógicos](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
  
4.  En la base de datos de publicación del publicador, ejecute [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md). Especifique **\@publication**, el nombre de un artículo en la relación para **\@article**, el nombre del segundo artículo para **\@join_articlename**, un nombre para la relación para **\@filtername**, una cláusula que defina la relación entre los dos artículos para **\@join_filterclause**, el tipo de combinación para **\@join_unique_key** y uno de los valores siguientes para **\@filter_type**:  
  
    -   **2** : define una relación lógica.  
  
    -   **3** : define una relación lógica con un filtro de combinación.  
  
    > [!NOTE]  
    >  Si no se usa un filtro de combinación, la dirección de la relación entre los dos artículos no es importante.  
  
5.  Repita el paso 2 para cada una de las demás relaciones de registros lógicos de la publicación.  
  
#### <a name="to-change-conflict-detection-and-resolution-for-logical-records"></a>Para cambiar la detección y resolución de conflictos para los registros lógicos  
  
1.  Para detectar y solucionar conflictos que se producen dentro de las filas relacionadas en el registro lógico:  
  
    -   En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de **logical_record_level_conflict_detection** para **\@property** y un valor de **true** para **\@value**. Especifique un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
    -   En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de **logical_record_level_conflict_resolution** para **\@property** y un valor de **true** para **\@value**. Especifique un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
2.  Para usar la detección y resolución de conflictos de nivel de columna y de fila estándar:  
  
    -   En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de **logical_record_level_conflict_detection** para **\@property** y un valor de **false** para **\@value**. Especifique un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
    -   En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de **logical_record_level_conflict_resolution** para **\@property** y un valor de **false** para **\@value**. Especifique un valor de **1** para **\@force_invalidate_snapshot** y **\@force_reinit_subscription**.  
  
#### <a name="to-remove-a-logical-record-relationship"></a>Para quitar una relación de registros lógicos  
  
1.  En el Publicador de la base de datos de publicación, ejecute la consulta siguiente para devolver información sobre todas las relaciones de registros lógicos definidas para la publicación especificada:  
  
     [!code-sql[HowTo#sp_ReturnMergeLogicalRecords](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_1.sql)]  
  
     Tenga en cuenta el nombre de la relación de registros lógicos que se está quitando en la columna **filtername** del conjunto de resultados.  
  
    > [!NOTE]  
    >  Esta consulta devuelve la misma información que [sp_helpmergefilter](../../../relational-databases/system-stored-procedures/sp-helpmergefilter-transact-sql.md); sin embargo, este procedimiento almacenado del sistema solamente devuelve información sobre las relaciones de registros lógicos que también son filtros de combinación.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_dropmergefilter](../../../relational-databases/system-stored-procedures/sp-dropmergefilter-transact-sql.md). Especifique **\@publication**, el nombre de uno de los artículos de la relación para **\@article** y el nombre de la relación del paso 1 para **\@filtername**.  
  
###  <a name="example-transact-sql"></a><a name="TsqlExample"></a> Ejemplo (Transact-SQL)  
 En este ejemplo se habilitan las particiones calculadas previamente en una publicación existente y se crea un registro lógico que comprende los dos artículos nuevos para las tablas `SalesOrderHeader` y `SalesOrderDetail` .  
  
 [!code-sql[HowTo#sp_AddMergeLogicalRecord](../../../relational-databases/replication/codesnippet/tsql/define-a-logical-record-_2.sql)]  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
  
> [!NOTE]  
>  La replicación de mezcla le permite especificar que se realice un seguimiento de los conflictos y se resuelvan en el nivel de registro lógico, pero dichas opciones no se pueden establecer usando RMO.  
  
#### <a name="to-define-a-logical-record-relationship-without-an-associated-join-filter"></a>Para definir una relación de registros lógicos sin un filtro de combinación asociado  
  
1.  Cree una conexión al publicador mediante la clase <xref:Microsoft.SqlServer.Management.Common.ServerConnection> .  
  
2.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePublication> , establezca las propiedades <xref:Microsoft.SqlServer.Replication.Publication.Name%2A> y <xref:Microsoft.SqlServer.Replication.Publication.DatabaseName%2A> para la publicación y establezca la propiedad <xref:Microsoft.SqlServer.Replication.ReplicationObject.ConnectionContext%2A> en la conexión creada en el paso 1.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.ReplicationObject.LoadProperties%2A> para obtener las propiedades del objeto. Si este método devuelve **false**, significa que las propiedades de publicación del paso 2 se definieron incorrectamente, o bien que la publicación no existe.  
  
4.  Si la propiedad <xref:Microsoft.SqlServer.Replication.MergePublication.PartitionGroupsOption%2A> está establecida en <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.False>, cámbiela a <xref:Microsoft.SqlServer.Replication.PartitionGroupsOption.True>.  
  
5.  Si los artículos que van a componer el registro lógico no existen, cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeArticle> y establezca las propiedades siguientes:  
  
    -   El nombre del artículo para <xref:Microsoft.SqlServer.Replication.Article.Name%2A>.  
  
    -   El nombre de la publicación para <xref:Microsoft.SqlServer.Replication.Article.PublicationName%2A>.  
  
    -   (Opcional) Si se filtra el artículo horizontalmente, especifique la cláusula de filtro de filas para la propiedad <xref:Microsoft.SqlServer.Replication.MergeArticle.FilterClause%2A> . Utilice esta propiedad para especificar un filtro de fila estático o con parámetros. Para obtener más información, consulte [Filtros de fila con parámetros](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
6.  Llame al método <xref:Microsoft.SqlServer.Replication.Article.Create%2A> .  
  
7.  Repita los pasos 5 y 6 para cada artículo que comprende el registro lógico.  
  
8.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> para definir la relación de registros lógicos entre los artículos. A continuación, establezca las siguientes propiedades:  
  
    -   El nombre del artículo secundario en la relación de registros lógicos para la propiedad <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.ArticleName%2A> .  
  
    -   El nombre del artículo primario, ya existente, en la relación de registros lógicos para la propiedad <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinArticleName%2A> .  
  
    -   Un nombre para la relación de registros lógicos para la propiedad <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterName%2A> .  
  
    -   La expresión que define la relación para la propiedad <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.JoinFilterClause%2A> .  
  
    -   El valor <xref:Microsoft.SqlServer.Replication.FilterTypes.LogicalRecordLink> para la propiedad <xref:Microsoft.SqlServer.Replication.MergeJoinFilter.FilterTypes%2A> . Si la relación de registros lógicos también es un filtro de combinación, especifique el valor <xref:Microsoft.SqlServer.Replication.FilterTypes.JoinFilterAndLogicalRecordLink> para esta propiedad. Para más información, vea [Agrupar cambios en filas relacionadas con registros lógicos](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
9. Llame al método <xref:Microsoft.SqlServer.Replication.MergeArticle.AddMergeJoinFilter%2A> en el objeto que representa el artículo secundario en la relación. Pase el objeto <xref:Microsoft.SqlServer.Replication.MergeJoinFilter> del paso 8 para definir la relación.  
  
10. Repita los pasos 8 y 9 para cada una de las demás relaciones de registros lógicos de la publicación.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Ejemplo (RMO)  
 En este ejemplo se crea un registro lógico que incluye los dos artículos nuevos para las tablas `SalesOrderHeader` y `SalesOrderDetail` .  
  
 [!code-cs[HowTo#rmo_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createlogicalrecord)]  
  
 [!code-vb[HowTo#rmo_vb_CreateLogicalRecord](../../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createlogicalrecord)]  
  
## <a name="see-also"></a>Consulte también  
 [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Definición y modificación de un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Definir y modificar un filtro de fila estático](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [Agrupar cambios en filas relacionadas con registros lógicos](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)   
 [Optimizar el rendimiento de los filtros con parámetros con particiones calculadas previamente](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md)   
 [Agrupar cambios en filas relacionadas con registros lógicos](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md)  
  
  
