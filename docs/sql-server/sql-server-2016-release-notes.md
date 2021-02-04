---
title: Notas de la versión de SQL Server 2016 | Microsoft Docs
description: En estas notas de la versión se describen los problemas conocidos con los que familiarizarse antes de instalar cualquier versión de Microsoft SQL Server 2016 o solucionar problemas en ella.
ms.date: 04/25/2018
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- build notes
- release issues
ms.assetid: c64077a2-bec8-4c87-9def-3dbfb1ea1fb6
author: rothja
ms.author: jroth
monikerRange: = sql-server-2016
ms.openlocfilehash: dad7665ffa825a80aa6bcb3050b04a96060c3f65
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076047"
---
# <a name="sql-server-2016-release-notes"></a>Notas de la versión de SQL Server 2016
[!INCLUDE [SQL Server 2016](../includes/applies-to-version/sqlserver2016.md)]  
  En este artículo se describen las limitaciones y los problemas de las versiones de SQL Server 2016, incluidos los Service Packs. Para obtener más información sobre las novedades, vea el artículo de [novedades de SQL Server 2016](./what-s-new-in-sql-server-2016.md).

- [![Descargar desde el Centro de evaluación](../includes/media/download2.png)](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)  Descargar SQL Server 2016 desde el **[Centro de evaluación](https://www.microsoft.com/evalcenter/evaluate-sql-server-2016)**
- [![Azure Virtual Machine pequeña](../includes/media/azure-vm.png)](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview) ¿Tiene una cuenta de Azure?  Si es así, vaya **[aquí](https://azuremarketplace.microsoft.com/marketplace/apps/microsoftsqlserver.sql2017-ws2019?tab=Overview)** para poner en marcha una máquina virtual con SQL Server 2016 SP1 ya instalado.
- [![Descargar SSMS](../includes/media/download2.png)](../ssms/download-sql-server-management-studio-ssms.md) Para obtener la versión más reciente de SQL Server Management Studio, vea **[Descarga de SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)** .

## <a name="sql-server-2016-service-pack-2-sp2"></a><a name="bkmk_2016sp2"></a>SQL Server 2016 Service Pack 2 (SP2)

![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP2 incluye todas las actualizaciones acumulativas publicadas después de 2016 SP1, hasta e incluyendo CU8.

- [![Centro de descarga de Microsoft](../includes/media/download2.png)](https://www.microsoft.com/download/details.aspx?id=56836) [Descargar SQL Server 2016 Service Pack 2 (SP2)](https://www.microsoft.com/download/details.aspx?id=56836)
- Para obtener una lista completa de las actualizaciones, vea [Información de versión de Service Pack 2 de SQL Server de 2016](https://support.microsoft.com/help/4052908/sql-server-2016-service-pack-2-release-information).

La instalación de SQL Server 2016 SP2 puede requerir el reinicio después de la instalación. Como procedimiento recomendado, se aconseja planear y llevar a cabo un reinicio después de la instalación de SQL Server 2016 SP2.

Mejoras relacionadas con rendimiento y escala incluidas en SQL Server 2016 SP2.

|Característica|Descripción|Más información|
|---|---|---|
|Procedimiento de limpieza de bases de datos de distribución mejorado   |   Las tablas de una base de datos de distribución demasiado grande provocaron la situación de bloqueo e interbloqueo. Un procedimiento de limpieza mejorado aspira a eliminar algunos de estos escenarios de bloqueo o interbloqueo.   |   [KB4040276](https://support.microsoft.com/help/4040276/fix-indirect-checkpoints-on-the-tempdb-database-cause-non-yielding)   |
|Limpieza de seguimiento de cambios   |   Rendimiento y eficacia mejorados de limpieza del seguimiento de cambios para tablas de Change Tracking.   |   [KB4052129](https://support.microsoft.com//help/4052129/update-for-manual-change-tracking-cleanup-procedure-in-sql-server-2016)   |
|Uso del tiempo de espera de CPU para cancelar la solicitud de Resource Governor   |   Mejora el control de las solicitudes de consulta mediante la cancelación de la solicitud, si se alcanzan los umbrales de CPU para una solicitud. Este comportamiento se habilita en la marca de seguimiento 2422.   |   [KB4038419](https://support.microsoft.com/help/4038419/add-cpu-timeout-to-resource-governor-request-max-cpu-time-sec)   |
|SELECT INTO para crear la tabla de destino en el grupo de archivos   |   A partir de SQL Server 2016 SP2, la sintaxis T-SQL de SELECT INTO permite cargar una tabla en un grupo de archivos distinto del grupo de archivos predeterminado del usuario mediante la palabra clave ON <Filegroup name> en la sintaxis T-SQL.   |      |
|Punto de control indirecto mejorado para TempDB   |   El punto de control indirecto para TempDB se mejoró para minimizar la contención de bloqueos por subproceso en DPLists. Esta mejora permite que a carga de trabajo de TempDB en SQL Server 2016 escale de manera predeterminada si el valor del punto de control indirecto es ON (Activado) para TempDB.   |   [KB4040276](https://support.microsoft.com/help/4040276)   |
|Rendimiento mejorado de la copia de seguridad de base de datos en máquinas con memoria de gran tamaño   |   SQL Server 2016 SP2 optimiza la manera en que se purga la E/S en curso durante la copia de seguridad, lo que genera mejoras impresionantes en el rendimiento de la copia de seguridad para bases de datos pequeñas y medianas. Hemos visto una mejora 100 veces mayor cuando se realizan copias de seguridad de base de datos del sistema en una máquina de 2 TB. Esta mejora del rendimiento se va percibiendo cada vez menos a medida que aumenta el tamaño de la base de datos, ya que las páginas de las que hay que hacer una copia de seguridad y la E/S de la copia de seguridad tardan más tiempo en comparación con la iteración del grupo de búferes. Este cambio ayudará a aumentar el rendimiento de copia de seguridad para los clientes que hospedan varias bases de datos pequeñas en servidores grandes de alta gama con gran cantidad de memoria.   |      |
|Compatibilidad de compresión de copia de seguridad de VDI para bases de datos habilitadas para usar TDE   |   SQL Server 2016 SP2 agrega compatibilidad de VDI para permitir que las soluciones de copia de seguridad de VDI usen la compresión para las bases de datos habilitadas para usar TDE. Con esta mejora, se introdujo un nuevo formato de copia de seguridad para admitir la compresión de copia de seguridad para las bases de datos habilitadas para usar TDE. El motor de SQL Server controlará de manera transparente los formatos nuevos y antiguos de copias de seguridad para restaurarlas.   |      |
|Carga dinámica de parámetros de perfil de agente de replicación   |   Esta nueva mejora permite que los parámetros de agentes de replicación se carguen de manera dinámica sin tener que reiniciar el agente. Este cambio solo se aplica a los parámetros de perfil de agente más usados habitualmente.   |      |
|Compatibilidad de la opción MAXDOP para la creación o actualización de estadísticas   |    Esta mejora permite especificar la opción MAXDOP para una instrucción CREATE/UPDATE de estadísticas, así como garantizar que se use la configuración MAXDOP correcta cuando se actualicen las estadísticas como parte de la creación o la recompilación de todos los tipos de índices (si existe la opción MAXDOP).   |   [KB4041809](https://support.microsoft.com/help/4041809)   |
|Actualización automática de estadísticas mejorada para las estadísticas incrementales   |    En determinados escenarios, cuando se generaban diversos cambios de datos en varias particiones de una tabla de manera que el contador de modificaciones totales de las estadísticas incrementadas supera el umbral de las actualizaciones automáticas, pero ninguna de las particiones individuales supera este umbral, la actualización de las estadísticas podía retrasarse hasta que se generaran muchas otras modificaciones en la tabla. Este comportamiento se corrigió en la marca de seguimiento 11024.   |      |

Mejoras relacionadas con la compatibilidad y el diagnóstico incluidas en SQL Server 2016 SP2.

|Característica|Descripción|Más información|
|---|---|---|
|Compatibilidad total de DTC con las bases de datos de un grupo de disponibilidad   |   Las transacciones entre varias bases de datos que no forman parte de un grupo de disponibilidad no se admiten por el momento en SQL Server 2016. Con SQL Server 2016 SP2, introducimos la compatibilidad total de transacciones distribuidas con las bases de datos de grupos de disponibilidad.   |      |
|Actualización a la columna is_encrypted de sys.databases para reflejar con precisión el estado de cifrado de TempDB   |   El valor de la columna is_encryptedcolumn en sys.databases es 1 para TempDB, incluso después de desactivar el cifrado para todas las bases de datos de usuario y reiniciar SQL Server. El comportamiento esperado sería que el valor de este parámetro fuese 0, porque TempDB ya no está cifrado en esta situación. A partir de SQL Server 2016 SP2, sys.databases.is_encrypted ahora refleja con precisión el estado de cifrado de TempDB.   |      |
|Nuevas opciones de DBCC CLONEDATABASE para generar una copia de seguridad y un clon comprobado   |   Con SQL Server 2016 SP2, DBCC CLONEDATABASE permite dos opciones nuevas: generar un clon comprobado o un clon de copia de seguridad. Cuando se crea una base de datos de clonación con la opción WITH VERIFY_CLONEDB, se crea un clon de base de datos coherente y se comprueba que Microsoft lo admitirá para su uso en el entorno de producción. Se ha introducido una propiedad nueva para validar si el clon es del tipo SELECT DATABASEPROPERTYEX("clone_database_name", "IsVerifiedClone") comprobado. Cuando se crea un clon con la opción BACKUP_CLONEDB, se genera una copia de seguridad en la misma carpeta en que está el archivo de datos para facilitar que los clientes muevan el clon a un servidor distinto o lo envíen a Soporte al cliente de Microsoft (CSS) para la solución de problemas.   |      |
|Compatibilidad de Service Broker (SSB) con DBCC CLONEDATABASE   |   Comando DBCC CLONEDATABASE mejorado para permitir el scripting de objetos SSB.   |      |
|Nueva DMV para supervisar el uso del espacio de almacenamiento de versiones de TempDB   |   En SQL Server 2016 SP2 se introduce una nueva DMV de sys.dm_tran_version_store_space_usage para permitir la supervisión de TempDB para el uso del almacenamiento de versiones. Los administradores de base de datos ahora pueden planear de manera proactiva el tamaño de TempDB en función del requisito de uso de almacenamiento de versiones por base de datos, sin ninguna sobrecarga de rendimiento cuando se ejecuta en los servidores de producción.   |      |
|Compatibilidad de volcados de memoria completos con los agentes de replicación | En este momento, si los agentes de replicación detectan una excepción no controlada, la configuración predeterminada es crear un minivolcado de los síntomas de la excepción. Esto dificulta mucho solucionar los problemas de excepciones no controladas. Con este cambio, estamos incorporando una nueva clave del Registro, la que permitiría crear un volcado completo para los agentes de replicación.   |      |
|Mejora de los eventos extendidos para el error de enrutamiento de lectura de un grupo de disponibilidad   |   Antes, el xEvent read_only_rout_fail se desencadenada si había una lista de enrutamiento, pero ninguno de los servidores de esta lista estaba disponible para las conexiones. SQL Server 2016 SP2 incluye información adicional para ayudar a solucionar el problema y además se expande en los puntos de código donde se desencadena este xEvent.   |      |
|Nueva DMV para supervisar el registro de transacciones |   Se ha agregado una nueva DMV sys.dm_db_log_stats que devuelve atributos de nivel de resumen e información sobre los archivos de registro de transacciones de bases de datos.   |      |
|Nueva DMV para supervisar la información de VLF |   Una nueva DMV, sys.dm_db_log_info, se incluye en SQL Server 2016 SP2 para exponer la información de VLF similar a DBCC LOGINFO para supervisar, notificar y prevenir posibles problemas de registro de transacciones experimentados por los clientes.   |      |
|Información del procesador en sys.dm_os_sys_info|   Se agregaron nuevas columnas a la DMV sys.dm_os_sys_info para exponer la información relacionada con el procesador, como socket_count y cores_per_numa.   |      |
|Información modificada de extensiones en sys.dm_db_file_space_usage|   Se agregó una nueva columna a sys.dm_db_file_space_usage para llevar un seguimiento del número de extensiones modificadas desde la última copia de seguridad completa.   |      |
|Información de segmentos en sys.dm_exec_query_stats   |   Se agregaron nuevas columnas a sys.dm_exec_query_stats para llevar un seguimiento del número de segmentos del almacén de columnas que se omitieron y leyeron, como total_columnstore_segment_reads y total_columnstore_segment_skips.   |   [KB4051358](https://support.microsoft.com/help/4051358)   |
|Establecimiento del nivel de compatibilidad correcto para la base de datos de distribución   |   Después de la instalación de Service Pack, el nivel de compatibilidad de la base de datos de distribución cambia a 90. Esto se debe a una ruta de acceso de código en el procedimiento almacenado sp_vupgrade_replication. Ahora el SP se ha modificado para establecer el nivel de compatibilidad correcto para la base de datos de distribución.   |      |
|Exposición de la última información de DBCC CHECKDB correcta conocida   |   Se agregó una nueva opción de base de datos para devolver mediante programación la fecha de la última ejecución correcta de DBCC CHECKDB. Los usuarios ahora pueden consultar DATABASEPROPERTYEX([base de datos], "lastgoodcheckdbtime") para obtener un valor único que represente la fecha y hora de la última ejecución correcta de DBCC CHECKDB en la base de datos especificada.   |      |
|Mejoras del XML del plan de presentación|   [Información sobre las estadísticas que se usaron para compilar el plan de consulta](/archive/blogs/sql_server_team/sql-server-2017-showplan-enhancements), incluido el nombre de la estadística, el contador de modificaciones, el porcentaje de muestreo y la última vez que se actualizaron las estadísticas. Tenga en cuenta que esto solo se agregó para los modelos CE 120 y posteriores. Por ejemplo, no es compatible con CE 70.| |
| |Se ha agregado un nuevo atributo, [EstimateRowsWithoutRowgoal](/archive/blogs/sql_server_team/more-showplan-enhancements-row-goal), al XML del plan de presentación si el optimizador de consultas usa la lógica de "objetivo de filas".| |
| |Nuevos atributos de runtime, [UdfCpuTime y UdfElapsedTime](/archive/blogs/sql_server_team/more-showplan-enhancements-udfs), en el XLM del plan de presentación real para llevar un seguimiento del tiempo empleado en las funciones escalares definidas por el usuario (UDF).| |
| |Se ha agregado el tipo de espera CXPACKET a la [lista de las 10 principales esperas posibles](/archive/blogs/sql_server_team/new-showplan-enhancements) en el XML del plan de presentación real: la ejecución de consultas en paralelo habitualmente implica esperas de CXPACKET, pero este tipo de espera no se informó en el XML del plan de presentación real.   |      |
| |Se extendió la advertencia de desbordamiento en el runtime para informar el número de páginas escritas en TempDB durante un desbordamiento del operador de paralelismo.| |
|Compatibilidad de la replicación con bases de datos con intercalaciones de caracteres complementarios   |   La replicación ahora es compatible en las bases de datos que usan la intercalación de caracteres complementarios.   |      |
|Control adecuado de Service Broker con conmutación por error del grupo de disponibilidad   |   En la implementación actual, cuando Service Broker está habilitado en bases de datos de un grupo de disponibilidad, durante una conmutación por error del grupo de disponibilidad se dejan abiertas todas las conexiones de Service Broker originadas en la réplica principal. Esta mejora apunta a cerrar todas estas conexiones abiertas durante una conmutación por error del grupo de disponibilidad.   |      |
|Mejora en la solución de problemas para las esperas de paralelismo   |   mediante la incorporación de una nueva espera [CXCONSUMER](/archive/blogs/sql_server_team/making-parallelism-waits-actionable).   |      |
|Coherencia mejorada entre las DMV para la misma información   |   La DMV sys.dm_exec_session_wait_stats ahora lleva un seguimiento de las esperas CXPACKET y CXCONSUMER de manera coherente con la DMV sys.dm_os_wait_stats.   |      |
|Mejora en la solución de problemas de los interbloqueos de paralelismo entre consultas | Un nuevo evento extendido exchange_spill para informar el número de páginas escritas en TempDB durante un desbordamiento del operador de paralelismo, en el nombre del campo worktable_physical_writes del xEvent.| |
| |Las columnas de desbordamiento en las DMV sys.dm_exec_query_stats, sys.dm_exec_procedure_stats y sys.dm_exec_trigger_stats (such as total_spills) ahora también incluyen los datos desbordados por los operadores de paralelismo.| |
| |Se mejoró el gráfico de interbloqueo de XML para los escenarios de interbloqueo de paralelismo, donde se agregaron más atributos al recurso exchangeEvent.| |
| |Se mejoró el gráfico de interbloqueo de XML para los interbloqueos que implican los operadores de modo por lotes, con más atributos agregados al recurso SyncPoint.| |
|Recarga dinámica de algunos parámetros de perfil de agente de replicación   |   En la implementación actual de los agentes de replicación, cualquier cambio en el parámetro del perfil del agente requiere que se detenga y reinicie el agente. Esta mejora permite que los parámetros se recarguen de manera dinámica sin tener que reiniciar el agente de replicación.   |      |

![Captura de pantalla de una barra horizontal.](media/horizontal-bar.png)

## <a name="sql-server-2016-service-pack-1-sp1"></a><a name="bkmk_2016sp1"></a>SQL Server 2016 Service Pack 1 (SP1)
![info_tip](../sql-server/media/info-tip.png) SQL Server 2016 SP1 incluye todas las actualizaciones acumulativas hasta SQL Server 2016 RTM CU3, incluida la actualización de seguridad MS16-136. Contiene una acumulación de soluciones proporcionadas en las actualizaciones acumulativas de SQL Server 2016 e incluye la actualización acumulativa más reciente (CU3) y la actualización de seguridad MS16-136, publicada el 8 de noviembre de 2016.

Están disponibles las siguientes características en las ediciones Standard, Web, Express y Base de datos local de SQL Server SP1 (salvo que se indique lo contrario):
- Always Encrypted
- Captura de datos modificada (no disponible en Express)
- columnstore
- Compresión
- Enmascaramiento de datos dinámicos
- Auditoría específica
- OLTP en memoria (no disponible en Base de datos local)
- Varios contenedores de secuencias de archivos (no disponibles en Base de datos local)
- Creación de particiones
- PolyBase
- Seguridad de nivel de fila

En la siguiente tabla se resumen las mejoras principales proporcionadas en SQL Server 2016 SP1.

|Característica|Descripción|Más información|
|---|---|---|
|Inserción masiva en montones con TABLOCK automático en TF 715| La marca de seguimiento 715 permite el bloqueo de tabla para operaciones de carga masiva en un montón sin índices no agrupados.|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster) (La migración de las cargas de trabajo de SAP a SQL Server ahora es 2,5 veces más rápida)|
|CREATE OR ALTER|Implementación de objetos (por ejemplo, procedimientos almacenados, desencadenadores, funciones definidas por el usuario o vistas).|[Blog del Motor de base de datos de SQL Server](/archive/blogs/sqlserverstorageengine/create-or-alter-another-great-language-enhancement-in-sql-server-2016-sp1)|
|Compatibilidad con DROP TABLE para la replicación|Compatibilidad con DROP TABLE DDL para la replicación para que se puedan quitar artículos de replicación.|[KB 3170123](https://support.microsoft.com/help/3170123/supports-drop-table-ddl-for-articles-that-are-included-in-transactiona)|
|Firma del controlador FileStream RsFx|El controlador Filestream RsFx se firma y se certifica mediante el portal del panel del centro de desarrolladores de hardware de Windows (portal de desarrollo), que permite instalar el controlador Filestream RsFx de SQL Server 2016 SP1 en Windows Server 2016 o en Windows 10 sin ningún problema.|[Migrating SAP workloads to SQL Server just got 2.5x faster](/archive/blogs/sql_server_team/migrating-sap-workloads-to-sql-server-just-got-2-5x-faster) (La migración de las cargas de trabajo de SAP a SQL Server ahora es 2,5 veces más rápida)|
|LPIM en una cuenta de servicio SQL: identificación mediante programación|Permite a los DBA identificar mediante programación si el privilegio Bloquear páginas en la memoria (LPIM) está en vigor en el momento de iniciar el servicio.|[Developers Choice: Programmatically identify LPIM and IFI privileges in SQL Server](/archive/blogs/sql_server_team/developers-choice-programmatically-identify-lpim-and-ifi-privileges-in-sql-server) (Elección de los desarrolladores: identificar mediante programación privilegios LPIM e IFI en SQL Server)|
|Limpieza de seguimiento de cambios manual|El nuevo procedimiento almacenado limpia la tabla interna de seguimiento de cambios a petición.| [KB 3173157](https://support.microsoft.com/help/3173157/adds-a-stored-procedure-for-the-manual-cleanup-of-the-change-tracking)|
|Cambios del operador INSERT..SELECT paralelo en las tablas temporales locales|Nuevo operador INSERT paralelo en operaciones INSERT..SELECT.|[Equipo de asesoramiento al cliente de SQL Server](/archive/blogs/sqlcat/real-world-parallel-insert-what-else-you-need-to-know)|
|Showplan XML|Se ha ampliado el diagnóstico, que incluye la advertencia de concesiones y la memoria máxima habilitada para una consulta, marcas de seguimiento habilitadas y la exposición de otros datos de diagnóstico. | [KB 3190761](https://support.microsoft.com/help/3190761/update-to-improve-diagnostics-by-expose-data-type-of-the-parameters-fo)|
|Memoria de la clase de almacenamiento|Mejore el procesamiento de transacciones usando la memoria de la clase de almacenamiento en Windows Server 2016, lo que permite acelerar los tiempos de confirmación de transacciones por órdenes de la magnitud.|[Blog del Motor de base de datos de SQL Server](/archive/blogs/sqlserverstorageengine/transaction-commit-latency-acceleration-using-storage-class-memory-in-windows-server-2016sql-server-2016-sp1)|
|USE HINT|Use la opción de consulta `OPTION(USE HINT('<option>'))` para modificar el comportamiento del optimizador de consultas usando sugerencias de nivel de consulta admitidas. A diferencia de QUERYTRACEON, la opción USE HINT no requiere ningún privilegio de administrador del sistema.|[Developers Choice: USE HINT query hints](/archive/blogs/sql_server_team/developers-choice-use-hint-query-hints) (Elección de los desarrolladores: sugerencias de consulta USE HINT)|
|Adiciones de XEvent|Las nuevas capacidades de diagnóstico de rendimiento y de XEvents mejoran la resolución de problemas de latencia.|[Eventos extendidos](../relational-databases/extended-events/extended-events.md)|

Además, tenga en cuenta las revisiones siguientes:
- A partir de los comentarios de los DBA y de la comunidad de SQL, a partir de SQL 2016 SP1, lo mensajes de registro de Hekaton se han reducido al mínimo.
- Revise las nuevas [marcas de seguimiento](../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md).
- Las versiones completas de las bases de datos de ejemplo de WideWorldImporters ya funcionan con las ediciones Standard y Express a partir de SQL Server 2016 SP1 y están disponibles en [Github]( https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0). En el ejemplo no es necesario efectuar ningún cambio. Las copias de seguridad de base de datos creadas en RTM para la edición Enterprise funcionan con las ediciones Standard y Express en SP1.

La instalación de SQL Server 2016 SP1 puede requerir el reinicio después de la instalación. Como procedimiento recomendado, se aconseja planear y llevar a cabo un reinicio después de la instalación de SQL Server 2016 SP1.

### <a name="download-pages-and-more-information"></a>Páginas de descarga y más información

- [Descargar Service Pack 1 para Microsoft SQL Server 2016](https://www.microsoft.com/download/details.aspx?id=54276)
- [SQL Server 2016 Service Pack 1 (SP1) publicado](/archive/blogs/sqlreleaseservices/sql-server-2016-service-pack-1-sp1-released)
- [Información de lanzamiento de SQL Server 2016 Service Pack 1](https://support.microsoft.com/kb/3182545)
- ![info_tip](../sql-server/media/info-tip.png) [Centro de actualización de SQL Server](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md) para obtener vínculos e información de todas las versiones compatibles, incluidos los Service Packs de [!INCLUDE[ssNoVersion_md](../includes/ssnoversion-md.md)]

![Otra captura de pantalla de una barra horizontal.](media/horizontal-bar.png)

##  <a name="sql-server-2016-release---general-availability-ga"></a><a name="bkmk_2016_ga"></a>SQL Server 2016 Release: Disponibilidad general
-   [Motor de base de datos (disponibilidad general)](#bkmk_ga_instalpatch)
-   [Stretch Database (disponibilidad general)](#bkmk_ga_stretch)
-   [Almacén de consultas (disponibilidad general)](#bkmk_ga_query_store)
-   [Documentación del producto (disponibilidad general)](#bkmk_ga_docs)

### <a name="repl_icon_warn--install-patch-requirement-ga"></a>![repl_icon_warn](../database-engine/availability-groups/windows/media/repl-icon-warn.gif) <a name="bkmk_ga_instalpatch"></a> Requisito de instalación de revisión (GA)
**Problema e impacto en el cliente:** Microsoft ha identificado un problema que afecta a los archivos binarios del Runtime de Microsoft VC++ 2013 que SQL Server 2016 instala como requisito previo. Hay disponible una actualización para corregir este problema. Si esta actualización de los archivos binarios en tiempo de ejecución de VC++ no se instala, puede que SQL Server 2016 experimente problemas de estabilidad en determinados escenarios. Antes de instalar SQL Server 2016, compruebe si el equipo necesita la revisión descrita en [KB 3164398](https://support.microsoft.com/kb/3164398). La revisión también está incluida en el [paquete de actualizaciones acumulativas 1 (CU1) para SQL Server 2016 RTM](https://www.microsoft.com/download/details.aspx?id=53338).

**Resolución:** Pruebe una de estas soluciones:

- Instale [la actualización de Visual C++ 2013 y del paquete redistribuible de Visual C++ (KB 3138367)](https://support.microsoft.com/kb/3138367). KB es el método preferido. Puede instalarla antes o después de instalar SQL Server 2016.

    Si SQL Server 2016 ya está instalado, haga lo siguiente en este orden:

    1.  Descargue el archivo *vcredist_\*exe* que proceda.
    1.  Detenga el servicio SQL Server de todas las instancias del motor de base de datos.
    1.  Instale **KB 3138367**.
    1.  Reinicie el equipo.


 - Instale la  [actualización crítica para requisitos previos de MSVCRT de SQL Server 2016 (KB 3164398)](https://support.microsoft.com/kb/3164398).

    Si se decanta por **KB 3164398**, puede instalar durante la instalación de SQL Server, a través de Microsoft Update o desde el Centro de descarga de Microsoft.

    - **Durante la instalación de SQL Server 2016:** Si el equipo donde se ejecuta el programa de instalación de SQL Server tiene acceso a Internet, el programa de instalación de SQL Server busca la actualización como parte de la instalación global de SQL Server. Si acepta la actualización, el programa de instalación descarga y actualiza los archivos binarios durante la instalación.

    - **Microsoft Update:** La actualización está disponible en Microsoft Update como una actualización crítica de SQL Server 2016 no de seguridad. Si la instalación se realiza a través de Microsoft Update (esto es, después de SQL Server 2016), es necesario reiniciar el servidor después de actualizar.

    - **Centro de descarga:** Por último, la actualización está disponible en el Centro de descarga de Microsoft. Puede descargar el software correspondiente a la actualización e instalarlo en los servidores cuando tengan instalado SQL Server 2016.


### <a name="stretch-database"></a><a name="bkmk_ga_stretch"></a>Stretch Database

#### <a name="problem-with-a-specific-character-in-a-database-or-table-name"></a>Problema con un carácter específico de un nombre de tabla o de base de datos

**Problema e impacto en el cliente:** Al intentar habilitar Stretch Database en una base de datos o una tabla se produce un error. El problema se produce cuando el nombre del objeto incluye un carácter que se trata como otro distinto al convertir de minúsculas a mayúsculas. Un ejemplo de un carácter que provoca este problema es "ƒ" (combinación de teclas: ALT+159).

**Solución alternativa:** Si quiere habilitar Stretch Database en la base de datos o en la tabla, la única opción es cambiar el nombre del objeto y quitar el carácter problemático.

#### <a name="problem-with-an-index-that-uses-the-include-keyword"></a>Problema con un índice en el que se usa la palabra clave INCLUDE

**Problema e impacto en el cliente:** Se produce un error al intentar habilitar Stretch Database en una tabla que tiene un índice en el que se usa la palabra clave INCLUDE para incluir más columnas.

**Solución alternativa:** Quite el índice en el que se usa la palabra clave INCLUDE, habilite Stretch Database en la tabla y luego vuelva a crear el índice. Si lo hace, procure respetar las prácticas y directivas de mantenimiento de su organización para evitar o minimizar cualquier impacto en los usuarios de la tabla afectada.

### <a name="query-store"></a><a name="bkmk_ga_query_store"></a>Query Store

#### <a name="problem-with-automatic-data-cleanup-on-editions-other-than-enterprise-and-developer"></a>Problema con la limpieza automática de los datos en las ediciones distintas de Enterprise y Developer

 **Problema e impacto en el cliente:** Error de la limpieza automática de datos en las ediciones distintas a Enterprise y Developer.
Por lo tanto, si los datos no se purgan de forma manual, el espacio usado por el Almacén de consultas crece con el tiempo hasta que se alcanza el límite configurado. Si no se soluciona, este problema también rellena el espacio en disco asignado a los registros de errores, ya que cada intento de ejecutar una limpieza crea un archivo de volcado. El período de activación de limpieza depende de la frecuencia de la carga de trabajo, pero no es superior a 15 minutos.

 **Solución alternativa:** Si tiene previsto usar Almacén de consultas en ediciones distintas a Enterprise y Developer, tiene que desactivar expresamente las directivas de limpieza. Esto se puede hacer desde SQL Server Management Studio (página Propiedades de la base de datos) o a través de este script de Transact-SQL:

```ALTER DATABASE <database name> SET QUERY_STORE (OPERATION_MODE = READ_WRITE, CLEANUP_POLICY = (STALE_QUERY_THRESHOLD_DAYS = 0), SIZE_BASED_CLEANUP_MODE = OFF)```

Considere también las opciones de limpieza manual para impedir que el almacén de consultas pase a modo de solo lectura. Por ejemplo, ejecute la siguiente consulta para limpiar periódicamente el espacio de datos completo:

```ALTER DATABASE <database name> SET QUERY_STORE CLEAR```

De igual modo, ejecute regularmente los siguientes procedimientos almacenados de almacén de consultas para limpiar las estadísticas de tiempo de ejecución o consultas o planes específicos:

- `sp_query_store_reset_exec_stats`

- `sp_query_store_remove_plan`

- `sp_query_store_remove_query`


###  <a name="product-documentation-ga"></a><a name="bkmk_ga_docs"></a> Documentación del producto (disponibilidad general)
 **Problema e impacto en el cliente:** Aún no hay disponible ninguna versión descargable de la documentación de SQL Server 2016. Cuando use el Administrador de bibliotecas de ayuda para intentar **instalar contenido desde Internet**, verá la documentación de SQL Server 2012 y SQL Server 2014, pero ninguna opción para la documentación de SQL Server 2016.

 **Solución alternativa:** Pruebe una de las siguientes soluciones:

 ![Administración de la configuración de ayuda de SQL Server](../sql-server/media/docs-sql2016-managehelpsettings.png "Administración de la configuración de ayuda de SQL Server")

-   **Elegir Ayuda en línea o local** para configurar la ayuda para "Quiero usar la Ayuda en línea".

-   **Instalar contenido desde Internet** y descargar el contenido de SQL Server 2014.

 **Ayuda F1:** Por diseño, cuando se presiona F1 en [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)], la versión en línea del artículo de F1 Ayuda se muestra en el explorador. El problema es que aparece la ayuda basada en el explorador incluso si se ha configurado e instalado la ayuda local.

**Actualización de contenido:** En SQL Server Management Studio y Visual Studio, la aplicación Visor de Ayuda puede dejar de responder mientras se agrega la documentación. Para solucionar este problema, realice los siguientes pasos. Para obtener más información sobre este problema, vea [Visual Studio Help Viewer freezes](/previous-versions/mt654096(v=vs.140))(El Visor de Ayuda de Visual Studio se bloquea).

* Abra el archivo %LOCALAPPDATA%\Microsoft\HelpViewer2.2\HlpViewer_SSMS16_en-US.settings | HlpViewer_VisualStudio14_en-US.settings en el Bloc de notas y cambie la fecha en el siguiente código a una fecha en el futuro.

```
     Cache LastRefreshed="12/31/2017 00:00:00"
```

## <a name="additional-information"></a>Información adicional
+ [Instalación de SQL Server 2016](../database-engine/install-windows/install-sql-server.md)
+ [Vínculos e información del Centro de actualizaciones de SQL Server sobre todas las versiones compatibles](../database-engine/install-windows/latest-updates-for-microsoft-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]

[!INCLUDE[contribute-to-content](../includes/paragraph-content/contribute-to-content.md)]

![MS_Logo_X-Small](../sql-server/media/ms-logo-x-small.png "MS_Logo_X-Small")
