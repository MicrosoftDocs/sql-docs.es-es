---
title: Base de datos tempdb | Microsoft Docs
description: En este tema se proporcionan detalles relacionados con la configuración y el uso de la base de datos tempdb en SQL Server en Azure SQL Database.
ms.custom: P360
ms.date: 09/16/2020
ms.prod: sql
ms.prod_service: database-engine
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- temporary tables [SQL Server], tempdb database
- tempdb database [SQL Server], about tempdb
- temporary stored procedures [SQL Server]
- tempdb database [SQL Server]
ms.assetid: ce4053fb-e37a-4851-b711-8e504059a780
author: stevestein
ms.author: sstein
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 5dc33d8bd3abf6e204c0ab602bc25105d78dced9
ms.sourcegitcommit: be74dc0966930f28b03d0429aed22b1f0a296d3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/13/2021
ms.locfileid: "103421967"
---
# <a name="tempdb-database"></a>tempdb [base de datos]

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

La base de datos del sistema `tempdb` es un recurso global disponible para todos los usuarios conectados a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a Azure SQL Database. `tempdb` contiene:  
  
- Los *objetos de usuario* temporales que se hayan creado explícitamente. Incluyen tablas e índices temporales locales o globales, procedimientos almacenados temporales, variables de tabla, tablas devueltas en funciones con valores de tabla y cursores.  
- *Objetos internos* que crea el motor de base de datos. Incluyen:
  - Tablas de trabajo para almacenar resultados intermedios para colas, cursores, ordenaciones y almacenamiento temporal de objetos grandes (LOB).
  - Archivos de trabajo para operaciones de combinación hash o de agregado hash.
  - Resultados de orden intermedio de operaciones como crear o volver a generar índices (si se ha especificado `SORT_IN_TEMPDB`), o algunas consultas `GROUP BY`, `ORDER BY` o `UNION`.

  Cada objeto interno usa un mínimo de nueve páginas: una página IAM y una extensión de ocho páginas. Para obtener más información acerca de las páginas y las extensiones, vea [Páginas y extensiones](../../relational-databases/pages-and-extents-architecture-guide.md#pages-and-extents).

  > [!IMPORTANT]
  > Los grupos elásticos y las bases de datos únicas de Azure SQL Database admiten tablas temporales globales y procedimientos almacenados temporales globales que se almacenan en `tempdb` y que tienen como ámbito el nivel de base de datos. 
  >
  > Las tablas temporales globales y los procedimientos almacenados temporales globales se comparten entre todos los usuarios en la misma base de datos SQL. Las sesiones de usuario de otras bases de datos no pueden acceder a tablas temporales globales. Para obtener más información, vea [Database scoped global temporary tables (Azure SQL Database)](../../t-sql/statements/create-table-transact-sql.md#database-scoped-global-temporary-tables-azure-sql-database) (Tablas temporales globales con ámbito de base de datos [Azure SQL Database]). [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance) admite los mismos objetos temporales que SQL Server.
  >
  > En el caso de los grupos elásticos y las bases de datos únicas de Azure SQL Database, solo se aplican la base de datos maestra y la base de datos `tempdb`. Para obtener más información, vea la [Qué es un servidor de Azure SQL Database](/azure/sql-database/sql-database-servers-databases#what-is-an-azure-sql-database-server). Para obtener una explicación de `tempdb` en el contexto de los grupos elásticos y las bases de datos únicas de Azure SQL Database, consulte [Base de datos tempdb en SQL Database](#tempdb-database-in-sql-database). 
  >
  > En el caso de Azure SQL Managed Instance, se aplican todas las bases de datos del sistema.

- *Almacenes de versiones*, que son colecciones de páginas de datos que contienen las filas de datos que admiten las características para las versiones de fila. Hay dos tipos: un almacén de versiones común y otro de generación de índices en línea. Los almacenes de versión contienen:
  - Versiones de fila generadas por las transacciones de modificación de datos en una base de datos que utiliza `READ COMMITTED` a través de transacciones de aislamiento de versiones de fila o de aislamiento de instantáneas.  
  - Versiones de fila que se generan mediante transacciones de modificación de datos para características, como operaciones de índice en línea, conjuntos de resultados activos múltiples (MARS) y desencadenadores `AFTER`.  
  
Las operaciones de `tempdb` se registran de forma mínima, por lo que las transacciones se pueden revertir. `tempdb` se vuelve a crear cada vez que se inicia [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], de forma que el sistema siempre se inicia con una copia limpia de la base de datos. Las tablas y los procedimientos almacenados temporales se quitan automáticamente en la desconexión y ninguna conexión permanece activa cuando se cierra el sistema. 

`tempdb` nunca tiene nada que guardarse de una sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a otra. No se permite realizar operaciones de copia de seguridad y restauración en `tempdb`.  

## <a name="physical-properties-of-tempdb-in-sql-server"></a>Propiedades físicas de tempdb en SQL Server

En la tabla siguiente se muestran los valores iniciales de configuración de los archivos de datos y registro de `tempdb` en SQL Server. Los valores se basan en los valores predeterminados para la base de datos `model`. El tamaño de estos archivos puede variar ligeramente para diferentes ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Archivo|Nombre lógico|Nombre físico|Tamaño inicial|Crecimiento del archivo|  
|----------|------------------|-------------------|------------------|-----------------|  
|Datos principales|tempdev|tempdb.mdf|8 megabytes|Crecimiento automático de 64 MB hasta llenar el disco.|  
|Archivos de datos secundarios|temp#|tempdb_mssql_ # .ndf|8 megabytes|Crecimiento automático de 64 MB hasta llenar el disco.|  
|Log|templog|templog.ldf|8 megabytes|Crecimiento automático de 64 megabytes hasta un máximo de 2 terabytes.|  
  
El número de archivos de datos secundarios depende del número de procesadores (lógicos) de la máquina. Como regla general, si el número de procesadores lógicos es inferior o igual a ocho, use el mismo número de archivos de datos que procesadores lógicos. Si el número de procesadores lógicos es superior a ocho, utilice ocho archivos de datos. Después, si se mantiene la contención, aumente el número de archivos de datos en múltiplos de cuatro hasta que la contención se reduzca a niveles aceptables, o bien modifique el código o la carga de trabajo.

> [!NOTE]
> El valor predeterminado para el número de archivos de datos se basa en las directrices [KB 2154845](https://support.microsoft.com/kb/2154845/).  

> [!NOTE]
> Para comprobar los parámetros de tamaño y de crecimiento actuales de `tempdb`, consulte la vista `tempdb.sys.database_files`.
  
### <a name="moving-the-tempdb-data-and-log-files-in-sql-server"></a>Mover los archivos de datos y registro de tempdb en SQL Server

Para mover los archivos de registro y de datos de `tempdb`, consulte [Mover bases de datos del sistema](../../relational-databases/databases/move-system-databases.md).  
  
### <a name="database-options-for-tempdb-in-sql-server"></a>Opciones de base de datos de tempdb en SQL Server

En la siguiente tabla se enumera el valor predeterminado de cada opción de base de datos en la base de datos `tempdb` y se indica si la opción se puede modificar. Para ver la configuración actual de estas opciones, utilice la vista de catálogo [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md) .  
  
|Opción de base de datos|Valor predeterminado|Se puede modificar|  
|---------------------|-------------------|---------------------|  
|ALLOW_SNAPSHOT_ISOLATION|Apagado|Sí|  
|ANSI_NULL_DEFAULT|Apagado|Sí|  
|ANSI_NULLS|Apagado|Sí|  
|ANSI_PADDING|Apagado|Sí|  
|ANSI_WARNINGS|Apagado|Sí|  
|ARITHABORT|Apagado|Sí|  
|AUTO_CLOSE|Apagado|No|  
|AUTO_CREATE_STATISTICS|ACTIVAR|Sí|  
|AUTO_SHRINK|Apagado|No|  
|AUTO_UPDATE_STATISTICS|ACTIVAR|Sí|  
|AUTO_UPDATE_STATISTICS_ASYNC|Apagado|Sí|  
|CHANGE_TRACKING|Apagado|No|  
|CONCAT_NULL_YIELDS_NULL|Apagado|Sí|  
|CURSOR_CLOSE_ON_COMMIT|Apagado|Sí|  
|CURSOR_DEFAULT|GLOBAL|Sí|  
|Opciones de disponibilidad de la base de datos|ONLINE<br /><br /> MULTI_USER<br /><br /> READ_WRITE|No<br /><br /> No<br /><br /> No|  
|DATE_CORRELATION_OPTIMIZATION|Apagado|Sí|  
|DB_CHAINING|ACTIVAR|No|  
|ENCRYPTION|Apagado|No|  
|MIXED_PAGE_ALLOCATION|Apagado|No|  
|NUMERIC_ROUNDABORT|Apagado|Sí|  
|PAGE_VERIFY|CHECKSUM para las nuevas instalaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]<br /><br /> NONE para las actualizaciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|Sí|  
|PARAMETERIZATION|SIMPLE|Sí|  
|QUOTED_IDENTIFIER|Apagado|Sí|  
|READ_COMMITTED_SNAPSHOT|Apagado|No|  
|RECOVERY|SIMPLE|No|  
|RECURSIVE_TRIGGERS|Apagado|Sí|  
|Opciones de Service Broker|ENABLE_BROKER|Sí|  
|TRUSTWORTHY|Apagado|No|  
  
Para obtener una descripción de estas opciones de la base de datos, vea [Opciones de ALTER DATABASE SET (Transact-SQL)](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
## <a name="tempdb-database-in-sql-database"></a>Base de datos tempdb en SQL Database

### <a name="tempdb-sizes-for-dtu-based-service-tiers"></a>tamaños de tempdb para los niveles de servicio basado en DTU

<!-- tempdb being larger for Basic and 50 eDTU pools than for 100-400 eDTU pools reflects actual config (historical reasons) --> 

|Objetivo de nivel de servicio|Tamaño máximo del archivo de datos de `tempdb` (GB)|Número de archivos de datos de `tempdb`|Tamaño máximo de los datos de `tempdb` (GB)|
|---|---:|---:|---:|
|Básica|13.9|1|13.9|
|S0|13.9|1|13.9|
|S1|13.9|1|13.9|
|S2|13.9|1|13.9|
|S3|32|1|32
|S4|32|2|64|
|S6|32|3|96|
|S7|32|6|192|
|S9|32|12|384|
|S12|32|12|384|
|P1|13.9|12|166.7|
|P2|13.9|12|166.7|
|P4|13.9|12|166.7|
|P6|13.9|12|166.7|
|P11|13.9|12|166.7|
|P15|13.9|12|166.7|
|Grupos elásticos básicos (todas las configuraciones de DTU)|13.9|12|166.7|
|Grupos elásticos estándar (50 eDTU)|13.9|12|166.7|
|Grupos elásticos estándar (100 eDTU)|32|1|32|
|Grupos elásticos estándar (200 eDTU)|32|2|64|
|Grupos elásticos estándar (300 eDTU)|32|3|96|
|Grupos elásticos estándar (400 eDTU)|32|3|96|
|Grupos elásticos estándar (800 eDTU)|32|6|192|
|Grupos elásticos estándar (1200 eDTU)|32|10|320|
|Grupos elásticos estándar (1600-3000 eDTU)|32|12|384|
|Grupos elásticos premium (todas las configuraciones de DTU)|13.9|12|166.7|
||||

### <a name="tempdb-sizes-for-vcore-based-service-tiers"></a>tamaños de tempdb para los niveles de servicio basado en núcleo virtual

Vea [Límites de recursos basados en núcleos virtuales](/azure/sql-database/sql-database-vcore-resource-limits).

## <a name="restrictions"></a>Restricciones

Las siguientes operaciones no se pueden realizar en la base de datos `tempdb`:  
  
- Agregar grupos de archivos.
- Realizar una copia de seguridad o restaurar la base de datos.
- Cambiar intercalaciones. La intercalación predeterminada es la intercalación de servidor.
- Cambiar el propietario de la base de datos. `tempdb` es propiedad de *sa*.
- Crear una instantánea de base de datos.
- Eliminar la base de datos.
- Eliminar el usuario *guest* de la base de datos.
- Habilitar el mecanismo de captura de cambios en los datos.
- Participar en el reflejo de la base de datos.
- Quitar el grupo de archivos principal, el archivo de datos principal o el archivo de registro.
- Cambiar el nombre de la base de datos o del grupo de archivos principal.
- Ejecutar `DBCC CHECKALLOC`.
- Ejecutar `DBCC CHECKCATALOG`.
- Establecer la base de datos en `OFFLINE`.
- Cambiar el nombre de la base de datos o del grupo de archivos principal a `READ_ONLY`.
  
## <a name="permissions"></a>Permisos

Cualquier usuario puede crear objetos temporales en `tempdb`. Los usuarios solo pueden acceder a sus propios objetos, a menos que reciban permisos adicionales. Es posible revocar el permiso de conexión a `tempdb` para impedir que un usuario use `tempdb`. No se recomienda porque algunas operaciones rutinarias requieren el uso de `tempdb`.  

## <a name="optimizing-tempdb-performance-in-sql-server"></a>Optimizar el rendimiento de tempdb en SQL Server
El tamaño y la ubicación física de la base de datos `tempdb` puede afectar al rendimiento de un sistema. Por ejemplo, si el tamaño definido para `tempdb` es demasiado pequeño, parte de la carga de procesamiento del sistema puede deberse al crecimiento automático de `tempdb` hasta alcanzar el tamaño necesario para admitir la carga de trabajo cada vez que se reinicie la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Si es posible, use la [inicialización de archivo instantáneos](../../relational-databases/databases/database-instant-file-initialization.md) para mejorar el rendimiento de las operaciones de crecimiento de los archivos de datos.

Asigne espacio previamente para todos los archivos de `tempdb`. Para ello, establezca el tamaño de archivo en un valor lo suficientemente alto para contener la carga de trabajo habitual del entorno. La asignación previa evita que `tempdb` se expanda con demasiada frecuencia, lo que afecta al rendimiento. La base de datos `tempdb` debe establecerse de modo que crezca automáticamente para aumentar el espacio en disco para las excepciones no previstas.

Los archivos de datos deberían ser del mismo tamaño dentro de cada [grupo de archivos](../../relational-databases/databases/database-files-and-filegroups.md#filegroups), ya que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utiliza un algoritmo de relleno proporcional que favorece las asignaciones en los archivos con más espacio libre. La división de `tempdb` en varios archivos de datos del mismo tamaño proporciona un alto grado de eficiencia paralela en las operaciones que usan `tempdb`.

Establezca el incremento de crecimiento de archivos en un tamaño razonable para evitar que los archivos de la base de datos `tempdb` crezcan en un porcentaje demasiado pequeño. Si el crecimiento de los archivos es demasiado pequeño comparado con la cantidad de datos que se escriben en `tempdb`, es posible que sea necesario expandir `tempdb` constantemente. Esto afectará al rendimiento.

Para comprobar los parámetros actuales de tamaño y de crecimiento de `tempdb`, use la consulta siguiente:

```sql
 SELECT name AS FileName,
    size*1.0/128 AS FileSizeInMB,
    CASE max_size
        WHEN 0 THEN 'Autogrowth is off.'
        WHEN -1 THEN 'Autogrowth is on.'
        ELSE 'Log file grows to a maximum size of 2 TB.'
    END,
    growth AS 'GrowthValue',
    'GrowthIncrement' =
        CASE
            WHEN growth = 0 THEN 'Size is fixed.'
            WHEN growth > 0 AND is_percent_growth = 0
                THEN 'Growth value is in 8-KB pages.'
            ELSE 'Growth value is a percentage.'
        END
FROM tempdb.sys.database_files;
GO
```

Coloque la base de datos `tempdb` en un subsistema de E/S rápido. Cree bandas en disco si hay muchos discos conectados directamente. No es necesario que los archivos de datos individuales o de grupos de `tempdb` estén en discos o ejes diferentes, a menos que también se estén produciendo cuellos de botella de E/S.

Coloque la base de datos `tempdb` en discos diferentes de los que usan las bases de datos de usuario.

## <a name="performance-improvements-in-tempdb-for-sql-server"></a>Mejoras de rendimiento de tempdb para SQL Server
A partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], se optimiza el rendimiento de `tempdb` de las maneras siguientes:  
  
- Las tablas temporales y las variables de tabla se almacenan en caché. El almacenamiento en caché permite que las operaciones que quitan y crean los objetos temporales se ejecuten muy rápidamente. También reduce la asignación de páginas y la contención de metadatos.  
- El protocolo de bloqueo temporal de página de asignación se ha mejorado para reducir el número de bloqueos temporales `UP` (actualizaciones).  
- Se reduce la sobrecarga del registro de `tempdb` para reducir el consumo de ancho de banda de E/S del disco en el archivo de registro de `tempdb`.  
- El programa de instalación agrega varios archivos de datos `tempdb` durante una instalación nueva de la instancia. Esta tarea puede realizarse con el nuevo control de entrada de la IU en la sección **Configuración del motor de base de datos** y un parámetro de línea de comandos `/SQLTEMPDBFILECOUNT`. De manera predeterminada, la configuración agrega tantos archivos de datos de `tempdb` como el número de procesadores lógicos u ocho, lo que sea menor.  
- Si hay varios archivos de datos `tempdb`, todos crecen automáticamente al mismo tiempo y la misma cantidad en función de la configuración de crecimiento. [La marca de seguimiento 1117](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) ya no es necesaria.  
- Todas las asignaciones de `tempdb` usarán extensiones uniformes. [La marca de seguimiento 1118](../../t-sql/database-console-commands/dbcc-traceon-trace-flags-transact-sql.md) ya no es necesaria.  
- Para el grupo de archivos principal, la propiedad `AUTOGROW_ALL_FILES` se activa y la propiedad no se puede modificar.

Para obtener más información acerca de las mejoras de rendimiento en `tempdb`, consulte el artículo del blog [TEMPDB: archivos y marcas de seguimiento y actualizaciones: ¡a por ello!](/archive/blogs/sql_server_team/tempdb-files-and-trace-flags-and-updates-oh-my).

## <a name="memory-optimized-tempdb-metadata"></a>Metadatos tempdb optimizados para memoria
La contención de metadatos en `tempdb` ha sido históricamente un cuello de botella en la escalabilidad para muchas cargas de trabajo que se ejecutan en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)] presenta una nueva característica que forma parte de la familia de características de [base de datos en memoria](../in-memory-database.md): metadatos de tempdb optimizados para memoria. 

Esta característica elimina eficazmente este cuello de botella y desbloquea un nuevo nivel de escalabilidad para cargas de trabajo con cargas de trabajo pesadas de tempdb. En [!INCLUDE[sql-server-2019](../../includes/sssql19-md.md)], las tablas del sistema implicadas en la administración de metadatos de la tabla temporal del sistema se pueden mover a tablas optimizadas para memoria no duraderas y sin bloqueos temporales.

Vea este vídeo de 7 minutos para obtener información general sobre cómo y cuándo usar los metadatos de tempdb optimizados para memoria:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/How-and-When-To-Memory-Optimized-TempDB-Metadata/player?WT.mc_id=dataexposed-c9-niner]


### <a name="configuring-and-using-memory-optimized-tempdb-metadata"></a>Configuración y uso de metadatos de tempdb con optimización para memoria

Para poder participar en esta nueva característica, use el siguiente script:

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON;
```

Para que este cambio de configuración surta efecto, es necesario reiniciar el servicio.

Puede comprobar si `tempdb` está optimizado para memoria mediante el siguiente comando de T-SQL:

```sql
SELECT SERVERPROPERTY('IsTempdbMetadataMemoryOptimized');
```

En caso de que se produzca un error al iniciar el servidor por algún motivo después de habilitar los metadatos de `tempdb` optimizados para memoria, se puede omitir la característica si se inicia la instancia de SQL Server con una [configuración mínima](../../database-engine/configure-windows/start-sql-server-with-minimal-configuration.md) mediante la opción de inicio **-f**. Después, puede deshabilitar la característica y reiniciar SQL Server en modo normal.

Para proteger el servidor de posibles condiciones de memoria insuficiente, puede enlazar `tempdb` a un [ grupo de recursos](../in-memory-oltp/bind-a-database-with-memory-optimized-tables-to-a-resource-pool.md). Esto se realiza a través del comando [`ALTER SERVER`](../../t-sql/statements/alter-server-configuration-transact-sql.md) en lugar de los pasos que normalmente seguiría para enlazar un grupo de recursos a una base de datos.

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED TEMPDB_METADATA = ON (RESOURCE_POOL = 'pool_name');
```

Este cambio también requiere que el reinicio surta efecto, incluso si los metadatos de tempdb optimizados para memoria ya están habilitados.

### <a name="memory-optimized-tempdb-limitations"></a>Limitaciones de tempdb optimizadas para memoria

- Activar o desactivar la característica no es una acción dinámica. Debido a los cambios intrínsecos que deben realizarse en la estructura de `tempdb`, es necesario llevar a cabo un reinicio para habilitar o deshabilitar la característica.

- Una única transacción no puede acceder a tablas optimizadas para memoria en más de una base de datos. Cualquier transacción que implique una tabla optimizada para memoria en una base de datos de usuario no podrá acceder a vistas del sistema `tempdb` en la misma transacción. Si intenta acceder a vistas del sistema `tempdb` en la misma transacción en forma de tabla optimizada para memoria en una base de datos de usuario, recibirá el error siguiente:
    
  ```
  A user transaction that accesses memory optimized tables or natively compiled modules cannot access more than one user database or databases model and msdb, and it cannot write to master.
  ```
    
  Ejemplo:
    
  ```sql
  BEGIN TRAN;
  
  SELECT *
  FROM tempdb.sys.tables;  -----> Creates a user in-memory OLTP transaction in tempdb
  
  INSERT INTO <user database>.<schema>.<mem-optimized table>
  VALUES (1); ----> Tries to create a user in-memory OLTP transaction in the user database but will fail
  
  COMMIT TRAN;
  ```
    
- Las consultas en tablas optimizadas para memoria no admiten las sugerencias de bloqueo y aislamiento, por lo que las consultas en vistas de catálogo `tempdb` optimizadas para memoria no respetarán dichas sugerencias. Como sucede con otras vistas de catálogo del sistema en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], todas las transacciones realizadas en vistas del sistema estarán en aislamiento `READ COMMITTED` (o, en este caso, `READ COMMITTED SNAPSHOT`).

- Los [índices de almacén de columnas](../indexes/columnstore-indexes-overview.md) no se pueden crear en tablas temporales cuando los metadatos de `tempdb` optimizados para memoria están habilitados.

- Debido a la limitación en los índices de almacén de columnas, no se admite el uso del procedimiento almacenado del sistema `sp_estimate_data_compression_savings` con el parámetro de compresión de datos `COLUMNSTORE` o `COLUMNSTORE_ARCHIVE` cuando se habilitan los metadatos de `tempdb` optimizados para memoria.

> [!NOTE] 
> Estas limitaciones se aplican solo cuando se hace referencia a vistas del sistema `tempdb`. Puede crear una tabla temporal en la misma transacción cuando tenga acceso a una tabla optimizada para memoria en una base de datos de usuario, si lo desea.

## <a name="capacity-planning-for-tempdb-in-sql-server"></a>Planeamiento de capacidad para tempdb en SQL Server
Determinar el tamaño adecuado para `tempdb` en un entorno de producción [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] depende de muchos factores. Como se ha descrito anteriormente, estos factores incluyen la carga de trabajo existente y las características de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se usan. Se recomienda analizar la carga de trabajo existente llevando a cabo las siguientes tareas en un entorno de prueba de SQL Server:

- Active el crecimiento automático para `tempdb`.
- Ejecute consultas individuales o archivos de seguimiento de carga de trabajo y supervise el uso del espacio de `tempdb`.
- Ejecute operaciones de mantenimiento de índice, como volver a generar índices, y supervise el espacio de `tempdb`.
- Use los valores de uso de espacio de los pasos anteriores para predecir el uso de carga de trabajo total. Ajuste este valor para la actividad simultánea proyectada y, luego, establezca el tamaño de `tempdb` según corresponda.

## <a name="monitoring-tempdb-use"></a>Supervisión del uso de tempdb
La falta de espacio en disco en `tempdb` puede provocar interrupciones importantes en el entorno de producción de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. También puede impedir que las aplicaciones que se ejecutan completen las operaciones. Puede utilizar la vista de administración dinámica [sys.dm_db_file_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) para supervisar el espacio en disco que utilizan los archivos de `tempdb`:

```sql
 -- Determining the amount of free space in tempdb
SELECT SUM(unallocated_extent_page_count) AS [free pages],
  (SUM(unallocated_extent_page_count)*1.0/128) AS [free space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by the version store
SELECT SUM(version_store_reserved_page_count) AS [version store pages used],
  (SUM(version_store_reserved_page_count)*1.0/128) AS [version store space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by internal objects
SELECT SUM(internal_object_reserved_page_count) AS [internal object pages used],
  (SUM(internal_object_reserved_page_count)*1.0/128) AS [internal object space in MB]
FROM sys.dm_db_file_space_usage;

-- Determining the amount of space used by user objects
SELECT SUM(user_object_reserved_page_count) AS [user object pages used],
  (SUM(user_object_reserved_page_count)*1.0/128) AS [user object space in MB]
FROM sys.dm_db_file_space_usage;
 ```

Para supervisar la actividad de asignación o desasignación de páginas en `tempdb` en la sesión o tarea, se pueden usar las vistas de administración dinámica [sys.dm_db_session_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-session-space-usage-transact-sql.md) y [sys.dm_db_task_space_usage](../../relational-databases/system-dynamic-management-views/sys-dm-db-task-space-usage-transact-sql.md). Estas vistas pueden ayudarle a detectar consultas grandes, tablas temporales o variables de tabla que emplean mucho espacio de disco de `tempdb`. También puede usar varios contadores para supervisar el espacio disponible en `tempdb` y los recursos que usan `tempdb`.

```sql
-- Obtaining the space consumed by internal objects in all currently running tasks in each session
SELECT session_id,
  SUM(internal_objects_alloc_page_count) AS task_internal_objects_alloc_page_count,
  SUM(internal_objects_dealloc_page_count) AS task_internal_objects_dealloc_page_count
FROM sys.dm_db_task_space_usage
GROUP BY session_id;

-- Obtaining the space consumed by internal objects in the current session for both running and completed tasks
SELECT R2.session_id,
  R1.internal_objects_alloc_page_count
  + SUM(R2.internal_objects_alloc_page_count) AS session_internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count
  + SUM(R2.internal_objects_dealloc_page_count) AS session_internal_objects_dealloc_page_count
FROM sys.dm_db_session_space_usage AS R1
INNER JOIN sys.dm_db_task_space_usage AS R2 ON R1.session_id = R2.session_id
GROUP BY R2.session_id, R1.internal_objects_alloc_page_count,
  R1.internal_objects_dealloc_page_count;;
```

## <a name="related-content"></a>Contenido relacionado
- [Opción SORT_IN_TEMPDB para índices](../../relational-databases/indexes/sort-in-TempDB-option-for-indexes.md)    
- [Bases de datos del sistema](../../relational-databases/databases/system-databases.md)    
- [sys.databases](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)    
- [sys.master_files](../../relational-databases/system-catalog-views/sys-master-files-transact-sql.md)    
- [Mover archivos de base de datos](../../relational-databases/databases/move-database-files.md)    
