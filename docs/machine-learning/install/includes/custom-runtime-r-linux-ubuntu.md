---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 83b39be5be128ba4fbda17765a7b67deead2c80c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072785"
---
## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../../sql-server-machine-learning-services.md) en SQL Server 2019, el paquete **mssql-server-extensibility** para las Extensiones de lenguaje ya está instalado y puede omitir este paso.

Ejecute los comandos siguientes para instalar [Extensiones de lenguaje de SQL Server](../../../language-extensions/language-extensions-overview.md) en Ubuntu Linux, que se usa para el entorno de ejecución personalizado de R.

1. Si es posible, ejecute este comando para actualizar los paquetes en el sistema antes de la instalación.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Instale **mssql-server-extensibility** con este comando.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-r"></a>Instalar R

1. Si ha instalado [Machine Learning Services](../../sql-server-machine-learning-services.md), R ya está instalado en `/opt/microsoft/ropen/3.5.2/lib64/R`. Si quiere seguir usando esta ruta de acceso como valor de R_HOME, puede omitir este paso.

    Si desea usar otro entorno de ejecución de R, primero debe eliminar `microsoft-r-open-mro` antes de continuar con la instalación de una nueva versión.

    ```bash
    sudo apt remove microsoft-r-open-mro-3.5.2
    ```

1. Instale [R (3.3 o una versión posterior)](https://www.r-project.org/) para Ubuntu. De forma predeterminada, R se instala en **/usr/lib/R**. Esta ruta de acceso es **R_HOME**. Si instala R en otra ubicación, anote esa ruta de acceso como **R_HOME**.

    A continuación se muestran instrucciones de ejemplo para Ubuntu. Cambie la dirección URL del repositorio siguiente para su versión de R.

    ```bash
    export DEBIAN_FRONTEND=noninteractive
    sudo apt-get update
    sudo apt-get --no-install-recommends -y install curl zip unzip apt-transport-https libstdc++6
    
    # Add R CRAN repository. This repository works for R 4.0.x.
    #
    sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
    sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu xenial-cran40/'
    sudo apt-get update
    
    # Install R runtime.
    #
    sudo apt-get -y install r-base-core
    ```
