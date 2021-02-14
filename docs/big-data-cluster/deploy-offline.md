---
title: Implementación sin conexión
titleSuffix: SQL Server big data clusters
description: Obtenga información sobre cómo realizar una implementación sin conexión de un clúster de macrodatos de SQL Server 2019 y cómo cargar imágenes de contenedor en un repositorio privado.
author: mihaelablendea
ms.author: mihaelab
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 9cbe53718818116ae9a0c435a1cf985c132f759b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047281"
---
# <a name="perform-an-offline-deployment-of-a-sql-server-big-data-cluster"></a>Realización de una implementación sin conexión de un clúster de macrodatos de SQL Server

En este artículo se explica cómo realizar una implementación sin conexión de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. Los clústeres de macrodatos deben tener acceso a un repositorio de Docker desde el que extraer imágenes de contenedor. Una instalación sin conexión es aquella en la que las imágenes necesarias se encuentran en un repositorio privado de Docker. Ese repositorio privado se usa como origen de imágenes de una nueva implementación.

## <a name="prerequisites"></a>Prerequisites

- Motor de Docker 1.8 o versiones posteriores en cualquier distribución de Linux admitida o Docker para Mac y Windows. Para obtener más información, consulte [Instalar Docker](https://docs.docker.com/engine/installation/).

## <a name="load-images-into-a-private-repository"></a>Cargar imágenes en un repositorio privado

En los pasos siguientes se explica cómo extraer las imágenes de contenedor de clúster de macrodatos del repositorio de Microsoft y luego insertarlas en el repositorio privado.

> [!TIP]
> En los pasos siguientes se explica el proceso. Pero para simplificar la tarea, puede usar el [script automatizado](#automated) en lugar de ejecutar estos comandos manualmente.

1. Extraiga las imágenes de contenedor de clúster de macrodatos mediante la repetición del siguiente comando. Reemplace `<SOURCE_IMAGE_NAME>` por cada [nombre de imagen](#images). Reemplace `<SOURCE_DOCKER_TAG>` por la etiqueta de la versión del clúster de macrodatos, como **2019-GDR1-ubuntu-16.04**.  

   ```PowerShell
   docker pull mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG>
   ```

1. Inicie sesión en el registro de Docker privado de destino.

   ```PowerShell
   docker login <TARGET_DOCKER_REGISTRY> -u <TARGET_DOCKER_USERNAME> -p <TARGET_DOCKER_PASSWORD>
   ```

1. Etiquete las imágenes locales con el siguiente comando para cada imagen:

   ```PowerShell
   docker tag mcr.microsoft.com/mssql/bdc/<SOURCE_IMAGE_NAME>:<SOURCE_DOCKER_TAG> <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```

1. Inserte las imágenes locales en el repositorio privado de Docker:

   ```PowerShell
   docker push <TARGET_DOCKER_REGISTRY>/<TARGET_DOCKER_REPOSITORY>/<SOURCE_IMAGE_NAME>:<TARGET_DOCKER_TAG>
   ```
 
> [!WARNING]
> No modifique las imágenes del clúster de macrodatos una vez insertadas en el repositorio privado. Si realiza una implementación con imágenes modificadas, se producirá una configuración de clúster de macrodatos no admitida.


### <a name="big-data-cluster-container-images"></a><a id="images"></a> Imágenes de contenedor de clúster de macrodatos

Las siguientes imágenes de contenedor de clúster de macrodatos son necesarias para una instalación sin conexión:
- **mssql-app-service-proxy**
- **mssql-control-watchdog**
- **mssql-controller**
- **mssql-dns**
- **mssql-hadoop**
- **mssql-mleap-serving-runtime**
- **mssql-mlserver-py-runtime**
- **mssql-mlserver-r-runtime**
- **mssql-monitor-collectd**
- **mssql-monitor-elasticsearch**
- **mssql-monitor-fluentbit**
- **mssql-monitor-grafana**
- **mssql-monitor-influxdb**
- **mssql-monitor-kibana**
- **mssql-monitor-telegraf**
- **mssql-security-knox**
- **mssql-security-support**
- **mssql-server-controller**
- **mssql-server-data**
- **mssql-ha-operator**
- **mssql-ha-supervisor**
- **mssql-service-proxy**
- **mssql-ssis-app-runtime**


## <a name="automated-script"></a><a id="automated"></a> Script automatizado

Puede usar un script de Python automatizado que extraiga automáticamente todas las imágenes de contenedor necesarias y las inserte en un repositorio privado.

> [!NOTE]
> Python es un requisito previo para usar el script. Para obtener más información sobre cómo instalar Python, vea la [documentación de Python](https://wiki.python.org/moin/BeginnersGuide/Download).

1. En Bash o PowerShell, descargue el script con **curl**:

   ```PowerShell
   curl -o push-bdc-images-to-custom-private-repo.py "https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/sql-big-data-cluster/deployment/offline/push-bdc-images-to-custom-private-repo.py"
   ```

1. Luego ejecute el script con uno de los siguientes comandos:

   **Windows:**

   ```PowerShell
   python push-bdc-images-to-custom-private-repo.py
   ```

   **Linux:**

   ```bash
   sudo python push-bdc-images-to-custom-private-repo.py
   ```

1. Siga los mensajes para especificar la información del repositorio de Microsoft y el repositorio privado. Una vez completado el script, todas las imágenes necesarias deben encontrarse en el repositorio privado.

1. Siga [estas instrucciones](deployment-custom-configuration.md#docker) para obtener información sobre cómo personalizar el archivo de configuración de la implementación de `control.json` para usar el repositorio y el registro de contenedor. Tenga en cuenta que debe establecer las variables de entorno `DOCKER_USERNAME` y `DOCKER_PASSWORD` antes de la implementación para permitir el acceso a su repositorio privado.

## <a name="install-tools-offline"></a>Instalación de herramientas sin conexión

Las implementaciones de clústeres de macrodatos requieren varias herramientas, como **Python**, [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] y **kubectl**. Siga estos pasos para instalar estas herramientas en un servidor sin conexión.

### <a name="install-python-offline"></a><a id="python"></a> Instalación de Python sin conexión

1. En un equipo con acceso a Internet, descargue uno de los siguientes archivos comprimidos que contienen Python:

   | Sistema operativo | Descargar |
   |---|---|
   | Windows | [https://go.microsoft.com/fwlink/?linkid=2074021](https://go.microsoft.com/fwlink/?linkid=2074021) |
   | Linux   | [https://go.microsoft.com/fwlink/?linkid=2065975](https://go.microsoft.com/fwlink/?linkid=2065975) |
   | OSX     | [https://go.microsoft.com/fwlink/?linkid=2065976](https://go.microsoft.com/fwlink/?linkid=2065976) |

1. Copie el archivo comprimido en el equipo de destino y extráigalo en una carpeta de su elección.

1. Solo para Windows, ejecute `installLocalPythonPackages.bat` desde esa carpeta y pase la ruta de acceso completa a la misma carpeta como un parámetro.

   ```PowerShell
   installLocalPythonPackages.bat "C:\python-3.6.6-win-x64-0.0.1-offline\0.0.1"
   ```

### <a name="install-azdata-offline"></a><a id="azdata"></a> Instalación de azdata sin conexión

1. En una máquina con acceso a Internet y [Python](https://wiki.python.org/moin/BeginnersGuide/Download), ejecute el comando siguiente para descargar todos los paquetes [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] en la carpeta actual.

   ```PowerShell
   pip download -r https://aka.ms/azdata
   ```

1. Copie los paquetes descargados y el archivo `requirements.txt` en la máquina de destino.

1. Ejecute el siguiente comando en el equipo de destino y especifique la carpeta en la que ha copiado los archivos anteriores.

   ```PowerShell
   pip install --no-index --find-links <path-to-packages> -r <path-to-requirements.txt>
   ```

### <a name="install-kubectl-offline"></a><a id="kubectl"></a> Instalación de kubectl sin conexión

Para instalar **kubectl** en un equipo sin conexión, siga estos pasos.

1. Use **curl** para descargar **kubectl** en la carpeta que prefiera. Para obtener más información, vea [Instalación del binario de kubectl mediante curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl).

1. Copie la carpeta en el equipo de destino.

## <a name="deploy-from-private-repository"></a>Implementación desde un repositorio privado

Para implementar desde el repositorio privado, siga los pasos de la [guía de implementación](deployment-guidance.md), pero use un archivo de configuración de implementación personalizado que especifique la información del repositorio privado de Docker. Los siguientes comandos de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] muestran cómo cambiar la configuración de Docker de un archivo de configuración de la implementación personalizado denominado `control.json`:

```bash
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.repository=<your-docker-repository>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.registry=<your-docker-registry>"
azdata bdc config replace --config-file custom/control.json --json-values "$.spec.docker.imageTag=<your-docker-image-tag>"
```

La implementación le pedirá el nombre de usuario y la contraseña de Docker, aunque también puede especificarlos en las variables de entorno `DOCKER_USERNAME` y `DOCKER_PASSWORD`.

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre las implementaciones de clústeres de macrodatos, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md).
