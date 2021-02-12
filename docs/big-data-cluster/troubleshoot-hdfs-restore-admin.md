---
title: Restauración de permisos de HDFS
titleSuffix: SQL Server Big Data Cluster
description: Restaure los derechos de administrador de HDFS.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d914b2006656162b233351815e238f282d73e913
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045535"
---
# <a name="restore-hdfs-permissions"></a>Restauración de permisos de HDFS

Las modificaciones de las listas de control de acceso (ACL) de HDFS pueden haber afectado a las carpetas `/system` y `/tmp` en HDFS. La causa más probable de la modificación de las ACL es que un usuario manipule manualmente las ACL de la carpeta. No se admite la modificación directa de los permisos en las carpetas /system y /tmp/logs.

## <a name="symptom"></a>Síntoma

Un trabajo de Spark se envía a través de ADS y se producen un error de inicialización de SparkContext y una excepción AccessControlException.

```
583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=<UserAccount>, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

La interfaz de usuario de Yarn muestra el identificador de la aplicación con el estado ELIMINADO.

Al intentar escribir en la carpeta como el usuario del dominio, también se produce un error. Puede proba con el ejemplo siguiente:

```bash
kinit <UserAccount>
hdfs dfs -touch /system/spark-events/test
hdfs dfs -rm /system/spark-events/test
```

## <a name="cause"></a>Causa

Las ACL de HDFS se modificaron para el grupo de seguridad del dominio de usuario de BDC. Las modificaciones posibles incluyen las ACL para las carpetas /system y /tmp. No se admiten las modificaciones de estas carpetas.

Compruebe el efecto en los registros de Livy:

```
INFO utils.LineBufferedStream: YYYY-MM-DD-HH:MM:SS,858 INFO yarn.Client: Application report for application_1580771254352_0041 (state: ACCEPTED)
...
WARN rsc.RSCClient: Client RPC channel closed unexpectedly.
INFO interactive.InteractiveSession: Failed to ping RSC driver for session <ID>. Killing application
```

La interfaz de usuario de YARN muestra las aplicaciones con el estado ELIMINADO en el identificador de la aplicación.

Para obtener la causa principal del cierre de la conexión RPC, compruebe el registro de aplicación de YARN de la aplicación correspondiente a la aplicación. En el ejemplo anterior, se refiere a `application_1580771254352_0041`. Use `kubectl` para conectarse al pod sparkhead-0 y ejecute este comando:

El siguiente comando consulta el registro de YARN de la aplicación.

```bash
yarn logs -applicationId application_1580771254352_0041
```

En los resultados siguientes, se deniega el permiso para el usuario. 

```
YYYY-MM-DD-HH:MM:SS,583 ERROR spark.SparkContext: Error initializing SparkContext.
org.apache.hadoop.security.AccessControlException: Permission denied: user=user1, access=WRITE, inode="/system/spark-events":sph:BDCAdmin:drwxr-xr-x
```

La causa puede ser que el usuario de BDC se agregó de forma recursiva a la carpeta raíz de HDFS. Esto puede haber afectado a los permisos predeterminados.

## <a name="resolution"></a>Solución

Restaure los permisos con el siguiente script: use `kinit` con derechos de administrador:

```bash
hdfs dfs -chmod 733 /system/spark-events
hdfs dfs -setfacl --set default:user:sph:rwx,default:other::--- /system/spark-events
hdfs dfs -setfacl --set default:user:app-setup:r-x,default:other::--- /system/appdeploy
hadoop fs -chmod 733 /tmp/logs
hdfs dfs -setfacl --set default:user:yarn:rwx,default:other::--- /tmp/logs
```
