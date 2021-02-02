---
description: Definir y modificar un filtro de fila estático
title: Definición y modificación de un filtro de fila estático | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- modifying filters, static row
- static row filters
- filters [SQL Server replication], static row
ms.assetid: a6ebb026-026f-4c39-b6a9-b9998c3babab
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 20632762a98c264deff09182f5f24a0602ab8d6c
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076773"
---
# <a name="define-and-modify-a-static-row-filter"></a>Definir y modificar un filtro de fila estático
[!INCLUDE[sql-asdbmi](../../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo definir y modificar un filtro de fila estático en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para definir y modificar un filtro de fila estático con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Si agrega, modifica o elimina un filtro de fila estático una vez inicializadas las suscripciones a la publicación, deberá generar una instantánea nueva y reinicializar todas las suscripciones después de realizar el cambio. Para obtener más información sobre los requisitos para los cambios de propiedad, consulte [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
-   Si la publicación está habilitada para la replicación transaccional punto a punto, no se pueden filtrar las tablas.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Dado que estos filtros son estáticos, todos los suscriptores recibirán el mismo subconjunto de los datos. Si necesita filtrar dinámicamente las filas en un artículo de la tabla que pertenece a una publicación de combinación para que cada suscriptor reciba una partición diferente de los datos, vea [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md). La replicación de mezcla también permite filtrar filas relacionadas con un filtro de filas existente. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Defina, modifique y elimine filtros de filtro de fila estático en la página **Filtrar filas de tabla** del Asistente para nueva publicación o en la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-static-row-filter"></a>Para definir un filtro de fila estático  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o en la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , la acción que realice depende del tipo de publicación:  
  
    -   Para una publicación de instantáneas o transaccional, haga clic en **Agregar**.  
  
    -   Para una publicación de combinación, haga clic en **Agregar** y, a continuación, en **Agregar filtro**.  
  
2.  En el cuadro de diálogo **Agregar filtro** , seleccione la tabla que va a filtrar en el cuadro de lista desplegable.  
  
3.  Cree una instrucción de filtro en el área de texto **Instrucción de filtro** . Puede escribir directamente en el área de texto o puede arrastrar y colocar columnas del cuadro de lista **Columnas** .  
  
    > [!NOTE]  
    >  La cláusula WHERE debe usar nombres de dos partes; los nombres de tres y cuatro partes no se admiten. Si la publicación es de un publicador de Oracle, la cláusula WHERE debe seguir la sintaxis de Oracle.  
  
    -   El área de texto **Instrucción de filtro** incluye el texto predeterminado, que tiene este formato:  
  
        ```sql  
        SELECT <published_columns> FROM [schema].[tablename] WHERE  
        ```  
  
    -   El texto predeterminado no se puede cambiar. Escriba la cláusula del filtro después de la palabra clave WHERE utilizando la sintaxis SQL estándar. La cláusula del filtro completa será similar a la siguiente:  
  
        ```sql  
        SELECT <published_columns> FROM [HumanResources].[Employee] WHERE [LoginID] = 'adventure-works\ranjit0'  
        ```  
  
    -   Un filtro de fila estático puede incluir una función definida por el usuario. La cláusula del filtro completa para un filtro de fila estático con una función definida por el usuario será similar a la siguiente:  
  
        ```sql  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] WHERE MyFunction([Freight]) > 100  
        ```  
  
4.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
5.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  

#### <a name="to-modify-a-static-row-filter"></a>Para modificar un filtro de fila estático  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un filtro en el panel **Tablas filtradas** y luego haga clic en **Editar**.  
  
2.  Modifique el filtro en el cuadro de diálogo **Editar filtro** .  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-static-row-filter"></a>Para eliminar un filtro de fila estático  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** , seleccione un filtro en el panel **Tablas filtradas** y luego haga clic en **Eliminar**.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Al crear los artículos de la tabla, puede definir una cláusula WHERE para filtrar las filas de un artículo. También puede cambiar un filtro de fila una vez definido. Los filtros de fila estáticos se pueden crear y modificar mediante programación con los procedimientos almacenados de la replicación.  
  
#### <a name="to-define-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Para definir un filtro de fila estático para una publicación transaccional o de instantáneas  
  
1.  Defina el artículo que se va a filtrar. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Especifique el nombre del artículo para **\@article**, el nombre de la publicación para **\@publication**, un nombre de filtro para **\@filter_name** y la cláusula de filtrado para **\@filter_clause** (sin incluir `WHERE`).  
  
3.  Si todavía debe definirse un filtro de columna, vea [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md). De lo contrario, ejecute [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Especifique el nombre de la publicación para **\@publication**, el nombre del artículo filtrado para **\@article** y la cláusula de filtro especificada en el paso 2 para **\@filter_clause**. Esto crea los objetos de sincronización para el artículo filtrado.  
  
#### <a name="to-modify-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Para modificar un filtro de fila estático para una publicación transaccional o de instantáneas  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Especifique el nombre del artículo para **\@article**, el nombre de la publicación para **\@publication**, un nombre de filtro nuevo para **\@filter_name** y la cláusula de filtrado nueva para **\@filter_clause** (sin incluir `WHERE`). Como este cambio invalidará los datos en las suscripciones existentes, especifique un valor de **1** para **\@force_reinit_subscription**.  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_articleview &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md). Especifique el nombre de la publicación para **\@publication**, el nombre del artículo filtrado para **\@article** y la cláusula de filtro especificada en el paso 1 para **\@filter_clause**. Esto vuelve a crear la vista que define el artículo filtrado.  
  
3.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
4.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-delete-a-static-row-filter-for-a-snapshot-or-transactional-publication"></a>Para eliminar un filtro de fila estático para una instantánea o una publicación transaccional  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_articlefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md). Especifique el nombre del artículo para **\@article**, el nombre de la publicación para **\@publication**, un valor de NULL para **\@filter_name** y un valor de NULL para **\@filter_clause**. Como este cambio invalidará los datos en las suscripciones existentes, especifique un valor de **1** para **\@force_reinit_subscription**.  
  
2.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
#### <a name="to-define-a-static-row-filter-for-a-merge-publication"></a>Para definir un filtro de fila estático para una publicación de combinación  
  
1.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md). Especifique la cláusula de filtrado para **\@subset_filterclause** (sin incluir `WHERE`). Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
2.  Si todavía debe definirse un filtro de columna, vea [Define and Modify a Column Filter](../../../relational-databases/replication/publish/define-and-modify-a-column-filter.md).  
  
#### <a name="to-modify-a-static-row-filter-for-a-merge-publication"></a>Para modificar un filtro de fila estático para una publicación de combinación  
  
1.  En el publicador de la base de datos de publicación, ejecute [sp_changemergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md). Especifique el nombre de la publicación para **\@publication**, el nombre del artículo filtrado para **\@article**, un valor de **subset_filterclause** para **\@property** y la nueva cláusula de filtrado para **\@value** (sin incluir `WHERE`). Como este cambio invalidará los datos en las suscripciones existentes, especifique un valor de 1 para **\@force_reinit_subscription**.  
  
2.  Vuelva a ejecutar el trabajo del Agente de instantáneas para que la publicación genere una instantánea actualizada. Para más información, consulte [Crear y aplicar la instantánea inicial](../../../relational-databases/replication/create-and-apply-the-initial-snapshot.md).  
  
3.  Reinicialice las suscripciones. Para obtener más información, vea [Reinicializar suscripciones](../../../relational-databases/replication/reinitialize-subscriptions.md).  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En este ejemplo de la replicación transaccional, el artículo se filtra horizontalmente para quitar todos los productos desusados.  
  
 [!code-sql[HowTo#sp_AddTranArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_1.sql)]  
  
 En este ejemplo de replicación de mezcla, los artículos se filtran horizontalmente para devolver solo las filas que pertenecen al vendedor especificado. También se usa un filtro de combinación. Para obtener más información, consulte [Definir y modificar un filtro de combinación entre artículos de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md).  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-stat_2.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Definición y modificación de un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrar datos publicados](../../../relational-databases/replication/publish/filter-published-data.md)   
 [Filtrar datos publicados para la replicación de mezcla](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)  
  
  
