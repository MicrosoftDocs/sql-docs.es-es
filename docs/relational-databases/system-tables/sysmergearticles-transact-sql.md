---
description: sysmergearticles (Transact-SQL)
title: sysmergearticles (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergearticles
- sysmergearticles_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergearticles system table
ms.assetid: e9b1648e-4660-4688-9f56-18b2baf7228c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 07c85b8560894b8c09060ca08720b521470d1c71
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99211515"
---
# <a name="sysmergearticles-transact-sql"></a>sysmergearticles (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contiene una fila por cada artículo de mezcla definido en la base de datos local. Esta tabla se almacena en la base de datos de publicación.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del artículo.|  
|**type**|**tinyint**|Indica el tipo de artículo, que puede ser uno de los siguientes:<br /><br /> **10** = tabla.<br /><br /> **32** = procedimiento almacenado (solo esquema).<br /><br /> **64** = vista o vista indizada (solo esquema).<br /><br /> **128** = función definida por el usuario (solo esquema).<br /><br /> **160** = sinónimo (solo esquema).|  
|**objid**|**int**|Identificador de objeto.|  
|**sync_objid**|**int**|Id. de objeto de la vista que representa el conjunto de datos sincronizado.|  
|**view_type**|**tinyint**|Tipo de vista:<br /><br /> **0** = no es una vista; usar todo el objeto base.<br /><br /> **1** = vista permanente.<br /><br /> **2** = vista temporal.|  
|**artid**|**uniqueidentifier**|El número de identificación único del artículo indicado.|  
|**description**|**nvarchar(255)**|Descripción breve del artículo.|  
|**pre_creation_command**|**tinyint**|La acción predeterminada que debe llevarse a cabo cuando se crea el artículo en la base de datos de suscripciones:<br /><br /> **0 =** Ninguno: Si la tabla ya existe en el suscriptor, no se realiza ninguna acción.<br /><br /> **1** = quita la tabla antes de volver a crearla.<br /><br /> **2** = delete: emite una eliminación basada en la cláusula WHERE del filtro de subconjunto.<br /><br /> **3** = TRUNCATE: igual que **2**, pero elimina páginas en lugar de filas. Sin embargo, no precisa la cláusula WHERE.|  
|**pubid**|**uniqueidentifier**|Id. de la publicación a la que pertenece el artículo actual.|  
|**deseado**|**int**|Alias asignado para la identificación del artículo.|  
|**column_tracking**|**int**|Hace referencia a si se ha implementado el seguimiento de columnas para el artículo.|  
|**status**|**tinyint**|Indica el estado del artículo, que puede ser uno de los siguientes:<br /><br /> **1** = sin sincronizar: el script de procesamiento inicial para publicar la tabla se ejecutará la próxima vez que se ejecute el agente de instantáneas.<br /><br /> **2** = activo: se ha ejecutado el script de procesamiento inicial para publicar la tabla.<br /><br /> **5** = New_inactive-que se va a agregar.<br /><br /> **6** = New_active-que se va a agregar.|  
|**conflict_table**|**sysname**|Nombre de la tabla local que contiene los registros en conflicto del artículo actual. Esta tabla solo tiene fines informativos y su contenido puede ser modificado o eliminado con rutinas de resolución de conflictos personalizadas, o directamente por el administrador.|  
|**creation_script**|**nvarchar(255)**|Script de creación de este artículo.|  
|**conflict_script**|**nvarchar(255)**|Script de conflicto de este artículo.|  
|**article_resolver**|**nvarchar(255)**|Solucionador de conflictos personalizado en el nivel de filas de este artículo.|  
|**ins_conflict_proc**|**sysname**|El procedimiento utilizado para escribir conflictos en **conflict_table**.|  
|**insert_proc**|**sysname**|Procedimiento que utiliza el solucionador de conflictos predeterminado para insertar filas durante la sincronización.|  
|**update_proc**|**sysname**|Procedimiento que utiliza el solucionador de conflictos predeterminado para actualizar filas durante la sincronización.|  
|**select_proc**|**sysname**|Nombre de un procedimiento almacenado generado automáticamente que utiliza el Agente de mezcla para llevar a cabo el bloqueo y buscar columnas y filas de un artículo.|  
|**metadata_select_proc**|**sysname**|Nombre del procedimiento almacenado, y generado automáticamente, utilizado para obtener acceso a los metadatos en las tablas del sistema de replicación de mezcla.|  
|**delete_proc**|**sysname**|Procedimiento que utiliza el solucionador de conflictos predeterminado para eliminar filas durante la sincronización.|  
|**schema_option**|**Binary(8**|Para obtener los valores admitidos de *schema_option*, vea [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nombre de la tabla creada en el suscriptor.|  
|**destination_owner**|**sysname**|Nombre del propietario del objeto de destino.|  
|**resolver_clsid**|**nvarchar(50)**|Identificador del solucionador de conflictos personalizado.|  
|**subset_filterclause**|**nvarchar(1000)**|Cláusula de filtro de este artículo.|  
|**missing_col_count**|**int**|Número de columnas que faltan.|  
|**missing_cols**|**varbinary(128)**|Mapa de bits de columnas que faltan.|  
|**excluded_cols**|**varbinary(128)**|Mapa de bits de las columnas excluidas del artículo cuando se envía al suscriptor.|  
|**excluded_col_count**|**int**|Número de columnas excluidas.|  
|**columnas**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|Mapa de bits de las columnas que han sido eliminadas de la tabla de origen.|  
|**resolver_info**|**nvarchar(255)**|Almacenamiento para la información adicional requerida por los solucionadores de conflictos personalizados.|  
|**view_sel_proc**|**nvarchar (290)**|Nombre de un procedimiento almacenado que utiliza el Agente de mezcla para llenar por primera vez un artículo en una publicación filtrada dinámicamente y para enumerar las filas que han cambiado en cualquier publicación filtrada.|  
|**gen_cur**|**int**|Número de generación para los cambios locales de la tabla base de un artículo.|  
|**vertical_partition**|**int**|Especifica si está habilitado el filtrado de columnas en un artículo de una tabla. **0** indica que no hay filtrado vertical y publica todas las columnas.|  
|**identity_support**|**int**|Especifica si se habilita el control automático del intervalo de identidad. **1** significa que el control de intervalo de identidad está habilitado y **0** significa que no hay compatibilidad con el intervalo de identidad.|  
|**before_image_objid**|**int**|Identificador de objeto de la tabla de seguimiento. La tabla de seguimiento contiene ciertos valores de columna de clave cuando se crea una publicación con *\@ keep_partition_changes*  =  **true**.|  
|**before_view_objid**|**int**|Id. del objeto de una tabla de vistas. La vista está en una tabla que mantiene un seguimiento de si una fila pertenecía a un suscriptor concreto antes de que se eliminara o actualizara. Solo se aplica cuando se crea una publicación con *\@ keep_partition_changes*  =  **true.**|  
|**verify_resolver_signature**|**int**|Especifica si una firma digital se comprueba antes de utilizar un solucionador en la replicación de mezcla:<br /><br /> **0** = no se comprueba la firma.<br /><br /> **1** = se comprueba la firma para ver si proviene de una fuente de confianza.|  
|**allow_interactive_resolver**|**bit**|Especifica si está habilitado el uso del Solucionador interactivo en un artículo. **1** especifica que se utiliza el solucionador interactivo en el artículo.|  
|**fast_multicol_updateproc**|**bit**|Especifica si se ha habilitado el Agente de mezcla para aplicar cambios a varias columnas de la misma fila en una instrucción UPDATE.<br /><br /> **0** = emite una actualización independiente para cada columna modificada.<br /><br /> **1** = emite una instrucción UPDATE que hace que se produzcan actualizaciones en varias columnas en una instrucción.|  
|**check_permissions**|**int**|Mapa de bits de los permisos de nivel de tabla que se comprueba cuando el agente de mezcla aplica cambios en el publicador. *check_permissions* puede tener uno de estos valores:<br /><br /> **0x00 =** No se comprueban los permisos.<br /><br /> **0x10 =** Comprueba los permisos en el publicador antes de que se puedan cargar las inserciones realizadas en un suscriptor.<br /><br /> **0x20 =** Comprueba los permisos del publicador antes de que se puedan cargar las actualizaciones realizadas en un suscriptor.<br /><br /> **0x40 =** Comprueba los permisos en el publicador antes de que se puedan cargar las ELIMINAciones realizadas en un suscriptor.|  
|**maxversion_at_cleanup**|**int**|El valor más alto de generación para el cual se limpian los metadatos.|  
|**processing_order**|**int**|Indica el orden de procesamiento de los artículos en una publicación de combinación; donde un valor de **0** indica que el artículo está desordenado y los artículos se procesan en orden desde el valor más bajo al más alto. Si existen dos artículos que tienen el mismo valor, se procesan al mismo tiempo. Para más información, vea [Specify merge replication properties](../../relational-databases/replication/merge/specify-merge-replication-properties.md) (Especificación de propiedades de replicación de mezcla).|  
|**upload_options**|**tinyint**|Define las restricciones impuestas a las actualizaciones realizadas en un suscriptor con suscripción de cliente. Pueden tener uno de estos valores:<br /><br /> **0** = no hay restricciones en las actualizaciones realizadas en un suscriptor con una suscripción de cliente; todos los cambios se cargan en el publicador.<br /><br /> **1** = se permiten cambios en un suscriptor con una suscripción de cliente, pero no se cargan en el publicador.<br /><br /> **2** = no se permiten cambios en un suscriptor con una suscripción de cliente.<br /><br /> Para más información, vea [Optimizar el rendimiento de la replicación de mezcla con artículos de solo descarga](../../relational-databases/replication/merge/optimize-merge-replication-performance-with-download-only-articles.md).|  
|**published_in_tran_pub**|**bit**|Indica que un artículo de una publicación de combinación también se publica en una publicación transaccional.<br /><br /> **0** = el artículo no está publicado en un artículo transaccional.<br /><br /> **1** = el artículo también se publica en un artículo transaccional.|  
|**liviano**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**NCHAR (32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|Se agregará.|  
|**delete_tracking**|**bit**|Indica si se replican las eliminaciones.<br /><br /> **0** = las eliminaciones no se replican<br /><br /> **1** = las eliminaciones se replican, que es el comportamiento predeterminado para la replicación de mezcla.<br /><br /> Cuando el valor de *delete_tracking* es **0**, las filas eliminadas en el suscriptor deben quitarse manualmente en el publicador, y las filas eliminadas en el publicador deben quitarse manualmente en el suscriptor.<br /><br /> Nota: un valor de **0** produce una no convergencia.|  
|**compensate_for_errors**|**bit**|Indica si se realizan acciones de compensación cuando se producen errores durante la sincronización.<br /><br /> **0** = las acciones de compensación están deshabilitadas.<br /><br /> **1** = los cambios que no se pueden aplicar en un suscriptor o publicador siempre conducen a acciones de compensación para deshacer estos cambios, que es el comportamiento predeterminado para la replicación de mezcla.<br /><br /> Nota: un valor de **0** produce una no convergencia.|  
|**pub_range**|**bigint**|Tamaño del intervalo de identidad del publicador.|  
|**range**|**bigint**|Tamaño de los valores de identidad consecutivos que podrían asignarse a los suscriptores en un ajuste.|  
|**threshold**|**int**|Porcentaje de umbral del intervalo de identidad.|  
|**stream_blob_columns**|**bit**|Indica si se usará una optimización del flujo de datos al replicar columnas binarias de objetos de gran tamaño. **1** significa que se ha intentado la optimización.|  
|**preserve_rowguidcol**|**bit**|Indica si la replicación usará una columna de tipo rowguid existente. Un valor de **1** significa que se utiliza una columna ROWGUIDCOL existente. **0** significa que la replicación ha agregado la columna ROWGUIDCOL.|  
  
## <a name="see-also"></a>Consulte también  
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergearticle &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md)   
 [sp_changemergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-changemergearticle-transact-sql.md)   
 [sp_helpmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helpmergearticle-transact-sql.md)  
  
  
