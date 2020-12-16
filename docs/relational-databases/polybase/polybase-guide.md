---
title: ¿Qué es PolyBase? | Microsoft Docs
description: PolyBase permite que la instancia de SQL Server procese consultas de Transact-SQL que lean datos de orígenes de datos externos como Hadoop y Azure Blob Storage.
ms.date: 06/10/2019
ms.prod: sql
ms.technology: polybase
ms.topic: overview
f1_keywords:
- PolyBase
- PolyBase, guide
helpviewer_keywords:
- PolyBase
- PolyBase, overview
- Hadoop import
- Hadoop export
- Hadoop export, PolyBase overview
- Hadoop import, PolyBase overview
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: ''
monikerRange: '>=sql-server-2016||>=sql-server-linux-2017||>=aps-pdw-2016||=azure-sqldw-latest'
ms.openlocfilehash: 43d5d214f1720513955c27c45349da74afe3888e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473366"
---
# <a name="what-is-polybase"></a>¿Qué es PolyBase?

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

<!--SQL Server 2016/2017-->
::: moniker range="= sql-server-2016 || = sql-server-2017 || >= aps-pdw-2016 || = azure-sqldw-latest"

PolyBase permite que la instancia de SQL Server 2016 procese consultas Transact-SQL que leen datos de Hadoop. La misma consulta también puede acceder a las tablas relacionales de SQL Server. PolyBase permite que la misma consulta también combine los datos de Hadoop y SQL Server. En SQL Server, una [tabla externa](../../t-sql/statements/create-external-table-transact-sql.md) o un [origen de datos externo](../../t-sql/statements/create-external-data-source-transact-sql.md) proporciona la conexión a Hadoop.

![PolyBase lógico](../../relational-databases/polybase/media/polybase-logical.png "PolyBase lógico")

PolyBase inserta algunos cálculos en el nodo de Hadoop para optimizar la consulta global. Sin embargo, el acceso externo de PolyBase no se limita a Hadoop. También se admiten otras tablas no relacionales no estructuradas, como archivos de texto delimitado.

> [!TIP]
> SQL Server 2019 presenta nuevos conectores para PolyBase, incluido SQL Server, Oracle, Teradata y MongoDB. Para obtener más información, consulte la [documentación de PolyBase para SQL Server 2019](polybase-guide.md?view=sql-server-ver15).

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

PolyBase permite que la instancia de SQL Server procese consultas Transact-SQL que lean datos de orígenes de datos externos. SQL Server 2016 y versiones posterior puede tener acceso a datos externos en Hadoop y Azure Blob Storage. A partir de SQL Server 2019, ya puede usar PolyBase para tener acceso a datos externos en [SQL Server](polybase-configure-sql-server.md), [Oracle](polybase-configure-oracle.md), [Teradata](polybase-configure-teradata.md) y [MongoDB](polybase-configure-mongodb.md).

Las mismas consultas que acceden a datos externos pueden dirigirse también a tablas relacionales en la instancia de SQL Server. Esto permite combinar datos de orígenes externos con datos relacionales de gran valor en la base de datos. En SQL Server, una [tabla externa](../../t-sql/statements/create-external-table-transact-sql.md) o un [origen de datos externo](../../t-sql/statements/create-external-data-source-transact-sql.md) proporciona la conexión a Hadoop.

PolyBase inserta algunos cálculos en el nodo de Hadoop para optimizar la consulta global. Sin embargo, el acceso externo de PolyBase no se limita a Hadoop. También se admiten otras tablas no relacionales no estructuradas, como archivos de texto delimitado.

::: moniker-end

### <a name="supported-sql-products-and-services"></a>Servicios y productos de SQL compatibles

PolyBase proporciona estas mismas funcionalidades para los siguientes productos SQL de Microsoft:

- SQL Server 2016 y versiones posteriores (solo Windows)
- Analytics Platform System (anteriormente Almacenamiento de datos paralelos)
- Azure Synapse Analytics

### <a name="azure-integration"></a>Integración con Azure

Con la ayuda de PolyBase, las consultas T-SQL también pueden importar y exportar datos desde Azure Blob Storage. Además, PolyBase permite a Azure Synapse Analytics importar y exportar datos desde Azure Data Lake Store y Azure Blob Storage.

## <a name="why-use-polybase"></a>Por qué usar PolyBase

En el pasado era más difícil combinar los datos de SQL Server con datos externos. Tenía las dos opciones desagradables siguientes:

- Transferir la mitad de sus datos de modo que todos sus datos estuvieran en un formato u otro.
- Consultar ambos orígenes de datos, después escribir una lógica de consulta personalizada para combinar e integrar los datos en el nivel de cliente.

PolyBase evita esas opciones incómodas mediante el uso de T-SQL para combinar los datos.

Por simplificar, PolyBase no requiere que instale más software en el entorno de Hadoop. Los datos externos se consultan mediante la misma sintaxis T-SQL que se utiliza para consultar una tabla de base de datos. Las acciones de asistencia implementadas por PolyBase son transparentes. El autor de la consulta no necesita ningún conocimiento sobre Hadoop.

### <a name="polybase-uses"></a>Usos de PolyBase

PolyBase permite estos escenarios en SQL Server:

- **Consultar datos almacenados en Hadoop desde SQL Server o PDW.** Los usuarios almacenan datos en sistemas rentables, distribuidos y escalables, como Hadoop. PolyBase facilita la consulta de datos mediante T-SQL.

- **Consultar datos almacenados en Azure Blob Storage.** El almacenamiento de blobs de Azure es un lugar muy cómodo donde almacenar datos para que los usen los servicios de Azure.  PolyBase facilita el acceso a los datos mediante T-SQL.

- **Importación de datos desde Hadoop, Azure Blob Storage o Azure Data Lake Store** Saque partido de la velocidad de la tecnología de almacén de columnas y las capacidades de análisis de Microsoft SQL e importe datos desde Hadoop, Azure Blob Storage o Azure Data Lake Store en tablas relacionales. No es necesaria ninguna herramienta independiente de ETL o de importación.

- **Exportar datos a Hadoop, Azure Blob Storage o Azure Data Lake Store** Archive datos en Hadoop, Azure Blob Storage o Azure Data Lake Store para disfrutar de un almacenamiento rentable y mantenerlo en línea para un fácil acceso.

- **Integrarse con herramientas de BI.** Use PolyBase con la pila de análisis y la inteligencia empresarial de Microsoft o recurra a cualquier herramienta de terceros que sea compatible con SQL Server.

## <a name="performance"></a>Rendimiento

- **Inserción de cálculo en Hadoop.** El optimizador de consultas toma una decisión basada en costos para insertar cálculo en Hadoop si, al hacerlo, se va a mejorar el rendimiento de las consultas.  El optimizador de consultas usa estadísticas relativas a las tablas externas para tomar la decisión basada en costos. La inserción de cálculo crea trabajos MapReduce y aprovecha los recursos de cálculo distribuidos de Hadoop.

- **Escala de los recursos de cálculo.** Para mejorar el rendimiento de las consultas, puede usar [grupos de escalado horizontal de PolyBase](../../relational-databases/polybase/polybase-scale-out-groups.md)de SQL Server. Gracias a esto, la transferencia de datos paralelos entre instancias de SQL Server y nodos de Hadoop es factible y, además, se agregan recursos de cálculo para operar en los datos externos.

<!--SQL Server 2016/2017-->
::: moniker range="=sql-server-2016||=sql-server-2017"

## <a name="next-steps"></a>Pasos siguientes

Antes de usar PolyBase, debe [instalar la característica PolyBase](polybase-installation.md). Después, eche un vistazo a estas guías de configuración según el origen de datos:

- [Hadoop](polybase-configure-hadoop.md)
- [Azure Blob Storage](polybase-configure-azure-blob-storage.md)

::: moniker-end
<!--SQL Server 2019-->
::: moniker range=">= sql-server-linux-ver15||>= sql-server-ver15"

## <a name="next-steps"></a>Pasos siguientes

Antes de usar PolyBase, debe [instalar la característica PolyBase](polybase-installation.md). Después, eche un vistazo a estas guías de configuración según el origen de datos:
- [Hadoop](polybase-configure-hadoop.md)
- [Azure Blob Storage](polybase-configure-azure-blob-storage.md)
- [SQL Server](polybase-configure-sql-server.md)
- [Oracle](polybase-configure-oracle.md)
- [Teradata](polybase-configure-teradata.md)
- [MongoDB](polybase-configure-mongodb.md)
- [Tipos genéricos de ODBC](polybase-configure-odbc-generic.md)

::: moniker-end
