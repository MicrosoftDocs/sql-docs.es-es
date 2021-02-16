---
title: Funcionamiento de la FCI de RHEL para SQL Server en Linux
description: Obtenga información sobre cómo usar una instancia de clúster de conmutación por error (FCI) de disco compartido de Red Hat Enterprise Linux (RHEL) para SQL Server de alta disponibilidad, como la conmutación por error manual de la FCI y agregar o quitar nodos del clúster.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 075ab7d8-8b68-43f3-9303-bbdf00b54db1
ms.openlocfilehash: 272bc33012e37c58f6a3f8ea0ed51f1c216d716f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346272"
---
# <a name="operate-rhel-failover-cluster-instance-fci-for-sql-server"></a>Funcionamiento de la instancia de clúster de conmutación por error (FCI) de RHEL para SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este documento se describe cómo realizar las siguientes tareas para SQL Server en un clúster de conmutación por error de disco compartido con Red Hat Enterprise Linux.

- Conmutación por error manual del clúster
- Supervisión de un servicio de SQL Server de clúster de conmutación por error
- Adición de un nodo de clúster
- Eliminación de un nodo de clúster
- Cambio de la frecuencia de supervisión de recursos de SQL Server

## <a name="architecture-description"></a>Descripción de la arquitectura

La capa de agrupación en clústeres se basa en el [complemento de alta disponibilidad](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) de Red Hat Enterprise Linux (RHEL) sobre [Pacemaker](https://clusterlabs.org/). Corosync y Pacemaker coordinan las comunicaciones del clúster y la administración de recursos. La instancia de SQL Server está activa en un nodo o en el otro.

En el siguiente diagrama se ilustran los componentes de un clúster de Linux con SQL Server. 

![Clúster de SQL de disco compartido de Red Hat Enterprise Linux 7](./media/sql-server-linux-shared-disk-cluster-red-hat-7-configure/LinuxCluster.png) 

Para obtener más información sobre la configuración del clúster, las opciones de los agentes de recursos y la administración, vaya a la [documentación de referencia de RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

## <a name="failover-cluster-manually"></a><a name = "failManual"></a>Conmutación por error manual del clúster

El comando `resource move` crea una restricción que obliga al recurso a iniciarse en el nodo de destino.    Después de ejecutar el comando `move`, al ejecutar el recurso `clear` se quitará la restricción, por lo que es posible volver a colocar el recurso o hacer que este conmute por error automáticamente. 

```bash
sudo pcs resource move <sqlResourceName> <targetNodeName>  
sudo pcs resource clear <sqlResourceName> 
```

En el ejemplo siguiente se mueve el recurso **mssqlha** a un nodo denominado **sqlfcivm2** y después se quita la restricción para que el recurso pueda moverse más adelante a otro nodo.  

```bash
sudo pcs resource move mssqlha sqlfcivm2 
sudo pcs resource clear mssqlha 
```

## <a name="monitor-a-failover-cluster-sql-server-service"></a>Supervisión de un servicio de SQL Server de clúster de conmutación por error

Vea el estado actual del clúster:

```bash
sudo pcs status  
```

Vea el estado activo del clúster y los recursos:

```bash
sudo crm_mon 
```

Vea los registros del agente de recursos en `/var/log/cluster/corosync.log`.

## <a name="add-a-node-to-a-cluster"></a>Adición de un nodo a un clúster

1. Compruebe la dirección IP de cada nodo. En el siguiente script se muestra la dirección IP del nodo actual. 

   ```bash
   ip addr show
   ```

3. El nuevo nodo necesita un nombre único que tenga 15 caracteres o menos. De forma predeterminada, en Red Hat Linux el nombre del equipo es `localhost.localdomain`. Es posible que este nombre predeterminado no sea único y sea demasiado largo. Establezca el nombre de equipo del nuevo nodo. Para establecer el nombre de equipo, agréguelo a `/etc/hosts`. El siguiente script le permite editar `/etc/hosts` con `vi`. 

   ```bash
   sudo vi /etc/hosts
   ```

   En el ejemplo siguiente se muestra `/etc/hosts` con adiciones para tres nodos denominados `sqlfcivm1`, `sqlfcivm2` y `sqlfcivm3`.

   ```
   127.0.0.1   localhost localhost4 localhost4.localdomain4
   ::1         localhost localhost6 localhost6.localdomain6
   10.128.18.128 fcivm1
   10.128.16.77 fcivm2
   10.128.14.26 fcivm3
    ```
    
   El archivo debe ser el mismo en todos los nodos. 

1. Detenga el servicio SQL Server en el nuevo nodo.

1. Siga las instrucciones para montar el directorio de archivos de base de datos en la ubicación compartida:

   En el servidor NFS, instale `nfs-utils`.

   ```bash
   sudo yum -y install nfs-utils 
   ``` 

   Abra el firewall en los clientes y el servidor NFS. 

   ```bash
   sudo firewall-cmd --permanent --add-service=nfs
   sudo firewall-cmd --permanent --add-service=mountd
   sudo firewall-cmd --permanent --add-service=rpc-bind
   sudo firewall-cmd --reload
   ```

   Edite el archivo /etc/fstab para incluir el comando mount: 

   ```bash
   <IP OF NFS SERVER>:<shared_storage_path> <database_files_directory_path> nfs timeo=14,intr
   ```

   Ejecute `mount -a` para aplicar los cambios.
   
1. En el nuevo nodo, cree un archivo para almacenar el nombre de usuario y la contraseña de SQL Server para el inicio de sesión de Pacemaker. Con el siguiente comando se crea y rellena este archivo:

   ```bash
   sudo touch /var/opt/mssql/passwd
   sudo echo "<loginName>" >> /var/opt/mssql/secrets/passwd
   sudo echo "<loginPassword>" >> /var/opt/mssql/secrets/passwd
   sudo chown root:root /var/opt/mssql/passwd
   sudo chmod 600 /var/opt/mssql/passwd
   ```

3. En el nuevo nodo, abra los puertos de firewall de Pacemaker. Para abrir estos puertos con `firewalld`, ejecute el comando siguiente:

   ```bash
   sudo firewall-cmd --permanent --add-service=high-availability
   sudo firewall-cmd --reload
   ```

   > [!NOTE]
   > Si usa otro firewall que no tiene una configuración de alta disponibilidad integrada, deberán abrirse los puertos siguientes para que Pacemaker pueda comunicarse con otros nodos del clúster.
   >
   > * TCP: puertos 2224, 3121, 21064
   > * UDP: puerto 5405

1. Instale paquetes de Pacemaker en el nuevo nodo.

   ```bash
   sudo yum install pacemaker pcs fence-agents-all resource-agents
   ```
 
2. Establezca la contraseña para el usuario predeterminado que se crea al instalar paquetes de Pacemaker y Corosync. Use la misma contraseña que los nodos existentes. 

   ```bash
   sudo passwd hacluster
   ```
 
3. Habilite e inicie el servicio `pcsd` y Pacemaker. Esto permitirá que el nuevo nodo se vuelva a unir al clúster después del reinicio. Ejecute el siguiente comando en el nuevo nodo.

   ```bash
   sudo systemctl enable pcsd
   sudo systemctl start pcsd
   sudo systemctl enable pacemaker
   ```

4. Instale el agente de recursos de FCI para SQL Server. Ejecute los siguientes comandos en el nuevo nodo. 

   ```bash
   sudo yum install mssql-server-ha
   ```

1. En un nodo existente del clúster, autentique el nuevo nodo y agréguelo al clúster:

    ```bash
    sudo pcs    cluster auth <nodeName3> -u hacluster 
    sudo pcs    cluster node add <nodeName3> 
    ```

    En el ejemplo siguiente se agrega un nodo denominado **vm3** al clúster.

    ```bash
    sudo pcs    cluster auth  
    sudo pcs    cluster start 
    ```

## <a name="remove-nodes-from-a-cluster"></a>Eliminación de nodos de un clúster

Para quitar un nodo de un clúster, ejecute el siguiente comando:

```bash
sudo pcs    cluster node remove <nodeName>  
```

## <a name="change-the-frequency-of-sqlservr-resource-monitoring-interval"></a>Cambio de la frecuencia del intervalo de supervisión de recursos sqlservr

```bash
sudo pcs    resource op monitor interval=<interval>s <sqlResourceName> 
```

En el ejemplo siguiente se establece el intervalo de supervisión en 2 segundos para el recurso mssql:

```bash
sudo pcs    resource op monitor interval=2s mssqlha 
```
## <a name="troubleshoot-red-hat-enterprise-linux-shared-disk-cluster-for-sql-server"></a>Solución de problemas del clúster de disco compartido de Red Hat Enterprise Linux para SQL Server

Para solucionar problemas del clúster, puede ser útil comprender cómo funcionan conjuntamente los tres demonios para administrar los recursos del clúster. 

| Demonio | Descripción 
| ----- | -----
| Corosync | Proporciona la pertenencia al cuórum y la mensajería entre los nodos del clúster.
| Pacemaker | Reside en Corosync y proporciona equipos de estado de los recursos. 
| PCSD | Administra Pacemaker y Corosync mediante las herramientas `pcs`.

PCSD debe estar en ejecución para poder usar las herramientas `pcs`. 

### <a name="current-cluster-status"></a>Estado actual del clúster 

`sudo pcs status` devuelve información básica sobre el clúster, el cuórum, los nodos, los recursos y el estado del demonio de cada nodo. 

Aquí tiene un ejemplo de una salida correcta del cuórum de Pacemaker:

```
Cluster name: MyAppSQL 
Last updated: Wed Oct 31 12:00:00 2016  Last change: Wed Oct 31 11:00:00 2016 by root via crm_resource on sqlvmnode1 
Stack: corosync 
Current DC: sqlvmnode1  (version 1.1.13-10.el7_2.4-44eb2dd) - partition with quorum 
3 nodes and 1 resource configured 

Online: [ sqlvmnode1 sqlvmnode2 sqlvmnode3] 

Full list of resources: 

mssqlha (ocf::sql:fci): Started sqlvmnode1 

PCSD Status: 
sqlvmnode1: Online 
sqlvmnode2: Online 
sqlvmnode3: Online 

Daemon Status: 
corosync: active/disabled 
pacemaker: active/enabled 
```

En el ejemplo, `partition with quorum` implica que un cuórum de mayoría de nodos está en línea. Si el clúster pierde un cuórum de mayoría de nodos, `pcs status` devolverá `partition WITHOUT quorum` y se detendrán todos los recursos. 

`online: [sqlvmnode1 sqlvmnode2 sqlvmnode3]` devuelve el nombre de todos los nodos que participan actualmente en el clúster. Si algún nodo no participa, `pcs status` devuelve `OFFLINE: [<nodename>]`.

`PCSD Status` muestra el estado del clúster de cada nodo.

### <a name="reasons-why-a-node-may-be-offline"></a>Motivos por los que un nodo puede estar sin conexión

Compruebe los elementos siguientes si un nodo está sin conexión.

- **Firewall**

    Los siguientes puertos deben estar abiertos en todos los nodos para que Pacemaker pueda comunicarse.
    
    - **TCP: 2224, 3121, 21064

- **Servicios de Pacemaker o Corosync en ejecución**

- **Comunicación de los nodos**

- **Asignaciones de nombres de los nodos**

## <a name="additional-resources"></a>Recursos adicionales

* Guía [Cluster from Scratch](https://clusterlabs.org/doc/Cluster_from_Scratch.pdf) (Clústeres partiendo de cero) de Pacemaker

## <a name="next-steps"></a>Pasos siguientes

[Configuración del clúster de disco compartido de Red Hat Enterprise Linux para SQL Server](sql-server-linux-shared-disk-cluster-red-hat-7-configure.md)

