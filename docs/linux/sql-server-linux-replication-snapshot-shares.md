---
title: Configuración de los recursos compartidos de la carpeta de instantáneas
titleSuffix: SQL Server on Linux
description: Obtenga información sobre cómo configurar la replicación de SQL Server de los recursos compartidos de la carpeta de instantáneas en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 09/24/2018
ms.topic: article
ms.prod: sql
ms.technology: linux
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: fdd43008abb9ba49ab3a5e3a2859a6366a3a6542
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346475"
---
# <a name="configure-replication-snapshot-folder-with-shares"></a>Configuración de la carpeta de instantáneas de replicación con recursos compartidos

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

La carpeta de instantáneas es un directorio designado como recurso compartido; los agentes que leen y escriben en esta carpeta deben tener permisos suficientes para acceder a ella.

![diagrama de replicación][1]

### <a name="replication-snapshot-folder-share-explained"></a>Explicación del recurso compartido de instantáneas de replicación

Antes de los ejemplos, echemos un vistazo al uso que hace SQL Server de los recursos compartidos de Samba en la replicación. A continuación se muestra un ejemplo básico de su funcionamiento.

1. Los recursos compartidos de Samba se configuran de manera que los archivos que los agentes de replicación escriben en `/local/path1` puedan ser vistos por el suscriptor.
2. SQL Server está configurado para usar rutas de acceso de recursos compartidos al establecer el publicador en el servidor de distribución, de modo que todas las instancias mirarían a `//share/path`.
3. SQL Server encuentra la ruta de acceso local de `//share/path` para saber dónde tiene que buscar los archivos.
4. SQL Server lee y escribe en las rutas de acceso locales respaldadas por un recurso compartido de Samba.


## <a name="configure-a-samba-share-for-the-snapshot-folder"></a>Configuración de un recurso compartido de Samba para la carpeta de instantáneas 

Los agentes de replicación necesitarán un directorio compartido entre los hosts de replicación para tener acceso a las carpetas de instantáneas de otras máquinas. Por ejemplo, en la replicación de extracción transaccional, el agente de distribución reside en el suscriptor, que requiere acceso al distribuidor para obtener artículos. En esta sección, veremos un ejemplo de cómo configurar un recurso compartido de Samba en dos hosts de replicación.


## <a name="steps"></a>Pasos

Por ejemplo, configuraremos una carpeta de instantáneas en el host 1 (el distribuidor) que se compartirá con el host 2 (el suscriptor) mediante Samba. 

### <a name="install-and-start-samba-on-both-machines"></a>Instalación e inicio de Samba en ambos equipos 

En Ubuntu:

```bash
sudo apt-get -y install samba
sudo service smbd restart
```

En RHEL:

```bash
sudo yum install samba
sudo service smb start
sudo service smb status
```

### <a name="on-host-1-distributor-set-up-the-samba-share"></a>Configurar en el host 1 (distribuidor) el recurso compartido de Samba 

1. Configure el usuario y la contraseña para Samba:

  ```bash
  sudo smbpasswd -a mssql 
  ```

1. Edite `/etc/samba/smb.conf` para incluir la entrada siguiente y rellene los campos *share_name* y *path*.
 ```bash
  <[share_name]>
  path = </local/path/on/host/1>
  writable = yes
  create mask = 770
  directory mask 
  valid users = mssql 
  ```

  **Ejemplo**

  ```bash
  [mssql_data]    <- Name of the shared directory
  path = /var/opt/mssql/repldata <- location of directory we wish to share
  writable = yes  <- determines if the share is writable from other hosts
  create mask = 770  <- Linux permissions for files created 
  directory mask = 770 <- Linux permissions for directories created
  valid users = mssql   <- list of users who can login to this share
  ```

### <a name="on-host-2-subscriber--mount-the-samba-share"></a>Montar en el host 2 (suscriptor) el recurso compartido de Samba

Edite el comando con las rutas de acceso correctas y ejecute el siguiente comando en el equipo 2:

  ```bash
  sudo mount //<name_of_host_1>/<share_name> </local/path/on/host/2> -o user=mssql,uid=mssql,gid=mssql
  ```

  **Ejemplo**

  ```bash
  mount //host1/mssql_data /var/opt/mssql/repldata_shared -o user=mssql,uid=mssql,gid=mssql

  user=mssql <- sets the login name for samba
  uid=mssql   <- makes the mssql user as the owner of the mounted directory
  gid=mssql   <- sets the mssql group as the owner of the mounted directory
  ```

### <a name="on-both-hosts--configure-sql-server-on-linux-instances-to-use-snapshot-share"></a>Configurar en ambos hosts las instancias de SQL Server en Linux para que usen el recurso compartido de instantáneas

Agregue la sección siguiente a `mssql.conf` en ambos equipos. Use la ubicación del recurso compartido de Samba para //share/path. En este ejemplo, sería `//host1/mssql_data`

  ```bash
  [uncmapping]
  //share/path = /local/path/on/hosts/
  ```

  **Ejemplo**

  En el host 1:

  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/1
  ```

  En el host 2:
  
  ```bash
  [uncmapping]
  //host1/mssql_data = /local/path/on/hosts/2
  ```

### <a name="configuring-publisher-with-shared-paths"></a>Configuración del publicador con rutas de acceso compartidas

* Al configurar la replicación, use la ruta de acceso de los recursos compartidos (por ejemplo, `//host1/mssql_data`).
* Asigne `//host1/mssql_data` a un directorio local y la asignación agregada a `mssql.conf`.

## <a name="next-steps"></a>Pasos siguientes

[Conceptos: Replicación de SQL Server en Linux](sql-server-linux-replication.md)

[Procedimientos almacenados de replicación](../relational-databases/system-stored-procedures/replication-stored-procedures-transact-sql.md)

[1]: ./media/sql-server-linux-replication-snapshot-shares/image1.png
