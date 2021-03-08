---
title: Administración de la biblioteca de Spark
titleSuffix: SQL Server big data clusters
description: Administración de la biblioteca de Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 02/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: bed687cb003bfc7748aefa3c98ae5e19089f9685
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837062"
---
# <a name="spark-library-management"></a>Administración de la biblioteca de Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se proporcionan instrucciones sobre cómo importar e instalar paquetes para una sesión de Spark mediante configuraciones de sesión y de cuaderno.

## <a name="built-in-tools"></a>Herramientas integradas

Paquetes base Scala Spark (Scala 2.11) y Hadoop. 

PySpark (Python 3.7). Pandas, Sklearn, Numpy y otros paquetes de procesamiento de datos y aprendizaje automático.

Paquetes de MRO 3.5.2. Sparklyr y SparkR para cargas de trabajo de R Spark.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Instalación de paquetes desde un repositorio de Maven en el clúster de Spark en tiempo de ejecución

Los paquetes de Maven se pueden instalar en el clúster de Spark mediante la configuración de las celdas del cuaderno al inicio de la sesión de Spark. Antes de iniciar una sesión de Spark en Azure Data Studio, ejecute el siguiente código:

```python
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-at-runtime"></a>Instalación de paquetes de Python en PySpark en tiempo de ejecución

La administración de paquetes de nivel de trabajo y de sesión garantiza la coherencia y el aislamiento de la biblioteca. La configuración es una configuración de biblioteca estándar de Spark que se puede aplicar en las sesiones de Livy. __azdata spark__ admite estas configuraciones. Los ejemplos que se muestran a continuación se presentan como __cuadernos de Azure Data Studio__. Configure las celdas que deben ejecutarse después de conectarse a un clúster con el kernel de PySpark.

Si no se establece la configuración de __"spark.pyspark.virtualenv.enabled" : "true"__ , la sesión usará las bibliotecas instaladas de Python predeterminadas del clúster.

### <a name="sessionjob-configuration-with-requirementstxt"></a>Configuración de la sesión o del trabajo con requirements.txt

Especifique la ruta de acceso a un archivo requirements.txt en HDFS para usarlo como referencia para los paquetes que se van a instalar.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.7",
        "spark.pyspark.virtualenv.requirements" : "hdfs://user/project-A/requirements.txt"
    }
}
```

### <a name="sessionjob-configuration-with-different-python-versions"></a>Configuración de la sesión o del trabajo con diferentes versiones de Python

Cree un entorno virtual (virtualenv) de conda sin un archivo de requisitos y agregue paquetes dinámicamente durante la sesión de Spark.

```python
%%configure -f \
{
    "conf": {
        "spark.pyspark.virtualenv.enabled" : "true",
        "spark.pyspark.virtualenv.python_version": "3.6"
    }
}
```

### <a name="library-installation"></a>Instalación de la biblioteca

Ejecute el archivo __sc.install_packages__ para instalar las bibliotecas de forma dinámica en la sesión. Las bibliotecas se instalarán en el controlador y en todos los nodos del ejecutor.

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

También es posible instalar varias bibliotecas en el mismo comando mediante una matriz.

 ```python
sc.install_packages(["numpy==1.11.0", "xgboost"])
import numpy as np
import xgboost as xgb
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importación de archivos .jar desde HDFS para su uso en tiempo de ejecución
Importe archivos .jar en tiempo de ejecución mediante la configuración de las celdas del cuaderno de Azure Data Studio.

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importación de archivos .jar en tiempo de ejecución mediante la configuración de las celdas del cuaderno de Azure Data Studio

```python
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos de SQL Server y los escenarios relacionados, vea [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).