---
title: ¿Qué es la implementación de la aplicación?
titleSuffix: SQL Server Big Data Clusters
description: Obtenga información sobre cómo la implementación de aplicaciones proporciona interfaces para crear, administrar y ejecutar aplicaciones en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 8c1f6b4895e872289095411cfd8ecdaf1e2eb087
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047665"
---
# <a name="what-is-application-deployment-on-a-sql-server-big-data-cluster"></a>¿Qué es la implementación de la aplicación en un clúster de macrodatos de SQL Server?

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La implementación de la aplicación permite la implementación de aplicaciones en un clúster de macrodatos de SQL Server proporcionando interfaces para crear, administrar y ejecutar aplicaciones. Las aplicaciones implementadas en un clúster de macrodatos de SQL Server se benefician de la capacidad de cálculo del clúster y pueden tener acceso a los datos que están disponibles en el clúster. Esto aumenta la escalabilidad y el rendimiento de las aplicaciones, al tiempo que se administran las aplicaciones en las que residen los datos. Los tiempos de ejecución de aplicaciones compatibles en los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] son R, Python, SSIS, MLeap.

En las secciones siguientes se describe la arquitectura y las funciones de la implementación de la aplicación.

## <a name="application-deployment-architecture"></a>Arquitectura de implementación de la aplicación

La implementación de la aplicación consta de un controlador y controladores de tiempo de ejecución de la aplicación. Al crear una aplicación, se proporciona un archivo de especificación (`spec.yaml`). Este archivo `spec.yaml` contiene todo lo que el controlador necesita saber para implementar correctamente la aplicación. A continuación se muestra un ejemplo del contenido de `spec.yaml`:

```yaml
#spec.yaml
name: add-app #name of your python script
version: v1  #version of the app
runtime: Python #the language this app uses (R or Python)
src: ./add.py #full path to the location of the app
entrypoint: add #the function that will be called upon execution
replicas: 1  #number of replicas needed
poolsize: 1  #the pool size that you need your app to scale
inputs:  #input parameters that the app expects and the type
  x: int
  y: int
output: #output parameter the app expects and the type
  result: int
```

El controlador inspecciona el `runtime` especificado en el archivo `spec.yaml` y llama al controlador en tiempo de ejecución correspondiente. El controlador en tiempo de ejecución crea la aplicación. En primer lugar, se crea un conjunto ReplicaSet de Kubernetes que contiene uno o varios pods, cada uno de los cuales contiene la aplicación que se va a implementar. El número de pods se define mediante el parámetro `replicas` establecido en el archivo `spec.yaml` de la aplicación. Cada pod puede tener uno o más grupos. El número de grupos se define mediante el parámetro `poolsize` establecido en el archivo `spec.yaml`.

Esta configuración afecta a la cantidad de solicitudes que la implementación puede controlar en paralelo. El número máximo de solicitudes en un momento dado es igual a `replicas` veces `poolsize`. Si tiene 5 réplicas y 2 grupos por réplica, la implementación puede administrar 10 solicitudes en paralelo. Vea la imagen a continuación para obtener una representación gráfica de `replicas` y `poolsize`:

![Poolsize y replicas](media/big-data-cluster-create-apps/poolsize-vs-replicas.png)

Una vez que se ha creado el conjunto ReplicaSet y se han iniciado los pods, se crea un trabajo cron si se ha establecido `schedule` en el archivo `spec.yaml`. Por último, se crea un servicio de Kubernetes que se puede usar para administrar y ejecutar la aplicación (consulte a continuación).

Cuando se ejecuta una aplicación, el servicio Kubernetes de la aplicación envía por proxy las solicitudes a una réplica y devuelve los resultados.

## <a name="security-considerations-for-applications-deployments-on-openshift"></a><a id="app-deploy-security"></a> Consideraciones de seguridad para las implementaciones de aplicaciones en OpenShift

SQL Server 2019 CU5 habilita la compatibilidad con la implementación de clústeres de macrodatos en Red Hat OpenShift, así como un modelo de seguridad actualizado para clústeres de macrodatos, por lo que ya no se necesitan contenedores con privilegios. Aparte de que ya no necesiten privilegios, los contenedores se ejecutan como un usuario que no es de raíz de forma predeterminada para todas las implementaciones nuevas mediante SQL Server 2019 CU5.

En el momento del lanzamiento de la versión CU5, el paso de instalación de las aplicaciones implementadas con las interfaces de [implementación de la aplicación]() se seguirá ejecutando como usuario *raíz*. Esto es necesario, ya que durante la configuración se instalan paquetes adicionales que usará la aplicación. Otro código de usuario implementado como parte de la aplicación se ejecutará como usuario con pocos privilegios. 

Además, **CAP_AUDIT_WRITE** es una capacidad opcional necesaria para permitir la programación de aplicaciones SSIS mediante trabajos de Cron. Cuando el archivo de especificación YAML de la aplicación especifica una programación, la aplicación se desencadenará a través de un trabajo Cron, que necesita la capacidad adicional.  Como alternativa, la aplicación se puede desencadenar a petición con *azdata app run* a través de una llamada de servicio web, que no requiere la capacidad CAP_AUDIT_WRITE. 

> [!NOTE]
> El SCC personalizado en el [artículo de implementación de OpenShift](deploy-openshift.md) no incluye esta capacidad, ya que no es necesaria para una implementación predeterminada del clúster de macrodatos. Para habilitar esta capacidad, primero debe actualizar el archivo YAML de SCC personalizado para que incluya CAP_AUDIT_WRITE. 

```yml
...
allowedCapabilities:
- SETUID
- SETGID
- CHOWN
- SYS_PTRACE
- AUDIT_WRITE
...
```

## <a name="how-to-work-with-application-deployment"></a>Cómo trabajar con la implementación de la aplicación

Las dos interfaces principales de la implementación de la aplicación son las siguientes: 
- [Interfaz de línea de comandos[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](app-create.md)
- [Extensión de Visual Studio Code y Azure Data Studio](app-deployment-extension.md)

También es posible ejecutar una aplicación mediante un servicio web de RESTful. Para más información, consulte [Consumo de aplicaciones en clústeres de macrodatos](app-consume.md).

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo crear y ejecutar aplicaciones en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea lo siguiente:

- [Implementar aplicaciones con azdata](app-create.md)
- [Implementación de aplicaciones con la extensión de implementación de la aplicación](app-deployment-extension.md)
- [Consumo de aplicaciones en clústeres de macrodatos](app-consume.md)

Para obtener más información sobre [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], vea la siguiente introducción:

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)