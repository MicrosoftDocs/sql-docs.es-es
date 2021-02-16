---
title: Configuración de FCI de RHEL para SQL Server en Linux
description: Obtenga información sobre cómo configurar una instancia de clúster de conmutación por error (FCI) de disco compartido de Red Hat Enterprise Linux (RHEL) para SQL Server en Linux con alta disponibilidad.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dcc0a8d3-9d25-4208-8507-a5e65d2a9a15
ms.openlocfilehash: f635a10d0a1165fb3f657e41147fe023f55d2874
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346282"
---
# <a name="configure-rhel-failover-cluster-instance-fci-cluster-for-sql-server"></a>Configuración del clúster de instancia de clúster de conmutación por error (FCI) de RHEL para SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En esta guía se proporcionan instrucciones para crear un clúster e conmutación por error de disco compartido de dos nodos para SQL Server en Red Hat Enterprise Linux. La capa de agrupación en clústeres se basa en el [complemento de alta disponibilidad](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) de Red Hat Enterprise Linux (RHEL) sobre [Pacemaker](https://clusterlabs.org/). La instancia de SQL Server está activa en un nodo o en el otro.

> [!NOTE] 
> El acceso al complemento de alta disponibilidad y la documentación de Red Hat exige una suscripción. 

Como se muestra en el diagrama siguiente, el almacenamiento se presenta en dos servidores. Los componentes de agrupación en clústeres (Corosync y Pacemaker) coordinan las comunicaciones y la administración de recursos. Uno de los servidores tiene la conexión activa a los recursos de almacenamiento y SQL Server. Cuando Pacemaker detecta un error, los componentes de agrupación en clústeres administran el traslado de los recursos al otro nodo.  

![Clúster de SQL de disco compartido de Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Para obtener más información sobre la configuración del clúster, las opciones de los agentes de recursos y la administración, vaya a la [documentación de referencia de RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).


> [!NOTE] 
> En este momento, la integración de SQL Server con Pacemaker no es tan perfecta como con WSFC en Windows. Desde dentro de SQL no hay conocimiento sobre la presencia del clúster, toda la orquestación está fuera y Pacemaker controla el servicio como una instancia independiente. Además, por ejemplo, las DMV de clúster sys.dm_os_cluster_nodes y sys.dm_os_cluster_properties no crearán ningún registro.
Para usar una cadena de conexión que apunte al nombre de servidor de una cadena y no use la dirección IP, se tiene que registrar en su servidor DNS la dirección IP usada para crear el recurso de IP virtual (como se explica en las secciones siguientes) con el nombre de servidor elegido.

En las secciones siguientes, se describen los pasos necesarios para configurar una solución de clúster de conmutación por error. 

## <a name="prerequisites"></a>Prerrequisitos

Para completar el siguiente escenario de un extremo a otro, necesita dos máquinas para implementar el clúster de dos nodos y otro servidor para configurar el servidor de NFS. En los pasos siguientes se describe cómo se configurarán estos servidores.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Instalación y configuración del sistema operativo en cada nodo del clúster

El primer paso es configurar el sistema operativo en los nodos del clúster. A efectos de este tutorial, use RHEL con una suscripción válida para el complemento de alta disponibilidad. 

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Instalación y configuración de SQL Server en cada nodo de clúster

1. Instale y configure SQL Server en ambos nodos.  Para obtener instrucciones detalladas, vea [Instalación de SQL Server en Linux](sql-server-linux-setup.md).

1. Para configurarlos, designe un nodo como principal y el otro como secundario. Use estos términos durante esta guía.  

1. En el nodo secundario, detenga y deshabilite SQL Server.

   En el siguiente ejemplo se detiene y se deshabilita SQL Server: 

   ```bash
   sudo systemctl stop mssql-server
   sudo systemctl disable mssql-server
   ```
> [!NOTE] 
> En el momento de la instalación, se genera una clave maestra del servidor para la instancia de SQL Server y se coloca en `/var/opt/mssql/secrets/machine-key`. En Linux, SQL Server siempre se ejecuta como una cuenta local denominada mssql. Dado que se trata de una cuenta local, su identidad no se comparte entre los nodos. Por lo tanto, debe copiar la clave de cifrado del nodo principal en cada nodo secundario para que cada cuenta local de mssql pueda acceder a ella y así descifrar la clave maestra del servidor. 

1. En el nodo principal, cree un inicio de sesión de SQL Server para Pacemaker y conceda el permiso de inicio de sesión para ejecutar `sp_server_diagnostics`. Pacemaker usa esta cuenta para comprobar qué nodo ejecuta SQL Server. 

   ```bash
   sudo systemctl start mssql-server
   ```

   Conéctese a la base de datos `master` de SQL Server con la cuenta "sa" y ejecute lo siguiente:

   ```bashsql
   USE [master]
   GO
   CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'

   ALTER SERVER ROLE [sysadmin] ADD MEMBER [<loginName>]
   ```
   Como alternativa, puede establecer los permisos con más detalle. El inicio de sesión de Pacemaker requiere `VIEW SERVER STATE` para consultar el estado de mantenimiento con sp_server_diagnostics `setupadmin` y `ALTER ANY LINKED SERVER` para actualizar el nombre de instancia de FCI con el nombre de recurso mediante la ejecución de sp_dropserver y sp_addserver. 

1. En el nodo principal, detenga y deshabilite SQL Server. 

1. Configure el archivo de hosts para cada nodo de clúster. El archivo de host debe incluir la dirección IP y el nombre de cada nodo de clúster. 

    Compruebe la dirección IP de cada nodo. En el siguiente script se muestra la dirección IP del nodo actual. 

   ```bash
   sudo ip addr show
   ```

   Establezca el nombre de equipo en cada nodo. Asigne a cada nodo un nombre único que tenga 15 caracteres o menos. Para establecer el nombre de equipo, agréguelo a `/etc/hosts`. El siguiente script le permite editar `/etc/hosts` con `vi`. 

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

En la siguiente sección, configurará el almacenamiento compartido y moverá los archivos de base de datos a ese almacenamiento. 

## <a name="configure-shared-storage-and-move-database-files"></a>Configuración del almacenamiento compartido y movimiento de los archivos de base de datos 

Hay varias soluciones para proporcionar almacenamiento compartido. En este tutorial se muestra cómo configurar el almacenamiento compartido con NFS. Le recomendamos que siga los procedimientos recomendados y use Kerberos para proteger NFS (encontrará un ejemplo aquí: https://www.certdepot.net/rhel7-use-kerberos-control-access-nfs-network-shares/). 

>[!Warning]
>Si no protege NFS, cualquier persona que pueda acceder a la red y suplantar la dirección IP de un nodo SQL podrá acceder a los archivos de datos. Como siempre, asegúrese de llevar a cabo un análisis del modelo de amenazas de su sistema antes de usarlo en producción. Otra opción de almacenamiento es usar el recurso compartido de archivos SMB.

### <a name="configure-shared-storage-with-nfs"></a>Configuración del almacenamiento compartido con NFS

> [!IMPORTANT] 
> En esta versión no se admite el hospedaje de archivos de base de datos en un servidor NFS con una versión anterior a la v4. Esto incluye el uso de NFS para la agrupación en clústeres de conmutación por error de discos compartidos, así como las bases de datos en instancias no agrupadas. Estamos trabajando para habilitar otras versiones del servidor NFS en las próximas versiones. 

En el servidor NFS, haga lo siguiente:

1. Instalar `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Habilite e inicie `rpcbind`.

   ```bash
   sudo systemctl enable rpcbind && sudo systemctl start rpcbind
   ```

1. Habilite e inicie `nfs-server`.
 
   ```bash
   sudo systemctl enable nfs-server && sudo systemctl start nfs-server
   ```
 
1.  Edite `/etc/exports` para exportar el directorio que quiere compartir. Necesita 1 línea para cada recurso compartido que quiera. Por ejemplo: 

   ```bash
   /mnt/nfs  10.8.8.0/24(rw,sync,no_subtree_check,no_root_squash)
   ```

1. Exporte los recursos compartidos.

   ```bash
   sudo exportfs -rav
   ```

1. Compruebe que las rutas de acceso se han compartido o exportado, ejecute desde el servidor NFS.

   ```bash
   sudo showmount -e
   ```

1. Agregue una excepción en SELinux.

   ```bash
   sudo setsebool -P nfs_export_all_rw 1
   ```
   
1. Abra el firewall en el servidor.

   ```bash 
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configuración de todos los nodos de clúster para conectarse al almacenamiento compartido de NFS

Realice los pasos siguientes en todos los nodos del clúster:

1.  Instalar `nfs-utils`

   ```bash
   sudo yum -y install nfs-utils
   ```

1. Abra el firewall en los clientes y el servidor NFS.

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

1. Compruebe que puede ver los recursos compartidos de NFS en los equipos cliente.

   ```bash
   sudo showmount -e <IP OF NFS SERVER>
   ```

1. Repita estos pasos en todos los nodos del clúster.

Para obtener más información sobre el uso de NFS, consulte los siguientes recursos:

* [Servidores NFS y firewalls | Stack Exchange](https://unix.stackexchange.com/questions/243756/nfs-servers-and-firewalld)
* [Montaje de un volumen NFS | Guía de administradores de red de Linux](https://www.tldp.org/LDP/nag2/x-087-2-nfs.mountd.html)
* [Configuración del servidor NFS | Portal del cliente de Red Hat](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/nfs-serverconfig)

### <a name="mount-database-files-directory-to-point-to-the-shared-storage"></a>Montaje del directorio de archivos de base de datos para que apunte al almacenamiento compartido

1.  **Solo en el nodo principal**, guarde los archivos de base de datos en una ubicación temporal. El siguiente script crea un directorio temporal, copia los archivos de base de datos en el nuevo directorio y quita los archivos antiguos de base de datos. Como SQL Server se ejecuta como el usuario local mssql, debe asegurarse de que, después de transferir los datos al recurso compartido montado, el usuario local tiene acceso de lectura y escritura al recurso compartido. 

   ``` 
   $ sudo su mssql
   $ mkdir /var/opt/mssql/tmp
   $ cp /var/opt/mssql/data/* /var/opt/mssql/tmp
   $ rm /var/opt/mssql/data/*
   $ exit
   ``` 

1.  En todos los nodos del clúster, edite el archivo `/etc/fstab` para que incluya el comando mount.  

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr 
   ```
   
   El script siguiente muestra un ejemplo de la edición:  

   ``` 
   10.8.8.0:/mnt/nfs /var/opt/mssql/data nfs timeo=14,intr 
   ``` 
> [!NOTE] 
>Si usa un recurso del sistema de archivos (FS) como se recomienda aquí, no es necesario conservar el comando de montaje en /etc/fstab. Pacemaker se encargará de montar la carpeta cuando inicie el recurso en clúster del FS. Con la ayuda de las barreras, se asegurará de que el FS nunca se monta dos veces. 

1.  Ejecute el comando `mount -a` para que el sistema actualice las rutas de acceso montadas.  

1.  Copie los archivos de base de datos y de registro que guardó en `/var/opt/mssql/tmp` en el recurso compartido `/var/opt/mssql/data` recién montado. Solo es necesario hacerlo **en el nodo principal**. Asegúrese de conceder permisos de lectura y escritura al usuario local "MSSQL".

   ``` 
   $ sudo chown mssql /var/opt/mssql/data
   $ sudo chgrp mssql /var/opt/mssql/data
   $ sudo su mssql
   $ cp /var/opt/mssql/tmp/* /var/opt/mssql/data/
   $ rm /var/opt/mssql/tmp/*
   $ exit
   ``` 
 
1.  Compruebe que SQL Server se inicia correctamente con la nueva ruta de acceso del archivo. Haga esto en cada nodo. En este momento, solo un nodo debe ejecutar SQL Server a la vez. No se pueden ejecutar ambos al mismo tiempo, ya que intentarán acceder a los archivos de datos simultáneamente (para evitar iniciar por error SQL Server en ambos nodos, use un recurso de clúster del sistema de archivos para asegurarse de que el recurso compartido no se monta dos veces en diferentes nodos). Los siguientes comandos inician SQL Server, comprueban el estado y después detienen SQL Server.
 
   ```bash
   sudo systemctl start mssql-server
   sudo systemctl status mssql-server
   sudo systemctl stop mssql-server
   ```
 
En este momento, ambas instancias de SQL Server están configuradas para ejecutarse con los archivos de base de datos en el almacenamiento compartido. El siguiente paso consiste en configurar SQL Server para Pacemaker. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Instalación y configuración de Pacemaker en todos los nodos del clúster


2. En ambos nodos del clúster, cree un archivo para almacenar el nombre de usuario de SQL Server y la contraseña para el inicio de sesión de Pacemaker. Con el siguiente comando se crea y rellena este archivo:

   ```bash
   sudo touch /var/opt/mssql/secrets/passwd
   echo '<loginName>' | sudo tee -a /var/opt/mssql/secrets/passwd
   echo '<loginPassword>' | sudo tee -a /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/secrets/passwd 
   sudo chmod 600 /var/opt/mssql/secrets/passwd    
   ```

3. En ambos nodos del clúster, abra los puertos de firewall de Pacemaker. Para abrir estos puertos con `firewalld`, ejecute el comando siguiente:

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

    

2. Establezca la contraseña para el usuario predeterminado que se crea al instalar paquetes de Pacemaker y Corosync. Use la misma contraseña en ambos nodos. 

   ```bash
   sudo passwd hacluster
   ```

    

3. Habilite e inicie el servicio `pcsd` y Pacemaker. Esto permitirá que los nodos se unan al clúster después del reinicio. Ejecute el comando siguiente en ambos nodos.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Instale el agente de recursos de FCI para SQL Server. Ejecute los comandos siguientes en ambos nodos. 

   ```bash
   sudo yum install mssql-server-ha
   ```

## <a name="configure-fencing-agent"></a>Configuración del agente de barrera

Un dispositivo STONITH proporciona un agente de barrera. En [Configuración de Pacemaker en Red Hat Enterprise Linux en Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices) se proporciona un ejemplo de cómo crear un dispositivo STONITH para este clúster en Azure. Modifique las instrucciones para el entorno.

## <a name="create-the-cluster"></a>Creación del clúster 

1. En uno de los nodos, cree el clúster.

   ```bash
   sudo pcs cluster auth <nodeName1 nodeName2 ...> -u hacluster
   sudo pcs cluster setup --name <clusterName> <nodeName1 nodeName2 ...>
   sudo pcs cluster start --all
   ```

2. Configure los recursos de clúster para SQL Server, el sistema de archivos y los recursos de IP virtual e inserte la configuración en el clúster. Necesita la siguiente información:

   - **Nombre del recurso de SQL Server**: nombre de recurso de SQL Server en clúster. 
   - **Nombre de recurso de IP flotante**: nombre de recurso de la dirección IP virtual.
   - **Dirección IP**: dirección IP que los clientes usarán para conectarse a la instancia en clúster de SQL Server. 
   - **Nombre de recurso del sistema de archivos**: nombre de recurso del sistema de archivos.
   - **dispositivo**: ruta de acceso del recurso compartido de NFS.
   - **dispositivo**: ruta de acceso local que se monta en el recurso compartido.
   - **fstype**: tipo de recurso compartido de archivos (p. ej. nfs).

   Actualice los valores del siguiente script para su entorno. Ejecute en un nodo para configurar e iniciar el servicio en clúster.  

   ```bash
   sudo pcs cluster cib cfg 
   sudo pcs -f cfg resource create <sqlServerResourceName> ocf:mssql:fci
   sudo pcs -f cfg resource create <floatingIPResourceName> ocf:heartbeat:IPaddr2 ip=<ip Address>
   sudo pcs -f cfg resource create <fileShareResourceName> Filesystem device=<networkPath> directory=<localPath>         fstype=<fileShareType>
   sudo pcs -f cfg constraint colocation add <virtualIPResourceName> <sqlResourceName>
   sudo pcs -f cfg constraint colocation add <fileShareResourceName> <sqlResourceName> 
   sudo pcs cluster cib-push cfg
   ```

   Por ejemplo, el siguiente script crea un recurso en clúster de SQL Server denominado `mssqlha` y recursos con una dirección IP flotante `10.0.0.99`. También crea un recurso de sistema de archivos y agrega restricciones para que todos los recursos se coloquen en el mismo nodo que el recurso de SQL. 

   ```bash
   sudo pcs cluster cib cfg
   sudo pcs -f cfg resource create mssqlha ocf:mssql:fci
   sudo pcs -f cfg resource create virtualip ocf:heartbeat:IPaddr2 ip=10.0.0.99
   sudo pcs -f cfg resource create fs Filesystem device="10.8.8.0:/mnt/nfs" directory="/var/opt/mssql/data" fstype="nfs"
   sudo pcs -f cfg constraint colocation add virtualip mssqlha
   sudo pcs -f cfg constraint colocation add fs mssqlha
   sudo pcs cluster cib-push cfg
   ```

   Una vez insertada la configuración, SQL Server se iniciará en un nodo. 

3. Confirme que se ha iniciado SQL Server. 

   ```bash
   sudo pcs status 
   ```

   En los siguientes ejemplos se muestran los resultados si Pacemaker ha iniciado correctamente una instancia en clúster de SQL Server. 

   ```
   fs     (ocf::heartbeat:Filesystem):    Started sqlfcivm1
   virtualip     (ocf::heartbeat:IPaddr2):      Started sqlfcivm1
   mssqlha  (ocf::mssql:fci): Started sqlfcivm1
   
   PCSD Status:
    sqlfcivm1: Online
    sqlfcivm2: Online
   
   Daemon Status:
    corosync: active/disabled
    pacemaker: active/enabled
    pcsd: active/enabled
   ```

## <a name="additional-resources"></a>Recursos adicionales

* Guía [Cluster from Scratch](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf) (Clústeres partiendo de cero) de Pacemaker

## <a name="next-steps"></a>Pasos siguientes

[Operación de SQL Server en el clúster de disco compartido de Red Hat Enterprise Linux](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md)
