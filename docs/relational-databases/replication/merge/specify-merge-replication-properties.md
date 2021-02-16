---
description: Especificación de las propiedades de replicación de mezcla
title: Especificación de las propiedades de replicación de mezcla | Microsoft Docs
ms.custom: ''
ms.date: 11/20/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- merge replication [SQL Server replication], download-only articles
- articles [SQL Server replication], download-only
- download-only articles
ms.assetid: 14839cec-6dbf-49c2-aa27-56847b09b4db
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f53a43fa102505121cd176eca6c94d4697a25372
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100336190"
---
# <a name="specify-merge-replication-properties"></a>Especificación de las propiedades de replicación de mezcla
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
En este tema se explica cómo especificar varias propiedades para la replicación de mezcla. 

## <a name="merge-article-is-download-only"></a>El artículo de mezcla es de solo descarga
Los artículos de solo descarga están diseñados para aplicaciones con datos que no se actualizan en suscriptores. Para más información, vea [Optimizar el rendimiento de la replicación de mezcla con artículos de solo descarga](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).  
   
###  <a name="considerations"></a>Consideraciones   
-   Si se especifica que un artículo es de solo descarga después de haber inicializado las suscripciones, deben reinicializarse todas las suscripciones de cliente que reciben el artículo. No se tienen que reinicializar las suscripciones de servidor. Para obtener más información sobre los efectos de los cambios de propiedad, vea [Change Publication and Article Properties](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
### <a name="use-sql-server-management-studio"></a>Usar SQL Server Management Studio  
  
#### <a name="on-the-articles-page"></a>En la página Artículos
En la página **Artículos** del Asistente para nueva publicación, seleccione una tabla y, a continuación, active la casilla **La tabla resaltada es de solo descarga**.  
  
#### <a name="on-the-properties-tab-of-the-article-properties"></a>En la pestaña Propiedades de las propiedades del artículo  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla y haga clic en **Propiedades del artículo**.    
2.  Haga clic en **Establecer propiedades del artículo de Tabla resaltado** o **Establecer propiedades de todos los artículos de la tabla**.  
  
3.  En la sección **Objeto de destino** de la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , especifique uno de los siguientes valores para **Dirección de la sincronización**:    
    -   **Solo descargar en suscriptor y prohibir cambios del suscriptor**    
    -   **Solo descargar en suscriptor y permitir cambios del suscriptor**    
4.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  

###  <a name="use-transact-sql"></a>Uso de Transact-SQL  
  
#### <a name="new-article"></a>Nuevo artículo  
  
1.  Ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) y especifique un valor de **1** o **2** para el parámetro `@subscriber_upload_options`. Los números corresponden al comportamiento siguiente:  
  
    -   **0** - Ninguna restricción (valor predeterminado). Los cambios realizados en el suscriptor se cargan en el publicador.    
    -   **1** - Se permiten cambios en el suscriptor, pero no se cargan en el publicador.    
    -   **2** - No se permite realizar cambios en el suscriptor.  
  
       > [!NOTE]  
       > Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de `@subscriber_upload_options` debe ser el mismo para los dos artículos.  
  
#### <a name="existing-article"></a>Artículo existente
  
1.  Para determinar si un artículo es de solo descarga, ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) y compruebe el valor de **upload_options** para el artículo en el conjunto de resultados. 
  
2.  Si el valor devuelto en el paso 1 es **0**, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), y especifique el valor **subscriber_upload_options** para `@property`, un valor de **1** para `@force_invalidate_snapshot` y `@force_reinit_subscription`, y un valor de **1** o **2** para `@value`, lo que se corresponde al comportamiento siguiente:  
  
    -   **1** - Se permiten cambios en el suscriptor, pero no se cargan en el publicador.    
    -   **2** - No se permite realizar cambios en el suscriptor.  
  
        > [!NOTE]  
        >  Si la tabla de origen de un artículo ya está publicada en otra publicación, el comportamiento de solo descarga debe ser el mismo para ambos artículos.  
  
## <a name="interactive-conflict-resolution"></a>Interactive Conflict Resolution 
  
 La replicación de Microsoft SQL Server proporciona un Solucionador interactivo que permite solucionar conflictos manualmente durante la sincronización a petición en el Administrador de sincronización de Microsoft Windows. Una vez habilitada la resolución interactiva, resuelva los conflictos interactivamente durante la sincronización, mediante el Solucionador interactivo. El Solucionador interactivo está disponible a través del Administrador de sincronización de Microsoft Windows. Para más información, vea [Sincronizar una suscripción mediante el Administrador de sincronización de Windows &#40;Administrador de sincronización de Windows&#41;](../../../relational-databases/replication/synchronize-a-subscription-using-windows-synchronization-manager.md).  
  
  
### <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Si se realiza una sincronización fuera del Administrador de sincronización de Windows (como una sincronización programada o una sincronización a petición en SQL Server Management Studio o el Monitor de replicación), los conflictos se resuelven automáticamente sin la intervención del usuario, utilizando la resolución especificada para el artículo. Para obtener más información, consulte [Interactive Conflict Resolution](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-interactive-resolution.md).  
  
### <a name="use-sql-server-management-studio"></a>Usar SQL Server Management Studio  
  
#### <a name="enable-interactive-conflict-resolution-for-an-article"></a>Habilitación de la resolución interactiva de conflictos para un artículo  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla. Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).   
2.  Haga clic en **Propiedades del artículo** y, a continuación, haga clic en **Establecer propiedades del artículo de tabla resaltado** o en **Establecer propiedades de todos los artículos de la tabla**.    
3.  En las páginas **Propiedades del artículo: \<Article>** o **Propiedades del artículo: \<ArticleType>** , haga clic en la pestaña **Resolución**.    
4.  Seleccione **Permitir que el suscriptor resuelva los conflictos de modo interactivo durante la sincronización a petición**.    
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]    
6.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
#### <a name="specify-that-a-subscription-should-use-interactive-conflict-resolution"></a>Especificación de que una suscripción debe usar la resolución interactiva de conflictos  
  
1.  En el cuadro de diálogo **Propiedades de suscripción: \<Subscriber>: \<SubscriptionDatabase>** , especifique un valor de **True** para la opción **Solucionar conflictos de manera interactiva**. Para obtener más información acerca de cómo obtener acceso a este cuadro de diálogo, vea [View and Modify Push Subscription Properties](../../../relational-databases/replication/view-and-modify-push-subscription-properties.md) y [View and Modify Pull Subscription Properties](../../../relational-databases/replication/view-and-modify-pull-subscription-properties.md).   
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
###  <a name="use-transact-sql"></a>Uso de Transact-SQL  
 Puede especificar mediante programación que el suscriptor utilizará esta interfaz gráfica para solucionar conflictos de artículos cuando se cree una suscripción de extracción a una publicación de combinación. Solo se mostrarán en el Solucionador interactivo los conflictos de artículos que admitan esta opción.  
  
#### <a name="create-a-merge-pull-subscription-that-uses-the-interactive-resolver"></a>Creación de una suscripción de extracción de mezcla que use el Solucionador interactivo  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) y especifique `@publication`. Tenga en cuenta el valor de **allow_interactive_resolver** para cada artículo del conjunto de resultados para el que se utilizará el Solucionador interactivo.   
    -   Si este valor es **1**, se utilizará el Solucionador interactivo.    
    -   Si este valor es **0**, debe habilitar primero el Solucionador interactivo para cada artículo. Para ello, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md), y especifique `@publication`, `@article`, un valor de **allow_interactive_resolver** para `@property` y un valor de **true** para `@value`.    
2.  En la base de datos de suscripciones del suscriptor, ejecute [sp_addmergepullsubscription](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-transact-sql.md). Para obtener más información, consulte [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).    
3.  En la base de datos de suscripciones del suscriptor, ejecute [sp_addmergesubscription_agent](../../../relational-databases/system-stored-procedures/sp-addmergepullsubscription-agent-transact-sql.md)y especifique los siguientes parámetros:    
    -   `@publisher`, `@publisher_db` (la base de datos publicada) y `@publication`.    
    -   Un valor de **true** para `@enabled_for_syncmgr`.    
    -   Un valor de **true** para `@use_interactive_resolver`.    
    -   La información de la cuenta de seguridad que necesita el Agente de mezcla. Para obtener más información, consulte [Create a Pull Subscription](../../../relational-databases/replication/create-a-pull-subscription.md).    
4.  En la base de datos de publicación del publicador, ejecute [sp_addmergesubscription](../../../relational-databases/system-stored-procedures/sp-addmergesubscription-transact-sql.md).  
  
#### <a name="define-an-article-that-supports-the-interactive-resolver"></a>Definición de un artículo que admita el Solucionador interactivo  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, un nombre de artículo para `@article`, el objeto de base de datos que se va a publicar para `@source_object` y un valor de **true** para `@allow_interactive_resolver`. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
 
## <a name="conflict-tracking-and-resolution-level-for-merge-articles"></a>Seguimiento de conflictos y el nivel de resolución para los artículos de mezcla
En este tema se describe cómo especificar el seguimiento de conflictos y el nivel de resolución para artículos de mezcla en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 Cuando se sincroniza una suscripción a una publicación de combinación, la replicación comprueba los conflictos producidos por los cambios a los mismos datos realizados en el Publicador y el Suscriptor. Puede especificar si los conflictos se detectan en el nivel de fila, donde cualquier cambio a la fila se considera un conflicto, o en el nivel de columna, donde solo se consideran un conflicto los cambios a la misma fila y columna. La resolución de conflictos para los artículos se realiza en el nivel de fila. Para obtener más información sobre la detección y resolución de conflictos cuando se usan registros lógicos, vea [Detecting and Resolving Conflicts in Logical Records](../../../relational-databases/replication/merge/advanced-merge-replication-conflict-resolving-in-logical-record.md).  
 
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
  
-   Si modifica el nivel de seguimiento después de que se hayan inicializado las suscripciones, se deben reinicializar dichas suscripciones. Para obtener más información sobre los efectos de los cambios de propiedad, vea [Change Publication and Article Properties](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).   
-   Con el seguimiento por columna y por fila, la resolución de conflictos se realiza siempre en el nivel de fila: la fila ganadora sobrescribe la fila perdedora. La replicación de mezcla también le permite especificar que se realice un seguimiento de los conflictos y se resuelvan en el nivel de registro lógico, pero dichas opciones no están disponibles en [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)]. Para obtener información acerca de cómo establecer estas opciones con procedimientos almacenados de replicación, vea [Definir una relación de registros lógicos entre artículos de tabla de mezcla](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md).  
  
### <a name="use-sql-server-management-studio"></a>Usar SQL Server Management Studio  
 Especifique el seguimiento de nivel de columna o fila para los artículos de combinación en la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo**, disponible en el Asistente para nueva publicación y el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="specify-row--or-column-level-tracking"></a>Especificación del seguimiento por fila o columna  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla.  
2.  Haga clic en **Propiedades del artículo** y, a continuación, haga clic en **Establecer propiedades del artículo de tabla resaltado** o en **Establecer propiedades de todos los artículos de la tabla**.   
3.  En la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo \<Article>** , seleccione uno de los valores siguientes para la propiedad **Nivel de seguimiento**: **Seguimiento por filas** o **Seguimiento por columna**.   
4.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
### <a name="use-transact-sql"></a>Uso de Transact-SQL  
  
#### <a name="to-specify-conflict-tracking-options-for-a-new-merge-article"></a>Para especificar las opciones de seguimiento de conflictos para un nuevo artículo de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) y especifique uno de los valores siguientes para `@column_tracking`:  
  
    -   **true** : Use el seguimiento del nivel de columna para el artículo.    
    -   **falso** : Use el seguimiento de nivel de fila, que es el valor predeterminado.  
  
#### <a name="change-conflict-tracking-options-for-a-merge-article"></a>Cambio de las opciones de seguimiento de conflictos para un artículo de mezcla  
  
1.  Para determinar las opciones de seguimiento de conflictos para un artículo de mezcla, ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Tenga en cuenta el valor de la opción **column_tracking** en el conjunto de resultados para el artículo. Un valor de **1** indica que se está usando el seguimiento del nivel de columna y un valor de **0** indica que se está usando el seguimiento de nivel de fila.    
2.  En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de **column_tracking** para `@property` y uno de los valores siguientes para `@value`:  
  
    -   **true** : Use el seguimiento del nivel de columna para el artículo.    
    -   **falso** : Use el seguimiento de nivel de fila, que es el valor predeterminado.  
  
     Especifique un valor de **1** para `@force_invalidate_snapshot` y `@force_reinit_subscription`. 

## <a name="manage-tracking--deletes"></a>Administración del seguimiento de eliminaciones
    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)]  
  
 De forma predeterminada, la replicación de mezcla sincroniza los comandos DELETE entre el Publicador y Suscriptor. La replicación de mezcla le permite conservar filas en la base de datos de suscripciones incluso cuando se han eliminado de la publicación, y viceversa. Puede especificar mediante programación que se omitan los comandos DELETE al crear un nuevo artículo o puede habilitar esta funcionalidad en un momento posterior usando los procedimientos almacenados de replicación.  
  
> [!IMPORTANT]  
>  Al habilitar esta funcionalidad se producirá la no convergencia, lo que significa que los datos presentes en el Suscriptor no reflejarán con precisión los datos en el Publicador. Debe implementar su propio mecanismo para quitar manualmente las filas eliminadas.  
  
### <a name="specify-that-deletes-be-ignored-for-a-new-merge-article"></a>Especificación de la omisión de las eliminaciones para un artículo de mezcla nuevo  
  
En la base de datos de publicación del publicador, ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique un valor de **false** para `@delete_tracking`. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).
  
> [!NOTE]  
>  Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de **delete_tracking** debe ser el mismo en los dos artículos.  
  
### <a name="specify-that-deletes-be-ignored-for-an-existing-merge-article"></a>Especificación de la omisión de las eliminaciones para un artículo de mezcla existente  
  
1.  Para determinar si la compensación de errores está habilitada para un artículo, ejecute [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) y observe el valor de **delete_tracking** en el conjunto de resultados. Si este valor es **0**, ya se están omitiendo las eliminaciones.  
  
2.  Si el valor del paso 1 es **1**, ejecute [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md) en la base de datos de publicación del publicador. Especifique un valor de `delete_tracking` para `@property` y un valor de **false** para `@value`.  
  
    > [!NOTE]  
    >  Si la tabla de origen de un artículo ya está publicada en otra publicación, el valor de **delete_tracking** debe ser el mismo en los dos artículos.  
  

## <a name="processing-order"></a>Orden de procesamiento
  La replicación de mezcla le permite especificar el orden en el que el Agente de mezcla procesa los artículos durante el proceso de sincronización. Al crear cada artículo, puede asignarle un orden mediante programación utilizando los procedimientos almacenados de replicación. Los artículos se procesan en orden desde el valor menor al mayor. Si existen dos artículos que tienen el mismo valor, se procesan al mismo tiempo. 

### <a name="how-processing-order-is-determined"></a>Cómo se determina el orden de procesamiento  
 De manera predeterminada, durante la sincronización de mezcla, los artículos se procesan en el orden requerido por las dependencias entre objetos, incluidas las restricciones de integridad referencial declarativa (DRI) definidas en las tablas base. El procesamiento implica enumerar los cambios de una tabla y aplicar esos cambios a continuación. Si no hay DRI presente pero hay filtros de combinación o registros lógicos entre los artículos de la tabla, los artículos se procesan en el orden que requieran los filtros y los registros lógicos. Los artículos que no estén relacionados con ningún otro artículo mediante DRI, filtros de combinación, registros lógicos u otras dependencias, se procesan según el alias del artículo de la tabla de sistema [sysmergearticles &#40;Transact-SQL&#41;](../../../relational-databases/system-tables/sysmergearticles-transact-sql.md).  
  
 Imagine una publicación que incluye las tablas **SalesOrderHeader** y **SalesOrderDetail** con una columna de clave principal **SalesOrderID** en la tabla **SalesOrderHeader** y una columna de clave externa correspondiente **SalesOrderID** en la tabla **SalesOrderDetail** . Durante la sincronización, la replicación de mezcla impide infracciones de clave externa insertando cualquier fila nueva en **SalesOrderHeader** antes de insertar filas asociadas en **SalesOrderDetail**. De forma similar, las filas se eliminan de **SalesOrderDetail** antes de que la fila asociada se elimine de **SalesOrderHeader**.  
  
 Sin embargo, en algunas aplicaciones, la integridad referencial se ve reforzada mediante desencadenadores de base de datos, o en el nivel de aplicación, y no mediante DRI. Si tomamos la publicación descrita más arriba, en vez de DRI, la tabla **SalesOrderDetail** podría tener un desencadenador de inserción que asegure que la fila asociada en la tabla **SalesOrderHeader** exista antes de permitir una inserción. **SalesOrderHeader** podría tener un desencadenador de eliminación que asegure que no haya filas asociadas en **SalesOrderDetail** antes de permitir una eliminación. La replicación de mezcla no tiene en cuenta los desencadenadores cuando determina el orden de procesamiento de artículos porque no puede determinar los resultados del desencadenador hasta que se haya activado. De forma similar, la replicación no puede tener en cuenta las restricciones definidas en el nivel de aplicación.  
  
 Cuando la integridad referencial se mantiene a través de los desencadenadores o en el nivel de aplicación, debe especificar el orden en el que se deben procesar los artículos. En el ejemplo con desencadenadores, debería especificar que la tabla **SalesOrderHeader** se debe procesar antes que **SalesOrderDetail**, puesto que el orden de los artículos se basa en el orden de inserción. La replicación de mezcla invertirá automáticamente el orden de las eliminaciones. La replicación de mezcla no producirá ningún error sin el orden de los artículos, porque el Agente de mezcla sigue procesando artículos si se produce una infracción de restricción. Lo que hace es reintentar las operaciones que hayan dado error cuando los otros artículos se hayan terminado de procesar. Especificar el orden de los artículos únicamente impide los reintentos y los procesos adicionales asociados con ellos. Si especifica un orden incorrecto (por ejemplo, uno que dé como resultado el procesamiento de los registros de detalle antes que el de los registros de encabezado), la replicación de mezcla volverá a realizar el proceso hasta que lo consiga.   
  
### <a name="new-article"></a>Nuevo artículo
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique un valor entero que represente el orden de procesamiento del artículo para `@processing_order`. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
    > [!NOTE]  
    >  Al crear artículos ordenados, debería dejar huecos entre los valores de orden de los artículos. Esto le permitirá establecer nuevos valores en el futuro con mayor facilidad. Por ejemplo, si tiene tres artículos para los que necesita especificar un orden de procesamiento fijo, establezca el valor `@processing_order` en 10, 20 y 30 en lugar de 1, 2 y 3, respectivamente.  
  
### <a name="existing-article"></a>Artículo existente
  
1.  Para determinar orden de procesamiento de un artículo, ejecute [sp_helpmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md) y tenga en cuenta el valor de **processing_order** en el conjunto de resultados.   
2.  En el publicador de la base de datos de publicación, ejecute [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique el valor **processing_order** para `@property` y un valor entero que represente el orden de procesamiento para `@value`.  


## <a name="see-also"></a>Consulte también  
 [Optimizar el rendimiento de la replicación de mezcla con artículos de solo descarga](../../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md)   
 [Define an Article](../../../relational-databases/replication/publish/define-an-article.md)   
 [er y modificar las propiedades de un artículo](../../../relational-databases/replication/publish/view-and-modify-article-properties.md)  
  
  
