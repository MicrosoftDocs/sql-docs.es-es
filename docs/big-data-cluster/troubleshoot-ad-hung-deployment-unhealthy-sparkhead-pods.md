---
title: 'Problema de implementación en modo de Active Directory: pods `sparkhead` incorrectos'
titleSuffix: SQL Server Big Data Cluster
description: Solución de problemas de implementaciones sin respuesta de un clúster de macrodatos de SQL Server en un dominio de Active Directory con pods `sparkhead` incorrectos.
author: macarv-ms
ms.author: macarv
ms.reviewer: mikeray
ms.date: 08/20/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8631595ad8d33e69b300b42b873e65ce39385cc6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044272"
---
# <a name="ad-mode-deployment-hangs---unhealthy-sparkhead-pods"></a>Problema de implementación en modo de Active Directory: pods `sparkhead` incorrectos

La implementación en el modo de Active Directory (AD) se inmoviliza. Compruebe los síntomas para ver si la causa es una entrada de zona de búsqueda inversa que falta para el controlador de dominio en las diferentes redes de los nodos del clúster.

## <a name="symptom"></a>Síntoma

Ha empezado a implementar BDC con el modo de AD, pero la implementación está bloqueada y no avanza.

En el ejemplo siguiente se muestran los resultados de la implementación en un shell de Bash.

```output
Starting cluster deployment.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Waiting for cluster controller to start.
Cluster controller endpoint is available at bdc-control.corpnet.contoso.com:30080, 10.166.6.77:30080.
Waiting for control plane to be ready after 5 minutes.
Cluster control plane is ready.
Cluster is not ready after 15 minutes. Check controller logs for more details.
Data pool is ready.
Storage pool is ready.
Compute pool is ready.
Master pool is ready.
Cluster is not ready after 30 minutes. Check controller logs for more details.
...
...
```

Compruebe los pods implementados actualmente.

```bash
kubectl get pods -n mssql-cluster
```

Los resultados indican que todos los pods se han implementado, pero la implementación no confirma que el proceso se haya realizado correctamente.

```output
NAME              READY   STATUS    RESTARTS   AGE 
appproxy-c7f2l    2/2     Running   0          3d13h 
compute-0-0       3/3     Running   0          3d13h 
control-88dgt     3/3     Running   0          3d13h 
controldb-0       2/2     Running   0          3d13h 
controlwd-zzkxz   1/1     Running   0          3d13h 
data-0-0          3/3     Running   0          3d13h 
data-0-1          3/3     Running   0          3d13h 
dns-xkdhh         2/2     Running   0          3d13h 
gateway-0         2/2     Running   0          3d13h 
logsdb-0          1/1     Running   0          3d13h 
logsui-qz8qq      1/1     Running   0          3d13h 
master-0          4/4     Running   0          3d13h 
master-1          4/4     Running   0          3d13h 
master-2          4/4     Running   0          3d13h 
metricsdb-0       1/1     Running   0          3d13h 
metricsdc-xezf7   1/1     Running   0          3d13h 
metricsdc-qdjkh   1/1     Running   0          3d13h 
metricsui-mr34w   1/1     Running   0          3d13h 
mgmtproxy-kz5gg   2/2     Running   0          3d13h 
nmnode-0-0        2/2     Running   1          3d13h 
nmnode-0-1        2/2     Running   0          3d13h 
operator-42ffv    1/1     Running   0          3d13h 
sparkhead-0       4/4     Running   0          3d13h 
sparkhead-1       4/4     Running   0          3d13h 
storage-0-0       4/4     Running   0          3d13h 
storage-0-1       4/4     Running   0          3d13h 
storage-0-2       4/4     Running   0          3d13h 
zookeeper-0       2/2     Running   0          3d13h 
zookeeper-1       2/2     Running   0          3d13h 
zookeeper-2       2/2     Running   0          3d13h 
```

Inspeccione el estado de los servicios HDFS y Spark. Busque errores de pods `sparkhead`.

## <a name="check-the-hdfs-and-spark-services"></a>Comprobación de los servicios HDFS y Spark 

En Azure Data Studio (ADS), conéctese al controlador y vea el panel del clúster de macrodatos. Compruebe si los servicios HDFS y Spark tienen pods `sparkhead` incorrectos.

![Los pods "sparkhead" de los servicios HDFS y Spark son incorrectos](./media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/hdfs_spark_unhealthy_sparkhead_pods.png)

Extraiga los registros y búsquelos.

`\mssql-cluster\control-<identifier>\controller\control-<identifier>-controller-stdout.log`.

> [!TIP]
> Hay varias formas de recopilar los registros. En lugar de copiar los registros con [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], puede usar un cuaderno en Azure Data Studio.
> En Azure Data Studio, conéctese al clúster de Kubernetes y ejecute un cuaderno de solución de problemas adecuado. A continuación, se muestran ejemplos de cuadernos.
>
> - TSG027: observación de la implementación del clúster
> - TSG061: obtención del final de todos los registros de contenedor de los pods del espacio de nombres del BDC
> - TSG001: Ejecución de `azdata copy-logs`
>
  
## <a name="inspect-the-logs"></a>Inspección de los registros

Busque el registro. En el ejemplo siguiente se apunta a un registro de implementación de controlador.

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`

```output
StatefulSet sparkhead is not healthy: 
{{Pod sparkhead-0 is unhealthy: 
{Container hadoop-yarn-jobhistory is unhealthy: 
{Found error properties: 
{Property: jobhistoryserver.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-0.corpnet.contoso.com:19888/ws/v1/history: dial tcp 10.244.2.33:19888: connect: connection refused'}}} 
{Container hadoop-livy-sparkhistory is unhealthy: 
{Found error properties: 
{Property: sparkhistory.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-0.corpnet.contoso.com:18480: dial tcp 10.244.2.33:18480: connect: connection refused'}}}, 
{Container hadoop-hivemetastore is unhealthy: 
{Found error properties: 
{Property: hivemetastorehttp.readiness, Details: 'Health module returned error state. error: Post https://sparkhead-0.corpnet.contoso.com:9084/api/hms: dial tcp 10.244.2.33:9084: connect: connection refused'}}}}}, 
  
{{Pod sparkhead-1 is unhealthy: 
{Container hadoop-yarn-jobhistory is unhealthy: 
{Found error properties: 
{Property: jobhistoryserver.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-1.corpnet.contoso.com:19888/ws/v1/history: dial tcp 10.244.1.24:19888: connect: connection refused'}}}, 
{Container hadoop-livy-sparkhistory is unhealthy: 
{Found error properties: 
{Property: sparkhistory.readiness, Details: 'Health module returned error state. error: Head https://sparkhead-1.corpnet.contoso.com:18480: dial tcp 10.244.1.24:18480: connect: connection refused'}}}, 
{Container hadoop-hivemetastore is unhealthy: 
{Found error properties: 
{Property: hivemetastorehttp.readiness, Details: 'Health module returned error state. error: Post https://sparkhead-1.corpnet.contoso.com:9084/api/hms: dial tcp 10.244.1.24:9084: connect: connection refused'}}}}} 
```

Inspeccione los pods `sparkhead` y preste atención a los registros del contenedor. En este ejemplo se examina `sparkhead-0`.

```output
sparkhead-0\hadoop-hivemetastore\supervisor\log\hivemetastorehttp-stderr---supervisor-pZ1gdb 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-0.corpnet.contoso.com/10.244.2.36:9000 after 8 failover attempts. Trying to failover after sleeping for 13518ms. 
  
sparkhead-0\hadoop-yarn-jobhistory\supervisor\log\jobhistoryserver-stderr---supervisor-GvebR8 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-1.corpnet.contoso.com/10.244.1.30:9000 after 5 failover attempts. Trying to failover after sleeping for 11416ms. 
  
sparkhead-0\hadoop-livy-sparkhistory\supervisor\log\livy-stderr---supervisor-XiHB1w 
  
YYYY-MM-DD HH:MM:SS.ms INFO retry.RetryInvocationHandler: org.apache.hadoop.ipc.RemoteException(org.apache.hadoop.ipc.StandbyException): Operation category READ is not supported in state standby. Visit https://s.apache.org/sbnn-error 
at org.apache.hadoop.hdfs.server.namenode.ha.StandbyState.checkOperation(StandbyState.java:98) 
at org.apache.hadoop.hdfs.server.namenode.NameNode$NameNodeHAContext.checkOperation(NameNode.java:1998) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.checkOperation(FSNamesystem.java:1502) 
at org.apache.hadoop.hdfs.server.namenode.FSNamesystem.getFileInfo(FSNamesystem.java:3227) 
at org.apache.hadoop.hdfs.server.namenode.NameNodeRpcServer.getFileInfo(NameNodeRpcServer.java:1158) 
at org.apache.hadoop.hdfs.protocolPB.ClientNamenodeProtocolServerSideTranslatorPB.getFileInfo(ClientNamenodeProtocolServerSideTranslatorPB.java:983) 
at org.apache.hadoop.hdfs.protocol.proto.ClientNamenodeProtocolProtos$ClientNamenodeProtocol$2.callBlockingMethod(ClientNamenodeProtocolProtos.java) 
at org.apache.hadoop.ipc.ProtobufRpcEngine$Server$ProtoBufRpcInvoker.call(ProtobufRpcEngine.java:527) 
at org.apache.hadoop.ipc.RPC$Server.call(RPC.java:1036) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:978) 
at org.apache.hadoop.ipc.Server$RpcCall.run(Server.java:906) 
at java.security.AccessController.doPrivileged(Native Method) 
at javax.security.auth.Subject.doAs(Subject.java:422) 
at org.apache.hadoop.security.UserGroupInformation.doAs(UserGroupInformation.java:1729) 
at org.apache.hadoop.ipc.Server$Handler.run(Server.java:2876) 
, while invoking ClientNamenodeProtocolTranslatorPB.getFileInfo over nmnode-0-1.corpnet.contoso.com/10.244.1.30:9000 after 1 failover attempts. Trying to failover after sleeping for 1401ms. 
```

## <a name="cause"></a>Causa

Falta la entrada de zona de búsqueda inversa para el controlador de dominio en el servidor DNS del DC para la red Kubernetes. En este ejemplo, la entrada que falta es `cni0 10.244`. Los contenedores de pods `sparkhead` intentaban usar la dirección IP 10.244.1.30:9000 para llegar a nnnode-0-1, pero el DNS no pudo resolverlo.

:::image type="content" source="media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/missing_reverse_lookup_zone_entry_for_domain_controller.png" alt-text="Falta la entrada de zona de búsqueda inversa para el controlador de dominio":::

## <a name="resolution"></a>Resolución

Agregue la entrada de DNS inversa (registro PTR) que falta para la zona a la que se hace referencia en los registros. En este ejemplo, se agregó 244.10.

:::image type="content" source="media/troubleshoot-ad-hung-deployment-unhealthy-sparkhead-pods/missing_reverse_lookup_zone_entry_for_domain_controller_add.png" alt-text="Agregar entrada de zona de búsqueda inversa para el controlador de dominio":::

> [!NOTE]
> Asegúrese de que haya una entrada de DNS inversa (registro PTR) para el propio controlador de dominio registrada en el servidor DNS para todas y cada una de las redes de los nodos del clúster.

## <a name="next-steps"></a>Pasos siguientes

[Comprobación de la entrada de DNS inverso (registro PTR) para el controlador de dominio](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
