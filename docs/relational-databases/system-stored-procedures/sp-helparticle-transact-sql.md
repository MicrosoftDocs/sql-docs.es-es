---
description: sp_helparticle (Transact-SQL)
title: sp_helparticle (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_helparticle_TSQL
- sp_helparticle
helpviewer_keywords:
- sp_helparticle
ms.assetid: 9c4a1a88-56f1-45a0-890c-941b8e0f0799
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 14baf63cc2944396a13bbc911fb6be7670244216
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176485"
---
# <a name="sp_helparticle-transact-sql"></a>sp_helparticle (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

  Muestra información acerca de un artículo. Este procedimiento almacenado se ejecuta en el publicador de la base de datos de publicación. En los publicadores de Oracle, este procedimiento almacenado se ejecuta en el distribuidor de cualquier base de datos.  
  
 ![Icono de vínculo de tema](../../database-engine/configure-windows/media/topic-link.gif "Icono de vínculo de tema") [Convenciones de sintaxis de Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Sintaxis  
  
```  
  
sp_helparticle [ @publication = ] 'publication'   
    [ , [ @article = ] 'article' ]  
    [ , [ @returnfilter = ] returnfilter ]  
    [ , [ @publisher = ] 'publisher' ]  
    [ , [ @found = ] found OUTPUT ]  
```  
  
## <a name="arguments"></a>Argumentos  
`[ @publication = ] 'publication'` Es el nombre de la publicación. *Publication* es de **tipo sysname** y no tiene ningún valor predeterminado.  
  
`[ @article = ] 'article'` Es el nombre de un artículo de la publicación. *article* es de **tipo sysname y su** valor predeterminado es **%** . Si no se proporciona el *artículo* , se devuelve información sobre todos los artículos de la publicación especificada.  
  
`[ @returnfilter = ] returnfilter` Especifica si se debe devolver la cláusula de filtro. *returnfilter* es de **bit** y su valor predeterminado es **1**, que devuelve la cláusula de filtro.  
  
`[ @publisher = ] 'publisher'` Especifica un publicador que no es de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . *Publisher* es de **tipo sysname y su** valor predeterminado es NULL.  
  
> [!NOTE]  
>  no se debe especificar el *publicador* al solicitar información sobre un artículo publicado por un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] publicador.  
  
`[ @found = ] found OUTPUT` Solo para uso interno.  
  
## <a name="result-sets"></a>Conjuntos de resultados  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**identificador de artículo**|**int**|IDENTIFICADOR del artículo.|  
|**article name**|**sysname**|Nombre del artículo.|  
|**base object**|**nvarchar (257)**|Nombre de la tabla subyacente representada por el artículo o el procedimiento almacenado.|  
|**Objeto de destino**|**sysname**|Nombre de la tabla de destino (suscripción).|  
|**synchronization object**|**nvarchar (257)**|Nombre de la vista que define el artículo publicado.|  
|**type**|**smallint**|Tipo de artículo:<br /><br /> **1** = basado en el registro.<br /><br /> **3** = basado en registro con filtro manual.<br /><br /> **5** = basado en registro con vista manual.<br /><br /> **7** = basado en registro con filtro manual y vista manual.<br /><br /> **8** = ejecución de procedimiento almacenado.<br /><br /> **24** = ejecución de procedimiento almacenado serializable.<br /><br /> **32** = procedimiento almacenado (solo esquema).<br /><br /> **64** = vista (solo esquema).<br /><br /> **96** = función de agregado (solo esquema).<br /><br /> **128** = función (solo esquema).<br /><br /> **257** = vista indizada basada en registro.<br /><br /> **259** = vista indizada basada en registro con filtro manual.<br /><br /> **261** = vista indizada basada en registro con vista manual.<br /><br /> **263** = vista indizada basada en registro con filtro manual y vista manual.<br /><br /> **320** = vista indizada (solo esquema).<br /><br />|  
|**status**|**tinyint**|Puede ser el resultado [& (and bit a bit)](../../t-sql/language-elements/bitwise-and-transact-sql.md) de una o más de estas propiedades de artículo:<br /><br /> **0x00** = [!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]<br /><br /> **0x01** = el artículo está activo.<br /><br /> **0x08** = incluir el nombre de la columna en las instrucciones INSERT.<br /><br /> **0x16** = usar instrucciones con parámetros.<br /><br /> debajo **= usar** instrucciones con parámetros e incluir el nombre de la columna en las instrucciones INSERT.|  
|**filter**|**nvarchar (257)**|Procedimiento almacenado utilizado para filtrar la tabla horizontalmente. Este procedimiento almacenado debe haber sido creado mediante la cláusula FOR REPLICATION.|  
|**description**|**nvarchar(255)**|Entrada descriptiva del artículo.|  
|**insert_command**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar inserciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**update_command**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar actualizaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**delete_command**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar eliminaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**creation script path**|**nvarchar(255)**|Ruta de acceso y nombre de un script de esquema del artículo que se utiliza para crear tablas de destino.|  
|**vertical partition**|**bit**|Indica si la creación de particiones verticales está habilitada para el artículo; donde un valor de **1** significa que está habilitada la creación de particiones verticales.|  
|**pre_creation_cmd**|**tinyint**|Comando anterior a la creación para DROP TABLE, DELETE TABLE o TRUNCATE TABLE.|  
|**filter_clause**|**ntext**|Cláusula WHERE que especifica el filtrado horizontal.|  
|**schema_option**|**Binary(8**|Mapa de bits de la opción de generación del esquema para el artículo dado. Para obtener una lista completa de los valores de **schema_option** , vea [sp_addarticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md).|  
|**dest_owner**|**sysname**|Nombre del propietario del objeto de destino.|  
|**source_owner**|**sysname**|Propietario del objeto de origen.|  
|**unqua_source_object**|**sysname**|Nombre del objeto de origen, sin el nombre del propietario.|  
|**sync_object_owner**|**sysname**|Propietario de la vista que define el artículo publicado .|  
|**unqualified_sync_object**|**sysname**|Nombre de la vista que define el artículo publicado, sin el nombre del propietario.|  
|**filter_owner**|**sysname**|Propietario del filtro.|  
|**unqua_filter**|**sysname**|Nombre del filtro, sin el nombre del propietario.|  
|**auto_identity_range**|**int**|Marca que establece si se activó el control automático del intervalo de identidad en la publicación en el momento en que se creó. **1** significa que el intervalo de identidad automático está habilitado; **0** significa que está deshabilitado.|  
|**publisher_identity_range**|**int**|Tamaño del intervalo del intervalo de identidad en el publicador si el artículo tiene el valor *identityrangemanagementoption* establecido en **auto** o **auto_identity_range** establecido en **true**.|  
|**identity_range**|**bigint**|Tamaño del intervalo del intervalo de identidad en el suscriptor si el artículo tiene el valor *identityrangemanagementoption* establecido en **auto** o **auto_identity_range** establecido en **true**.|  
|**threshold**|**bigint**|Valor de porcentaje que indica cuándo asigna el Agente de distribución un nuevo intervalo de identidad.|  
|**identityrangemanagementoption**|**int**|Indica la administración de intervalos de identidad controlada del artículo.|  
|**fire_triggers_on_snapshot**|**bit**|Indica si se ejecutan desencadenadores de usuario replicados al aplicar la instantánea inicial.<br /><br /> **1** = se ejecutan desencadenadores de usuario.<br /><br /> **0** = no se ejecutan los desencadenadores de usuario.|  
  
## <a name="return-code-values"></a>Valores de código de retorno  
 **0** (correcto) o **1** (error)  
  
## <a name="remarks"></a>Observaciones  
 **sp_helparticle** se utiliza en la replicación de instantáneas y en la replicación transaccional.  
  
## <a name="permissions"></a>Permisos  
 Solo los miembros del rol fijo de servidor **sysadmin** , del rol fijo de base de datos **db_owner** o de la lista de acceso a la publicación para la publicación actual pueden ejecutar **sp_helparticle**.  
  
## <a name="example"></a>Ejemplo  
 [!code-sql[HowTo#sp_helptranarticle](../../relational-databases/replication/codesnippet/tsql/sp-helparticle-transact-_1.sql)]  
  
## <a name="see-also"></a>Consulte también  
 [Ver y modificar las propiedades del artículo](../../relational-databases/replication/publish/view-and-modify-article-properties.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_articlecolumn &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-articlecolumn-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)   
 [sp_droparticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droparticle-transact-sql.md)   
 [Procedimientos almacenados de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)  
  
  
