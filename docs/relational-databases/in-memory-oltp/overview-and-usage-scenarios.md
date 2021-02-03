---
title: Información general y escenarios de uso | Microsoft Docs
description: Obtenga información sobre OLTP en memoria, una tecnología de SQL Server y Azure SQL Database para el procesamiento de transacciones optimizado. Revise ejemplos y recursos adicionales.
ms.custom: ''
ms.date: 04/10/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 62c964c5-eae4-4cf1-9024-d5a19adbd652
author: kevin-farlee
ms.author: kfarlee
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8492183591b2069115e8da8d05d89759f5eee46d
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235260"
---
# <a name="overview-and-usage-scenarios"></a>Información general y escenarios de uso

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

OLTP en memoria es la tecnología principal disponible en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)] para optimizar el rendimiento de escenarios de datos transitorios, carga de datos, ingesta de datos y procesamiento de transacciones. En este artículo se incluye información general sobre dicha tecnología y se describen escenarios de uso de OLTP en memoria. Use esta información para determinar si OLTP en memoria es adecuado para la aplicación. El artículo concluye con un ejemplo que muestra objetos de OLTP en memoria, hace referencia a una demostración de rendimiento y a recursos que puede usar para los pasos siguientes.

Este artículo trata sobre la tecnología de OLTP en memoria en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. La siguiente entrada de blog contiene una explicación exhaustiva sobre el rendimiento y los beneficios de usar los recursos de [!INCLUDE[ssSDS](../../includes/sssds-md.md)]: [OLTP en memoria en Azure SQL Database](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

## <a name="in-memory-oltp-overview"></a>Información general de OLTP en memoria

OLTP en memoria puede proporcionar excelentes ganancias de rendimiento para las cargas de trabajo adecuadas. Un cliente, BWIN, logró [alcanzar 1,2 millones de solicitudes por segundo](/archive/blogs/sqlcat/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale) con una sola máquina con [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] en la que ejecutaba OLTP en memoria. Otro cliente, Quorum, logró duplicar su carga de trabajo a la vez que [disminuía en un 70 % su uso de recursos](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database) mediante el uso de OLTP en memoria en [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. A pesar de que los clientes han visto un aumento de rendimiento de hasta 30 veces en algunos casos, el que usted obtendrá depende de la carga de trabajo.

¿De dónde proviene esta ganancia de rendimiento? Básicamente, OLTP en memoria mejora el rendimiento del procesamiento de transacciones al hacer que la ejecución de las transacciones y el acceso a los datos sea más eficaz y al quitar la contención de bloqueo y bloqueo temporal entre transacciones que se ejecutan de manera simultánea: no es rápido porque se haga en memoria, sino que es rápido porque está optimizado en torno a los datos que están en memoria. Los algoritmos de procesamiento, acceso y almacenamiento de datos se rediseñaron desde el principio para aprovechar las mejoras más recientes en los cálculos de alta simultaneidad y en memoria.

Ahora, solo porque los datos residen en memoria, no significa que los pierda si se produce un error. De manera predeterminada, todas las transacciones son completamente duraderas, lo que significa que tiene las mismas garantías de durabilidad que para cualquier otra tabla en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]: como parte de la confirmación de transacciones, todos los cambios se escriben en el registro de transacciones en el disco. Si se produce algún error en cualquier momento después de la confirmación de la transacción, los datos se mantienen cuando la base de datos vuelve a estar en línea. Además, OLTP en memoria funciona con todas las características de alta disponibilidad y recuperación ante desastres de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], como AlwaysOn, copia de seguridad/restauración, etc.

Para usar OLTP en memoria en la base de datos, debe usar uno o varios de los siguientes tipos de objetos:

- Las *tablas con optimización para memoria* se usan para almacenar datos de usuario. Declara que una tabla es una tabla optimizada para memoria en el momento de su creación.
- Las *tablas no duraderas* se usan para los datos transitorios, ya sea para el almacenamiento en caché o para un conjunto de resultados intermedio (que reemplaza las tablas temporales tradicionales). Una tabla no duradera es una tabla optimizada para memoria que se declara con DURABILITY=SCHEMA_ONLY, lo que significa que los cambios en estas tablas no generan ningún E/S. Esto evita que consuman recursos de E/S de registro en los casos donde la durabilidad no es tema.
- Los *tipos de tablas con optimización para memoria* se usan para los parámetros con valores de tabla (TVP), así como conjuntos de resultados intermedios en procedimientos almacenados. Se pueden usar en lugar de los tipos de tablas tradicionales. Las variables de tabla y los TVP que se declaran con un tipo de tabla optimizada para memoria heredan las ventajas de las tablas no duraderas optimizadas para memoria: acceso eficaz a los datos y no E/S.
- Los *módulos T-SQL compilados de manera nativa* se usan para reducir aún más el tiempo que demora una transacción individual mediante la disminución de los ciclos de CPU que se requieren para procesar las operaciones. Declara un módulo Transact-SQL como compilado de manera nativa en el momento de su creación. En este momento, se pueden compilar de manera nativa los siguientes módulos T-SQL: procedimientos almacenados, desencadenadores y funciones escalares definidas por el usuario.

OLTP en memoria está integrado en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Además, y dado que estos objetos tienen un comportamiento similar al de sus homólogos tradicionales, a menudo puede obtener ventajas de rendimiento aplicando solo cambios mínimos en la base de datos y la aplicación. Además, puede tener tablas optimizadas para memoria y tablas basadas en discos tradicionales en la misma base de datos, y ejecutar consultas entre ambas. Hacia el final de este artículo encontrará un script Transact-SQL que muestra un ejemplo de cada uno de estos tipos de objetos.

## <a name="usage-scenarios-for-in-memory-oltp"></a>Escenarios de uso de OLTP en memoria

OLTP en memoria no es un botón rápido mágico y no funciona en todas las cargas de trabajo. Por ejemplo, las tablas optimizadas para memoria no disminuyen realmente el uso de la CPU si la mayoría de las consultas ejecutan la agregación en grandes intervalos de datos; son los índices de almacén de columnas los que ayudan en ese escenario.

A continuación, puede ver una lista de escenarios y patrones de aplicaciones en los que se han visto buenos resultados con OLTP en memoria.

### <a name="high-throughput-and-low-latency-transaction-processing"></a>Procesamiento de transacciones de baja latencia y alto rendimiento

Este es el escenario central para el que compilamos OLTP en memoria: se admiten grandes volúmenes de transacciones con una baja latencia coherente para transacciones individuales.

Los escenarios de carga de trabajo comunes son: comercialización de instrumentos financieros, apuestas deportivas, juegos móviles y publicación de anuncios. Otro patrón común que hemos visto es un "catálogo" que se lee o actualiza con frecuencia. Un ejemplo es cuando tiene archivos de gran tamaño, cada uno de ellos distribuido sobre un número de nodos de clúster, y se cataloga la ubicación de la partición de cada archivo en una tabla optimizada para memoria.

#### <a name="implementation-considerations"></a>Consideraciones de implementación

Use las tablas optimizadas para memoria para sus tablas de transacciones principales, es decir, las tablas con las transacciones con rendimiento más crítico. Use los procedimientos almacenados compilados de manera nativa para optimizar la ejecución de la lógica asociada con la transacción comercial. Cuanta más lógica pueda insertar en los procedimientos almacenados de la base de datos, más ventajas obtendrá gracias a OLTP en memoria.

Para empezar a trabajar en una aplicación existente:

1. Use el [informe de análisis de rendimiento de transacciones](determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md) para identificar los objetos que desea migrar.
2. Use los asesores de [optimización para memoria](memory-optimization-advisor.md) y [compilación nativa](native-compilation-advisor.md) para ayudar en la migración.

#### <a name="customer-case-studies"></a>Casos prácticos de clientes

- CMC Markets aprovecha OLTP en memoria en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para lograr una latencia baja coherente: [Dado que un segundo es demasiado tiempo para esperar, esta empresa de servicios financieros está actualizando su software comercial ahora.](https://customers.microsoft.com/story/because-a-second-is-too-long-to-wait-this-financial-services-firm-is-updating-its-trading-software)
- Derivco aprovecha OLTP en memoria en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para admitir un mayor rendimiento y controlar los picos en la carga de trabajo: [Cuando una empresa de juegos en línea no desea arriesgar su futuro, su apuesta es [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)].](https://customers.microsoft.com/story/when-an-online-gaming-company-doesnt-want-to-risk-its-future-it-bets-on-sql-server-2016)

### <a name="data-ingestion-including-iot-internet-of-things"></a>Ingesta de datos, incluida IoT (Internet de las cosas)

OLTP en memoria es excelente en la ingesta de grandes volúmenes de datos desde distintos orígenes al mismo tiempo. Además, la ingesta de datos en una base de datos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suele ser beneficiosa en comparación con otros destinos, porque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] permite ejecutar las consultas de datos rápido y, además, le permite obtener información en tiempo real.

Patrones comunes de aplicación:

- Ingerir eventos y lecturas de sensores y permitir las notificaciones, además del análisis del historial.
- La administración de actualizaciones por lotes, incluso desde varios orígenes, mientras se minimiza el impacto en la carga de trabajo de lectura simultánea.

#### <a name="implementation-considerations"></a>Consideraciones de implementación

Use una tabla optimizada para memoria para la ingesta de datos. Si la ingesta consta principalmente de inserciones (en lugar de actualizaciones) y el espacio que ocupa el almacenamiento de OLTP en memoria de los datos es una preocupación, puede hacer una de las acciones siguientes:

- Use un trabajo para descargar por lote de manera regular los datos en una tabla basada en discos con un [índice de almacén de columnas en clúster](../indexes/columnstore-indexes-overview.md), a través de un trabajo que hace `INSERT INTO <disk-based table> SELECT FROM <memory-optimized table>`; o bien,
- Use una [tabla temporal optimizada para memoria](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md) para administrar los datos históricos; en este modo, los datos históricos residen en el disco y es el sistema el que administra el movimiento de los datos.

El repositorio de ejemplos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] contiene una aplicación de cuadrícula inteligente que usa una tabla temporal optimizada para memoria, un tipo de tabla optimizada para memoria y un procedimiento almacenado compilado de manera nativa para acelerar la ingesta de datos, mientras que se administra el espacio de almacenamiento de OLTP en memoria de los datos del sensor:

- [smart-grid-release](https://github.com/Microsoft/sql-server-samples/releases/tag/iot-smart-grid-v1.0)
- [smart-grid-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/iot-smart-grid)

#### <a name="customer-case-studies"></a>Casos prácticos de clientes

- [Quorum duplica la carga de trabajo de la base de datos clave mientras disminuye el uso en 70 % con el uso de OLTP en memoria en Azure SQL Database](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)
- EdgeNet mejoró el rendimiento de la carga de datos por lotes y eliminó la necesidad de mantener una caché de nivel medio, con OLTP en memoria en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]: [Una empresa de servicios de datos obtiene acceso en tiempo real a los datos de productos con la tecnología en memoria](https://customers.microsoft.com/story/data-services-firm-gains-real-time-access-to-product-d)
- Beth Israel Deaconess Medical Center ha podido aumentar considerablemente la tasa de ingesta de datos desde los controladores de dominio y manejar los picos en la carga de trabajo con OLTP en memoria en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]: https://customers.microsoft.com/story/strengthening-data-security-and-creating-more-time-for

### <a name="caching-and-session-state"></a>Almacenamiento en caché y estado de sesión

La tecnología de OLTP en memoria hace que SQL sea realmente atractivo para mantener el estado de la sesión (por ejemplo, en una aplicación ASP.NET) y para el almacenamiento en caché.

El estado de sesión de ASP.NET es un caso de uso muy exitoso de OLTP en memoria. Con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], un cliente estuvo a punto de lograr 1,2 millones de solicitudes por segundo. Mientras tanto, comenzó a usar OLTP en memoria para cumplir con las necesidades de almacenamiento en caché de todas las aplicaciones de nivel intermedio de la empresa. Detalles: [Cómo bwin usa OLTP en memoria de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para obtener un rendimiento y una escala sin precedentes](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

#### <a name="implementation-considerations"></a>Consideraciones de implementación

Puede usar tablas no duraderas optimizadas para memoria como un almacén de clave-valor simple mediante el almacenamiento de un BLOB en una columna varbinary(max). De manera alternativa, puede implementar una memoria caché semiestructurada con [compatibilidad de JSON](https://azure.microsoft.com/blog/json-support-is-generally-available-in-azure-sql-database/) en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y [!INCLUDE[ssSDS](../../includes/sssds-md.md)]. Por último, puede crear una caché completamente relacional a través de tablas no duraderas con un esquema completamente relacional, incluidas diversas restricciones y tipos de datos.

Comience con un estado de sesión de ASP.NET optimizado para memoria usando los scripts publicados en GitHub para reemplazar los objetos creados por el proveedor de estado de sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] integrado: [aspnet-session-state](https://github.com/Microsoft/sql-server-samples/tree/master/samples/applications/aspnet-session-state).

#### <a name="customer-case-studies"></a>Casos prácticos de clientes

- bwin pudo aumentar considerablemente el rendimiento y disminuir la superficie de hardware para el estado de sesión de ASP.NET con OLTP en memoria en [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]: [El sitio de juegos puede escalar hasta 250 000 solicitudes por segundo y mejorar la experiencia de los jugadores](https://customers.microsoft.com/story/gaming-site-can-scale-to-250000-requests-per-second-an)
- bwin aumentó incluso más el rendimiento con estado de sesión de ASP.NET e implementó un sistema de almacenamiento de nivel intermedio en toda la empresa con OLTP en memoria en [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]: [Cómo bwin usa OLTP en memoria de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] para obtener un rendimiento y una escala sin precedentes](https://blogs.msdn.microsoft.com/sqlcat/2016/10/26/how-bwin-is-using-sql-server-2016-in-memory-oltp-to-achieve-unprecedented-performance-and-scale/)

### <a name="tempdb-object-replacement"></a>Reemplazo de objeto tempdb

Use las tablas no duraderas y los tipos de tabla optimizada para memoria para reemplazar las estructuras tradicionales basadas en tempdb, como las tablas temporales, las variables de tabla y los parámetros con valores de tabla (TVP).

Las tablas no duraderas y las variables de tabla con optimización para memoria normalmente reducen la CPU y quitan completamente E/S del registro en comparación con la tabla #temp y las variables de tabla tradicionales.

#### <a name="implementation-considerations"></a>Consideraciones de implementación

Para empezar, consulte: [Mejora del rendimiento de la tabla temporal y de variable de tabla mediante optimización de memoria.](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)

#### <a name="customer-case-studies"></a>Casos prácticos de clientes

- Un cliente pudo mejorar el rendimiento en un 40% simplemente reemplazando los TVP tradicionales con TVP optimizados para memoria: [Cómo acelerar la ingesta de datos de IoT con OLTP en memoria en Azure](/archive/blogs/sqlserverstorageengine/a-technical-case-study-high-speed-iot-data-ingestion-using-in-memory-oltp-in-azure)
- SentryOne ha mejorado considerablemente la capacidad de ingesta de datos con una latencia de casi cero en su solución de supervisión mediante el intercambio de las tablas de tempdb con las tablas OLTP en memoria como parte de sus mejoras de escalabilidad empresarial: [Un proveedor de soluciones rompe el techo de rendimiento con innovación de supervisión de datos.](https://customers.microsoft.com/story/sentryone-partner-professional-services-sql-server-azure)

### <a name="etl-extract-transform-load"></a>ETL (extracción, transformación, carga)

A menudo, los flujos de trabajo de ETL incluyen cargar datos en una tabla provisional, transformaciones de los datos y su carga en las tablas finales.

#### <a name="implementation-considerations"></a>Consideraciones de implementación

Use tablas no duraderas optimizadas para memoria para el almacenamiento provisional de datos. Estas tablas quitan completamente E/S y hacen que el acceso a los datos sea más eficaz.

Si hace transformaciones en la tabla de almacenamiento provisional como parte del flujo de trabajo, puede usar procedimientos almacenados compilados de manera nativa para acelerar estas transformaciones. Si puede hacer estas transformaciones en paralelo, obtendrá ventajas adicionales de escalabilidad a partir de la optimización de memoria.

## <a name="sample-script"></a>Script de ejemplo

Antes de que pueda comenzar a usar OLTP en memoria, debe crear un grupo de archivos MEMORY_OPTIMIZED_DATA. Además, se recomienda usar el nivel 130 (o superior) de compatibilidad de base de datos y establecer en ON la opción MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT de base de datos.

Puede usar el script que se encuentra en la siguiente ubicación para crear el grupo de archivos en la carpeta de datos predeterminada y configurar los valores recomendados:

- [enable-in-memory-oltp.sql](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)

El script siguiente ilustra los objetos de OLTP en memoria que puede crear en la base de datos:

```sql
-- configure recommended DB option
ALTER DATABASE CURRENT SET MEMORY_OPTIMIZED_ELEVATE_TO_SNAPSHOT=ON
GO
-- memory-optimized table
CREATE TABLE dbo.table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- non-durable table
CREATE TABLE dbo.temp_table1
( c1 INT IDENTITY PRIMARY KEY NONCLUSTERED,
  c2 NVARCHAR(MAX))
WITH (MEMORY_OPTIMIZED=ON,
      DURABILITY=SCHEMA_ONLY)
GO
-- memory-optimized table type
CREATE TYPE dbo.tt_table1 AS TABLE
( c1 INT IDENTITY,
  c2 NVARCHAR(MAX),
  is_transient BIT NOT NULL DEFAULT (0),
  INDEX ix_c1 HASH (c1) WITH (BUCKET_COUNT=1024))
WITH (MEMORY_OPTIMIZED=ON)
GO
-- natively compiled stored procedure
CREATE PROCEDURE dbo.usp_ingest_table1
  @table1 dbo.tt_table1 READONLY
WITH NATIVE_COMPILATION, SCHEMABINDING
AS
BEGIN ATOMIC
    WITH (TRANSACTION ISOLATION LEVEL=SNAPSHOT,
          LANGUAGE=N'us_english')

  DECLARE @i INT = 1

  WHILE @i > 0
  BEGIN
    INSERT dbo.table1
    SELECT c2
    FROM @table1
    WHERE c1 = @i AND is_transient=0

    IF @@ROWCOUNT > 0
      SET @i += 1
    ELSE
    BEGIN
      INSERT dbo.temp_table1
      SELECT c2
      FROM @table1
      WHERE c1 = @i AND is_transient=1

      IF @@ROWCOUNT > 0
        SET @i += 1
      ELSE
        SET @i = 0
    END
  END

END
GO
-- sample execution of the proc
DECLARE @table1 dbo.tt_table1
INSERT @table1 (c2, is_transient) VALUES (N'sample durable', 0)
INSERT @table1 (c2, is_transient) VALUES (N'sample non-durable', 1)
EXECUTE dbo.usp_ingest_table1 @table1=@table1
SELECT c1, c2 from dbo.table1
SELECT c1, c2 from dbo.temp_table1
GO
```

## <a name="resources-to-learn-more"></a>Recursos para obtener más información

- [Tecnologías de OLTP en memoria para acelerar el rendimiento de Transact-SQL](./survey-of-initial-areas-in-in-memory-oltp.md)
- Puede encontrar una demostración de rendimiento con OLTP en memoria en: [in-memory-oltp-perf-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- [Vídeo de 17 minutos que explica OLTP en memoria y exhibe una demostración](in-memory-oltp-in-memory-optimization.md#anchorname-17minute-video)
- [Script para habilitar OLTP en memoria y establecer las opciones recomendadas](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/in-memory-database/in-memory-oltp/t-sql-scripts/enable-in-memory-oltp.sql)
- [Documentación principal de OLTP en memoria](in-memory-oltp-in-memory-optimization.md)
- [Beneficios del uso de los recursos y el rendimiento de OLTP en memoria en Azure SQL Database](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)
- [Mejora del rendimiento de la tabla temporal y de variable de tabla mediante optimización de memoria](/archive/blogs/sqlserverstorageengine/improving-temp-table-and-table-variable-performance-using-memory-optimization)
- [Optimización del rendimiento mediante las tecnologías en memoria de SQL Database](/azure/sql-database/sql-database-in-memory)
- [Tablas temporales con control de versiones del sistema con tablas con optimización para memoria](../tables/system-versioned-temporal-tables-with-memory-optimized-tables.md)
