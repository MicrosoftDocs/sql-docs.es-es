---
title: 'SUSE: Instalación de SQL Server en Linux'
description: En esta guía de inicio rápido se explica cómo instalar SQL Server 2017 o SQL Server 2019 en SUSE Linux Enterprise Server y, después, crear y consultar una base de datos con sqlcmd.
author: VanMSFT
ms.author: vanto
ms.date: 04/10/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31ddfb80-f75c-4f51-8540-de6213cb68b8
ms.openlocfilehash: 7aadd757e9e825ace17b69d28f2ab8ecc78123da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340404"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-suse-linux-enterprise-server"></a>Inicio rápido: Instalación de SQL Server y creación de una base de datos en SUSE Linux Enterprise Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En este inicio rápido, instalará SQL Server 2017 o SQL Server 2019 en SUSE Linux Enterprise Server (SLES) v12 SP2. Después, se conectará con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

En este inicio rápido, instalará SQL Server 2019 en SUSE Linux Enterprise Server (SLES) v12. Después, se conectará con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

> [!IMPORTANT]
> SQL Server 2019 se admite en SUSE Enterprise Linux Server v12 SP2, SP3, SP4 o SP5.

::: moniker-end

> [!TIP]
> Este tutorial necesita la intervención del usuario y una conexión a Internet. Para obtener más información sobre los procedimientos de instalación [desatendida](sql-server-linux-setup.md#unattended) o [sin conexión](sql-server-linux-setup.md#offline), vea la [Guía de instalación para SQL Server en Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Prerrequisitos

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Debe tener un equipo SLES v12 SP2 con **al menos 2 GB** de memoria. El sistema de archivos debe ser **XFS** o **EXT4**. No se admiten otros sistemas de archivos, como **BTRFS**.

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Debe tener un equipo SLES v12 SP2, SP3, SP4 o SP5 con **al menos 2 GB** de memoria. El sistema de archivos debe ser **XFS** o **EXT4**. No se admiten otros sistemas de archivos, como **BTRFS**.

::: moniker-end

Para instalar SUSE Linux Enterprise Server en su equipo, vaya a [https://www.suse.com/products/server](https://www.suse.com/products/server). También puede crear máquinas virtuales de SLES en Azure. Vea [Creación y administración de máquinas virtuales Linux con la CLI de Azure](/azure/virtual-machines/linux/tutorial-manage-vm) y use `--image SLES` en la llamada a `az vm create`.

Si anteriormente ha instalado una versión CTP o RC de SQL Server, primero debe quitar el repositorio anterior para seguir estos pasos. Para obtener más información, vea [Configurar repositorios de Linux para SQL Server 2017 y 2019](sql-server-linux-change-repo.md).

> [!NOTE]
> En este momento, no se admite como destino de instalación el [subsistema de Windows para Linux](/windows/wsl/about) para Windows 10.

Para conocer otros requisitos del sistema, vea [Requisitos del sistema para SQL Server en Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server-2017"></a><a id="install"></a>Instalación de SQL Server 2017

Para configurar SQL Server 2017 en SLES, ejecute los siguientes comandos en un terminal para instalar el paquete **mssql-server**:

1. Descargue el archivo de configuración del repositorio de SLES de Microsoft SQL Server 2017.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
   ```

   > [!TIP]
   > Si quiere instalar SQL Server 2019, debe registrar en su lugar el repositorio de SQL Server 2019. Use el comando siguiente para las instalaciones de SQL Server 2019:
   >
   > ```bash
   > sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   > ```

2. Actualice los repositorios.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```
   
   Para asegurarse de que la clave de firma de paquetes de Microsoft está instalada en el sistema, impórtela con el siguiente comando: 
   
   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Ejecute los comandos siguientes para instalar SQL Server:

   ```bash
   sudo zypper install -y mssql-server
   ```

4. Cuando finalice la instalación del paquete, ejecute **mssql-conf setup** y siga las indicaciones para establecer la contraseña de administrador del sistema y elegir la edición.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Las siguientes ediciones de SQL Server 2017 tienen licencia gratuita: Evaluation, Developer y Express.

   > [!NOTE]
   > Asegúrese de especificar una contraseña segura para la cuenta del administrador del sistema (longitud mínima de 8 caracteres, incluidas mayúsculas y minúsculas, dígitos en base 10 o símbolos no alfanuméricos).

5. Cuando finalice la configuración, compruebe que el servicio se esté ejecutando:

   ```bash
   systemctl status mssql-server
   ```

6. Si planea conectarse de forma remota, es posible que también tenga que abrir el puerto TCP de SQL Server (valor predeterminado: 1433) en el firewall. Si usa el firewall de SUSE, tiene que editar el archivo de configuración **/etc/sysconfig/SuSEfirewall2**. Modifique la entrada **FW_SERVICES_EXT_TCP** para incluir el número de puerto de SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

En este momento, SQL Server se está ejecutando en el equipo SLES y está listo para usarse.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server-2019"></a><a id="install"></a>Instalación de SQL Server 2019

Para configurar SQL Server 2019 en SLES, ejecute los siguientes comandos en un terminal para instalar el paquete **mssql-server**:

1. Descargue el archivo de configuración del repositorio de SLES de Microsoft SQL Server 2019:

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo
   ```

2. Actualice los repositorios.

   ```bash
   sudo zypper --gpg-auto-import-keys refresh 
   ```

   Para asegurarse de que la clave de firma de paquetes de Microsoft está instalada en el sistema, use el siguiente comando para importar la clave: 

   ```bash
   sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
   ```
   
3. Ejecute los comandos siguientes para instalar SQL Server:

   ```bash
   sudo zypper install -y mssql-server
   ```

4. Cuando finalice la instalación del paquete, ejecute **mssql-conf setup** y siga las indicaciones para establecer la contraseña de administrador del sistema y elegir la edición.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Asegúrese de especificar una contraseña segura para la cuenta del administrador del sistema (longitud mínima de 8 caracteres, incluidas mayúsculas y minúsculas, dígitos en base 10 o símbolos no alfanuméricos).

5. Cuando finalice la configuración, compruebe que el servicio se esté ejecutando:

   ```bash
   systemctl status mssql-server
   ```

6. Si planea conectarse de forma remota, es posible que también tenga que abrir el puerto TCP de SQL Server (valor predeterminado: 1433) en el firewall. Si usa el firewall de SUSE, tiene que editar el archivo de configuración **/etc/sysconfig/SuSEfirewall2**. Modifique la entrada **FW_SERVICES_EXT_TCP** para incluir el número de puerto de SQL Server.

   ```
   FW_SERVICES_EXT_TCP="1433"
   ```

En este momento, SQL Server 2019 se ejecuta en la máquina SLES y está listo para usarse.

::: moniker-end


## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Instalar las herramientas de línea de comandos de SQL Server

Para crear una base de datos, necesita conectarse con una herramienta que pueda ejecutar instrucciones Transact-SQL en SQL Server. En los pasos siguientes, se instalan las herramientas de línea de comandos de SQL Server [sqlcmd](../tools/sqlcmd-utility.md) y [bcp](../tools/bcp-utility.md).

1. Agregue el repositorio de Microsoft SQL Server en Zypper.

   ```bash
   sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/prod.repo 
   sudo zypper --gpg-auto-import-keys refresh
   ```

1. Instale **mssql-tools** con el paquete de desarrollador de unixODBC. Para más información, consulte [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo zypper install -y mssql-tools unixODBC-devel
   ```

1. Por comodidad, agregue `/opt/mssql-tools/bin/` a la variable de entorno **PATH**. De este modo, podrá ejecutar las herramientas sin especificar la ruta de acceso completa. Ejecute los comandos siguientes para modificar la variable **PATH**, tanto para las sesiones de inicio de sesión como para las sesiones interactivas o sin inicio de sesión:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]