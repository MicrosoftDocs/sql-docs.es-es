---
title: Propiedades de configuración de Clústeres de macrodatos de SQL Server
titleSuffix: SQL Server big data clusters
description: Artículo de referencia de las propiedades de configuración de Clústeres de macrodatos.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/11/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: ecaba704d9c08619f42c5cdf8d726917ccc61b9c
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343545"
---
# <a name="sql-server-big-data-clusters-configuration-properties"></a>Propiedades de configuración de Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Los valores de configuración de Clústeres de macrodatos pueden definirse en los siguientes ámbitos: `cluster`, `service` y `resource`. La jerarquía de la configuración también sigue este orden, de mayor a menor. Los componentes de BDC tomarán el valor de la configuración que se define en el ámbito inferior. Si la configuración no se define en un ámbito determinado, heredará el valor de su ámbito principal superior. A continuación se muestra una lista de las opciones de configuración disponibles para cada componente de BDC en los distintos ámbitos. También puede ver los valores configurables de BDC mediante azdata.

## <a name="bdc-cluster-scope-settings"></a>Configuración del ámbito del clúster de BDC
Puede configurar las siguientes opciones en el ámbito del clúster.

|Propiedad|Opciones|
| --- | --- |
|`mssql.telemetry`|`customerfeedback = { true | false }` |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="sql-service-scope-settings"></a>Configuración del ámbito del servicio SQL
Puede configurar las siguientes opciones en el ámbito del servicio SQL.

|Propiedad|Opciones|
| --- | --- |
|`mssql.language`|`lcid = <language_identifier>` |

## <a name="spark-service-scope-settings"></a>Configuración del ámbito del servicio Spark
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="hdfs-service-scope-settings"></a>Configuración del ámbito del servicio HDFS
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="gateway-service-scope-settings"></a>Configuración del ámbito del servicio de la puerta de enlace
No se puede configurar el ámbito del servicio de la puerta de enlace. En su lugar, configure el ámbito del recurso de la puerta de enlace.

## <a name="app-service-scope-settings"></a>Configuración del ámbito del servicio de la aplicación
Ninguna opción disponible

## <a name="master-pool-resource-scope-settings"></a>Configuración del ámbito del recurso del grupo maestro
|Propiedad|Opciones|
| --- | --- |
|`mssql.sqlagent`|`enabled = { true | false }` |
|`mssql.licensing`|`pid = { Enterprise | Developer }` |
<!-- |`mssql.collation`|`x = <language_identifier>` | -->

> [!NOTE]
> Cambiar la intercalación predeterminada de una instancia de SQL Server es una operación compleja. Además de cambiar el parámetro `mssql.collation`, puede que tenga que volver a crear las bases de datos de usuario y todos los objetos que contienen. Para obtener instrucciones sobre cómo hacerlo, vea el artículo [Configurar o cambiar la intercalación del servidor](../relational-databases/collations/set-or-change-the-server-collation.md#changing-the-server-collation-in-sql-server).

## <a name="storage-pool-resource-scope-settings"></a>Configuración del ámbito del recurso del bloque de almacenamiento
El bloque de almacenamiento consta de los componentes de SQL, Spark y HDFS.

### <a name="available-sql-configurations"></a>Configuraciones disponibles de SQL
|Propiedad|Opciones|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.storagePoolCacheSize`| |
|`mssql.storagePoolMaxCacheSize`| |
|`mssql.storagePoolCacheAutogrowth`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |


### <a name="available-apache-spark-and-hadoop-configurations"></a>Configuraciones disponibles de Apache Spark y Hadoop
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="data-pool-resource-scope-settings"></a>Configuración del ámbito del recurso del grupo de datos
|Propiedad|Opciones|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="compute-pool-resource-scope-settings"></a>Configuración del ámbito del recurso del grupo de proceso
|Propiedad|Opciones|
| --- | --- |
|`mssql.degreeOfParallelism`| |
|`mssql.minServerMemory`| |
|`mssql.maxServerMemory`| |
|`mssql.network.tlscert`| |
|`mssql.network.tlskey`| |
|`mssql.numberOfCpus`| |
|`mssql.tempdb.autogrowthPerDataFile`| |
|`mssql.tempdb.autogrowthPerLogFile`| |
|`mssql.tempdb.dataFileSize`| |
|`mssql.tempdb.dataFileMaxSize`| |
|`mssql.tempdb.logFileMaxSize`| |
|`mssql.tempdb.numberOfDataFiles`| |
|`mssql.traceflag`|`traceflag<#> = <####>` |

## <a name="spark-pool-resource-scope-settings"></a>Configuración del ámbito del recurso del grupo de Spark
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="gateway-resource-scope-settings"></a>Configuración del ámbito del recurso de la puerta de enlace
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="sparkhead-resource-scope-settings"></a>Configuración del ámbito del recurso de `Sparkhead`
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="zookeeper-resource-scope-settings"></a>Configuración del ámbito del recurso de Zookeeper
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="namenode-resource-scope-settings"></a>Configuración del ámbito del recurso de NameNode
Consulte el artículo sobre la [configuración de Apache Spark y Apache Hadoop](reference-config-spark-hadoop.md) para ver todas las configuraciones admitidas y no admitidas.

## <a name="app-proxy-resource-scope-settings"></a>Configuración del ámbito del recurso del proxy de aplicación
Ninguna opción disponible

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Clústeres de macrodatos de SQL Server](configure-bdc-overview.md)