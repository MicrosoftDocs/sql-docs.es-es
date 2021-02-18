---
title: Novedades de SQL Server 2019 | Microsoft Docs
description: Obtenga información sobre las nuevas características de SQL Server 2019 (15.x), que proporciona opciones de lenguajes de desarrollo, tipos de datos, entornos y sistemas operativos.
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: aadb59645e474557a92e3e87a0cd9c0c50df0b46
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271752"
---
# <a name="whats-new-in-sql-server-2019"></a>Novedades de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] se basa en versiones anteriores para potenciar SQL Server como una plataforma que proporciona diversas opciones de lenguajes de desarrollo, tipos de datos, entornos locales o en la nube, y sistemas operativos.

En este artículo se resumen las nuevas características y mejoras de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

Para obtener más información y consultar los problemas conocidos, vea [Notas de la versión de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](sql-server-version-15-release-notes.md).

Para obtener la mejor experiencia con [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], use las [herramientas más recientes](../azure-data-studio/download-azure-data-studio.md).

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] presenta [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] para [!INCLUDE[sql-server](../includes/ssnoversion-md.md)]. También proporciona funciones y mejoras adicionales para el motor de base de datos de SQL Server, SQL Server Analysis Services, SQL Server Machine Learning Services, SQL Server en Linux y SQL Server Master Data Services.

En el vídeo siguiente se proporciona una introducción a SQL Server 2019 de 13 minutos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-SQL-Server-2019/player?WT.mc_id=dataexposed-c9-niner]


En las secciones siguientes se proporciona información general sobre estas características.

## <a name="data-virtualization-and-big-data-clusters-2019"></a>Virtualización de datos y [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]

Hoy en día, las empresas son propietarias de ingentes cantidades de datos que constan de una amplia gama de conjuntos de datos en constante crecimiento y que están hospedados en orígenes de datos almacenados en toda la empresa. Obtenga información casi en tiempo real de todos los datos con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)], que proporcionan un entorno completo para trabajar con grandes conjuntos de datos, incluidas funciones de inteligencia artificial y aprendizaje automático.

| Nueva característica o actualización | Detalles |
|:---|:---|
| Solución de macrodatos escalable | [Implementación de clústeres escalables](../big-data-cluster/deploy-get-started.md) de contenedores de SQL Server, Spark y HDFS que se ejecutan en Kubernetes. <br/><br/> Leer, escribir y procesar macrodatos desde Transact-SQL o Spark.<br/><br/> Combinar y analizar de forma sencilla datos relacionales de alto valor con macrodatos de gran volumen.<br/><br/>Consultar orígenes de datos externos.<br/><br/>Almacenar macrodatos en HDFS administrados mediante SQL Server.<br/><br/>Consultar datos de varios orígenes de datos externos a través del clúster.<br/><br/> Usar los datos para tareas de inteligencia artificial, aprendizaje automático y otras tareas de análisis.<br/><br/> [Implementar y ejecutar aplicaciones](../big-data-cluster/concept-application-deployment.md) en [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)]. <br/><br/> La instancia maestra de SQL Server proporciona alta disponibilidad y recuperación ante desastres para todas las bases de datos mediante la tecnología de grupos de disponibilidad AlwaysOn.<br/>|
|Virtualización de datos con PolyBase | Consulte datos de orígenes de datos externos de SQL Server, Oracle, Teradata, MongoDB y ODBC con tablas externas, ahora [compatibles con la codificación UTF-8](../relational-databases/collations/collation-and-unicode-support.md). Para obtener más información, vea [¿Qué es PolyBase?](../relational-databases/polybase/polybase-guide.md).|
| &nbsp; | &nbsp; |

Para obtener más información, vea [¿Qué son los [!INCLUDE[big-data-clusters](../includes/ssbigdataclusters-nover.md)] de SQL Server?](../big-data-cluster/big-data-cluster-overview.md).

## <a name="intelligent-database"></a>Base de datos inteligente
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] mejora las innovaciones de las versiones anteriores para proporcionar un rendimiento inmediato líder del sector. Desde el [procesamiento de consultas inteligente](../relational-databases/performance/intelligent-query-processing.md) hasta la compatibilidad con dispositivos de memoria persistente, las características de base de datos inteligente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mejoran el rendimiento y la escalabilidad de todas las cargas de trabajo de bases de datos sin realizar cambios en el diseño de la aplicación o la base de datos.

### <a name="intelligent-query-processing"></a>Procesamiento de consultas inteligentes
Con el [procesamiento de consultas inteligente](../relational-databases/performance/intelligent-query-processing.md), las cargas de trabajo paralelas críticas mejoran cuando se ejecutan a escala. Y, al mismo tiempo, continúan adaptándose al mundo de los datos que cambia constantemente. El procesamiento de consultas inteligente está disponible de forma predeterminada en la configuración más reciente del [nivel de compatibilidad de la base de datos](../t-sql/statements/alter-database-transact-sql-compatibility-level.md#differences-between-compatibility-level-140-and-level-150), lo cual proporciona un gran impacto que mejora el rendimiento de las cargas de trabajo existentes con un esfuerzo de implementación mínimo.

|Nueva característica o actualización | Detalles |
|:---|:---|
|Comentarios de concesión de memoria del modo de fila |Se expande en la característica de comentarios de concesión de memoria de modo de proceso por lotes al ajustar los tamaños de concesión de memoria tanto para los operadores del modo de proceso por lotes como del modo de fila. Este ajuste puede corregir automáticamente las concesiones excesivas que producen una pérdida de memoria y una simultaneidad reducida. También puede corregir las concesiones de memoria insuficiente que provocan un costoso desbordamiento en disco. Consulte [Comentarios de concesión de memoria del modo de fila](../relational-databases/performance/intelligent-query-processing.md#row-mode-memory-grant-feedback). |
|Modo por lotes en el almacén de filas | Permite la ejecución en modo por lotes sin necesidad de índices de almacén de columnas. La ejecución en modo por lotes usa la CPU de manera más eficaz durante las cargas de trabajo analíticas, aunque, hasta [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], solo se usaba cuando una consulta incluía operaciones con índices de almacén de columnas. Pero puede que algunas aplicaciones usen características que no son compatibles con los índices de almacén de columnas y, por lo tanto, no pueden aprovechar el modo por lotes. A partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], el modo por lotes está habilitado en las cargas de trabajo analíticas válidas cuyas consultas incluyan operaciones con cualquier tipo de índice (almacén de filas o almacén de columnas). Consulte [Modo por lotes en el almacén de filas](../relational-databases/performance/intelligent-query-processing.md#batch-mode-on-rowstore). |
|Inserción de UDF escalares|Transforma automáticamente las UDF escalares en expresiones relacionales y las inserta en la consulta SQL de llamada. Esta transformación mejora el rendimiento de las cargas de trabajo que aprovechan las UDF escalares. Vea [Inserción de UDF escalar](../relational-databases/performance/intelligent-query-processing.md#scalar-udf-inlining).|
|Compilación diferida de variables de tabla|Mejora la calidad del plan y el rendimiento general de las consultas que hacen referencia a las variables de tabla. Durante la optimización y la compilación inicial, esta característica propaga las estimaciones de cardinalidad que se basan en los recuentos de filas de variables de tabla reales. Esta información precisa del recuento de filas optimiza las operaciones del plan de bajada. Consulte [Compilación diferida de variables de tabla](../relational-databases/performance/intelligent-query-processing.md#table-variable-deferred-compilation). |
|Procesamiento de consultas aproximado con `APPROX_COUNT_DISTINCT` |En escenarios en los que la precisión absoluta no es importante, pero la capacidad de respuesta es crítica, se agrega `APPROX_COUNT_DISTINCT` en grandes conjuntos de datos con menos recursos que `COUNT(DISTINCT())` para obtener una simultaneidad superior. Consulte [Procesamiento de consultas aproximado](../relational-databases/performance/intelligent-query-processing.md#approximate-query-processing).|
| &nbsp; | &nbsp; |


### <a name="in-memory-database"></a>Base de datos en memoria
Las tecnologías de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] de [base de datos en memoria](../relational-databases/in-memory-database.md) aprovechan la innovación de hardware moderna para ofrecer un rendimiento y una escalabilidad sin precedentes. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] se basa en las innovaciones anteriores en esta área, como el procesamiento de transacciones en línea (OLTP) en memoria, para impulsar un nuevo nivel de escalabilidad en todas las cargas de trabajo de base de datos.  

|Nueva característica o actualización | Detalles |
|:---|:---|
|Grupo de búferes híbrido| Nueva característica del [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)], en que se accede directamente a las páginas de base de datos ubicadas en archivos de base de datos presentes en un dispositivo de memoria persistente (PMEM) cuando sea necesario. Vea [Grupo de búferes híbrido](../database-engine/configure-windows/hybrid-buffer-pool.md).|
|Metadatos tempdb optimizados para memoria| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce una nueva característica que forma parte de la familia de características [Base de datos en memoria](../relational-databases/in-memory-database.md), metadatos TempDB optimizados para memoria, que quita este cuello de botella de forma eficaz y desbloquea un nuevo nivel de escalabilidad para cargas de trabajo intensivas de TempDB. En [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], las tablas del sistema implicadas en la administración de metadatos de la tabla temporal del sistema se pueden mover a tablas optimizadas para memoria no duraderas y sin bloqueos temporales. Vea [Metadatos tempdb optimizados para memoria](../relational-databases/databases/tempdb-database.md#memory-optimized-tempdb-metadata).|
| Compatibilidad de OLTP en memoria para instantáneas de base de datos | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduce compatibilidad para crear [instantáneas de base de datos](../relational-databases/databases/database-snapshots-sql-server.md) de bases de datos que incluyen grupos de archivos optimizados para memoria. |
| &nbsp; | &nbsp; |

### <a name="intelligent-performance"></a>Rendimiento inteligente
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] se basa en las innovaciones en la base de datos inteligente de versiones anteriores para garantizar que [funciona más rápido](/archive/blogs/bobsql/). Estas mejoras ayudan a superar cuellos de botella de recursos conocidos y proporcionan varias opciones para configurar el servidor de base de datos para que ofrezca un rendimiento predecible en todas las cargas de trabajo.

|Nueva característica o actualización | Detalles |
|:---|:---|
|`OPTIMIZE_FOR_SEQUENTIAL_KEY`|Activa una optimización en el [!INCLUDE[ssDEnoversion](../includes/ssdenoversion-md.md)] que ayuda a mejorar el rendimiento de las inserciones de alta simultaneidad en el índice. Esta opción está diseñada para los índices que son propensos a la contención de la inserción de la última página, que suele darse con índices que tienen una clave secuencial, como una columna de identidad, de secuencia o de fecha y hora. Consulte [CREATE INDEX](../t-sql/statements/create-index-transact-sql.md#sequential-keys).|
|Forzado de cursores estáticos y de avance rápido | Proporciona un plan de Almacén de consultas para forzar la compatibilidad con cursores estáticos y de avance rápido. Vea [Plan para forzar la compatibilidad con cursores estáticos y de avance rápido](../relational-databases/performance/monitoring-performance-by-using-the-query-store.md#ctp23).|
|Regulación de recursos| El tipo de datos del valor configurable para la opción `REQUEST_MAX_MEMORY_GRANT_PERCENT` de `CREATE WORKLOAD GROUP` y `ALTER WORKLOAD GROUP` se ha cambiado de "integer" a "float" para permitir un mayor control granular de los límites de la memoria. Consulte [ALTER WORKLOAD GROUP](../t-sql/statements/alter-workload-group-transact-sql.md) y [CREATE WORKLOAD GROUP](../t-sql/statements/create-workload-group-transact-sql.md).|
|Recompilaciones reducidas para cargas de trabajo| Mejora el rendimiento cuando se usan tablas temporales en varios ámbitos al reducir las recompilaciones innecesarias. Vea [Recompilaciones reducidas para cargas de trabajo](../relational-databases/tables/tables.md#ctp23). |
|Escalabilidad de puntos de control indirectos |Vea [Escalabilidad mejorada de puntos de control indirectos](../relational-databases/logs/database-checkpoints-sql-server.md#ctp23).|
|Actualizaciones de PFS simultáneas|[Las páginas PFS](https://techcommunity.microsoft.com/t5/SQL-Server/Under-the-covers-GAM-SGAM-and-PFS-pages/ba-p/383125) son páginas especiales dentro de un archivo de base de datos que SQL Server usa para ayudar a localizar espacio libre cuando asigna espacio para un objeto. La contención de bloqueos temporales de página en las páginas PFS normalmente se asocia con [TempDB](https://support.microsoft.com/help/2154845/recommendations-to-reduce-allocation-contention-in-sql-server-tempdb-d), pero también se puede producir en las bases de datos de usuario cuando hay muchos subprocesos de asignación de objetos simultáneos. Esta mejora cambia la manera en que se administra la simultaneidad con las actualizaciones de PFS para que puedan actualizarse en un bloqueo temporal compartido, en lugar de un bloqueo exclusivo. Este comportamiento está activado de forma predeterminada en todas las bases de datos (incluida TempDB) a partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].|
|Migración de trabajo con un programador |La migración de trabajos permite que un programador inactivo migre un trabajo de la cola de ejecutables de otro programador en el mismo nodo NUMA y que reanude inmediatamente la tarea de este trabajo migrado. Esta mejora proporciona un uso de CPU más equilibrado en situaciones en las que se asignan varias tareas de ejecución prolongada al mismo programador. Para obtener más información, consulte [Rendimiento inteligente de SQL Server 2019: migración de trabajo](https://techcommunity.microsoft.com/t5/SQL-Server/SQL-Server-2019-Intelligent-Performance-Worker-Migration/ba-p/939610). |
| &nbsp; | &nbsp; |

### <a name="monitoring"></a>Supervisión
La supervisión de las mejoras le permite obtener información de rendimiento de cualquier carga de trabajo de base de datos cuando la necesite.

|Nueva característica o actualización | Detalles |
|:---|:---|
|`WAIT_ON_SYNC_STATISTICS_REFRESH` | Nuevo tipo de espera en la vista de administración dinámica `sys.dm_os_wait_stats`. Muestra el tiempo de nivel de instancia acumulado empleado en las operaciones de actualización de estadísticas sincrónicas. Vea [`sys.dm_os_wait_stats`](../relational-databases/system-dynamic-management-views/sys-dm-os-wait-stats-transact-sql.md).|
|Directiva de captura personalizada para Almacén de consultas | Cuando se habilita esta directiva, una nueva configuración de la directiva de captura del almacén de consultas incluye más configuraciones del almacén de consultas para ajustar la recopilación de datos en un servidor específico. Consulte [Opciones SET de ALTER DATABASE](../t-sql/statements/alter-database-transact-sql-set-options.md).|
|`LIGHTWEIGHT_QUERY_PROFILING`| Una nueva configuración de ámbito de base de datos. Vea [`LIGHTWEIGHT_QUERY_PROFILING`](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#lqp). |
|`command` columna `sys.dm_exec_requests` | Muestra `SELECT (STATMAN)` si un elemento `SELECT` está esperando a que finalice una operación de actualización de estadísticas sincrónica para poder continuar con la ejecución de la consulta. Vea [`sys.dm_exec_requests`](../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md).|
|`sys.dm_exec_query_plan_stats` | Una nueva función de administración dinámica (DMF) que devuelve el equivalente del último plan de ejecución real conocido para todas las consultas. Vea [sys.dm_exec_query_plan_stats](../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-stats-transact-sql.md).|
|`LAST_QUERY_PLAN_STATS` | Una nueva configuración de ámbito de base de datos que habilita `sys.dm_exec_query_plan_stats`. Vea [ALTER DATABASE SCOPED CONFIGURATION](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md).|
|`query_post_execution_plan_profile` | Un evento extendido que recopila el equivalente de un plan de ejecución real que se basa en la generación de perfiles ligera, a diferencia de `query_post_execution_showplan`, que usa la generación de perfiles estándar. Vea [Infraestructura de generación de perfiles de consultas](../relational-databases/performance/query-profiling-infrastructure.md).|
|`sys.dm_db_page_info(database_id, file_id, page_id, mode)` | Una nueva DMF que devuelve información sobre una página de una base de datos. Consulte [sys.dm_db_page_info (Transact-SQL)](../relational-databases/system-dynamic-management-views/sys-dm-db-page-info-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="developer-experience"></a>Experiencia del desarrollador
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] sigue proporcionando una experiencia de desarrollo de primera clase con mejoras en los tipos de datos de gráficos y espaciales, con compatibilidad con UTF-8 y con un nuevo marco de extensibilidad que permite a los desarrolladores usar el lenguaje que prefieran para obtener información sobre sus datos.

### <a name="graph"></a>Grafo

|Nueva característica o actualización | Detalles |
|:---|:---|
|Acciones de eliminación en cascada de restricción perimetral | Ahora puede definir acciones de eliminación en cascada en una restricción perimetral de una base de datos del gráfico. Consulte [Restricciones perimetrales](../relational-databases/tables/graph-edge-constraints.md). |
|Nueva función de grafo: `SHORTEST_PATH` | Ahora puede usar `SHORTEST_PATH` dentro de `MATCH` para encontrar la ruta más corta entre dos nodos en un grafo o para realizar recorridos de longitud arbitraria.|
|Índices y tablas de particiones| Ahora las tablas de gráficos admiten la creación de particiones de tabla e índice. |
|Uso de alias de vista o tabla derivada en una consulta de coincidencia del grafo. |Vea [Consulta de coincidencia de grafos](../t-sql/queries/match-sql-graph.md). |
| &nbsp; | &nbsp; |

### <a name="unicode-support"></a>Compatibilidad con Unicode
Para ayudar a las empresas de diferentes países y regiones en las que el requisito de proporcionar servicios y aplicaciones de bases de datos multilingües mundiales es fundamental para satisfacer las demandas de los clientes y para cumplir las normativas de mercado específicas. 

|Nueva característica o actualización | Detalles |
|:---|:---|
|Compatibilidad con la codificación de caracteres UTF-8 |Compatibilidad con UTF-8 para la codificación de importación y exportación, y como intercalación de nivel de base de datos o de columna para los datos de cadena. Incluye la compatibilidad con tablas externas de PolyBase y Always Encrypted (cuando no se usa con enclaves). Vea [Compatibilidad con la intercalación y Unicode](../relational-databases/collations/collation-and-unicode-support.md).|
| &nbsp; | &nbsp; |

### <a name="language-extensions"></a>Extensiones de lenguaje

|Nueva característica o actualización | Detalles |
|:---|:---|
|Nuevo SDK del lenguaje Java | Simplifica el desarrollo de los programas Java que se pueden ejecutar desde SQL Server. Vea [SDK de extensibilidad de Microsoft para Java para SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md). |
|El SDK del lenguaje Java está en código abierto |El [SDK de extensibilidad de Microsoft para Java para Microsoft SQL Server](../language-extensions/how-to/extensibility-sdk-java-sql-server.md) está ahora en código abierto y [disponible en GitHub](https://github.com/microsoft/sql-server-language-extensions).|
|Compatibilidad con tipos de datos de Java|Vea [Java data types](../language-extensions/how-to/java-to-sql-data-types.md) (Tipo de datos Java).|
|Nuevo runtime de Java predeterminado | Ahora, SQL Server incluye Zulu Embedded de Azul Systems para agregar compatibilidad con Java en todo el producto. Vea [La compatibilidad gratuita con Java en SQL Server 2019 ya está disponible](https://cloudblogs.microsoft.com/sqlserver/2019/07/24/free-supported-java-in-sql-server-2019-is-now-available/). |
|Extensiones de lenguaje de SQL Server| Ejecute código externo con el marco de extensibilidad. Vea [Extensiones de lenguaje de SQL Server](../language-extensions/language-extensions-overview.md).
|Registrar lenguajes externos|Un nuevo lenguaje de definición de datos (DDL), `CREATE EXTERNAL LANGUAGE`, registra lenguajes externos, como Java, en SQL Server. Vea [CREATE EXTERNAL LANGUAGE](../t-sql/statements/create-external-language-transact-sql.md) (CREAR UN LENGUAJE EXTERNO). |
| &nbsp; | &nbsp; |

### <a name="spatial"></a>Espacial

|Nueva característica o actualización | Detalles |
|:---|:---|
| Nuevos identificadores de referencia espacial (SRID) |El marco australiano [GDA2020](http://www.ga.gov.au/scientific-topics/positioning-navigation/geodesy/datums-projections/gda2020) proporciona datos más sólidos y precisos que están más estrechamente alineados con sistemas de posicionamiento global. Los nuevos SRID son:<ul><li>7843 para 2D geográfico</li><li>7844 para 3D geográfico</li></ul> La vista [sys.spatial_reference_systems](../relational-databases/system-catalog-views/sys-spatial-reference-systems-transact-sql.md) contiene las definiciones de nuevos SRID. |
| &nbsp; | &nbsp; |

### <a name="error-messages"></a>Mensajes de error
Cuando se produce un error en un proceso de extracción, transformación y carga de datos (ETL) porque el origen y el destino no tienen los mismos tipos de datos o la misma longitud, la solución solía llevar mucho tiempo, especialmente en grandes conjuntos de datos. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] permite obtener información de forma más rápida sobre los errores de truncamiento de datos.

|Nueva característica o actualización | Detalles |
|:---|:---|
|Advertencias de truncamiento detalladas | Los valores predeterminados del mensaje de error del truncamiento de datos incluyen los nombres de tabla y de columna, así como el valor truncado. Vea [VERBOSE_TRUNCATION_WARNINGS](../t-sql/statements/alter-database-scoped-configuration-transact-sql.md#verbose-truncation).|
| &nbsp; | &nbsp; |

## <a name="mission-critical-security"></a>Seguridad crítica
[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] proporciona una arquitectura de seguridad diseñada para permitir que los administradores de bases de datos y los desarrolladores creen aplicaciones de base de datos seguras y combatan las amenazas. Cada versión de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ha mejorado las versiones anteriores con la introducción de nuevas características y funcionalidades, y [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] continúa mejorando en este aspecto.

|Nueva característica o actualización | Detalles |
|:---|:---|
|Always Encrypted con enclaves seguros|Expande Always Encrypted con cifrado en contexto y cálculos enriquecidos mediante la habilitación de cálculos en los datos de texto no cifrado dentro de un enclave seguro del lado servidor. El cifrado en contexto mejora el rendimiento y la confiabilidad de las operaciones criptográficas (cifrado de columnas, rotación de las claves de cifrado de columnas, etc.), puesto que evita mover los datos fuera de la base de datos.<br><br> La compatibilidad con cálculos enriquecidos (operaciones de coincidencia y comparación de patrones) abre Always Encrypted a un conjunto mucho más amplio de escenarios y aplicaciones que demandan protección de datos confidenciales, a la vez que requieren funciones más enriquecidas en las consultas de Transact-SQL. [Always Encrypted con enclaves seguros](../relational-databases/security/encryption/always-encrypted-enclaves.md).|
|Administración de certificados en el Administrador de configuración de SQL Server|Las tareas de administración de certificados, como ver e implementar certificados, ahora son posibles mediante el uso del Administrador de configuración de SQL Server. Vea [Administración de certificados (Administrador de configuración de SQL Server)](../database-engine/configure-windows/manage-certificates.md).|
|Clasificación y detección de datos|La clasificación y detección de datos proporcionan funciones para clasificar y etiquetar columnas en las tablas de usuario. La clasificación de la información confidencial (empresarial, financiera, sanitaria, personal, etc.) puede desempeñar un papel fundamental en el estado de protección de la información de la organización. Puede servir como infraestructura para lo siguiente:<ul><li>Ayudar a cumplir los requisitos de cumplimiento reglamentario y los estándares de privacidad de datos</li><li>Varios escenarios de seguridad, como la supervisión (auditoría) y las alertas relacionadas con accesos anómalos a información confidencial</li><li>Facilitar la identificación de dónde reside la información confidencial de la empresa, para que los administradores pueden adoptar los pasos adecuados para proteger la base de datos.</li></ul>|
|SQL Server Audit|La [auditoría](../relational-databases/security/auditing/sql-server-audit-database-engine.md) también se ha mejorado para incluir un campo nuevo `data_sensitivity_information` en el registro de auditoría, que contiene las clasificaciones de confidencialidad (etiquetas) de los datos reales que devuelve la consulta. Para información detallada y ejemplos, consulte [`ADD SENSITIVITY CLASSIFICATION`](../t-sql/statements/add-sensitivity-classification-transact-sql.md).|
| &nbsp; | &nbsp; |

## <a name="high-availability"></a>Alta disponibilidad
Una tarea habitual que todo usuario que implemente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] debe tener en cuenta es la comprobación de que todas las instancias críticas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y las bases de datos que contienen están disponibles cuando el negocio y los usuarios finales las necesitan. La disponibilidad es un pilar fundamental de la plataforma de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], y [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] presenta muchas características nuevas y mejoras que permiten a las empresas asegurarse de que sus entornos de base de datos tienen una alta disponibilidad.

### <a name="availability-groups"></a>Grupos de disponibilidad

|Nueva característica o actualización | Detalles |
|:---|:---|
|Hasta cinco réplicas sincrónicas|[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] aumenta el número máximo de réplicas sincrónicas a 5, de las 3 que eran en [!INCLUDE[ssSQL17](../includes/sssql17-md.md)]. Puede configurar este grupo de cinco réplicas para habilitar la conmutación automática por error dentro del grupo. Hay una réplica principal, además de cuatro réplicas secundarias sincrónicas.|
|Redireccionamiento de la conexión de réplicas de secundaria a principal| Permite que las conexiones de las aplicaciones cliente se dirijan a la réplica principal, independientemente del servidor de destino especificado en la cadena de conexión. Para obtener más información, consulte [Secondary to primary replica read/write connection redirection (Always On Availability Groups)](../database-engine/availability-groups/windows/secondary-replica-connection-redirection-always-on-availability-groups.md) (Redireccionamiento de la conexión de lectura/escrita de réplicas de secundaria a principal [Grupos de disponibilidad Always On]).|
|Ventajas de HADR| Todos los clientes de Software Assurance de SQL Server podrán disfrutar de tres ventajas mejoradas en cualquier versión de SQL Server que todavía sea compatible con Microsoft. Para obtener más información, consulte [nuestro anuncio](https://cloudblogs.microsoft.com/sqlserver/2019/10/30/new-high-availability-and-disaster-recovery-benefits-for-sql-server/).
| &nbsp; | &nbsp; |

### <a name="recovery"></a>Recuperación

|Nueva característica o actualización | Detalles |
|:---|:---|
|Recuperación acelerada de bases de datos. | Reduzca el tiempo de recuperación después de un reinicio o una reversión de transacción de ejecución prolongada con la recuperación acelerada de bases de datos. Vea [Recuperación acelerada de bases de datos](../relational-databases/backup-restore/restore-and-recovery-overview-sql-server.md#adr).|
| &nbsp; | &nbsp; |

### <a name="resumable-operations"></a>Operaciones reanudables

|Nueva característica o actualización | Detalles |
|:---|:---|
|Compilación y recompilación en línea de índices de almacén de columnas en clúster | Vea [Realización de operaciones de índice en línea](../relational-databases/indexes/perform-index-operations-online.md). |
|Compilación reanudable en línea de índices de almacén de filas | Vea [Realización de operaciones de índice en línea](../relational-databases/indexes/perform-index-operations-online.md). |
|Suspensión y reanudación del examen inicial del Cifrado de datos transparente (TDE)|Vea [Análisis del Cifrado de datos transparente (TDE): suspensión y reanudación](../relational-databases/security/encryption/transparent-data-encryption.md#scan-suspend-resume).|
| &nbsp; | &nbsp; |

## <a name="platform-choice"></a>Elección de la plataforma
[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] se basa en las innovaciones introducidas en [!INCLUDE[ssSQL17](../includes/sssql17-md.md)] para que pueda ejecutar [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en la plataforma de su elección con más funcionalidad y seguridad que nunca.

### <a name="linux"></a><a id="sql-server-on-linux"></a>Linux

| Nueva característica o actualización | Detalles |
|:-----|:-----|
|Compatibilidad con la replicación | Vea [Replicación de SQL Server en Linux](../linux/sql-server-linux-replication.md). |
|Compatibilidad con el Coordinador de transacciones distribuidas de Microsoft (MSDTC) | Vea [Cómo configurar el Coordinador de transacciones distribuidas de Microsoft (MSDTC) en Linux](../linux/sql-server-linux-configure-msdtc.md). |
|Compatibilidad de OpenLDAP con proveedores terceros de AD | Consulte [Tutorial: usar la autenticación de Active Directory con SQL Server en Linux](../linux/sql-server-linux-active-directory-authentication.md). |
|Machine Learning Services en Linux | Vea [Instalación de SQL Server Machine Learning Services (Python y R) en Linux](../linux/sql-server-linux-setup-machine-learning.md). |
|Mejoras de tempdb | De forma predeterminada, una nueva instalación de SQL Server en Linux crea varios archivos de datos de TempDB en función del número de núcleos lógicos (con un máximo de ocho archivos de datos). Esto no se aplica a las actualizaciones de versión principal o secundaria en contexto. Cada archivo tempdb es de 8 MB con un crecimiento automático de 64 MB. Este comportamiento es similar a la instalación de SQL Server predeterminada en Windows. |
|PolyBase en Linux | Vea [Instalación de PolyBase en Linux](../relational-databases/polybase/polybase-linux-setup.md) para informarse sobre conectores que no son de Hadoop.<br/><br/>Consulte [Asignación del tipo de PolyBase](../relational-databases/polybase/polybase-type-mapping.md). |
| Compatibilidad con Captura de datos modificados (CDC) | La característica Captura de datos modificados (CDC) ahora es compatible con Linux para [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| &nbsp; | &nbsp; |

### <a name="containers"></a>Contenedores
La manera más sencilla de empezar a trabajar con [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es usar contenedores. [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] amplía las innovaciones introducidas en versiones anteriores para que pueda implementar contenedores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en nuevas plataformas de forma más segura y con mayor funcionalidad.

|Nueva característica o actualización | Detalles |
|:---|:---|
| Registro de contenedor de Microsoft | Ahora [Registro de contenedor de Microsoft](https://azure.microsoft.com/blog/microsoft-syndicates-container-catalog/) reemplaza a Docker Hub para nuevas imágenes de contenedor de Microsoft oficiales, incluido [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]. |
| Contenedores no raíz | [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] presenta la capacidad de crear contenedores más seguros mediante el inicio del proceso de [!INCLUDE[sql-server](../includes/ssnoversion-md.md)] como un usuario no raíz de forma predeterminada. Vea [Compilación y ejecución de contenedores de SQL Server como un usuario no raíz](../linux/sql-server-linux-docker-container-deployment.md). |
| Imágenes de contenedor certificadas de Red Hat | A partir de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)], puede ejecutar contenedores de SQL Server en Red Hat Enterprise Linux. |
| Compatibilidad con PolyBase y Machine Learning| [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] presenta nuevas formas de trabajar con contenedores de SQL Server como Machine Learning Services y PolyBase. Consulte algunos ejemplos en el [repositorio de GitHub de contenedores de SQL Server](https://github.com/microsoft/mssql-docker/tree/master/linux/preview/examples). |
| &nbsp; | &nbsp; |

## <a name="setup-options"></a>Opciones de configuración

|Nueva característica o actualización | Detalles |
|:---|:---| 
|Nuevas opciones de configuración de memoria | Durante la instalación, establece las configuraciones de servidor *memoria de servidor mínima (MB)* y *memoria de servidor máxima (MB)* . Consulte [Configuración del motor de base de datos: página de memoria](./install/instance-configuration.md#memory) y los parámetros `USESQLRECOMMENDEDMEMORYLIMITS`, `SQLMINMEMORY` y `SQLMAXMEMORY` en [Instalar SQL Server desde el símbolo del sistema](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). El valor propuesto se alinea con las directrices de configuración de memoria en [Opciones de configuración de memoria del servidor](../database-engine/configure-windows/server-memory-server-configuration-options.md#manually).| 
|Opciones de configuración de nuevo paralelismo | Durante la instalación, establece la configuración del servidor *Grado máximo de paralelismo*. Consulte [Configuración del motor de base de datos: página de MaxDOP](./install/instance-configuration.md#maxdop) y el parámetro `SQLMAXDOP` en [Instalar SQL Server desde el símbolo del sistema](../database-engine/install-windows/install-sql-server-from-the-command-prompt.md#Install). El valor predeterminado se alinea con las directrices sobre el grado máximo de paralelismo en [Establecer la opción de configuración del servidor Grado máximo de paralelismo](../database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option.md#Guidelines).| 
|Advertencia de configuración sobre la clave de producto de la licencia Server/CAL|Si se especifica una clave de producto de licencia Enterprise Server o CAL y el equipo tiene más de 20 núcleos físicos, o 40 núcleos lógicos si la tecnología Hyper-Threading está habilitada, se muestra una advertencia durante la instalación. Los usuarios pueden seguir confirmando la limitación y continuar con la instalación, o bien especificar una clave de licencia que admita el número máximo de procesadores del sistema operativo.|
| &nbsp; | &nbsp; |

## <a name="sql-server-machine-learning-services"></a><a id="ml"></a> Machine Learning Services de SQL Server

|Nueva característica o actualización | Detalles |
|:---|:---|
|Modelos basados en particiones | Puede procesar scripts externos por partición de datos mediante los nuevos parámetros agregados a `sp_execute_external_script`. Esta funcionalidad admite el aprendizaje de muchos modelos pequeños (un modelo por cada partición de datos) en lugar de uno grande. Vea [Creación de modelos basados en particiones](../machine-learning/tutorials/r-tutorial-create-models-per-partition.md).|
|Clúster de conmutación por error de Windows Server| Puede configurar la alta disponibilidad de Machine Learning Services en un clúster de conmutación por error de Windows Server.|
| &nbsp; | &nbsp; |

## <a name="sql-server-analysis-services"></a>SQL Server Analysis Services

Esta versión introduce nuevas características y mejoras en el rendimiento, el gobierno de recursos y la compatibilidad con clientes.

| Nueva característica o actualización | Detalles |
|:---|:---|
|Grupos de cálculo en modelos tabulares| Los grupos de cálculo pueden reducir significativamente el número de medidas redundantes mediante la agrupación de expresiones de medida comunes como *elementos de cálculo*. Para obtener más información, vea [Grupos de cálculo en el modelo tabular](/analysis-services/tabular-models/calculation-groups). |
|Intercalación de consultas| La intercalación de consultas es una configuración del sistema en modo tabular que puede mejorar los tiempos de respuesta a las consultas de usuario en escenarios de alta simultaneidad. Para obtener más información, vea [Intercalación de consultas](/analysis-services/tabular-models/query-interleaving). |
|Relaciones varios a varios en modelos tabulares| Permite relaciones de varios a varios entre tablas donde las dos columnas no son únicas. Para obtener más información, consulte [Relaciones en modelos tabulares](/analysis-services/tabular-models/relationships-ssas-tabular).|
|Configuración de propiedad para la gobernanza de recursos| Esta versión incluye nuevas opciones de configuración de la memoria: Memory\QueryMemoryLimit, DbpropMsmdRequestMemoryLimit y OLAP\Query\RowsetSerializationLimit para el gobierno de recursos. Para obtener más información, consulte [Configuración de la memoria](/analysis-services/server-properties/memory-properties).|
|Configuración de gobernanza para las actualizaciones de la caché de Power BI | Esta versión introduce la propiedad ClientCacheRefreshPolicy, la cual invalida el almacenamiento en caché de datos del mosaico del panel y datos de informe para la carga inicial de los informes de Live Connect mediante el servicio Power BI. Para obtener más información, vea [Propiedades generales](/analysis-services/server-properties/general-properties). |
| Adjunto en línea  | Adjunto en línea se puede usar para la sincronización de réplicas de solo lectura en entornos locales de escalabilidad horizontal de consultas. Para obtener más información, vea [Adjunto en línea](/analysis-services/what-s-new-in-sql-server-analysis-services#online-attach). |
| &nbsp; | &nbsp; |

## <a name="sql-server-integration-services"></a>SQL Server Integration Services

Esta versión presenta nuevas características para mejorar las operaciones de archivos.

| Nueva característica o actualización | Detalles |
|:---|:---|
|Tarea de archivo flexible |Realice operaciones de archivo en el sistema de archivos local, Azure Blob Storage y Azure Data Lake Storage Gen2. Vea [Tarea de archivo flexible](../integration-services/control-flow/flexible-file-task.md).|
|Origen y destino de archivo flexible |Lea y escriba datos para Azure Blob Storage y Azure Data Lake Storage Gen2. Vea [Origen de archivo flexible](../integration-services/data-flow/flexible-file-source.md) y [Destino de archivo flexible](../integration-services/data-flow/flexible-file-destination.md). |

## <a name="sql-server-master-data-services"></a>SQL Server [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]

| Nueva característica o actualización | Detalles |
|:---|:---|
|Compatibilidad con bases de datos de Azure SQL Managed Instance| Hospede [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)] en Azure SQL Managed Instance. Vea [Instalación y configuración de [!INCLUDE[master-data-services](../includes/ssmdsshort-md.md)]](../master-data-services/master-data-services-installation-and-configuration.md#SetUpWeb).|
|Nuevos controles HTML| Los controles HTML reemplazan a los componentes anteriores de Silverlight. Quitada la dependencia de Silverlight.|
| &nbsp; | &nbsp; |

## <a name="sql-server-reporting-services"></a>SQL Server Reporting Services

Esta versión de las características de SQL Server Reporting Services admite instancias administradas de Azure SQL, conjuntos de datos de Power BI Premium, accesibilidad mejorada, Azure Active Directory Application Proxy y cifrado de bases de datos transparente. También proporciona una actualización de la herramienta Generador de informes de Microsoft. Para obtener más información, consulte las [novedades de SQL Server Reporting Services](../reporting-services/what-s-new-in-sql-server-reporting-services-ssrs.md).

## <a name="see-also"></a>Consulte también

- [`SqlServer` Módulo de PowerShell](https://www.powershellgallery.com/packages/Sqlserver)
- [Documentación de SQL Server PowerShell](../powershell/sql-server-powershell.md)

## <a name="next-steps"></a>Pasos siguientes

- [Talleres de SQL Server](https://aka.ms/sqlworkshops)
- [Notas de la versión de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]](sql-server-version-15-release-notes.md)
- [Microsoft [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)]: Notas técnicas del producto](https://aka.ms/sql2019whitepaper)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
