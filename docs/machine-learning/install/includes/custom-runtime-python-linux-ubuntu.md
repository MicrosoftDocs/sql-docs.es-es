---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 0c0efdc599574748112c1d5909404603de24da2c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072947"
---
## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../../sql-server-machine-learning-services.md) en SQL Server 2019, el paquete **mssql-server-extensibility** para las Extensiones de lenguaje ya está instalado y puede omitir este paso.

Ejecute los comandos siguientes para instalar [Extensiones de lenguaje de SQL Server](../../../language-extensions/language-extensions-overview.md) en Ubuntu Linux, que se usa para el entorno de ejecución personalizado de Python.

1. Si es posible, ejecute este comando para actualizar los paquetes en el sistema antes de la instalación.

    ```bash
    # Install as root or sudo
    sudo apt-get update
    ```

1. Ubuntu podría no tener la opción de transporte de apt para https. Para realizar la instalación, ejecute este comando.

    ```bash
    # Install as root or sudo
    apt-get install apt-transport-https
    ```

1. Instale **mssql-server-extensibility** con este comando.

    ```bash
    # Install as root or sudo
    sudo apt-get install mssql-server-extensibility
    ```

## <a name="install-python-37-and-pandas"></a>Instalación de Python 3.7 y pandas

La extensión del lenguaje Python que se usa para el entorno de ejecución de Python actualmente admite solo [Python 3.7](https://www.python.org/). Si quiere usar una versión distinta de Python, siga la instrucción que aparece en el [repositorio de GitHub sobre Extensiones de lenguaje de Python](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/python) para modificar y recompilar la extensión.

1. Ejecute los comandos siguientes para instalar Python 3.7.

    ```bash
    # Install python3.7 and the corresponding library:
    sudo add-apt-repository ppa:deadsnakes/ppa
    sudo apt-get update
    sudo apt-get install python3.7 python3-pip libpython3.7
    ```

1. Ejecute el comando siguiente para instalar el paquete pandas

    ```bash
    # Install pandas to /usr/lib:
    sudo python3.7 -m pip install pandas -t /usr/lib/python3.7/dist-packages
    ```
