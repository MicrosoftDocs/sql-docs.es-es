---
title: Consulta de los registros de clúster con el panel de Kibana
titleSuffix: SQL Server Big Data Clusters
description: Supervisión del clúster con el panel de Kibana en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 02/25/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7c26dc7e193d3dd5ad688af4d4dc79e2dd3eeea7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835972"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Consulta de los registros de clúster con el panel de Kibana

En este artículo se describe cómo supervisar una aplicación dentro de [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)].

## <a name="prerequisites"></a>Prerrequisitos

- [[!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)
- [Utilidad de línea de comandos azdata](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capacidades

En [!INCLUDE[sssql19-md](../includes/sssql19-md.md)], puede crear, eliminar, describir, inicializar, enumerar, ejecutar y actualizar la aplicación. En la tabla siguiente se describen los comandos de implementación de aplicaciones que puede usar con **azdata**.

|Get-Help |Descripción |
|:---|:---|
|`azdata bdc endpoint list` | Enumera los puntos de conexión de [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)]. |


Puede usar el ejemplo siguiente para enumerar el punto de conexión del **panel de Kibana**:

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

La salida le proporcionará el punto de conexión, que puede usar el nombre de usuario del clúster y la contraseña para iniciar sesión. 

![Panel de Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Vínculo a un panel de Kibana:

![Panel de Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> El explorador Microsoft Edge más antiguo no es compatible con Kibana; deberá usar el explorador basado en Edge Chromium para que el panel se muestre correctamente. Si se usa un explorador no compatible, aparecerá una página en blanco al cargar los paneles. Consulte los [exploradores admitidos para Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].


