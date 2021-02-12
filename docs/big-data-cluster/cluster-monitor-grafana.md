---
title: Supervisión de un clúster con el panel de Grafana
titleSuffix: SQL Server Big Data Clusters
description: Supervisión del clúster con el panel de Grafana en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 09/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6b9e97e90e844391f62d920b64f7a09de5b6e7ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047735"
---
# <a name="monitor-cluster-with-azdata-and-grafana-dashboard"></a>Supervisión de aplicaciones con azdata y el panel de Grafana

En este artículo se describe cómo supervisar una aplicación dentro de un clúster de macrodatos de SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos de SQL Server 2019](deployment-guidance.md)
- [Utilidad de línea de comandos azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capacidades

En SQL Server 2019, puede crear, eliminar, describir, inicializar, enumerar, ejecutar y actualizar la aplicación. En la tabla siguiente se describen los comandos de implementación de aplicaciones que puede usar con **azdata**.

|Get-Help |Descripción |
|:---|:---|
|`azdata bdc endpoint list` | Enumera los puntos de conexión para el clúster de macrodatos. |


Puede usar el ejemplo siguiente para enumerar el punto de conexión del **panel de Grafana**:

```bash
azdata bdc endpoint list --endpoint-name metricsui 
```

La salida le proporcionará el punto de conexión, que puede usar el nombre de usuario del clúster y la contraseña para iniciar sesión. 

![Panel de Grafana](media/big-data-cluster-monitor-apps/grafana-dashboard-endpoint.png)

Los valores `nodeMetricsUrl` y `sqlMetricsUrl` vinculan a un panel de Grafana para supervisar las métricas de nodo de Kubernetes y las métricas de servicio del clúster de macrodatos:

![Panel de Grafana](./media/view-cluster-status/grafana-dashboard.png)

![SQL](./media/view-cluster-status/grafana-sql-status.png)



## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].