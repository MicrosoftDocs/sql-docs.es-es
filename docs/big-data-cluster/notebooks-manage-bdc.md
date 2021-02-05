---
title: 'Administrar: Azure Data Studio Notebooks'
titleSuffix: SQL Server Big Data Clusters
description: Use un cuaderno de Azure Data Studio para administrar Clústeres de macrodatos de SQL Server y solucionar sus problemas.
author: yualan
ms.author: alanyu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 03/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a526f4d1da472a8d5bae9c5bceb296978d24986b
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99235704"
---
# <a name="manage-sql-server-big-data-clusters-with-azure-data-studio-notebooks"></a>Administración de clústeres de macrodatos de SQL Server con cuadernos de Azure Data Studio

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] proporciona una extensión para Azure Data Studio que incluye cuadernos. Un cuaderno proporciona documentación y código que se puede usar en Azure Data Studio para administrar clústeres de macrodatos de SQL Server 2019.

Los [cuadernos](../azure-data-studio/notebooks/notebooks-guidance.md), que originalmente se implementaban como un proyecto de código abierto, se han incorporado a [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md). Puede usar Markdown para el texto de las celdas de texto y uno de los kernels disponibles para escribir código en las celdas de código.

Puede usar cuadernos para implementar clústeres de macrodatos para [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

Además de los cuadernos, puede ver una colección de cuadernos, que se denomina "libro de Jupyter". Un libro de Jupyter proporciona una tabla de contenido que le permite examinar una colección de cuadernos para que pueda encontrar fácilmente el cuaderno que necesita, tanto para solucionar problemas de SQL Server como para ver el estado del clúster.

## <a name="prerequisites"></a>Prerrequisitos

Deberá cumplir estos requisitos previos para abrir un cuaderno:

* La versión más reciente de [Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
* La extensión [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] instalada en Azure Data Studio

Además de estos requisitos previos, para implementar clústeres de macrodatos de SQL Server 2019, también necesitará:

* [azdata](../azdata/install/deploy-install-azdata.md)
* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
* [CLI de Azure](/cli/azure/install-azure-cli)

## <a name="access-troubleshooting-notebooks"></a>Acceso a los cuadernos de solución de problemas

Hay tres formas de acceder a los cuadernos de solución de problemas.

### <a name="command-palette"></a>Paleta de comandos

1. Seleccione **Ver** > **Paleta de comandos**.

2. Escriba **Libros de Jupyter: Guía de SQL Server 2019**.

Se abrirá el viewlet Libros de Jupyter con el libro de Jupyter que contiene los cuadernos de solución de problemas relacionados con los clústeres de macrodatos de SQL Server.

### <a name="sql-master-dashboard"></a>Panel maestro de SQL

1. Después de instalar Azure Data Studio Insiders, conéctese a una instancia de clúster de macrodatos de SQL Server.

2. Una vez que se haya conectado a la instancia, haga clic con el botón derecho en el nombre del servidor en **CONEXIONES** y seleccione **Administrar**.

3. En el panel, seleccione **Clúster de macrodatos de SQL Server**. Seleccione **Guía de SQL Server 2019** para abrir el libro de Jupyter que contiene los cuadernos que necesita.
    ![Cuadernos de Jupyter en el panel](media/manage-notebooks/jupyter-book-button.png)

4. Seleccione el cuaderno de la tarea que debe completar.

### <a name="controller-dashboard"></a>Panel del controlador

1. En la vista **Conexiones**, expanda **Clúster de macrodatos de SQL Server**.

2. Agregue los detalles del punto de conexión del controlador.

3. Una vez que se haya conectado al controlador, haga clic con el botón derecho en el punto de conexión y seleccione **Administrar**.

4. Una vez que se haya cargado el panel, seleccione **Solucionar problemas** para abrir las guías de solución de problemas de los libros de Jupyter.

## <a name="use-troubleshooting-notebooks"></a>Uso de los cuadernos de solución de problemas

1. Busque la guía de solución de problemas que necesite en la tabla de contenido del libro de Jupyter.

2. Los cuadernos están optimizados, por lo que solo tiene que seleccionar **Ejecutar celdas**. Esta acción ejecutará cada celda del cuaderno de forma individual hasta que se haya completado el cuaderno.

3. Si se encuentra un error, el libro de Jupyter sugerirá un cuaderno que puede ejecutar para corregir el error. Siga los pasos recomendados y, a continuación, vuelva a ejecutar el cuaderno.

## <a name="change-the-big-data-cluster"></a>Cambio del clúster de macrodatos

Para cambiar el clúster de macrodatos de SQL Server de un cuaderno:

1. Haga clic en el menú **Asociar a** de la barra de herramientas del cuaderno.

   ![Clic en el menú Asociar a de la barra de herramientas del cuaderno](./media/notebooks-how-to-manage/select-attach-to-1.png)

2. Haga clic en un servidor en el menú **Asociar a**.

   ![Selección de un servidor desde el menú Asociar a](./media/notebooks-how-to-manage/select-attach-to-2.png)

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los cuadernos en Azure Data Studio, vea [Procedimiento para usar cuadernos con SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).