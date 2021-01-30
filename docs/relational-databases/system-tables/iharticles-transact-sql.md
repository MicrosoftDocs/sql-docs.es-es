---
description: IHarticles (Transact-SQL)
title: IHarticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- IHarticles
- IHarticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- IHarticles system table
ms.assetid: 773ef9b7-c993-4629-9516-70c47b9dcf65
author: cawrites
ms.author: chadam
ms.openlocfilehash: d0fa94df7e518dd3c0348b5f729393ad0af64175
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99212037"
---
# <a name="iharticles-transact-sql"></a>IHarticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La tabla del sistema **IHarticles** contiene una fila por cada artículo que se está replicando desde un publicador que no es de SQL Server mediante el distribuidor actual. Esta tabla se almacena en la base de datos de distribución.  
  
## <a name="definition"></a>Definición  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**article_id**|**int**|La columna de identidad que proporciona un número de identificación único para el artículo.|  
|**name**|**sysname**|El nombre asociado al artículo, único en la publicación.|  
|**publication_id**|**smallint**|Id. de la publicación a la que pertenece el artículo.|  
|**table_id**|**int**|IDENTIFICADOR de la tabla que se va a publicar desde [IHpublishertables](../../relational-databases/system-tables/ihpublishertables-transact-sql.md).|  
|**publisher_id**|**smallint**|Id. del publicador que no es de SQL Server.|  
|**creation_script**|**nvarchar(255)**|El script de esquema del artículo.|  
|**del_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar eliminaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**filter**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**filter_clause**|**ntext**|Cláusula WHERE del artículo, utilizada para el filtrado horizontal y escrita en Transact-SQL estándar que puede interpretar un publicador que no sea de SQL.|  
|**ins_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar inserciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**pre_creation_cmd**|**tinyint**|Comando para ejecutar antes de que la instantánea inicial se aplique cuando ya existe un objeto con el mismo nombre en el suscriptor.<br /><br /> **0** = ninguno: no se ejecuta un comando.<br /><br /> **1** = quitar la tabla de destino.<br /><br /> **2** = eliminar: elimina datos de la tabla de destino.<br /><br /> **3** = TRUNCATE: trunca la tabla de destino.|  
|**status**|**tinyint**|Máscara de bits para las opciones y estado del artículo; puede ser el resultado OR lógico bit a bit de uno o más de estos valores:<br /><br /> **0** = no hay propiedades adicionales.<br /><br /> **1** = activo.<br /><br /> **8** = incluir el nombre de la columna en las instrucciones INSERT.<br /><br /> **16** = usar instrucciones con parámetros.<br /><br /> Por ejemplo, un artículo activo que utilice instrucciones con parámetros tendrá un valor de 17 en esta columna. Un valor de 0 significa que el artículo no está activo y no tiene otras propiedades definidas.|  
|**type**|**tinyint**|Tipo de artículo:<br /><br /> **1** = artículo basado en registro.|  
|**upd_cmd**|**nvarchar(255)**|El tipo de comando de replicación utilizado al replicar actualizaciones con artículos de la tabla. Para más información, vea [Especificar cómo se propagan los cambios para los artículos transaccionales](../../relational-databases/replication/transactional/transactional-articles-specify-how-changes-are-propagated.md).|  
|**schema_option**|**Binary(8**|Mapa de bits de la opción de generación de esquema del artículo dado, que puede ser el resultado OR lógico bit a bit de uno o varios de estos valores:<br /><br /> **0x00** = deshabilita el scripting por el agente de instantáneas y usa el CreationScript proporcionado.<br /><br /> **0x01** = genera la creación del objeto (CREATE TABLE, CREATE PROCEDURE, etc.).<br /><br /> **0x10** = generar un índice clúster correspondiente.<br /><br /> **0x40** = generar índices no clúster correspondientes.<br /><br /> **0x80** = incluye la integridad referencial declarada en las claves principales.<br /><br /> **0x1000** = Replica la intercalación de nivel de columna. Nota: esta opción está establecida de forma predeterminada para que los publicadores de Oracle habiliten comparaciones que distinguen mayúsculas de minúsculas.<br /><br /> **0x4000** = Replica claves únicas si se define en un artículo de tabla.<br /><br /> **0x8000** = replica una clave principal y las claves únicas de un artículo de tabla como restricciones mediante instrucciones ALTER TABLE.|  
|**dest_owner**|**sysname**|Propietario de la tabla de la base de datos de destino.|  
|**dest_table**|**sysname**|Nombre de la tabla de destino.|  
|**tablespace_name**|**nvarchar(255)**|Identifica el espacio de tablas utilizado por la tabla de registro del artículo.|  
|**objid**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**sync_objid**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**description**|**nvarchar(255)**|La entrada descriptiva del artículo.|  
|**publisher_status**|**int**|Se utiliza para indicar si la vista que define el artículo publicado se ha definido mediante una llamada a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md).<br /><br /> **0**  =  se ha llamado a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) .<br /><br /> **1**  =  no se ha llamado a [sp_articleview](../../relational-databases/system-stored-procedures/sp-articleview-transact-sql.md) .|  
|**article_view_owner**|**nvarchar(255)**|El propietario del objeto de sincronización del publicador utilizado por el Agente de registro del LOG.|  
|**article_view**|**nvarchar(255)**|El objeto de sincronización del publicador utilizado por el Agente de registro del LOG.|  
|**ins_scripting_proc**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**del_scripting_proc**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**upd_scripting_proc**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**custom_script**|**int**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**fire_triggers_on_snapshot**|**bit**|Esta columna no se utiliza y solo se incluye para que la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) de la tabla **IHarticles** sea compatible con la vista [sysarticles](../../relational-databases/system-views/sysarticles-system-view-transact-sql.md) utilizada para los artículos de SQL Server ([sysarticles](../../relational-databases/system-tables/sysarticles-transact-sql.md)).|  
|**instance_id**|**int**|Identifica la instancia actual del registro del artículo para la tabla publicada.|  
|**use_default_datatypes**|**bit**|Indica si el artículo utiliza asignaciones de tipos de datos predeterminadas; un valor de **1** indica que se usan las asignaciones de tipos de datos predeterminados.|  
  
## <a name="see-also"></a>Vea también  
 [Replicación de bases de datos heterogéneas](../../relational-databases/replication/non-sql/heterogeneous-database-replication.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addarticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addarticle-transact-sql.md)   
 [sp_changearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changearticle-transact-sql.md)  
  
  
