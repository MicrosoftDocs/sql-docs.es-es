---
title: Actualización a una nueva versión
titleSuffix: SQL Server Big Data Clusters
description: Aprenda a actualizar clústeres de macrodatos de SQL Server a una nueva versión.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 09/02/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 48edd5e76a87a866a2090329f144c5a7dc0934a2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046895"
---
# <a name="how-to-upgrade-big-data-clusters-2019"></a>Cómo actualizar los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

La ruta de actualización depende de la versión actual del clúster de macrodatos (BDC) de SQL Server. Para actualizar desde una versión compatible, incluida la versión de distribución general (GDR), la actualización acumulativa (CU) o la actualización de ingeniería de corrección rápida (QFE), puede realizar una actualización local. No se admite la actualización local a partir de Customer Technology Preview (CTP) ni de la versión candidata para lanzamiento de BDC. Debe quitar y volver a crear el clúster. En las secciones siguientes se describen los pasos para cada escenario:

- [Actualización desde una versión compatible](#upgrade-from-supported-release)
- [Actualización de una implementación de BDC desde CTP o una versión candidata para lanzamiento](#update-a-bdc-deployment-from-ctp-or-release-candidate)

>[!NOTE]
>La primera versión compatible de los Clústeres de macrodatos es SQL Server 2019 GDR1.

## <a name="upgrade-release-notes"></a>Notas de la versión de la actualización

Antes de continuar, consulte las [notas de la versión de la actualización para ver los problemas conocidos](release-notes-big-data-cluster.md#known-issues).

## <a name="upgrade-from-supported-release"></a>Actualización desde una versión compatible

En esta sección se explica cómo actualizar un clúster de macrodatos de SQL Server desde una versión compatible (a partir de SQL Server 2019 GDR1) a una versión compatible más reciente.

1. Compruebe que no hay sesiones de Livy activas.

   Asegúrese de que no haya ninguna sesión de Livy activa o trabajos por lotes en ejecución en Azure Data Studio. Una manera fácil de confirmar esto es a través del comando `curl` o un explorador para solicitar estas direcciones URL:

    - `<your-gateway-endpoint>/gateway/default/livy/v1/sessions`
    - `<your-gateway-endpoint>/gateway/default/livy/v1/batches`

1. Haga una copia de seguridad de la instancia maestra de SQL Server.

1. Haga una copia de seguridad de HDFS.

   ```
   azdata bdc hdfs cp --from-path <path> --to-path <path>
   ```
   
   Por ejemplo: 

   ```
   azdata bdc hdfs cp --from-path hdfs://user/hive/warehouse/%%D --to-path ./%%D
   ```

1. Actualice [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   Siga las instrucciones para instalar [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. 
   - [Windows Installer](../azdata/install/deploy-install-azdata-installer.md)
   - [Linux con apt](../azdata/install/deploy-install-azdata-linux-package.md)
   - [Linux con yum](../azdata/install/deploy-install-azdata-yum.md)
   - [Linux con zypper](../azdata/install/deploy-install-azdata-zypper.md)

   >[!NOTE]
   >Si [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] se instaló con `pip`, debe quitarlo manualmente antes de realizar la instalación con Windows Installer o el administrador de paquetes de Linux.

1. Actualice el clúster de macrodatos.

   ```
   azdata bdc upgrade -n <clusterName> -t <imageTag> -r <containerRegistry>/<containerRepository>
   ```

   Por ejemplo, el script siguiente usa la etiqueta de imagen `2019-CU6-ubuntu-16.04`:

   ```
   azdata bdc upgrade -n bdc -t 2019-CU6-ubuntu-16.04 -r mcr.microsoft.com/mssql/bdc
   ```

>[!NOTE]
>Las etiquetas de imagen más recientes están disponibles en las [notas de la versión de los Clústeres de macrodatos de SQL Server 2019](release-notes-big-data-cluster.md).

>[!IMPORTANT]
>Si usa un repositorio privado para extraer previamente las imágenes para implementar o actualizar BDC, asegúrese de que las imágenes de compilación actuales, así como las imágenes de compilación de destino se encuentran en el repositorio privado. Esto permite una reversión correcta, si es necesario. Además, si ha cambiado las >credenciales del repositorio privado desde la implementación original, actualice las variables de entorno DOCKER_PASSWORD y >DOCKER_USERNAME correspondientes. No se admite la actualización mediante distintos repositorios privados para compilaciones actuales y de destino.

### <a name="increase-the-timeout-for-the-upgrade"></a>Aumento del tiempo de espera de la actualización

Se puede producir un tiempo de espera si no se actualizan determinados componentes en el tiempo asignado. En el código siguiente se muestra el aspecto del error:

   ```
   >azdata.EXE bdc upgrade --name <mssql-cluster>
   Upgrading cluster to version 15.0.4003

   NOTE: Cluster upgrade can take a significant amount of time depending on
   configuration, network speed, and the number of nodes in the cluster.

   Upgrading Control Plane.
   Control plane upgrade failed. Failed to upgrade controller.
   ```

Para aumentar los tiempos de expiración de una actualización, use los parámetros **--controller-timeout** y **--component-timeout** para especificar valores más altos al emitir la actualización. Esta opción solo está disponible a partir de la versión SQL Server 2019 CU2. Por ejemplo:

   ```bash
   azdata bdc upgrade -t 2019-CU6-ubuntu-16.04 --controller-timeout=40 --component-timeout=40 --stability-threshold=3
   ```
**--controller-timeout** designa el número de minutos que se esperará a que finalice la actualización del controlador o de la base de datos del controlador.
**--component-timeout** designa la cantidad de tiempo en que se debe completar cada fase posterior de la actualización.

Para aumentar los tiempos de espera de una actualización antes de la versión SQL Server 2019 CU2, edite la asignación de configuración de la actualización. Para editar la asignación de configuración de la actualización:

Ejecute el siguiente comando:

   ```bash
   kubectl edit configmap controller-upgrade-configmap
   ```

Edite estos campos:

   **controllerUpgradeTimeoutInMinutes** designa el número de minutos que se esperará a que finalice la actualización del controlador o de la base de datos del controlador. El valor predeterminado es 5. Actualice al menos a 20.
   **totalUpgradeTimeoutInMinutes**: designa la cantidad de tiempo para que el controlador y la base de datos del controlador terminen de actualizarse (actualización del controlador y de la base de datos del controlador). El valor predeterminado es 10. Actualice al menos a 40.
   **componentUpgradeTimeoutInMinutes**: designa la cantidad de tiempo en que se debe completar cada fase posterior de la actualización. El valor predeterminado es 30. Actualice a 45.

Guarde y salga.

## <a name="update-a-bdc-deployment-from-ctp-or-release-candidate"></a>Actualización de una implementación de BDC desde CTP o una versión candidata para lanzamiento

No se admite la actualización local desde una compilación de CTP o de versión candidata para lanzamiento de los Clústeres de macrodatos de SQL Server. En la sección siguiente se explica cómo quitar y volver a crear manualmente el clúster.

### <a name="backup-and-delete-the-old-cluster"></a>Copia de seguridad del clúster anterior y eliminación

No hay ninguna actualización local para los clústeres de macrodatos implementados antes del lanzamiento de SQL Server 2019 GDR1. Actualmente, la única manera de actualizar un clúster de macrodatos a una nueva versión es quitarlo manualmente y volver a crearlo. Cada versión tiene una versión única de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] que no es compatible con la anterior. Además, si se descarga una imagen de contenedor más reciente en un clúster implementado con una versión anterior distinta, es posible que la imagen más reciente no sea compatible con las imágenes anteriores del clúster. La imagen más reciente se extrae si se usa la etiqueta de imagen `latest` en el archivo de configuración de implementación para la configuración del contenedor. De forma predeterminada, cada versión tiene una etiqueta de imagen específica que corresponde a la versión de lanzamiento de SQL Server. Para actualizar a la versión más reciente, siga estos pasos:

1. Antes de eliminar el clúster anterior, realice una copia de seguridad de los datos en la instancia maestra de SQL Server y en HDFS. En la instancia maestra de SQL Server, puede usar [Copia de seguridad y restauración de SQL Server](data-ingestion-restore-database.md). En HDFS, se [pueden copiar los datos con `curl`](data-ingestion-curl.md).

1. Elimine el clúster anterior con el comando `azdata delete cluster`.

   ```bash
    azdata bdc delete --name <old-cluster-name>
   ```

   > [!Important]
   > Use la versión de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] que coincida con el clúster. No elimine un clúster anterior con la versión más reciente de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   > [!Note]
   > La emisión de un comando de `azdata bdc delete` resultará en la eliminación de todos los objetos creados en el espacio de nombres identificado con el nombre del clúster de macrodatos, pero no del espacio de nombres en sí. El espacio de nombres se puede volver a utilizar para las implementaciones posteriores, siempre y cuando esté vacío y no se haya creado ninguna otra aplicación dentro del mismo.

1. Desinstale la versión anterior de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

   ```powershell
   pip3 uninstall -r https://azdatacli.blob.core.windows.net/python/azdata/2019-rc1/requirements.txt
   ```

1. Instale la última versión de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Los comandos siguientes instalan [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] desde la versión más reciente:

   **Windows:**

   ```powershell
   pip3 install -r https://aka.ms/azdata
   ```

   **Linux:**

   ```bash
   pip3 install -r https://aka.ms/azdata --user
   ```

   > [!IMPORTANT]
   > La ruta de acceso a la versión `n-1` de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] cambia en cada versión. Aunque se haya instalado anteriormente [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], se debe volver a instalar desde la ruta de acceso más reciente antes de crear el clúster nuevo.

### <a name="verify-the-azdata-version"></a><a id="azdataversion"></a> Comprobar la versión de azdata

Antes de implementar un nuevo clúster de macrodatos, compruebe que se está usando la versión más reciente de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] con el parámetro `--version`:

```bash
azdata --version
```

### <a name="install-the-new-release"></a>Instalar la nueva versión

Después de quitar el clúster de macrodatos anterior e instalar la versión de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] más reciente, implemente el nuevo clúster de macrodatos con las instrucciones de implementación actuales. Para obtener más información, vea [Cómo implementar [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] en Kubernetes](deployment-guidance.md). Luego, restaure las bases de datos o los archivos necesarios.

## <a name="next-steps"></a>Pasos siguientes

Vea [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]?](big-data-cluster-overview.md) para obtener más información sobre los clústeres de macrodatos.