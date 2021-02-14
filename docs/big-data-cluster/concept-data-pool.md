---
title: ¿Qué son los grupos de datos?
titleSuffix: SQL Server big data clusters
description: Conozca el rol de los grupos de datos de SQL Server en un clúster de macrodatos de SQL Server, así como la arquitectura y funcionalidad de un grupo de datos de SQL.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69ef4f739b35c55a31cb19fd8cb317d7750f8639
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047495"
---
# <a name="what-are-data-pools-in-a-sql-server-big-data-cluster"></a>¿Qué son los grupos de datos en un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describe el rol de los *grupos de datos de SQL Server* en un clúster de macrodatos de SQL Server. En las secciones siguientes se describen la arquitectura, la funcionalidad y los escenarios de uso de un grupo de datos.

Este vídeo de 5 minutos presenta grupos de datos y muestra cómo consultar los datos de los grupos de datos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Querying-Data-from-Big-Data-Cluster-Data-Pool/player?WT.mc_id=dataexposed-c9-niner]

## <a name="data-pool-architecture"></a>Arquitectura de un grupo de datos

Un grupo de datos consta de una o varias instancias de grupo de datos de SQL Server que proporcionan almacenamiento persistente de SQL Server para el clúster. Permite consultas de alto rendimiento de los datos en caché sobre orígenes de datos externos y la descarga de trabajo. Los datos se ingieren en el grupo de datos mediante consultas T-SQL o desde trabajos de Spark. Con el fin de mejorar el rendimiento en conjuntos de datos grandes, los datos ingeridos se distribuyen en particiones y se almacenan entre todas las instancias de SQL Server del grupo. Los métodos de distribución admitidos son round robin y replicados. Para la optimización del acceso de lectura, se crea un índice de almacén de columnas en clúster en cada tabla de cada instancia del grupo de datos. Un grupo de datos sirve como data mart de escalado horizontal para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].

![Data mart de escalado horizontal](media/concept-data-pool/data-virtualization-improvements.png)

El acceso a las instancias de SQL Server del grupo de datos se administra desde la instancia maestra de SQL Server. Se crea un origen de datos externo al grupo de datos, junto con las tablas externas de PolyBase para almacenar la memoria caché de datos. En segundo plano, el controlador crea una base de datos en el grupo de datos con tablas que coinciden con las tablas externas. Desde la instancia maestra de SQL Server, el flujo de trabajo es transparente: el controlador redirige las solicitudes de tablas externas específicas a las instancias de SQL Server del grupo de datos (quizás mediante el grupo de proceso), ejecuta las consultas y devuelve el conjunto de resultados. Los datos del grupo de datos solo se pueden ingerir o consultar y no se pueden modificar. Por lo tanto, cualquier actualización de los datos requeriría una eliminación de la tabla, seguida de una nueva creación de la tabla y el rellenado de datos subsiguiente.

## <a name="data-pool-scenarios"></a>Escenarios del grupo de datos

 La elaboración de informes es un escenario común del grupo de datos. Por ejemplo, se podría descargar en el grupo de datos una consulta compleja que combina varios orígenes de datos de PolyBase que se usan para un informe semanal. Los datos en caché proporcionan un proceso local rápido y eliminan la necesidad de volver a los conjuntos de datos originales. Del mismo modo, los datos de paneles que requieren una actualización periódica se pueden almacenar en caché en el grupo de datos para la elaboración de informes optimizada. La exploración repetitiva del aprendizaje automático también puede aprovechar las ventajas del almacenamiento en caché de conjuntos de datos en el grupo de datos.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
