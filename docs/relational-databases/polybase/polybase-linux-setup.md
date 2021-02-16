---
title: Instalación de PolyBase en Linux
titlesuffix: SQL Server
description: Obtenga información sobre cómo instalar SQL Server PolyBase en Linux. PolyBase le permite ejecutar consultas externas en orígenes de datos remotos.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: dakryze
ms.date: 8/18/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
monikerRange: '>= sql-server-linux-ver15 || >= sql-server-ver15'
ms.openlocfilehash: 85beab1eb4d256c0433678ca34a8ac2627ebfb52
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062270"
---
# <a name="install-polybase-on-linux"></a>Instalación de PolyBase en Linux

[!INCLUDE [sqlserver2019-linux](../../includes/applies-to-version/sqlserver2019-linux.md)]

Siga estos pasos para instalar [PolyBase](../../relational-databases/polybase/polybase-guide.md) (`mssql-server-polybase` y `mssql-server-polybase-hadoop`) en Linux. PolyBase le permite ejecutar consultas externas en orígenes de datos remotos.

>[!NOTE]
> Antes de instalar PolyBase, hay que [instalar SQL Server 2019](../../linux/sql-server-linux-setup.md#platforms). Esto configura las claves y los repositorios que se usan para instalar el paquete `mssql-server-polybase` y `mssql-server-polybase-hadoop`.

>[!NOTE]
>
> - PolyBase no es compatible con SQL Server 2017 para Linux.
> - Actualmente la escalabilidad horizontal de PolyBase en Linux está deshabilitada.

Instalación de PolyBase en el sistema operativo:

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name="install-on-rhel"></a><a name="RHEL"></a>Instalación en RHEL

Use el comando siguiente para instalar el paquete `mssql-server-polybase` en Red Hat Enterprise Linux. 

```bash
sudo yum install -y mssql-server-polybase
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Después de la instalación, debe [habilitar la característica PolyBase](#enable).

Use el siguiente comando para instalar `mssql-server-polybase-hadoop`. 

```bash
sudo yum install -y mssql-server-polybase-hadoop
```

El paquete de PolyBase Hadoop tiene dependencias en los siguientes paquetes:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

La instalación le pide que reinicie `launchpadd`. Para ello, use el comando siguiente.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Después de la instalación, debe [establecer el nivel de conectividad de Hadoop](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md#c-set-hadoop-connectivity).

Si necesita una instalación sin conexión, busque la descarga del paquete PolyBase en [Notas de la versión](../../linux/sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](../../linux/sql-server-linux-setup.md#offline).

## <a name="install-on-ubuntu"></a><a name="ubuntu"></a>Instalación en Ubuntu

Use el comando siguiente para instalar el paquete `mssql-server-polybase` en Ubuntu. 

```bash
sudo apt-get install mssql-server-polybase
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Después de la instalación, debe [habilitar la característica PolyBase](#enable).

Si necesita una instalación sin conexión, busque la descarga del paquete PolyBase en [Notas de la versión](../../linux/sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](../../linux/sql-server-linux-setup.md#offline).

Use el siguiente comando para instalar `mssql-server-polybase-hadoop`. 

```bash
sudo apt-get install mssql-server-polybase-hadoop
```

El paquete de PolyBase Hadoop tiene dependencias en los siguientes paquetes:
- `mssql-server`
- `mssql-server-polybase`
- `mssql-server-extensibility`
- `mssql-zulu-jre-11`. 

La instalación le pide que reinicie `launchpadd`. Para ello, use el comando siguiente.

```bash
sudo systemctl restart mssql-launchpadd
```

>[!NOTE]
>Después de la instalación, debe [establecer el nivel de conectividad de Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md#configure-hadoop-connectivity).

## <a name="install-on-sles"></a><a name="SLES"></a>Instalación en SLES

Use los comandos siguientes para instalar el paquete `mssql-server-polybase` en SUSE Linux Enterprise Server. 

```bash
sudo zypper install mssql-server-polybase
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```bash
sudo systemctl restart mssql-server
```

>[!NOTE]
>Después de la instalación, debe [habilitar la característica PolyBase](#enable).

Si necesita una instalación sin conexión, busque la descarga del paquete PolyBase en [Notas de la versión](../../linux/sql-server-linux-release-notes.md). Luego use los mismos pasos de instalación sin conexión descritos en el artículo [Instalar SQL Server](../../linux/sql-server-linux-setup.md#offline).


## <a name="enable-polybase"></a><a name="enable"></a> Habilitar PolyBase

Tras la instalación, se debe habilitar PolyBase para acceder a sus características. Conéctese a la instancia de SQL Server instalada y use el comando de Transact-SQL siguiente para habilitarlo.

```sql
exec sp_configure @configname = 'polybase enabled', @configvalue = 1;
RECONFIGURE WITH OVERRIDE;
```

## <a name="update-polybase"></a>Actualización de PolyBase

Si ya tiene `mssql-server-polybase` instalado, se puede actualizar a la versión más reciente con los comandos siguientes:

### <a name="rhel"></a>RHEL

```bash
sudo yum remove -y mssql-server-polybase-hadoop
sudo yum remove -y mssql-server-polybase
sudo yum check-update
sudo yum install -y mssql-server-polybase
sudo yum install -y mssql-server-polybase-hadoop
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```
sudo systemctl restart mssql-server
```

### <a name="ubuntu"></a>Ubuntu

```bash
sudo apt-get remove mssql-server-polybase-hadoop
sudo apt-get remove mssql-server-polybase
sudo apt-get update 
sudo apt-get install mssql-server-polybase
sudo apt-get remove mssql-server-polybase-hadoop
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```
sudo systemctl restart mssql-server
```

### <a name="sles"></a>SLES

```bash
sudo zypper remove mssql-server-polybase
sudo zypper refresh
sudo zypper install mssql-server-polybase
```

Se le pedirá que reinicie la instancia de SQL Server. Para ello, use el comando siguiente.

```
sudo systemctl restart mssql-server
```

>[!NOTE]
>Después de la instalación, debe [habilitar la característica PolyBase](#enable).

## <a name="next-steps"></a>Pasos siguientes

PolyBase en Linux puede tener acceso a los orígenes de datos siguientes. Siga los vínculos proporcionados para obtener más información sobre cómo crear una tabla externa a partir de estos orígenes cuando PolyBase está habilitado. 

- [SQL Server, SQL Database, Azure Synapse Analytics)](../../relational-databases/polybase/polybase-configure-sql-server.md)
- [Hadoop](../../relational-databases/polybase/polybase-configure-hadoop.md)
- [Azure Blob Storage](../../relational-databases/polybase/polybase-configure-azure-blob-storage.md)
- [Oracle](../../relational-databases/polybase/polybase-configure-oracle.md)
- [Teradata](../../relational-databases/polybase/polybase-configure-teradata.md)
- [MongoDB (y Cosmos DB)](../../relational-databases/polybase/polybase-configure-mongodb.md)

Para obtener más información sobre cómo se usa, consulte el artículo de referencia de Transact-SQL relativo a [CREATE EXTERNAL TABLE](../../t-sql/statements/create-external-table-transact-sql.md).
