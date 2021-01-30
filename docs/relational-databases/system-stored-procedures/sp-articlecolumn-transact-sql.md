---
description: sp_articlecolumn (Transact-SQL)
title: sp_articlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_articlecolumn
- sp_articlecolumn_TSQL
helpviewer_keywords:
- sp_articlecolumn
ms.assetid: 8abaa8c1-d99e-4788-970f-c4752246c577
author: markingmyname
ms.author: maghan
ms.openlocfilehash: da57a5aa4ae6645c7b1b2ff6377b79160fa110c6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203208"
---
# <a name="sp_articlecolumn-transact-sql"></a>sp_articlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Se utiliza para especificar las columnas incluidas en el artículo para filtrar verticalmente los datos de una tabla publicada. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_articlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column' ]  
    [ , [ @operation = ] 'operation' ]  
    [ , [ @refresh_synctran_procs = ] refresh_synctran_procs ]  
    [ , [ @ignore_distributor = ] ignore_distributor ]  
    [ , [ @change_active = ] change_actve ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]  
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @internal = ] 'internal' ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación que contiene este artículo. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @column = ] 'column'` Es el nombre de la columna que se va a agregar o quitar. la *columna* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL, se publican todas las columnas.  
  
`[ @operation = ] 'operation'` Especifica si se van a agregar o quitar columnas en un artículo. *Operation* es de tipo **nvarchar (5)** y su valor predeterminado es Add. **Agregar** marca la columna para la replicación. **Drop** desmarca la columna.  
  
`[ @refresh_synctran_procs = ] refresh_synctran_procs` Especifica si se vuelven a generar los procedimientos almacenados que admiten suscripciones de actualización inmediata para que coincidan con el número de columnas replicadas. *refresh_synctran_procs* es de **bit** y su valor predeterminado es **1**. Si es **1**, se vuelven a generar los procedimientos almacenados.  
  
`[ @ignore_distributor = ] ignore_distributor` Indica si este procedimiento almacenado se ejecuta sin conectarse al distribuidor. *ignore_distributor* es de **bit** y su valor predeterminado es **0**. Si es **0**, la base de datos debe estar habilitada para la publicación y la caché de artículos debe actualizarse para reflejar las nuevas columnas replicadas en el artículo. Si es **1**, permite quitar columnas de artículo para los artículos que residen en una base de datos no publicada. solo se debe usar en situaciones de recuperación.  
  
`[ @change_active = ] change_active` Permite modificar las columnas de las publicaciones que tienen suscripciones. *change_active* es de **tipo int** y su valor predeterminado es **0**. Si es **0**, las columnas no se modifican. Si es **1**, se pueden agregar o quitar columnas de los artículos activos que tienen suscripciones.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Confirma que la acción realizada por este procedimiento almacenado puede invalidar una instantánea existente. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo no hacen que la instantánea no sea válida. Si el procedimiento almacenado detecta que el cambio requiere una nueva instantánea, se producirá un error y no se realizarán cambios.  
  
 **1** especifica que los cambios en el artículo pueden hacer que la instantánea no sea válida y, si hay suscripciones existentes que requieran una nueva instantánea, concede permiso para que la instantánea existente se marque como obsoleta y se genere una nueva instantánea.  
  
 [**@force_reinit_subscription =** ] *force_reinit_subscription*  
 Confirma que la acción realizada por este procedimiento almacenado puede requerir que se reinicialicen las suscripciones existentes. *force_reinit_subscription* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo no harán que se reinicialice la suscripción. Si el procedimiento almacenado detecta que el cambio requiere la reinicialización de suscripciones, se producirá un error y no se realizarán cambios. **1** especifica que los cambios en el artículo hacen que se reinicialicen las suscripciones existentes y concede permiso para que se produzca la reinicialización de la suscripción.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  el *publicador* no debe usarse con un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
`[ @internal = ] 'internal'` Solo para uso interno.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_articlecolumn** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
 Solo se puede filtrar un artículo no suscrito mediante **sp_articlecolumn**.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddTranArticle](../../relational-databases/replication/codesnippet/tsql/sp-articlecolumn-transac_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_articlecolumn**.  
  
## <a name="see-also"></a>Consulte también  
 [Define an Article](../../relational-databases/replication/publish/define-an-article.md)   
 [Definir y modificar un filtro de columna](../../relational-databases/replication/publish/define-and-modify-a-column-filter.md)   
 [Filtrar datos publicados](../../relational-databases/replication/publish/filter-published-data.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articleview &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [sp_helparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helparticle-transact-sql.md)   
 [sp_helparticlecolumns &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helparticlecolumns-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
