---
title: Ejecución de un cuaderno de ejemplo con Spark
titleSuffix: SQL Server big data clusters
description: En este tutorial, se muestra cómo cargar y ejecutar un cuaderno de Spark de ejemplo en un clúster de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 03/30/2020
ms.topic: tutorial
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 6996e2b9b7705511fc13d479e39e9436a5e054f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044118"
---
# <a name="run-a-sample-notebook-using-spark"></a>Ejecución de un cuaderno de ejemplo con Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este tutorial, se muestra cómo cargar y ejecutar un cuaderno en Azure Data Studio en un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Esto permite a los científicos de datos e ingenieros de datos ejecutar código de Python, S o Scala en el clúster.

> [!TIP]
> Si lo prefiere, puede descargar y ejecutar un script con los comandos de este tutorial. Para obtener instrucciones, vea los [ejemplos de Spark](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark) en GitHub.

## <a name="prerequisites"></a><a id="prereqs"></a> Requisitos previos

- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **kubectl**
   - **Azure Data Studio**
   - **Extensión de SQL Server 2019**
- [Cargar datos de ejemplo en un clúster de macrodatos](tutorial-load-sample-data.md)

## <a name="download-the-sample-notebook-file"></a>Descargar el archivo de cuaderno de ejemplo

Siga estas instrucciones para cargar el archivo de cuaderno de ejemplo **spark-sql.ipynb** en Azure Data Studio.

1. Abra un símbolo del sistema de Bash (Linux) o Windows PowerShell.

1. Vaya al directorio donde quiera descargar el archivo del cuaderno de ejemplo.

1. Ejecute el siguiente comando de **curl** para descargar el archivo del cuaderno desde GitHub:

   ```bash
   curl https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/data-loading/transform-csv-files.ipynb -o transform-csv-files.ipynb
   ```

## <a name="open-the-notebook"></a>Apertura del cuaderno

En los pasos siguientes, se muestra cómo abrir el archivo del cuaderno en Azure Data Studio:

1. En Azure Data Studio, conéctese a la instancia maestra del clúster de macrodatos. Para obtener más información, vea [Conexión a un clúster de macrodatos](connect-to-big-data-cluster.md).

1. Haga doble clic en la conexión de la puerta de enlace de HDFS/Spark de la ventana **Servidores**. Después, seleccione **Abrir cuaderno**.

   ![Abrir el cuaderno](media/notebook-tutorial-spark/azure-data-studio-open-notebook.png)

1. Espere hasta que se rellenen el **Kernel** y el contexto del destino (**Conectar a**). Establezca el **Kernel** en **PySpark3** y el valor de **Conectar a** en la dirección IP del punto de conexión del clúster de macrodatos.

   ![Establecer el Kernel y Conectar a](media/notebook-tutorial-spark/set-kernel-and-attach-to.png)

## <a name="run-the-notebook-cells"></a>Ejecutar las celdas del cuaderno

Para ejecutar cada celda del cuaderno, pulse el botón Reproducir a la izquierda de la celda. Cuando la celda termine de ejecutarse, los resultados se mostrarán en el cuaderno.

![Ejecutar la celda del cuaderno](media/notebook-tutorial-spark/run-notebook-cell.png)

Ejecute todas las celdas del cuaderno de ejemplo en sucesión. Para obtener más información sobre el uso de cuadernos con [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los siguientes recursos:

- [Procedimiento para usar cuadernos](../azure-data-studio/notebooks/notebooks-guidance.md)
- [Cómo se administran los cuadernos en Azure Data Studio](notebooks-manage-bdc.md)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre los cuadernos:
> [!div class="nextstepaction"]
> [Procedimiento para usar cuadernos](../azure-data-studio/notebooks/notebooks-guidance.md)