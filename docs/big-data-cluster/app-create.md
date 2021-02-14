---
title: Implementación de aplicaciones con azdata
titleSuffix: SQL Server Big Data Clusters
description: Implemente un script de Python o R como una aplicación en un clúster de macrodatos de SQL Server 2019.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9d475edb720d329d5fb52f7bd75c055fc3a8494a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047995"
---
# <a name="how-to-deploy-an-app-on-sql-server-big-data-clusters"></a>Cómo implementar una aplicación en Clústeres de macrodatos de SQL Server

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Las aplicaciones implementadas en Clústeres de macrodatos (BDC) de SQL Server no solo se benefician de muchas ventajas, como la capacidad de cálculo del clúster, sino que también tienen acceso a datos masivos que están disponibles en el clúster. Mejora drásticamente el rendimiento, ya que la aplicación se encuentra en el mismo clúster en el que residen los datos.

En este artículo se describe cómo implementar y administrar scripts de R y Python como una aplicación dentro de un clúster de macrodatos de SQL Server 2019.

## <a name="whats-new-and-improved"></a>Novedades y mejoras

- Una única utilidad de línea de comandos para administrar el clúster y la aplicación.
- Implementación simplificada de aplicaciones al tiempo que se proporciona un control granular sobre los archivos de especificación.
- Compatibilidad con el hospedaje de más tipos de aplicación: SQL Server Integration Services (SSIS) y MLeap.
- [Extensión de Visual Studio Code](app-deployment-extension.md) para administrar la implementación de aplicaciones.

Las aplicaciones se implementan y administran mediante [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. En este artículo se proporcionan ejemplos de cómo implementar aplicaciones desde la línea de comandos. Para obtener información sobre cómo usar esto en Visual Studio Code, consulte [Extensión de Visual Studio Code](app-deployment-extension.md).

Se admiten los siguientes tipos de aplicaciones:

- **Python**: uno de los lenguajes de programación generales más populares para varios roles, como ingenieros de datos, científicos de datos o ingenieros de DevOps, admite numerosos escenarios, como limpieza y transformación de datos, automatización, creación de prototipos y, en cierta medida, también se usa cada vez más para programar aplicaciones de nivel empresarial junto con plataformas de desarrollo web como Flask y Django para resolver diferentes requisitos empresariales.  
- **R**: otro lenguaje de programación popular para la ingeniería de datos y los científicos de datos. En comparación con Python, R es un lenguaje de programación con un enfoque más específico sobre la informática estadística y los gráficos.  
- **SQL Server Integration Services (SSIS)** : estas soluciones de integración de datos de alto rendimiento para compilar y depurar paquetes ETL, utilizan el formato de archivo de paquete de servicios de transformación de datos (DTSX), que es un formato de archivo basado en XML que almacena las instrucciones para el procesamiento de la migración de datos entre bases de datos y la integración de orígenes de datos externos.   
- **MLeap**: es un formato de serialización común y proporciona todo lo necesario para ejecutar y serializar canalizaciones SparkML y otras que se pueden cargar en tiempo de ejecución para procesar las tareas de puntuación de ML casi en tiempo real y próximas a los datos.  

## <a name="prerequisites"></a>Prerrequisitos

- [Clúster de macrodatos de SQL Server 2019](deployment-guidance.md)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)

## <a name="capabilities"></a>Capacidades

En SQL Server 2019, puede crear, eliminar, describir, inicializar, enumerar, ejecutar y actualizar la aplicación. En la tabla siguiente se describen los comandos de implementación de aplicaciones que puede usar con **azdata**.

|Get-Help |Descripción |
|:---|:---|
|`azdata login` | Iniciar sesión en un clúster de macrodatos SQL Server |
|`azdata app create` | Crea una aplicación. |
|`azdata app delete` | Elimina una aplicación. |
|`azdata app describe` | Describe una aplicación. |
|`azdata app init` | Inicio rápido de un nuevo esqueleto de la aplicación. |
|`azdata app list` | Muestra una lista de aplicaciones. |
|`azdata app run` | Ejecuta una aplicación. |
|`azdata app update`| Actualiza una aplicación. |

Puede obtener ayuda con el parámetro `--help`, como en el ejemplo siguiente:

```bash
azdata app create --help
```

En las siguientes secciones se describen estos comandos con más detalle.

## <a name="sign-in"></a>Iniciar sesión

Antes de implementar aplicaciones o interactuar con ellas, primero inicie sesión en el clúster de macrodatos de SQL Server con el comando `azdata login`. Especifique la dirección IP externa del servicio `controller-svc-external` (por ejemplo: `https://ip-address:30080`) junto con el nombre de usuario y la contraseña para el clúster.

```bash
azdata login --controller-endpoint https://<ip-address-of-controller-svc-external>:30080 --controller-username <user-name>
```

## <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Si usa AKS, debe ejecutar el siguiente comando en una ventana de bash o CMD para obtener la dirección IP del servicio `controller-svc-external`:


```bash
kubectl get svc controller-svc-external -n <name of your big data cluster>
```

## <a name="kubernetes-clusters-created-with-kubeadm"></a>Clústeres de Kubernetes creados con kubeadm

Ejecute el comando siguiente para obtener la dirección IP en la que iniciar sesión en el clúster.

```bash
kubectl get node --selector='node-role.kubernetes.io/master'
```

## <a name="create-an-app-skeleton"></a>Crear un esqueleto de aplicación

El comando **azdata app init** proporciona un scaffolding con los artefactos pertinentes necesarios para implementar una aplicación. En el ejemplo siguiente se crea add-app. Puede hacerlo con el siguiente comando.

```bash
azdata app init --name add-app --version v1 --template python
```

Se creará una carpeta denominada Hello.  Puede usar el comando `cd` en el directorio e inspeccionar los archivos generados en la carpeta. spec.yaml define la aplicación, como el nombre, la versión y el código fuente. Puede editar spec para cambiar el nombre, la versión, la entrada y las salidas.

Este es un ejemplo de salida del comando init que verá en la carpeta:

```
add-app.py
run-spec.yaml
spec.yaml
```

## <a name="create-an-app"></a>Creación de una aplicación

Para crear una aplicación, use [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] con el comando `app create`. Estos archivos residen localmente en el equipo desde el que va a crear la aplicación.

Use la sintaxis siguiente para crear una nueva aplicación en el clúster de macrodatos:

```bash
azdata app create --spec <directory containing spec file>
```

El comando siguiente muestra un ejemplo de cómo podría ser este comando:

```bash
azdata app create --spec ./addpy
```

Se supone que la aplicación está almacenada en la carpeta `addpy`. Esta carpeta también debe contener un archivo de especificación para la aplicación, denominado `spec.yaml`. Para más información, consulte la [página de implementación de aplicaciones](concept-application-deployment.md) en el archivo `spec.yaml`.

Para implementar esta aplicación de ejemplo, cree los archivos siguientes en un directorio denominado `addpy`:

- `add.py`. Copie el siguiente código de Python en este archivo:
   ```py
   #add.py
  def add(x, y):
    result = x+y
    return result
  result=add(x,y)
   ```
- `spec.yaml`. Copie el siguiente código en este archivo:
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

A continuación, ejecute el siguiente comando:

```bash
azdata app create --spec ./addpy
```

Para comprobar si la aplicación se ha implementado, use el comando list:

```bash
azdata app list
```

Si la implementación no se ha completado, `state` debería mostrar `WaitingforCreate`, como en el ejemplo siguiente:

```json
[
  {
    "name": "add-app",
    "state": "WaitingforCreate",
    "version": "v1"
  }
]
```

Una vez que la implementación se realiza correctamente, `state` debería cambiar al estado `Ready`:

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="list-an-app"></a>Enumerar una aplicación

El comando `app list` enumera las aplicaciones que se crearon correctamente.

El comando siguiente muestra todas las aplicaciones disponibles en el clúster de macrodatos:

```bash
azdata app list
```

Si especifica un nombre y una versión, se muestra la aplicación específica y su estado: Creating (en creación) o Ready (lista).

```bash
azdata app list --name <app_name> --version <app_version>
```

El ejemplo siguiente demuestra este comando:

```bash
azdata app list --name add-app --version v1
```

Debería ver una salida similar al ejemplo siguiente:

```json
[
  {
    "name": "add-app",
    "state": "Ready",
    "version": "v1"
  }
]
```

## <a name="delete-an-app"></a>Eliminar una aplicación

Para eliminar una aplicación del clúster de macrodatos, use la sintaxis siguiente:

```bash
azdata app delete --name add-app --version v1
```

## <a name="next-steps"></a>Pasos siguientes

Averigüe cómo integrar aplicaciones implementadas en [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en aplicaciones propias en [Ejecución de aplicaciones en clúster de macrodatos](app-run.md) y [Consumo de aplicaciones en clústeres de macrodatos](app-consume.md) para más información. También puede ver otros ejemplos en [Ejemplos de implementación de aplicaciones](https://aka.ms/sql-app-deploy).

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md) para obtener más información sobre los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)].