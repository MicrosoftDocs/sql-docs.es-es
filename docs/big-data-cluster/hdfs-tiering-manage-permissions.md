---
title: Permisos de niveles de HDFS para clústeres de macrodatos de SQL Server
titleSuffix: Manage HDFS tiering permissions for SQL Server Big Data Clusters
description: Administre la seguridad de los niveles de HDFS en clústeres de macrodatos de SQL Server como permisos en otros sistemas basados en Linux.
author: nelgson
ms.author: negust
ms.reviewer: mikeray
ms.date: 11/04/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 0f791cd64dd177e01dc086dd08f9216bda4a55b4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100046690"
---
# <a name="manage-hdfs-permissions-for-big-data-clusters-2019"></a>Administración de permisos de HDFS para [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

HDFS es similar, como sistema de archivos, a los sistemas de archivos basados en Linux que usan POSIX para los permisos de archivo. Además del modelo de permisos de POSIX tradicional, HDFS también admite listas de control de acceso (ACL) de POSIX. Para obtener más información, consulte el [artículo de Apache Hadoop sobre las ACL](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_.28Access_Control_Lists.29).

En las secciones siguientes se proporcionan ejemplos de cómo usar [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] para administrar los permisos de archivo y directorio de HDFS.

## <a name="prerequisites"></a>Prerequisites

- [Clúster de macrodatos implementado](deployment-guidance.md)
- [Herramientas de macrodatos](deploy-big-data-tools.md)
  
## <a name="hdfs-shell"></a>Shell de HDFS

La funcionalidad de shell de `hdfs` en [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)] permite emitir comandos directamente en un shell para administrar los permisos de HDFS en archivos y directorios. El mecanismo subyacente usa llamadas a WebHDFS para emitir los comandos.

El comando siguiente abrirá el shell.

```bash
azdata bdc hdfs shell
```

Para acceder a la ayuda del shell de `hdfs` y comprender cómo emitir comandos, ejecute el comando siguiente una vez que el shell esté activo.

```bash
[hdfs] ?
```

En el ejemplo siguiente se muestra cómo crear un directorio, enumerar directorios, modificar permisos en un directorio y asignar a un usuario llamado `bob` acceso de lectura, escritura y ejecución al directorio `sales`.

```bash
[hdfs] mkdir sales
[hdfs] ls
rwxr-xr-x  hdfs bdcadmins        0 Oct 09 18:02 system/
rwxrwxr-x admin bdcadmins        0 Oct 10 16:47 sales/
--xrwxrwxrwx  hdfs bdcadmins        0 Oct 09 18:03 tmp/
rwxrwxrwx  hdfs bdcadmins        0 Oct 09 17:59 user/

[hdfs] acl modify  '/sales/' 'user:bob:rwx'
acl modify: Change completed.
[hdfs] acl status  '/sales/'
{
  `AclStatus`: {
    `entries`: [
      `user:bob:rwx`,
      `group::r-x`
    ],
    `group`: `bdcadmins`,
    `owner`: `admin`,
    `permission`: `775`,
    `stickyBit`: false
  }
}
```

## <a name="create-a-directory-in-hdfs-using-azure-data-cli-azdata"></a>Creación de un directorio en HDFS mediante [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]

Cree un directorio denominado `data` en la ruta de acceso `/sales`.

```bash
azdata bdc hdfs mkdir --path '/sales/data'
```

## <a name="change-owner-of-a-directory-or-file"></a>Cambio del propietario de un directorio o archivo

Cambie el usuario propietario del directorio `data` de HDFS y convierta a *`alice`* en el usuario propietario y *`salesgroup`* en el grupo propietario. Para cambiar el propietario, es necesario ser propietario.

```bash
azdata bdc hdfs chown --owner alice --group 'salesgroup' --path '/sales/data'
```

## <a name="change-permissions-of-a-file-or-directory-with-chmod"></a>Cambio de los permisos de un archivo o un directorio con `chmod`

Use `chmod` para cambiar los permisos en archivos y directorios (para el propietario, el grupo propietario y demás). Para obtener más información, vea cómo [cambiar permisos en un sistema de archivos de Linux](https://www.lifewire.com/uses-of-command-chmod-2201064). En HDFS, el patrón es el mismo. Por ejemplo:

```bash
azdata bdc hdfs chmod --permission 750 --path /sales/data
```

```bash
azdata bdc hdfs chmod --permission 775 --path /sales/data/file.txt
```

## <a name="set-sticky-bit-on-directories"></a>Establecimiento del bit de permanencia en los directorios

Establezca el bit de permanencia en los directorios para evitar la eliminación o reubicación involuntaria de los archivos. El bit de permanencia limita el permiso para eliminar o mover un archivo al superusuario, el propietario del directorio o el propietario del archivo. Esta opción no afecta al archivo. En el ejemplo siguiente se establece un bit de permanencia en el directorio `users` mediante la adición del prefijo `1` a los permisos.

```bash
azdata bdc hdfs chmod --path /sales/users --permission 1750
```

## <a name="setting-acls-on-files-and-directories"></a>Establecimiento de listas de control de acceso (ACL) en archivos y directorios

Para establecer ACL en archivos y directorios en HDFS, use los comandos [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)].

Establezca ACL en un directorio y asigne al usuario llamado *`tom`* acceso de lectura, escritura y ejecución al directorio *`data`* . 

> [!NOTE]
> Al usar el comando `set`, asegúrese de que está proporcionando la especificación de ACL completa, incluida la especificación de ACL para el usuario propietario, el grupo propietario y demás.

```bash
azdata bdc hdfs acl set --path '/sales' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-'
```

## <a name="default-acl-on-directories"></a>ACL predeterminada en directorios

La ACL predeterminada permite que los subdirectorios hereden permisos del directorio principal. Solo los directorios pueden tener una ACL predeterminada. Cuando se crea un archivo o un subdirectorio, este hereda automáticamente la ACL predeterminada de su elemento primario en su propia ACL de acceso. De esta manera, la ACL predeterminada se heredará a través de niveles de directorio con una profundidad arbitraria a medida que se creen otros subdirectorios.

A continuación se muestra un ejemplo de cómo establecer la ACL predeterminada mediante azdata.

```bash
azdata bdc hdfs acl set --path '/sale' --aclspec  'user::rw-,user:tom:rwx,group::rw-,other::rw-,default:group::rw-,default:user::rw-,default:other::rw-'
```

## <a name="next-steps"></a>Pasos siguientes

- [Referencia de [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/reference/reference-azdata.md)

- [¿Qué son los [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]?](big-data-cluster-overview.md)