---
title: Definición y modificación de un filtro de combinación entre artículos de combinación
description: Obtenga información sobre cómo definir y modificar el filtro de combinación usado entre los artículos de combinación mediante SQL Server Management Studio (SSMS) o Transact-SQL (T-SQL).
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- filters [SQL Server replication], join
- merge replication join filters [SQL Server replication]
- modifying filters, join
- join filters
ms.assetid: f7f23415-43ff-40f5-b3e0-0be1d148ee5b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 4ef84f22bf255b8dcb21ba371de5c3045da3ca6b
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076753"
---
# <a name="define-and-modify-a-join-filter-between-merge-articles"></a>Definir y modificar un filtro de combinación entre artículos de mezcla
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema, se describe cómo definir y modificar un filtro de combinación entre artículos de mezcla en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../../includes/tsql-md.md)]. La replicación de mezcla admite filtros de combinación, que se usan normalmente junto con los filtros con parámetros para extender la partición de tabla a otros artículos de tabla relacionados.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Recomendaciones](#Recommendations)  
  
-   **Para definir y modificar un filtro de combinación entre artículos de mezcla con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Para crear un filtro de combinación, una publicación debe contener dos tablas relacionadas, como mínimo. Un filtro de combinación amplía un filtro de fila. Por tanto, debe definir un filtro de fila en una tabla para poder extender el filtro con una combinación a otra. Una vez definido un filtro de combinación, puede extenderlo con otro si la publicación contiene más tablas relacionadas.  
  
-   Si agrega, modifica o elimina un filtro de combinación una vez inicializadas las suscripciones a la publicación, deberá generar una instantánea nueva y reinicializar todas las suscripciones después de realizar el cambio. Para obtener más información sobre los requisitos para los cambios de propiedad, consulte [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md) (Cambiar las propiedades de la publicación y de los artículos).  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
  
-   Los filtros de combinación se deben crear manualmente para un conjunto de tablas, o la replicación puede generar los filtros automáticamente en función de las relaciones entre las claves externas y las claves principales definidas en las tablas. Para obtener más información sobre cómo generar un conjunto de filtros de combinación automáticamente, consulte [Generar automáticamente un conjunto de filtros de combinación entre artículos de mezcla &#40;SQL Server Management Studio&#41;](../../../relational-databases/replication/publish/automatically-generate-join-filters-between-merge-articles.md).  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Defina, modifique y elimine filtros de combinación en la página **Filtrar filas de tabla** del Asistente para nueva publicación o en la página **Filtrar filas** del cuadro de diálogo **Propiedades de la publicación: \<Publication>** . Para obtener más información sobre el uso del asistente y el acceso al cuadro de diálogo, consulte [Create a Publication](../../../relational-databases/replication/publish/create-a-publication.md) (Crear una publicación) y [Ver y modificar propiedades de publicación](../../../relational-databases/replication/publish/view-and-modify-publication-properties.md).  
  
#### <a name="to-define-a-join-filter"></a>Para definir un filtro de combinación  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o la página **Filtrar filas** de **Propiedades de la publicación: \<Publication>** , seleccione un filtro de fila o un filtro de combinación existentes en el panel **Tablas filtradas**.  
  
2.  Haga clic en **Agregar** y, a continuación, en **Agregar combinación para ampliar el filtro seleccionado**.  
  
3.  Cree la instrucción de combinación: seleccione **Usar generador de instrucciones para crear la instrucción** o **Escribir instrucción de combinación manualmente**.  
  
    -   Si selecciona utilizar el generador, utilice las columnas de la cuadrícula (**Conjunción**, **Columna de tabla filtrada**, **Operador** y **Columna de tabla combinada**) para generar una instrucción de combinación.  
  
         Cada una de las columnas de la cuadrícula contiene un cuadro combinado desplegable que le permite seleccionar dos columnas y un operador ( **=** , **<>** , **<=** , **\<**, **>=** , **>** y **like**). Los resultados se muestran en el área de texto **Vista previa** . Si la combinación implica más de un par de columnas, seleccione una conjunción (AND u OR) de la columna **Conjunción** y, a continuación, especifique dos columnas más y un operador.  
  
    -   Si selecciona escribir la instrucción manualmente, escriba la instrucción de combinación en el área de texto **Instrucción de combinación** . Utilice los cuadros de lista **Columnas de la tabla filtrada** y **Columnas de la tabla combinada** para arrastrar y colocar columnas en el área de texto **Instrucción de combinación** .  
  
    -   La instrucción de combinación completa aparecerá así:  
  
        ```  
        SELECT <published_columns> FROM [Sales].[SalesOrderHeader] INNER JOIN [Sales].[SalesOrderDetail] ON [SalesOrderHeader].[SalesOrderID] = [SalesOrderDetail].[SalesOrderID]  
        ```  
  
         La cláusula JOIN debe utilizar nombres de dos partes; los nombres de tres o cuatro partes no se permiten.  
  
4.  Especifique las opciones de combinación:  
  
    -   Si la columna en la que combina la tabla filtrada (la tabla principal) es exclusiva, seleccione **Clave única**.  
  
        > [!CAUTION]  
        >  Al seleccionar esta opción indica que la relación entre la tabla principal y la secundaria de un filtro de combinación es de uno a uno o de uno a varios. Seleccione esta opción únicamente si tiene una restricción en la columna combinada en la tabla secundaria que garantiza la exclusividad. Si la opción no se establece correctamente, se podría producir la no convergencia de datos.  
  
    -   De manera predeterminada, la replicación de mezcla procesa los cambios fila a fila durante la sincronización. Para que los cambios relacionados de las filas de la tabla filtrada y de la tabla combinada se procesen como una unidad, seleccione **Registro lógico** ([!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] y versiones posteriores). Esta opción solo está disponible si se cumplen los requisitos del artículo y de la publicación para utilizar registros lógicos. Para obtener más información, vea la sección sobre consideraciones para utilizar registros lógicos en el tema [Agrupar cambios en filas relacionadas con registros lógicos](../../../relational-databases/replication/merge/group-changes-to-related-rows-with-logical-records.md).  
  
5.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
6.  Si está en el cuadro de diálogo **Propiedades de la publicación: \<Publication>** , haga clic en **Aceptar** para guardar y cerrar el cuadro de diálogo.  

#### <a name="to-modify-a-join-filter"></a>Para modificar un filtro de combinación  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o en la página **Filtrar filas** de **Propiedades de la publicación: \<Publication>** , seleccione un filtro en el panel **Tablas filtradas** y luego haga clic en **Editar**.  
  
2.  Modifique el filtro en el cuadro de diálogo **Editar combinación** .  
  
3.  [!INCLUDE[clickOK](../../../includes/clickok-md.md)]  
  
#### <a name="to-delete-a-join-filter"></a>Para eliminar un filtro de combinación  
  
1.  En la página **Filtrar filas de tabla** del Asistente para nueva publicación o en la página **Filtrar filas** de **Propiedades de la publicación: \<Publication>** , seleccione un filtro en el panel **Tablas filtradas** y luego haga clic en **Eliminar**. Si el filtro de combinación que elimina está a su vez ampliado por otras combinaciones, esas combinaciones también se eliminarán.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Estos procedimientos muestran un filtro con parámetros en un artículo primario con filtros de combinación entre este artículo y los artículos secundarios relacionados. Los filtros de combinación se pueden definir y modificar mediante programación con los procedimientos almacenados de la replicación.  
  
#### <a name="to-define-a-join-filter-to-extend-an-article-filter-to-related-articles-in-a-merge-publication"></a>Para definir un filtro de combinación para extender un filtro de artículo a los artículos relacionados en una publicación de combinación  
  
1.  Defina el filtrado para el artículo con el que se combina, conocido también como artículo primario.  
  
    -   Para obtener más información acerca de los artículos filtrados mediante un filtro de fila con parámetros, vea [Define and Modify a Parameterized Row Filter for a Merge Article](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
    -   Para obtener más información acerca de los artículos filtrados mediante un filtro de fila estático, vea [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md).  
  
2.  En la base de datos de publicación del publicador, ejecute [sp_addmergearticle &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md) para definir uno o más artículos relacionados, también conocidos como artículos secundarios, para la publicación. Para más información, consulte [Define an Article](../../../relational-databases/replication/publish/define-an-article.md).  
  
3.  En la base de datos de publicación del publicador, ejecute [sp_addmergefilter &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-addmergefilter-transact-sql.md). Especifique `@publication`, un nombre único de este filtro para `@filtername`, el nombre del artículo secundario creado en el paso 2 para `@article`, el nombre del artículo primario con el que se combina para `@join_articlename` y uno de los valores siguientes para `@join_unique_key`:  
  
    -   **0** - indica una combinación de varios a uno o de varios a varios entre los artículos primarios y secundarios.  
  
    -   **1** - indica una combinación de uno a uno o de uno a varios entre los artículos primarios y secundarios.  
  
     Esto define un filtro de combinación entre los dos artículos.  
  
    > [!CAUTION]  
    >  Establezca `@join_unique_key` en **1** solo si tiene una restricción en la columna de combinación de la tabla subyacente del artículo primario que garantice su unicidad. Si `@join_unique_key` se establece en **1** de manera incorrecta, se puede producir una no convergencia de datos.  
  
###  <a name="examples-transact-sql"></a><a name="TsqlExample"></a> Ejemplos (Transact-SQL)  
 En este ejemplo se define un artículo para una publicación de combinación, donde el artículo de tabla `SalesOrderDetail` se filtra respecto a la tabla `SalesOrderHeader` que se filtra a su vez con un filtro de fila estático. Para más información, consulte [Define and Modify a Static Row Filter](../../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md).  
  
 [!code-sql[HowTo#sp_AddMergeArticle](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_1.sql)]  
  
 En este ejemplo se define un grupo de artículos en una publicación de combinación, donde los artículos se filtran con una serie de filtros de combinación respecto a la tabla `Employee` , que se filtra a su vez mediante un filtro de fila con parámetros en el valor de [HOST_NAME](../../../t-sql/functions/host-name-transact-sql.md) , en la columna **LoginID** . Para más información, consulte [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md).  
  
 [!code-sql[HowTo#sp_MergeDynamicPub1](../../../relational-databases/replication/codesnippet/tsql/define-and-modify-a-join_2.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Join Filters](../../../relational-databases/replication/merge/join-filters.md)   
 [Filtros de fila con parámetros](../../../relational-databases/replication/merge/parameterized-filters-parameterized-row-filters.md)   
 [Cambiar las propiedades de la publicación y de los artículos](../../../relational-databases/replication/publish/change-publication-and-article-properties.md)   
 [Filtrar datos publicados para la replicación de mezcla](../../../relational-databases/replication/merge/filter-published-data-for-merge-replication.md)   
 [Procedimiento para definir y modificar un filtro de combinación entre artículos de mezcla (SQL Server Management Studio)](../../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Replication System Stored Procedures Concepts](../../../relational-databases/replication/concepts/replication-system-stored-procedures-concepts.md)   
 [Define a Logical Record Relationship Between Merge Table Articles](../../../relational-databases/replication/publish/define-a-logical-record-relationship-between-merge-table-articles.md)   
 [Definir y modificar un filtro de fila con parámetros para un artículo de mezcla](../../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)  
  
  
