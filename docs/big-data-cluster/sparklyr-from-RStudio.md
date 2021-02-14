---
title: Uso de sparklyr desde RStudio
titleSuffix: SQL Server big data clusters
description: Obtenga información sobre cómo usar sparklyr en un clúster de macrodatos de SQL Server para conectarse a Spark a través de la interfaz de R.
author: jejiang
ms.author: jejiang
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: 8a15336a22f564dd22870de8843732cbb583d2f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043549"
---
# <a name="use-sparklyr-in-sql-server-big-data-cluster"></a>Uso de sparklyr en clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Sparklyr proporciona una interfaz de R para Apache Spark. Sparklyr es una forma popular para que los desarrolladores de R usen Spark. En este artículo se describe cómo usar sparklyr en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] mediante RStudio.

## <a name="prerequisites"></a>Prerequisites

- [Implementación de un clúster de macrodatos de SQL Server 2019](quickstart-big-data-cluster-deploy.md).

### <a name="install-rstudio-desktop"></a>Instalación de RStudio Desktop

Instale y configure **RStudio Desktop** con los pasos siguientes:

1. Si ejecuta un cliente Windows, [descargue e instale R 3.4.4](https://cran.rstudio.com/bin/windows/base/old/3.4.4).

1. [Descargue e instale RStudio Desktop](https://www.rstudio.com/products/rstudio/download/).

1. Una vez finalizada la instalación, ejecute los comandos siguientes dentro de RStudio Desktop para instalar los paquetes necesarios:

   ```RStudioDesktop
   install.packages("DBI", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("dplyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   install.packages("sparklyr", repos = "https://cran.microsoft.com/snapshot/2019-01-01")
   ```

## <a name="connect-to-spark-in-a-big-data-cluster"></a>Conexión a Spark en un clúster de macrodatos

Puede usar sparklyr para conectarse desde un cliente al clúster de macrodatos mediante Livy y la puerta de enlace de HDFS/Spark. 

En RStudio, cree un script de R y conéctese a Spark como en el ejemplo siguiente:

> [!TIP]
> Para los valores `<AZDATA_USERNAME>` y `<AZDATA_PASSWORD>`, use el nombre de usuario y la contraseña que haya establecido durante la implementación del clúster de macrodatos.

[!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

Para los valores `<IP>` y `<PORT>`, vea la documentación sobre [cómo conectarse a un clúster de macrodatos](connect-to-big-data-cluster.md).

```r
library(sparklyr)
library(dplyr)
library(DBI)

#Specify the Knox username and password
config <- livy_config(user = "<AZDATA_USERNAME>", password = "<AZDATA_PASSWORD>")

httr::set_config(httr::config(ssl_verifypeer = 0L, ssl_verifyhost = 0L))

sc <- spark_connect(master = "https://<IP>:<PORT>/gateway/default/livy/v1",
                    method = "livy",
                    config = config)
```

## <a name="run-sparklyr-queries"></a>Ejecución de consultas de sparklyr

Después de conectarse a Spark, puede ejecutar sparklyr. En el ejemplo siguiente se realiza una consulta en el conjunto de datos de iris mediante sparklyr:

```r
iris_tbl <- copy_to(sc, iris)

iris_count <- dbGetQuery(sc, "SELECT COUNT(*) FROM iris")

iris_count
```

## <a name="distributed-r-computations"></a>Cálculos de R distribuidos

Una característica de sparklyr es la capacidad de [distribuir cálculos de R](https://spark.rstudio.com/guides/distributed-r/) con [spark_apply](https://spark.rstudio.com/guides/distributed-r/#apply-an-r-function-to-a-spark-object).

Como los clústeres de macrodatos usan conexiones Livy, debe establecer `packages = FALSE` en la llamada a **spark_apply**. Para obtener más información, vea la [sección sobre Livy](https://spark.rstudio.com/guides/distributed-r/#livy) de la documentación de sparklyr sobre cálculos de R distribuidos. Con esta configuración, solo puede usar los paquetes de R que ya estén instalados en el clúster de Spark en el código de R que se pasa a **spark_apply**. En el ejemplo siguiente se muestra esta funcionalidad:

```r
iris_tbl %>% spark_apply(function(e) nrow(e), names = "nrow", group_by = "Species", packages = FALSE)
```

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.
