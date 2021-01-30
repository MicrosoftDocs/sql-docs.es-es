---
description: sp_articleview (Transact-SQL)
title: sp_articleview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articleview
- sp_articleview_TSQL
helpviewer_keywords:
- sp_articleview
ms.assetid: a3d63fd6-f360-4a2f-8a82-a0dc15f650b3
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 014ccf37727534d16fa5b16bc280da5b76dfb76e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203196"
---
# <a name="sp_articleview-transact-sql"></a>sp_articleview (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crea la vista que define el artículo publicado cuando una tabla se filtra horizontal o verticalmente. Esta vista se utiliza como el origen filtrado del esquema y los datos de las tablas de destino. Con este procedimiento almacenado solamente pueden modificarse artículos sin suscripciones. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_articleview [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @view_name = ] 'view_name']  
    [ , [ @filter_clause = ] 'filter_clause']  
    [ , [ @change_active = ] change_active ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @refreshsynctranprocs = ] refreshsynctranprocs ]  
    [ , [ @internal = ] internal ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene el artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @view_name = ] 'view_name'` Es el nombre de la vista que define el artículo publicado. *view_name* es de tipo **nvarchar (386)** y su valor predeterminado es NULL.  
  
`[ @filter_clause = ] 'filter_clause'` Es una cláusula de restricción (WHERE) que define un filtro horizontal. Cuando escriba la cláusula de restricción, omita la palabra clave WHERE. *filter_clause* es **ntext** y su valor predeterminado es NULL.  
  
`[ @change_active = ] change_active` Permite modificar las columnas de las publicaciones que tienen suscripciones. *change_active* es de **tipo int** y su valor predeterminado es **0**. Si es **0**, no se cambian las columnas. Si es **1**, las vistas se pueden crear o volver a crear en artículos activos que tienen suscripciones.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo no hacen que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que los cambios en el artículo pueden hacer que la instantánea no sea válida y, si hay suscripciones existentes que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea.  
  
`[ @force_reinit_subscription = ] _force_reinit_subscription_` Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es un **bit** con un valor predeterminado de **0**.  
  
 **0** especifica que los cambios en el artículo no harán que se reinicialice la suscripción. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de suscripciones, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que los cambios en el artículo hacen que se reinicialice la suscripción existente y concede permiso para que se produzca la reinicialización de la suscripción.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe usar el *publicador* al publicar desde un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
`[ @refreshsynctranprocs = ] refreshsynctranprocs` Indica si los procedimientos almacenados utilizados para sincronizar la replicación se vuelven a crear automáticamente. *refreshsynctranprocs* es de **bit** y su valor predeterminado es 1.  
  
 **1** significa que los procedimientos almacenados se vuelven a crear.  
  
 **0** significa que los procedimientos almacenados no se vuelven a crear.  
  
`[ @internal = ] internal` [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_articleview** crea la vista que define el artículo publicado e inserta el ID. de esta vista en la **columna sync_objid** de la tabla de [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) e inserta el texto de la cláusula Restriction en la columna **filter_clause** . Si se replican todas las columnas y no hay ningún **filter_clause**, el **sync_objid** de la tabla de [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) se establece en el identificador de la tabla base y no es necesario el uso de **sp_articleview** .  
  
 Para publicar una tabla filtrada verticalmente (es decir, para filtrar columnas), ejecute primero **sp_addarticle** sin *sync_object* parámetro, ejecute [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una vez para cada columna que se va a replicar (definiendo el filtro vertical) y, a continuación, ejecute **sp_articleview** para crear la vista que define el artículo publicado.  
  
 Para publicar una tabla filtrada horizontalmente (es decir, para filtrar filas), ejecute [sp_addarticle &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sin ningún parámetro de *filtro* . Ejecute [sp_articlefilter &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)y proporcione todos los parámetros, incluido *filter_clause*. A continuación, ejecute **sp_articleview** y proporcione todos los parámetros, incluido el *filter_clause* idéntico.  
  
 Para publicar una tabla filtrada vertical y horizontalmente, ejecute [sp_addarticle &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md) sin parámetros *sync_object* o *Filter* . Ejecute [sp_articlecolumn &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md) una vez por cada columna que se va a replicar y, a continuación, ejecute [sp_articlefilter &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md) y **sp_articleview**.  
  
 Si el artículo ya tiene una vista que define el artículo publicado, **sp_articleview** quita la vista existente y crea una nueva automáticamente. Si la vista se creó manualmente (**Escriba** [sysarticles &#40;Transact-SQL&#41;](../../relational-databases/system-tables/sysarticles-transact-sql.md) es **5**), no se quita la vista existente.  
  
 Si crea un procedimiento almacenado de filtro personalizado y una vista que define el artículo publicado manualmente, no ejecute **sp_articleview**. En su lugar, proporcione estos parámetros como *filtro* y *sync_object* para [sp_addarticle &#40;&#41;de Transact-SQL](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md), junto con el valor de *tipo* adecuado.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articleview-transact-_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_articleview**.  
  
## <a name="see-also"></a>Consulte también  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Definir y modificar un filtro de fila estático](../../relational-databases/replication/publish/define-and-modify-a-static-row-filter.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlefilter &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlefilter-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
