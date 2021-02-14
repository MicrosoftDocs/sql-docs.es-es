---
title: Persistencia de los datos en Kubernetes
titleSuffix: SQL Server big data clusters
description: Obtenga información sobre cómo los volúmenes persistentes proporcionan un modelo de complemento para el almacenamiento en Kubernetes. También, puede obtener información sobre cómo funciona la persistencia de los datos en un clúster de macrodatos de SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a2836a6966727c0c2d27b363048f2e1dcd6afa49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052143"
---
# <a name="data-persistence-with-sql-server-big-data-cluster-in-kubernetes"></a>Persistencia de los datos con un clúster de macrodatos de SQL Server en Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Los [volúmenes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/) proporcionan un modelo de complemento para el almacenamiento en Kubernetes. En este modelo, la forma en que se proporciona el almacenamiento depende de cómo se use. Por lo tanto, puede usar su propio almacenamiento con alta disponibilidad y conectarlo al clúster de macrodatos de SQL Server. Esto le proporciona control total sobre el tipo de almacenamiento, la disponibilidad y el rendimiento que necesite. Kubernetes admite [distintos tipos de soluciones de almacenamiento](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner), como discos o archivos de Azure, Network File System (NFS) y almacenamiento local, entre otras.

## <a name="configure-persistent-volumes"></a>Configurar volúmenes persistentes

Un clúster de macrodatos de SQL Server consume estos volúmenes persistentes mediante [clases de almacenamiento](https://kubernetes.io/docs/concepts/storage/storage-classes/). Puede crear distintas clases de almacenamiento para diferentes tipos de almacenamiento y especificarlos todos en el momento de la implementación del clúster de macrodatos. Puede configurar la clase de almacenamiento y el tamaño de la reclamación de volumen persistente que se usará para cada finalidad específica en el nivel de grupo. Un clúster de macrodatos de SQL Server crea [reclamaciones de volúmenes persistentes](https://kubernetes.io/docs/concepts/storage/persistent-volumes/#persistentvolumeclaims) con el nombre de la clase de almacenamiento especificado para cada componente que necesite volúmenes persistentes. Después, monta el volumen (o los volúmenes) persistente correspondiente en el pod. 

Estos son algunos aspectos importantes que se deben tener en cuenta al planear la configuración de almacenamiento de un clúster de macrodatos:

- Para que una implementación de clúster de macrodatos sea correcta, asegúrese de que hay disponible el número necesario de volúmenes persistentes. Si va a implementar en un clúster de Azure Kubernetes Service (AKS) y usa una clase de almacenamiento integrada (`default` o `managed-premium`), esta clase admite el aprovisionamiento dinámico de volúmenes persistentes. Por lo tanto, no es necesario crear los volúmenes persistentes previamente, pero deberá asegurarse de que los nodos de trabajo disponibles en el clúster de AKS pueden asociar tantos discos como volúmenes persistentes sean necesarios para la implementación. En función del [tamaño de máquina virtual](/azure/virtual-machines/linux/sizes) especificado para los nodos de trabajo, cada nodo podrá asociar un número determinado de discos. En un clúster de tamaño predeterminado (sin alta disponibilidad), se requiere un mínimo de 24 discos. Si va a habilitar la alta disponibilidad o a escalar verticalmente un grupo, asegúrese de que haya como mínimo dos volúmenes persistentes por cada réplica de más, con independencia del recurso que se esté escalando verticalmente.

- Si el aprovisionador de almacenamiento de la clase de almacenamiento que está proporcionando en la configuración no admite el aprovisionamiento dinámico, habrá que crear los volúmenes persistentes previamente. Por ejemplo, el aprovisionador `local-storage` no permite el aprovisionamiento dinámico. Vea este [script de ejemplo](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu) para obtener instrucciones cómo llevar esto a cabo en un clúster de Kubernetes implementado con `kubeadm`.

- Al implementar un clúster de macrodatos, puede configurar la misma clase de almacenamiento para que la usen todos los componentes del clúster, pero como procedimiento recomendado en una implementación de producción, varios componentes requerirán distintas configuraciones de almacenamiento para acomodar diversas cargas de trabajo en términos de tamaño o rendimiento. Puede sobrescribir la configuración de almacenamiento predeterminada especificada en el controlador para cada uno de los grupos de almacenamiento, conjuntos de datos e instancias maestras de SQL Server. En este artículo se facilitan ejemplos de cómo hacerlo.

- Al calcular los requisitos de tamaño del bloque de almacenamiento, debe tener en cuenta el factor de replicación con el que se configura HDFS.  El factor de replicación se puede configurar en el momento de la implementación en el archivo de configuración de implementación del clúster. El valor predeterminado para los perfiles de desarrollo y pruebas (es decir, `aks-dev-test` o `kubeadm-dev-test`) es 2, y para los perfiles que se recomiendan para las implementaciones de producción (es decir, `kubeadm-prod`) el valor predeterminado es 3. Como procedimiento recomendado, configure la implementación de producción de un clúster de macrodatos con un factor de replicación de al menos 3 para HDFS. El valor del factor de replicación afectará al número de instancias del bloque de almacenamiento: como mínimo, debe implementar tantas instancias del bloque de almacenamiento como el valor del factor de replicación. Además, debe ajustar el tamaño del almacenamiento en consecuencia, y la cuenta de los datos que se replican en HDFS tantas veces como el valor del factor de replicación. Puede encontrar más información sobre la replicación de datos en HDFS [aquí](https://hadoop.apache.org/docs/r3.2.1/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html#Data_Replication). 

- A partir de la versión 2019 CU1 de SQL Server, los clústeres de macrodatos no habilitan una experiencia para actualizar la opción de configuración de almacenamiento posterior a la implementación. Esta limitación le impide usar las operaciones de los clústeres de macrodatos para modificar el tamaño de la demanda de volumen persistente de cada instancia o escalar un grupo tras la implementación. Por lo tanto, es muy importante que planee el diseño del almacenamiento antes de implementar un clúster de macrodatos. Sin embargo, puede expandir el tamaño del volumen persistente directamente con las API de Kubernetes. En este caso, los metadatos del clúster de macrodatos no se actualizarán y verá incoherencias con respecto a los tamaños de volumen en los metadatos del clúster de configuración.

- Al implementar en Kubernetes como aplicaciones en contenedor y usar características como conjuntos con estado y almacenamiento persistente, Kubernetes garantiza que los pods se van a reiniciar en caso de haya problemas de estado y, asimismo, que van a estar asociados al mismo almacenamiento persistente. Con todo, si se produce un error de nodo y el pod debe reiniciarse en otro nodo, existe un mayor riesgo de que no esté disponible si el almacenamiento es local para el nodo con el error. Para reducir este riesgo, debe configurar más redundancia y habilitar [características de alta disponibilidad](deployment-high-availability.md), o bien usar almacenamiento con redundancia remota. Aquí se muestra información general de las opciones de almacenamiento de diversos componentes en los clústeres de macrodatos:

| Recursos | Tipo de almacenamiento de datos | Tipo de almacenamiento de registros |  Notas |
|---|---|---|--|
| Instancia maestra de SQL Server | Almacenamiento local (réplicas>= 3) o con redundancia remota (réplica=1) | Almacenamiento local | Implementación basada en conjuntos con estado donde la adherencia de los pods a los nodos garantizará que se reinician y que los errores transitorios no van a provocar pérdidas de datos. |
| Grupo de proceso | Almacenamiento local | Almacenamiento local | No se almacenan datos de usuario. |
| Grupo de datos | Almacenamiento con redundancia local o remota | Almacenamiento local | Use el almacenamiento con redundancia remota si el grupo de datos no se puede volver a generar desde otros orígenes de datos.  |
| Grupo de almacenamiento (HDFS) | Almacenamiento con redundancia local o remota | Almacenamiento local | La replicación de HDFS está habilitada. |
| Grupo de Spark | Almacenamiento local | Almacenamiento local | No se almacenan datos de usuario. |
| Control (controldb, metricsdb, logsdb)| Almacenamiento con redundancia remota | Almacenamiento local | Fundamental para usar la redundancia de nivel de almacenamiento para metadatos del clústeres de macrodatos. |

> [!IMPORTANT]
> Asegúrese de que los componentes relacionados con el control están en un dispositivo con almacenamiento con redundancia remota. El pod `controldb-0` hospeda una instancia de SQL Server con bases de datos que almacenan todos los metadatos relativos a los estados de servicio de los clústeres, diversas configuraciones y otra información pertinente. Si esta instancia deja de estar disponible, habrá problemas de estado con otros servicios dependientes del clúster.

## <a name="configure-big-data-cluster-storage-settings"></a>Configurar las opciones de almacenamiento del clúster de macrodatos

De forma similar a otras personalizaciones, puede especificar opciones de configuración de almacenamiento en los archivos de configuración del clúster en el momento de la implementación para cada grupo en el archivo de configuración `bdc.json` y, para los servicios de control, en el archivo `control.json`. Si no hay ninguna opción de configuración de almacenamiento en las especificaciones del grupo, se usará la configuración de almacenamiento de control en todos los demás componentes, incluida la instancia maestra de SQL Server (recurso `master`), HDFS (recurso `storage-0`) y los componentes del grupo de datos. El ejemplo de código siguiente representa la sección de configuración de almacenamiento que puede incluir en las especificaciones:

```json
    "storage": 
    {
      "data": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "15Gi"
      },
      "logs": {
        "className": "default",
        "accessMode": "ReadWriteOnce",
        "size": "10Gi"
    }
```

La implementación del clúster de macrodatos usa el almacenamiento persistente para almacenar datos, metadatos y registros para distintos componentes. Puede personalizar el tamaño de las reclamaciones de volúmenes persistentes creadas como parte de la implementación. El procedimiento recomendado es usar clases de almacenamiento con una *directiva de reclamación de* [Conservar](https://kubernetes.io/docs/concepts/storage/storage-classes/#reclaim-policy).

> [!WARNING]
> La ejecución sin almacenamiento persistente puede funcionar en un entorno de prueba, pero daría como resultado un clúster no funcional. Al reiniciarse el pod, los metadatos del clúster o los datos de usuario se pierden de forma permanente. No se recomienda ejecutar con esta configuración.

En la sección [Configurar el almacenamiento](#config-samples), se proporcionan más ejemplos de cómo configurar el almacenamiento para su implementación del clúster de macrodatos de SQL Server.

## <a name="aks-storage-classes"></a>Clases de almacenamiento de AKS

AKS viene con [dos clases de almacenamiento integradas](/azure/aks/azure-disks-dynamic-pv/) (`default` y `managed-premium`), además del aprovisionador dinámico de estas. Puede especificar una de estas, o bien puede crear su propia clase de almacenamiento para implementar el clúster de macrodatos con el almacenamiento persistente habilitado. En el archivo de configuración del clúster integrado para AKS (`aks-dev-test`) se incluyen de forma predeterminada configuraciones de almacenamiento persistente que usan la clase de almacenamiento `default`.

> [!WARNING]
> Los volúmenes persistentes creados con las clases de almacenamiento integradas `default` y `managed-premium` tienen una directiva de reclamación de tipo *Eliminar*. Por lo tanto, al eliminar el clúster de macrodatos de SQL Server, también se eliminarán las reclamaciones de volúmenes persistentes, así como los volúmenes persistentes. Puede crear clases de almacenamiento personalizadas mediante el aprovisionador `azure-disk` con una directiva de reclamación de tipo `Retain`, como se explica en los [conceptos de almacenamiento](/azure/aks/concepts-storage/#storage-classes).

## <a name="storage-classes-for-kubeadm-clusters"></a>Clases de almacenamiento de clústeres `kubeadm` 

Los clústeres de Kubernetes implementados mediante `kubeadm` carecen de una clase de almacenamiento integrada. Debe crear sus propias clases de almacenamiento y volúmenes persistentes mediante almacenamiento local o su [aprovisionador de almacenamiento preferido](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner). En ese caso, establezca el elemento `className` en la clase de almacenamiento que haya configurado.

> [!IMPORTANT]
>  En los archivos de configuración de implementación integrados de kubeadm (`kubeadm-dev-test` y `kubeadm-prod`), no se especifica ningún nombre de clase de almacenamiento para el almacenamiento de datos o de registros. Antes de la implementación, tendrá que personalizar el archivo de configuración y establecer el valor de `className`. De lo contrario, se producirá un error en las validaciones anteriores a la implementación. La implementación también tiene un paso de validación que comprueba si existe la clase de almacenamiento, pero no para los volúmenes persistentes necesarios. Asegúrese de crear volúmenes suficientes, según la escala del clúster. Como tamaño mínimo predeterminado de clúster (escala predeterminada, sin alta disponibilidad), se deben crear al menos 24 volúmenes persistentes. Para ver un ejemplo de cómo crear volúmenes persistentes mediante un aprovisionador local, consulte [Configuración de Kubernetes con kubeadm](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/deployment/kubeadm/ubuntu).

## <a name="customize-storage-configurations-for-each-pool"></a>Personalizar las configuraciones de almacenamiento para cada grupo

Para todas las personalizaciones, primero tiene que crear una copia del archivo de configuración integrado que quiera usar. Por ejemplo, el siguiente comando crea una copia de los archivos de configuración de implementación de `aks-dev-test` en un subdirectorio denominado `custom`:

```bash
azdata bdc config init --source aks-dev-test --target custom
```

Este proceso crea dos archivos, `bdc.json` y `control.json`, que puede personalizar manualmente o mediante el comando `azdata bdc config`. Puede usar una combinación de las bibliotecas jsonpath y jsonpatch para proporcionar formas de editar los archivos de configuración.


### <a name="configure-storage-class-name-andor-claims-size"></a><a id="config-samples"></a> Configurar el nombre de la clase de almacenamiento o el tamaño de las reclamaciones

De forma predeterminada, el tamaño de las reclamaciones de volúmenes persistentes aprovisionadas para cada pod en el clúster es de 10 GB. Puede actualizar este valor para adaptarse a las cargas de trabajo que ejecute en un archivo de configuración personalizado antes de la implementación del clúster.

En el siguiente ejemplo se actualiza a 32 GB el tamaño de las reclamaciones de volúmenes persistentes en `control.json`. Si no se reemplaza en el nivel de grupo, esta opción se aplicará en todos los grupos.

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.size=100Gi"
```

En el siguiente ejemplo se muestra cómo modificar la clase de almacenamiento del archivo `control.json`:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.storage.data.className=<yourStorageClassName>"
```

También tiene la opción de editar de forma manual el archivo de configuración personalizado o usar una revisión de JSON que cambia la clase de almacenamiento para el grupo de almacenamiento, como en el ejemplo siguiente:

```json
{
  "patch": [
    {
      "op": "replace",
      "path": "$.spec.resources.storage-0.spec",
      "value": {
        "type":"Storage",
        "replicas":2,
        "storage":{
            "data":{
                    "size": "100Gi",
                    "className": "myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    },
            "logs":{
                    "size":"32Gi",
                    "className":"myStorageClass",
                    "accessMode":"ReadWriteOnce"
                    }
                }
            }
        }
    ]
}
```

Aplique el archivo de revisión. Use el comando `azdata bdc config patch` para aplicar los cambios al archivo de revisión .json. En el siguiente ejemplo, el archivo `patch.json` se aplica a un archivo de configuración de implementación de destino, `custom.json`:

```bash
azdata bdc config patch --config-file custom/bdc.json --patch-file ./patch.json
```

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre los volúmenes en Kubernetes, vea la [documentación de Kubernetes sobre volúmenes](https://kubernetes.io/docs/concepts/storage/volumes/).

- Para obtener más información sobre cómo implementar un clúster de macrodatos de SQL Server, vea [Procedimiento para implementar clústeres de macrodatos de SQL Server en Kubernetes](deployment-guidance.md).
