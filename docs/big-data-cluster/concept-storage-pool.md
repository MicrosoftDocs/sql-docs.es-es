---
title: ¿Qué es el bloque de almacenamiento?
titleSuffix: SQL Server big data clusters
description: Conozca el rol del grupo de almacenamiento de SQL Server 2019 en un clúster de macrodatos de SQL Server, así como la arquitectura y funcionalidad de un grupo de almacenamiento de SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9d9b74b3d0f3822152927197b71d77731a5c775c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047481"
---
# <a name="what-is-the-storage-pool-in-a-sql-server-big-data-cluster"></a>¿Qué es el grupo de almacenamiento en un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe el papel del *grupo de almacenamiento de SQL Server* en un clúster de macrodatos de SQL Server. En las secciones siguientes se describen la arquitectura y la funcionalidad de un bloque de almacenamiento.

## <a name="storage-pool-architecture"></a>Arquitectura del bloque de almacenamiento

El bloque de almacenamiento es el clúster de HDFS (Hadoop) local en un clúster de macrodatos de SQL Server. Proporciona almacenamiento persistente para datos no estructurados y semiestructurados. Los archivos de datos, como texto delimitado o Parquet, se pueden almacenar en el bloque de almacenamiento. Para conservar el almacenamiento, cada pod del grupo tiene un volumen persistente asociado. Los archivos del grupo de almacenamiento son accesibles a través de [PolyBase](../relational-databases/polybase/polybase-guide.md) mediante SQL Server o directamente con una puerta de enlace de Apache Knox.

Una configuración de HDFS clásica se compone de un conjunto de equipos de hardware estándar con almacenamiento asociado. Los datos se distribuyen en bloques por los nodos para la tolerancia a errores y aprovechan el procesamiento paralelo. Uno de los nodos del clúster funciona como nodo de nombre y contiene la información de metadatos sobre los archivos ubicados en los nodos de datos.

![Configuración de HDFS clásica](media/concept-storage-pool/classic-hdfs-setup.png)

El bloque de almacenamiento está formado por nodos de almacenamiento que son miembros de un clúster de HDFS. Ejecuta uno o varios pods de Kubernetes, y cada pod hospeda los contenedores siguientes:

- Un contenedor de Hadoop vinculado a un volumen persistente (almacenamiento). Todos los contenedores de este tipo forman el clúster de Hadoop. Dentro del contenedor de Hadoop hay un proceso de administrador de nodos de YARN que puede crear procesos de trabajo de Apache Spark bajo petición. El nodo principal de Spark hospeda el metastore de Hive, el historial de Spark y los contenedores de historial de trabajos de YARN.
- Una instancia de SQL Server para leer datos de HDFS mediante la tecnología OpenRowSet.
- `collectd` para recopilar datos de métricas.
- `fluentbit` para recopilar datos de registro.

![Arquitectura del bloque de almacenamiento](media/concept-storage-pool/scale-big-data-on-demand.png)

## <a name="responsibilities"></a>Responsabilidades

Los nodos de almacenamiento se encargan de varios aspectos:

- Ingesta de datos a través de Apache Spark.
- Almacenamiento de datos en HDFS (formato de texto delimitado y Parquet). HDFS también proporciona persistencia de datos, ya que los datos de HDFS se reparten por todos los nodos de almacenamiento de BDC de SQL.
- Acceso a datos a través de los puntos de conexión de HDFS y SQL Server.

## <a name="accessing-data"></a>Acceso a datos

Estos son los métodos principales para acceder a los datos del bloque de almacenamiento:

- Trabajos de Spark.
- Uso de tablas externas de SQL Server para permitir la consulta de los datos mediante nodos de proceso de PolyBase y las instancias de SQL Server que se ejecutan en los nodos HDFS.

También puede interactuar con HDFS mediante:

- Azure Data Studio.
- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].
- kubectl para emitir comandos al contenedor de Hadoop.
- Puerta de enlace HTTP de HDFS.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
