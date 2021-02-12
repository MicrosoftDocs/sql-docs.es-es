---
title: Objetos de Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Describe objetos de Active Directory creados para los clústeres de macrodatos de SQL Server.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 347129c6627bce865ffd4962a9cf2cb3b0beb862
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039786"
---
# <a name="auto-generated-active-directory-objects"></a>Objetos de Active Directory generados automáticamente

En este artículo se describen las cuentas de Active Directory (AD) y los grupos que crea SQL Server durante la implementación de un clúster de macrodatos (BDC).

## <a name="accounts--groups"></a>Cuentas y grupos

Las cuentas de usuario y los grupos se generan en la [unidad organizativa (UO)](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts) proporcionada durante la implementación del clúster.

Cada una de las cuentas representa un servicio en el clúster de macrodatos. Las cuentas poseen los nombres de entidad de seguridad de servicio (SPN) necesarios para cada servicio. Los SPN propiedad de cada cuenta se pueden mostrar mediante el comando [setspn](https://social.technet.microsoft.com/wiki/contents/articles/717.service-principal-names-spn-setspn-syntax.aspx).

La implementación genera automáticamente los nombres de cuenta y de grupo. A partir de SQL Server 2019 CU5, el prefijo del nombre de cuenta o de grupo es el nombre del espacio de nombres de la implementación (nombre del clúster de macrodatos). Si el nombre del clúster es `bdc` para los elementos de este artículo, reemplace `<prefix>` por `bdc` para identificar sus cuentas.

El sufijo de pod (-x) denota un identificador de pod variable a continuación. Los nombres siguientes no incluyen un prefijo de variable proporcionado por el usuario durante la implementación.

El nombre de cuenta clásico se aplica a las implementaciones que usan versiones anteriores a SQL Server 2019 CU5, así como a las implementaciones realizadas con la opción "useSubdomain" establecida en false en la configuración de seguridad.

| Nombre de cuenta o de grupo|Más información|
|----|---|
|`<prefix>-ctrl`|[Cuenta de servicio del controlador](#controller-service-account)|
|`<prefix>-ngxm`|[Cuenta de servicio del proxy del servicio de supervisión](#monitoring-service-proxy-service-account)|
|`<prefix>-ldap`|[Búsqueda de usuario LDAP](#ldap-lookup-user)|
|`<prefix>-sqc0-x/<prefix>-sqc0`|[Usuario de SQL Server del grupo de proceso](#compute-pool-sql-server-user)|
|`<prefix>-dmc0-x`|[Usuario de DMS del almacenamiento de datos del grupo de proceso](#compute-pool-data-warehouse-dms-user)|
|`<prefix>-dec0-x`|[Usuario del motor de almacenamiento de datos del grupo de proceso](#compute-pool-data-warehouse-engine-user)|
|`<prefix>-sqd0`|[Usuario de SQL Server del grupo de datos](#data-pool-sql-server-user)|
|`<prefix>-sqs0`|[Usuario de SQL Server del bloque de almacenamiento](#storage-pool-sql-server-user)|
|`<prefix>-ynt0-x`|[Usuario del servicio de administrador de nodos de Yarn del bloque de almacenamiento](#storage-pool-yarn-node-manager-service-user)|
|`<prefix>-htt0`|[Usuario del servicio HTTP del bloque de almacenamiento](#storage-pool-http-service-user)|
|`<prefix>-hdt0`|[Usuario del servicio de nodo de datos de HDFS del bloque de almacenamiento](#storage-pool-hdfs-datanode-service-user)|
|`<prefix>-hdnn`|[Usuario del servicio de nodo de nombre HDFS](#hdfs-name-node-service-user)|
|`<prefix>-htnn`|[Usuario del servicio HTTP de nodo de nombre HDFS](#hdfs-name-node-http-service-user)|
|`<prefix>-kmnn-x`|[Usuario del servicio KMS del nodo de nombre](#name-node-kms-service-user)|
|`<prefix>-jnzk-x`|[Usuarios del servicio JournalNode de Zookeeper](#zookeeper-journalnode-service-users)|
|`<prefix>-htzk`|[Usuario del servicio HTTP de Zookeeper](#zookeeper-http-service-user)|
|`<prefix>-yrsh-x`|[Usuario del servicio de administrador de recursos de Yarn de Sparkhead](#sparkhead-yarn-resource-manager-service-user)|
|`<prefix>-htsh`|[Usuario HTTP de Sparkhead](#sparkhead-http-user)|
|`<prefix>-shsh-x`|[Usuario del servicio de historial de Spark de Sparkhead](#sparkhead-spark-history-service-user)|
|`<prefix>-lvsh-x`|[Usuario del servicio Livy de Sparkhead](#sparkhead-livy-service-user)|
|`<prefix>-hvsh-x`|[Usuario del servicio Hive de Sparkhead](#sparkhead-hive-service-user)|
|`<prefix>-yns0-x`|[Usuario del servicio de administrador de nodos de Yarn del grupo de Spark](#spark-pool-yarn-node-manager-service-user)|
|`<prefix>-hts0`|[Usuario HTTP de administrador de nodos de Yarn del grupo de Spark](#spark-pool-yarn-node-manager-http-user)|
|`<prefix>-knox-x`|[Usuario de puerta de enlace de Knox](#knox-gateway-user)|
|`<prefix>-htgw`|[Usuario HTTP de puerta de enlace de Knox](#knox-gateway-http-user)|
|`<prefix>-apst`|[Usuario de configuración de la aplicación](#app-setup-user)|
|`<prefix>-dmsvc`|[Grupo de servicio DMS de almacenamiento de datos](#data-warehouse-dms-service-group)|
|`<prefix>-desvc`|[Grupo de servicio del motor de almacenamiento de datos](#data-warehouse-engine-service-group)|

En la sección siguiente se proporcionan más detalles sobre cada una de estas cuentas. Para obtener información sobre los grupos, vaya a [Grupos](#groups).

## <a name="controller-management--ldap-related-accounts"></a>Controlador, administración y cuentas relacionadas con LDAP

### <a name="controller-service-account"></a>Cuenta de servicio del controlador

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`control`|
|Nombre del pod|`control-x`|
|Nombre del contenedor|`controller`|
|Nombre del servicio|`controller`|
|Nombre de cuenta (sin prefijo)| `ctrl`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-ctrl`|
|Nombre de cuenta clásico|`ctrl-controller`|

### <a name="monitoring-service-proxy-service-account"></a>Cuenta de servicio del proxy del servicio de supervisión

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`mgmtproxy`|
|Nombre del pod|`mgmtproxy-x`|
|Nombre del contenedor|`service-proxy`|
|Nombre del servicio|`nginx`|
|Cuenta (sin prefijo)| `ngxm`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-ngxm`|
|Nombre de cuenta clásico|`nginx-mgmtproxy`|

### <a name="ldap-lookup-user"></a>Búsqueda de usuario LDAP

La usan los servicios de Grafana y Hadoop para buscar usuarios a través de LDAP.

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`metricsui`|
|Nombre del pod|`metricsui-x`|
|Nombre del contenedor|`grafana`|
|Nombre del servicio|`grafana`|
|Nombre de cuenta (sin prefijo)| `ldap`|
|Nombre de cuenta (con prefijo del espacio de nombres)|`<prefix>-ldap`|
|Nombre de cuenta clásico|`ldap-user`|

## <a name="master-pool-accounts"></a>Cuentas del grupo maestro

### <a name="master-pool-sql-server-user"></a>Usuario de SQL Server del grupo maestro

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`master`|
|Nombre del pod|`master-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`mssql`|
|Nombre de cuenta (sin prefijo)| `sqmp-x/sqmp`|
|Nombre de cuenta (con prefijo del espacio de nombres)|`<prefix>-sqmp-x/<prefix>-sqmp`|
|Nombre de cuenta clásico|`mssql-master-x`|

### <a name="master-pool-data-warehouse-dms-user"></a>Usuario de DMS del almacenamiento de datos del grupo maestro

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`master`|
|Nombre del pod|`master-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dwdms`|
|Cuenta (sin prefijo)| `dmmp-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-dmmp-x`|
|Nombre de cuenta clásico|`dwdms-master-x`|

### <a name="master-pool-data-warehouse-engine-user"></a>Usuario del motor del almacenamiento de datos del grupo maestro

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`master`|
|Nombre del pod|`master-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dweng`|
|Cuenta (sin prefijo)| `demp`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-demp-x`|
|Nombre de cuenta clásico|`dweng-master-x`|

## <a name="compute-pool-accounts"></a>Cuentas de grupo de proceso

### <a name="compute-pool-sql-server-user"></a>Usuario de SQL Server del grupo de proceso

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`compute-0`|
|Nombre del pod|`compute-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`mssql`|
|Cuenta (sin prefijo)| `sqc0-x/sqlc0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-sqc0-x/<prefix>-sqc0`|
|Nombre de cuenta clásico|`mssql-compute-0-x`|

### <a name="compute-pool-data-warehouse-dms-user"></a>Usuario de DMS del almacenamiento de datos del grupo de proceso

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`compute-0`|
|Nombre del pod|`compute-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dwdms`|
|Cuenta (sin prefijo)| `dmc0-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-dmc0-x`|
|Nombre de cuenta clásico|`dwdms-compute-0-x`|

### <a name="compute-pool-data-warehouse-engine-user"></a>Usuario del motor de almacenamiento de datos del grupo de proceso

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`compute-0`|
|Nombre del pod|`compute-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dweng`|
|Cuenta (sin prefijo)| `dec0-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-dec0-x`|
|Nombre de cuenta clásico|`dweng-compute-0-x`|

## <a name="data-pool-accounts"></a>Cuentas del grupo de datos

### <a name="data-pool-sql-server-user"></a>Usuario de SQL Server del grupo de datos

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`data-0`|
|Nombre del pod|`data-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`mssql`|
|Cuenta (sin prefijo)| `sqd0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-sqd0`|
|Nombre de cuenta clásico|`mssql-data-0`|

## <a name="storage-pool-accounts"></a>Cuentas del bloque de almacenamiento

### <a name="storage-pool-sql-server-user"></a>Usuario de SQL Server del bloque de almacenamiento

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`storage-0`|
|Nombre del pod|`storage-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`mssql`|
|Cuenta (sin prefijo)| `sqs0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-sqs0`|
|Nombre de cuenta clásico|`mssql-storage-0`|

### <a name="storage-pool-yarn-node-manager-service-user"></a>Usuario del servicio de administrador de nodos de Yarn del bloque de almacenamiento

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`storage-0`|
|Nombre del pod|`storage-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`Yarn Node Manager`|
|Cuenta (sin prefijo)| `ynt0-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-ynt0-x`|
|Nombre de cuenta clásico|`yarnnm-storage-0-x`|

### <a name="storage-pool-http-service-user"></a>Usuario del servicio HTTP del bloque de almacenamiento

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`storage-0`|
|Nombre del pod|`storage-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`HDFS Datanode`|
|Cuenta (sin prefijo)| `hdt0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-hdt0`|
|Nombre de cuenta clásico|`http-storage-0`|

### <a name="storage-pool-hdfs-datanode-service-user"></a>Usuario del servicio de nodo de datos de HDFS del bloque de almacenamiento

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`storage-0`|
|Nombre del pod|`storage-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`HDFS Datanode`|
|Cuenta (sin prefijo)| `hdt0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-hdt0`|
|Nombre de cuenta clásico|`hdfsdn-storage-0`|

## <a name="hdfs-accounts"></a>Cuentas de HDFS

### <a name="hdfs-name-node-service-user"></a>Usuario del servicio de nodo de nombre HDFS

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`nmnode-0`|
|Nombre del pod|`nmnode-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`HDFS Namenode`|
|Cuenta (sin prefijo)| `hdnn`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-hdnn`|
|Nombre de cuenta clásico|`hdfsnn-nmnode`|

### <a name="hdfs-name-node-http-service-user"></a>Usuario del servicio HTTP de nodo de nombre HDFS

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`nmnode-0`|
|Nombre del pod|`nmnode-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`HDFS Namenode`|
|Cuenta (sin prefijo)| `htnn`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-htnn`|
|Nombre de cuenta clásico|`http-nmnode`|

## <a name="kms-accounts"></a>Cuentas de KMS

### <a name="name-node-kms-service-user"></a>Usuario del servicio KMS del nodo de nombre

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`nmnode-0`|
|Nombre del pod|`nmnode-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`KMS`|
|Cuenta (sin prefijo)| `kmnn-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-kmnn-x`|
|Nombre de cuenta clásico|`kms-nmnode-x`|

## <a name="zookeeper-accounts"></a>Cuentas de Zookeeper

### <a name="zookeeper-journalnode-service-users"></a>Usuarios del servicio JournalNode de Zookeeper

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`zookeeper`|
|Nombre del pod|`zookeeper-x`|
|Nombre del contenedor|`zookeeper`|
|Nombre del servicio|`Journal node`|
|Cuenta (sin prefijo)| `jnzk-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-jnzk-x`|
|Nombre de cuenta clásico|`jn-zookeeper-x`|

### <a name="zookeeper-http-service-user"></a>Usuario del servicio HTTP de Zookeeper

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`zookeeper`|
|Nombre del pod|`zookeeper-x`|
|Nombre del contenedor|`zookeeper`|
|Nombre del servicio|`Zookeeper`|
|Cuenta (sin prefijo)| `htzk`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-htzk`|
|Nombre de cuenta clásico|`http-zookeeper`|

## <a name="spark-related-accounts"></a>Cuentas relacionadas con Spark

### <a name="sparkhead-yarn-resource-manager-service-user"></a>Usuario del servicio de administrador de recursos de Yarn de Sparkhead

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`sparkhead`|
|Nombre del pod|`sparkhead-x`|
|Nombre del contenedor|`hadoop-yarn-jobhistory`|
|Nombre del servicio|`Yarn Resource Manager`|
|Cuenta (sin prefijo)| `yrsh-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-yrsh-x`|
|Nombre de cuenta clásico|`yarnrm-sparkhead-x`|

### <a name="sparkhead-http-user"></a>Usuario HTTP de Sparkhead

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`sparkhead`|
|Nombre del pod|`sparkhead-x`|
|Nombre del contenedor|`*`|
|Nombre del servicio|`*`|
|Cuenta (sin prefijo)| `htsh`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-htsh`|
|Nombre de cuenta clásico|`http-sparkhead`|

### <a name="sparkhead-spark-history-service-user"></a>Usuario del servicio de historial de Spark de Sparkhead

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`sparkhead`|
|Nombre del pod|`sparkhead-x`|
|Nombre del contenedor|`hadoop-livy-sparkhistory`|
|Nombre del servicio|`Spark History Server`|
|Cuenta (sin prefijo)| `shsh-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-shsh-x`|
|Nombre de cuenta clásico|`sph-sparkhead-x`|

### <a name="sparkhead-livy-service-user"></a>Usuario del servicio Livy de Sparkhead

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`sparkhead`|
|Nombre del pod|`sparkhead-x`|
|Nombre del contenedor|`hadoop-livy-sparkhistory`|
|Nombre del servicio|`Livy`|
|Cuenta (sin prefijo)| `lvsh-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-lvsh-x`|
|Nombre de cuenta clásico|`livy-sparkhead-x`|

### <a name="sparkhead-hive-service-user"></a>Usuario del servicio Hive de Sparkhead

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`sparkhead`|
|Nombre del pod|`sparkhead-x`|
|Nombre del contenedor|`hadoop-hivemetastore`|
|Nombre del servicio|`Hive Metastore`|
|Cuenta (sin prefijo)| `hvsh-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-hvsh-x`|
|Nombre de cuenta clásico|`hive-sparkhead-x`|

### <a name="spark-pool-yarn-node-manager-service-user"></a>Usuario del servicio de administrador de nodos de Yarn del grupo de Spark

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`spark-0`|
|Nombre del pod|`spark-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`Yarn Node Manager`|
|Cuenta (sin prefijo)| `yns0-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-yns0-x`|
|Nombre de cuenta clásico|`yarnnm-spark-0-x`|

### <a name="spark-pool-yarn-node-manager-http-user"></a>Usuario HTTP de administrador de nodos de Yarn del grupo de Spark

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`spark-0`|
|Nombre del pod|`spark-0-x`|
|Nombre del contenedor|`hadoop`|
|Nombre del servicio|`Yarn Node Manager`|
|Cuenta (sin prefijo)| `hts0`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-hts0`|
|Nombre de cuenta clásico|`http-spark-0`|

## <a name="knox-accounts"></a>Cuentas de Knox

### <a name="knox-gateway-user"></a>Usuario de puerta de enlace de Knox

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`gateway`|
|Nombre del pod|`gateway-x`|
|Nombre del contenedor|`knox`|
|Nombre del servicio|`Knox`|
|Cuenta (sin prefijo)| `knox-x`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-knox-x`|
|Nombre de cuenta clásico|`knox-gateway-x`|

### <a name="knox-gateway-http-user"></a>Usuario HTTP de puerta de enlace de Knox

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`gateway`|
|Nombre del pod|`gateway-x`|
|Nombre del contenedor|`knox`|
|Nombre del servicio|`Knox`|
|Cuenta (sin prefijo)| `htgw`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-htgw`|
|Nombre de cuenta clásico|`http-gateway`|

## <a name="app-accounts"></a>Cuentas de aplicación

### <a name="app-setup-user"></a>Usuario de configuración de la aplicación

|Object|Nombre de cuenta|
|---|---|
|Nombre del conjunto de escalado|`appproxy`|
|Nombre del pod|`appproxy-x`|
|Nombre del contenedor|`App Service Proxy`|
|Nombre del servicio|`nginx`|
|Cuenta (sin prefijo)| `apst`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-apst`|
|Nombre de cuenta clásico|`app-setup`|

## <a name="groups"></a>Grupos

Los siguientes grupos se crean en la unidad organizativa proporcionada por el usuario. Los miembros de los grupos son los usuarios que se crearon anteriormente para los servicios correspondientes.

### <a name="data-warehouse-dms-service-group"></a>Grupo de servicio DMS de almacenamiento de datos

|Object|Nombre del grupo|
|---|---|
|Nombre del conjunto de escalado|`master/compute-0`|
|Nombre del pod|`master-x/compute-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dwdms`|
|Grupo (sin prefijo)| `dmsvc`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-dmsvc`|
|Nombre de cuenta clásico|`dwdms-service`|

### <a name="data-warehouse-engine-service-group"></a>Grupo de servicio del motor de almacenamiento de datos

|Object|Nombre del grupo|
|---|---|
|Nombre del conjunto de escalado|`master/compute-0`|
|Nombre del pod|`master-x/compute-0-x`|
|Nombre del contenedor|`mssql-server`|
|Nombre del servicio|`dweng`|
|Grupo (sin prefijo)| `desvc`|
|Cuenta (con prefijo del espacio de nombres)|`<prefix>-desvc`|
|Nombre de cuenta clásico|`desvc`|

## <a name="next-steps"></a>Pasos siguientes

[Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el modo de Active Directory](active-directory-deploy.md)

[Implementación de varios [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en el mismo dominio de Active Directory](active-directory-deployment-background.md)
