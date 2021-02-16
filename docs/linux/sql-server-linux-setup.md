---
title: Guía de instalación de SQL Server en Linux
titleSuffix: SQL Server
description: Instale, actualice y desinstale SQL Server en Linux. En este artículo se tratan escenarios en línea, sin conexión y desatendidos.
author: VanMSFT
ms.author: vanto
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.custom: sqlfreshmay19
ms.technology: linux
ms.assetid: 565156c3-7256-4e63-aaf0-884522ef2a52
ms.openlocfilehash: eacd871674d15bfa96cd77dd290ff9640520b58a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346418"
---
# <a name="installation-guidance-for-sql-server-on-linux"></a>Guía de instalación de SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se ofrecen instrucciones para instalar, actualizar y desinstalar SQL Server 2017 y SQL Server 2019 en Linux.

Para otros escenarios de implementación, consulte:

- [Windows](../database-engine/install-windows/install-sql-server.md)
- [Contenedores de Docker](./sql-server-linux-docker-container-deployment.md)
- [Kubernetes: clústeres de macrodatos](../big-data-cluster/deploy-get-started.md)

> [!TIP]
> En esta guía se tratan varios escenarios de implementación. Si solo busca instrucciones de instalación paso a paso, vaya a uno de los inicios rápidos:
> - [Inicio rápido de RHEL](quickstart-install-connect-red-hat.md)
> - [Inicio rápido de SLES](quickstart-install-connect-suse.md)
> - [Inicio rápido de Ubuntu](quickstart-install-connect-ubuntu.md)
> - [Inicio rápido de Docker](quickstart-install-connect-docker.md)

Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](../linux/sql-server-linux-faq.md).

## <a name="supported-platforms"></a><a id="supportedplatforms"></a> Plataformas compatibles

SQL Server se admite en Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) y Ubuntu. También se admite como una imagen de Docker que se puede ejecutar en el motor de Docker en Linux o en Docker para Windows o Mac.

<!--SQL Server 2017 on Linux-->
::: moniker range="= sql-server-linux-2017 || = sql-server-2017"

| Plataforma | Versiones admitidas | Obtener
|-----|-----|-----
| **Red Hat Enterprise Linux** | 7.3, 7.4, 7.5, 7.6, 8  | [Obtener RHEL 7.6](https://access.redhat.com/products/red-hat-enterprise-linux/evaluation)
| **SUSE Linux Enterprise Server** | v12 SP2 | [Obtener SLES v12 SP2](https://www.suse.com/products/server)
| **Ubuntu** | 16.04 | [Obtener Ubuntu 16.04](http://releases.ubuntu.com/xenial/)
| **Motor de Docker** | 1.8+ | [Obtener Docker](https://www.docker.com/get-started)

::: moniker-end

<!--SQL Server 2019 on Linux-->
::: moniker range=">= sql-server-linux-ver15 || >= sql-server-ver15 "

[!INCLUDE [linux-supported-platfoms-2019](../includes/linux-supported-platfoms-2019.md)]

::: moniker-end

Microsoft también admite la implementación y administración de contenedores de SQL Server mediante OpenShift y Kubernetes.

> [!NOTE]
> SQL Server se ha probado y se admite en Linux en las distribuciones indicadas anteriormente. Si decide instalar SQL Server en un sistema operativo no compatible, revise la sección **Directiva de soporte técnico** de la [Directiva de soporte técnico de Microsoft SQL Server](https://support.microsoft.com/help/4047326/support-policy-for-microsoft-sql-server) a fin de comprender las repercusiones para el soporte técnico.

## <a name="system-requirements"></a><a id="system"></a> Requisitos del sistema

SQL Server presenta los siguientes requisitos del sistema para Linux:

|||
|-----|-----|
| **Memoria** | 2 GB |
| **Sistema de archivos** | **XFS** o **EXT4** (no se admiten otros sistemas de archivos, como **BTRFS**) |
| **Espacio en disco** | 6 GB |
| **Velocidad del procesador** | 2 GHz |
| **Núcleos de procesador** | 2 núcleos |
| **Tipo de procesador** | Solo compatible con x64 |

Si usa recursos compartidos remotos de **NFS (Network File System)** en producción, tenga en cuenta los siguientes requisitos de compatibilidad:

- Use la versión de NFS **4.2 o posteriores**. Las versiones anteriores de NFS no admiten las características necesarias (como fallocate y la creación de archivos dispersos) que son comunes en los sistemas de archivos modernos.
- Busque solo los directorios **/var/opt/mssql** en el montaje NFS. No se admiten otros archivos, como binarios del sistema de SQL Server.
- Asegúrese de que los clientes de NFS usen la opción “nolock” al montar el recurso compartido remoto.

## <a name="configure-source-repositories"></a><a id="repositories"></a> Configuración de los repositorios de origen

Al instalar o actualizar SQL Server, se obtiene la versión más reciente de SQL Server desde el repositorio de Microsoft configurado. En los inicios rápidos, se usa el repositorio de actualización acumulativa **CU** para SQL Server. Sin embargo, en su lugar se puede configurar un repositorio **GDR**. Para obtener más información sobre los repositorios y cómo configurarlos, vea [Configuración de repositorios de SQL Server en Linux](sql-server-linux-change-repo.md).

## <a name="install-sql-server"></a><a id="platforms"></a> Instalación de SQL Server

Puede instalar SQL Server 2017 o SQL Server 2019 en Linux mediante la línea de comandos. Para obtener instrucciones paso a paso, vea uno de los inicios rápidos siguientes:

| Plataforma | Inicios rápidos de instalación |
|---|---|
| Red Hat Enterprise Linux (RHEL) | [2017](quickstart-install-connect-red-hat.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-red-hat.md?view=sql-server-linux-ver15&preserve-view=true) |
| SUSE Linux Enterprise Server (SLES) | [2017](quickstart-install-connect-suse.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-suse.md?view=sql-server-linux-ver15&preserve-view=true) |
| Ubuntu | [2017](quickstart-install-connect-ubuntu.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-ubuntu.md?view=sql-server-linux-ver15&preserve-view=true) |
| Docker | [2017](quickstart-install-connect-docker.md?view=sql-server-2017&preserve-view=true) \| [2019](quickstart-install-connect-docker.md?view=sql-server-linux-ver15&preserve-view=true) |

También puede ejecutar SQL Server en Linux en una máquina virtual de Azure. Para obtener más información, consulte [Aprovisionamiento de máquinas virtuales SQL en Azure](/azure/virtual-machines/linux/sql/provision-sql-server-linux-virtual-machine?toc=/sql/toc/toc.json).

Después de instalar, considere la posibilidad de realizar cambios de configuración adicionales para lograr un rendimiento óptimo. Para obtener más información, consulte [Procedimientos recomendados e instrucciones de configuración de SQL Server en Linux](sql-server-linux-performance-best-practices.md).

## <a name="update-or-upgrade-sql-server"></a><a id="upgrade"></a> Instalación o actualización de SQL Server

Para actualizar el paquete **mssql-server** a la versión más reciente, use uno de los siguientes comandos en función de la plataforma:

| Plataforma | Comandos de actualización del paquete |
|-----|-----|
| RHEL | `sudo yum update mssql-server` |
| SLES | `sudo zypper update mssql-server` |
| Ubuntu | `sudo apt-get update`<br/>`sudo apt-get install mssql-server` |

Estos comandos descargan el paquete más reciente y reemplazan los archivos binarios que se encuentran en `/opt/mssql/`. Las bases de datos del sistema y las generadas por el usuario no se ven afectadas por esta operación.

Para actualizar SQL Server, primero [cambie el repositorio configurado](sql-server-linux-change-repo.md) a la versión de SQL Server que quiera. Luego, use el mismo comando **upgrade** para actualizar la versión de SQL Server. Esto solo es posible si la ruta de actualización se admite en ambos repositorios.

## <a name="rollback-sql-server"></a><a id="rollback"></a> Reversión de SQL Server

Para revertir SQL Server o cambiar a una versión anterior, siga estos pasos:

1. Identifique el número de versión del paquete de SQL Server al que quiere cambiar. Para obtener una lista de números de paquete, vea las [Notas de la versión](../linux/sql-server-linux-release-notes.md).

1. Cambie a una versión anterior de SQL Server. En los siguientes comandos, reemplace `<version_number>` por el número de versión de SQL Server que ha identificado en el paso uno.

   | Plataforma | Comandos de actualización del paquete |
   |-----|-----|
   | RHEL | `sudo yum downgrade mssql-server-<version_number>.x86_64` |
   | SLES | `sudo zypper install --oldpackage mssql-server=<version_number>` |
   | Ubuntu | `sudo apt-get install mssql-server=<version_number>`<br/>`sudo systemctl start mssql-server` |

> [!NOTE]
> Solo se admite el cambio a una versión anterior dentro de la misma versión principal, como SQL Server 2019.

## <a name="check-installed-sql-server-version"></a><a id="versioncheck"></a> Comprobación de la versión instalada de SQL Server

Para comprobar la versión actual y la edición de SQL Server en Linux, use el siguiente procedimiento:

1. Si aún no están instaladas, instale las [herramientas de línea de comandos de SQL Server](sql-server-linux-setup-tools.md).

1. Use **sqlcmd** para ejecutar un comando de Transact-SQL que muestre la versión y la edición de SQL Server.

   ```bash
   sqlcmd -S localhost -U SA -Q 'select @@VERSION'
   ```

## <a name="uninstall-sql-server"></a><a id="uninstall"></a> Desinstalación de SQL Server

Para quitar el paquete **mssql-server** en Linux, use uno de los siguientes comandos en función de la plataforma:

| Plataforma | Comandos de eliminación de paquetes |
|-----|-----|
| RHEL | `sudo yum remove mssql-server` |
| SLES | `sudo zypper remove mssql-server` |
| Ubuntu | `sudo apt-get remove mssql-server` |

Al quitar el paquete, no se eliminan los archivos de base de datos generados. Si quiere eliminar los archivos de base de datos, use el siguiente comando:

```bash
sudo rm -rf /var/opt/mssql/
```

## <a name="unattended-install"></a><a id="unattended"></a> Instalación desatendida

Puede realizar una instalación desatendida de la siguiente manera:

- Siga los pasos iniciales de los [inicios rápidos](#platforms) para registrar los repositorios e instalar SQL Server.
- Al ejecutar `mssql-conf setup`, establezca [variables de entorno](sql-server-linux-configure-environment-variables.md) y use la opción `-n` (sin solicitud).

En el ejemplo siguiente se configura la edición Developer de SQL Server con la variable de entorno **MSSQL_PID**. También se acepta el CLUF (**ACCEPT_EULA**) y se establece la contraseña de usuario de SA (**MSSQL_SA_PASSWORD**). El parámetro `-n` realiza una instalación sin solicitudes en la que los valores de configuración se extraen de las variables de entorno.

```bash
sudo MSSQL_PID=Developer ACCEPT_EULA=Y MSSQL_SA_PASSWORD='<YourStrong!Passw0rd>' /opt/mssql/bin/mssql-conf -n setup
```

También puede crear un script que realice otras acciones. Por ejemplo, podría instalar otros paquetes de SQL Server.

Para obtener un script de ejemplo más detallado, vea los ejemplos siguientes:

- [Script de instalación desatendida de Red Hat](sample-unattended-install-redhat.md)
- [Script de instalación desatendida de SUSE](sample-unattended-install-suse.md)
- [Script de instalación desatendida de Ubuntu](sample-unattended-install-ubuntu.md)

## <a name="offline-install"></a><a id="offline"></a> Instalación sin conexión

Si el equipo Linux no tiene acceso a los repositorios en línea que se usan en los [inicios rápidos](#platforms), puede descargar los archivos de paquete directamente. Estos paquetes se encuentran en el repositorio de Microsoft, [https://packages.microsoft.com](https://packages.microsoft.com).

> [!TIP]
> Si los ha instalado correctamente mediante los pasos de los inicios rápidos, no tiene que descargar ni instalar manualmente los paquetes de SQL Server. Esta sección es solo para el escenario sin conexión.

1. **Descargue el paquete del motor de base de datos para la plataforma**. Busque vínculos de descarga de paquetes en la sección de detalles de paquetes de las [Notas de la versión](../linux/sql-server-linux-release-notes.md).

1. **Mueva el paquete descargado al equipo Linux**. Si ha usado otro equipo para descargar los paquetes, una manera de trasladarlos al equipo Linux es con el comando **scp**.

1. **Instale el paquete del motor de base de datos**. Use uno de los siguientes comandos en función de la plataforma. Reemplace el nombre de archivo del paquete de este ejemplo por el nombre exacto que haya descargado.

   | Plataforma | Comandos de instalación del paquete |
   |-----|-----|
   | RHEL | `sudo yum localinstall mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `sudo zypper install mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `sudo dpkg -i mssql-server_versionnumber_amd64.deb` |

    > [!NOTE]
    > También puede instalar los paquetes RPM (RHEL y SLES) con el comando `rpm -ivh`, pero los comandos de la tabla anterior instalan automáticamente las dependencias si están disponibles en los repositorios aprobados.

1. **Resuelva las dependencias que faltan**: Es posible que falten dependencias en este punto. Si no es así, puede omitir este paso. En Ubuntu, si tiene acceso a los repositorios aprobados que contienen esas dependencias, la solución más sencilla es usar el comando `apt-get -f install`. Este comando además completa la instalación de SQL Server. Para inspeccionar manualmente las dependencias, use los siguientes comandos:

   | Plataforma | Comando de enumeración de dependencias |
   |-----|-----|
   | RHEL | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | SLES | `rpm -qpR mssql-server_versionnumber.x86_64.rpm` |
   | Ubuntu | `dpkg -I mssql-server_versionnumber_amd64.deb` |

   Después de resolver las dependencias que faltan, intente instalar de nuevo el paquete mssql-server.

1. **Complete la instalación de SQL Server**. Use **mssql-conf** para completar la instalación de SQL Server:

   ```bash
   sudo /opt/mssql/bin/mssql-conf setup
   ```

## <a name="licensing-and-pricing"></a>Licencias y precios

SQL Server tiene las mismas licencias para Linux y Windows. Para obtener más información sobre las licencias y los precios de SQL Server, vea [Cómo obtener una licencia de SQL Server](https://www.microsoft.com/sql-server/sql-server-2017-pricing).

## <a name="optional-sql-server-features"></a>Características opcionales de SQL Server

Tras la instalación, también puede instalar o habilitar características opcionales de SQL Server.

- [Herramientas de línea de comandos de SQL Server](sql-server-linux-setup-tools.md)
- [Agente SQL Server](sql-server-linux-setup-sql-agent.md)
- [Búsqueda de texto completo de SQL Server](sql-server-linux-setup-full-text-search.md)
- [Machine Learning Services (R, Python)](sql-server-linux-setup-machine-learning.md)
- [SQL Server Integration Services](sql-server-linux-setup-ssis.md)

[!INCLUDE[Get Help Options](../includes/paragraph-content/get-help-options.md)]

> [!TIP]
> Para obtener respuesta a las preguntas más frecuentes, vea [Preguntas más frecuentes sobre SQL Server en Linux](sql-server-linux-faq.md).
