---
title: Configuración de repositorios de Linux para SQL Server 2017 y 2019
description: Vea y configure repositorios de origen para SQL Server 2019 y SQL Server 2017 en Linux. El repositorio de origen afecta a la versión de SQL Server que se aplica durante la instalación y la actualización.
author: VanMSFT
ms.author: vanto
ms.date: 04/28/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
zone_pivot_groups: ld2-linux-distribution
ms.openlocfilehash: d284a049cb1266dc33c30657f3d86e8421825868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340388"
---
# <a name="configure-repositories-for-installing-and-upgrading-sql-server-on-linux"></a>Configuración de repositorios para instalar y actualizar SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

::: zone pivot="ld2-rhel"
En este artículo se explica cómo configurar el repositorio correcto para las instalaciones y las actualizaciones de SQL Server 2017 y SQL Server 2019 en Linux. En la parte superior, la selección actual es **Red Hat (RHEL)** .
::: zone-end

::: zone pivot="ld2-sles"
En este artículo se explica cómo configurar el repositorio correcto para las instalaciones y las actualizaciones de SQL Server 2017 y SQL Server 2019 en Linux. En la parte superior, la selección actual es **SUSE (SLES)** .
::: zone-end

::: zone pivot="ld2-ubuntu"
En este artículo se explica cómo configurar el repositorio correcto para las instalaciones y las actualizaciones de SQL Server 2017 y SQL Server 2019 en Linux. En la parte superior, la selección actual es **Ubuntu**.
::: zone-end

> [!TIP]
> SQL Server 2019 ya está disponible. Para probarlo, use este artículo para configurar el nuevo repositorio **mssql-server-2019**. Luego instale con las instrucciones de la [guía de instalación](sql-server-linux-setup.md).

## <a name="repositories"></a><a id="repositories"></a> Repositorios

Al instalar SQL Server en Linux, debe configurar un repositorio de Microsoft. Este repositorio se usa para adquirir el paquete del motor de base de datos, **mssql-server**, y los paquetes de SQL Server relacionados. Actualmente hay cinco repositorios principales:

| Repositorio | Nombre | Descripción |
|---|---|---|
| **2019** | **mssql-server-2019** | Repositorio de SQL Server 2019 con actualización acumulativa (CU). |
| **2019 GDR** | **mssql-server-2019-gdr** | Repositorio de SQL Server 2019 GDR solo para actualizaciones críticas. |
| **2019 versión preliminar** | **mssql-server-preview** | Repositorio de SQL Server 2019 versión preliminar y RC. |
| **2017** | **mssql-server-2017** | Repositorio SQL Server 2017 Actualización acumulativa (CU). |
| **2017 GDR** | **mssql-server-2017-gdr** | Repositorio SQL Server 2017 GDR solo para actualizaciones críticas. |

## <a name="cumulative-update-versus-gdr"></a><a id="cuversusgdr"></a> Actualización acumulativa frente a GDR

Es importante tener en cuenta que hay dos tipos principales de repositorios para cada distribución:

- **Actualización acumulativa (CU)** : el repositorio Actualización acumulativa (CU) contiene paquetes para la versión básica de SQL Server y las correcciones de errores o mejoras desde esa versión. Las actualizaciones acumulativas son específicas de una versión de lanzamiento, como SQL Server 2019. Se publican periódicamente.

- **GDR**: el repositorio GDR contiene paquetes para la versión básica de SQL Server y solo correcciones críticas y actualizaciones de seguridad desde esa versión. Estas actualizaciones también se agregan a la siguiente versión CU.

Cada versión CU y GDR contiene el paquete de SQL Server completo y todas las actualizaciones anteriores para ese repositorio. Se puede actualizar desde una versión GDR a una versión CU si se cambia el repositorio configurado para SQL Server. También se puede [cambiar a una versión anterior](sql-server-linux-setup.md#rollback) dentro de la versión principal (por ejemplo: 2017).

> [!NOTE]
> Puede actualizar desde una versión GDR a una versión CU en cualquier momento mediante el cambio de repositorios. No se admite la actualización desde una versión CU a una versión GDR.

## <a name="configure-repositories"></a>Configuración de repositorios

::: zone pivot="ld2-rhel"
Siga los pasos de las secciones siguientes para configurar repositorios en Red Hat Enterprise Server (RHEL).
::: zone-end

::: zone pivot="ld2-sles"
Siga los pasos de las secciones siguientes para configurar repositorios en SUSE Linux Enterprise Server (SLES).
::: zone-end

::: zone pivot="ld2-ubuntu"
Siga los pasos de las secciones siguientes para configurar repositorios en Ubuntu.
::: zone-end

## <a name="check-for-previously-configured-repositories"></a>Buscar repositorios configurados previamente

<!--RHEL-->
::: zone pivot="ld2-rhel"
En primer lugar, compruebe si ya ha registrado un repositorio de SQL Server.

1. Vea los archivos del directorio **/etc/yum.repos.d** con el siguiente comando:

   ```bash
   sudo ls /etc/yum.repos.d
   ```

2. Busque un archivo que configure el directorio de SQL Server, como **mssql-server.repo**.

3. Imprima el contenido del archivo.

   ```bash
   sudo cat /etc/yum.repos.d/mssql-server.repo
   ```

4. La propiedad **name** es el repositorio configurado. Puede identificarlo con la tabla de la sección [Repositorios](#repositories) de este artículo.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
En primer lugar, compruebe si ya ha registrado un repositorio de SQL Server.

1. Use **zypper info** para obtener información sobre cualquier repositorio configurado previamente.

   ```bash
   sudo zypper info mssql-server
   ```

2. La propiedad **Repository** es el repositorio configurado. Puede identificarlo con la tabla de la sección [Repositorios](#repositories) de este artículo.

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
En primer lugar, compruebe si ya ha registrado un repositorio de SQL Server.

1. Vea el contenido del archivo **/etc/apt/sources.list**.

   ```bash
   sudo cat /etc/apt/sources.list
   ```

2. Examine la dirección URL del paquete de mssql-server. Puede identificarlo con la tabla de la sección [Repositorios](#repositories) de este artículo.

::: zone-end

## <a name="remove-old-repository"></a>Eliminación de un repositorio antiguo

<!--RHEL-->
::: zone pivot="ld2-rhel"
En caso necesario, quite el repositorio antiguo con el siguiente comando.

```bash
sudo rm -rf /etc/yum.repos.d/mssql-server.repo
```

Este comando supone que el archivo identificado en la sección anterior se denominaba **mssql-server.repo**.

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
En caso necesario, quite el repositorio antiguo. Use uno de los siguientes comandos en función del tipo de repositorio configurado previamente.

| Repositorio | Comando para quitar |
|---|---|
| **Versión preliminar (2019)** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-preview'` |
| **2019 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019'` |
| **2019 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2019-gdr'`|
| **2017 CU** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017'` |
| **2017 GDR** | `sudo zypper removerepo 'packages-microsoft-com-mssql-server-2017-gdr'`|

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
En caso necesario, quite el repositorio antiguo. Use uno de los siguientes comandos en función del tipo de repositorio configurado previamente.

> [!NOTE]
> A partir de SQL Server 2019 CU3 y SQL Server 2017 CU20, se admite Ubuntu 18.04. Si usa Ubuntu 16.04, cambie la ruta de acceso siguiente a `/ubuntu/16.04` en lugar de `/ubuntu/18.04`y use el [nombre del código de distribución](https://releases.ubuntu.com/) correcto.

| Repositorio | Comando para quitar |
|---|---|
| **Versión preliminar (2019)** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-preview xenial main'` |
| **2019 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019 bionic main'` | 
| **2019 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/mssql-server-2019-gdr bionic main'` |
| **2017 CU** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017 xenial main'` | 
| **2017 GDR** | `sudo add-apt-repository -r 'deb [arch=amd64] https://packages.microsoft.com/ubuntu/16.04/mssql-server-2017-gdr xenial main'` |

::: zone-end

## <a name="configure-new-repository"></a>Configuración de un nuevo repositorio

<!--RHEL-->
::: zone pivot="ld2-rhel"
Configure el nuevo repositorio que se va a usar para las instalaciones y las actualizaciones de SQL Server. Use uno de los siguientes comandos para configurar el repositorio que prefiera.

> [!NOTE]
> Los siguientes comandos para SQL Server 2019 apunta al repositorio de RHEL 8. RHEL 8 no viene preinstalado con python2. SQL Server requiere RHEL 8. Para obtener más información, vea el siguiente blog sobre la instalación de python2 y su configuración como intérprete predeterminado: https://www.redhat.com/en/blog/installing-microsoft-sql-server-red-hat-enterprise-linux-8-beta.
>
> A partir de SQL Server 2017 CU20, se admite RHEL 8.
>
> Si usa RHEL 7 o RHEL 8, asegúrese de que las rutas de acceso coinciden con `/rhel/7` o `/rhel/8`. Nuestros paquetes son independientes de las versiones secundarias de RHEL. Esto significa que, si usa RHEL 7.6, tendrá que usar la ruta de acceso `/rhel/7` para configurar el repositorio.

| Repositorio | Versión | Get-Help |
|---|---|---|
| **2019 CU** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo curl -o /etc/yum.repos.d/mssql-server.repo https://packages.microsoft.com/config/rhel/8/mssql-server-2017-gdr.repo` |

::: zone-end

<!--SLES-->
::: zone pivot="ld2-sles"
Configure el nuevo repositorio que se va a usar para las instalaciones y las actualizaciones de SQL Server. Use uno de los siguientes comandos para configurar el repositorio que prefiera.

| Repositorio | Versión | Get-Help |
|---|---|---|
| **2019 CU** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019.repo` |
| **2019 GDR** | 2019 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2019-gdr.repo` |
| **2017 CU** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo` |
| **2017 GDR** | 2017 | `sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017-gdr.repo` |

::: zone-end

<!--Ubuntu-->
::: zone pivot="ld2-ubuntu"
Configure el nuevo repositorio que se va a usar para las instalaciones y las actualizaciones de SQL Server.

> [!NOTE]
> A partir de SQL Server 2019 CU3 y SQL Server 2017 CU20, se admite Ubuntu 18.04. Los siguientes comandos apuntan al repositorio de Ubuntu 18.04.
>
> Si usa Ubuntu 16.04, cambie la ruta de acceso siguiente a `/ubuntu/16.04` en lugar de `/ubuntu/18.04`.

1. Importe las claves de GPG del repositorio público.

   ```bash
   sudo curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
   ```

2. Use uno de los siguientes comandos para configurar el repositorio que prefiera.

   | Repositorio | Versión | Get-Help |
   |---|---|---|
   | **2019 CU** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019.list)"` |
   | **2019 GDR** | 2019 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2019-gdr.list)"` |
   | **2017 CU** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017.list)"` |
   | **2017 GDR** | 2017 | `sudo add-apt-repository "$(curl https://packages.microsoft.com/config/ubuntu/18.04/mssql-server-2017-gdr.list)"` |

3. Ejecute **apt-get update**.

   ```bash
   sudo apt-get update
   ```

::: zone-end

## <a name="next-steps"></a>Pasos siguientes

Una vez configurado el repositorio correcto, puede continuar con la [instalación](sql-server-linux-setup.md#platforms) o la [actualización](sql-server-linux-setup.md#upgrade) de SQL Server y los paquetes relacionados del nuevo repositorio.

::: zone pivot="ld2-rhel"
> [!IMPORTANT]
> En este punto, si decide usar el [Inicio rápido de RHEL](quickstart-install-connect-red-hat.md), recuerde que ya ha configurado el repositorio de destino. No repita ese paso en los tutoriales. Esto es especialmente así si configura el repositorio GDR, ya que el inicio rápido usa el repositorio CU.
::: zone-end

::: zone pivot="ld2-sles"
> [!IMPORTANT]
> En este punto, si decide usar el [Inicio rápido de SLES](quickstart-install-connect-suse.md), recuerde que ya ha configurado el repositorio de destino. No repita ese paso en los tutoriales. Esto es especialmente así si configura el repositorio GDR, ya que el inicio rápido usa el repositorio CU.
::: zone-end

::: zone pivot="ld2-ubuntu"
> [!IMPORTANT]
> En este punto, si decide usar el [Inicio rápido de Ubuntu](quickstart-install-connect-ubuntu.md), recuerde que ya ha configurado el repositorio de destino. No repita ese paso en los tutoriales. Esto es especialmente así si configura el repositorio GDR, ya que el inicio rápido usa el repositorio CU.
::: zone-end

Para obtener más información sobre cómo instalar SQL Server 2017 en Linux, vea [Guía de instalación para SQL Server en Linux](sql-server-linux-setup.md).
