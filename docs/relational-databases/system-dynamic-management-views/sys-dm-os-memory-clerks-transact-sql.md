---
description: sys.dm_os_memory_clerks (Transact-SQL)
title: sys.dm_os_memory_clerks (Transact-SQL)
ms.custom: ''
ms.date: 02/18/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- dm_os_memory_clerks
- sys.dm_os_memory_clerks
- dm_os_memory_clerks_TSQL
- sys.dm_os_memory_clerks_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_os_memory_clerks dynamic management view
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0cd75d2eb6e613f36cecee8e79e3c8d6c99eed8d
ms.sourcegitcommit: ecf074e374426c708073c7da88313d4915279fb9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2021
ms.locfileid: "103575349"
---
# <a name="sysdm_os_memory_clerks-transact-sql"></a>sys.dm_os_memory_clerks (Transact-SQL)
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Devuelve el conjunto de todos los distribuidores de memoria activos actualmente en la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Para llamar a este método desde [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] o [!INCLUDE[ssPDW](../../includes/sspdw-md.md)] , use el nombre **Sys.dm_pdw_nodes_os_memory_clerks**. 
 
|Nombre de la columna|Tipo de datos|Descripción|  
|-----------------|---------------|-----------------|  
|**memory_clerk_address**|**varbinary(8**|Especifica la dirección de memoria exclusiva del distribuidor de memoria. Es la columna de clave principal. No admite valores NULL.|  
|**type**|**nvarchar(60)**|Especifica el tipo de distribuidor de memoria. Cada distribuidor tiene un tipo específico, como MEMORYCLERK_SQLCLR de distribuidores de CLR. No admite valores NULL.|  
|**name**|**nvarchar(256)**|Especifica el nombre asignado internamente de este distribuidor de memoria. Un componente puede tener varios distribuidores de memoria de un tipo específico. Un componente puede optar por usar nombres específicos para identificar distribuidores de memoria del mismo tipo. No admite valores NULL.|  
|**memory_node_id**|**smallint**|Especifica el identificador del nodo de memoria. No acepta valores NULL.|  
|**single_pages_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Para obtener más información, consulte [cambios en la administración de memoria a partir de SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).|  
|**pages_kb**|**bigint**|**Válido para** : [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] y versiones posteriores.<br /><br /> Especifica la cantidad de memoria de página asignada en kilobytes (KB) para este distribuidor de memoria. No admite valores NULL.|  
|**multi_pages_kb**|**bigint**|**Se aplica a**: desde [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] hasta [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)]. Para obtener más información, consulte [cambios en la administración de memoria a partir de SQL Server 2012 (11. x)](../memory-management-architecture-guide.md#changes-to-memory-management-starting-with-).<br /><br /> Cantidad de memoria de páginas múltiples asignada en KB. Es la cantidad de memoria asignada mediante el asignador de páginas múltiples de los nodos de memoria. Esta memoria se asigna fuera del grupo de búferes y aprovecha las ventajas del asignador virtual de los nodos de memoria. No admite valores NULL.|  
|**virtual_memory_reserved_kb**|**bigint**|Especifica la cantidad de memoria virtual reservada por un distribuidor de memoria. No admite valores NULL.|  
|**virtual_memory_committed_kb**|**bigint**|Especifica la cantidad de memoria virtual confirmada por un distribuidor de memoria. La cantidad de memoria confirmada debe ser siempre menor que la cantidad de memoria reservada. No admite valores NULL.|  
|**awe_allocated_kb**|**bigint**|Especifica la cantidad de memoria en kilobytes (KB) bloqueada en la memoria física y no transferida por el sistema operativo. No admite valores NULL.|  
|**shared_memory_reserved_kb**|**bigint**|Especifica la cantidad de memoria compartida reservada por un distribuidor de memoria. Es la cantidad de memoria reservada que van a utilizar la memoria compartida y la asignación de archivos. No admite valores NULL.|  
|**shared_memory_committed_kb**|**bigint**|Especifica la cantidad de memoria compartida confirmada por el distribuidor de memoria. No admite valores NULL.|  
|**page_size_in_bytes**|**bigint**|Especifica la granularidad de la asignación de páginas para este distribuidor de memoria. No admite valores NULL.|  
|**page_allocator_address**|**varbinary(8**|Especifica la dirección del asignador de páginas. Esta dirección es única para un distribuidor de memoria y se puede usar en **Sys.dm_os_memory_objects** para buscar objetos de memoria enlazados a este funcionario. No admite valores NULL.|  
|**host_address**|**varbinary(8**|Especifica la dirección de memoria del host para este distribuidor de memoria. Para obtener más información, vea [sys.dm_os_hosts &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-os-hosts-transact-sql.md). Los componentes de, como [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, acceden a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] los recursos de memoria a través de la interfaz de host.<br /><br /> 0x00000000 = El distribuidor de memoria pertenece a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].<br /><br /> No admite valores NULL.|  
|**pdw_node_id**|**int**|**Se aplica a**: [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] , [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]<br /><br /> Identificador del nodo en el que se encuentra esta distribución.|  

## <a name="permissions"></a>Permisos

En [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] , requiere el `VIEW SERVER STATE` permiso.   
En los [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] objetivos de servicio Basic, S0 y S1, y para las bases de datos de grupos elásticos, se requiere la cuenta de [Administrador del servidor](/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) o la cuenta de administrador de [Azure Active Directory](/azure/azure-sql/database/authentication-aad-overview#administrator-structure) . En el resto de los [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] objetivos de servicio, `VIEW DATABASE STATE` se requiere el permiso en la base de datos.   
  
## <a name="remarks"></a>Comentarios

 El administrador de memoria de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] consta de una jerarquía de tres capas. En la parte inferior de la jerarquía están los nodos de memoria. El nivel intermedio incluye los distribuidores de memoria, los almacenamientos en caché de la memoria y los bloques de memoria. La capa superior incluye los objetos de memoria. Estos objetos se utilizan para asignar memoria en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
 Los nodos de memoria proporcionan la interfaz y la implementación de los asignadores de nivel inferior. En [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], solo los distribuidores de memoria tienen acceso a nodos de memoria. Los distribuidores de memoria tienen acceso a interfaces de nodos de memoria para asignar memoria. Los nodos de memoria también realizan el seguimiento de la memoria asignada utilizando el distribuidor para diagnósticos. Cada componente que asigna una cantidad de memoria importante debe crear su propio distribuidor de memoria y asignar toda su memoria utilizando las interfaces del distribuidor. Con frecuencia, los componentes crean sus distribuidores correspondientes cuando se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

### <a name="cachestore-and-userstore"></a>ALMACÉN borrado y USERSTORE

ALMACÉN borrado y USERSTORE son distribuidores de memoria, pero funcionan como memorias caché reales. Normalmente, las memorias caché mantienen las asignaciones hasta que una directiva de eliminación de caché libera esas asignaciones. Para evitar que se vuelva a crear, una asignación almacenada en caché se conserva en la memoria caché tanto como sea posible y, normalmente, se quita de la caché cuando es demasiado antigua para ser útil, o cuando se necesita espacio en memoria para obtener información nueva (para obtener más información, vea [barrido de mano del reloj](sys-dm-os-memory-cache-clock-hands-transact-sql.md#remarks)). Éste es uno de los dos controles principales para el control de la visibilidad y el control de la duración de las memorias caché.

El almacén de caché y el almacén de usuario difieren en la forma en que controlan la duración de las asignaciones. En el caso de un almacén de caché, el marco de trabajo de almacenamiento en caché de SQLOS controla completamente la duración de las entradas. Con el almacén de usuario, la duración de las entradas solo está controlada parcialmente por un almacén. La implementación de cada almacén de usuario puede ser específica de la naturaleza de las asignaciones de memoria y, por lo tanto, los almacenes de usuarios participan en el control de la duración de sus entradas. 

El control de visibilidad administra la visibilidad de una entrada. Puede existir una entrada en una memoria caché, pero es posible que no esté visible. Por ejemplo, si una entrada de caché se marca solo para uso único, la entrada no estará visible después de usarse. Además, es posible que la entrada de caché esté marcada como desfasada. continuará en vivo en la caché, pero no será visible para las búsquedas. En ambos almacenes, la visibilidad de la entrada se controla mediante el marco de almacenamiento en caché.

Para obtener más información, vea [almacenamiento en caché de SQLOS](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

### <a name="objectstore"></a>OBJECTSTORE

El almacén de objetos es un grupo simple. Se utiliza para almacenar en caché datos homogéneos. Todas las entradas en los grupos se consideran iguales.  Los almacenes de objetos implementan un límite máximo para el tamaño del control en relación con otras memorias caché.

Para obtener más información, vea [almacenamiento en caché de SQLOS](https://docs.microsoft.com/archive/blogs/slavao/sqlos-caching).

## <a name="types"></a>Tipos

En la tabla siguiente se enumeran los tipos de distribuidor de memoria:

|Tipo  |Descripción  |
|---------|---------|
|CACHESTORE_BROKERDSH     |     Este almacén de caché se usa para almacenar asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) caché de encabezado de seguridad de diálogo.    |
|CACHESTORE_BROKERKEK     |   Este almacén de caché se usa para almacenar asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  caché de claves de intercambio de claves.    |
|CACHESTORE_BROKERREADONLY     |     Este almacén de caché se usa para almacenar asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) caché de solo lectura.    |
|CACHESTORE_BROKERRSB     |  Este almacén de caché se usa para almacenar asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) caché de [enlace de servicio remoto](../../t-sql/statements/create-remote-service-binding-transact-sql.md) .    |
|CACHESTORE_BROKERTBLACS     |     Este almacén de caché se usa para almacenar las asignaciones por [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) para las estructuras de acceso de seguridad.    |
|CACHESTORE_BROKERTO     |  Este almacén de caché se usa para almacenar asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) caché de [objetos de transmisión](../performance-monitor/sql-server-broker-to-statistics-object.md) .   |
|CACHESTORE_BROKERUSERCERTLOOKUP     |    Este almacén de caché se usa para almacenar las asignaciones por [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)  memoria caché de búsqueda de certificados de usuario  |
|CACHESTORE_COLUMNSTOREOBJECTPOOL     |     Este almacén de caché se usa para las asignaciones por [índices de almacén de columnas](../indexes/columnstore-indexes-overview.md) para [segmentos](../system-catalog-views/sys-column-store-segments-transact-sql.md) y [diccionarios](../system-catalog-views/sys-column-store-dictionaries-transact-sql.md) .   |
|CACHESTORE_CONVPRI     |  Este almacén de caché se usa para almacenar las asignaciones mediante [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) para realizar un seguimiento de las prioridades de las [conversaciones](../system-catalog-views/sys-conversation-priorities-transact-sql.md) .     |
|CACHESTORE_EVENTS     |     Este almacén de caché se usa para almacenar asignaciones mediante el [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) de [notificaciones de eventos](../service-broker/event-notifications.md) . |
|CACHESTORE_FULLTEXTSTOPLIST     |    Este distribuidor de memoria se usa para las asignaciones por Full-Text motor para la funcionalidad de lista de [palabras irrelevantes](../search/configure-and-manage-stopwords-and-stoplists-for-full-text-search.md) .       |
|CACHESTORE_NOTIF     |    Este almacén de caché se utiliza para las asignaciones mediante la funcionalidad de [notificación de consulta](../../connect/ado-net/sql/query-notifications-sql-server.md)    |
|CACHESTORE_OBJCP     |    Este almacén de caché se usa para almacenar en caché objetos con planes compilados (CP): procedimientos almacenados, funciones y desencadenadores. Para ilustrar, después de crear un plan de consulta para un procedimiento almacenado, su plan se almacena en esta caché.  |
|CACHESTORE_PHDR     |    Este almacén de caché se usa para el almacenamiento en caché temporal de la memoria durante el análisis de vistas, restricciones y valores predeterminados de árboles el algebrizador construyen durante la compilación de una consulta. Una vez analizada la consulta, se debe liberar la memoria. Algunos ejemplos son: muchas de las instrucciones de un lote: miles de inserciones o actualizaciones en un lote, un lote de T-SQL que contiene una consulta grande generada dinámicamente, un gran número de valores en una cláusula IN.      |
|CACHESTORE_QDSRUNTIMESTATS     |   Este almacén de caché se usa para almacenar en caché [almacén de consultas](../performance/monitoring-performance-by-using-the-query-store.md)  estadísticas de tiempo de ejecución |
|CACHESTORE_SEARCHPROPERTYLIST     |     El motor de Full-Text para la caché de la [lista de propiedades](../search/search-document-properties-with-search-property-lists.md) utiliza este almacén de caché para las asignaciones.  |
|CACHESTORE_SEHOBTCOLUMNATTRIBUTE     |  Este almacén de caché lo usa el motor de almacenamiento para las estructuras de metadatos de la columna de árbol B o de montón de almacenamiento en caché.      |
|CACHESTORE_SQLCP     |    Este almacén de caché se usa para almacenar en caché las consultas ad hoc, las instrucciones preparadas y los cursores del lado servidor en la caché del plan. Las consultas ad hoc son normalmente instrucciones T-SQL de eventos de lenguaje que se envían al servidor sin parametrización explícita. Las instrucciones preparadas también usan este almacén de caché, que la aplicación envía mediante llamadas API como [SQLPrepare ()](../../odbc/reference/syntax/sqlprepare-function.md) /  [SQLExecute](../../odbc/reference/syntax/sqlexecute-function.md) (ODBC) o [SqlCommand. Prepare](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.prepare) / [SqlCommand.ExecuteNonQuery](https://docs.microsoft.com/dotnet/api/system.data.sqlclient.sqlcommand.executenonquery) (ADO.net) y aparecerá en el servidor como [sp_prepare](../system-stored-procedures/sp-prepare-transact-sql.md)las / ejecuciones de procedimiento del sistema de[sp_execute](../system-stored-procedures/sp-execute-transact-sql.md) o [sp_prepexec](../system-stored-procedures/sp-prepexec-transact-sql.md) . Además, los cursores del lado servidor consumirían de este almacén de caché ([Sp_cursoropen](../system-stored-procedures/sp-cursoropen-transact-sql.md), [sp_cursorfetch](../system-stored-procedures/sp-cursorfetch-transact-sql.md), [sp_cursorclose](../system-stored-procedures/sp-cursorclose-transact-sql.md)).    |
|CACHESTORE_STACKFRAMES     |    Este almacén de caché se usa para las asignaciones de estructuras internas del sistema operativo SQL relacionadas con los marcos de pila.     |
|CACHESTORE_SYSTEMROWSET     |   Este almacén de caché se utiliza para las asignaciones de estructuras internas relacionadas con el registro y la recuperación de transacciones.      |
|CACHESTORE_TEMPTABLES     |     Este almacén de caché se usa para las asignaciones relacionadas con [tablas temporales y almacenamiento en caché de variables de tabla](../databases/tempdb-database.md#performance-improvements-in-tempdb-for-sql-server) , parte de la memoria caché del plan.    |
|CACHESTORE_VIEWDEFINITIONS     |     Este almacén de caché se usa para almacenar en caché las definiciones de vista como parte de la optimización de consultas.    |
|CACHESTORE_XML_SELECTIVE_DG     |     Este almacén de caché se utiliza para almacenar en caché estructuras XML para el procesamiento de XML.    |
|CACHESTORE_XMLDBATTRIBUTE     |     Este almacén de caché se usa para almacenar en caché estructuras de atributo XML para la actividad XML como [XQuery](../../xquery/xquery-language-reference-sql-server.md).    |
|CACHESTORE_XMLDBELEMENT     |   Este almacén de caché se usa para almacenar en caché estructuras de elementos XML para la actividad XML como [XQuery](../../xquery/xquery-language-reference-sql-server.md).      |
|CACHESTORE_XMLDBTYPE     |    Este almacén de caché se usa para almacenar en caché estructuras XML para la actividad XML como XQuery.     |
|CACHESTORE_XPROC     |   Este almacén de caché se usa para almacenar en caché estructuras para [procedimientos almacenados extendidos (Xprocs)](../extended-stored-procedures-programming/database-engine-extended-stored-procedures-programming.md) en la caché del plan.     |
|MEMORYCLERK_BACKUP     |    Este distribuidor de memoria se usa para varias asignaciones mediante la funcionalidad de [copia de seguridad](../../t-sql/statements/backup-transact-sql.md)    |
|MEMORYCLERK_BHF     |      Este distribuidor de memoria se usa para las asignaciones de administración de objetos binarios grandes (BLOB) durante la ejecución de la consulta (compatibilidad con identificadores de BLOB)  |
|MEMORYCLERK_BITMAP     |     La funcionalidad del sistema operativo SQL para el filtrado de mapas de bits usa este distribuidor de memoria para las asignaciones    |
|MEMORYCLERK_CSILOBCOMPRESSION     |  Este distribuidor de memoria se usa para las asignaciones por la [compresión de objetos binarios grandes (BLOB) del índice de almacén de columnas](../data-compression/data-compression.md#columnstore-and-columnstore-archive-compression) .    |
|MEMORYCLERK_DRTLHEAP     |    La funcionalidad del sistema operativo SQL usa este distribuidor de memoria para las asignaciones   <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_EXPOOL     |    La funcionalidad del sistema operativo SQL usa este distribuidor de memoria para las asignaciones   <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_EXTERNAL_EXTRACTORS     |   Este distribuidor de memoria se usa para las asignaciones mediante el motor de ejecución de consultas para las operaciones en [modo por lotes](../query-processing-architecture-guide.md#batch-mode-execution) .   <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_FILETABLE     |      Este distribuidor de memoria se usa para varias asignaciones mediante la funcionalidad de [FileTables](../blob/filetables-sql-server.md) .   |
|MEMORYCLERK_FSAGENT     |      Este distribuidor de memoria se usa para varias asignaciones mediante la funcionalidad de [FileStream](../blob/filestream-sql-server.md) .    |
|MEMORYCLERK_FSCHUNKER     |      Este distribuidor de memoria se usa para varias asignaciones mediante la funcionalidad de [FileStream](../blob/filestream-sql-server.md) para crear fragmentos de FileStream.   |
|MEMORYCLERK_FULLTEXT     |     Este distribuidor de memoria se usa para las asignaciones por Full-Text estructuras de motor.   |
|MEMORYCLERK_FULLTEXT_SHMEM     |   Este distribuidor de memoria se usa para las asignaciones Full-Text estructuras de motor relacionadas con la conectividad de memoria compartida con el proceso de demonio de texto completo.      |
|MEMORYCLERK_HADR     |     Este distribuidor de memoria se usa para las asignaciones de memoria mediante la funcionalidad de AlwaysOn     |
|MEMORYCLERK_HOST     |    La funcionalidad del sistema operativo SQL usa este distribuidor de memoria para las asignaciones.     |
|MEMORYCLERK_LANGSVC     |     Este distribuidor de memoria se usa para las asignaciones mediante instrucciones y comandos de T-SQL de SQL (parser, el algebrizador construyen, etc.).    |
|MEMORYCLERK_LWC     |   Este distribuidor de memoria se usa para las asignaciones por Full-Text motor de [búsqueda semántica](../search/semantic-search-sql-server.md)       |
|MEMORYCLERK_POLYBASE     |    Este distribuidor de memoria realiza un seguimiento de las asignaciones de memoria para la funcionalidad de [polybase](../polybase/polybase-guide.md) dentro de SQL Server.     |
|MEMORYCLERK_QSRANGEPREFETCH     |  Este distribuidor de memoria se usa para las asignaciones durante la ejecución de la consulta para la captura previa del intervalo de examen de consultas.      |
|MEMORYCLERK_QUERYDISKSTORE     |     Este distribuidor de memoria se usa en las asignaciones de memoria [almacén de consultas](../performance/monitoring-performance-by-using-the-query-store.md) dentro de SQL Server.    |
|MEMORYCLERK_QUERYDISKSTORE_HASHMAP     |   Este distribuidor de memoria se usa en las asignaciones de memoria [almacén de consultas](../performance/monitoring-performance-by-using-the-query-store.md) dentro de SQL Server.      |
|MEMORYCLERK_QUERYDISKSTORE_STATS     |  Este distribuidor de memoria se usa en las asignaciones de memoria [almacén de consultas](../performance/monitoring-performance-by-using-the-query-store.md) dentro de SQL Server.      |
|MEMORYCLERK_QUERYPROFILE     |  Este distribuidor de memoria se usa durante el inicio del servidor para habilitar la generación de perfiles de consulta.    <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_RTLHEAP     |    La funcionalidad del sistema operativo SQL usa este distribuidor de memoria para las asignaciones.  <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_SECURITYAPI     |    La funcionalidad del sistema operativo SQL usa este distribuidor de memoria para las asignaciones.  <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_SERIALIZATION     |     Exclusivamente para uso interno    |
|MEMORYCLERK_SLOG     |     Este distribuidor de memoria se usa para las asignaciones por sLog (flujo de registro en memoria secundario) en la [recuperación de base de datos acelerada](../accelerated-database-recovery-concepts.md) . <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_SNI     |     Este distribuidor de memoria asigna memoria a los componentes de la interfaz de red del servidor (SNI). SNI administra la conectividad y los paquetes [TDS](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/893fcc7e-8a39-4b3c-815a-773b7b982c50) para [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]  |
|MEMORYCLERK_SOSMEMMANAGER     |   Este distribuidor de memoria asigna estructuras para la programación y la memoria de subprocesos de SQLOS (SOS) y la administración de e/s.     |
|MEMORYCLERK_SOSNODE     |     Este distribuidor de memoria asigna estructuras para la programación y la memoria de subprocesos de SQLOS (SOS) y la administración de e/s.    |
|MEMORYCLERK_SOSOS     |     Este distribuidor de memoria asigna estructuras para la programación y la memoria de subprocesos de SQLOS (SOS) y la administración de e/s.    |
|MEMORYCLERK_SPATIAL     |    Los componentes de [datos espaciales](../spatial/spatial-data-sql-server.md) usan este distribuidor de memoria para las asignaciones de memoria.     |
|MEMORYCLERK_SQLBUFFERPOOL     |    Este distribuidor de memoria realiza un seguimiento de la frecuencia del consumidor de memoria más grande dentro [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de las páginas de datos y de índice. El grupo de búferes o la caché de datos conserva las páginas de datos y de índices cargadas en la memoria para proporcionar un acceso rápido a los datos. Para obtener más información, consulte [Administración de búfer](../memory-management-architecture-guide.md#buffer-management).     |
|MEMORYCLERK_SQLCLR     |     [SQLCLR ](../clr-integration/clr-integration-overview.md)usa este distribuidor de memoria para las asignaciones.     |
|MEMORYCLERK_SQLCLRASSEMBLY     |     Este distribuidor de memoria se usa para las asignaciones de ensamblados de [SQLCLR ](../clr-integration/clr-integration-overview.md) .     |
|MEMORYCLERK_SQLCONNECTIONPOOL     |     Este distribuidor de memoria almacena en caché la información en el servidor de la que la aplicación cliente puede necesitar el servidor para realizar un seguimiento. Un ejemplo es una aplicación que crea identificadores de preparación a través de  [sp_prepexecrpc](../system-stored-procedures/sp-prepexecrpc-transact-sql.md). La aplicación debe deshacer correctamente la preparación (cierre) de esos identificadores después de la ejecución.  |
|MEMORYCLERK_SQLEXTENSIBILITY     |    Este distribuidor de memoria se usa para las asignaciones por el [marco de extensibilidad](../../machine-learning/concepts/extensibility-framework.md) para ejecutar scripts externos de Python o R en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  <br /><br />**Válido para** [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] y versiones posteriores.   |
|MEMORYCLERK_SQLGENERAL     |   Este distribuidor de memoria podría ser utilizado por varios consumidores dentro del motor de SQL. Entre los ejemplos se incluye la memoria de replicación, el diagnóstico o la depuración interna, algunas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] funciones de inicio, algunas funciones del analizador de SQL, la generación de índices del sistema, la inicialización de objetos de memoria global, la creación de una conexión OleDb dentro del servidor y las consultas de servidor vinculado, el seguimiento del generador de perfiles, la creación de datos de SHOWPLAN     |
|MEMORYCLERK_SQLHTTP     |    Obsoleto     |
|MEMORYCLERK_SQLLOGPOOL     |   El grupo de registros utiliza este distribuidor de memoria [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  El grupo de registros es una caché que se usa para mejorar el rendimiento al leer el registro de transacciones. En concreto, mejora el uso de la caché del registro durante varias lecturas del registro, reduce las lecturas del registro de e/s de disco y permite el uso compartido de los exámenes del registro. Los consumidores principales del grupo de registros son AlwaysOn (cambio de captura y envío), administrador de rehacer, recuperación de base de datos-análisis/rehacer/rehacer, reversión en tiempo de ejecución de transacciones, replicación/CDC, copia de seguridad/restauración.    |
|MEMORYCLERK_SQLOPTIMIZER     |     Este distribuidor de memoria se utiliza para las asignaciones de memoria durante diferentes fases de la compilación de una consulta. Algunos usos incluyen la optimización de consultas, index Statistics Manager, View Definitions Compilation, histograma Generation.   |
|MEMORYCLERK_SQLQERESERVATIONS     |     Este distribuidor de memoria se utiliza para las asignaciones de concesión de memoria, que es la memoria asignada a las consultas para realizar operaciones de ordenación y hash durante la ejecución de la consulta. Para obtener más información sobre las reservas de ejecución de consultas (concesiones de memoria), vea [este blog](https://techcommunity.microsoft.com/t5/sql-server-support/memory-grants-the-mysterious-sql-server-memory-consumer-with/ba-p/333994) .     |
|MEMORYCLERK_SQLQUERYCOMPILE     |    Este distribuidor de memoria lo usa el optimizador de consultas para asignar memoria durante la compilación de la consulta.   |
|MEMORYCLERK_SQLQUERYEXEC     |    Este distribuidor de memoria se usa para las asignaciones en las siguientes áreas: [procesamiento en modo por lotes](../query-processing-architecture-guide.md#batch-mode-execution), ejecución de [consultas en paralelo](../query-processing-architecture-guide.md#parallel-query-processing) , contexto de ejecución de consultas, [teselación de índices espaciales](../spatial/spatial-indexes-overview.md#tessellation), operaciones de ordenación y hash (tablas de ordenación, tablas hash), algunos procesamientos de DVM, ejecución de [Update Statistics](../statistics/update-statistics.md)    |
|MEMORYCLERK_SQLQUERYPLAN     |     Este distribuidor de memoria se usa para las asignaciones mediante la administración de páginas del [montón](../indexes/heaps-tables-without-clustered-indexes.md) , las asignaciones de [DBCC CHECKTABLE](../../t-sql/database-console-commands/dbcc-checktable-transact-sql.md) y [sp_cursor * asignaciones de procedimientos almacenados](../system-stored-procedures/cursor-stored-procedures-transact-sql.md) .   |
|MEMORYCLERK_SQLSERVICEBROKER     |   Este distribuidor de memoria se usa en [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) asignaciones de memoria.       |
|MEMORYCLERK_SQLSERVICEBROKERTRANSPORT     |     Este distribuidor de memoria se usa en las asignaciones de memoria de transporte de [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md) .    |
|MEMORYCLERK_SQLSLO_OPERATIONS     |      Este distribuidor de memoria se usa para recopilar estadísticas de rendimiento. <br /><br />**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]   |
|MEMORYCLERK_SQLSOAP     |     Obsoleto    |
|MEMORYCLERK_SQLSOAPSESSIONSTORE     |    Obsoleto     |
|MEMORYCLERK_SQLSTORENG     |   Este distribuidor de memoria se usa para las asignaciones por varios componentes del motor de almacenamiento. Entre los ejemplos de componentes se incluyen estructuras para archivos de base de datos, administrador de archivos de réplica de instantáneas de base de datos, monitor de interbloqueos, estructuras DBTABLE, estructuras de log Manager, algunas estructuras de control de versiones de tempdb, funcionalidad de inicio del servidor, contexto de ejecución para subprocesos secundarios      |
|MEMORYCLERK_SQLTRACE     |     Este distribuidor de memoria se usa para las asignaciones de memoria de [seguimiento de SQL](../sql-trace/sql-trace.md) del lado servidor.     |
|MEMORYCLERK_SQLUTILITIES     |    Varios asignadores pueden usar este distribuidor de memoria dentro de SQL Server. Entre los ejemplos se incluyen copias de seguridad y restauración, trasvase de registros, creación de reflejo de la base de datos, comandos DBCC, código BCP en el lado servidor, algunos trabajos de paralelismo de consultas, búferes de examen de registro.    |
|MEMORYCLERK_SQLXML     |     Este distribuidor de memoria se utiliza para las asignaciones de memoria al realizar operaciones XML.    |
|MEMORYCLERK_SQLXP     |     Este distribuidor de memoria se utiliza para las asignaciones de memoria al llamar a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [procedimientos almacenados extendidos](../extended-stored-procedures-reference/database-engine-extended-stored-procedures-reference.md).    |
|MEMORYCLERK_SVL     |    Este distribuidor de memoria se usa para las asignaciones de estructuras internas del sistema operativo SQL. |
|MEMORYCLERK_TEST     |    Exclusivamente para uso interno   |
|MEMORYCLERK_UNITTEST     |      Exclusivamente para uso interno  |
|MEMORYCLERK_WRITEPAGERECORDER     |    Este distribuidor de memoria se usa para las asignaciones mediante la grabadora de páginas de escritura.   |
|MEMORYCLERK_XE     |    Este distribuidor de memoria se utiliza para las asignaciones de memoria de [eventos extendidos](../extended-events/extended-events.md)      |
|MEMORYCLERK_XE_BUFFER     |      Este distribuidor de memoria se utiliza para las asignaciones de memoria de [eventos extendidos](../extended-events/extended-events.md)   |
|MEMORYCLERK_XLOG_SERVER     |   Este distribuidor de memoria se usa para las asignaciones por Xlog que se usan para la administración de archivos de registro en SQL Azure base de datos   <br /><br />**Se aplica a**: [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] |
|MEMORYCLERK_XTP     |    Este distribuidor de memoria se utiliza para las asignaciones [de memoria de OLTP en memoria](../in-memory-oltp/in-memory-oltp-in-memory-optimization.md) .     |
|OBJECTSTORE_LBSS     |    Este almacén de objetos se usa para asignar variables de LOB temporales, parámetros y resultados intermedios para las expresiones. Un ejemplo que usa este almacén son [los parámetros con valores de tabla](../../connect/ado-net/sql/table-valued-parameters.md) (TVP). Vea el [artículo de kb 4468102](https://support.microsoft.com/topic/kb4468102-fix-excessive-memory-usage-when-you-trace-rpc-events-that-involve-table-valued-parameters-in-sql-server-2016-and-2017-c68aa214-26f1-98de-6b4d-c7dcad82dbd4) y el  [artículo de KB 4051359](https://support.microsoft.com/topic/kb4051359-fix-sql-server-runs-out-of-memory-when-table-valued-parameters-are-captured-in-extended-events-sessions-in-sql-server-2016-even-if-collecting-statement-or-data-stream-isn-t-enabled-a3639efa-0618-82a8-f6b1-8cdcba29ce6d) para obtener más información sobre las correcciones de este espacio.     |
|OBJECTSTORE_LOCK_MANAGER     |      Este distribuidor de memoria realiza un seguimiento de las asignaciones realizadas por el [Administrador de bloqueos](../sql-server-transaction-locking-and-row-versioning-guide.md#Lock_Engine) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .   |
|OBJECTSTORE_SECAUDIT_EVENT_BUFFER     |   Este almacén de objetos se utiliza para las asignaciones de memoria de [auditoría SQL Server](../security/auditing/sql-server-audit-database-engine.md) .        |
|OBJECTSTORE_SERVICE_BROKER     |     Este almacén de objetos se usa en [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)    |
|OBJECTSTORE_SNI_PACKET     |     Este almacén de objetos lo usan los componentes de la interfaz de red del servidor (SNI) que administran la conectividad    |
|OBJECTSTORE_XACT_CACHE     |    Este almacén de objetos se usa para almacenar en caché información de transacciones     |
|USERSTORE_DBMETADATA     |      Este almacén de objetos se usa para las estructuras de metadatos     |
|USERSTORE_OBJPERM     |     Este almacén se usa para las estructuras que realizan el seguimiento de la seguridad o el permiso de objetos.     |
|USERSTORE_QDSSTMT     |  Este almacén de caché se usa para almacenar en memoria caché [almacén de consultas](../performance/monitoring-performance-by-using-the-query-store.md)  instrucciones       |
|USERSTORE_SCHEMAMGR     |    La caché del administrador de esquemas almacena distintos tipos de información de metadatos sobre los objetos de base de datos en memoria (por ejemplo, tablas). Un usuario común de este almacén podría ser la base de datos Tempdb con objetos como tablas, procedimientos temporales, variables de tabla, parámetros con valores de tabla, tablas de archivos creados, almacén de versiones.  |
|USERSTORE_SXC     |    Este almacén de usuario se usa para las asignaciones para almacenar todos los parámetros de [RPC](https://docs.microsoft.com/openspecs/windows_protocols/ms-tds/619c43b6-9495-4a58-9e49-a4950db245b3) .     |
|USERSTORE_TOKENPERM     |    Tokenandpermuserstore aumenta es un único almacén de usuarios de SOS que realiza un seguimiento de las entradas de seguridad del contexto de seguridad, el inicio de sesión, el usuario, el permiso y la auditoría. Se asignan varias tablas hash para almacenar estos objetos.    |

## <a name="see-also"></a>Consulte también  

 [SQL Server vistas de administración dinámica relacionadas con el sistema operativo &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sql-server-operating-system-related-dynamic-management-views-transact-sql.md)   
 [sys.dm_os_sys_info &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-os-sys-info-transact-sql.md)   
 [sys.dm_exec_query_memory_grants &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-memory-grants-transact-sql.md)   
 [sys.dm_exec_requests &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql.md)   
 [sys.dm_exec_query_plan &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-query-plan-transact-sql.md)   
 [sys.dm_exec_sql_text &#40;&#41;de Transact-SQL ](../../relational-databases/system-dynamic-management-views/sys-dm-exec-sql-text-transact-sql.md)  
  
