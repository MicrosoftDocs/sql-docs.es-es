---
title: Instalación de Microsoft ODBC Driver for SQL Server (Linux)
description: Aprenda a instalar el controlador Microsoft ODBC Driver for SQL Server en clientes de Linux para habilitar la conectividad de base de datos.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- driver, installing
ms.assetid: f78b81ed-5214-43ec-a600-9bfe51c5745a
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d4e2ee02544955cab007d542bca75b8f14cfeeee
ms.sourcegitcommit: f10f0d604be1dce6c600a92aec4c095e7b52e19c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2021
ms.locfileid: "102770463"
---
# <a name="install-the-microsoft-odbc-driver-for-sql-server-linux"></a>Instalación de Microsoft ODBC Driver for SQL Server (Linux)

En este artículo se explica cómo instalar Microsoft ODBC Driver for SQL Server en Linux. También se incluyen instrucciones para las herramientas de línea de comandos opcionales para SQL Server (`bcp` y `sqlcmd`), y los encabezados de desarrollo de unixODBC.

En este artículo se proporcionan comandos para instalar el controlador ODBC desde el shell de Bash. Si quiere descargar directamente los paquetes, vea [Descarga del controlador ODBC para SQL Server](../download-odbc-driver-for-sql-server.md).

## <a name="microsoft-odbc-17"></a><a id="17"></a> Microsoft ODBC 17

En las secciones siguientes se explica cómo instalar el controlador ODBC 17 de Microsoft desde el shell de Bash para diferentes distribuciones de Linux.

- [Alpine Linux](#alpine17)
- [Debian](#debian17)
- [Red Hat Enterprise Linux y Oracle](#redhat17)
- [SUSE Linux Enterprise Server](#suse17)
- [Ubuntu](#ubuntu17)

> [!IMPORTANT]
> Si ha instalado el paquete `msodbcsql` v17 que estaba disponible brevemente, debe quitarlo antes de instalar el paquete `msodbcsql17`. Esto evitará conflictos. El paquete `msodbcsql17` se puede instalar en paralelo con el paquete `msodbcsql` v13.

### <a name="alpine-linux"></a><a id="alpine17"></a> Alpine Linux

```bash
#Download the desired package(s)
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.apk
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.apk


#(Optional) Verify signature, if 'gpg' is missing install it using 'apk add gnupg':
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.7.2.1-1_amd64.sig
curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.7.1.1-1_amd64.sig

curl https://packages.microsoft.com/keys/microsoft.asc  | gpg --import -
gpg --verify msodbcsql17_17.7.2.1-1_amd64.sig msodbcsql17_17.7.2.1-1_amd64.apk
gpg --verify mssql-tools_17.7.1.1-1_amd64.sig mssql-tools_17.7.1.1-1_amd64.apk


#Install the package(s)
sudo apk add --allow-untrusted msodbcsql17_17.7.2.1-1_amd64.apk
sudo apk add --allow-untrusted mssql-tools_17.7.1.1-1_amd64.apk
```

> [!NOTE]
> Se requiere la versión del controlador 17.5 o posterior para la compatibilidad con Alpine.

### <a name="debian"></a><a id="debian17"></a> Debian

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Debian 8
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 9
curl https://packages.microsoft.com/config/debian/9/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Debian 10
curl https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
# optional: kerberos library for debian-slim distributions
sudo apt-get install libgssapi-krb5-2
```

> [!NOTE]
> Puede sustituir el valor de la variable de entorno "ACCEPT_EULA" con el establecimiento de la variable debconf "msodbcsql/ACCEPT_EULA" en su lugar: `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

### <a name="red-hat-enterprise-server-and-oracle-linux"></a><a id="redhat17"></a> Red Hat Enterprise Server y Oracle Linux

```bash
sudo su

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#RedHat Enterprise Server 6
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 7 and Oracle Linux 7
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo

#RedHat Enterprise Server 8 and Oracle Linux 8
curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo

exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server"></a><a id="suse17"></a> SUSE Linux Enterprise Server

```bash
sudo su
curl -O https://packages.microsoft.com/keys/microsoft.asc
rpm --import microsoft.asc

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#SUSE Linux Enterprise Server 11 SP4
#Ensure SUSE Linux Enterprise 11 Security Module has been installed 
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo

#SUSE Linux Enterprise Server 12
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo

#SUSE Linux Enterprise Server 15
zypper ar https://packages.microsoft.com/config/sles/15/prod.repo
#(Only for driver 17.3 and below)
SUSEConnect -p sle-module-legacy/15/x86_64

exit
sudo ACCEPT_EULA=Y zypper install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu"></a><a id="ubuntu17"></a> Ubuntu

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -

#Download appropriate package for the OS version
#Choose only ONE of the following, corresponding to your OS version

#Ubuntu 16.04
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 18.04
curl https://packages.microsoft.com/config/ubuntu/18.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.04
curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list > /etc/apt/sources.list.d/mssql-release.list

#Ubuntu 20.10
curl https://packages.microsoft.com/config/ubuntu/20.10/prod.list > /etc/apt/sources.list.d/mssql-release.list

exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql17
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

> [!NOTE]
> - Se requiere la versión del controlador 17.2 o posterior para la compatibilidad con Ubuntu 18.04.
> - Se requiere la versión del controlador 17.3 o posterior para la compatibilidad con Ubuntu 18.10.

> [!NOTE]
> Puede sustituir el valor de la variable de entorno "ACCEPT_EULA" con el establecimiento de la variable debconf "msodbcsql/ACCEPT_EULA" en su lugar: `echo msodbcsql17 msodbcsql/ACCEPT_EULA boolean true | sudo debconf-set-selections`

## <a name="previous-versions"></a>Versiones anteriores

En las secciones siguientes se proporcionan instrucciones para instalar versiones anteriores de Microsoft ODBC Driver en Linux. Se describen las siguientes versiones del controlador:

- [Microsoft ODBC Driver 13.1 for SQL Server](#13.1)
- [Microsoft ODBC Driver 13 for SQL Server](#13)
- [Microsoft ODBC Driver 11 for SQL Server](#11)

## <a name="odbc-131"></a><a id="13.1"></a> ODBC 13.1

En las secciones siguientes se explica cómo instalar Microsoft ODBC Driver 13.1 desde el shell de Bash para diferentes distribuciones de Linux.

### <a name="debian-8"></a>Debian 8

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/debian/8/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum remove unixODBC-utf16 unixODBC-utf16-devel #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y yum install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo yum install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-11"></a>SUSE Linux Enterprise Server 11

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/11/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo
exit
sudo ACCEPT_EULA=Y zypper install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y zypper install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo zypper install unixODBC-devel
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

### <a name="ubuntu-1610"></a>Ubuntu 16.10

```bash
sudo su
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql
# optional: for bcp and sqlcmd
sudo ACCEPT_EULA=Y apt-get install mssql-tools
echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
source ~/.bashrc
# optional: for unixODBC development headers
sudo apt-get install unixodbc-dev
```

## <a name="odbc-13"></a><a id="13"></a> ODBC 13

En las secciones siguientes se explica cómo instalar Microsoft ODBC Driver 13 desde el shell de Bash para diferentes distribuciones de Linux.

### <a name="redhat-enterprise-server-6"></a>RedHat Enterprise Server 6

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/6/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="redhat-enterprise-server-7"></a>RedHat Enterprise Server 7

```bash
sudo su
curl https://packages.microsoft.com/config/rhel/7/prod.repo > /etc/yum.repos.d/mssql-release.repo
exit
sudo yum update
sudo yum remove unixODBC #to avoid conflicts
sudo ACCEPT_EULA=Y yum install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
sudo yum install unixODBC-utf16-devel #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1510"></a>Ubuntu 15.10

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/15.10/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="ubuntu-1604"></a>Ubuntu 16.04

```bash
sudo su 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
exit
sudo apt-get update
sudo ACCEPT_EULA=Y apt-get install msodbcsql=13.0.1.0-1 mssql-tools=14.0.2.0-1
sudo apt-get install unixodbc-dev-utf16 #this step is optional but recommended*
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

```bash
sudo su 
zypper ar https://packages.microsoft.com/config/sles/12/prod.repo 
zypper update 
sudo ACCEPT_EULA=Y zypper install msodbcsql-13.0.1.0-1 mssql-tools-14.0.2.0-1
zypper install unixODBC-utf16-devel
#Create symlinks for tools
ln -sfn /opt/mssql-tools/bin/sqlcmd-13.0.1.0 /usr/bin/sqlcmd 
ln -sfn /opt/mssql-tools/bin/bcp-13.0.1.0 /usr/bin/bcp
```

### <a name="offline-installation"></a>Instalación sin conexión

Si prefiere o necesita que la versión 13 del controlador ODBC de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] se instale en un equipo sin conexión a Internet, deberá resolver manualmente las dependencias del paquete. La versión 13 del controlador ODBC de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] tiene las siguientes dependencias directas:
- Ubuntu: libc6 (>= 2.21), libstdc++6 (>= 4.9), libkrb5-3, libcurl3, openssl, debconf (>= 0.5), unixodbc (>= 2.3.1-1)
- Red Hat: ```glibc, e2fsprogs, krb5-libs, openssl, unixODBC```
- SUSE: ```glibc, libuuid1, krb5, openssl, unixODBC```

Cada uno de estos paquetes a su vez tiene sus propias dependencias, que pueden o no estar presentes en el sistema. Para una solución general de este problema, consulte la documentación del administrador de paquetes de su distribución: [Redhat](https://wiki.centos.org/HowTos/CreateLocalRepos), [Ubuntu](https://unix.stackexchange.com/questions/87130/how-to-quickly-create-a-local-apt-repository-for-random-packages-using-a-debian) y [SUSE](https://en.opensuse.org/Portal:Zypper)

También es común descargar manualmente todos los paquetes dependientes y colocarlos juntos en el equipo de instalación, luego instalar manualmente cada paquete a su vez, finalizando con el paquete de la versión 13 del controlador ODBC de [!INCLUDE[msCoName](../../../includes/msconame_md.md)].

#### <a name="redhat-linux-enterprise-server-7"></a>Redhat Linux Enterprise Server 7

- Descargue la versión más reciente de `msodbcsql` `.rpm` en [https://packages.microsoft.com/rhel/7/prod/](https://packages.microsoft.com/rhel/7/prod/).
- Instale las dependencias y el controlador.
  
```bash
yum install glibc e2fsprogs krb5-libs openssl unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

#### <a name="ubuntu-1604"></a>Ubuntu 16.04

- Descargue la versión más reciente de `msodbcsql` `.deb` en [https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/](https://packages.microsoft.com/ubuntu/16.04/prod/pool/main/m/msodbcsql/).
- Instale las dependencias y el controlador.

```bash
sudo apt-get install libc6 libstdc++6 libkrb5-3 libcurl3 openssl debconf unixodbc unixodbc-dev #install dependencies
sudo dpkg -i msodbcsql_13.1.X.X-X_amd64.deb #install the Driver
```

#### <a name="suse-linux-enterprise-server-12"></a>SUSE Linux Enterprise Server 12

- Descargue la versión más reciente de `msodbcsql` `.rpm` en [https://packages.microsoft.com/sles/12/prod/](https://packages.microsoft.com/sles/12/prod/).
- Instale las dependencias y el controlador.

```bash
zypper install glibc, libuuid1, krb5, openssl, unixODBC unixODBC-devel #install dependencies
sudo rpm -i  msodbcsql-13.1.X.X-X.x86_64.rpm #install the Driver
```

Una vez que haya completado la instalación del paquete, puede comprobar que [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver 13 encuentre todas sus dependencias si ejecuta ldd e inspecciona su salida para las bibliotecas que faltan:

```bash
ldd /opt/microsoft/msodbcsql/lib64/libmsodbcsql-*
```

## <a name="odbc-11"></a><a id="11"></a> ODBC 11

En las secciones siguientes se explica cómo instalar Microsoft ODBC Driver 11 en Linux. Para poder utilizar el controlador, instale el Administrador de controladores unixODBC. Para obtener más información, vea [Installing the Driver Manager](../../../connect/odbc/linux-mac/installing-the-driver-manager.md) (Instalación del administrador de controladores).

### <a name="installation-steps"></a>Pasos de instalación  

> [!IMPORTANT]  
> En estas instrucciones se hace referencia a `msodbcsql-11.0.2270.0.tar.gz`, que es el archivo de instalación para Red Hat Linux. Si va a instalar la versión preliminar para SUSE Linux, el nombre de archivo es `msodbcsql-11.0.2260.0.tar.gz`.  
  
Para instalar el controlador, haga lo siguiente:

1. Asegúrese de que tiene permisos de raíz.  

2. Cambie al directorio donde la descarga ha colocado el archivo `msodbcsql-11.0.2270.0.tar.gz`. Asegúrese de que dispone del archivo \*.tar.gz que coincida con la versión de Linux. Para extraer los archivos, ejecute el comando siguiente: `tar xvzf msodbcsql-11.0.2270.0.tar.gz`.  
  
3. Cambie al directorio `msodbcsql-11.0.2270.0`; allí verá un archivo llamado **install.sh**.  
  
4. Para ver una lista de las opciones de instalación disponibles, ejecute el siguiente comando: **./install.sh**.  
  
5. Realice una copia de seguridad de **odbcinst.ini**. El programa de instalación del controlador actualiza **odbcinst.ini**. odbcinst.ini contiene la lista de controladores que están registrados en el Administrador de controladores unixODBC. Para detectar la ubicación de odbcinst.ini en el equipo, ejecute el siguiente comando: ```odbc_config --odbcinstini```.  
  
6. Antes de instalar el controlador, ejecute el siguiente comando: `./install.sh verify`. La salida de `./install.sh verify` notifica si el equipo tiene el software necesario para admitir el controlador ODBC en Linux.  
  
7. Cuando esté preparado para instalar el controlador ODBC en Linux, ejecute el comando `./install.sh install`. Si tiene que especificar un comando de instalación (`bin-dir` o `lib-dir`), especifíquelo después de la opción **install**.  
  
8. Cuando revise el contrato de licencia, escriba **YES** para continuar con la instalación.  
  
El programa de instalación coloca el controlador en `/opt/microsoft/msodbcsql/11.0.2270.0`. El controlador y sus archivos auxiliares deben estar en `/opt/microsoft/msodbcsql/11.0.2270.0`.  
  
Para comprobar que se haya registrado correctamente Microsoft ODBC Driver en Linux, ejecute el siguiente comando: ```odbcinst -q -d -n "ODBC Driver 11 for SQL Server"```.  
  
### <a name="uninstall"></a>Desinstalación  
  
Puede desinstalar ODBC Driver 11 en Linux ejecutando los siguientes comandos:  
  
1. `rm -f /usr/bin/sqlcmd`
  
2. `rm -f /usr/bin/bcp`  
  
3. `rm -rf /opt/microsoft/msodbcsql`  
  
4. `odbcinst -u -d -n "ODBC Driver 11 for SQL Server"`
  
## <a name="driver-files"></a>Archivos de controlador

El controlador ODBC en Linux consta de los componentes siguientes:

|Componente|Descripción|  
|---------------|-----------------|  
|libmsodbcsql-17.X.so.X.X o libmsodbcsql-13.X.so.X.X|El archivo de biblioteca dinámica del objeto compartido (`so`) que contiene toda la funcionalidad del controlador. Este archivo se instala en `/opt/microsoft/msodbcsql17/lib64/` para el controlador 17 y en `/opt/microsoft/msodbcsql/lib64/` para el controlador 13.|  
|`msodbcsqlr17.rll` o `msodbcsqlr13.rll`|El archivo de recursos asociado de la biblioteca de controladores. Este archivo se instala en `[driver .so directory]../share/resources/en_US/`.| 
|msodbcsql.h|El archivo de encabezado que contiene todas las definiciones nuevas necesarias para utilizar el controlador.<br /><br /> **Nota:**  No se puede hacer referencia a msodbcsql.h y odbcss.h en el mismo programa.<br /><br /> msodbcsql.h se instala en `/opt/microsoft/msodbcsql17/include/` para el controlador 17 y en `/opt/microsoft/msodbcsql/include/` para el controlador 13. |
|LICENSE.txt|El archivo de texto que contiene los términos del Contrato de licencia del usuario final. Este archivo se coloca en `/usr/share/doc/msodbcsql17/` para el controlador 17 y en `/usr/share/doc/msodbcsql/` para el controlador 13.|
|RELEASE_NOTES|El archivo de texto que contiene las notas de la versión. Este archivo se coloca en `/usr/share/doc/msodbcsql17/` para el controlador 17 y en `/usr/share/doc/msodbcsql/` para el controlador 13.|

## <a name="resource-file-loading"></a>Carga del archivo de recursos

El controlador debe cargar el archivo de recursos para poder funcionar. Este archivo se denomina `msodbcsqlr17.rll` o `msodbcsqlr13.rll` según la versión del controlador. La ubicación del archivo `.rll` es relativa a la ubicación del propio controlador (`so` o `dylib`), tal como se indica en la tabla anterior. A partir de la versión 17.1, el controlador también intentará cargar el `.rll` desde el directorio predeterminado si se produce un error en la carga de la ruta de acceso relativa. La ruta de acceso del archivo de recursos predeterminada en Linux es `/opt/microsoft/msodbcsql17/share/resources/en_US/`.

## <a name="troubleshooting"></a>Solución de problemas

Si no puede establecer una conexión con SQL Server con el controlador ODBC, vea el artículo sobre problemas conocidos en [Solución de problemas de conexión](known-issues-in-this-version-of-the-driver.md#connectivity).

## <a name="next-steps"></a>Pasos siguientes

Después de instalar el controlador, puede probar la [aplicación de ejemplo ODBC de C++](../../odbc/cpp-code-example-app-connect-access-sql-db.md). Para obtener más información sobre el desarrollo de aplicaciones ODBC, vea [Desarrollo de aplicaciones](../../../odbc/reference/develop-app/developing-applications.md).

Para obtener más información, vea las [notas de la versión](release-notes-odbc-sql-server-linux-mac.md) y los [requisitos del sistema](system-requirements.md) del controlador ODBC.
