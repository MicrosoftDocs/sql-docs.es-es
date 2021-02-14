---
title: Ejecución de aplicaciones con azdata
titleSuffix: SQL Server Big Data Clusters
description: Ejecución de aplicaciones con azdata en clústeres de macrodatos de SQL Server 2019
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 08/16/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: b83c79398dbcc6c7d55eb0ddfe6cb1d8f0d398ed
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039595"
---
# <a name="run-apps-with-azdata---sql-server-big-data-clusters"></a>Ejecución de aplicaciones con azdata: clústeres de macrodatos de SQL Server

En este artículo se describe cómo ejecutar una aplicación dentro de un clúster de macrodatos de SQL Server.

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos de SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capacidades

En SQL Server 2019, puede crear, eliminar, describir, inicializar, enumerar, ejecutar y actualizar la aplicación. En la tabla siguiente se describen los comandos de implementación de aplicaciones que puede usar con **azdata**.

|Get-Help |Descripción |
|:---|:---|
|`azdata app describe` | Describe una aplicación. |
|`azdata app run` | Ejecuta una aplicación. |


En las siguientes secciones se describen estos comandos con más detalle.


## <a name="run-an-app"></a>Ejecutar una aplicación

Si la aplicación está en un estado `Ready` y quiere usarla, puede ejecutar los parámetros de entrada especificados. Use la sintaxis siguiente para ejecutar una aplicación:

```bash
azdata app run --name <app_name> --version <app_version> --inputs <inputs_params>
```

El siguiente comando de ejemplo muestra el comando ejecutar:

```bash
azdata app run --name add-app --version v1 --inputs x=1,y=2
```

Si la ejecución se realiza correctamente, deberá ver la salida que se especificó al crear la aplicación. La siguiente salida es un ejemplo:

```json
{
  "changedFiles": [],
  "consoleOutput": "",
  "errorMessage": "",
  "outputFiles": {},
  "outputParameters": {
    "result": 3
  },
  "success": true
}
```


## <a name="describe-an-app"></a>Describir una aplicación

El comando describe proporciona información detallada sobre la aplicación, incluido el punto de conexión del clúster. Lo suelen usar los desarrolladores de aplicaciones para compilar una aplicación con el cliente de Swagger y con el servicio web para interactuar con la aplicación de manera RESTful. Consulte [Consumo de aplicaciones en clústeres de macrodatos](app-consume.md) para más información.

```json
{
  "input_param_defs": [
    {
      "name": "x",
      "type": "int"
    },
    {
      "name": "y",
      "type": "int"
    }
  ],
  "links": {
    "app": "https://10.1.1.3:30080/api/app/add-app/v1",
    "swagger": "https://10.1.1.3:30080/api/app/add-app/v1/swagger.json"
  },
  "name": "add-app",
  "output_param_defs": [
    {
      "name": "result",
      "type": "int"
    }
  ],
  "state": "Ready",
  "version": "v1"
}
```

Averigüe cómo integrar aplicaciones implementadas en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en aplicaciones propias en [Consumo de aplicaciones en clústeres de macrodatos](app-consume.md) para obtener más información. También puede ver otros ejemplos en [Ejemplos de implementación de aplicaciones](https://aka.ms/sql-app-deploy).

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].