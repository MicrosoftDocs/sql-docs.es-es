---
title: Supervisión de aplicaciones con el panel de azdata y Grafana
titleSuffix: SQL Server Big Data Clusters
description: Supervisión de aplicaciones con azdata y Grafana en el clúster de macrodatos de SQL Server 2019
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 380a6c8ab8812ee6a8aae132cbc12125777e2960
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047795"
---
# <a name="monitor-applications-with-azdata-and-grafana-dashboard"></a>Supervisión de aplicaciones con el panel de azdata y Grafana

Grafana es una de las mejores herramientas de virtualización nativa en la nube, que se puede usar para proporcionar varias métricas de supervisión de la aplicación que se ejecuta en Kubernetes.  

En este artículo se describe cómo supervisar una aplicación dentro de un clúster de macrodatos de SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos de SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

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


Al abrir el panel, vaya a las  **métricas de aplicaciones de host**, donde obtendrá más información sobre la aplicación y mantendrá en seguimiento.  

![Métricas de aplicaciones host](media/big-data-cluster-monitor-apps/host-apps-metrics.png)


Para más información sobre un único pod de la aplicación (en algunos casos, tiene varias copias de la aplicación), vaya a las  **métricas de los pods de host** y elija el pod respecto al cual obtendrá una vista de las métricas, tal como se indica a continuación:  

![Métricas de los pods de host](media/big-data-cluster-monitor-apps/host-pods-metrics.png) 


## <a name="next-steps"></a>Pasos siguientes

Puede ver otros ejemplos en [Ejemplos de implementación de aplicaciones](https://aka.ms/sql-app-deploy).

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].