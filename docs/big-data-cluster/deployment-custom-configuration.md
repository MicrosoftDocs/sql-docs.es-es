---
title: Configuración de implementaciones
titleSuffix: SQL Server big data clusters
description: Aprenda a personalizar una implementación de clúster de macrodatos con los archivos de configuración integrados en la herramienta de administración de azdata.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d983b4d0d7cfb02a587675984fdc42c54bf9f0ec
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047205"
---
# <a name="configure-deployment-settings-for-cluster-resources-and-services"></a>Configuración de opciones de implementación de recursos y servicios de clúster

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Partiendo de un conjunto predefinido de perfiles de configuración integrados en la herramienta de administración [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], la configuración predeterminada se puede modificar fácilmente para satisfacer de mejor forma los requisitos de carga de trabajo de BDC. La estructura de los archivos de configuración permite actualizar de forma granular la configuración de cada servicio del recurso.

Vea este vídeo de 13 minutos para información general sobre la configuración de los clústeres de macrodatos:

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Big-Data-Cluster-Configuration/player?WT.mc_id=dataexposed-c9-niner]

> [!TIP]
> Vea los artículos sobre cómo configurar la **alta disponibilidad** de componentes esenciales como la [instancia maestra de SQL Server](deployment-high-availability.md) o el [nodo de nombre de HDFS](deployment-high-availability-hdfs-spark.md) para más información sobre cómo implementar servicios de alta disponibilidad.

También puede definir configuraciones de nivel de recurso o actualizar las configuraciones de todos los servicios de un recurso. Este es un resumen de la estructura de `bdc.json`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "BigDataCluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "resources": {
            "nmnode-0": {...
            },
            "sparkhead": {...
            },
            "zookeeper": {...
            },
            "gateway": {...
            },
            "appproxy": {...
            },
            "master": {...
            },
            "compute-0": {...
            },
            "data-0": {...
            },
            "storage-0": {...
        },
        "services": {
            "sql": {
                "resources": [
                    "master",
                    "compute-0",
                    "data-0",
                    "storage-0"
                ]
            },
            "hdfs": {
                "resources": [
                    "nmnode-0",
                    "zookeeper",
                    "storage-0",
                    "sparkhead"
                ],
                "settings": {...
            },
            "spark": {
                "resources": [
                    "sparkhead",
                    "storage-0"
                ],
                "settings": {...
            }
        }
    }
}
```

Para actualizar las configuraciones de nivel de recurso (como las instancias de un grupo), hay que actualizar la especificación del recurso. Por ejemplo, para actualizar el número de instancias en el grupo de proceso, habrá que modificar esta sección en el archivo de configuración `bdc.json`:

```json
"resources": {
    ...
    "compute-0": {
        "metadata": {
            "kind": "Pool",
            "name": "default"
        },
        "spec": {
            "type": "Compute",
            "replicas": 4
        }
    }
    ...
}
``` 

Y lo mismo para cambiar la configuración de un único servicio dentro de un recurso específico. Por ejemplo, si quiere cambiar la configuración de memoria de Spark solo para el componente Spark del grupo de almacenamiento, habrá que actualizar el recurso `storage-0` con una sección `settings` para el servicio `spark` en el archivo de configuración `bdc.json`.

```json
"resources":{
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
                    "spark-defaults-conf.spark.driver.memory": "2g",
                    "spark-defaults-conf.spark.driver.cores": "1",
                    "spark-defaults-conf.spark.executor.instances": "3",
                    "spark-defaults-conf.spark.executor.memory": "1536m",
                    "spark-defaults-conf.spark.executor.cores": "1",
                    "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                    "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                    "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                    "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                    "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
                }
            }
        }
    }
    ...
}
```

Si quiere aplicar la misma configuración a un servicio asociado a varios recursos, deberá actualizar el elemento `settings` correspondiente en la sección `services`. Por ejemplo, si quiere definir la misma configuración para Spark tanto en el grupo de almacenamiento como en los grupos de Spark, habrá que actualizar la sección `settings` de la sección servicio `spark` en el archivo de configuración `bdc.json`.

```json
"services": {
    ...
    "spark": {
        "resources": [
            "sparkhead",
            "storage-0"
        ],
        "settings": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
        }
    }
    ...
}
```

Para personalizar los archivos de configuración de implementación de clústeres, puede usar cualquier editor de formato JSON, como VSCode. Para crear scripts de estas ediciones para automatizarlas, use el comando `azdata bdc config`. En este artículo se explica cómo configurar las implementaciones de clústeres de macrodatos mediante la modificación de los archivos de configuración de implementación. Se proporcionan ejemplos sobre cómo cambiar la configuración de distintos escenarios. Para obtener más información sobre cómo se usan los archivos de configuración en las implementaciones, vea la [guía de implementación](deployment-guidance.md#configfile).

## <a name="prerequisites"></a>Prerrequisitos

- [Instalación de azdata](../azdata/install/deploy-install-azdata.md).

- En cada uno de los ejemplos de esta sección se supone que ha creado una copia de una de las configuraciones estándar. Para obtener más información, vea [Creación de una configuración personalizada](deployment-guidance.md#customconfig). Por ejemplo, el siguiente comando crea un directorio denominado `custom-bdc` que contiene dos archivos de configuración de implementación JSON, `bdc.json` y `control.json`, según la configuración predeterminada de `aks-dev-test`:

   ```bash
   azdata bdc config init --source aks-dev-test --target custom-bdc
   ```

## <a name="change-default-docker-registry-repository-and-images-tag"></a><a id="docker"></a> Cambio de la etiqueta predeterminada de registro, repositorio e imágenes de Docker

Los archivos de configuración integrados (en concreto, control.json) incluyen una sección `docker` donde la etiqueta de registro de contenedor, repositorio e imágenes se rellena previamente. Las imágenes necesarias para clústeres de macrodatos se encuentran de forma predeterminada en Microsoft Container Registry (`mcr.microsoft.com`) en el repositorio `mssql/bdc`:

```json
{
    "apiVersion": "v1",
    "metadata": {
        "kind": "Cluster",
        "name": "mssql-cluster"
    },
    "spec": {
        "docker": {
            "registry": "mcr.microsoft.com",
            "repository": "mssql/bdc",
            "imageTag": "2019-GDR1-ubuntu-16.04",
            "imagePullPolicy": "Always"
        },
        ...
    }
}
```

Antes de la implementación, puede personalizar la configuración de `docker`, ya sea editando directamente el archivo de configuración `control.json` o usando comandos `azdata bdc config`. Por ejemplo, los siguientes comandos actualizan el archivo de configuración control.json de un elemento `custom-bdc` con unas etiquetas `<registry>`, `<repository>` e `<image_tag>` distintas:

```bash
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.registry=<registry>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.repository=<repository>"
azdata bdc config replace -c custom-bdc/control.json -j "$.spec.docker.imageTag=<image_tag>"
```

> [!TIP]
> Como procedimiento recomendado, debe usar una etiqueta de imagen específica de la versión y no usar la etiqueta de imagen `latest`, ya que esto podría provocar problemas de estado del clúster.

> [!TIP]
> La implementación de clústeres de macrodatos debe tener acceso a un registro de contenedor y a un repositorio desde el que extraer imágenes de contenedor. Si el entorno carece de acceso a la instancia de Microsoft Container Registry predeterminada, puede llevar a cabo una instalación sin conexión en la que las imágenes necesarias se coloquen en primer lugar en un repositorio de Docker privado. Para más información sobre las instalaciones sin conexión, vea [Realización de una implementación sin conexión de un clúster de macrodatos de SQL Server](deploy-offline.md). Cabe decir que, antes de emitir la implementación, hay que establecer las [variables de entorno](deployment-guidance.md#env) `DOCKER_USERNAME` y `DOCKER_PASSWORD` ya que así se asegurará de que el flujo de trabajo de implementación tiene acceso al repositorio privado del que extraer las imágenes.

## <a name="change-cluster-name"></a><a id="clustername"></a> Cambio del nombre del clúster

El nombre del clúster es tanto el nombre del clúster de macrodatos como el espacio de nombres de Kubernetes que se van a crear al implementar. Se especifica en la siguiente parte del archivo de configuración de implementación `bdc.json`:

```json
"metadata": {
    "kind": "BigDataCluster",
    "name": "mssql-cluster"
},
```

El siguiente comando envía un par clave-valor al parámetro `--json-values` para cambiar el nombre del clúster de macrodatos a `test-cluster`:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "metadata.name=test-cluster"
```

> [!IMPORTANT]
> El nombre del clúster de macrodatos debe estar formado solo por caracteres alfanuméricos en minúsculas sin espacios. Todos los artefactos de Kubernetes (contenedores, pods, conjuntos con estado, servicios) para el clúster se crean en un espacio de nombres con el mismo nombre que el nombre de clúster especificado.

## <a name="update-endpoint-ports"></a><a id="ports"></a> Actualización de los puertos de puntos de conexión

Se definen puntos de conexión para el controlador de `control.json` y para la puerta de enlace y la instancia maestra de SQL Server en las correspondientes secciones de `bdc.json`. La siguiente parte del archivo de configuración `control.json` muestra las definiciones de puntos de conexión del controlador:

```json
{
  "endpoints": [
    {
      "name": "Controller",
      "serviceType": "LoadBalancer",
      "port": 30080
    },
    {
      "name": "ServiceProxy",
      "serviceType": "LoadBalancer",
      "port": 30777
    }
  ]
}
```

En el siguiente ejemplo se usa JSON en línea para cambiar el puerto del punto de conexión de `controller`:

```bash
azdata bdc config replace --config-file custom-bdc/control.json --json-values "$.spec.endpoints[?(@.name==""Controller"")].port=30000"
```

## <a name="configure-scale"></a><a id="replicas"></a> Configuración de la escala

Las configuraciones de cada recurso (como, por ejemplo, el grupo de almacenamiento) se definen en el archivo de configuración `bdc.json`. Por ejemplo, la siguiente parte de `bdc.json` muestra una definición de recursos de `storage-0`:

```json
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
                "spark-defaults-conf.spark.driver.memory": "2g",
                "spark-defaults-conf.spark.driver.cores": "1",
                "spark-defaults-conf.spark.executor.instances": "3",
                "spark-defaults-conf.spark.executor.memory": "1536m",
                "spark-defaults-conf.spark.executor.cores": "1",
                "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
                "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
                "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
                "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
                "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
            }
        }
    }
}
```

Para configurar el número de instancias de un grupo de almacenamiento, un grupo de proceso y un grupo de datos, hay que modificar el valor de `replicas` de cada grupo. En el siguiente ejemplo se usa JSON en línea para cambiar estos valores del grupo de almacenamiento, del grupo de proceso y del grupo de datos por `10`, `4` y `4` respectivamente:

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.replicas=10"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.compute-0.spec.replicas=4"
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.data-0.spec.replicas=4"
```

> [!NOTE]
> El número máximo de instancias validadas para los grupos de proceso y de datos es de `8` cada una. No existe límite que haya que cumplir en el momento de la implementación, pero se desaconseja configurar una escala mayor en las implementaciones de producción.

## <a name="configure-storage"></a><a id="storage"></a> Configuración del almacenamiento

También puede cambiar la clase de almacenamiento y las características que se usan para cada grupo. En el ejemplo siguiente se asigna una clase de almacenamiento personalizada al grupo de almacenamiento y al grupo de datos, y se actualiza el tamaño de la notificación de volumen persistente para almacenar datos de hasta 500 GB para HDFS (grupo de almacenamiento) y de hasta 100 GB para el grupo maestro y de datos. 

> [!TIP]
> Para obtener más información sobre la configuración de almacenamiento, vea [Persistencia de datos con clústeres de macrodatos de SQL Server en Kubernetes](concept-data-persistence.md).

En primer lugar, cree un archivo patch.json como se muestra a continuación para ajustar la configuración de *almacenamiento*.

```json
{
        "patch": [
                {
                        "op": "add",
                        "path": "spec.resources.storage-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "500Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
        {
                        "op": "add",
                        "path": "spec.resources.master.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                },
                {
                        "op": "add",
                        "path": "spec.resources.data-0.spec.storage",
                        "value": {
                                "data": {
                                        "size": "100Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                },
                                "logs": {
                                        "size": "30Gi",
                                        "className": "default",
                                        "accessMode": "ReadWriteOnce"
                                }
                        }
                }
        ]
}

```

Después, puede usar el comando `azdata bdc config patch` para actualizar el archivo de configuración `bdc.json`.
```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch ./patch.json
```

> [!NOTE]
> Un archivo de configuración basado en `kubeadm-dev-test` no tiene una definición de almacenamiento para cada grupo, pero puede usar el proceso anterior para agregarla si fuese necesario.

## <a name="configure-storage-pool-without-spark"></a><a id="sparkstorage"></a> Configuración del bloque de almacenamiento sin Spark

También puede configurar los bloques de almacenamiento para que se ejecuten sin Spark y crear un grupo de Spark independiente. Esta configuración permite escalar la potencia de proceso de Spark independientemente del almacenamiento. Para saber cómo se configura el grupo de Spark, vea la sección [Creación de un grupo de Spark](#sparkpool) de este artículo.

> [!NOTE]
> No se puede implementar un clúster de macrodatos sin Spark, por lo que hay que establecer `includeSpark` en `true`, o bien crear un [grupo de Spark](#sparkpool) aparte con al menos una instancia. También puede tener Spark en ejecución en el grupo de almacenamiento (`includeSpark` es `true`) y tener un grupo de Spark aparte.

El valor `includeSpark` del recurso del grupo de almacenamiento está establecido de forma predeterminada en true, por lo que debe editar el campo `includeSpark` con la configuración de almacenamiento para poder realizar cambios. El siguiente comando muestra cómo editar este valor mediante JSON en línea.

```bash
azdata bdc config replace --config-file custom-bdc/bdc.json --json-values "$.spec.resources.storage-0.spec.settings.spark.includeSpark=false"
```

## <a name="create-a-spark-pool"></a><a id="sparkpool"></a> Creación de un grupo de Spark

Puede crear un grupo de Spark como complemento de o en lugar de las instancias de Spark que se ejecutan en el grupo de almacenamiento. En el siguiente ejemplo se muestra cómo crear un grupo de Spark con dos instancias revisando el archivo de configuración `bdc.json`. 

En primer lugar, cree un archivo `spark-pool-patch.json` del siguiente modo:

```json
{
    "patch": [
        {
            "op": "add",
            "path": "spec.resources.spark-0",
            "value": {
                "metadata": {
                    "kind": "Pool",
                    "name": "default"
                },
                "spec": {
                    "type": "Spark",
                    "replicas": 2
                }
            }
        },
        {
            "op": "add",
            "path": "spec.services.spark.resources/-",
            "value": "spark-0"
        },
        {
            "op": "add",
            "path": "spec.services.hdfs.resources/-",
            "value": "spark-0"
        }
    ]
}
```

Luego, ejecute el comando `azdata bdc config patch`:

```bash
azdata bdc config patch -c custom-bdc/bdc.json -p spark-pool-patch.json
```

## <a name="configure-pod-placement-using-kubernetes-labels"></a><a id="podplacement"></a> Configuración de la ubicación de pods mediante etiquetas de Kubernetes

Puede controlar la ubicación de pods en nodos de Kubernetes con recursos específicos para acomodar varios tipos de requisitos de carga de trabajo. Con las etiquetas de Kubernetes, puede personalizar qué nodos del clúster de Kubernetes se van a usar para implementar los recursos del clúster de macrodatos, pero también restringir los nodos que se usan en recursos concretos.
Por ejemplo, puede que quiera asegurarse de que los pods del recurso de grupo de almacenamiento se colocan en nodos con más almacenamiento, mientras que las instancias maestras de SQL Server se colocan en nodos con más recursos de CPU y memoria. En este caso, primero crea un clúster de Kubernetes heterogéneo con distintos tipos de hardware y luego [asigna etiquetas de nodo](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/) en consecuencia. En el momento de implementar el clúster de macrodatos, puede especificar las mismas etiquetas en el nivel de clúster para indicar qué nodos se usan en el clúster de macrodatos, usando para ello el atributo `clusterLabel` en el archivo `control.json`. Después, se usarán etiquetas diferentes para la colocación del nivel de grupo. Estas etiquetas se pueden especificar en los archivos de configuración de implementación del clúster de macrodatos con el atributo `nodeLabel`. Kubernetes asigna los pods en los nodos que coincidan con las etiquetas especificadas. Las claves de etiqueta específicas que se deben agregar a los nodos del clúster de Kubernetes son `mssql-cluster` (para indicar qué nodos se usan con el clúster de macrodatos) y `mssql-resource` (para indicar los nodos específicos en los que se van a colocar los pods de varios recursos). Los valores de estas etiquetas pueden ser cualquier cadena que elija.

> [!NOTE]
> Debido a la naturaleza de los pods, que recopilan métricas de nivel de nodo, se implementan pods `metricsdc` en todos los nodos con la etiqueta `mssql-cluster`, y el elemento `mssql-resource` no se aplicará a estos pods.

En el siguiente ejemplo se muestra cómo editar un archivo de configuración personalizado para incluir una etiqueta de nodo `bdc` para todo el clúster de macrodatos, una etiqueta `bdc-master` para colocar pods de la instancia maestra de SQL Server en un nodo específico, `bdc-storage-pool` para los recursos de grupo de almacenamiento, `bdc-compute-pool` para los pods de los grupos de proceso y de datos y `bdc-shared` para el resto de los recursos. 

Primero, etiquete los nodos de Kubernetes:

```bash
kubectl label node <kubernetesNodeName1> mssql-cluster=bdc mssql-resource=bdc-shared --overwrite=true
kubectl label node <kubernetesNodeName2> mssql-cluster=bdc mssql-resource=bdc-master --overwrite=true
kubectl label node <kubernetesNodeName3> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName4> mssql-cluster=bdc mssql-resource=bdc-compute-pool --overwrite=true
kubectl label node <kubernetesNodeName5> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName6> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName7> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
kubectl label node <kubernetesNodeName8> mssql-cluster=bdc mssql-resource=bdc-storage-pool --overwrite=true
```

Luego, actualice los archivos de configuración de la implementación de clúster para incluir los valores de etiqueta. En este ejemplo se da por hecho que está personalizando archivos de configuración en un perfil de `custom-bdc`. En las configuraciones integradas no existen claves `nodeLabel` y `clusterLabel` de forma predeterminada, por lo que tendrá que editar manualmente un archivo de configuración personalizado o usar los comandos `azdata bdc config add` para realizar las modificaciones necesarias.

```bash
azdata bdc config add -c custom-bdc/control.json -j "$.spec.clusterLabel=bdc"
azdata bdc config add -c custom-bdc/control.json -j "$.spec.nodeLabel=bdc-shared"

azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.master.spec.nodeLabel=bdc-master"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.compute-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.data-0.spec.nodeLabel=bdc-compute-pool"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.storage-0.spec.nodeLabel=bdc-storage-pool"

# below can be omitted in which case we will take the node label default from the control.json
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.nmnode-0.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.sparkhead.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.zookeeper.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.gateway.spec.nodeLabel=bdc-shared"
azdata bdc config add -c custom-bdc/bdc.json -j "$.spec.resources.appproxy.spec.nodeLabel=bdc-shared"
```
>[!NOTE]
> El procedimiento recomendado es evitar que se conceda al nodo principal de Kubernetes cualquiera de los roles de BDC anteriores. Si tiene previsto asignar estos roles al nodo principal de Kubernetes, tendrá que [quitar el valor taint ``master:NoSchedule``.](https://kubernetes.io/docs/concepts/configuration/taint-and-toleration/) Tenga en cuenta que esto podría sobrecargar el nodo principal e inhibir su capacidad de realizar sus tareas de administración de Kubernetes en clústeres más grandes. Es normal ver algunos pods programados para el principal en cualquier implementación: ya toleran el valor taint ``master:NoSchedule`` y se usan principalmente para facilitar la administración del clúster. 

## <a name="other-customizations-using-json-patch-files"></a><a id="jsonpatch"></a> Otras personalizaciones mediante archivos de revisión JSON

Los archivos de revisión JSON configuran varias opciones a la vez. Para obtener más información sobre las revisiones de JSON, vea [Revisiones de JSON en Python](https://github.com/stefankoegl/python-json-patch) y [JSONPath Online Evaluator](https://jsonpath.com/).

Los siguientes archivos `patch.json` realizan los siguientes cambios:

- Actualización del puerto del punto de conexión único de `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.endpoints[?(@.name=='Controller')].port",
      "value": 30000
    }
  ]
}
```

- Actualización de todos los puntos de conexión (`port` y `serviceType`) en `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.endpoints",
      "value": [
        {
          "serviceType": "LoadBalancer",
          "port": 30001,
          "name": "Controller"
        },
        {
          "serviceType": "LoadBalancer",
          "port": 30778,
          "name": "ServiceProxy"
        }
      ]
    }
  ]
}
```

- Actualización de la configuración de almacenamiento del controlador de `control.json`. Esta configuración se aplica a todos los componentes del clúster, a menos que se invalide en el nivel de grupo.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage",
      "value": {
        "data": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "100Gi"
        },
        "logs": {
          "className": "managed-premium",
          "accessMode": "ReadWriteOnce",
          "size": "32Gi"
        }
      }
    }
  ]
}
```

- Actualización del nombre de la clase de almacenamiento en `control.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.storage.data.className",
      "value": "managed-premium"
    }
  ]
}
```

- Actualización de la configuración de almacenamiento de grupo del grupo de almacenamiento en `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.resources.storage-0.spec",
      "value": {
        "type": "Storage",
        "replicas": 2,
        "storage": {
          "data": {
            "size": "100Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          },
          "logs": {
            "size": "32Gi",
            "className": "myStorageClass",
            "accessMode": "ReadWriteOnce"
          }
        }
      }
    }
  ]
}
```

- Actualización de la configuración de Spark del grupo de almacenamiento en `bdc.json`.

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "spec.services.spark.settings",
      "value": {
            "spark-defaults-conf.spark.driver.memory": "2g",
            "spark-defaults-conf.spark.driver.cores": "1",
            "spark-defaults-conf.spark.executor.instances": "3",
            "spark-defaults-conf.spark.executor.memory": "1536m",
            "spark-defaults-conf.spark.executor.cores": "1",
            "yarn-site.yarn.nodemanager.resource.memory-mb": "18432",
            "yarn-site.yarn.nodemanager.resource.cpu-vcores": "6",
            "yarn-site.yarn.scheduler.maximum-allocation-mb": "18432",
            "yarn-site.yarn.scheduler.maximum-allocation-vcores": "6",
            "yarn-site.yarn.scheduler.capacity.maximum-am-resource-percent": "0.3"
      }
    }
  ]
}
```

> [!TIP]
> Para obtener más información sobre la estructura y las opciones para cambiar un archivo de configuración de implementación, vea [Referencia del archivo de configuración de implementación para los clústeres de datos de gran tamaño](reference-deployment-config.md).

Use comandos `azdata bdc config` para aplicar los cambios al archivo de revisión JSON. En el siguiente ejemplo, el archivo `patch.json` se aplica a un archivo de configuración de implementación de destino `custom-bdc/bdc.json`.

```bash
azdata bdc config patch --config-file custom-bdc/bdc.json --patch-file ./patch.json
```

## <a name="disable-elasticsearch-to-run-in-privileged-mode"></a>Desactivación de la ejecución de Elasticsearch en modo privilegiado

El contenedor de Elasticsearch se ejecuta de forma predeterminada en modo privilegiado en el clúster de macrodatos. Esta configuración garantiza que, en el momento de la inicialización del contenedor, este va a tener permisos suficientes para actualizar un valor en el host necesario cuando Elasticsearch procese una mayor cantidad de registros. Encontrará más información sobre este tema en [este artículo](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html). 

Para deshabilitar la ejecución del contenedor de Elasticsearch en modo privilegiado, debe actualizar la sección `settings` en `control.json` y especificar un valor en `vm.max_map_count` de `-1`. Este es un ejemplo de cómo sería esta sección:

```json
{
    "apiVersion": "v1",
    "metadata": {...},
    "spec": {
        "docker": {...},
        "storage": {...},
        "endpoints": [...],
        "settings": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
            }
        }
    }
}
```

`control.json` se puede editar manualmente y agregar la sección anterior a `spec`, o también puede crear un archivo de revisión `elasticsearch-patch.json` como el que se indica a continuación y usar [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] para aplicar dicha revisión al archivo `control.json`:

```json
{
  "patch": [
    {
      "op": "add",
      "path": "spec.settings",
      "value": {
            "ElasticSearch": {
                "vm.max_map_count": "-1"
        }
      }
    }
  ]
}
```

Ejecute este comando para aplicar la revisión al archivo de configuración:

```bash
azdata bdc config patch --config-file custom-bdc/control.json --patch-file elasticsearch-patch.json
```

> [!IMPORTANT]
> Como procedimiento recomendado, se aconseja actualizar manualmente el valor de `max_map_count` en cada host del clúster de Kubernetes según se indica en las instrucciones de [este artículo](https://www.elastic.co/guide/en/elasticsearch/reference/current/vm-max-map-count.html).

## <a name="turn-pods-and-nodes-metrics-collection-onoff"></a>Activación y desactivación de la recopilación de métricas de pods y nodos

En SQL Server 2019 CU5 se han habilitado dos modificadores de características para controlar la recopilación de métricas de pods y nodos. Si usa soluciones distintas para la supervisión de la infraestructura de Kubernetes, y quiere desactivar la recopilación de métricas integradas para pods y nodos de host, establezca *allowNodeMetricsCollection* y *allowPodMetricsCollection* en *false* en el archivo de configuración de implementación de *control.json*. En el caso de entornos OpenShift, esta configuración se establece en *false* de forma predeterminada en los perfiles de implementación integrados, ya que la recopilación de métricas de pods y nodos requiere capacidades con privilegios.
Ejecute este comando para actualizar los valores de esta configuración en el archivo de configuración personalizado mediante la CLI de *azdata*:

```bash
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowNodeMetricsCollection=false"
 azdata bdc config replace -c custom-bdc/control.json -j "$.security.allowPodMetricsCollection=false"
 ```

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre cómo usar archivos de configuración en implementaciones de clústeres de macrodatos, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md#configfile).