---
title: 'Configuración de FCI para almacenamiento NFS: SQL Server en Linux'
description: Aprenda a configurar una instancia de clúster de conmutación por error (FCI) mediante almacenamiento NFS para SQL Server en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 90240b383e19fefd254cd5f28b6d113b40b20d98
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346312"
---
# <a name="configure-failover-cluster-instance---nfs---sql-server-on-linux"></a>Configuración de la instancia de clúster de conmutación por error: NFS: SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo configurar el almacenamiento NFS de una instancia de clúster de conmutación por error (FCI) en Linux. 

NFS, o Network File System, es un método común para compartir discos en el entorno de Linux, pero no en el de Windows. De forma similar a iSCSI, NFS se puede configurar en un servidor o en algún tipo de dispositivo o unidad de almacenamiento siempre que cumpla los requisitos de almacenamiento para SQL Server.

## <a name="important-nfs-server-information"></a>Información importante del servidor NFS

El NFS host de origen (ya sea un servidor Linux u otro dispositivo) debe usar, o ser compatible con, la versión 4.2 o posterior. Las versiones anteriores no funcionan con SQL Server en Linux.

Al configurar las carpetas que se van a compartir en el servidor NFS, asegúrese de seguir estas opciones generales:
- `rw` para asegurarse de que es posible leer la carpeta y escribir en ella
- `sync` para garantizar las escrituras garantizadas en la carpeta
- No use `no_root_squash` como opción; se considera un riesgo de seguridad
- Asegúrese de que la carpeta tiene aplicados derechos completos (777)

Asegúrese de que los estándares de seguridad se aplican para el acceso. Al configurar la carpeta, asegúrese de que solo los servidores que participan en la FCI deben ver la carpeta NFS. A continuación se muestra un ejemplo de /etc/exports modificado en una solución NFS basada en Linux, donde la carpeta está restringida a FCIN1 y FCIN2.

![Captura de pantalla de un ejemplo de /etc/exports modificado en una solución NFS basada en Linux, donde la carpeta está restringida a FCIN1 y FCIN2.][1]

## <a name="instructions"></a>Instructions

1. Elija uno de los servidores que participarán en la configuración de la FCI. No importa el que elija. 

2. Compruebe que el servidor puede ver los montajes en el servidor NFS.

    ```bash
    sudo showmount -e <IPAddressOfNFSServer>
    ```

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar.

3. En el caso de las bases de datos del sistema o cualquier elemento almacenado en la ubicación de datos predeterminada, siga estos pasos. De lo contrario, vaya al paso 4.
 
   * Asegúrese de que SQL Server está detenido en el servidor en el que está trabajando.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```
   * Cambie por completo para ser el superusuario. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    sudo -i
    ```

   * Cambie para ser el usuario de mssql. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    su mssql
    ```

   * Cree un directorio temporal para almacenar los archivos de registro y los datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    mkdir <TempDir>
    ```

    \<TempDir> es el nombre de la carpeta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/tmp.

    ```bash
    mkdir /var/opt/mssql/tmp
    ```

   * Copie los archivos de registro y los datos de SQL Server en el directorio temporal. No recibirá ninguna confirmación si se realiza correctamente.
    
    ```bash
    cp /var/opt/mssql/data/* <TempDir>
    ```

    \<TempDir> es el nombre de la carpeta del paso anterior.

   * Compruebe que los archivos están en el directorio.

    ```bash
    ls TempDir
    ```

    \<TempDir> es el nombre de la carpeta del paso d.

   * Elimine los archivos del directorio de datos de SQL Server existente. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    rm - f /var/opt/mssql/data/*
    ```

   * Compruebe que se han eliminado los archivos. 

    ```bash
    ls /var/opt/mssql/data
    ```
    
   * Escriba exit para volver al usuario raíz.

   * Monte el recurso compartido de NFS en la carpeta de datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar. 

    \<FolderOnNFSServer> es el nombre del recurso compartido de NFS. La siguiente sintaxis de ejemplo coincide con la información de NFS del paso 2.

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

   * Para comprobar que el montaje se ha realizado correctamente, envíe mount sin modificadores.

    ```bash
    mount
    ```

    ![Captura de pantalla del comando mount y la respuesta al comando en la que no se muestra ningún modificador.][2]

   * Cambie al usuario de mssql. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    su mssql
    ```

   * Copie los archivos del directorio temporal /var/opt/mssql/data. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    cp /var/opt/mssql/tmp/* /var/opt/mssqldata
    ```

   * Compruebe que los archivos están allí.

    ```bash
    ls /var/opt/mssql/data
    ```

   * Escriba exit para no ser mssql. 
    
   * Escriba exit para no ser el usuario raíz.

   * Inicie SQL Server. Si todo se ha copiado correctamente y la seguridad se ha aplicado correctamente, SQL Server debería mostrarse como iniciado.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```
    
   * Cree una base de datos para comprobar que la seguridad se ha configurado correctamente. En el ejemplo siguiente se muestra cómo se hace mediante Transact-SQL; puede realizarse mediante SSMS.
 
    ![CreateTestdatabase][3]

   * Detenga SQL Server y compruebe que se ha apagado.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

   * Si no va a crear ningún otro montaje NFS, desmonte el recurso compartido. Si lo va a hacer, no desmonte.

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar.

    \<FolderOnNFSServer> es el nombre del recurso compartido de NFS.

    \<FolderMountedIn> es la carpeta creada en el paso anterior. 

4. Para comprobar otros aspectos que no sean las bases de datos del sistema (por ejemplo, las bases de datos de usuario o las copias de seguridad), siga estos pasos. Si solo usa la ubicación predeterminada, vaya al paso 5.

   * Cambie para ser el superusuario. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    sudo -i
    ```

   * Cree una carpeta que usará SQL Server. 

    ```bash
    mkdir <FolderName>
    ```

    \<FolderName> es el nombre de la carpeta. Es necesario especificar la ruta de acceso completa de la carpeta si no está en la ubicación correcta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/userdata.

    ```bash
    mkdir /var/opt/mssql/userdata
    ```

   * Monte el recurso compartido de NFS en la carpeta creada en el paso anterior. No recibirá ninguna confirmación si se realiza correctamente.

    ```bash
    Mount -t nfs4 <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn> -o nfsvers=4.2,timeo=14,intr
    ```

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar.

    \<FolderOnNFSServer> es el nombre del recurso compartido de NFS.

    \<FolderToMountIn> es la carpeta creada en el paso anterior. A continuación se muestra un ejemplo. 

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci2 /var/opt/mssql/userdata -o nfsvers=4.2,timeo=14,intr
    ```

   * Para comprobar que el montaje se ha realizado correctamente, envíe mount sin modificadores.
  
   * Escriba exit para dejar de ser el superusuario.

   * Para probar, cree una base de datos en esa carpeta. En el ejemplo siguiente se usa sqlcmd para crear una base de datos, cambiar el contexto a ella y comprobar que los archivos existen en el nivel del sistema operativo; después, se elimina la ubicación temporal. Puede usar SSMS.

    ![Captura de pantalla del comando sqlcmd y la respuesta al comando.][4]
 
   * Desmontaje del recurso compartido 

    ```bash
    sudo umount <IPAddressOfNFSServer>:<FolderOnNFSServer> <FolderToMountIn>
    ```

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar.
    
    \<FolderOnNFSServer> es el nombre del recurso compartido de NFS.

    \<FolderMountedIn> es la carpeta creada en el paso anterior. A continuación se muestra un ejemplo. 
 
5. Repita los pasos en los demás nodos.


## <a name="next-steps"></a>Pasos siguientes

[Configurar la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/05-nfsacl.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/10-mountnoswitches.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/20-createtestdatabase.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-nfs/15-createtestdatabase.png
