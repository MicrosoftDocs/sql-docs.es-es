---
title: Configuración del Agente SQL Server en Linux
description: Obtenga información sobre cómo habilitar o instalar el Agente SQL Server en Linux. A partir de SQL Server 2017 CU4, el Agente SQL Server se incluye con el paquete mssql-server.
author: VanMSFT
ms.author: vanto
ms.date: 12/05/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 77f16adc-e6cb-4a57-82f3-7b9780369868
ms.openlocfilehash: 8ca8de4b922ad2108ad40996713cf47501b29dca
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346397"
---
# <a name="install-sql-server-agent-on-linux"></a>Instalar el Agente SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo habilitar o instalar el Agente SQL Server en Linux.

El [Agente SQL Server](../ssms/agent/sql-server-agent.md) ejecuta trabajos programados de SQL Server. A partir de SQL Server 2017 CU4, el Agente SQL Server se incluye con el paquete **mssql-server** y está deshabilitado de forma predeterminada. Para obtener información sobre las características admitidas en esta versión del Agente SQL Server junto con información de versión, vea las [Notas de la versión](sql-server-linux-release-notes.md).

## <a name="instructions"></a>Instructions

Antes de usar el Agente SQL Server en Linux, siga estos pasos para habilitarlo o instalarlo.

1. Agregue su nombre de host (con y sin dominio) en los archivos `/etc/hosts`. En las siguientes líneas se muestra un ejemplo del formato de estas entradas:

   ```bash
   "IP Address" "hostname"
   "IP Address" "hostname.domain.com"
   ```

1. Siga las instrucciones de una de las siguientes secciones en función de su versión de SQL Server:

   | Versiones | Instructions |
   |---|---|
   | SQL Server 2017 CU4 y versiones posteriores</br>SQL Server 2019 | [Habilitar el Agente SQL Server](#EnableAgentAfterCU4) |
   | SQL Server 2017 CU3 y versiones anteriores | [Instalar el Agente SQL Server](#InstallAgentBelowCU4) |

## <a name="enable-the-sql-server-agent"></a><a id="EnableAgentAfterCU4"></a>Habilitar el Agente SQL Server

Para SQL Server 2019 y SQL Server 2017 CU4 y versiones posteriores, solo tiene que habilitar el Agente SQL Server. No es necesario instalar un paquete independiente.

Para habilitar el Agente SQL Server, siga los pasos siguientes.

```bash
sudo /opt/mssql/bin/mssql-conf set sqlagent.enabled true 
sudo systemctl restart mssql-server
```

> [!NOTE]
> Si va a actualizar desde 2017 CU3 o anterior con el agente instalado, el Agente SQL Server se habilita automáticamente y se desinstalan los paquetes anteriores.  

## <a name="install-the-sql-server-agent"></a><a name="InstallAgentBelowCU4"></a>Instalar el Agente SQL Server

Para SQL Server 2017 CU3 y versiones anteriores, debe instalar el paquete del Agente SQL Server.

> [!NOTE]
> Las instrucciones de instalación siguientes se aplican a SQL Server versiones 2017 CU3 y anteriores. Antes de instalar el Agente SQL Server, [instale SQL Server](sql-server-linux-setup.md#platforms). Con esto se configuran las claves y los repositorios que se usan al instalar el paquete **mssql-server-agent**.

Instale el Agente SQL Server para la plataforma:
- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

### <a name=""></a><a name="RHEL">Instalación en RHEL</a>

Use los pasos siguientes para instalar **mssql-server-agent** en Red Hat Enterprise Linux. 

```bash
sudo yum install mssql-server-agent
sudo systemctl restart mssql-server
```

Si ya tiene instalado **mssql-server-agent**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo yum check-update
sudo yum update mssql-server-agent
sudo systemctl restart mssql-server
```

Si necesita una instalación sin conexión, busque la descarga del paquete del Agente SQL Server en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="ubuntu">Instalación en Ubuntu</a>

Use los siguientes pasos para instalar **mssql-server-agent** en Ubuntu. 

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si ya tiene instalado **mssql-server-agent**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo apt-get update 
sudo apt-get install mssql-server-agent
sudo systemctl restart mssql-server
```

Si necesita una instalación sin conexión, busque la descarga del paquete del Agente SQL Server en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

### <a name=""></a><a name="SLES">Instalación en SLES</a>

Use los pasos siguientes para instalar **mssql-server-agent** en SUSE Linux Enterprise Server. 

Instale **mssql-server-agent**. 

```bash
sudo zypper install mssql-server-agent
sudo systemctl restart mssql-server
```

Si ya tiene instalado **mssql-server-agent**, puede actualizar a la versión más reciente con los comandos siguientes:

```bash
sudo zypper refresh
sudo zypper update mssql-server-agent
sudo systemctl restart mssql-server
```

Si necesita una instalación sin conexión, busque la descarga del paquete del Agente SQL Server en las [Notas de la versión](sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](sql-server-linux-setup.md#offline).

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre cómo usar el Agente SQL Server para crear, programar y ejecutar trabajos, vea [Ejecución de un trabajo del Agente SQL Server en Linux](sql-server-linux-run-sql-server-agent-job.md).