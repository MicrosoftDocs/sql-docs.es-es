---
title: Recursos implementados
titleSuffix: SQL Server Big Data Clusters
description: Una descripción de los pods que normalmente se implementan en un clúster de macrodatos de SQL Server.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 02/11/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 20f31d6cbf4bcdbbdc666b50dea626f79e298db1
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2021
ms.locfileid: "100343931"
---
# <a name="resources-deployed-with-big-data-cluster"></a>Recursos implementados con el clúster de macrodatos

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describen los recursos que implementa un clúster de macrodatos de SQL Server.

Un clúster de macrodatos implementa pods en función del perfil de implementación. Para obtener más información, vea [Configuraciones predeterminadas](deployment-guidance.md#configfile). 

En este artículo se describen los pods implementados con el perfil `aks-dev-test-ha` y se incluye un grupo de Spark. Consulte Kubernetes para ver los pods implementados en el clúster. En el ejemplo siguiente se devuelve una lista de pods en un espacio de nombres específico.

```bash
kubectl get pods -n <namespace>
```

Reemplace `<namespace>` por el nombre del clúster de macrodatos. 

Para obtener más información, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md#configfile).

En el diagrama siguiente se muestran los componentes implementados en un clúster de macrodatos:

:::image type="content" source="media/big-data-cluster-overview/architecture-diagram-overview.png" alt-text="big-data-cluster-diagram":::

Para obtener información sobre la arquitectura, vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md).

## <a name="deployed-pods"></a>Pods implementados

En la tabla siguiente se enumeran los pods implementados en un clúster de macrodatos.

|Nombre  |Área|
|---------|---------|
|`control-<nnnn>`        |[Control](#control)|
|`controldb-<#>`         |[Control](#control)|
|`controlwd-<nnnn>`      |[Control](#control)|
|`logsdb-<#>`            |[Control](#control)|
|`logsui-<nnnn>`         |[Control](#control)|
|`metricsdb-<#>`         |[Control](#control)|
|`metricsdc-<nnnn>`      |[Control](#control)|
|`metricsui-<nnnn>`      |[Control](#control)|
|`mgmtproxy-<nnnn>`      |[Control](#control)|
|`zookeeper-<#>`         |[Control](#control)|
|`dns-<nnnn>`            |[Control](#control)|
|`master-<#n>`           |[Instancia principal](#master-instance)|
|`operator-<nnnn>`       |[Instancia principal](#master-instance)
|`compute-<#n>-<#m>`     |[Grupo de proceso](#compute-pool)|
|`data-<#>-<#>`          |[Grupo de datos](#data-pool) |
|`storage-<#>-<#>`       |[Grupo de almacenamiento](#storage-pool)|
|`nmnode-<#>-<#>`        |[Grupo de almacenamiento](#storage-pool)|
|`sparkhead-<#>`         |[Grupo de almacenamiento](#storage-pool)|
|`appproxy-<#m>`         |[Grupo de aplicaciones](#application-pool)|
|`gateway-<#>`           |[Servicio de puerta de enlace](#gateway-service)|

No todos los pods se incluyen en todos los clústeres de BDC. Las implementaciones con alta disponibilidad o la integración con Active Directory incluyen pods específicos. 

### <a name="high-availability-specific-pods"></a>Pods específicos de alta disponibilidad:

- `operator-<nnnn>`
- `zookeeper-<#>`

### <a name="active-directory-specific-pods"></a>Pods específicos de Active Directory:

- `dns-<nnnn>`

En las secciones siguientes se describen los pods y se enumeran los contenedores de cada pod.

## <a name="control"></a>Control

Los pods de control proporcionan el servicio de control.

|Nombre del pod |Count| Tipo del controlador de Kubernetes | Contenedores |
|--------|----|------|--------|-------|
|`control-#`|1| ReplicaSet |- `controller`<br><br>- `security-support`<br><br>- `fluentbit`
|`controldb`|1| StatefulSet |- `mssql-server`<br><br>- `fluentbit`
|`controlwd`|1|  ReplicaSet |- `controlwatchdog`
|`logsdb-#` |1| StatefulSet |- `elasticsearch`
|`logsui`   |1| ReplicaSet |- `kibana`
|`metricsdb-#`|1| StatefulSet |- `influxdb`
|`metricsdc`|1 por nodo de Kubernetes. | DaemonSet |- `telegraf` |
|`metricsui-nnnn`|1| ReplicaSet |- `grafana` |
|`mgmtproxy-nnnn`|1| ReplicaSet |- `service-proxy`<br><br>- `fluentbit`|
|`dns-nnnn`|0 o 1 para la integración de Active Directory| ReplicaSet |- `dns`<br><br>- `fluentbit`|

## <a name="master-instance"></a>Instancia principal

`master-<#n>` es la instancia maestra de SQL Server.

- Administra el grupo de datos a través de DDL.
- Manipula los datos del grupo de datos a través de DML.
- Descarga la ejecución de consultas de análisis en el grupo de datos.

|Nombre del pod |Count| Tipo del controlador de Kubernetes | Contenedores |
|--------|----|------|--------|
|`master-<#n>`|1 o más para alta disponibilidad.| StatefulSet|- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`<br><br>- `mssql-ha-supervisor` <sup>*</sup>|
|`operator`<sup>*</sup>| 0 o 1 para alta disponibilidad | ReplicaSet |- `mssql-ha-operator`

<sup>*</sup> Solo implementaciones de alta disponibilidad. El operador implementa y registra la definición de recursos personalizada para SQL Server y los recursos del grupo de disponibilidad. Cuando se implementa el operador, se registra a sí mismo como un cliente de escucha de notificaciones sobre los recursos de SQL Server que se implementan en el clúster de Kubernetes. `mssql-ha-supervisor` admite el grupo de disponibilidad.

Cada pod `master` contiene una instancia de SQL Server. Un implementación de alta disponibilidad incluye tres pods. Cada pod incluye una instancia de SQL Server con bases de datos en un grupo de disponibilidad AlwaysOn de SQL Server.

Incluya pods adicionales en el momento de la implementación, en función de la carga de trabajo. 

## <a name="compute-pool"></a>Grupo de proceso

El grupo de proceso proporciona una instancia de SQL Server para el cálculo.

|Nombre del pod |Count| Tipo del controlador de Kubernetes | Contenedores |
|--------|----|------|--------|
|`compute-<#n>-<#m>`|1 o más.| StatefulSet |- `mssql-server`<br><br>- `fluentbit`<br><br>- `collectd`

- `#n` identifica el grupo de proceso.
- `#m` identifica el id. de instancia en el grupo.

Las instancias de SQL Server del grupo de proceso no tienen estado. Solo necesitan almacenamiento para `tempdb`.

Incluya pods adicionales en el momento de la implementación, en función de la carga de trabajo. 

## <a name="data-pool"></a>Grupo de datos

El grupo de datos proporciona instancias de SQL Server para el almacenamiento y el proceso.

|Nombre del pod |Count| Tipo del controlador de Kubernetes | Contenedores |
|--------|----|------|--------|-------|
|`data-<#n>-<#m>` | 0 o más | StatefulSet | -` mssql-server` <br><br>- `fluentbit`<br><br>- `collectd`|

- `#n` identifica el grupo de datos.
- `#m` identifica el id. de instancia en el grupo.

Incluya pods adicionales en el momento de la implementación, en función de la carga de trabajo.

## <a name="storage-pool"></a>Bloque de almacenamiento

El bloque de almacenamiento proporciona ingesta de datos a través de Spark, almacenamiento en HDFS, acceso a datos a través de HDFS y puntos de conexión de SQL Server.

|Nombre del pod |Count| Tipo del controlador de Kubernetes | Contenedores |
|--------|----|------|--------|
|`storage-0-#`|1 o más. Incluya pods adicionales en el momento de la implementación, en función de la carga de trabajo. | StatefulSet |- `hadoop`<br><br>- `mssql-server`<br><br>- `fluentbit`<br><br>
|`nmnode-0-#`|1 o más para alta disponibilidad| StatefulSet |- `hadoop`<br><br>- `fluentbit`
|`sparkehead-#`|1 o más para alta disponibilidad| StatefulSet |- `hadoop-yarn-jobhistory`<br><br>- `hadoop-livy-sparkhistory`<br><br>- `hadoop-hivemetastore`<br><br>-- `fluentbit`
|`zookeeper`|0 o 3 para alta disponibilidad. | StatefulSet |- `zookeeper`<br><br>- `fluentbit`

## <a name="application-pool"></a>Grupo de aplicaciones

El grupo de aplicaciones se incluye en algunos de los perfiles de configuración de pruebas. El grupo de aplicaciones hospeda los proxies del servicio de aplicación que se definen al implementar las aplicaciones para los clústeres de macrodatos. 

`appproxy` es una API web que se sitúa delante de las aplicaciones del grupo de aplicaciones. Autentica a los usuarios y, después, enruta las solicitudes a las aplicaciones.

|Nombre del pod | Tipo del controlador de Kubernetes | Contenedores  |
|--------|----|------|
|`appproxy`| ReplicaSet |- `app-service-proxy`<br><br>- `fluentbit`

Vea [¿Qué es la implementación de la aplicación en un clúster de macrodatos?](concept-application-deployment.md) para obtener más información.

Incluya pods adicionales en el momento de la implementación, en función de la carga de trabajo. 

## <a name="gateway-service"></a>Servicio de puerta de enlace

Los servicios de puerta de enlace proporcionan la puerta de enlace Knox a Spark, HDFS, [Yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html) y las interfaces de usuario de Yarn y Spark.

|Nombre del pod | Tipo del controlador de Kubernetes | Contenedores |
|--------|-----|-----|
|`gateway-<#>`| StatefulSet |- `knox`<br><br>- `fluentbit`

Solo se admite una puerta de enlace.

## <a name="open-source-container-references"></a>Referencias de contenedor de código abierto

Para obtener información sobre proyectos y versiones específicos de código abierto, consulte el artículo [Referencia de software de código abierto](reference-open-source-software.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea los recursos siguientes:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)
- [Taller: Arquitectura de los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] de Microsoft](https://github.com/Microsoft/sqlworkshops/tree/master/sqlserver2019bigdataclusters)
- [Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md#configfile)
