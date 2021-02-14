---
title: Solución de problemas de Kubernetes
titleSuffix: SQL Server big data clusters
description: En este artículo se proporcionan comandos útiles para supervisar y solucionar problemas de un clúster de macrodatos de SQL Server 2019.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3f0d8ba02c56ca625094829770771c053e31802e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039645"
---
# <a name="troubleshoot-big-data-clusters-2019-kubernetes"></a>Solución de problemas de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Kubernetes

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describen varios comandos útiles de Kubernetes que puede usar para supervisar y solucionar problemas de un [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Se muestra cómo ver información detallada de un pod u otros artefactos de Kubernetes que se encuentran en el clúster de macrodatos. En este artículo también se tratan las tareas habituales, como copiar archivos en un contenedor que ejecute uno de los servicios de clúster de macrodatos de SQL Server, o bien copiarlos desde uno.

> [!TIP]
> Para supervisar el estado de los componentes de los clústeres de macrodatos, puede usar los comandos [**azdata bdc status**](deployment-guidance.md#status) o los [cuadernos de solución de problemas](notebooks-manage-bdc.md) integrados proporcionados con Azure Data Studio.

> [!TIP]
> Ejecute los siguientes comandos de **kubectl** en un equipo cliente Windows (cmd o PS) o Linux (bash). Requieren la autenticación previa en el clúster y un contexto de clúster en el que ejecutarse. Por ejemplo, para un clúster de AKS creado anteriormente, puede ejecutar `az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>` para descargar el archivo de configuración del clúster de Kubernetes y establecer el contexto del clúster.

## <a name="get-status-of-pods"></a>Obtención del estado de los pods

Puede usar el comando `kubectl get pods` para obtener el estado de los pods en el clúster para todos los espacios de nombres o el espacio de nombres del clúster de macrodatos. En las secciones siguientes se muestran ejemplos de ambos.

### <a name="show-status-of-all-pods-in-the-kubernetes-cluster"></a>Muestra del estado de todos los pods en el clúster de Kubernetes

Ejecute el siguiente comando para obtener todos los pods y sus estados, incluidos los pods que forman parte del espacio de nombres en el que se crean los pods del clúster de macrodatos de SQL Server:

```bash
kubectl get pods --all-namespaces
```

### <a name="show-status-of-all-pods-in-the-sql-server-big-data-cluster"></a>Muestra del estado de todos los pods en el clúster de macrodatos de SQL Server

Use el parámetro `-n` para especificar un espacio de nombres concreto. Tenga en cuenta que los pods del clúster de macrodatos de SQL Server se crean en un nuevo espacio de nombres creado cuando se arranca el clúster en función del nombre del clúster especificado en el archivo de configuración de la implementación. Aquí se usa el nombre predeterminado, `mssql-cluster`.

```bash
kubectl get pods -n mssql-cluster
```

Debería ver un resultado similar a la lista siguiente para un clúster de macrodatos en ejecución:

```output
PS C:\> kubectl get pods -n mssql-cluster
NAME              READY   STATUS    RESTARTS   AGE
appproxy-f2qqt    2/2     Running   0          110m
compute-0-0       3/3     Running   0          110m
control-zlncl     4/4     Running   0          118m
data-0-0          3/3     Running   0          110m
data-0-1          3/3     Running   0          110m
gateway-0         2/2     Running   0          109m
logsdb-0          1/1     Running   0          112m
logsui-jtdnv      1/1     Running   0          112m
master-0          7/7     Running   0          110m
metricsdb-0       1/1     Running   0          112m
metricsdc-shv2f   1/1     Running   0          112m
metricsui-9bcj7   1/1     Running   0          112m
mgmtproxy-x6gcs   2/2     Running   0          112m
nmnode-0-0        1/1     Running   0          110m
storage-0-0       7/7     Running   0          110m
storage-0-1       7/7     Running   0          110m
```

> [!NOTE]
> Durante la implementación, los pods con un valor de **STATUS** de **ContainerCreating** siguen apareciendo. Si la implementación se bloquea por cualquier motivo, esto le puede dar una idea de dónde puede estar el problema. Fíjese también en la columna **READY**. En ella se le indica cuántos contenedores se han iniciado en el pod. Tenga en cuenta que las implementaciones pueden tardar 30 minutos o más, en función de la configuración y la red. Gran parte de este tiempo se dedica a descargar las imágenes de contenedor de los diferentes componentes.

## <a name="get-pod-details"></a>Obtención de los detalles del pod

Ejecute el siguiente comando para obtener una descripción detallada de un pod específico en la salida en formato JSON. Se incluyen detalles, como el nodo de Kubernetes actual en el que se coloca el pod, los contenedores que se ejecutan en el pod y la imagen que se usa para arrancar los contenedores. También se muestran otros detalles, como las etiquetas, el estado y las notificaciones de los volúmenes persistentes que están asociadas con el pod.

```bash
kubectl describe pod  <pod_name> -n <namespace_name>
```

En el ejemplo siguiente se muestran los detalles del pod `master-0` en un clúster de macrodatos denominado `mssql-cluster`:

```bash
kubectl describe pod  master-0 -n mssql-cluster
```

Si se ha producido algún error, a veces puede verlo en los eventos recientes del pod.

## <a name="get-pod-logs"></a>Obtención de los registros del pod

Puede recuperar los registros de los contenedores que se ejecutan en un pod. El siguiente comando recupera los registros de todos los contenedores que se ejecutan en el pod denominado `master-0` y los envía a un archivo denominado `master-0-pod-logs.txt`:

```bash
kubectl logs master-0 --all-containers=true -n mssql-cluster > master-0-pod-logs.txt
```

## <a name="get-status-of-services"></a><a id="services"></a> Obtención del estado de los servicios

Ejecute el siguiente comando para obtener los detalles de los servicios del clúster de macrodatos. Estos detalles incluyen su tipo y las IP asociadas a los servicios y puertos correspondientes. Tenga en cuenta que los servicios del clúster de macrodatos de SQL Server se crean en un nuevo espacio de nombres creado cuando se arranca el clúster en función del nombre del clúster especificado en el archivo de configuración de la implementación.

```bash
kubectl get svc -n <namespace_name>
```

En el ejemplo siguiente se muestra el estado de los servicios en un clúster de macrodatos denominado `mssql-cluster`:

```bash
kubectl get svc -n mssql-cluster
```

Los siguientes servicios admiten conexiones externas al clúster de macrodatos:

| Servicio | Descripción |
|---|---|
| **master-svc-external** | Proporciona acceso a la instancia maestra.<br/>(**EXTERNAL-IP,31433** y el usuario de **SA**) |
| **controller-svc-external** | Admite herramientas y clientes que administran el clúster. |
| **gateway-svc-external** | Proporciona acceso a la puerta de enlace de HDFS/Spark.<br/>(**EXTERNAL-IP** y el usuario `<AZDATA_USERNAME>`)<sup>1</sup>|
| **appproxy-svc-external** | Admite escenarios de implementación de aplicaciones. |

<sup>1</sup> [!INCLUDE [big-data-cluster-root-user](../includes/big-data-cluster-root-user.md)]

> [!TIP]
> Esta es una forma de ver los servicios con **kubectl**, pero también se puede usar el comando `azdata bdc endpoint list` para ver estos puntos de conexión. Para obtener más información, vea [Obtención de los puntos de conexión del clúster de macrodatos](deployment-guidance.md#endpoints).

## <a name="get-service-details"></a>Obtención de detalles del servicio

Ejecute este comando para obtener una descripción detallada de un servicio en la salida en formato JSON. Se incluirán detalles como las etiquetas, el selector, la IP, la IP externa (si el servicio es de tipo LoadBalancer), el puerto, etc.

```bash
kubectl describe service <service_name> -n <namespace_name>
```

En el ejemplo siguiente se recuperan los detalles del servicio **master-svc-external**.

```bash
kubectl describe service master-svc-external -n mssql-cluster
```

## <a name="copy-files"></a><a id="copy"></a> Copia de archivos

Si necesita copiar archivos del contenedor en el equipo local, use el comando `kubectl cp` con la siguiente sintaxis:

```bash
kubectl cp <pod_name>:<source_file_path> -c <container_name> -n <namespace_name> <target_local_file_path>
```

Del mismo modo, puede usar `kubectl cp` para copiar archivos del equipo local en un contenedor específico.

```bash
kubectl cp <source_local_file_path> <pod_name>:<target_container_path> -c <container_name>  -n <namespace_name>
```

### <a name="copy-files-from-a-container"></a><a id="copyfrom"></a> Copia de archivos desde un contenedor

En el ejemplo siguiente se copian los archivos de registro de SQL Server del contenedor en la ruta de acceso `~/temp/sqlserverlogs` del equipo local (en este ejemplo, el equipo local es un cliente Linux):

```bash
kubectl cp master-0:/var/opt/mssql/log -c mssql-server -n mssql-cluster ~/tmp/sqlserverlogs
```

### <a name="copy-files-into-container"></a><a id="copyinto"></a> Copia de archivos en un contenedor

En el ejemplo siguiente se copia el archivo **AdventureWorks2016CTP3.bak** del equipo local en el contenedor de instancias maestras de SQL Server (`mssql-server`) en el pod `master-0`. El archivo se copia en el directorio `/tmp` del contenedor. 

```bash
kubectl cp ~/Downloads/AdventureWorks2016CTP3.bak master-0:/tmp -c mssql-server -n mssql-cluster
```

## <a name="force-delete-a-pod"></a><a id="forcedelete"></a> Forzamiento de la eliminación de un pod
 
No se recomienda forzar la eliminación de un pod. Pero, para probar la disponibilidad, la resistencia o la persistencia de los datos, puede eliminar un pod para simular un error del pod con el comando `kubectl delete pods`.

```bash
kubectl delete pods <pod_name> -n <namespace_name> --grace-period=0 --force
```

En el ejemplo siguiente se elimina el pod del bloque de almacenamiento `storage-0-0`:

```bash
kubectl delete pods storage-0-0 -n mssql-cluster --grace-period=0 --force
```

## <a name="get-pod-ip"></a><a id="getip"></a> Obtención de la IP del pod
 
Para solucionar problemas, es posible que tenga que obtener la IP del nodo en el que se está ejecutando actualmente un pod. Para obtener la dirección IP, use el comando `kubectl get pods` con la siguiente sintaxis:

```bash
kubectl get pods <pod_name> -o yaml -n <namespace_name> | grep hostIP
```

En el ejemplo siguiente se obtiene la dirección IP del nodo en el que se ejecuta el pod `master-0`:

```bash
kubectl get pods master-0 -o yaml -n mssql-cluster | grep hostIP
```

## <a name="kubernetes-dashboard"></a>Panel de Kubernetes

Puede iniciar el panel de Kubernetes para obtener información adicional sobre el clúster. En las secciones siguientes se explica cómo iniciar el panel de Kubernetes en AKS y de Kubernetes arrancado mediante kubeadm.
 
### <a name="start-dashboard-when-cluster-is-running-in-aks"></a>Inicio del panel cuando el clúster se está ejecutando en AKS

Para iniciar el panel de Kubernetes, ejecute:

```bash
az aks browse --resource-group <azure_resource_group> --name <aks_cluster_name>
```

> [!Note]
> Si recibe el siguiente error: *Unable to listen on port 8001: All listeners failed to create with the following errors: Unable to create listener: Error listen tcp4 127.0.0.1:8001: >bind: Only one usage of each socket address (protocol/network address/port) is normally permitted. Unable to create listener: Error listen tcp6: address [[::1]]:8001: missing port in >address error: Unable to listen on any of the requested ports: [{8001 9090}]* (No se puede escuchar en el puerto 8001: no se pudieron crear los clientes de escucha debido a los siguientes errores: no se puede crear el cliente de escucha: error al escuchar tcp4 127.0.0.1:8001: >bind: solo se permite un uso de cada dirección de socket (protocolo/dirección de red/puerto). No se puede crear el cliente de escucha: error al escuchar tcp6: dirección [[::1]]:8001: falta un puerto, error en la dirección; no se puede escuchar en ninguno de los puertos solicitados: [{8001 9090}]), asegúrese de que no había iniciado el panel en otra ventana.

Al iniciar el panel en el explorador, es posible que reciba advertencias de permisos debido a que RBAC está habilitado de forma predeterminada en los clústeres de AKS y la cuenta de servicio que usa el panel no tiene permisos suficientes para acceder a todos los recursos (por ejemplo, *pods is forbidden: User "system:serviceaccount:kube-system:kubernetes-dashboard" cannot list pods in the namespace "default"* [pods prohibidos: el usuario "system:serviceaccount:kube-system:kubernetes-dashboard" no puede enumerar pods en el espacio de nombres "default"]). Ejecute el siguiente comando para conceder los permisos necesarios a `kubernetes-dashboard` y luego reinicie el panel:

```bash
kubectl create clusterrolebinding kubernetes-dashboard -n kube-system --clusterrole=cluster-admin --serviceaccount=kube-system:kubernetes-dashboard
```

### <a name="start-dashboard-when-kubernetes-cluster-is-bootstrapped--using-kubeadm"></a>Inicio del panel cuando el clúster de Kubernetes se arranca mediante kubeadm

Para obtener instrucciones detalladas sobre cómo implementar y configurar el panel en el clúster de Kubernetes, consulte [la documentación de Kubernetes](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/). Para iniciar el panel de Kubernetes, ejecute este comando:

```bash
kubectl proxy
```

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.
