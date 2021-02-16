---
title: Alta disponibilidad para contenedores de SQL Server
description: Obtenga información sobre la alta disponibilidad para contenedores de SQL Server. Además, obtenga información sobre la implementación de un contenedor con SQL Server en Kubernetes.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/09/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-2017||>=sql-server-linux-2017'
ms.openlocfilehash: 168b1e11896ac29cf1d34037b1a28d7421fcfcc3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061460"
---
# <a name="high-availability-for-sql-server-containers"></a>Alta disponibilidad para contenedores de SQL Server

Cree y administre las instancias de SQL Server de forma nativa en Kubernetes.

Implemente SQL Server en contenedores de Docker administrados por [Kubernetes](https://kubernetes.io/). En Kubernetes, un contenedor con una instancia de SQL Server se puede recuperar automáticamente en caso de error en un nodo de clúster.

SQL Server 2017 introduce una imagen de Docker que se puede implementar en Kubernetes. Puede configurar la imagen con una notificación de volumen persistente (PVC) de Kubernetes. Kubernetes supervisa el proceso de SQL Server en el contenedor. Si se produce un error en el proceso, el pod, el contenedor o el nodo, Kubernetes arranca automáticamente otra instancia y vuelve a conectar al almacenamiento.

## <a name="container-with-sql-server-instance-on-kubernetes"></a>Contenedor con instancia de SQL Server en Kubernetes

Kubernetes 1.6 y posterior admite [*clases de almacenamiento*](https://kubernetes.io/docs/concepts/storage/storage-classes/), [*notificaciones de volumen persistente*](https://kubernetes.io/docs/concepts/storage/storage-classes/#persistentvolumeclaims) y el [*tipo de volumen de disco de Azure*](https://github.com/kubernetes/examples/tree/master/staging/volumes/azure_disk). 

En esta configuración, Kubernetes desempeña el rol de orquestador de contenedores. 

![Diagrama de clúster de SQL Server de Kubernetes](media/tutorial-sql-server-containers-kubernetes/kubernetes-sql.png)

En el diagrama anterior, `mssql-server` es una instancia de SQL Server (contenedor) en un [*pod*](https://kubernetes.io/docs/concepts/workloads/pods/pod/). Un [conjunto de réplicas](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/) garantiza que el pod se recupere automáticamente tras un error de nodo. Las aplicaciones se conectan al servicio. En este caso, el servicio representa un equilibrador de carga que hospeda una dirección IP que permanece igual tras un error de `mssql-server`.

Kubernetes orquesta los recursos del clúster. Cuando se produce un error en un nodo que hospeda un contenedor de instancia de SQL Server, arranca un nuevo contenedor con una instancia de SQL Server y lo asocia al mismo almacenamiento persistente.

SQL Server 2017 y posterior admite contenedores en Kubernetes.

Para crear un contenedor en Kubernetes, vea [Implementar un contenedor de SQL Server en Kubernetes](tutorial-sql-server-containers-kubernetes.md)

## <a name="next-steps"></a>Pasos siguientes

Para implementar contenedores de SQL Server en Azure Kubernetes Service (AKS), vea estos ejemplos:
* [Implementación de SQL Server en contenedor de Docker](./sql-server-linux-docker-container-deployment.md)
* [Implementar un contenedor de SQL Server en Kubernetes](tutorial-sql-server-containers-kubernetes.md)