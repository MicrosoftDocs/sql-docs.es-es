---
title: 'RHEL: Instalación de SQL Server en Linux'
titleSuffix: SQL Server
description: En este inicio rápido, se muestra cómo instalar SQL Server 2017 o SQL Server 2019 en Red Hat Enterprise Linux (RHEL) y, después, crear y consultar una base de datos con sqlcmd.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 92503f59-96dc-4f6a-b1b0-d135c43e935e
ms.openlocfilehash: cb266632fccaceb4639c7fdf0cfbf5368aad2c80
ms.sourcegitcommit: 2bdf1f1ee88f4fe3e872227d025e965e95d1b2b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2021
ms.locfileid: "98711981"
---
# <a name="quickstart-install-sql-server-and-create-a-database-on-red-hat"></a>Inicio rápido: instalar SQL Server y crear una base de datos en Red Hat

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

En este inicio rápido, instalará SQL Server 2017 o SQL Server 2019 en Red Hat Enterprise Linux (RHEL). Después, se conectará con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

En este inicio rápido, instalará SQL Server 2019 en Red Hat Enterprise Linux (RHEL) 7/8. Después, se conectará con **sqlcmd** para crear la primera base de datos y ejecutar consultas.

::: moniker-end

> [!TIP]
> Este tutorial necesita la intervención del usuario y una conexión a Internet. Para obtener más información sobre los procedimientos de instalación [desatendida](sql-server-linux-setup.md#unattended) o [sin conexión](sql-server-linux-setup.md#offline), vea la [Guía de instalación para SQL Server en Linux](sql-server-linux-setup.md).

## <a name="prerequisites"></a>Requisitos previos

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

Necesita un equipo con RHEL 7.3-7.8 u 8.0-8.3, con un **mínimo de 2 GB** de memoria.

::: moniker-end

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

Necesita un equipo con RHEL 7.3-7.8 u 8.0-8.3, con un **mínimo de 2 GB** de memoria.

::: moniker-end

Para instalar Red Hat Enterprise Linux en su propio equipo, vaya a [https://access.redhat.com/products/red-hat-enterprise-linux/evaluation](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation). También puede crear máquinas virtuales con RHEL en Azure. Vea [Creación y administración de máquinas virtuales Linux con la CLI de Azure](/azure/virtual-machines/linux/tutorial-manage-vm) y use `--image RHEL` en la llamada a `az vm create`.

Si anteriormente ha instalado una versión CTP o RC de SQL Server, primero debe quitar el repositorio anterior para seguir estos pasos. Para obtener más información, vea [Configurar repositorios de Linux para SQL Server 2017 y 2019](sql-server-linux-change-repo.md).

Para conocer otros requisitos del sistema, vea [Requisitos del sistema para SQL Server en Linux](sql-server-linux-setup.md#system).

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

## <a name="install-sql-server"></a><a id="install"></a>Instalar SQL Server

> [!NOTE]
> RHEL 8 es compatible con SQL Server 2017 a partir de CU20. Los siguientes comandos para SQL Server 2017 apuntan al repositorio de RHEL 8. RHEL 8 no viene preinstalado con python2. SQL Server requiere RHEL 8. Antes de comenzar los pasos de instalación de SQL Server, ejecute el comando y compruebe que python2 está seleccionado como intérprete:
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ```
> Para obtener más información, vea el siguiente blog sobre la instalación de python2 y su configuración como intérprete predeterminado: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>

Para configurar SQL Server en RHEL, ejecute los comandos siguientes en un terminal para instalar el paquete **mssql-server**:

1. Descargue el archivo de configuración del repositorio de Red Hat de Microsoft SQL Server 2017:

   Para RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2017.repo
   ```
   
   Para RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo
   ```

   > [!TIP]
   > Si quiere instalar SQL Server 2019, debe registrar en su lugar el repositorio de SQL Server 2019. Use el comando siguiente para las instalaciones de SQL Server 2019:
   >
   > Para RHEL7:
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   > ```
   >
   > Para RHEL8:
   >
   > ```bash
   > sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   > ```

2. Ejecute los comandos siguientes para instalar SQL Server:

   ```bash
   sudo yum install -y mssql-server
   ```

3. Cuando finalice la instalación del paquete, ejecute **mssql-conf setup** y siga las indicaciones para establecer la contraseña de administrador del sistema y elegir la edición.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!TIP]
   > Las siguientes ediciones de SQL Server 2017 tienen licencia gratuita: Evaluation, Developer y Express.

   > [!NOTE]
   > Asegúrese de especificar una contraseña segura para la cuenta del administrador del sistema (longitud mínima de 8 caracteres, incluidas mayúsculas y minúsculas, dígitos en base 10 o símbolos no alfanuméricos).

4. Cuando finalice la configuración, compruebe que el servicio se esté ejecutando:

   ```bash
   systemctl status mssql-server
   ```

5. Para permitir las conexiones remotas, abra el puerto de SQL Server en el firewall en RHEL. El puerto de SQL Server predeterminado es TCP 1433. Si usa **FirewallD** para el firewall, puede usar los comandos siguientes:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

En este momento, SQL Server se ejecuta en el equipo con RHEL y está listo para usarse.

::: moniker-end
<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

## <a name="install-sql-server"></a><a id="install"></a>Instalar SQL Server

> [!NOTE]
> Los siguientes comandos para SQL Server 2019 apunta al repositorio de RHEL 8. RHEL 8 no viene preinstalado con python2. SQL Server requiere RHEL 8. Antes de comenzar los pasos de instalación de SQL Server, ejecute el comando y compruebe que python2 está seleccionado como intérprete: 
>
> ```
> sudo alternatives --config python
> # If not configured, install python2 and openssl10 using the following commands: 
> sudo yum install python2
> sudo yum install compat-openssl10
> # Configure python2 as the default interpreter using this command: 
> sudo alternatives --config python
> ``` 
> 
> Para obtener más información sobre estos pasos, vea el siguiente blog sobre cómo instalar python2 y cómo configurarlo como intérprete predeterminado: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
> 

Para configurar SQL Server en RHEL, ejecute los comandos siguientes en un terminal para instalar el paquete **mssql-server**:

1. Descargue el archivo de configuración del repositorio de Red Hat de Microsoft SQL Server 2019:
   
   Para RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/7/mssql-server-2019.repo
   ```
   
   Para RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo
   ```

2. Ejecute los comandos siguientes para instalar SQL Server:

   ```bash
   sudo yum install -y mssql-server
   ```

3. Cuando finalice la instalación del paquete, ejecute **mssql-conf setup** y siga las indicaciones para establecer la contraseña de administrador del sistema y elegir la edición.

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

   > [!NOTE]
   > Asegúrese de especificar una contraseña segura para la cuenta del administrador del sistema (longitud mínima de 8 caracteres, incluidas mayúsculas y minúsculas, dígitos en base 10 o símbolos no alfanuméricos).

4. Cuando finalice la configuración, compruebe que el servicio se esté ejecutando:

   ```bash
   systemctl status mssql-server
   ```

5. Para permitir las conexiones remotas, abra el puerto de SQL Server en el firewall en RHEL. El puerto de SQL Server predeterminado es TCP 1433. Si usa **FirewallD** para el firewall, puede usar los comandos siguientes:

   ```bash
   sudo firewall-cmd --zone=public --add-port=1433/tcp --permanent
   sudo firewall-cmd --reload
   ```

En este momento, SQL Server 2019 se ejecuta en el equipo con RHEL y está listo para usarse.

::: moniker-end

## <a name="install-the-sql-server-command-line-tools"></a><a id="tools"></a>Instalar las herramientas de línea de comandos de SQL Server

Para crear una base de datos, necesita conectarse con una herramienta que pueda ejecutar instrucciones Transact-SQL en SQL Server. En los pasos siguientes, se instalan las herramientas de línea de comandos de SQL Server [sqlcmd](../tools/sqlcmd-utility.md) y [bcp](../tools/bcp-utility.md).

1. Descargue el archivo de configuración del repositorio de Red Hat de Microsoft.

   Para RHEL7:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/7/prod.repo
   ```
   
   Para RHEL8:
   
   ```bash
   sudo curl -o /etc/yum.repos.d/msprod.repo https://packages.microsoft.com/config/rhel/8/prod.repo
   ```

1. Si tiene instalada una versión anterior de **mssql-tools**, quite los paquetes unixODBC anteriores.

   ```bash
   sudo yum remove unixODBC-utf16 unixODBC-utf16-devel
   ```

1. Ejecute los comandos siguientes para instalar **mssql-tools** con el paquete de desarrollador de unixODBC. Para más información, consulte [Instalación de Microsoft ODBC Driver for SQL Server (Linux)](../connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server.md).

   ```bash
   sudo yum install -y mssql-tools unixODBC-devel
   ```

1. Por comodidad, agregue `/opt/mssql-tools/bin/` a la variable de entorno **PATH**. De este modo, podrá ejecutar las herramientas sin especificar la ruta de acceso completa. Ejecute los comandos siguientes para modificar la variable **PATH**, tanto para las sesiones de inicio de sesión como para las sesiones interactivas o sin inicio de sesión:

   ```bash
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bash_profile
   echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
   source ~/.bashrc
   ```

[!INCLUDE [Connect, create, and query data](../includes/sql-linux-quickstart-connect-query.md)]
