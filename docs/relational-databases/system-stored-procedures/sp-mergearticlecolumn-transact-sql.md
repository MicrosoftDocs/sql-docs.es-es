---
description: sp_mergearticlecolumn (Transact-SQL)
title: sp_mergearticlecolumn (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_mergearticlecolumn
- sp_mergearticlecolumn_TSQL
helpviewer_keywords:
- sp_mergearticlecolumn
ms.assetid: b4f2b888-e094-4759-a472-d893638995eb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b5d8d2f63afd421081058dd1b53ec8084f4b5e13
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185318"
---
# <a name="sp_mergearticlecolumn-transact-sql"></a>sp_mergearticlecolumn (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crea particiones verticales en una publicación de combinación. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_mergearticlecolumn [ @publication = ] 'publication'  
        , [ @article = ] 'article'  
    [ , [ @column = ] 'column'  
    [ , [ @operation = ] 'operation'   
    [ , [ @schema_replication = ] 'schema_replication' ]  
    [ , [ @force_invalidate_snapshot = ] force_invalidate_snapshot ]   
    [ , [ @force_reinit_subscription = ] force_reinit_subscription ]   
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre del artículo de la publicación. *article* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @column = ] 'column'` Identifica las columnas en las que se va a crear la partición vertical. la *columna* es de **tipo sysname y su** valor predeterminado es NULL. Si es NULL y `@operation = N'add'`, de manera predeterminada se agregan al artículo todas las columnas de la tabla de origen. la *columna* no puede ser NULL cuando la *operación* está establecida en **Drop**. Para excluir columnas de un artículo, ejecute **sp_mergearticlecolumn** y especifique la *columna* y `@operation = N'drop'` para cada columna que se va a quitar del *artículo* especificado.  
  
`[ @operation = ] 'operation'` Es el estado de replicación. *Operation* es de tipo **nvarchar (4)** y su valor predeterminado es Add. **Agregar** marca la columna para la replicación. **Drop** borra la columna.  
  
`[ @schema_replication = ] 'schema_replication'` Especifica que un cambio de esquema se propagará cuando se ejecute el Agente de mezcla. *schema_replication* es de tipo **nvarchar (5)** y su valor predeterminado es false.  
  
> [!NOTE]  
>  Solo se admite **false** para *schema_replication*.  
  
`[ @force_invalidate_snapshot = ] force_invalidate_snapshot` Habilita o deshabilita la posibilidad de invalidar una instantánea. *force_invalidate_snapshot* es de **bit** y su valor predeterminado es **0**.  
  
 **0** especifica que los cambios en el artículo de mezcla no harán que la instantánea no sea válida.  
  
 **1** especifica que los cambios en el artículo de mezcla pueden hacer que la instantánea no sea válida y, en ese caso, el valor **1** concede permiso para que se produzca la nueva instantánea.  
  
`[ @force_reinit_subscription = ]force_reinit_subscription_` Habilita o deshabilita la capacidad de reinicializar la suscripción. *force_reinit_subscription* es un bit con un valor predeterminado de **0**.  
  
 **0** especifica que los cambios en el artículo de mezcla no harán que se reinicialice la suscripción.  
  
 **1** especifica que los cambios en el artículo de mezcla pueden hacer que la suscripción se reinicialice y, en tal caso, el valor **1** concede permiso para que se produzca la reinicialización de la suscripción.  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_mergearticlecolumn** se utiliza en la replicación de mezcla.  
  
 No es posible quitar una columna de identidad del artículo si se está utilizando la administración automática del intervalo de identidad. Para más información, vea [Replicar columnas de identidad](../../relational-databases/replication/publish/replicate-identity-columns.md).  
  
 Si una aplicación establece una nueva partición vertical después de crear la instantánea inicial, es necesario generar una instantánea nueva y volverla a aplicar a cada suscripción. Las instantáneas se aplican al ejecutar el siguiente Agente de instantáneas, distribución o mezcla programado.  
  
 Si se utiliza el seguimiento por fila en la detección de conflictos (valor predeterminado), la tabla base puede incluir un máximo de 1024 columnas, pero en el artículo deben filtrarse las columnas de forma que se publique un máximo de 246 columnas. Si se utiliza el seguimiento por columna, la tabla base puede incluir 246 columnas como máximo.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_AddMergeArticle](../../relational-databases/replication/codesnippet/tsql/sp-mergearticlecolumn-tr_1.sql)]  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** o del rol fijo de base de datos **db_owner** pueden ejecutar **sp_mergearticlecolumn**.  
  
## <a name="see-also"></a>Consulte también  
 [Definir y modificar un filtro de combinación entre artículos de mezcla](../../relational-databases/replication/publish/define-and-modify-a-join-filter-between-merge-articles.md)   
 [Definición y modificación de un filtro de fila con parámetros para un artículo de mezcla](../../relational-databases/replication/publish/define-and-modify-a-parameterized-row-filter-for-a-merge-article.md)   
 [Filtrar datos publicados](../../relational-databases/replication/publish/filter-published-data.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
