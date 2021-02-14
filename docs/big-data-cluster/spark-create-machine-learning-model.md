---
title: 'Creación y exportación de modelos de aprendizaje automático de Spark: MLeap'
titleSuffix: SQL Server Big Data Clusters
description: Use PySpark para entrenar y crear modelos de aprendizaje automático con Spark en clústeres de macrodatos de SQL Server. Expórtelo con MLeap y puntúe el modelo con Java en SQL Server.
author: RogPodge
ms.author: roliu
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 4b403644de65f866d4bafb191356fd08212c706f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043605"
---
# <a name="create-export-and-score-spark-machine-learning-models-on-big-data-clusters-2019"></a>Creación, exportación y puntuación de modelos de aprendizaje automático de Spark en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

En el siguiente ejemplo se muestra cómo crear un modelo con [Spark ML](https://spark.apache.org/docs/latest/ml-guide.html), exportarlo a [MLeap](http://mleap-docs.combust.ml/) y puntuarlo en SQL Server con su [extensión de lenguaje Java](../language-extensions/language-extensions-overview.md). Esto se hace en el contexto de un clúster de macrodatos de SQL Server 2019.

En el siguiente diagrama se muestra el trabajo que se lleva a cabo en este ejemplo:

![Entrenamiento de la exportación de la puntuación con Spark](./media/spark-create-machine-learning-model/train-score-export-with-spark.png)

## <a name="prerequisites"></a>Prerrequisitos

Todos los archivos de este ejemplo se encuentran en [https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/spark/sparkml).

Para ejecutar el ejemplo, también debe cumplir los siguientes requisitos previos:

- Un [clúster de macrodatos de SQL Server](deploy-get-started.md)

- [Herramientas de macrodatos](deploy-big-data-tools.md)
   - **kubectl**
   - **curl**
   - **Azure Data Studio**

## <a name="model-training-with-spark-ml"></a>Entrenamiento de modelos con Spark ML

En este ejemplo, los datos de censo (**AdultCensusIncome.csv**) sirven para crear un modelo de canalización de Spark ML.

1. Use el archivo [mleap_sql_test/Setup.sh](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/setup.sh) para descargar el conjunto de datos de Internet y colocarlo en HDFS en el clúster de macrodatos de SQL Server. Esto permite que Spark tenga acceso a él.

1. Luego, descargue el cuaderno de ejemplo [train_score_export_ml_models_with_spark.ipynb](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb). Desde una línea de comandos de PowerShell o Bash, ejecute el siguiente comando para descargar el cuaderno:

   ```PowerShell
   curl -o mssql_spark_connector.ipynb "https://raw.githubusercontent.com/microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/spark/sparkml/train_score_export_ml_models_with_spark.ipynb"
   ```

   Este cuaderno contiene celdas con los comandos necesarios para esta sección del ejemplo.

1. Abra el cuaderno en Azure Data Studio y ejecute cada bloque de código. Para obtener más información sobre el trabajo con cuadernos, vea [Procedimiento para usar cuadernos con SQL Server](../azure-data-studio/notebooks/notebooks-guidance.md).

Los datos se leen primero en Spark y se dividen en conjuntos de datos de entrenamiento y de prueba. Después, el código entrena un modelo de canalización con los datos de entrenamiento. Por último, exporta el modelo a un conjunto de MLeap.

> [!TIP]
> También puede revisar o ejecutar el código de Python asociado a estos pasos fuera del cuaderno en el archivo [mleap_sql_test/mleap_pyspark.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_pyspark.py).

## <a name="model-scoring-with-sql-server"></a>Puntuación del modelo con SQL Server

Ahora que el modelo de canalización de Spark ML se encuentra en un formato de serialización común de [agrupación de MLeap](http://mleap-docs.combust.ml/core-concepts/mleap-bundles.html), puede puntuarlo en Java sin la presencia de Spark.

En este ejemplo se usa la [extensión del lenguaje Java](../language-extensions/language-extensions-overview.md) en SQL Server. Para poder puntuar el modelo en SQL Server, primero debe compilar una aplicación Java que pueda cargar el modelo en Java y puntuarlo. Encontrará el código de ejemplo para esta aplicación Java en la [carpeta mssql-mleap-app](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mssql-mleap-app).

Después de compilar el ejemplo, puede usar Transact-SQL para llamar a la aplicación Java y puntuar el modelo con una tabla de base de datos. Puede verse en el archivo de código fuente [mleap_sql_test/mleap_sql_tests.py](https://github.com/microsoft/sql-server-samples/blob/master/samples/features/sql-big-data-cluster/spark/sparkml/mleap_sql_test/mleap_sql_tests.py).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md).