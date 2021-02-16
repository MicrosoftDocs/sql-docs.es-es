---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: a10c54f61151835692f2bc05de440062fe2f3cfb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072796"
---
## <a name="install-language-extensions"></a>Instalación de Extensiones de lenguaje

> [!NOTE]
> Si instaló [Machine Learning Services](../../sql-server-machine-learning-services.md) en SQL Server 2019, el paquete **mssql-server-extensibility** para las Extensiones de lenguaje ya está instalado y puede omitir este paso.

Ejecute el comando siguiente para instalar [Extensiones de lenguaje de SQL Server](../../../language-extensions/language-extensions-overview.md) en SUSE Linux Enterprise Server (SLES), que se usa para el entorno de ejecución personalizado de R.

```bash
# Install as root or sudo
sudo zypper install mssql-server-extensibility
```

## <a name="install-r"></a>Instalar R

1. Si ha instalado [Machine Learning Services](../../sql-server-machine-learning-services.md), R ya está instalado en `/opt/microsoft/ropen/3.5.2/lib64/R`. Si quiere seguir usando esta ruta de acceso como valor de R_HOME, puede omitir este paso.

    Si desea usar otro entorno de ejecución de R, primero debe eliminar `microsoft-r-open-mro` antes de continuar con la instalación de una nueva versión.

    ```bash
    sudo zypper remove microsoft-r-open-mro-3.5.2
    ```

1. Instale [R (3.3 o una versión posterior)](https://www.r-project.org/) para SUSE Linux Enterprise Server (SLES). De forma predeterminada, R se instala en **/usr/lib/R**. Esta ruta de acceso es **R_HOME**. Si instala R en otra ubicación, anote esa ruta de acceso como **R_HOME**.
