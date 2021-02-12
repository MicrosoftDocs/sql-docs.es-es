---
title: ¿Qué son los grupos de proceso?
titleSuffix: SQL Server big data clusters
description: En este artículo se describen los grupos de proceso en un clúster de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 10/15/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3a8816943ca952b11fdaae959f706e49e084386d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047615"
---
# <a name="what-are-compute-pools-in-a-sql-server-big-data-cluster"></a>¿Qué son los grupos de proceso en un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe el rol de los *grupos de proceso de SQL Server* en un clúster de macrodatos de SQL Server. Los grupos de proceso proporcionan recursos computacionales de escalado horizontal para un clúster de macrodatos de SQL Server. Se usan para descargar el trabajo de cálculo o los conjuntos de resultados intermedios de la instancia maestra de SQL Server. En las secciones siguientes se describe la arquitectura, las funciones y los escenarios de uso de un grupo de proceso.

También puede ver este vídeo de 5 minutos para obtener una introducción a los grupos de proceso:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Overview-Big-Data-Cluster-Compute-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="compute-pool-architecture"></a>Arquitectura de grupo de proceso

Un grupo de proceso se compone de uno o varios pods de proceso que se ejecutan en Kubernetes. La creación y administración automatizadas de estos pods se coordinan mediante la [instancia maestra de SQL Server](concept-master-instance.md). Cada pod contiene un conjunto de servicios base y una instancia del motor de base de datos de SQL Server.

![Arquitectura de grupo de proceso](media/concept-compute-pool/compute-pool-architecture.png)

## <a name="scale-out-groups"></a>Grupos de escalado horizontal

Un grupo de proceso puede actuar como grupo de escalado horizontal de PolyBase para las consultas distribuidas en diferentes orígenes de datos externos, como SQL Server, Oracle, MongoDB, Teradata y HDFS. Mediante el uso de pods de proceso en Kubernetes, un clúster de macrodatos de SQL Server puede automatizar la creación y configuración de los pods de proceso para los grupos de escalado horizontal de PolyBase.

## <a name="compute-pool-scenarios"></a>Escenarios de grupos de proceso

Estos son algunos escenarios en los que se usan grupos de proceso:

- Cuando las consultas enviadas a la instancia maestra usan una o más tablas ubicadas en el [grupo de almacenamiento](concept-storage-pool.md).

- Cuando las consultas enviadas a la instancia maestra usan una o más tablas con distribución round robin ubicada en el [grupo de datos](concept-data-pool.md).

- Cuando las consultas enviadas a la instancia maestra usan tablas **con particiones** con orígenes de datos externos de SQL Server, Oracle, MongoDB y Teradata. En este escenario, la sugerencia de consulta OPTION (FORCE SCALEOUTEXECUTION) debe estar habilitada.

- Cuando las consultas enviadas a la instancia maestra usan una o más tablas ubicadas en el [almacenamiento por niveles de HDFS](hdfs-tiering.md).

Estos son algunos escenarios en los que **no** se usan grupos de proceso:

- Cuando las consultas enviadas a la instancia maestra usan una o más tablas en un clúster HDFS de Hadoop externo.

- Cuando las consultas enviadas a la instancia maestra usan una o más tablas ubicadas en Azure Blob Storage.

- Cuando las consultas enviadas a la instancia maestra usan tablas **sin particiones** con orígenes de datos externos de SQL Server, Oracle, MongoDB y Teradata.

- Cuando está habilitada la sugerencia de consulta OPTION (DISABLE SCALEOUTEXECUTION).

- Cuando las consultas enviadas a la instancia maestra se aplican a las bases de datos ubicadas en la instancia maestra.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
