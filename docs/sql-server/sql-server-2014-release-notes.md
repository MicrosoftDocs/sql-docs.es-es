---
title: Notas de la versión de SQL Server 2014 | Microsoft Docs
description: En estas notas de la versión se describen los problemas conocidos con los que familiarizarse antes de instalar cualquier versión de Microsoft SQL Server 2014 (12.x) o solucionar problemas en ella.
ms.custom: ''
ms.date: 07/22/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: bf4c4922-80b3-4be3-bf71-228247f97004
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: ad4cdfc2ba27051b5186a4934b2afceb42245b49
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076027"
---
# <a name="sql-server-2014-release-notes"></a>SQL Server 2014 Release Notes
[!INCLUDE[tsql-appliesto-ss2014-xxxx-xxxx-xxx-md](../includes/tsql-appliesto-ss2014-xxxx-xxxx-xxx-md.md)]
En este artículo se describen problemas conocidos relacionados con las versiones de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)], incluidos los Service Packs relacionados.

## <a name="sql-server-2014-service-pack-2-sp2"></a>SQL Server 2014 Service Pack 2 (SP2)

SQL Server 2014 SP2 contiene acumulaciones de las revisiones publicadas de SQL Server 2014 SP1 CU7. Contiene mejoras que se centran en el rendimiento, la escalabilidad y el diagnóstico, basadas en los comentarios de los clientes y de la comunidad de SQL.

### <a name="performance-and-scalability-improvements-in-sp2"></a>Mejoras de rendimiento y de escalabilidad en SP2

|Característica|Descripción|Para obtener más información|
|---|---|---|
|Particionamiento automático de Soft NUMA|Puede configurar automáticamente Soft NUMA en los sistemas que notifican ocho o más CPU por nodo NUMA.|[Soft-NUMA (SQL Server)](../database-engine/configure-windows/soft-numa-sql-server.md)|
|Buffer Pool Extension|Permite al grupo de búferes de SQL Server escalar más allá de 8 TB.|[Extensión del grupo de búferes](../database-engine/configure-windows/buffer-pool-extension.md)|
|Escalado de objetos de memoria dinámica| Particiones dinámicas de objeto de memoria según el número de nodos y de núcleos. Esta mejora elimina la necesidad de la marca de seguimiento 8048 después de SQL 2014 SP2.|[Escalado de objetos de memoria dinámica](/archive/blogs/sql_server_team/dynamic-memory-object-scaling)|
|Sugerencia MAXDOP para los comandos DBCC CHECK*|Esta mejora es útil para ejecutar DBCC CHECKDB con una opción MAXDOP que no sea el valor sp_configure.|[Sugerencias (Transact-SQL): consulta](../t-sql/queries/hints-transact-sql-query.md)|
|Mejora en el bloqueo por subproceso de SOS_RWLock|Elimina la necesidad de efectuar un bloqueo por subproceso en SOS_RWLock y aplica técnicas similares a OLTP en memoria. |[SOS_RWLock Redesign](/archive/blogs/psssql/sql-2016-it-just-runs-faster-sos_rwlock-redesign)|
|Implementación nativa espacial|Mejora significativa en el rendimiento de las consultas espaciales.|[Mejoras en el rendimiento espacial en SQL Server 2012 y 2014](https://support.microsoft.com/help/3107399/spatial-performance-improvements-in-sql-server-2012-and-2014)

### <a name="supportability-and-diagnostics-improvements-in-sp2"></a>Mejoras de compatibilidad y diagnóstico en SP2

|Característica|Descripción|Para obtener más información|
|---|---|---|
|Registro del tiempo de espera de AlwaysON|Se ha incorporado una nueva capacidad de registro para los mensajes de tiempo de espera de concesión para que se registre el tiempo actual y los tiempos de renovación previstos. |[Diagnóstico mejorado del tiempo de espera de concesión de los grupos de disponibilidad AlwaysOn](/archive/blogs/alwaysonpro/improved-alwayson-availability-group-lease-timeout-diagnostics)
|XEvents y contadores de rendimiento AlwaysOn|Nuevos contadores de rendimiento y XEvents AlwaysOn para mejorar el diagnóstico al solucionar problemas de latencia con AlwaysOn. |[KB 3107172](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve) y [KB 3107400](https://support.microsoft.com/help/3107400/improved-tempdb-spill-diagnostics-in-showplan-xml-schema-in-sql-server)
|Limpieza de seguimiento de cambios|Un nuevo procedimiento almacenado sp_flush_CT_internal_table_on_demand limpia las tablas internas de seguimiento de cambios a petición.|[KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)
|Clonación de bases de datos|Puede usar el nuevo comando DBCC para solucionar problemas de bases de datos de producción existentes clonando el esquema, los metadatos y las estadísticas, pero sin los datos. Las bases de datos clonadas no están diseñadas para usarse en entornos de producción.|[KB 3177838](https://support.microsoft.com/help/3177838/how-to-use-dbcc-clonedatabase-to-generate-a-schema-and-statistics-only)
|Adiciones de DMF|La nueva DMF sys.dm_db_incremental_stats_properties expone información por cada partición para las estadísticas incrementales.|[KB 3170114](https://support.microsoft.com/help/3170114/update-to-add-dmf-sys-dm-db-incremental-stats-properties-in-sql-server)
|DMF para recuperar el búfer de entrada en SQL Server|Ahora está disponible una nueva DMF para recuperar el búfer de entrada de una sesión o solicitud (sys.dm_exec_input_buffer). Es funcionalmente equivalente a DBCC INPUTBUFFER.|[sys.dm_exec_input_buffer](../relational-databases/system-dynamic-management-views/sys-dm-exec-input-buffer-transact-sql.md)
|Compatibilidad con DROP DDL para la replicación|Permite quitar de la base de datos y de la publicación una tabla que se incluye como un artículo en una publicación de replicación transaccional.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)
|Privilegio de IFI en la cuenta de servicio de SQL|Determina si la inicialización instantánea de archivos (IFI) está en vigor al iniciar el servicio SQL Server.|[Inicialización de archivos de base de datos](../relational-databases/databases/database-instant-file-initialization.md)
|Concesiones de memoria: gestión de problemas|Puede aprovechar las sugerencias de diagnóstico durante la ejecución de consultas restringiendo sus concesiones de memoria para evitar la contención de la memoria.|[KB 3107401](https://support.microsoft.com/help/3107401/new-query-memory-grant-options-are-available-min-grant-percent-and-max)
|Generación de perfiles ligera por cada operador en la ejecución de consultas |Optimiza la recopilación de estadísticas de ejecución de consultas por cada operador (por ejemplo, el número real de filas).|[Developers Choice: Query progress - anytime, anywhere](/archive/blogs/sql_server_team/query-progress-anytime-anywhere) (Elección de los desarrolladores: progreso de la consulta, en cualquier momento y en cualquier lugar)
|Diagnóstico de la ejecución de consultas|El número real de filas leídas ahora se notifica en los planes de ejecución de consultas para mejorar la resolución de problemas del rendimiento de las consultas.|[KB 3107397](https://support.microsoft.com/help/3107397/improved-diagnostics-for-query-execution-plans-that-involve-residual-p)
|Diagnóstico de la ejecución de consultas para el desbordamiento de tempdb|Las advertencias hash y las advertencias de ordenación ahora tienen más columnas para realizar el seguimiento de estadísticas de E/S física, la memoria usada y las filas afectadas. |[Mejora del diagnóstico del desbordamiento de temptdb](https://support.microsoft.com/help/3107172/improve-tempdb-spill-diagnostics-by-using-extended-events-in-sql-serve)
|Compatibilidad con tempdb |Puede usar un nuevo mensaje de registro de errores para el número de archivos de tempdb y los cambios del archivo de datos de tempdb al iniciar el servidor.|[KB 2963384](https://support.microsoft.com/help/2963384/fix-sql-server-crashes-when-the-log-file-of-tempdb-database-is-full-in)


Además, tenga en cuenta las revisiones siguientes:
- La pila de llamadas de XEvent ahora incluye nombres de módulos y desplazamientos en lugar de direcciones absolutas.
- Mejor correlación entre DMV y XE de diagnóstico: query_hash y query_plan_hash se usan para identificar una consulta de forma exclusiva. DMV los define como varbinary (8), mientras que XEvent los define como UINT64. Puesto que SQL Server no tiene "bigint sin signo", la conversión no siempre funciona. Esta mejora presenta nuevas columnas de filtro o acción XEvent equivalentes a query_hash y query_plan_hash, salvo si se definen como INT64. Esta revisión permite correlacionar las consultas entre XE y las DMV.
- Compatibilidad con UTF-8 en BULK INSERT y BCP: se ha incorporado compatibilidad con la exportación e importación de datos codificados en el juego de caracteres UTF-8 en BULK INSERT y BCP.

### <a name="download-pages-and-more-information-for-sp2"></a>Páginas de descarga y más información para SP2

- [Descargar Service Pack 2 para Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=53168)
- [SQL Server 2014 Service Pack 2 ya está disponible](https://www.microsoft.com/download/details.aspx?id=53167)
- [SQL Server 2012 SP2 Express](https://www.microsoft.com/download/details.aspx?id=53167)
- [Feature Pack de SQL Server 2014 SP2](https://www.microsoft.com/download/details.aspx?id=53164)
- [Generador de informes de SQL Server 2014 SP2](https://www.microsoft.com/download/details.aspx?id=53163)
- [Complemento de SQL Server 2014 SP2 Reporting Services para Microsoft Sharepoint](https://www.microsoft.com/download/details.aspx?id=53162)
- [Estadísticas de lenguaje semántico de SQL Server 2014 SP2](https://www.microsoft.com/download/details.aspx?id=53165)
- [Información de lanzamiento de SQL Server 2014 Service Pack 2](https://support.microsoft.com/help/3171021/sql-server-2014-service-pack-2-release-information)

## <a name="sql-server-2014-service-pack-1-sp1"></a>SQL Server 2014 Service Pack 1 (SP1)

SQL Server 2014 SP1 contiene revisiones proporcionadas en SQL Server 2014 CU 1 hasta CU 5 (incluido), así como una acumulación de revisiones enviadas a SQL Server 2012 SP2.

> [!NOTE]
> Si su instancia de SQL Server tiene habilitado el catálogo de SSISDB y recibe un error de instalación al actualizar a SP1, siga las instrucciones descritas para este problema en [Error 912 o 3417 al instalar SQL Server 2014 SP1](https://support.microsoft.com/topic/kb3018269-error-912-or-3417-when-you-install-sql-server-2014-sp1-build-12-0-4050-0-7f34525c-d517-6399-a31b-e7033db13706).

### <a name="download-pages-and-more-information-for-sp1"></a>Páginas de descarga y más información para SP1

- [Descargar Service Pack 1 para Microsoft SQL Server 2014](https://www.microsoft.com/download/details.aspx?id=46694)
- [Se ha publicado SQL Server 2014 Service Pack 1: actualización](/archive/blogs/sqlreleaseservices/sql-server-2014-service-pack-1-has-released-updated)
- [Microsoft SQL Server 2014 SP1 Express](https://www.microsoft.com/download/details.aspx?id=42299)
- [Feature Pack de Microsoft SQL Server 2014 SP1](https://www.microsoft.com/download/details.aspx?id=46694)


## <a name="before-you-install-sql-server-2014-rtm"></a>Antes de instalar SQL Server 2014 RTM

### <a name="limitations-and-restrictions-in-sql-server-2014-rtm"></a>Limitaciones y restricciones de SQL Server 2014 RTM

1.  NO se admite la actualización de SQL Server 2014 CTP 1 a SQL Server 2014 RTM.  
2.  NO se admite la instalación de SQL Server 2014 CTP 1 en paralelo con SQL Server 2014 RTM.  
3.  NO se admite adjuntar o restaurar una base de datos de SQL Server 2014 CTP 1 a SQL Server 2014 RTM.  

**Solución alternativa:** Ninguno.

#### <a name="upgrading-from-sql-server-2014-ctp-2-to-sql-server-rtm"></a>Actualizar de SQL Server 2014 CTP 2 a SQL Server RTM
La actualización es totalmente compatible. En concreto, puede:

1.  Adjuntar una base de datos de SQL Server 2014 CTP 2 a una instancia de SQL Server 2014 RTM.    
2.  Restaurar una copia de seguridad de una base de datos realizada en SQL Server 2014 CTP 2 en una instancia de SQL Server 2014 RTM.    
3.  Actualizar en contexto a SQL Server 2014 RTM.
4.  Realizar una actualización gradual a SQL Server 2014 RTM. Es necesario cambiar al modo de conmutación por error manual antes de iniciar la actualización gradual. Vea [Actualizar servidores de un grupo de disponibilidad con una pérdida de datos y un tiempo de inactividad mínimos](../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md) para obtener más detalles.    
5.  Los datos recopilados por los conjuntos de recopilación de rendimiento de transacción instalados en SQL Server 2014 CTP 2 no se pueden ver mediante SQL Server Management Studio en SQL Server 2014 RTM y viceversa.
  
#### <a name="downgrading-from-sql-server-2014-rtm-to-sql-server-2014-ctp-2"></a>Degradar de SQL Server 2014 RTM a SQL Server 2014 CTP 2  
No se admite esta acción.  
  
**Solución alternativa:** no hay ninguna solución alternativa para la degradación. Se recomienda hacer una copia de seguridad de la base de datos antes de actualizarse a SQL Server 2014 RTM.  
  
#### <a name="incorrect-version-of-streaminsight-client-on-sql-server-2014-mediaisocab"></a>Versión incorrecta del cliente StreamInsight en SQL Server 2014 media/ISO/CAB  
En la ruta de acceso de siguiente de SQL Server media/ISO/CAB (StreamInsight\\\<Architecture\>\\\<Language ID\>) se encuentra la versión incorrecta de StreamInsight.msi y StreamInsightClient.msi.  
  
**Solución alternativa:** descargue e instale la versión correcta del paquete de descarga de [SQL Server 2014 Feature Pack](https://www.microsoft.com/download/details.aspx?id=57474).  
  
### <a name="product-documentation-rtm"></a><a name="ProdDoc"></a>Documentación del producto RTM
  
El contenido del Generador de informes y de PowerPivot no está disponible en algunos idiomas. 

**Problema:** el contenido del Generador de informes no está disponible en los idiomas siguientes:  
  
-   Griego (el-GR)  
-   Noruego (Bokmal) (nb-NO)  
-   Finés (fi-FI)  
-   Danés (DA-DK)  
  
En [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], este contenido estaba disponible en un archivo CHM que se incluía con el producto y estaba disponible en estos idiomas. Los archivos CHM ya no se incluyen con el producto y el contenido del Generador de informes solo está disponible en MSDN. MSDN no admite estos idiomas. El Generador de informes también se quitó de TechNet y ya no está disponible en esos idiomas admitidos.  
  
**Solución alternativa:** Ninguno.  
  
**Problema:** el contenido de Power Pivot no está disponible en los idiomas siguientes:
  
-   Griego (el-GR)  
-   Noruego (Bokmal) (nb-NO)  
-   Finés (fi-FI)  
-   Danés (DA-DK)  
-   Checo (cs-CZ)  
-   Húngaro (hu-HU)  
-   Holandés (Países Bajos) (nl-NL)  
-   Polaco (pl-PL)  
-   Sueco (sv-SE)  
-   Turco (tr-TR)  
-   Portugués (Portugal) (pt-PT)  
  
En [!INCLUDE[ssSQL11](../includes/sssql11-md.md)], este contenido estaba disponible en TechNet y en estos idiomas. Este contenido se quitó de TechNet y ya no está disponible en estos idiomas admitidos.  
  
**Solución alternativa:** Ninguno.  
  
### <a name="database-engine-rtm"></a><a name="DBEngine"></a>Motor de base de datos (RTM)
  
#### <a name="changes-made-for-standard-edition-in-sql-server-2014-rtm"></a>Cambios realizados en la edición Standard de SQL Server 2014 RTM  
SQL Server 2014 Standard incluye los cambios siguientes:  
  
-   La característica de extensión del grupo de búferes permite usar el tamaño máximo de hasta 4 veces la memoria configurada.    
-   La memoria máxima se ha ampliado de 64 a 128 GB.  
 
#### <a name="memory-optimization-advisor-flags-default-constraints-as-incompatible"></a>El Asesor de optimización para memoria marca las restricciones DEFAULT como incompatibles  
**Problema:** el Asesor de optimización para memoria de SQL Server Management Studio marca todas las restricciones DEFAULT como incompatibles. En una tabla optimizada para memoria no se admiten todas las restricciones DEFAULT; el asesor no distingue entre los tipos admitidos y no admitidos de las restricciones DEFAULT. Entre las restricciones DEFAULT admitidas se incluyen todas las constantes, expresiones y funciones integradas que se admiten dentro de los procedimientos almacenados compilados de forma nativa. Para ver la lista de funciones admitidas en los procedimientos almacenados compilados de forma nativa, vea [Construcciones admitidas en procedimientos almacenados compilados de forma nativa](../relational-databases/in-memory-oltp/supported-features-for-natively-compiled-t-sql-modules.md?viewFallbackFrom=sql-server-2014).  
  
**Solución alternativa:** si quiere usar el asesor para identificar bloqueadores, omita las restricciones DEFAULT compatibles. Para usar el Asesor de optimización para memoria con el fin de migrar tablas que tienen restricciones DEFAULT compatibles, pero no otros bloqueadores, siga estos pasos:  
  
1.  Quite la restricción DEFAULT de la definición de tabla.    
2.  Use el asesor para generar un script de migración en la tabla.    
3.  Vuelva a agregar las restricciones DEFAULT al script de migración.    
4.  Ejecute el script de migración.  
  
#### <a name="informational-message-file-access-denied-incorrectly-reported-as-an-error-in-the-sql-server-2014-error-log"></a>El mensaje informativo "acceso a archivo denegado" se ha notificado incorrectamente como un error en el registro de errores de SQL Server 2014  
**Problema:** al reiniciar un servidor que tiene bases de datos que contienen tablas optimizadas para memoria, podría ver el siguiente tipo de mensajes de error en el registro de errores de SQL Server 2014:  
  
```  
[ERROR]Unable to delete file C:\Program Files\Microsoft SQL   
Server\....old.dll. This error may be due to a previous failure to unload   
memory-optimized table DLLs.  
```  
Este mensaje es solo informativo y no se requiere ninguna acción por parte del usuario.  
  
**Solución alternativa:** Ninguno. Este mensaje es meramente informativo.  
  
#### <a name="missing-index-details-incorrectly-report-included-columns-for-memory-optimized-table"></a>Los detalles de índices que faltan indican incorrectamente columnas incluidas para la tabla optimizada para memoria  
**Problema:** si SQL Server 2014 detecta que falta un índice para una consulta en una tabla optimizada para memoria, notificará un índice que falta en SHOWPLAN_XML, así como en las DMV de índices que faltan, como sys.dm_db_missing_index_details. En algunos casos, los detalles de los índices que faltan contendrán columnas incluidas. Como todas las columnas se incluyen implícitamente con todos los índices de las tablas optimizadas para memoria, no se permite especificar explícitamente columnas incluidas con índices optimizados para memoria.  
  
**Solución alternativa:** no especifique la cláusula INCLUDE con índices en tablas optimizadas para memoria.  
  
#### <a name="missing-index-details-omit-missing-indexes-when-a-hash-index-exists-but-is-not-suitable-for-the-query"></a>Los detalles de índices que faltan omiten índices que faltan si un índice hash existe pero no es adecuado para la consulta  
**Problema:** si tiene un índice HASH en columnas de una tabla optimizada para memoria a la que se hace referencia en una consulta, pero el índice no se puede usar para la consulta, SQL Server 2014 no notificará siempre un índice que falta en SHOWPLAN_XML y en la DMV sys.dm_db_missing_index_details.  
  
En concreto, si una consulta contiene predicados de igualdad que implican un subconjunto de las columnas de clave de índice o si contiene predicados de desigualdad que implican las columnas de clave de índice, el índice HASH no se puede usar tal cual y se necesitaría otro índice para ejecutar la consulta de forma eficaz.  
  
**Solución alternativa:** en caso de que esté usando índices hash, inspeccione las consultas y los planes de consulta para determinar si las consultas pueden beneficiarse de operaciones Index Seek en un subconjunto de la clave de índice, o de operaciones Index Seek en predicados de desigualdad. Si necesita buscar en un subconjunto de la clave de índice, use un índice NO CLÚSTER, o use un índice HASH solamente en las columnas en las que necesita buscar. Si necesita buscar en un predicado de desigualdad, use un índice NO CLÚSTER en lugar de HASH.  
  
#### <a name="failure-when-using-a-memory-optimized-table-and-memory-optimized-table-variable-in-the-same-query-if-the-database-option-read_committed_snapshot-is-set-to-on"></a>Error al usar una tabla optimizada para memoria y una variable de tabla optimizada para memoria en la misma consulta, si la opción de base de datos READ_COMMITTED_SNAPSHOT está establecida en ON  
**Problema:** si la opción de base de datos READ_COMMITTED_SNAPSHOT está establecida en ON, y tiene acceso a una tabla optimizada para memoria y a una variable de tabla optimizada para memoria en la misma instrucción en un contexto que no sea de transacción de usuario, puede aparecer este mensaje de error:  
  
```  
Msg 41359  
A query that accesses memory optimized tables using the READ COMMITTED  
isolation level, cannot access disk based tables when the database option  
READ_COMMITTED_SNAPSHOT is set to ON. Provide a supported isolation level  
for the memory optimized table using a table hint, such as WITH (SNAPSHOT).  
```  
  
**Solución alternativa:** use la sugerencia de tabla WITH (SNAPSHOT) con la variable de tabla, o establezca la opción de base de datos MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT en ON, con la siguiente instrucción:  
  
```  
ALTER DATABASE CURRENT   
SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON  
```  
  
#### <a name="procedure-and-query-execution-statistics-for-natively-compiled-stored-procedures-record-worker-time-in-multiples-of-1000"></a>Las estadísticas de ejecución de procedimientos y consultas para los procedimientos almacenados compilados de forma nativa registran el tiempo de trabajo en múltiplos de 1.000  
**Problema:** después de habilitar la recopilación de estadísticas de ejecución de procedimientos o de consultas para los procedimientos almacenados compilados de forma nativa mediante sp_xtp_control_proc_exec_stats o sp_xtp_control_query_exec_stats, verá que *_worker_time se muestra en múltiplos de 1000 en las DMV sys.dm_exec_procedure_stats y sys.dm_exec_query_stats. Las ejecuciones de consultas que tienen un tiempo de trabajo inferior a 500 microsegundos se notificarán con un valor de worker_time de 0.  
  
**Solución alternativa:** Ninguno. No se fie del valor de worker_time notificado en las DMV de estadísticas de ejecución para las consultas de ejecución breve en procedimientos almacenados compilados de forma nativa.  
  
#### <a name="error-with-showplan_xml-for-natively-compiled-stored-procedures-that-contain-long-expressions"></a>Error con SHOWPLAN_XML para los procedimientos almacenados compilados de forma nativa que contienen expresiones largas  
**Problema:** si un procedimiento almacenado compilado de forma nativa contiene una expresión larga, al obtener SHOWPLAN_XML para el procedimiento, ya sea con la opción T-SQL establecida en SET SHOWPLAN_XML ON o mediante la opción "Mostrar plan de ejecución estimado" de Management Studio, puede aparecer el error siguiente:  
  
```  
Msg 41322. MAT/PIT export/import encountered a failure for memory  
optimized table or natively compiled stored procedure with object ID  
278292051 in database ID 6. The error code was  
0xc00cee81.  
```  
  
**Solución alternativa:** Hay dos soluciones alternativas:  
  
1.  Agregar paréntesis a la expresión, de manera similar al ejemplo siguiente:  
  
    En lugar de:  
  
    ```  
    SELECT @v0 + @v1 + @v2 + ... + @v199  
    ```  
  
    Escritura:  
  
    ```  
    SELECT((@v0 + ... + @v49) + (@v50 + ... + @v99)) + ((@v100 + ... + @v149) + (@v150 + ... + @v199))  
    ```  
  
2.  Crear un segundo procedimiento con una expresión ligeramente simplificada, para el plan de presentación: la forma general del plan debe ser al misma. Por ejemplo, en lugar de:  
  
    ```  
    SELECT @v0 +@v1 +@v2 +...+@v199  
    ```  
  
    Escritura:  
  
    ```  
    SELECT @v0 +@v1  
    ```  
  
#### <a name="using-a-string-parameter-or-variable-with-datepart-and-related-functions-in-a-natively-compiled-stored-procedure-results-in-an-error"></a>El uso de un parámetro o una variable de cadena con DATEPART y funciones relacionadas en un procedimiento almacenado compilado de forma nativa produce un error  
**Problema:** al usar un procedimiento almacenado que se ha compilado de forma nativa y que usa un parámetro de cadena o una variable con las funciones integradas DATEPART, DAY, MONTH y YEAR, se muestra un mensaje de error que indica que no se admite datetimeoffset con procedimientos almacenados que se han compilado de forma nativa.  
  
**Solución alternativa:** asigne el parámetro o la variable de cadena a una nueva variable de tipo datetime2 y use esa variable en la función DATEPART, DAY, MONTH o YEAR. Por ejemplo:  
  
```  
DECLARE @d datetime2 = @string  
DATEPART(weekday, @d)  
```  
  
#### <a name="native-compilation-advisor-flags-delete-from-clauses-incorrectly"></a>El Asistente de compilación nativa marca incorrectamente las cláusulas DELETE FROM  
**Problema:** el Asistente de compilación nativo marca incorrectamente las cláusulas DELETE FROM dentro de un procedimiento almacenado como incompatibles.  
  
**Solución alternativa:** Ninguno.  
  
#### <a name="register-through-ssms-adds-dac-meta-data-with-mismatched-instance-ids"></a>el registro mediante SSMS agrega metadatos de DAC con identificadores de instancia no coincidentes  
**Problema:** al registrar o eliminar un paquete de aplicación de capa de datos (.dacpac) mediante SQL Server Management Studio, las tablas sysdac* no se actualizan correctamente para que un usuario pueda consultar el historial de dacpac de la base de datos.  Los valores de instance_id en sysdac_history_internal y sysdac_instances_internal no coinciden para permitir una combinación.  
  
**Solución alternativa:** este problema se corrige con la redistribución del Feature Pack de [Data-Tier Application Framework](https://www.microsoft.com/download/details.aspx?id=100297).  Una vez aplicada la actualización, todas las nuevas entradas del historial usarán el valor mostrado para instance_id en la tabla sysdac_instances_internal.  
  
Si ya tiene el problema con valores no coincidentes de instance_id, la única manera de corregir los valores no coincidentes es conectarse al servidor como un usuario con privilegios para escribir en la base de datos MSDB y actualizar los valores de instance_id para que coincidan.  Si recibe varios eventos de registro y anulación de registro de la misma base de datos, puede que necesite ver la fecha y hora para ver qué registros coinciden con el valor actual de instance_id.  
  
1.  Conéctese al servidor en SQL Server Management Studio con un inicio de sesión que tenga permisos de actualización en MSDB.    
2.  Abra una consulta nueva usando la base de datos MSDB.    
3.  Ejecute esta consulta para ver todas las instancias dac activas.  Busque la instancia que quiere corregir y anote el valor de instance_id:  
  
    `select * from` sysdac_instances_internal  
  
4.  Ejecute esta consulta para ver todas las entradas del historial:  
  
    `select * from` sysdac_history_internal  
  
5.  Identifique las filas que deben corresponder a la instancia que está corrigiendo. 
6.  Actualice el valor de sysdac_history_internal.instance_id al valor que anotó en el paso 3 (de la tabla sysdac_instances_internal):  
  
    `update` sysdac_history_internal `set` instance_id = "\<value from step 3\>" `where` \<expression that matches the rows you want to update\>  
  
### <a name="reporting-services-rtm"></a><a name="SSRS"></a>Reporting Services (RTM)
  
#### <a name="the-sql-server-2012-reporting-services-native-mode-report-server-cannot-run-side-by-side-with-sql-server-2014-reporting-services-sharepoint-components"></a>El servidor de informes en modo nativo de SQL Server 2012 Reporting Services no puede ejecutarse en paralelo con los componentes de SharePoint de SQL Server 2014 Reporting Services  
**Problema:** el servicio de Windows de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] en modo nativo "SQL Server Reporting Services" (ReportingServicesService.exe) no se puede iniciar si hay componentes de SharePoint de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] instalados en el mismo servidor.  
  
**Solución alternativa:** desinstale los componentes de SharePoint de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] y reinicie el servicio de Windows Microsoft SQL Server 2012 Reporting Services.  
  
**Más información:**  
  
[!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] en modo nativo no se puede ejecutar en paralelo bajo ninguna de las siguientes condiciones:  
  
-   Complemento de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] para productos de SharePoint    
-   Servicio compartido de SharePoint de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]  
  
La instalación en paralelo impide el inicio del servicio de Windows en modo nativo [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)]. Aparecerán mensajes de error, similares a los que se muestran aquí, en el registro de eventos de Windows:  
  
```  
Log Name:   Application  
Source:          Report Server (<SQL instance ID>)  
Event ID:        117  
Task Category:   Startup/Shutdown  
Level:           Error  
Keywords:        Classic  
Description:     The report server database is an invalid version.  
  
Log Name:      Application  
Source:        Report Server (<SQL instance ID>)  
Event ID:      107  
Task Category: Management  
Level:         Error  
Keywords:      Classic  
Description:   Report Server (DENALI) cannot connect to the report server database.  
```  
  
Para obtener más información, vea [Sugerencias, trucos y solución de problemas de SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
#### <a name="required-upgrade-order-for-multi-node-sharepoint-farm-to-sql-server-2014-reporting-services"></a>Orden necesario para actualizar una granja de servidores de SharePoint de varios nodos a SQL Server 2014 Reporting Services  
**Problema:** se produce un error en la representación de informes en una granja de varios nodos si las instancias del servicio compartido de SharePoint de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] se actualizan antes que todas las instancias del Complemento de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] para productos de SharePoint.  
  
**Solución alternativa:** en una granja de SharePoint de varios nodos:  
  
1.  Actualice primero todas las instancias del Complemento de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] para productos de SharePoint.    
2.  Actualice después todas las instancias del servicio compartido de SharePoint de [!INCLUDE[ssRSnoversion](../includes/ssrsnoversion-md.md)] .  
  
Para obtener más información, vea [Sugerencias, trucos y solución de problemas de SQL Server 2014 Reporting Services](https://go.microsoft.com/fwlink/?LinkID=391254).  
  
### <a name="sql-server-2014-rtm-on-azure-virtual-machines"></a><a name="AzureVM"></a>SQL Server 2014 RTM en máquinas virtuales de Azure  
  
#### <a name="the-add-azure-replica-wizard-returns-an-error-when-configuring-an-availability-group-listener-in-azure"></a>El Asistente para agregar réplicas de Azure devuelve un error al configurar un agente de escucha del grupo de disponibilidad en Azure  
**Problema:** Si un grupo de disponibilidad tiene un agente de escucha, el Asistente para crear réplicas de Azure devolverá un error al intentar configurar el agente de escucha en Azure.  
  
Este problema se debe a que los agentes de escucha de los grupos de disponibilidad necesitan que se asigne una dirección IP a cada subred que hospede réplicas de grupos de disponibilidad, incluida la subred de Azure.  
  
**Solución alternativa:**  
  
1.  En la página Agente de escucha, asigne una dirección IP estática disponible en la subred de Azure que hospedará la réplica del grupo de disponibilidad al Agente de escucha del grupo de disponibilidad.  
  
    Esta solución alternativa permitirá que el asistente agregue la réplica a Azure.  
  
2.  Cuando se complete el asistente, necesitará finalizar la configuración del agente de escucha en Azure como se describe en [Configuración del agente de escucha de los Grupos de disponibilidad AlwaysOn en Azure](/previous-versions/azure/dn376546(v=azure.100)).  
  
### <a name="analysis-services-rtm"></a><a name="SSAS"></a>Analysis Services (RTM)
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2010-new-farm-configured-with-sql-server-2014"></a>Se debe descargar, instalar y registrar MSOLAP.5 para una nueva granja de SharePoint 2010 configurada con SQL Server 2014  
**Problema:**  
  
-   En SharePoint 2010 se debe descargar, instalar y registrar MSOLAP.5 para una nueva granja de SharePoint 2013 configurada con SQL Server 2014, configurada con una implementación de SQL Server 2014 RTM, libros PowerPivot no se pueden conectar a los modelos de datos porque el proveedor al que se hace referencia en la cadena de conexión no está instalado.  
  
**Solución alternativa:**  
  
1.  Descargue el proveedor MSOLAP.5 desde [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] Feature Pack. Instale el proveedor en los servidores de aplicaciones que ejecutan Excel Services. Para obtener más información, vea la sección "Proveedor OLE DB de Microsoft Analysis Services para Microsoft SQL Server 2012 SP1" de [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Registre MSOLAP.5 como proveedor de confianza con Servicios de Excel de SharePoint. Para obtener más información, vea [Agregar MSOLAP.5 como proveedor de datos de confianza en Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Más información:**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] incluye MSOLAP.6. [!INCLUDE[ssSQL11](../includes/sssql11-md.md)] y [!INCLUDE[ssSQL14](../includes/sssql14-md.md)][!INCLUDE[ssGemini](../includes/ssgemini-md.md)] usan MSOLAP.5. Si MSOLAP.5 no está instalado en el equipo en el que se ejecuta Servicios de Excel, Servicios de Excel no puede cargar los modelos de datos.  
  
#### <a name="msolap5-must-be-downloaded-installed-and-registered-for-a-sharepoint-2013-new-farm-configured-with-sql-server-2014"></a>Se debe descargar, instalar y registrar MSOLAP.5 para una nueva granja de SharePoint 2013 configurada con SQL Server 2014  
**Problema:**  
  
-   En una granja de SharePoint 2013 configurada con una implementación de [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] , libros de Excel que hacen referencia al proveedor MSOLAP.5 no se pueden conectar a los modelos de datos tabulares porque el proveedor al que se hace referencia en la cadena de conexión no está instalado.  
  
**Solución alternativa:**  
  
1.  Descargue el proveedor MSOLAP.5 desde [!INCLUDE[ssSQL11SP1](../includes/sssql11sp1-md.md)] Feature Pack. Instale el proveedor en los servidores de aplicaciones que ejecutan Excel Services. Para obtener más información, vea la sección "Proveedor OLE DB de Microsoft Analysis Services para Microsoft SQL Server 2012 SP1" de [Microsoft SQL Server 2012 SP1 Feature Pack](https://www.microsoft.com/download/details.aspx?id=35575).  
  
2.  Registre MSOLAP.5 como proveedor de confianza con Servicios de Excel de SharePoint. Para obtener más información, vea [Agregar MSOLAP.5 como proveedor de datos de confianza en Excel Services](/analysis-services/power-pivot-for-sharepoint-ssas?viewFallbackFrom=sql-server-ver15).  
  
**Más información:**  
  
-   [!INCLUDE[ssSQL14](../includes/sssql14-md.md)] incluye MSOLAP.6. Pero los libros PowerPivot de SQL Server 2014 usan MSOLAP.5. Si MSOLAP.5 no está instalado en el equipo en el que se ejecuta Servicios de Excel, Servicios de Excel no puede cargar los modelos de datos.  
  
#### <a name="corrupt-data-refresh-schedules-rtm"></a>Programaciones de actualización de datos dañadas (RTM)
**Problema:**  
  
-   Actualiza una programación de actualización y la programación resulta dañada y no se puede usar.  
  
**Solución alternativa:**  
  
1.  En Microsoft Excel, desactive las propiedades avanzadas personalizadas. Vea la sección "Solución alternativa" del siguiente artículo [KB 2927748](https://support.microsoft.com/kb/2927748) de Knowledge Base.  
  
**Más información:**  
  
-    Si la longitud serializada de la programación de actualización es menor que la de la programación original, al actualizar una programación de actualización de datos para un libro, el tamaño de búfer no se actualiza correctamente y la nueva información de programación se combina con la información de programación anterior, dando lugar a una programación dañada.  
  
### <a name="data-quality-services-rtm"></a><a name="DQS"></a>Data Quality Services (RTM)
  
#### <a name="no-cross-version-support-for-data-quality-services-in-master-data-services"></a>No hay compatibilidad entre versiones con Data Quality Services de Master Data Services  
**Problema:** Los siguientes escenarios no se admiten:  
  
-   Master Data Services 2012 hospedado en una base de datos de motor de base de datos de SQL Server en SQL Server 2012 con Data Quality Services 2012 instalado.  
  
-   Master Data Services 2012 hospedado en una base de datos de motor de base de datos de SQL Server en SQL Server 2014 con Data Quality Services 2014 instalado.  
  
**Solución alternativa:** use la misma versión de Master Data Services que la base de datos del motor de base de datos y Data Quality Services.  
  
### <a name="upgrade-advisor-issues-rtm"></a><a name="UA"></a>Problemas del Asesor de actualizaciones (RTM)
  
#### <a name="sql-server-2014-upgrade-advisor-reports-irrelevant-upgrade-issues-for-sql-server-reporting-services"></a>El Asesor de actualizaciones de SQL Server 2014 notifica problemas irrelevantes de actualización de SQL Server Reporting Services  
**Problema:** el Asesor de actualizaciones de SQL Server (SSUA) incluido con SQL Server 2014 notifica incorrectamente varios errores al analizar el servidor de SQL Server Reporting Services.  
  
**Solución alternativa:** este problema se ha corregido en el Asesor de actualizaciones de SQL Server incluido en el [SQL Server 2014 Feature Pack para SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
#### <a name="sql-server-2014-upgrade-advisor-reports-an-error-when-analyzing-sql-server-integration-services-server"></a>El Asesor de actualizaciones de SQL Server 2014 notifica un error al analizar el servidor de SQL Server Integration Services  
**Problema:** el Asesor de actualizaciones de SQL Server (SSUA) incluido con SQL Server 2014 notifica un error al analizar el servidor de SQL Server Integration Services.  El error que se muestra al usuario es:  
  
```  
The installed version of Integration Services does not support Upgrade Advisor.   
The assembly information is "Microsoft.SqlServer.ManagedDTS, Version=11.0.0.0,   
Culture=neutral, PublicKeyToken=89845dcd8080cc91  
```  
  
**Solución alternativa:** este problema se ha corregido en el Asesor de actualizaciones de SQL Server incluido en el [SQL Server 2014 Feature Pack para SSUA](https://www.microsoft.com/download/details.aspx?id=57474).  
  
[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
