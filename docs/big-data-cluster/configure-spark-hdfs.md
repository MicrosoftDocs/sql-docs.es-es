---
title: Apache Spark y Apache Hadoop
titleSuffix: Configure Apache Spark and Apache Hadoop in Big Data Clusters
description: Los clústeres de macrodatos de SQL Server permiten soluciones de Spark y HDFS. Obtenga más información sobre cómo configurarlas.
author: rajmera3
ms.author: raajmera
ms.reviewer: mikeray
ms.date: 08/04/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 2fa2b5a3e9b7f996866ae1fca16c32ff1e1ba39e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039165"
---
# <a name="configure-apache-spark-and-apache-hadoop-in-big-data-clusters"></a>Configuración de Apache Spark y Apache Hadoop en clústeres de macrodatos

Para configurar Apache Spark y Apache Hadoop en clústeres de macrodatos, debe modificar el perfil de clúster en el momento de la implementación.

Un Clúster de macrodatos tiene cuatro categorías de configuración: 

- `sql` 
- `hdfs` 
- `spark` 
- `gateway` 

`sql`, `hdfs`, `spark` y `sql` son servicios. Cada servicio se asigna a la misma categoría de configuración con nombre. Todas las configuraciones de puerta de enlace se corresponden a la categoría `gateway`. 

Por ejemplo, todas las configuraciones del servicio `hdfs` pertenecen a la categoría `hdfs`. Tenga en cuenta que todas las configuraciones de Hadoop (sitio principal), HDFS y Zookeeper pertenecen a la categoría `hdfs`; todas las configuraciones de Livy, Spark, Yarn, Hive y Metastore de Hive pertenecen a la categoría `spark`. 

[Configuraciones admitidas](reference-config-spark-hadoop.md#supported-configurations) muestra propiedades de Apache Spark y Hadoop que se pueden configurar al implementar un clúster de macrodatos de SQL Server.

En las siguientes secciones se enumeran las propiedades que **no puede** modificar en un clúster:

- [Configuraciones de `spark` no admitidas](reference-config-spark-hadoop.md#unsupported-spark-configurations)
- [Configuraciones de `hdfs` no admitidas](reference-config-spark-hadoop.md#unsupported-hdfs-configurations)
- [Configuraciones de `gateway` no admitidas](reference-config-spark-hadoop.md#unsupported-gateway-configurations)


## <a name="configurations-via-cluster-profile"></a>Configuraciones mediante el perfil de clúster

En el perfil de clúster hay recursos y servicios. En tiempo de implementación, se pueden especificar configuraciones de una de estas dos maneras: 

* En primer lugar, en el nivel de recurso: 

   Los ejemplos siguientes son los archivos de revisión para el perfil: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.zookeeper.spec.settings", 
         "value": { 
           "hdfs": { 
             "zoo-cfg.syncLimit": "6" 
           } 
         } 
   }
   ```

   O: 

   ```json
   { 
         "op": "add", 
         "path": "spec.resources.gateway.spec.settings", 
         "value": { 
           "gateway": { 
             "gateway-site.gateway.httpclient.socketTimeout": "95s" 
           } 
         } 
   } 
   ```

* En segundo lugar, en el nivel de servicio. Asigne varios recursos a un servicio y especifique configuraciones para el servicio.

El siguiente es un ejemplo del archivo de revisión para el perfil para establecer el tamaño de bloque de HDFS: 

   ```json
   { 
         "op": "add", 
         "path": "spec.services.hdfs.settings", 
         "value": { 
           "hdfs-site.dfs.block.size": "268435456" 
        } 
   } 
   ```

El servicio `hdfs` se define como:

```json
{ 
  "spec": { 
   "services": { 
     "hdfs": { 
        "resources": [ 
          "nmnode-0", 
          "zookeeper", 
          "storage-0", 
          "sparkhead" 
        ], 
        "settings":{ 
          "hdfs-site.dfs.block.size": "268435456" 
        } 
      } 
    } 
  } 
} 
```
 
> [!NOTE]
> Las configuraciones de nivel de recurso invalidan las de nivel de servicio. Un recurso se puede asignar a varios servicios.

## <a name="enable-spark-in-the-storage-pool"></a>Habilitación de Spark en el bloque de almacenamiento
Además de las configuraciones de Apache admitidas, también se ofrece la posibilidad de configurar si los trabajos de Spark se pueden ejecutar o no en el grupo de almacenamiento. Este valor booleano, `includeSpark`, se encuentra en el archivo de configuración `bdc.json`, en `spec.resources.storage-0.spec.settings.spark`.

Una definición de grupo de almacenamiento de ejemplo en bdc.json puede tener el siguiente aspecto:
```json
...
"storage-0": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Storage",
                    "replicas": 2,
                    "settings": {
                        "spark": {
                            "includeSpark": "true"
                        }
                    }
                }
            }
```


## <a name="limitations"></a>Limitaciones

Las configuraciones solo se pueden especificar en el nivel de categoría. Para especificar varias configuraciones con la misma subcategoría, no se puede extraer el prefijo común en el perfil de clúster. 

```json
{ 
      "op": "add", 
      "path": "spec.services.hdfs.settings.core-site.hadoop", 
      "value": { 
        “proxyuser.xyz.users”: “*”, 
        “proxyuser.abc.users”: “*” 
     } 
} 
```

## <a name="next-steps"></a>Pasos siguientes

- [Propiedades de configuración de Apache Spark y Apache Hadoop (HDFS).](reference-config-spark-hadoop.md)
- [Referencia de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)
- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)