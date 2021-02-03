---
description: Directrices para operaciones de índices en línea
title: Directrices para operaciones de índices en línea | Microsoft Docs
ms.custom: ''
ms.date: 11/12/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- clustered indexes, online operations
- online index operations
- indexes [SQL Server], online operations
- disk space [SQL Server], indexes
- nonclustered indexes [SQL Server], online operations
- transaction logs [SQL Server], indexes
ms.assetid: d82942e0-4a86-4b34-a65f-9f143ebe85ce
author: MikeRayMSFT
ms.author: mikeray
ms.prod_service: table-view-index, sql-database
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 9612070531dc13d9646d439501925f59f62c8da7
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236909"
---
# <a name="guidelines-for-online-index-operations"></a>Directrices para operaciones de índices en línea

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Al realizar operaciones de índice en línea se aplican las siguientes directrices:  

- Los índices agrupados deben crearse, volver a compilarse o quitarse sin conexión cuando la tabla subyacente contienen los siguientes tipos de datos de objetos grandes (LOB): **image**, **ntext** y **text**.  
- Los índices no clúster que no son únicos se pueden crear en línea cuando la tabla contiene tipos de datos LOB, pero ninguna de estas columnas se utiliza en la definición del índice como columna de clave o sin clave (incluida).  
- Los índices de tablas temporales locales no se pueden crear, reconstruir o quitar en línea. Esta restricción no se aplica a los índices de tablas temporales globales.
- Los índices se pueden reanudar desde la ubicación en la que se hayan detenido tras un error inesperado, una conmutación por error de la base de datos o un comando **PAUSE**. Consulte [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) y [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).

> [!NOTE]  
> Las operaciones de índices en línea no están disponibles en todas las ediciones de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vea [Features supported by editions](../../sql-server/editions-and-components-of-sql-server-2016.md) (Características compatibles con las ediciones de SQL Server 2016).  

En la siguiente tabla se muestran operaciones de índice que se pueden llevar a cabo estando en línea, los índices que se excluyen de esas operaciones en línea y restricciones de índice reanudables. También se incluyen restricciones adicionales.  

| Operación de índice en línea | Índices excluidos | Otras restricciones |  
|----------------------------|----------------------|------------------------|  
|ALTER INDEX REBUILD|Índice clúster deshabilitado o vista indizada deshabilitada<br /><br /> Índice XML<br /><br />Índice de almacén de columnas <br /><br /> Índice de una tabla temporal local|Si se especifica la palabra clave ALL la operación puede ser errónea si la tabla contiene un índice excluido.<br /><br /> Se aplican restricciones adicionales para reconstruir índices deshabilitados. Para obtener más información, vea [Deshabilitar índices y restricciones](../../relational-databases/indexes/disable-indexes-and-constraints.md).|  
|CREATE INDEX|Índice XML<br /><br /> Índice clúster único inicial en una vista<br /><br /> Índice de una tabla temporal local||  
|CREATE INDEX WITH DROP_EXISTING|Índice clúster deshabilitado o vista indizada deshabilitada<br /><br /> Índice de una tabla temporal local<br /><br /> Índice XML||  
|DROP INDEX|Índice deshabilitado<br /><br /> Índice XML<br /><br /> Índice no clúster<br /><br /> Índice de una tabla temporal local|No se pueden especificar varios índices en una única instrucción.|  
|ALTER TABLE ADD CONSTRAINT (PRIMARY KEY o UNIQUE)|Índice de una tabla temporal local<br /><br /> Índice agrupado|Solo se permite una subcláusula cada vez. Por ejemplo, no puede agregar y quitar restricciones PRIMARY KEY o UNIQUE en la misma instrucción ALTER TABLE.|  
|ALTER TABLE DROP CONSTRAINT (PRIMARY KEY o UNIQUE)|Índice agrupado||  
  
 La tabla subyacente no se puede modificar, truncar o quitar mientras se está llevando a cabo una operación de índice en línea.  
  
 La configuración de opción en línea (ON u OFF) especificada al crear o quitar un índice clúster se aplica a índices no clúster que se deben reconstruir. Por ejemplo, si el índice clúster se genera en línea utilizando CREATE INDEX WITH DROP_EXISTING, ONLINE=ON, todos los índices no clúster asociados se vuelven a crear en línea también.  
  
 Cuando crea o reconstruye un índice UNIQUE en línea, el generador de índices y una transacción de usuario simultánea pueden intentar insertar la misma clave, infringiendo su unicidad. Si una fila especificada por un usuario se inserta en el nuevo índice (destino) antes de que la fila original de la tabla de origen se mueva al nuevo índice, se producirá un error en la operación de índice en línea.  
  
 Aunque no es común, la operación de índice en línea puede causar un interbloqueo cuando interactúa con las actualizaciones de la base de datos debido a las actividades de una aplicación o de un usuario. En esos casos poco comunes, el [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] seleccionará el usuario o la actividad de la aplicación como sujeto de interbloqueo.  
  
 Solo puede realizar operaciones DDL de índice en línea simultáneas en la misma tabla o vista cuando crea varios índices no clúster o reorganiza índices no clúster. Se producirá un error en todas las operaciones de índice en línea que se realizan al mismo momento. Por ejemplo, no puede crear un índice en línea mientras reconstruye un índice en línea existente en la misma tabla.  
  
 No se puede realizar una operación en línea cuando un índice contiene una columna del tipo de objetos grandes y en la misma transacción hay operaciones de actualización delante de esta operación en línea. Para solucionar temporalmente este problema, ponga la operación en línea fuera de la transacción o colóquela antes que cualquier actualización en la transacción.  
  
## <a name="disk-space-considerations"></a>Consideraciones acerca del espacio en disco

Las operaciones de índice en línea requieren más espacio en disco necesario que las operaciones de índice sin conexión.

- Durante las operaciones de creación y regeneración de índices, se requiere espacio adicional para el índice que se crea o regenera.
- Además, se requiere espacio en disco para el índice de asignación temporal. Este índice temporal se utiliza en operaciones de índice en línea que crean, reconstruyen o quitan un índice clúster.
- Para quitar un índice agrupado en línea se requiere tanto espacio como para crearlo o regenerarlo.

Para más información, consulte [Disk Space Requirements for Index DDL Operations](../../relational-databases/indexes/disk-space-requirements-for-index-ddl-operations.md).  
  
## <a name="performance-considerations"></a>Consideraciones de rendimiento

Aunque las operaciones de índice en línea permiten actividades de actualización de usuario simultáneas, las operaciones de índice tardan más si la actividad de actualización es muy grande. Normalmente, las operaciones de índice en línea son más lentas que las operaciones de índice sin conexión equivalentes, independientemente del nivel de actividad de actualización simultánea.  
  
Como las estructuras de origen y de destino se mantienen durante la operación de índice en línea, el uso de recursos para insertar, actualizar y eliminar transacciones aumenta, potencialmente hasta el doble. Esto puede provocar una reducción del rendimiento y un mayor uso de los recursos, especialmente de tiempo de CPU, durante la operación de índice. Las operaciones de índice en línea se registran totalmente.  
  
Aunque se recomiendan las operaciones en línea, se debe evaluar el entorno y los requisitos específicos. Puede ser mejor ejecutar operaciones de índice sin conexión. Al hacerlo así, los usuarios tienen acceso restringido a los datos durante la operación, pero la operación acaba más rápido y utiliza menos recursos.  
  
En los equipos con varios procesadores que ejecutan SQL Server 2016, puede que las instrucciones de índice usen más procesadores para realizar las operaciones de examen y ordenación asociadas a la instrucción, al igual que hacen otras consultas. Puede utilizar la opción de índice MAXDOP para controlar el número de procesadores dedicados a la operación de índice en línea. De este modo, puede equilibrar los recursos utilizados por la operación de índice con los de los usuarios simultáneos. Para obtener más información, vea [Configurar operaciones de índice en paralelo](../../relational-databases/indexes/configure-parallel-index-operations.md). Para obtener más información sobre las ediciones de SQL que admiten operaciones indexadas en paralelo, vea [Features Supported by edition](../../sql-server/editions-and-components-of-sql-server-2016.md) (Características compatibles con las ediciones de SQL Server 2016).  
  
Debido a que un bloqueo S o un bloqueo Sch-M se conservan en la fase final de la operación de índice, debe tener cuidado cuando ejecute una operación de índice en línea dentro de una transacción de usuario explícita, como el bloque BEGIN TRANSACTION...COMMIT. De esta manera el bloqueo se conserva hasta el final de la transacción y se impide la simultaneidad de usuarios.  
  
La regeneración de índices en línea puede aumentar la fragmentación cuando se puede ejecutar con las opciones `MAX DOP > 1` y `ALLOW_PAGE_LOCKS = OFF` . Para obtener más información, vea el blog [How It Works: Online Index Rebuild - Can Cause Increased Fragmentation](/archive/blogs/psssql/how-it-works-online-index-rebuild-can-cause-increased-fragmentation)(Cómo la regeneración de índices en línea puede provocar una fragmentación mayor).  
  
## <a name="transaction-log-considerations"></a>Consideraciones del registro de transacciones

Las operaciones de índice a gran escala, realizadas sin conexión o en línea, pueden generar grandes cargas de datos que pueden hacer que el registro de transacciones se llene rápidamente. Para estar seguros de que la operación de índice se pueda revertir, el registro de transacciones no se puede truncar hasta que se haya completado la operación de índice; no obstante, se puede realizar una copia de seguridad del registro durante la operación de índice. Por lo tanto, el registro de transacciones debe tener suficiente espacio para almacenar las transacciones de la operación de índice y cualquier transacción de usuario simultánea durante la operación de índice. Para más información, consulte [Transaction Log Disk Space for Index Operations](../../relational-databases/indexes/transaction-log-disk-space-for-index-operations.md).  

## <a name="resumable-index-considerations"></a>Consideraciones sobre índices reanudables

> [!NOTE]
> La opción de índice reanudable para crear y recompilar índices se aplica a SQL Server (la recompilación de índices empieza con SQL Server 2017 y la creación de índices también se admite en SQL Server 2019) y SQL Database. Consulte [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md) y [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md).

Al realizar la recompilación o la creación de índices en línea reanudables, se aplican las siguientes directrices:

- Administración, planeamiento y ampliación de los períodos de mantenimiento del índice. Puede pausar y reiniciar una operación de recompilación o creación de índice varias veces para que se ajuste a los períodos de mantenimiento.
- Recuperación de errores de recompilación o creación de índice (por ejemplo, las conmutaciones por error de base de datos o quedarse sin espacio en disco).
- Cuando una operación de índice está en pausa, tanto el índice original como el recién creado requieren espacio en disco y deben actualizarse durante las operaciones de DML.
- Permite el truncamiento de registros de transacciones durante la operación de recompilación o creación de índice.
- Opción SORT_IN_TEMPDB=ON no admitida

> [!IMPORTANT]
> La recompilación o creación de índice reanudable no le exige que mantenga abierta una transacción de larga ejecución, lo que permite el truncamiento del registro durante esta operación y una mejor administración del espacio de registro. Con el nuevo diseño, se consigue mantener los datos necesarios en una base de datos junto con todas las referencias necesarias para reiniciar la operación reanudable.

Por lo general, no hay ninguna diferencia de rendimiento entre la recompilación de índices en línea reanudables y no reanudables. Para crear el índice reanudable, hay una sobrecarga constante que provoca una pequeña diferencia de rendimiento entre la creación de índices reanudables y no reanudables. Esta diferencia se aprecia principalmente solo en las tablas más pequeñas.

Al actualizar un índice reanudable mientras una operación de índice está en pausa:

- Para las cargas de trabajo principalmente de solo lectura, el impacto de rendimiento es insignificante.
- Para las cargas de trabajo con muchas actualizaciones, puede experimentar una reducción del rendimiento (nuestras pruebas muestran una inferior al 10 %).

Por lo general, no hay ninguna diferencia en la calidad de desfragmentación entre la recompilación o la creación de índices en línea reanudables y no reanudables.

## <a name="online-default-options"></a>Opciones predeterminadas en línea

> [!IMPORTANT]
> Estas opciones están en fase de versión preliminar pública para [!INCLUDE[ssNoVersion](../../includes/sssql19-md.md)].

Para establecer opciones predeterminadas para ONLINE o RESUMABLE en un nivel de base de datos, establezca las opciones de configuración de ámbito de base de datos ELEVATE_ONLINE o ELEVATE_RESUMABLE. Con estas opciones predeterminadas puede evitar que se lleve a cabo por error una operación que desactive la tabla de base de datos. Ambas opciones harán que el motor eleve automáticamente determinadas operaciones a la ejecución ONLINE o RESUMABLE.  
Puede establecer estas opciones en FAIL_UNSUPPORTED, WHEN_SUPPORTED u OFF con el comando [ALTER DATABASE SCOPED CONFIGURATION](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md). Puede establecer valores diferentes para ONLINE y RESUMABLE.

ELEVATE_ONLINE y ELEVATE_RESUMABLE solo se aplican a las instrucciones de DDL que admiten la sintaxis de ONLINE y RESUMABLE, respectivamente. Por ejemplo, si intenta crear un índice XML con ELEVATE_ONLINE=FAIL_UNSUPORTED, la operación se ejecutará sin conexión, ya que los índices XML no admiten la sintaxis ONLINE=. Las opciones solo afectan a las instrucciones de DDL que se envían sin especificar una opción ONLINE o RESUMABLE. Por ejemplo, si se envía una instrucción con ONLINE=OFF o RESUMABLE=OFF, el usuario puede reemplazar un valor de configuración FAIL_UNSUPPORTED y ejecutar una instrucción sin conexión o no reanudable.

> [!NOTE]
> ELEVATE_ONLINE y ELEVATE_RESUMABLE no se aplican a las operaciones de índice XML.

## <a name="related-content"></a>Contenido relacionado

- [Cómo funcionan las operaciones de índice en línea](../../relational-databases/indexes/how-online-index-operations-work.md)  
- [Realizar operaciones de índice en línea](../../relational-databases/indexes/perform-index-operations-online.md)  
- [ALTER INDEX](../../t-sql/statements/alter-index-transact-sql.md)  
- [CREATE INDEX](../../t-sql/statements/create-index-transact-sql.md)