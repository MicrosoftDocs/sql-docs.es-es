---
description: Optimizar los filtros de fila con parámetros
title: Optimización de los filtros de fila con parámetros | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- precomputed partitions [SQL Server replication]
- filters [SQL Server replication], parameterized
- merge replication precomputed partitions [SQL Server replication], SQL Server Management Studio
- parameterized filters [SQL Server replication], optimizing
ms.assetid: 49349605-ebd0-4757-95be-c0447f30ba13
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: c8b58c7cf7ffe2b56feb8f59df9b0adef857ab02
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076663"
---
# <a name="optimize-parameterized-row-filters"></a>Optimizar los filtros de fila con parámetros
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo optimizar los filtros de fila con parámetros en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para optimizar los filtros de fila con parámetros con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Al usar los filtros con parámetros, puede controlar cómo se procesan los filtros por la replicación de mezcla especificando la opción **use partition groups** o la opción **keep partition changes** al crear una publicación. Estas opciones mejoran el rendimiento de la sincronización para las publicaciones con artículos filtrados almacenando los metadatos adicionales en la base de datos de publicación. Puede controlar cómo se comparten los datos entre los Suscriptores estableciendo **partition options** al crear un artículo. Para obtener más información acerca de estos requisitos, vea [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
     Con suscriptores de [!INCLUDE[ssEW](../../../includes/ssew-md.md)]SQL Server Compact, keep_partition_changes se debe establecer en true para asegurarse de que las eliminaciones se propagan correctamente. Si se establece en false, el suscriptor puede tener más filas de las esperadas.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Para optimizar los filtros de fila con parámetros se puede utilizar la siguiente configuración:  
  
 **Partition Options**  
 Establezca esta opción en la página **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** o en el cuadro **Agregar filtro**. Ambos cuadros de diálogo están disponibles en el Asistente para nueva publicación y el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . El cuadro de diálogo **Propiedades del artículo: \<Article>** permite especificar valores adicionales para esta opción que no están disponibles en el cuadro de diálogo **Agregar filtro**.  
  
 **Calcular particiones previamente**  
 Esta opción se establece en **True** de forma predeterminada si los artículos de la publicación cumplen un conjunto de requisitos. Para obtener más información sobre estos requisitos, vea [Optimizar el rendimiento de los filtros con parámetros con particiones calculadas previamente](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md). Modifique esta opción en la página **Opciones de suscripción** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** .  
  
 **Optimizar sincronización**  
 Esta opción debe establecerse en **True** solamente si **Calcular particiones previamente** se establece en **False**. Establezca esta opción en la página **Opciones de suscripción** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** .  
  
 Para obtener más información sobre el uso del Asistente para nueva publicación y el acceso al cuadro de diálogo **Propiedades de la publicación: \<Publication>** , vea [Crear una publicación](../../../relational-databases/replication/publish/create-a-publication.md) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-set-partition-options-in-the-add-filter-or-edit-filter-dialog-box"></a>Para establecer opciones de partición en el cuadro de diálogo Agregar filtro o Editar filtro  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Agregar** y en **Agregar filtro**.  
  
2.  Cree un filtro con parámetros. Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
3.  Seleccione la opción que indique cómo se van a compartir los datos entre los suscriptores:  
  
    -   **Una fila de esta tabla irá a varias suscripciones**  
  
    -   **Una fila de esta tabla irá a una sola suscripción**  
  
     Si selecciona **Una fila de esta tabla irá a una sola suscripción**, la replicación de mezcla puede optimizar el rendimiento almacenando y procesando menos metadatos. No obstante, debe asegurarse de que los datos se particionan de forma que una fila no se pueda replicar en más de un suscriptor. Para obtener más información, vea la sección sobre cómo configurar opciones de partición en el tema [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
#### <a name="to-set-partition-options-in-the-article-properties---article-dialog-box"></a>Para establecer Opciones de partición en el cuadro de diálogo Propiedades del artículo: \<Article>  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione una tabla y haga clic en **Propiedades del artículo**.  
  
2.  Haga clic en **Establecer propiedades del artículo de Tabla resaltado** o **Establecer propiedades de todos los artículos de la tabla**.  
  
3.  En la sección **Objeto de destino** de la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , especifique uno de los siguientes valores para **Opciones de partición**:  
  
    -   **Superpuestas**  
  
    -   **Superpuestas, no permitir cambios de datos fuera de la partición**  
  
    -   **No superpuestas, una sola suscripción**  
  
    -   **No superpuestas, compartir entre suscripciones**  
  
     Para obtener más información acerca de estas opciones y cómo se relacionan con las opciones disponibles en los cuadros de diálogo **Agregar filtro** y **Editar filtro** , vea la sección sobre cómo establecer opciones de partición en el tema [Parameterized Row Filters](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md).  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  
  
#### <a name="to-set-precompute-partitions"></a>Para establecer Calcular particiones previamente  
  
1.  En la página **Opciones de suscripción** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un valor para la opción **Calcular particiones previamente**. La propiedad es de solo lectura si:  
  
    -   La publicación no cumple los requisitos de las particiones precalculadas.  
  
    -   No se ha generado una instantánea para la publicación. En este caso, la opción muestra el valor **Establecer automáticamente cuando se crea una instantánea**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-set-optimize-synchronization"></a>Para establecer Optimizar sincronización  
  
1.  En la página **Opciones de suscripción** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un valor de `True` para la opción **Optimizar la sincronización**.  
  
2.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Para las definiciones de las opciones de filtrado para `@keep_partition_changes` y `@use_partition_groups`, vea [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md).  
  
#### <a name="to-specify-merge-filter-optimizations-when-creating-a-new-publication"></a>Para especificar las optimizaciones de filtro de mezcla al crear una nueva publicación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergepublication](../../../relational-databases/system-stored-procedures/sp-addmergepublication-transact-sql.md). Especifique `@publication` y un valor de `true` para uno de los parámetros siguientes:  
  
    -   `@use_partition_groups`: la optimización de mayor rendimiento, siempre que los artículos cumplan los requisitos para las particiones precalculadas. Para obtener más información, vea [Optimización del rendimiento de los filtros con parámetros con particiones calculadas previamente](../../../relational-databases/replication/merge/parameterized-filters-optimize-for-precomputed-partitions.md).  
  
    -   `@keep_partition_changes`: use esta optimización si no se pueden usar particiones precalculadas.  
  
2.  Agregue un trabajo de instantánea para la publicación. Para obtener más información, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md).  
  
3.  En el publicador de la base de datos de publicación, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)y especifique los siguientes parámetros:  
  
    -   `@publication`: el nombre de la publicación del paso 1.  
  
    -   `@article`: un nombre para el artículo.  
  
    -   `@source_object`: el objeto de base de datos que se va a publicar.  
  
    -   `@subset_filterclause`: la cláusula de filtro con parámetros opcional que se usa para filtrar horizontalmente el artículo.  
  
    -   `@partition_options`: las opciones de partición para el artículo filtrado.  
  
4.  Repita el paso 3 para cada artículo de la publicación.  
  
5.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_addmergefilter](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md) para definir un filtro de combinación entre dos artículos. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
#### <a name="to-view-and-modify-merge-filter-behaviors-for-an-existing-publication"></a>Para ver y modificar los comportamientos de filtro de mezcla para una publicación existente  
  
1.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_helpmergepublication](../../../relational-databases/system-stored-procedures/sp-helpmergepublication-transact-sql.md) y especifique `@publication`. Observe el valor de `keep_partition_changes` y `use_partition_groups` en el conjunto de resultados.  
  
2.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Especifique un valor de `use_partition_groups` para `@property` y `true` o `false` para `@value`.  
  
3.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_changemergepublication](../../../relational-databases/system-stored-procedures/sp-changemergepublication-transact-sql.md). Especifique un valor de `keep_partition_changes` para `@property` y `true` o `false` para `@value`.  
  
    > [!NOTE]  
    >  Al habilitar `keep_partition_changes`, primero debe deshabilitar `use_partition_groups` y especificar un valor de `1` para `@force_reinit_subscription`.  
  
4.  (Opcional) En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique un valor de `partition_options` para `@property` y el valor adecuado para `@value`. Vea [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para las definiciones de estas opciones de filtrado.  
  
5.  (Opcional) Inicie el Agente de instantáneas para regenerar la instantánea si es necesario. Para obtener información sobre qué cambios exigen la generación de una nueva instantánea, vea [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md).  
  
## <a name="see-also"></a>Consulte también  
 [Generar automáticamente un conjunto de filtros de combinación entre artículos de mezcla &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md)   
 [Definición y modificación de un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtros de fila con parámetros](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)  
  
  
