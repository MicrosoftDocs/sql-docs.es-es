---
title: Administración de la biblioteca de Spark
titleSuffix: SQL Server big data clusters
description: Administración de la biblioteca de Spark
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: rahul.ajmera
ms.date: 01/25/2021
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 70611d3f7d4ed6825911908942d9ed707dabbd2e
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549977"
---
# <a name="spark-library-management"></a>Administración de la biblioteca de Spark

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se proporcionan instrucciones sobre cómo importar e instalar paquetes para una sesión de Spark mediante configuraciones de sesión y de cuaderno.

## <a name="built-in-tools"></a>Herramientas integradas
Paquetes básicos para Spark y Hadoop, como Python 3.7 y Python 2.7 Pandas, Sklearn, NumPy y otros paquetes de procesamiento de datos.
Paquetes de R y MRO, como Sparklyr.

## <a name="install-packages-from-a-maven-repository-onto-the-spark-cluster-at-runtime"></a>Instalación de paquetes desde un repositorio de Maven en el clúster de Spark en tiempo de ejecución
Los paquetes de Maven se pueden instalar en el clúster de Spark mediante la configuración de las celdas del cuaderno al inicio de la sesión de Spark. Antes de iniciar una sesión de Spark en Azure Data Studio, ejecute el siguiente código:

```
%%configure -f \
{"conf": {"spark.jars.packages": "com.microsoft.azure:azure-eventhubs-spark_2.11:2.3.1"}}
```

## <a name="install-python-packages-at-pyspark-job-submission-time"></a>Instalación de paquetes de Python en el tiempo de envío de trabajos de PySpark
1. Especifique la ruta de acceso a un archivo requirements.txt en HDFS para usarlo como referencia para los paquetes que se van a instalar.
```
%%configure -f \
{"conf": {
    "spark.pyspark.virtualenv.enabled" : "true",
    "spark.pyspark.virtualenv.type" : "conda",
    "spark.pyspark.virtualenv.requirements" : "requirements.txt",
    "spark.pyspark.virtualenv.bin.path" : "/opt/mls/python/bin/conda"
    }, 
"files": ["hdfs://nmnode-0/tmp/requirements.txt"]
}
```
2. Cree un entorno virtual (virtualenv) de conda sin un archivo de requisitos y agregue paquetes dinámicamente durante la sesión de Spark.
```
%%configure -f \
{"conf": {
    'spark.pyspark.virtualenv.enabled' : 'true',
    'spark.pyspark.virtualenv.type' : 'conda',
    'spark.pyspark.virtualenv.bin.path' : '/opt/mls/python/bin/conda',
    'spark.pyspark.virtualenv.python_version': '3.6'
 }
 ```

 ```python
sc.install_packages("numpy==1.11.0")
import numpy as np
```

## <a name="import-jar-from-hdfs-for-use-at-runtime"></a>Importación de archivos .jar desde HDFS para su uso en tiempo de ejecución
Importe archivos .jar en tiempo de ejecución mediante la configuración de las celdas del cuaderno de Azure Data Studio.

```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

### <a name="import-jar-at-runtime-through-azure-data-studio-notebook-cell-configuration"></a>Importación de archivos .jar en tiempo de ejecución mediante la configuración de las celdas del cuaderno de Azure Data Studio
```
%%configure -f
{"conf": {"spark.jars": "/jar/mycodeJar.jar"}}
```

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre los clústeres de macrodatos de SQL Server y los escenarios relacionados, vea [[!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](big-data-cluster-overview.md).