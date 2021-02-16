---
title: 'Configuración de FCI: SQL Server en Linux (RHEL)'
description: Aprenda a configurar una instancia de clúster de conmutación por error (FCI) en Red Hat Enterprise Linux (RHEL) para SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 31c8c92e-12fe-4728-9b95-4bc028250d85
ms.openlocfilehash: b2b97fa250b5af1a0030b51b0ea29a89324db9c1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346307"
---
# <a name="configure-failover-cluster-instance---sql-server-on-linux-rhel"></a>Configuración de la instancia de clúster de conmutación por error: SQL Server en Linux (RHEL)

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Una instancia de clúster de conmutación por error de disco compartido de dos nodos de SQL Server proporciona redundancia de nivel de servidor para alta disponibilidad. En este tutorial, aprenderá a crear una instancia de clúster de conmutación por error de dos nodos de SQL Server en Linux. Entre los pasos específicos que completará se incluyen los siguientes:

> [!div class="checklist"]
> * Configuración de Linux
> * Instalación y configuración de SQL Server
> * Configuración del archivo hosts
> * Configuración del almacenamiento compartido y movimiento de los archivos de base de datos
> * Instalación y configuración de Pacemaker en todos los nodos del clúster
> * Configuración de la instancia de clúster de conmutación por error

En este artículo se explica cómo crear una instancia de clúster de conmutación por error (FCI) de disco compartido de dos nodos para SQL Server. El artículo incluye instrucciones y ejemplos de scripts para Red Hat Enterprise Linux (RHEL). Las distribuciones de Ubuntu son similares a RHEL, por lo que los ejemplos de script también funcionarán en Ubuntu. 

Para obtener información conceptual, consulte [Instancias de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-concepts.md).

## <a name="prerequisites"></a>Requisitos previos

Para completar el siguiente escenario de un extremo a otro, necesita dos máquinas para implementar el clúster de dos nodos y otro servidor para el almacenamiento. En los pasos siguientes se describe cómo se configurarán estos servidores.

## <a name="set-up-and-configure-linux"></a>Configuración de Linux

El primer paso es configurar el sistema operativo en los nodos del clúster. En cada nodo de clúster, configure una distribución de Linux. Use la misma distribución y versión en ambos nodos. Use una de las siguientes distribuciones:
    
* RHEL con una suscripción válida para el complemento de alta disponibilidad

## <a name="install-and-configure-sql-server"></a>Instalación y configuración de SQL Server

1. Instale y configure SQL Server en ambos nodos.  Para obtener instrucciones detalladas, vea [Instalación de SQL Server en Linux](sql-server-linux-setup.md).
1. Para configurarlos, designe un nodo como principal y el otro como secundario. Use estos términos durante esta guía.  
1. En el nodo secundario, detenga y deshabilite SQL Server.
    En el siguiente ejemplo se detiene y se deshabilita SQL Server: 
    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE] 
    > En el momento de la instalación, se genera una clave maestra del servidor para la instancia de SQL Server y se coloca en `var/opt/mssql/secrets/machine-key`. En Linux, SQL Server siempre se ejecuta como una cuenta local denominada mssql. Dado que se trata de una cuenta local, su identidad no se comparte entre los nodos. Por lo tanto, debe copiar la clave de cifrado del nodo principal en cada nodo secundario para que cada cuenta local de mssql pueda acceder a ella y así descifrar la clave maestra del servidor. 

1.  En el nodo principal, cree un inicio de sesión de SQL Server para Pacemaker y conceda el permiso de inicio de sesión para ejecutar `sp_server_diagnostics`. Pacemaker usa esta cuenta para comprobar qué nodo ejecuta SQL Server. 

    ```bash
    sudo systemctl start mssql-server
    ```
   
   Conéctese a la base de datos `master` de SQL Server con la cuenta "sa" y ejecute lo siguiente:

   ```sql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```

   Como alternativa, puede establecer los permisos con más detalle. El inicio de sesión de Pacemaker requiere `VIEW SERVER STATE` para consultar el estado de mantenimiento con sp_server_diagnostics, `setupadmin` y `ALTER ANY LINKED SERVER` para actualizar el nombre de instancia de FCI con el nombre de recurso mediante la ejecución de sp_dropserver y sp_addserver. 

1. En el nodo principal, detenga y deshabilite SQL Server. 

## <a name="configure-the-hosts-file"></a>Configuración del archivo hosts

En cada nodo de clúster, configure el archivo de hosts. El archivo de hosts debe incluir la dirección IP y el nombre de cada nodo de clúster.

1. Compruebe la dirección IP de cada nodo. En el siguiente script se muestra la dirección IP del nodo actual. 

    ```bash
    sudo ip addr show
    ```

1. Establezca el nombre de equipo en cada nodo. Asigne a cada nodo un nombre único que tenga 15 caracteres o menos. Para establecer el nombre de equipo, agréguelo a `/etc/hosts`. El siguiente script le permite editar `/etc/hosts` con `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```
   En el ejemplo siguiente se muestra `/etc/hosts` con adiciones para dos nodos denominados `sqlfcivm1` y `sqlfcivm2`.

   ```bash
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1       localhost localhost6 localhost6.localdomain6
   10.128.18.128 sqlfcivm1
   10.128.16.77 sqlfcivm2
   ```

## <a name="configure-storage--move-database-files"></a>Configurar el almacenamiento y mover archivos de base de datos  

Debe proporcionar almacenamiento al que puedan acceder ambos nodos. Puede usar iSCSI, NFS o SMB. Configure el almacenamiento, preséntelo a los nodos del clúster y, luego, mueva los archivos de base de datos al nuevo almacenamiento. En los siguientes artículos se explican los pasos para cada tipo de almacenamiento:

- [Configurar la instancia de clúster de conmutación por error - iSCSI - SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure-iscsi.md)
- [Configurar la instancia de clúster de conmutación por error - NFS - SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure-nfs.md)
- [Configurar la instancia de clúster de conmutación por error - SMB - SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure-smb.md)

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Instalación y configuración de Pacemaker en todos los nodos del clúster

1. En ambos nodos del clúster, cree un archivo para almacenar el nombre de usuario de SQL Server y la contraseña para el inicio de sesión de Pacemaker. 

    Con el siguiente comando se crea y rellena este archivo:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd    
    ```

1. En ambos nodos del clúster, abra los puertos de firewall de Pacemaker. Para abrir estos puertos con `firewalld`, ejecute el comando siguiente:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > Si usa otro firewall que no tiene una configuración de alta disponibilidad integrada, deberán abrirse los puertos siguientes para que Pacemaker pueda comunicarse con otros nodos del clúster.
   >
   > * TCP: puertos 2224, 3121, 21064
   > * UDP: puerto 5405

1. Instale paquetes de Pacemaker en cada nodo.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
1. Establezca la contraseña para el usuario predeterminado que se crea al instalar paquetes de Pacemaker y Corosync. Use la misma contraseña en ambos nodos. 

   ```bash
   sudo passwd hacluster
   ```
1. Habilite e inicie el servicio `pcsd` y Pacemaker. Esto permitirá que los nodos se unan al clúster después del reinicio. Ejecute el comando siguiente en ambos nodos.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

1. Instale el agente de recursos de FCI para SQL Server. Ejecute los comandos siguientes en ambos nodos. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-the-failover-cluster-instance"></a>Configuración de la instancia de clúster de conmutación por error

La FCI se creará en un grupo de recursos. Esto es un poco más fácil, ya que el grupo de recursos reduce la necesidad de restricciones. Aun así, agregue los recursos al grupo de recursos en el orden en el que deben iniciarse, que es el siguiente: 

1. Recurso de almacenamiento
2. Recurso de red
3. Recurso de aplicación

En este ejemplo se creará una FCI en el grupo NewLinFCIGrp. El nombre del grupo de recursos debe ser distinto del de cualquier recurso creado en Pacemaker.

1.  Cree el recurso de disco. Si no se produce ningún problema, no recibirá ninguna respuesta. La forma de crear el recurso de disco depende del tipo de almacenamiento. A continuación se incluye un ejemplo para cada tipo de almacenamiento. Use el ejemplo que se aplique a su tipo de almacenamiento de clúster.

    **iSCSI**

    ```bash
    sudo pcs resource create <iSCSIDiskResourceName> Filesystem device="/dev/<VolumeGroupName>/<LogicalVolumeName>" directory="<FolderToMountiSCSIDisk>" fstype="<FileSystemType>" --group RGName
    ```

    \<iSCSIDIskResourceName> es el nombre del recurso asociado al disco iSCSI.

    \<VolumeGroupName> es el nombre del montaje de volumen.  

    \<LogicalVolumeName> es el nombre del volumen lógico que se ha creado.  

    \<FolderToMountiSCSIDIsk> es la carpeta para montar el disco (para las bases de datos del sistema y la ubicación predeterminada, sería /var/opt/mssql/data).

    \<FileSystemType> sería EXT4 o XFS en función de cómo se han formateado los elementos y qué es lo que admite la distribución. 

    **NFS**

    ```bash
    sudo pcs resource create <NFSDiskResourceName> Filesystem device="<IPAddressOfNFSServer>:<FolderOnNFSServer>" directory="<FolderToMountNFSShare>" fstype=nfs4 options=" nfsvers=4.2,timeo=14,intr" --group RGName
    mount -t nfs4 IPAddressOfNFSServer:FolderOnNFSServer /var/opt/mssql/data -o 
    ```

    \<NFSDIskResourceName> es el nombre del recurso asociado al recurso compartido de NFS.

    \<IPAddressOfNFSServer> es la dirección IP del servidor NFS que se va a usar.

    \<FolderOnNFSServer> es el nombre del recurso compartido de NFS.

    \<FolderToMountNFSShare> es la carpeta para montar el disco (para las bases de datos del sistema y la ubicación predeterminada, sería /var/opt/mssql/data).

    A continuación, se muestra un ejemplo:

    ```bash
    mount -t nfs4 200.201.202.63:/var/nfs/fci1 /var/opt/mssql/data -o nfsvers=4.2,timeo=14,intr
    ```

    **SMB**

    ```bash
    sudo pcs resource create SMBDiskResourceName Filesystem device="//<ServerName>/<ShareName>" directory="<FolderName>" fstype=cifs options="vers=3.0,username=<UserName>,password=<Password>,domain=<ADDomain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777" --group <RGName>
    ```

    \<ServerName> es el nombre del servidor con el recurso compartido de SMB.

    \<ShareName> es el nombre del recurso compartido.

    \<FolderName> es el nombre de la carpeta creada en el último paso.
    
    \<UserName> es el nombre del usuario para acceder al recurso compartido.

    \<Password> es la contraseña del usuario.

    \<ADDomain> es el dominio de AD DS (si es aplicable cuando se usa un recurso compartido de SMB basado en Windows Server).

    \<mssqlUID> es el UID del usuario de mssql.

    \<mssqlGID> es el GID del usuario de mssql.

    \<RGName> es el nombre del grupo de recursos.
 
2.  Cree la dirección IP que usará la FCI. Si no se produce ningún problema, no recibirá ninguna respuesta.

    ```bash
    sudo pcs resource create <IPResourceName> ocf:heartbeat:IPaddr2 ip=<IPAddress> nic=<NetworkCard> cidr_netmask=<NetMask> --group <RGName>
    ```

    \<IPResourceName> es el nombre del recurso asociado a la dirección IP.

    \<IPAddress> es la dirección IP para la FCI.

    \<NetworkCard> es la tarjeta de red asociada a la subred (es decir, eth0).

    \<NetMask> es la máscara de red de la subred (es decir, 24).

    \<RGName> es el nombre del grupo de recursos.
 
3.  Cree el grupo de FCI. Si no se produce ningún problema, no recibirá ninguna respuesta.

    ```bash
    sudo pcs resource create FCIResourceName ocf:mssql:fci op defaults timeout=60s --group RGName
    ```

    \<FCIResourceName> no es solo el nombre del recurso, sino el nombre descriptivo asociado a la FCI. Esto es lo que los usuarios y las aplicaciones usarán para conectarse. 

    \<RGName> es el nombre del grupo de recursos.
 
4.  Ejecute el comando `sudo pcs resource`. La FCI debe estar en línea.
 
5.  Conéctese a la FCI con SSMS o sqlcmd mediante el nombre DNS o de recurso de la FCI.

6.  Emita la instrucción `SELECT @@SERVERNAME`. Debería devolver el nombre de la FCI.

7.  Emita la instrucción `SELECT SERVERPROPERTY('ComputerNamePhysicalNetBIOS')`. Debería devolver el nombre del nodo en el que se ejecuta la FCI.

8.  Conmute por error manualmente la FCI a los otros nodos. Consulte las instrucciones indicadas en [Operación de la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-operate.md).

9.  Por último, vuelva a conmutar por error la FCI al nodo original y quite la restricción de ubicación.

<!---

|Distribution |Topic 
|----- |-----
|**Red Hat Enterprise Linux with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)<br/>[Operate](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
|**SUSE Linux Enterprise Server with HA add-on** |[Configure](sql-server-linux-shared-disk-cluster-sles-configure.md)

-->
## <a name="summary"></a>Resumen

En este tutorial, ha completado las tareas siguientes.

> [!div class="checklist"]
> * Configuración de Linux
> * Instalación y configuración de SQL Server
> * Configuración del archivo hosts
> * Configuración del almacenamiento compartido y movimiento de los archivos de base de datos
> * Instalación y configuración de Pacemaker en todos los nodos del clúster
> * Configuración de la instancia de clúster de conmutación por error

## <a name="next-steps"></a>Pasos siguientes

- [Operación de la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-operate.md)

<!--Image references-->
