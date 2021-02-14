---
title: sys.database_query_store_options (Transact-SQL)
description: sys.database_query_store_options (Transact-SQL)
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- DATABASE_QUERY_STORE_OPTIONS_TSQL
- DATABASE_QUERY_STORE_OPTIONS
- SYS.DATABASE_QUERY_STORE_OPTIONS_TSQL
- SYS.DATABASE_QUERY_STORE_OPTIONS
dev_langs:
- TSQL
helpviewer_keywords:
- sys.database_query_store_options catalog view
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 1/8/2021
monikerRange: =azuresqldb-current||>=sql-server-2016||= azure-sqldw-latest||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 643f9990294499df5be059a79320a6f954e2aa39
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100341366"
---
# <a name="sysdatabase_query_store_options-transact-sql"></a>sys.database_query_store_options (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa.md)]

  Devuelve las opciones de Almacén de consultas para esta base de datos.  
  
**Se aplica a**: [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ([!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores), [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)]
  
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**desired_state**|**smallint**|Indica el modo de operación deseado de Almacén de consultas, establecido explícitamente por el usuario.<br /> 0 = OFF <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE|  
|**desired_state_desc**|**nvarchar(60)**|Descripción textual del modo de operación deseado de Almacén de consultas:<br />Apagado<br />READ_ONLY<br />READ_WRITE|  
|**actual_state**|**smallint**|Indica el modo de operación de Almacén de consultas. Además de la lista de los Estados deseados requeridos por el usuario, el estado real puede ser un estado de error.<br /> 0 = OFF <br /> 1 = READ_ONLY<br /> 2 = READ_WRITE<br /> 3 = ERROR|  
|**actual_state_desc**|**nvarchar(60)**|Descripción textual del modo de operación real de Almacén de consultas.<br />Apagado<br />READ_ONLY<br />READ_WRITE<br />ERROR<br /><br /> Hay situaciones en las que el estado real es diferente del estado deseado:<br />-Si la base de datos se establece en modo de solo lectura o si Almacén de consultas tamaño supera la cuota configurada, Almacén de consultas puede funcionar en modo de solo lectura aunque el usuario haya especificado la lectura y escritura.<br />-En escenarios extremos Almacén de consultas puede entrar en un estado de ERROR debido a errores internos. A partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] , si esto sucede, se puede recuperar almacén de consultas ejecutando el `sp_query_store_consistency_check` procedimiento almacenado en la base de datos afectada. Si `sp_query_store_consistency_check` la ejecución no funciona, o si está usando [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] , debe borrar los datos mediante la ejecución de `ALTER DATABASE [YourDatabaseName] SET QUERY_STORE CLEAR ALL;`|  
|**readonly_reason**|**int**|Cuando se READ_WRITE la **desired_state_desc** y se READ_ONLY la **actual_state_desc** , **readonly_reason** devuelve un mapa de bits para indicar por qué el almacén de consultas está en modo de solo lectura.<br /><br /> **1** : la base de datos está en modo de solo lectura<br /><br /> **2** -la base de datos está en modo de usuario único<br /><br /> **4** -la base de datos está en modo de emergencia<br /><br /> **8** : la base de datos es una réplica secundaria (se aplica a Always on y a [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] la replicación geográfica). Este valor solo se puede observar en réplicas secundarias **legibles**<br /><br /> **65536** -el almacén de consultas ha alcanzado el límite de tamaño establecido por la `MAX_STORAGE_SIZE_MB` opción. Para obtener más información acerca de esta opción, vea [Opciones de ALTER DATABASE set (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).<br /><br /> **131072** -el número de instrucciones diferentes en almacén de consultas ha alcanzado el límite de memoria interna. Considere la posibilidad de quitar las consultas que no necesite o actualizar a un nivel de servicio superior para habilitar la transferencia de Almacén de consultas al modo de lectura y escritura.<br /><br /><br /> **262144** : el tamaño de los elementos en memoria que esperan ser persistentes en el disco ha alcanzado el límite de memoria interna. Almacén de consultas estará en modo de solo lectura temporalmente hasta que los elementos en memoria se conserven en el disco. <br /><br /><br /> **524288** : la base de datos ha alcanzado el límite de tamaño de disco. Almacén de consultas forma parte de la base de datos de usuario, por lo que si no hay más espacio disponible para una base de datos, eso significa que Almacén de consultas ya no puede crecer.<br /><br /> <br /> Para volver a cambiar el modo de operaciones de Almacén de consultas a lectura y escritura, consulte la sección **comprobar almacén de consultas está recopilando datos de consulta continuamente** en [el almacén de consultas de prácticas recomendadas](../../relational-databases/performance/best-practice-with-the-query-store.md#Verify).|  
|**current_storage_size_mb**|**bigint**|Tamaño de Almacén de consultas en el disco en megabytes.|  
|**flush_interval_seconds**|**bigint**|El período de vaciado normal de Almacén de consultas datos en el disco en segundos. El valor predeterminado es **900** (15 min).<br /><br /> Cambie mediante la `ALTER DATABASE <database> SET QUERY_STORE (DATA_FLUSH_INTERVAL_SECONDS  = <interval>)` instrucción.|  
|**interval_length_minutes**|**bigint**|El intervalo de agregación de estadísticas en minutos. No se permiten valores arbitrarios. Use uno de los siguientes: 1, 5, 10, 15, 30, 60 y 1440 minutos. El valor predeterminado es de **60** minutos.|  
|**max_storage_size_mb**|**bigint**|Tamaño máximo del disco para la Almacén de consultas en megabytes (MB). El valor predeterminado es **100** MB hasta [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y **1 GB** a partir de [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] .<br />Para la [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] Edición Premium, el valor predeterminado es 1 GB y, para la [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] edición Basic, el valor predeterminado es 10 MB.<br /><br /> Cambie mediante la `ALTER DATABASE <database> SET QUERY_STORE (MAX_STORAGE_SIZE_MB = <size>)` instrucción.|  
|**stale_query_threshold_days**|**bigint**|Número de días que se conserva la información de una consulta en el Almacén de consultas. El valor predeterminado es **30**. Establézcalo en 0 para deshabilitar la Directiva de retención.<br />En la edición básica de [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] , el valor predeterminado es 7 días.<br /><br /> Cambie mediante la `ALTER DATABASE <database> SET QUERY_STORE ( CLEANUP_POLICY = ( STALE_QUERY_THRESHOLD_DAYS = <value> ) )` instrucción.|  
|**max_plans_per_query**|**bigint**|Limita el número máximo de planes almacenados. El valor predeterminado es **200**. Si se alcanza el valor máximo, Almacén de consultas deja de capturar nuevos planes para esa consulta. Si se establece en 0, se quita la limitación con respecto al número de planes capturados.<br /><br /> Cambie mediante la `ALTER DATABASE<database> SET QUERY_STORE (MAX_PLANS_PER_QUERY = <n>)` instrucción.|  
|**query_capture_mode**|**smallint**|El modo de captura de consulta actualmente activo:<br /><br /> **1** = todas las consultas se capturan. Este es el valor de configuración predeterminado para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ( [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] y versiones posteriores).<br /><br /> 2 = captura automática de consultas relevantes según el recuento de ejecución y el consumo de recursos. Es el valor de configuración predeterminado para [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)].<br /><br /> 3 = ninguno: detiene la captura de nuevas consultas. El almacén de consultas seguirá recopilando estadísticas de compilación y tiempo de ejecución para las consultas que ya se capturaron. Use esta configuración con precaución, ya que puede que se pierda la captura de consultas importantes. <br /><br /> 4 = personalizado: permite el control adicional sobre la Directiva de captura de consultas mediante las [Opciones de QUERY_CAPTURE_POLICY](../../t-sql/statements/alter-database-transact-sql-set-options.md#SettingOptions).<br /> **Válido para** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] y versiones posteriores.|  
|**query_capture_mode_desc**|**nvarchar(60)**|Descripción textual del modo de captura real de Almacén de consultas:<br /><br /> TODOS (valor predeterminado para [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] )<br /><br /> **Auto** (valor predeterminado para [!INCLUDE[sqldbesa](../../includes/sqldbesa-md.md)] )<br /><br /> Ninguno <br /><br /> CUSTOM|  
|**capture_policy_execution_count**|**int**|Opción de directiva personalizada del modo de captura de consultas. Define el número de veces que se ejecuta una consulta durante el período de evaluación. El valor predeterminado es 30.<br />**Válido para** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] y versiones posteriores.| 
|**capture_policy_total_compile_cpu_time_ms**|**bigint**|Opción de directiva personalizada del modo de captura de consultas. Define el tiempo total de CPU de compilación transcurrido que usa una consulta durante el período de evaluación. El valor predeterminado es 1000.<br /> **Válido para** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] y versiones posteriores.|
|**capture_policy_total_execution_cpu_time_ms**|**bigint**|Opción de directiva personalizada del modo de captura de consultas. Define el tiempo de ejecución total de CPU transcurrido que ha utilizado una consulta durante el período de evaluación. El valor predeterminado es 100.<br /> **Válido para** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] y versiones posteriores.|
|**capture_policy_stale_threshold_hours**|**int**|Opción de directiva personalizada del modo de captura de consultas. Define el período de intervalo de evaluación para determinar si se debe capturar una consulta. El valor predeterminado es 24 horas.<br /> **Válido para** : [!INCLUDE[ssSQL19](../../includes/sssql19-md.md)] y versiones posteriores.|
|**size_based_cleanup_mode**|**smallint**|Controla si la limpieza se activará automáticamente cuando la cantidad total de datos se acerque al tamaño máximo:<br /><br /> 0 = la limpieza basada en el tamaño no se activará automáticamente.<br /><br /> **1** = la limpieza basada en el tamaño automático se activará automáticamente cuando el tamaño en disco alcance el **90 por ciento** de *max_storage_size_mb*. Es el valor de configuración predeterminado.<br /><br />La limpieza según el tamaño quita primero las consultas menos caras y más antiguas. Se detiene cuando se alcanza aproximadamente el **80 por ciento** de *max_storage_size_mb* .|  
|**size_based_cleanup_mode_desc**|**nvarchar(60)**|Descripción textual del modo de limpieza real basado en el tamaño de Almacén de consultas:<br /><br /> Apagado <br /> **Auto** (valor predeterminado)|  
|**wait_stats_capture_mode**|**smallint**|Controla si Almacén de consultas realiza la captura de las estadísticas de espera: <br /><br /> 0 = OFF <br /> **1** = activado<br /> **Válido para** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores.|
|**wait_stats_capture_mode_desc**|**nvarchar(60)**|Descripción textual del modo real de captura de las estadísticas de espera: <br /><br /> Apagado <br /> **Activado** (valor predeterminado)<br /> **Válido para** : [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)] y versiones posteriores.|
|**actual_state_additional_info**|**nvarchar (8000)**|Actualmente no se usa. Puede implementarse en el futuro.|
  
## <a name="permissions"></a>Permisos  
 Requiere el permiso `VIEW DATABASE STATE`.  
  
## <a name="see-also"></a>Consulte también  
 [sys.query_context_settings &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-context-settings-transact-sql.md)   
 [sys.query_store_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-plan-transact-sql.md)   
 [sys.query_store_query &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-transact-sql.md)   
 [sys.query_store_query_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-query-text-transact-sql.md)   
 [sys.query_store_runtime_stats &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-transact-sql.md)   
 [sys.query_store_wait_stats &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-query-store-wait-stats-transact-sql.md)  
 [sys.query_store_runtime_stats_interval &#40;&#41;de Transact-SQL ](../../relational-databases/system-catalog-views/sys-query-store-runtime-stats-interval-transact-sql.md)   
 [Monitoring Performance By Using the Query Store](../../relational-databases/performance/monitoring-performance-by-using-the-query-store.md)   
 [Vistas de catálogo &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)   
 [sys.fn_stmt_sql_handle_from_sql_stmt &#40;&#41;de Transact-SQL ](../../relational-databases/system-functions/sys-fn-stmt-sql-handle-from-sql-stmt-transact-sql.md)   
 [Procedimientos almacenados del almacén de consultas &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/query-store-stored-procedures-transact-sql.md)  
