---
description: sysmergepartitioninfoview (Transact-SQL)
title: sysmergepartitioninfoview (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sysmergepartitioninfoview
- sysmergepartitioninfoview_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergepartitioninfoview view
ms.assetid: 714e2935-1bc7-4901-aea2-64b1bbda03d6
author: stevestein
ms.author: sstein
ms.openlocfilehash: f9a5063691cb0d8ed77fdc19ce3bba1adebf8858
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181582"
---
# <a name="sysmergepartitioninfoview-transact-sql"></a>sysmergepartitioninfoview (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La vista **sysmergepartitioninfoview** expone información de creación de particiones para los artículos de la tabla. Esta vista se almacena en el publicador de la base de datos de publicación y en el suscriptor de la base de datos de suscripciones.  
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**name**|**sysname**|Nombre del artículo.|  
|**type**|**tinyint**|Indica el tipo de artículo, que puede ser uno de los siguientes:<br /><br /> **0x0A** = tabla.<br /><br /> **0x20** = solo esquema de procedimiento.<br /><br /> **0x40** = solo esquema de vista o esquema de vista indizada.<br /><br /> **0x80** = solo esquema de función.|  
|**objid**|**int**|El identificador del objeto publicado.|  
|**sync_objid**|**int**|Id. de objeto de la vista que representa el conjunto de datos sincronizado.|  
|**view_type**|**tinyint**|Tipo de vista:<br /><br /> **0** = no es una vista; usar todo el objeto base.<br /><br /> **1** = vista permanente.<br /><br /> **2** = vista temporal.|  
|**artid**|**uniqueidentifier**|El número de identificación único del artículo indicado.|  
|**description**|**nvarchar(255)**|Descripción breve del artículo.|  
|**pre_creation_command**|**tinyint**|La acción predeterminada que debe llevarse a cabo cuando se crea el artículo en la base de datos de suscripciones:<br /><br /> **0** = ninguno: Si la tabla ya existe en el suscriptor, no se realiza ninguna acción.<br /><br /> **1** = quita la tabla antes de volver a crearla.<br /><br /> **2** = delete: emite una eliminación basada en la cláusula WHERE del filtro de subconjunto.<br /><br /> **3** = TRUNCATE: igual que 2, pero elimina páginas en lugar de filas. Sin embargo, no precisa la cláusula WHERE.|  
|**pubid**|**uniqueidentifier**|Id. de la publicación a la que pertenece el artículo actual.|  
|**deseado**|**int**|Alias asignado para la identificación del artículo.|  
|**column_tracking**|**int**|Indica si está implementado el seguimiento de columnas en el artículo.|  
|**status**|**tinyint**|Indica el estado del artículo, que puede ser uno de los siguientes:<br /><br /> **1** = sin sincronizar: el script de procesamiento inicial para publicar la tabla se ejecutará la próxima vez que se ejecute el agente de instantáneas.<br /><br /> **2** = activo: se ha ejecutado el script de procesamiento inicial para publicar la tabla.|  
|**conflict_table**|**sysname**|Nombre de la tabla local que contiene los registros en conflicto del artículo actual. Esta tabla solo tiene fines informativos y su contenido puede ser modificado o eliminado con rutinas de resolución de conflictos personalizadas, o directamente por el administrador.|  
|**creation_script**|**nvarchar(255)**|Script de creación de este artículo.|  
|**conflict_script**|**nvarchar(255)**|Script de conflicto de este artículo.|  
|**article_resolver**|**nvarchar(255)**|El solucionador de conflictos de este artículo.|  
|**ins_conflict_proc**|**sysname**|El procedimiento utilizado para escribir información de conflictos en la tabla de conflictos.|  
|**insert_proc**|**sysname**|El procedimiento utilizado para insertar filas durante la sincronización.|  
|**update_proc**|**sysname**|El procedimiento utilizado para actualizar filas durante la sincronización.|  
|**select_proc**|**sysname**|El nombre de un procedimiento almacenado generado automáticamente que utiliza el Agente de mezcla para llevar a cabo el bloqueo y buscar columnas y filas de un artículo.|  
|**metadata_select_proc**|**sysname**|El nombre de un procedimiento almacenado generado automáticamente utilizado para obtener acceso a los metadatos de las tablas del sistema de replicación de mezcla.|  
|**delete_proc**|**sysname**|El procedimiento utilizado para eliminar filas durante la sincronización.|  
|**schema_option**|**Binary(8**|El mapa de bits de la opción de generación del esquema para el artículo dado. Para obtener información sobre los valores de *schema_option* compatibles, vea [sp_addmergearticle &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addmergearticle-transact-sql.md).|  
|**destination_object**|**sysname**|Nombre de la tabla creada en el suscriptor.|  
|**destination_owner**|**sysname**|Nombre del propietario del objeto de destino.|  
|**resolver_clsid**|**nvarchar(50)**|Identificador del solucionador de conflictos personalizado. En un controlador de lógica empresarial, este valor es NULL.|  
|**subset_filterclause**|**nvarchar(1000)**|Cláusula de filtro de este artículo.|  
|**missing_col_count**|**int**|El número de columnas publicadas que faltan en el artículo.|  
|**missing_cols**|**varbinary(128)**|El mapa de bits que describe las columnas que faltan en el artículo.|  
|**excluded_cols**|**varbinary(128)**|El mapa de bits de las columnas excluidas del artículo.|  
|**excluded_col_count**|**int**|El número de columnas excluidas del artículo. |  
|**columnas**|**varbinary(128)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**deleted_cols**|**varbinary(128)**|El mapa de bits que describe las columnas eliminadas del artículo.|  
|**resolver_info**|**nvarchar(255)**|Almacenamiento para la información adicional requerida por los solucionadores de conflictos personalizados.|  
|**view_sel_proc**|**nvarchar (290)**|El nombre de un procedimiento almacenado que utiliza el Agente de mezcla para llenar por primera vez un artículo en una publicación filtrada dinámicamente y para enumerar las filas que han cambiado en cualquier publicación filtrada.|  
|**gen_cur**|**bigint**|Genera el número para los cambios locales de la tabla base de un artículo.|  
|**vertical_partition**|**int**|Especifica si está habilitado el filtrado de columnas en un artículo de una tabla. **0** indica que no hay filtrado vertical y publica todas las columnas.|  
|**identity_support**|**int**|Especifica si se habilita el control automático del intervalo de identidad. **1** significa que el control de intervalo de identidad está habilitado y **0** significa que no hay compatibilidad con el intervalo de identidad.|  
|**before_image_objid**|**int**|Identificador de objeto de la tabla de seguimiento. La tabla de seguimiento contiene ciertos valores de columna de clave cuando se han habilitado optimizaciones de cambio de particiones para la publicación.|  
|**before_view_objid**|**int**|Id. del objeto de una tabla de vistas. La vista está en una tabla que mantiene un seguimiento de si una fila pertenecía a un suscriptor concreto antes de que se eliminara o actualizara. Se aplica solamente cuando se han habilitado optimizaciones de cambio de particiones en la publicación.|  
|**verify_resolver_signature**|**int**|Especifica si una firma digital se comprueba antes de utilizar un solucionador en la replicación de mezcla:<br /><br /> **0** = no se comprueba la firma.<br /><br /> **1** = se comprueba la firma para ver si proviene de una fuente de confianza.|  
|**allow_interactive_resolver**|**bit**|Especifica si está habilitado el uso del Solucionador interactivo en un artículo. **1** significa que se puede usar el solucionador interactivo en el artículo.|  
|**fast_multicol_updateproc**|**bit**|Especifica si se ha habilitado el Agente de mezcla para aplicar cambios a varias columnas de la misma fila en una instrucción UPDATE.<br /><br /> **0** = emite una actualización independiente para cada columna modificada.<br /><br /> **1** = se emite en una instrucción UPDATE que hace que las actualizaciones se realicen en varias columnas en una instrucción.|  
|**check_permissions**|**int**|El mapa de bits de los permisos de nivel de tabla que se comprobarán cuando el agente de mezcla aplique cambios en el publicador. *check_permissions* puede tener uno de estos valores:<br /><br /> **0x00** = no se comprueban los permisos.<br /><br /> **0x10** = comprueba los permisos en el publicador antes de que se realicen las inserciones en un suscriptor.<br /><br /> **0x20** = comprueba los permisos en el publicador antes de que se puedan cargar las actualizaciones realizadas en un suscriptor.<br /><br /> **0x40** = comprueba los permisos en el publicador antes de que se puedan cargar las eliminaciones realizadas en un suscriptor.|  
|**maxversion_at_cleanup**|**int**|La generación máxima que se limpiará la próxima vez que se ejecute el Agente de mezcla.|  
|**processing_order**|**int**|Indica el orden de procesamiento de los artículos en una publicación de combinación; donde un valor de **0** indica que el artículo está desordenado y los artículos se procesan en orden desde el valor más bajo al más alto. Si existen dos artículos que tienen el mismo valor, se procesan al mismo tiempo. Para más información, vea [Specify merge replication properties](../../relational-databases/replication/merge/specify-merge-replication-properties.md) (Especificación de propiedades de replicación de mezcla).|  
|**upload_options**|**tinyint**|Define si es posible realizar cambios en el suscriptor o cargarlos desde él. Puede se uno de los siguientes valores.<br /><br /> **0** = no hay restricciones en las actualizaciones realizadas en el suscriptor; todos los cambios se cargan en el publicador.<br /><br /> **1** = se permiten cambios en el suscriptor, pero no se cargan en el publicador.<br /><br /> **2** = no se permiten cambios en el suscriptor.|  
|**published_in_tran_pub**|**bit**|Indica que un artículo de una publicación de combinación también se publica en una publicación transaccional.<br /><br /> **0** = el artículo no está publicado en un artículo transaccional.<br /><br /> **1** = el artículo también se publica en un artículo transaccional.|  
|**liviano**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**procname_postfix**|**NCHAR (32)**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**well_partitioned_lightweight**|**bit**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**before_upd_view_objid**|**int**|El Id. de la vista de la tabla antes de las actualizaciones.|  
|**delete_tracking**|**bit**|Indica si las eliminaciones se replican.<br /><br /> **0** = las eliminaciones no se replican.<br /><br /> **1** = las eliminaciones se replican, que es el comportamiento predeterminado para la replicación de mezcla.<br /><br /> Cuando el valor de *delete_tracking* es **0**, las filas eliminadas en el suscriptor deben quitarse manualmente en el publicador, y las filas eliminadas en el publicador deben quitarse manualmente en el suscriptor.<br /><br /> Nota: un valor de **0** produce una no convergencia.|  
|**compensate_for_errors**|**bit**|Indica si se llevan a cabo acciones de compensación cuando se producen errores durante la sincronización.<br /><br /> **0** = las acciones de compensación están deshabilitadas.<br /><br /> **1** = los cambios que no se pueden aplicar en un suscriptor o publicador siempre conducen a acciones de compensación para deshacer estos cambios, que es el comportamiento predeterminado para la replicación de mezcla.<br /><br /> Nota: un valor de **0** produce una no convergencia.|  
|**pub_range**|**bigint**|Tamaño del intervalo de identidad del publicador.|  
|**range**|**bigint**|Tamaño de los valores de identidad consecutivos que podrían asignarse a los suscriptores en un ajuste.|  
|**threshold**|**int**|Porcentaje de umbral del intervalo de identidad.|  
|**stream_blob_columns**|**bit**|Indica si se utiliza la optimización de transmisión para columnas de objetos binarios grandes. **1** significa que se ha intentado la optimización.|  
|**preserve_rowguidcol**|**bit**|Indica si la replicación utiliza una columna rowguid existente. Un valor de **1** significa que se utiliza una columna ROWGUIDCOL existente. **0** significa que la replicación ha agregado la columna ROWGUIDCOL.|  
|**partition_view_id**|**int**|Identifica la vista que define a una partición del suscriptor.|  
|**repl_view_id**|**int**|[!INCLUDE[ssInternalOnly](../../includes/ssinternalonly-md.md)]|  
|**partition_deleted_view_rule**|**sysname**|La instrucción utilizada en un desencadenador de replicación de mezcla para recuperar el Id. de partición de cada fila eliminada o actualizada en función de sus valores de columna antiguos.|  
|**partition_inserted_view_rule**|**Predeterminado**|La instrucción utilizada en un desencadenador de replicación de mezcla para recuperar el Id. de partición de cada fila insertada o actualizada en función de sus valores de columna nuevos.|  
|**membership_eval_proc_name**|**sysname**|Nombre del procedimiento que evalúa los identificadores de las particiones actuales en [MSmerge_contents &#40;&#41;de Transact-SQL ](../../relational-databases/system-tables/msmerge-contents-transact-sql.md).|  
|**lista_de_columnas**|**sysname**|Una lista delimitada por comas de las columnas publicadas en un artículo.|  
|**column_list_blob**|**sysname**|Una lista delimitada por comas de las columnas publicadas en un artículo, incluidas las columnas de objetos binarios grandes.|  
|**expand_proc**|**sysname**|Nombre del procedimiento que vuelve a evaluar los ID. de partición para todas las filas secundarias de una fila primaria recién insertada y para las filas primarias que han sufrido un cambio de partición o que se han eliminado.|  
|**logical_record_parent_nickname**|**int**|El alias del primario de nivel superior de un artículo dado de un registro lógico.|  
|**logical_record_view**|**int**|Una vista que produce el rowguid del artículo primario de nivel superior correspondiente a cada rowguid secundario.|  
|**logical_record_deleted_view_rule**|**sysname**|Similar a **logical_record_view**, con la salvedad de que muestra las filas secundarias en la tabla "eliminada" en los desencadenadores Update y DELETE.|  
|**logical_record_level_conflict_detection**|**bit**|Indica si los conflictos se deben detectar en el nivel de registro lógico o en el nivel de fila o de columna.<br /><br /> **0** = se utiliza la detección de conflictos de nivel de fila o de columna.<br /><br /> **1** = se utiliza la detección de conflictos de registros lógicos, donde un cambio en una fila en el publicador y el cambio en una fila independiente, el mismo registro lógico en el suscriptor se trata como un conflicto.<br /><br /> Cuando este valor es 1, solo se puede utilizar la resolución de conflictos de nivel de registro lógico.|  
|**logical_record_level_conflict_resolution**|**bit**|Indica si los conflictos se deben solucionar en el nivel de registro lógico o en el nivel de fila o de columna.<br /><br /> **0** = se utiliza la resolución de nivel de columna o fila.<br /><br /> **1** = en caso de conflicto, todo el registro lógico del ganador sobrescribe todo el registro lógico en el lado perdedor.<br /><br /> Se puede utilizar un valor de 1 tanto con la detección de nivel de registro lógico como con la detección de nivel de fila o de columna.|  
|**partition_options**|**tinyint**|Define el modo en el que se realiza la partición de los datos en el artículo, lo que permite optimizaciones de rendimiento cuando todas las filas pertenecen solamente a una partición o solamente a una suscripción. El *partition_options* puede ser uno de los valores siguientes.<br /><br /> **0** = el filtro para el artículo es estático o no produce un subconjunto de datos único para cada partición, es decir, una partición "superpuesta".<br /><br /> **1** = las particiones se superponen y las actualizaciones DML realizadas en el suscriptor no pueden cambiar la partición a la que pertenece una fila.<br /><br /> **2** = el filtro del artículo produce particiones no superpuestas, pero varios suscriptores pueden recibir la misma partición.<br /><br /> **3** = el filtro del artículo produce particiones no superpuestas que son únicas para cada suscripción.|  
|**name**|**sysname**|El nombre de una partición.|  
  
## <a name="see-also"></a>Consulte también  
 [Administrar particiones para una publicación de combinación con filtros con parámetros](../../relational-databases/replication/publish/manage-partitions-for-a-merge-publication-with-parameterized-filters.md)   
 [Tablas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vistas de replicación &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)   
 [sp_addmergepartition &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-addmergepartition-transact-sql.md)   
 [sp_helpmergepartition &#40;&#41;de Transact-SQL ](../../relational-databases/system-stored-procedures/sp-helpmergepartition-transact-sql.md)  
  
  
