---
description: Especificar opciones de esquema para la replicación de SQL Server
title: Especificar opciones de esquema para la replicación de SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- schemas [SQL Server replication], options
- articles [SQL Server replication], transactional replication options
- articles [SQL Server replication], merge replication options
- articles [SQL Server replication], schema options
ms.assetid: 1f85a479-bd6e-4023-abf7-7435a7e5b567
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 53c7c3b803a16fd7ee95f35f33ee2439deb86199
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076623"
---
# <a name="specify-schema-options-for-sql-server-replication"></a>Especificar opciones de esquema para la replicación de SQL Server
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo especificar las opciones de esquema en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. Al publicar una tabla o una vista, puede controlar las opciones de creación de objetos que se replican para el objeto publicado. Puede establecer estas opciones cuando se haya creado el artículo y también puede modificarlas posteriormente. Si no especifican explícitamente estas opciones para un artículo, se definirá un conjunto predeterminado de opciones.  
  
> [!NOTE]  
>  Las opciones de esquema predeterminadas cuando se usan procedimientos almacenados de replicación pueden diferir de las opciones predeterminadas cuando los artículos se agregan mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para especificar las opciones del esquema con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si se cambian opciones de esquema después de crear una publicación, se debe generar una nueva instantánea.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Para obtener la lista completa de opciones de esquema, vea el parámetro `@schema_option` de [sp_addarticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) y [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Especifique las opciones de los esquemas como, por ejemplo, si quiere copiar las restricciones y los desencadenadores para los Suscriptores, en la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** . Esta pestaña está disponible en el Asistente para nueva publicación y en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-specify-schema-options"></a>Para especificar las opciones del esquema  
  
1.  En la página **Artículos** del Asistente para nueva publicación o en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un artículo y, luego, haga clic en **Propiedades del artículo**.  
  
2.  Seleccione qué cambios de opción de esquema de artículos se deben aplicar:  
  
    -   Haga clic en **Establecer propiedades del artículo de \<ObjectType> resaltado** para iniciar el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** ; los cambios de propiedad realizados en este cuadro de diálogo solo se aplican al objeto que está resaltado en el panel de objetos de la página **Artículos**.  
  
    -   Haga clic en **Establecer propiedades de todos los artículos de \<ObjectType>** para iniciar el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** . Los cambios de propiedad realizados en este cuadro de diálogo se aplican a todos los objetos de ese tipo en el panel de objetos de la página **Artículos**, incluidos los que todavía no se hayan seleccionado para la publicación.  
  
        > [!NOTE]  
        >  Los cambios de propiedades realizados en el cuadro de diálogo **Propiedades de todos los artículos de \<ObjectType>** reemplazan los que se hicieran anteriormente en el cuadro de diálogo **Propiedades del artículo: \<ObjectName>** . Por ejemplo, si desea establecer varios valores predeterminados para todos los artículos de un tipo de objeto, pero solamente desea establecer algunas propiedades para objetos individuales, establezca primero los valores predeterminados para todos los artículos. A continuación, establezca las propiedades de los objetos individuales.  
  
3.  En las secciones **Copiar objetos y configuración en el suscriptor** y **Objeto de destino** de la pestaña **Propiedades** del cuadro de diálogo **Propiedades del artículo: \<Article>** , especifique los valores de las opciones.  
  
4.  Modifique las propiedades si es necesario y, a continuación, haga clic en **Aceptar**.  
  
5.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  

##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Las opciones de esquema se especifican como un valor hexadecimal que es el resultado [| (OR bit a bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) de una o más opciones. Para obtener más información, vea [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) y [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).  
  
> [!NOTE]  
>  Debe convertir los valores de opción de esquema de **binary** a **int** antes de realizar una operación bit a bit. Para obtener más información, vea [CAST y CONVERT &#40;Transact-SQL&#41;](../../../t-sql/functions/cast-and-convert-transact-sql.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-snapshot-or-transactional-publication"></a>Para especificar las opciones de esquema al definir un artículo para una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addarticle](../../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, un nombre de artículo para `@article`, el objeto de base de datos que se publica para `@source_object`, el tipo de objeto de base de datos para `@type` y el resultado [| (Bitwise OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) de una o más opciones de esquema para `@schema_option`. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-specify-schema-options-when-defining-an-article-for-a-merge-publication"></a>Para especificar las opciones de esquema al definir un artículo para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, un nombre de artículo para `@article`, el objeto de base de datos que se publica para `@source_object`, y el resultado [| (Bitwise OR)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) de una o más opciones de esquema para `@schema_option`. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-snapshot-or-transactional-publication"></a>Para cambiar las opciones de esquema para un artículo existente en una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_helparticle](../../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication` y el nombre de artículo para `@article`. Observe el valor de la columna `schema_option` en el conjunto de resultados.  
  
2.  Para determinar si la opción está o no establecida, ejecute una operación [& (AND bit a bit)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) con el valor del paso 1 y el valor de opción de esquema pertinente.  
  
    -   Si el resultado es **0**, la opción no está establecida.  
  
    -   Si el resultado es el valor de opción, ésta ya está establecida.  
  
3.  Si la opción no está establecida, ejecute una operación [| (OR bit a bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) con el valor del paso 1 y el valor de opción de esquema deseado.  
  
4.  En la base de datos de publicación del publicador, ejecute [sp_changearticle](../../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, el nombre del artículo para `@article`, un valor de `schema_option` para `@property` y el resultado hexadecimal del paso 3 para `@value`.  
  
5.  Ejecute el Agente de instantáneas para generar una nueva instantánea. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
#### <a name="to-change-schema-options-for-an-existing-article-in-a-merge-publication"></a>Para cambiar las opciones de esquema de un artículo existente en una publicación de mezcla  
  
1.  En el publicador de la base de datos de publicación, ejecute [sp_helpmergearticle](../../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication` y el nombre de artículo para `@article`. Tenga en cuenta el valor de la columna de **schema_option** en el conjunto de resultados.  
  
2.  Para determinar si la opción está o no establecida, ejecute una operación [& (AND bit a bit)](../../../t-sql/language-elements/bitwise-and-transact-sql.md) con el valor del paso 1 y el valor de opción de esquema pertinente.  
  
    -   Si el resultado es **0**, la opción no está establecida.  
  
    -   Si el resultado es el valor de opción, ésta ya está establecida.  
  
3.  Si la opción no está establecida, ejecute una operación [| (OR bit a bit)](../../../t-sql/language-elements/bitwise-or-transact-sql.md) con el valor del paso 1 y el valor de opción de esquema deseado.  
  
4.  En la base de datos de publicación del publicador, ejecute [sp_changemergearticle](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique el nombre de la publicación a la que pertenece el artículo para `@publication`, el nombre del artículo para `@article`, un valor de `schema_option` para `@property` y el resultado hexadecimal del paso 3 para `@value`.  
  
5.  Ejecute el Agente de instantáneas para generar una nueva instantánea. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
## <a name="see-also"></a>Consulte también  
 [Publicar datos y objetos de base de datos](../../../relational-databases/replication/publish/publish-data-and-database-objects.md)   
 [Article Options for Transactional Replication](../../../relational-databases/replication/transactional/article-options-for-transactional-replication.md)  
  
  
