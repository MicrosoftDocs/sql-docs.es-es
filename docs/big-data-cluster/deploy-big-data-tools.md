---
title: Instalación de herramientas de macrodatos
titleSuffix: SQL Server big data clusters
description: Aprenda a instalar herramientas usadas con clústeres de macrodatos de SQL Server 2019.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 98226c45b9bc677386870e432ee835803bde32a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047531"
---
# <a name="install-sql-server-2019-big-data-tools"></a>Instalación de las herramientas de macrodatos de SQL Server 2019

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

En este artículo se describen las herramientas de cliente que se deben instalar para crear, administrar y usar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]. En la siguiente sección se proporciona una lista de herramientas y vínculos a instrucciones de instalación. Antes de implementar un clúster de macrodatos, configure las herramientas marcadas como obligatorias en Windows o Linux.

## <a name="big-data-cluster-tools"></a>Herramientas de clúster de macrodatos

En la tabla siguiente se indican herramientas comunes de clúster de macrodatos y cómo instalarlas:

| Herramienta | Obligatorio | Descripción | Instalación |
|---|---|---|---|
| `python` | Sí | Python es un lenguaje de programación de alto nivel, interpretado y orientado a objetos con semántica dinámica. Muchas partes de los clústeres de macrodatos para SQL Server usan Python. | [Instalación de Python](#python)|
| [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] | Sí | Herramienta de línea de comandos para instalar y administrar un clúster de macrodatos. | [Instalación](../azdata/install/deploy-install-azdata.md) |
| `kubectl`<sup>1</sup> | Sí | Herramienta de línea de comandos para supervisar el clúster de Kubernetes subyacente ([Más información](https://kubernetes.io/docs/tasks/tools/install-kubectl/)). | [Windows](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-with-powershell-from-psgallery) \| [Linux](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-using-native-package-management) |
| **Azure Data Studio** | Sí | Herramienta gráfica multiplataforma para consultar SQL Server. | [Instalación](../azure-data-studio/download-azure-data-studio.md) |
| **Extensión de virtualización de datos** | Sí | Extensión de Azure Data Studio que proporciona un asistente para la virtualización de datos. | [Instalación](../azure-data-studio/extensions/data-virtualization-extension.md) |
| **CLI de Azure**<sup>2</sup> | Para AKS | Interfaz de línea de comandos moderna para administrar servicios de Azure. Se usa con implementaciones de clústeres de macrodatos de AKS ([Más información](/cli/azure/)). | [Instalación](/cli/azure/install-azure-cli) |
| **mssql-cli** | Opcional | Interfaz de línea de comandos moderna para consultar SQL Server ([Más información](../tools/mssql-cli.md)). | [Windows](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/windows.md) \| [Linux](https://github.com/dbcli/mssql-cli/blob/master/doc/installation/linux.md) |
| **sqlcmd** | Para algunos scripts | Herramienta de línea de comandos heredada para consultar SQL Server ([Más información](../tools/sqlcmd-utility.md)). Es posible que tenga que instalar Microsoft ODBC Driver 11 para SQL Server antes de instalar el paquete SQLCMD. | [Windows](https://www.microsoft.com/download/details.aspx?id=36433) \| [Linux](../linux/sql-server-linux-setup-tools.md) |
| `curl` <sup>3</sup> | Para algunos scripts | Herramienta de línea de comandos para transferir datos con direcciones URL. | [Windows](https://curl.haxx.se/windows/) \| Linux: instalación de paquete curl |
| `oc` | Obligatoria para las implementaciones de Red Hat OpenShift y Red Hat OpenShift en Azure. |`oc` es la interfaz de la línea de comandos (CLI) de Open Shift. | [Instalación de la CLI](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html#installing-the-cli)

<sup>1</sup> Debe usar la versión 1.13 de `kubectl` o posterior. Además, la versión de `kubectl` debe ser más o menos una versión secundaria del clúster de Kubernetes. Si quiere instalar una versión concreta en el cliente de `kubectl`, vea [Instalación de `kubectl` binario mediante curl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-curl) (en Windows 10, use cmd.exe y no Windows PowerShell para ejecutar curl).

> [!TIP]
> Para usar `kubectl` con un clúster implementado previamente en Azure Kubernetes Service (AKS), debe establecer el contexto del clúster con el siguiente comando de la CLI de Azure:
>
>    ```azurecli
>    az aks get-credentials --name <aks_cluster_name> --resource-group <azure_resource_group_name>
>    ```

<sup>2</sup> Debe usar la CLI de Azure versión 2.0.4 o posterior. Ejecute `az --version` para encontrar la versión si fuera necesario.

<sup>3</sup> Si se ejecuta en Windows 10, `curl` ya está en la RUTA DE ACCESO cuando se ejecuta desde un símbolo del sistema cmd. En otras versiones de Windows, descargue `curl` con el vínculo y colóquelo en la RUTA DE ACCESO.

## <a name="which-tools-are-required"></a>Herramientas necesarias

En la tabla anterior se proporcionan todas las herramientas comunes que se usan con clústeres de macrodatos. Las herramientas necesarias dependen del escenario. Pero, en general, las siguientes herramientas son más importantes para administrar, conectarse y consultar el clúster:

- [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]
- `kubectl`
- **Azure Data Studio**
- **Extensión de virtualización de datos**

Las herramientas restantes solo son necesarias en determinados escenarios. La **CLI de Azure** se puede usar para administrar servicios de Azure asociados a las implementaciones de AKS. **mssql-cli** es una herramienta opcional, aunque útil, que permite conectarse a la instancia maestra de SQL Server del clúster y ejecutar consultas desde la línea de comandos. Y **sqlcmd** y `curl` son necesarios si piensa instalar datos de ejemplo con el script de GitHub.

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

## <a name="download-and-install-azure-data-studio"></a>Descarga e instalación de Azure Data Studio

Azure Data Studio proporciona funcionalidades y características específicamente para clústeres de macrodatos de SQL Server.

[Obtenga la versión más reciente de Azure Data Studio](../azure-data-studio/download-azure-data-studio.md).

Para más información sobre la última versión, consulte las [notas de la versión](./release-notes-big-data-cluster.md).

## <a name="next-steps"></a>Pasos siguientes

Después de configurar las herramientas, implemente un clúster de macrodatos de SQL Server 2019 en Kubernetes en la nube o el entorno local. Para obtener más información, vea los siguientes artículos de implementación:

- [Inicio rápido: Implementación de un clúster de macrodatos de SQL Server en Azure Kubernetes Service (AKS)](quickstart-big-data-cluster-deploy.md)
- [Implementación de [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md)

Para obtener más información sobre los clústeres de macrodatos, vea [¿Qué son [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)