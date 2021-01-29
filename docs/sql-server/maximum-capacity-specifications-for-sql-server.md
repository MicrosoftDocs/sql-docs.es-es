---
title: Especificaciones de capacidad máxima para SQL Server
description: En este artículo se muestran los tamaños y números máximos de diversos objetos definidos en los componentes de SQL Server, junto con información adicional.
ms.date: 03/05/2020
ms.prod: sql
ms.reviewer: ''
ms.custom: ''
ms.technology: release-landing
ms.topic: conceptual
helpviewer_keywords:
- objects [SQL Server]
- number capacity specifications [SQL Server]
- size [SQL Server], capacity specifications
- number of objects
- capacity specifications [SQL Server]
- maximum capacity specifications [SQL Server]
- size [SQL Server]
- replication capacity specifications [SQL Server]
- objects [SQL Server], capacity specifications
- Database Engine [SQL Server], capacity specifications
ms.assetid: 13e95046-0e76-4604-b561-d1a74dd824d7
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: b4f0e4ed75e992095cff1f9ae315e3036ba7ef90
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766228"
---
# <a name="maximum-capacity-specifications-for-sql-server"></a>Especificaciones de capacidad máxima para SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

En este artículo se muestran los tamaños y números máximos de diversos objetos definidos en los componentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].

>[!NOTE]
>Además de la información de este artículo, también puede encontrar útiles los vínculos siguientes:
>
>* [Descarga de SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads)
>* [Requisitos de hardware y software para instalar SQL Server](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md)
>* [Comprobación de los parámetros del Comprobador de configuración del sistema](../database-engine/install-windows/check-parameters-for-the-system-configuration-checker.md)
>

## <a name="ssde-objects"></a>Objetos [!INCLUDE[ssDE](../includes/ssde-md.md)].

Tamaño y número máximo de diversos objetos definidos en las bases de datos [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o a los que se hace referencia en las instrucciones [!INCLUDE[tsql](../includes/tsql-md.md)] .

|Objeto de [!INCLUDE[ssDE](../includes/ssde-md.md)] de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Tamaños o cifras máximas [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bits)|Información adicional|
|---------------------------------------------------------|------------------------------------------------------------------|----------------------------|
|Tamaño de lote|65 536 <sup>*</sup> (Tamaño del paquete de red)|El tamaño del paquete de red es el tamaño de los paquetes de flujo TDS que se usan para la comunicación entre aplicaciones y el [!INCLUDE[ssDE](../includes/ssde-md.md)] relacional. El tamaño del paquete predeterminado es 4 KB y se controla mediante la opción de configuración Tamaño de paquete de red.|
|Bytes por columna de cadenas cortas|8,000||
|Bytes por `GROUP BY`, `ORDER BY`|8,060||
|Bytes por clave de índice|900 bytes para un índice agrupado. 1700 para un índice no agrupado.|El número máximo de bytes de una clave de índice agrupado no puede superar los 900 en [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Para una clave de índice no agrupado, el máximo es 1700 bytes.<br /><br /> Puede definir una clave usando columnas de longitud variable cuyos tamaños máximos sumen más del límite. Pero los tamaños combinados de los datos de dichas columnas no pueden superar el límite.<br /><br /> En un índice no agrupado, puede incluir más columnas sin clave. Estas no contarán para el límite de tamaño de la clave. Las columnas sin clave pueden ayudar a que algunas consultas den mejores resultados.|
|Bytes por clave de índice en tablas optimizadas para memoria|2500 bytes para un índice no agrupado. No hay límite para un índice de hash, siempre y cuando todas claves de índice quepan en la fila.|En una tabla optimizada para memoria, un índice no agrupado no puede tener columnas de clave cuyos tamaños máximos declarados superen los 2500 bytes. No importa si los datos reales de las columnas de clave son más cortos que los tamaños máximos declarados.<br /><br /> Las claves de índice de hash no tienen límite máximo de tamaño.<br /><br /> En el caso de los índices de tablas optimizadas para memoria no existe el concepto de columnas incluidas, ya que todos los índices cubren de forma inherente todas las columnas.<br /><br /> En el caso de las tablas optimizadas para memoria, aunque el tamaño de fila sea de 8060 bytes, algunas columnas de longitud variable pueden almacenarse físicamente fuera de esos 8060 bytes. Pero los tamaños máximos declarados de todas las columnas de clave para todos los índices de una tabla, más las columnas adicionales de longitud fija de la tabla, deben caber en dichos 8060 bytes.|
|Bytes por clave externa|900||
|Bytes por clave principal|900||
|Bytes por fila|8,060|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] admite el almacenamiento de desbordamiento de fila, lo que habilita la inserción de columnas de longitud variable de manera no consecutiva. Solo se almacena una raíz de 24 bytes en el registro principal para las columnas de longitud variable insertadas de manera no consecutiva. Esta característica permite que el límite sea más alto que en versiones anteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Para obtener más información, consulte [Compatibilidad con filas largas](../relational-databases/pages-and-extents-architecture-guide.md#large-row-support).|
|Bytes por fila en tablas optimizadas para memoria|8,060|A partir de [!INCLUDE[ssSQL15](../includes/sssql16-md.md)] , las tablas optimizadas para memoria admiten el almacenamiento no consecutivo. Las columnas de longitud variable se insertan de manera no consecutiva si el tamaño máximo de todas las columnas de la tabla supera los 8060 bytes; esta acción es una decisión en tiempo de compilación. Solo se almacena una referencia de 8 bytes de forma consecutiva para las columnas almacenadas de forma no consecutiva. Para obtener más información, vea [Tamaño de tabla y fila de las tablas con optimización para memoria](../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).|
|Bytes en texto de origen de un procedimiento almacenado|El menor del tamaño del lote o 250 MB||
|Bytes por columna `varchar(max) `, `varbinary(max)`, `xml`, `text` o `image`|2^31-1||
|Caracteres por columna `ntext` o `nvarchar(max)`|2^30-1||
|Índices clúster por tabla|1||
|Columnas en `GROUP BY`, `ORDER BY`|Limitado solo por el número de bytes||
|Columnas o expresiones en una instrucción `GROUP BY WITH CUBE` o `WITH ROLLUP`|10||
|Columnas por clave de índice|32|Si la tabla contiene uno o varios índices XML, la clave de agrupación en clústeres de la tabla de usuario estará limitada a 31 columnas, ya que la columna XML se agrega a la clave de agrupación en clústeres del índice XML principal. En [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], puede incluir columnas sin clave en un índice no agrupado para evitar la limitación de un máximo de 32 columnas de clave. Para más información, consulte [Create Indexes with Included Columns](../relational-databases/indexes/create-indexes-with-included-columns.md).|
|Columnas por clave externa o clave principal|32||
|Columnas por instrucción `INSERT`|4 096||
|Columnas por instrucción `SELECT`|4 096||
|Columnas por tabla|1024|Las tablas que contienen conjuntos de columnas dispersas incluyen hasta 30 000 columnas. Vea [Conjuntos de columnas dispersas](../relational-databases/tables/use-column-sets.md).|
|Columnas por instrucción `UPDATE`|4 096|En los [conjuntos de columnas dispersas](../relational-databases/tables/use-column-sets.md) se aplican distintos límites.|
|Columnas por vista|1024||
|Conexiones por cliente|Valor máximo de conexiones configuradas||
|Tamaño de base de datos|524 272 terabytes||
|Bases de datos por instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|32 767||
|Grupos de archivos por base de datos|32 767||
|Grupos de archivo por base de datos para datos optimizados para memoria.|1||
|Archivos por base de datos|32 767||
|Tamaño de archivo (datos)|16 terabytes||
|Tamaño de archivo (registro)|2 terabytes||
|Archivos de datos para datos optimizados para memoria por base de datos|4\.096 en [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)]. Las versiones posteriores de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no imponen un límite tan estricto.||
|Archivo delta por archivo de datos para datos optimizados para memoria|1||
|Referencias de tabla de claves externas por tabla|Saliente = 253. Entrante = 10 000.|Para ver las restricciones, vea [Create Foreign Key Relationships](../relational-databases/tables/create-foreign-key-relationships.md).|
|Longitud del identificador (en caracteres)|128||
|Instancias por equipo|50 instancias en un servidor independiente.<br /><br />25 instancias de clúster de conmutación por error cuando se usan discos de clúster compartidos como almacenamiento.<br/><br/>50 instancias de clúster de conmutación por error con recursos compartidos de archivos SMB como opción de almacenamiento.||
|Índices por tabla optimizada para memoria|999 a partir de [!INCLUDE[ssSQL17](../includes/ssSQL17-md.md)] y en [!INCLUDE[ssSDSFull](../includes/ssSDSFull-md.md)]<br/>8 en [!INCLUDE[ssSQL14](../includes/ssSQL14-md.md)] y [!INCLUDE[ssSQL15](../includes/sssql16-md.md)]||
|Longitud de una cadena que contiene instrucciones SQL (tamaño de lote)|65 536 (tamaño del paquete de red)|El tamaño del paquete de red es el tamaño de los paquetes de flujo TDS que se usan para la comunicación entre aplicaciones y el [!INCLUDE[ssDE](../includes/ssde-md.md)] relacional. El tamaño del paquete predeterminado es 4 KB y se controla mediante la opción de configuración Tamaño de paquete de red.|
|Bloqueos por conexión|Máximo de bloqueos por servidor||
|Bloqueos por instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|Limitado solo por la memoria|Este valor sirve para asignaciones de bloqueo estático. Los bloqueos dinámicos están limitados solo por la memoria.|
|Niveles de procedimientos almacenados anidados|32|Si un procedimiento almacenado accede a más de 64 bases de datos o a más de 2 bases de datos en intercalación, recibirá un error.|
|Subconsultas anidadas|32||
|Transacciones anidadas|4 294 967 296|| 
|Niveles de desencadenadores anidados|32||
|Índices no clúster por tabla|999||
|Número de expresiones distintas en la cláusula `GROUP BY` cuando existe alguna de las opciones siguientes: `CUBE`, `ROLLUP`, `GROUPING SETS`, `WITH CUBE`, `WITH ROLLUP`|32||
|Número de conjuntos de agrupamiento generados por los operadores de la cláusula `GROUP BY`|4 096||
|Parámetros por procedimiento almacenado|2,100||
|Parámetros por función definida por el usuario|2,100||
|REFERENCES por tabla|253||
|Filas por tabla|Limitado por el espacio de almacenamiento disponible||
|Tablas por base de datos|Limitado por el número total de objetos de una base de datos|Los objetos incluyen tablas, vistas, procedimientos almacenados, funciones definidas por el usuario, desencadenadores, reglas, valores predeterminados y restricciones. La suma de todos estos objetos en una base de datos no puede superar 2.147.483.647.|
|Particiones por tabla o índice con particiones|15,000||
|Estadísticas en columnas no indizadas|30,000|| 
|Tablas por instrucción `SELECT`|Limitado solo por los recursos disponibles||
|Desencadenadores por tabla|Limitado por el número de objetos de la base de datos|Los objetos incluyen tablas, vistas, procedimientos almacenados, funciones definidas por el usuario, desencadenadores, reglas, valores predeterminados y restricciones. La suma de todos estos objetos en una base de datos no puede superar 2.147.483.647.|
|Conexiones de usuario|32 767||
|índices XML|249||

## <a name="ssnoversion-utility-objects"></a>Objetos Utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Tamaño y número máximo de diversos objetos que se probaron en la Utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objeto de utilidad|Tamaños o cifras máximas [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bits)|
|----------------------------------------------|------------------------------------------------------------------|
|Equipos (equipos físicos o máquinas virtuales) por cada utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|100|
|Instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] por equipo|5|
|Número total de instancias de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] por cada utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|200<sup>*</sup>|
|Bases de datos de usuario por instancia de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], incluidas las aplicaciones de capa de datos|50|
|Número total de bases de datos de usuario por cada utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|1,000|
|Grupos de archivos por base de datos|1|
|Archivos de datos por grupo de archivos|1|
|Archivos de registro por base de datos|1|
|Volúmenes por equipo|3|

<sup>*</sup> El número máximo de instancias administradas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] admitidas por la Utilidad de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] puede variar según la configuración de hardware del servidor. Para obtener información de introducción, vea [Características y tareas de la utilidad de SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no está disponible en todas las ediciones de [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)]. Para obtener una lista de las características admitidas por las ediciones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], consulte [Características compatibles con las ediciones de SQL Server 2019](./editions-and-components-of-sql-server-version-15.md), [Características compatibles con las ediciones de SQL Server 2017](./editions-and-components-of-sql-server-2017.md) y [Características compatibles con las ediciones de SQL Server 2016](./editions-and-components-of-sql-server-2016.md).

## <a name="ssnoversion-data-tier-application-objects"></a>Objetos de aplicación de capa de datos de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]

Tamaño y número máximo de diversos objetos que se probaron en las aplicaciones de capa de datos (DAC) de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objeto DAC|Tamaños o cifras máximas [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (64 bits)|
|------------------------------------------|------------------------------------------------------------------|
|Bases de datos por DAC|1|
|Objetos por DAC <sup>*</sup>|Se limita por el número de objetos de una base de datos o la memoria disponible.|

<sup>*</sup> Los tipos de objetos incluidos en el límite son usuarios, tablas, vistas, procedimientos almacenados, funciones definidas por el usuario, tipos de datos definidos por el usuario, roles de base de datos, esquemas y tipos de tabla definidos por el usuario.

## <a name="replication-objects"></a>Objetos de replicación

Tamaño y número máximo de los diversos objetos definidos en Replicación de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .

|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Objeto de replicación|Tamaño y número máximo SQL Server (64 bits)|
|--------------------------------------------------|---------------------------------------------------|
|Artículos (publicación de combinación)|2048|
|Artículos (publicación de instantáneas o transaccional)|32 767|
|Columnas de una tabla<sup>*</sup> (publicación de mezcla)|246|
|Columnas de una tabla<sup>**</sup> (instantánea [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o publicación transaccional)|1,000|
|Columnas de una tabla<sup>**</sup> (publicación de instantáneas o transaccional de Oracle)|995|
|Bytes para una columna utilizada en un filtro de fila (publicación de combinación)|1024|
|Bytes para una columna utilizada en un filtro de fila (publicación de instantáneas o transaccional)|8,000|

<sup>*</sup>Si se usa el seguimiento por filas en la detección de conflictos (valor predeterminado), la tabla base puede incluir un máximo de 1024 columnas, pero en el artículo se deben filtrar las columnas de forma que se publique un máximo de 246. Si se utiliza el seguimiento por columna, la tabla base puede incluir 246 columnas como máximo.

<sup>**</sup>La tabla base puede incluir el número máximo de columnas permitido en la base de datos de publicación (1024 para [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]), pero las columnas se deben filtrar desde el artículo si superan el máximo especificado para el tipo de publicación.

## <a name="see-also"></a>Consulte también

[Características y tareas de utilidad de SQL Server](../relational-databases/manage/sql-server-utility-features-and-tasks.md)