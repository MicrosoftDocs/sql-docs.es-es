---
title: Machine Learning Services (Python, R)
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre ejecutar scripts de Python y R en la instancia maestra de clústeres de macrodatos de SQL Server con Machine Learning Services.
author: dphansen
ms.author: davidph
ms.date: 11/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: machine-learning-bdc
ms.openlocfilehash: af0611396dcd50fcbea41e96ecc9d651b56a91d0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047535"
---
# <a name="run-python-and-r-scripts-with-machine-learning-services-on-sql-server-big-data-clusters"></a>Ejecución de scripts de Python y R con Machine Learning Services en clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Se pueden ejecutar scripts de Python y R en la instancia maestra de [clústeres de macrodatos de SQL Server](big-data-cluster-overview.md) con [Machine Learning Services](../machine-learning/index.yml).

> [!NOTE]
> También puede ejecutar código Java en la instancia maestra de Clústeres de macrodatos de SQL Server con la [extensión del lenguaje Java](../language-extensions/java-overview.md). Si sigue los pasos que se indican aquí, también se habilitarán las [extensiones de lenguaje de SQL Server](../language-extensions/language-extensions-overview.md).

## <a name="enable-machine-learning-services"></a>Habilitar Machine Learning Services

Machine Learning Services se instala de forma predeterminada en clústeres de macrodatos y no precisa de una instalación independiente.

Para habilitar Machine Learning Services, ejecute esta instrucción en la instancia maestra:

```sql
EXEC sp_configure 'external scripts enabled', 1
RECONFIGURE WITH OVERRIDE
GO
```

Ya está listo para ejecutar scripts de Python y R en la instancia maestra de clústeres de macrodatos. Vea las guías de inicio rápido en [Pasos siguientes](#next-steps) para ejecutar su primer script.

>[!NOTE]
>No se puede establecer el valor de configuración en una conexión del cliente de escucha del grupo de disponibilidad. Si la característica Clústeres de macrodatos se implementa con alta disponibilidad, establezca `external scripts enabled` en cada réplica. Vea [Habilitación en un clúster de alta disponibilidad](#enable-on-cluster-with-high-availability).

## <a name="enable-on-cluster-with-high-availability"></a>Habilitación en un clúster de alta disponibilidad

Cuando se [implementan clústeres de macrodatos de SQL Server con alta disponibilidad](deployment-high-availability.md), la implementación crea un grupo de disponibilidad para la instancia maestra. Para habilitar Machine Learning Services, establezca `external scripts enabled` en cada instancia del grupo de disponibilidad. En el caso de un clúster de macrodatos, debe ejecutar `sp_configure` en cada réplica de la instancia maestra de SQL Server.

En la siguiente sección se describe cómo habilitar los scripts externos en cada instancia.

### <a name="create-an-external-load-balancer-for-each-instance"></a>Creación de un equilibrador de carga externo para cada instancia

Para cada réplica del grupo de disponibilidad, cree un equilibrador de carga que le permita conectarse a la instancia. 

`kubectl expose pod <pod-name> --port=<connection port number> --name=<load-balancer-name> --type=LoadBalancer -n <kubernetes namespace>`

En los ejemplos de este artículo se utilizan los valores siguientes:

- `<pod-name>`: `master-#`
- `<connection port number>`: `1533`
- `<load-balancer-name>`: `mymaster-#`
- `<kubernetes namespace>`: `mssql-cluster`

Actualice el script siguiente para su entorno y ejecute los comandos:

```bash
kubectl expose pod master-0 --port=1533 --name=mymaster-0 --type=LoadBalancer -n mssql-cluster 
kubectl expose pod master-1 --port=1533 --name=mymaster-1 --type=LoadBalancer -n mssql-cluster
kubectl expose pod master-2 --port=1533 --name=mymaster-2 --type=LoadBalancer -n mssql-cluster 
```

`kubectl` devuelve la salida siguiente.

```bash
service/mymaster-0 exposed
service/mymaster-1 exposed
service/mymaster-2 exposed
```

Cada equilibrador de carga es un punto de conexión de la réplica maestra.

### <a name="enable-script-execution-on-each-replica"></a>Habilitación de la ejecución de scripts en cada réplica

1. Obtiene la dirección IP del punto de conexión de la réplica maestra.

   El siguiente comando devuelve la dirección IP externa del punto de conexión de la réplica. 

   `kubectl get services <load-balancer-name> -n <kubernetes namespace>`

   Para obtener la dirección IP externa de cada réplica en este escenario, ejecute los siguientes comandos:

   ```bash
   kubectl get services mymaster-0 -n mssql-cluster
   kubectl get services mymaster-1 -n mssql-cluster
   kubectl get services mymaster-2 -n mssql-cluster
   ```

   >[!NOTE]
   > Es posible que la dirección IP externa tarde un poco en estar disponible. Ejecute el script anterior periódicamente hasta que cada punto de conexión devuelva una dirección IP externa.

1. Conéctese al punto de conexión de la réplica maestra y habilite la ejecución de scripts.

    Ejecute esta instrucción:

    ```sql
    EXEC sp_configure 'external scripts enabled', 1
    RECONFIGURE WITH OVERRIDE
    GO
    ```

   Por ejemplo, puede ejecutar el comando anterior con `sqlcmd`. En el ejemplo siguiente se conecta al punto de conexión de la réplica maestra y se habilita la ejecución de scripts. Actualice los valores del script para su entorno.

   ```bash
   sqlcmd -S <IP address>,1533 -U <user name> -P <password> -Q "EXEC sp_configure 'external scripts enabled', 1; RECONFIGURE WITH OVERRIDE;"
   ```

   Repita el paso para cada réplica.

### <a name="demonstration"></a>Demostración

La imagen siguiente muestra este proceso.

[![Demostración](media/machine-learning-services/example-kube-enable-scripts.png "Demostración de la característica de habilitación en Kubernetes")](media/machine-learning-services/example-kube-enable-scripts.png#lightbox)

Ya está listo para ejecutar scripts de Python y R en la instancia maestra de clústeres de macrodatos. Vea las guías de inicio rápido en [Pasos siguientes](#next-steps) para ejecutar su primer script.

### <a name="delete-the-master-replica-endpoints"></a>Eliminación de los puntos de conexión de la réplica maestra

En el clúster de Kubernetes, elimine el punto de conexión para cada réplica. El punto de conexión se expone en Kubernetes como un servicio de equilibrio de carga.

El siguiente comando elimina el servicio de equilibrio de carga.

`kubectl delete svc <load-balancer-name> -n mssql-cluster`

En los ejemplos de este artículo, ejecute los siguientes comandos.

```bash
kubectl delete svc mymaster-0 -n mssql-cluster
kubectl delete svc mymaster-1 -n mssql-cluster
kubectl delete svc mymaster-2 -n mssql-cluster
```

## <a name="next-steps"></a>Pasos siguientes

+ [Ejecución de scripts de Python simples](../machine-learning/tutorials/quickstart-python-create-script.md?toc=/sql/toc.json)
+ [Entrenamiento y puntuación de un modelo predictivo en Python](../machine-learning/tutorials/quickstart-python-train-score-model.md?toc=/sql/toc.json)
+ [Ejecución de scripts de R simples](../machine-learning/tutorials/quickstart-r-create-script.md?toc=/sql/toc.json)
+ [Entrenamiento y puntuación de un modelo predictivo en R](../machine-learning/tutorials/quickstart-r-train-score-model.md?toc=/sql/toc.json)
