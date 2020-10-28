---
title: Consulta de los registros de clúster con el panel de Kibana
titleSuffix: SQL Server Big Data Clusters
description: Supervisión del clúster con el panel de Kibana en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/01/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 66b405f020728c0ed7040a712d56bcadc3180e38
ms.sourcegitcommit: 29a2be59c56f8a4b630af47760ef38d2bf56a3eb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/22/2020
ms.locfileid: "92378477"
---
# <a name="check-out-cluster-logs--with-kibana-dashboard"></a>Consulta de los registros de clúster con el panel de Kibana

En este artículo se describe cómo supervisar una aplicación dentro de un clúster de macrodatos de SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos de SQL Server 2019](deployment-guidance.md)
- [Utilidad de línea de comandos azdata](deploy-install-azdata.md)

## <a name="capabilities"></a>Capacidades

En SQL Server 2019, puede crear, eliminar, describir, inicializar, enumerar, ejecutar y actualizar la aplicación. En la tabla siguiente se describen los comandos de implementación de aplicaciones que puede usar con **azdata** .

|Get-Help |Descripción |
|:---|:---|
|`azdata bdc endpoint list` | Enumera los puntos de conexión para el clúster de macrodatos. |


Puede usar el ejemplo siguiente para enumerar el punto de conexión del **panel de Kibana** :

```bash
azdata bdc endpoint list --endpoint-name logsui 
```

La salida le proporcionará el punto de conexión, que puede usar el nombre de usuario del clúster y la contraseña para iniciar sesión. 

![Panel de Kibana](media/big-data-cluster-monitor-cluster/kibana-dashboard-endpoint.png)


Vínculo a un panel de Kibana:

![Panel de Kibana](./media/view-cluster-status/kibana-dashboard.png)

> [!NOTE]
> El explorador Microsoft Edge (antiguo) no es compatible con Kibana; deberá usar el explorador basado en Chromium para que el panel se muestre correctamente. Si se usa un explorador no compatible, aparecerá una página en blanco al cargar los paneles. Consulte aquí qué exploradores son compatibles con Kibana.

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].
