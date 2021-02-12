---
title: Administración de clústeres de macrodatos de SQL Server con el panel del controlador
titleSuffix: Manage SQL Server big data cluster with controller dashboard
description: Use un cuaderno de Azure Data Studio para administrar un clúster de macrodatos y solucionar problemas de este.
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: e7b71318f55b15df6a758237c26eedb9be5646c4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046315"
---
# <a name="manage-big-data-clusters-for-sql-server-controller-dashboard"></a>Administración de clústeres de macrodatos en el panel del controlador de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Además de **azdata** y el cuaderno de estado del clúster, hay otra manera de ver el estado de un clúster de macrodatos de SQL Server. Ahora puede agregar un controlador de clúster de macrodatos de SQL Server mediante el viewlet **Conexiones**. Esto le permite disponer de un panel para ver el estado del clúster.

![dashboard](media/manage-with-controller-dashboard/controller-dashboard.png)
## <a name="prerequisites"></a>Prerequisites

Los siguientes requisitos previos son indispensables para iniciar el cuaderno:

* Última versión de [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
* [Extensión [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] instalada en Azure Data Studio](../azure-data-studio/data-virtualization-extension.md)

Además de lo anterior, el clúster de macrodatos de SQL Server 2019 también requiere:

* **azdata**
    - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
    - [Administrador de paquetes de Linux](../azdata/install/deploy-install-azdata-linux-package.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [CLI de Azure](/cli/azure/install-azure-cli)

## <a name="add-sql-server-big-data-cluster-controller"></a>Agregar un controlador de clúster de macrodatos de SQL Server

1. Haga clic en la vista Conexiones del panel izquierdo.
2. En la parte inferior de la vista Conexiones, haga clic en **Clústeres de macrodatos de SQL Server** para expandir.
3. Haga clic en **Add SQL Server big data cluster controller** (Agregar controlador de clúster de macrodatos de SQL Server), con lo que se abre un nuevo cuadro de diálogo.

## <a name="add-new-controller"></a>Agregar nuevo controlador

1. Agregue el nombre del clúster.
2. Agregue la dirección URL del Servicio de administración de clústeres. La puede encontrar en la tabla Puntos de conexión de servicio del panel de BDC o puede preguntar al administrador de clústeres.
3. Agregue el nombre de usuario y la contraseña.

## <a name="launch-controller-dashboard"></a>Iniciar el panel del controlador

1. Una vez que haya agregado correctamente el controlador, puede verlo en Clústeres de macrodatos de SQL Server.
2. Para iniciar el panel, haga clic con el botón derecho en el controlador y haga clic en **Administrar**.

## <a name="controller-dashboard"></a>Panel del controlador

1. En el panel del controlador hay tres componentes principales:

    - **Barra de herramientas** en la parte superior que contiene las acciones del panel.
    - **Panel de navegación** a la izquierda que cambia a las distintas vistas del panel.
    - **Vista** que cubre la mayor parte de la página.

2. En la página **Big data cluster overview** (Información general de clúster de macrodatos) puede ver:

    - **Propiedades de clúster** para ver información sobre el clúster.
    - **Cluster Overview** (Información general del clúster) para ver información general de todos los componentes del clúster y cuáles no tienen un estado correcto.
    - **Puntos de conexión de servicio** para copiar o acceder a diferentes servicios del clúster de macrodatos de SQL Server.

3. En el **panel de navegación** puede ver la lista de servicios y hacer clic en uno para ver detalles adicionales del clúster. Se trata de las mismas vistas que al hacer clic en un servicio en **Cluster Overview** (Información general del clúster).

4. Cada vista de **Detalles del clúster** consta de los mismos componentes de la interfaz de usuario:

    - **Detalles del estado de mantenimiento**, que comparte el estado y el estado de mantenimiento del componente.
    - **Métricas y registros**, para ver métricas y registros adicionales a través de Grafana y Kibana.

1. Si ve un componente con un estado incorrecto, haga clic en **Solucionar problemas** en la barra de herramientas para iniciar una instancia de Jupyter Notebook que contiene un cuaderno para ayudar a diagnosticar el problema.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre el controlador, vea la [documentación del controlador](concept-controller.md).