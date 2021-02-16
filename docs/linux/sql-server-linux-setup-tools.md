---
title: Instalación de herramientas de línea de comandos de SQL Server en Linux
titleSuffix: SQL Server
description: Obtenga información sobre cómo instalar las herramientas de línea de comandos de SQL Server, los controladores ODBC proporcionados por Microsoft y sus dependencias en Linux.
author: VanMSFT
ms.author: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: eff8e226-185f-46d4-a3e3-e18b7a439e63
ms.openlocfilehash: 36508869b30688e7a87035b50a7a06ca1e2a083a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346356"
---
# <a name="install-sqlcmd-and-bcp-the-sql-server-command-line-tools-on-linux"></a>Instalación de las herramientas de línea de comandos sqlcmd y bcp de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En los pasos siguientes se instalan las herramientas de línea de comandos, los controladores ODBC de Microsoft y sus dependencias. El paquete **mssql-tools** contiene:

- **sqlcmd**: utilidad de consulta de línea de comandos.
- **bcp**: utilidad de importación y exportación masiva.

Instale las herramientas para su plataforma:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)
- [macOS](#macos)
- [Docker](#docker)

En este artículo se explica cómo instalar las herramientas línea de comandos. Si busca ejemplos de cómo usar **sqlcmd** o **bcp**, vea los [vínculos](#next-steps) al final de este tema.

## <a name="a-idrhelinstall-tools-on-rhel-8"></a><a id="RHEL"><a/>Instalación de herramientas en RHEL 8

Use los pasos siguientes para instalar **mssql-tools** en Red Hat Enterprise Linux. 

1. Acceda al modo de superusuario.

   ```bash
   sudo su
   ```

1. Descargue el archivo de configuración del repositorio de Red Hat de Microsoft.

   ```bash
   curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/msprod.repo
   ```

1. Salga del modo de superusuario.

   ```bash
   exit
   ```

1. Si tiene instalada una versión anterior de **mssql-tools**, quite los paquetes unixODBC anteriores.

   ```bash
   sudo yum remove mssql-tools unixODBC-utf16-devel
   ```

1. Ejecute los comandos siguientes para instalar **mssql-tools** con el paquete de desarrollador de unixODBC.

   ```bash
   sudo yum install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Para actualizar a la versión más reciente de **mssql-tools**, ejecute los siguientes comandos:
   >    ```bash
   >   sudo yum check-update
   >   sudo yum update mssql-tools
   >   ```

1. **Opcional**: agregue `/opt/mssql-tools/bin/` a la variable de entorno **PATH** en un shell de Bash.

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para inicios de sesión, modifique la variable **PATH** en el archivo **~/.bash_profile** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para sesiones interactivas o que no sean de inicio de sesión, modifique la variable **PATH** en el archivo **~/.bashrc** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-ubuntu-1604"></a><a id="ubuntu"></a>Instalación de herramientas en Ubuntu 16.04

Siga estos pasos para instalar **mssql-tools** en Ubuntu.

> [!NOTE]
> Ubuntu 18.04 se admite a partir de SQL Server 2019 CU3. Si usa Ubuntu 18.04, cambie la ruta de acceso del repositorio de `/ubuntu/16.04` a `/ubuntu/18.04`.

1. Importe las claves de GPG del repositorio público.

   ```bash
   curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

1. Registre el repositorio de Ubuntu de Microsoft.

   ```bash
   curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list
   ```

1. Actualice la lista de orígenes y ejecute el comando de instalación con el paquete para desarrolladores de unixODBC.

   ```bash
   sudo apt-get update 
   sudo apt-get install mssql-tools unixodbc-dev
   ```

   > [!Note] 
   > Para actualizar a la versión más reciente de **mssql-tools**, ejecute los siguientes comandos:
   >    ```bash
   >   sudo apt-get update 
   >   sudo apt-get install mssql-tools 
   >   ```

1. **Opcional**: agregue `/opt/mssql-tools/bin/` a la variable de entorno **PATH** en un shell de Bash.

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para inicios de sesión, modifique la variable **PATH** en el archivo **~/.bash_profile** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para sesiones interactivas o que no sean de inicio de sesión, modifique la variable **PATH** en el archivo **~/.bashrc** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-sles-12"></a><a id="SLES"></a>Instalación de herramientas en SLES 12

Use los pasos siguientes para instalar **mssql-tools** en SUSE Linux Enterprise Server. 

1. Agregue el repositorio de Microsoft SQL Server en Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Instale **mssql-tools** con el paquete de desarrollador de unixODBC.

   ```bash
   sudo zypper install mssql-tools unixODBC-devel
   ```

   > [!Note] 
   > Para actualizar a la versión más reciente de **mssql-tools**, ejecute los siguientes comandos:
   >    ```bash
   >   sudo zypper refresh
   >   sudo zypper update mssql-tools
   >   ```

1. **Opcional**: agregue `/opt/mssql-tools/bin/` a la variable de entorno **PATH** en un shell de Bash.

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para inicios de sesión, modifique la variable **PATH** en el archivo **~/.bash_profile** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   ```

   Para que **sqlcmd/bcp** sea accesible desde el shell de Bash para sesiones interactivas o que no sean de inicio de sesión, modifique la variable **PATH** en el archivo **~/.bashrc** con el comando siguiente:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

## <a name="install-tools-on-macos"></a><a id="macos"></a>Instalación de herramientas en macOS

Ahora hay disponible una versión preliminar de **sqlcmd** y **bcp** en MacOS. Para obtener más información, consulte el [anuncio](https://blogs.technet.microsoft.com/dataplatforminsider/2017/05/16/sql-server-command-line-tools-for-macos-released/).

*Instale [Homebrew](https://brew.sh) si aún no lo tiene:*

- `/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"`

Para instalar las herramientas para Mac El Capitan y Sierra, use los comandos siguientes:

```bash
# brew untap microsoft/mssql-preview if you installed the preview version 
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
brew update
brew install mssql-tools
#for silent install: 
#HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install mssql-tools
```

## <a name="docker"></a><a id="docker"></a> Docker

Si [ejecuta SQL Server en un contenedor de Docker](quickstart-install-connect-docker.md), las herramientas de línea de comandos de SQL Server ya están incluidas en la imagen de contenedor de SQL Server para Linux. Si adjunta un contenedor en ejecución con un shell de Bash interactivo, puede ejecutar las herramientas de forma local.

## <a name="offline-installation"></a>Instalación sin conexión

[!INCLUDE[SQL Server Linux offline package installation](../includes/sql-server-linux-offline-package-install-intro.md)]

1. En primer lugar, busque y copie el paquete **mssql-tools** para su distribución de Linux:

   | Distribución de Linux | Ubicación del paquete **mssql-tools** |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/7.3/prod](https://packages.microsoft.com/rhel/7.3/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/mssql-tools) |

1. Busque y copie también el paquete **msodbcsql**, que es una dependencia. El paquete **msodbcsql** también depende de **unixODBC-devel** (Red Hat y SLES) o **unixodbc-dev** (Ubuntu). La ubicación de los paquetes **msodbcsql** se muestra en la tabla siguiente:

   | Distribución de Linux | Ubicación de paquetes ODBC |
   |---|---|
   | Red Hat | [https://packages.microsoft.com/rhel/8/prod](https://packages.microsoft.com/rhel/8/prod) |
   | SLES | [https://packages.microsoft.com/sles/12/prod](https://packages.microsoft.com/sles/12/prod)|
   | Ubuntu 16.04 | [**msodbcsql**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql)<br/>[**unixodbc-dev**](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/u/unixodbc/) |

1. **Mueva los paquetes descargados a la máquina Linux**. Si ha usado otro equipo para descargar los paquetes, una manera de trasladarlos al equipo Linux es con el comando **scp**.

1. **Instale los paquetes**: Instale los paquetes **mssql-toolds** y **msodbc**. Si obtiene algún error de dependencia, ignórelo hasta el siguiente paso.

    | Plataforma | Comandos de instalación de paquetes |
    |-----|-----|
    | Red Hat | `sudo yum localinstall msodbcsql-<version>.rpm`<br/>`sudo yum localinstall mssql-tools-<version>.rpm` |
    | SLES | `sudo zypper install msodbcsql-<version>.rpm`<br/>`sudo zypper install mssql-tools-<version>.rpm` |
    | Ubuntu | `sudo dpkg -i msodbcsql_<version>.deb`<br/>`sudo dpkg -i mssql-tools_<version>.deb` |

1. **Resuelva las dependencias que faltan**: Es posible que falten dependencias en este punto. Si no es así, puede omitir este paso. En algunos casos, debe localizar e instalar manualmente estas dependencias.

    En el caso de paquetes RPM, puede inspeccionar las dependencias necesarias con los siguientes comandos:

    ```bash
    rpm -qpR msodbcsql-<version>.rpm
    rpm -qpR mssql-tools-<version>.rpm
    ```

    En el caso de paquetes de Debian, si tiene acceso a los repositorios aprobados que contienen dichas dependencias, la solución más sencilla es usar el comando **apt-get**:

    ```bash
    sudo apt-get -f install
    ```

    > [!NOTE]
    > Este comando completa también la instalación de los paquetes de SQL Server.

    Si esto no funciona con su paquete de Debian, puede inspeccionar las dependencias necesarias con los siguientes comandos:

    ```bash
    dpkg -I msodbcsql_<version>_amd64.deb | grep "Depends:"
    dpkg -I mssql-tools_<version>_amd64.deb | grep "Depends:"
    ```

## <a name="next-steps"></a>Pasos siguientes

Para obtener un ejemplo de cómo usar **sqlcmd** para conectarse a SQL Server y crear una base de datos, consulte una de las siguientes guías de inicio rápido:

- [Instalación en Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Instalación en SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Instalación en Ubuntu](quickstart-install-connect-ubuntu.md)
- [Ejecución en Docker](quickstart-install-connect-ubuntu.md)

Para obtener un ejemplo de cómo usar **bcp** para importar y exportar datos de forma masiva, consulte [Copia masiva de datos a SQL Server en Linux](sql-server-linux-migrate-bcp.md).
