---
title: Configuración del clúster de disco compartido de SLES para SQL Server
description: Implemente la alta disponibilidad al configurar el clúster de disco compartido de SUSE Linux Enterprise Server (SLES) para SQL Server.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: e5ad1bdd-c054-4999-a5aa-00e74770b481
ms.openlocfilehash: 87a4fd9edf52c3590fb19781fc5bf76c9bb0168b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346261"
---
# <a name="configure-sles-shared-disk-cluster-for-sql-server"></a>Configuración del clúster de disco compartido de SLES para SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En esta guía se proporcionan instrucciones para crear un clúster de disco compartido de dos nodos para SQL Server en SUSE Linux Enterprise Server (SLES). La capa de agrupación en clústeres se basa en SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability) desarrollado sobre [Pacemaker](https://clusterlabs.org/). 

Para obtener más información sobre la configuración del clúster, las opciones del agente de recursos y la administración, así como para obtener recomendaciones y ver los procedimientos recomendados, vea [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

## <a name="prerequisites"></a>Prerrequisitos

Para completar el siguiente escenario de un extremo a otro, necesita dos máquinas para implementar el clúster de dos nodos y otro servidor para configurar el recurso compartido de NFS. En los pasos siguientes se describe cómo se configurarán estos servidores.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Instalación y configuración del sistema operativo en cada nodo del clúster

El primer paso es configurar el sistema operativo en los nodos del clúster. A efectos de este tutorial, use SLES con una suscripción válida para el complemento de alta disponibilidad.

## <a name="install-and-configure-sql-server-on-each-cluster-node"></a>Instalación y configuración de SQL Server en cada nodo de clúster

1. Instale y configure SQL Server en ambos nodos. Para obtener instrucciones detalladas, vea [Instalación de SQL Server en Linux](sql-server-linux-setup.md).
2. Para configurarlos, designe un nodo como principal y el otro como secundario. Use estos términos durante esta guía. 
3. En el nodo secundario, detenga y deshabilite SQL Server. En el siguiente ejemplo se detiene y se deshabilita SQL Server:

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl disable mssql-server
    ```

    > [!NOTE]
    > En el momento de la instalación, se genera una clave maestra del servidor para la instancia de SQL Server y se coloca en `/var/opt/mssql/secrets/machine-key`. En Linux, SQL Server siempre se ejecuta como una cuenta local denominada mssql. Dado que se trata de una cuenta local, su identidad no se comparte entre los nodos. Por lo tanto, debe copiar la clave de cifrado del nodo principal en cada nodo secundario para que cada cuenta local de mssql pueda acceder a ella y así descifrar la clave maestra del servidor.
4. En el nodo principal, cree un inicio de sesión de SQL Server para Pacemaker y conceda el permiso de inicio de sesión para ejecutar `sp_server_diagnostics`. Pacemaker usa esta cuenta para comprobar qué nodo ejecuta SQL Server.

    ```bash
    sudo systemctl start mssql-server
    ```
    Conéctese a la base de datos maestra de SQL Server con la cuenta “sa” y ejecute lo siguiente:

    ```sql
    USE [master]
    GO
    CREATE LOGIN [<loginName>] with PASSWORD= N'<loginPassword>'
    GRANT VIEW SERVER STATE TO <loginName>
    ```
5. En el nodo principal, detenga y deshabilite SQL Server.
6. Siga las instrucciones [de la documentación de SUSE](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) para configurar y actualizar el archivo “hosts” de cada nodo de clúster. El archivo "hosts" debe incluir la dirección IP y el nombre de cada nodo de clúster.

    Para comprobar la dirección IP del nodo actual, ejecute:

    ```bash
    sudo ip addr show
    ```

    Establezca el nombre de equipo en cada nodo. Asigne a cada nodo un nombre único que tenga 15 caracteres o menos. Para establecer el nombre de equipo, agréguelo a `/etc/hostname` con [yast](https://www.suse.com/documentation/sles11/book_sle_admin/data/sec_basicnet_yast.html) o [manualmente](https://www.suse.com/documentation/sled11/book_sle_admin/data/sec_basicnet_manconf.html).

    En el ejemplo siguiente se muestra `/etc/hosts` con adiciones para dos nodos denominados `SLES1` y `SLES2`.

    ```
    127.0.0.1   localhost
    10.128.18.128 SLES1
    10.128.16.77 SLES2
    ```

    > [!NOTE]
    > Todos los nodos de clúster deben poder acceder entre sí mediante SSH. Herramientas como hb_report o crm_report (para solucionar problemas) y explorador de historial de Hawk requieren el acceso con SSH sin contraseña entre los nodos, ya que, de lo contrario, puede que solo recopilen datos desde el nodo actual. En caso de usar un puerto de SSH no estándar, use la opción -X ([vea la página man](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html)). Por ejemplo, si el puerto SSH es 3479, invoque un crm_report del siguiente modo:
    >
    >```bash
    >crm_report -X "-p 3479" [...]
    >```
    >Para obtener más información, consulte [la guía de administración](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc).

En la siguiente sección, configurará el almacenamiento compartido y moverá los archivos de base de datos a ese almacenamiento.  

## <a name="configure-shared-storage-and-move-database-files"></a>Configuración del almacenamiento compartido y movimiento de los archivos de base de datos

Hay varias soluciones para proporcionar almacenamiento compartido. En este tutorial se muestra cómo configurar el almacenamiento compartido con NFS. Le recomendamos que siga los procedimientos recomendados y use Kerberos para proteger NFS: 

- [Uso compartido de sistemas de archivos con NFS](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.nfs)

Si no sigue esta guía, cualquier persona que pueda acceder a la red y suplantar la dirección IP de un nodo SQL podrá acceder a los archivos de datos. Como siempre, asegúrese de llevar a cabo un análisis del modelo de amenazas de su sistema antes de usarlo en producción. 

Otra opción de almacenamiento es usar el recurso compartido de archivos SMB:

- [Sección Samba de la documentación de SUSE](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#cha.samba)

### <a name="configure-an-nfs-server"></a>Configuración de un servidor NFS

Para configurar un servidor NFS, vea los pasos siguientes de la documentación de SUSE: [Configuring NFS Server](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-server) (Configuración del servidor NFS).

### <a name="configure-all-cluster-nodes-to-connect-to-the-nfs-shared-storage"></a>Configuración de todos los nodos de clúster para conectarse al almacenamiento compartido de NFS

Antes de configurar el cliente NFS para montar la ruta de acceso de los archivos de base de datos de SQL Server para que apunte a la ubicación de almacenamiento compartido, asegúrese de guardar los archivos de base de datos en una ubicación temporal para poder copiarlos más adelante en el recurso compartido:

1. **Solo en el nodo principal**, guarde los archivos de base de datos en una ubicación temporal. El siguiente script crea un directorio temporal, copia los archivos de base de datos en el nuevo directorio y quita los archivos antiguos de base de datos. Como SQL Server se ejecuta como el usuario local mssql, debe asegurarse de que, después de transferir los datos al recurso compartido montado, el usuario local tiene acceso de lectura y escritura al recurso compartido. 

    ```bash
    su mssql
    mkdir /var/opt/mssql/tmp
    cp /var/opt/mssql/data/* /var/opt/mssql/tmp
    rm /var/opt/mssql/data/*
    exit
    ```

    Configure el cliente NFS en todos los nodos de clúster:

    - [Configuración de clientes](https://www.suse.com/documentation/sles-12/singlehtml/book_sle_admin/book_sle_admin.html#sec.nfs.configuring-nfs-clients)

    > [!NOTE]
    > Se recomienda seguir los procedimientos recomendados y las recomendaciones de SUSE con respecto al almacenamiento NFS de alta disponibilidad: [Highly Available NFS Storage with DRBD and Pacemaker](https://www.suse.com/documentation/sle-ha-12/book_sleha_techguides/data/art_ha_quick_nfs.html) (Almacenamiento NFS de alta disponibilidad con DRBD y Pacemaker).

2. Compruebe que SQL Server se inicia correctamente con la nueva ruta de acceso del archivo. Haga esto en cada nodo. En este momento, solo un nodo debe ejecutar SQL Server a la vez. No se pueden ejecutar ambos al mismo tiempo, ya que intentarán acceder a los archivos de datos simultáneamente (para evitar iniciar por error SQL Server en ambos nodos, use un recurso de clúster del sistema de archivos para asegurarse de que el recurso compartido no se monta dos veces en diferentes nodos). Los siguientes comandos inician SQL Server, comprueban el estado y después detienen SQL Server.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    sudo systemctl stop mssql-server
    ```

En este momento, ambas instancias de SQL Server están configuradas para ejecutarse con los archivos de base de datos en el almacenamiento compartido. El siguiente paso consiste en configurar SQL Server para Pacemaker. 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Instalación y configuración de Pacemaker en todos los nodos del clúster

1. **En ambos nodos del clúster, cree un archivo para almacenar el nombre de usuario y la contraseña de SQL Server para el inicio de sesión de Pacemaker**. Con el siguiente comando se crea y rellena este archivo:

    ```bash
    sudo touch /var/opt/mssql/secrets/passwd
    sudo echo '<loginName>' >> /var/opt/mssql/secrets/passwd
    sudo echo '<loginPassword>' >> /var/opt/mssql/secrets/passwd
    sudo chown root:root /var/opt/mssql/secrets/passwd 
    sudo chmod 600 /var/opt/mssql/secrets/passwd
    ```
2. **Todos los nodos del clúster deben poder tener acceso entre sí a través de SSH**. Herramientas como hb_report o crm_report (para solucionar problemas) y explorador de historial de Hawk requieren el acceso con SSH sin contraseña entre los nodos, ya que, de lo contrario, puede que solo recopilen datos desde el nodo actual. En caso de usar un puerto de SSH no estándar, use la opción -X (vea la página man). Por ejemplo, si el puerto SSH es 3479, invoque un hb_report del siguiente modo:

    ```bash
    crm_report -X "-p 3479" [...]
    ```

    Para más información, vea los [requisitos de sistema y recomendaciones en la documentación de SUSE](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_requirements_other.html).

3. **Instale la extensión de alta disponibilidad**. Para instalar la extensión, siga los pasos descritos en el siguiente tema de SUSE:
    
    [Installation and Setup Quick Start](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html) (Inicio rápido de instalación y configuración)

4. **Instale el agente de recursos de FCI para SQL Server**. Ejecute los siguientes comandos en ambos nodos:

    ```bash
    sudo zypper addrepo -fc https://packages.microsoft.com/config/sles/12/mssql-server-2017.repo
    sudo zypper --gpg-auto-import-keys refresh
    sudo zypper install mssql-server-ha
    ```

5. **Configure automáticamente el primer nodo**. El siguiente paso consiste en configurar un clúster de un nodo en ejecución, para lo cual configuraremos el primer nodo, SLES1. Siga las instrucciones del tema de SUSE [Setting Up the First Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node) (Configurar el primer nodo).

    Cuando acabe, compruebe el estado del clúster con `crm status`:
    ```bash
    crm status
    ```

    Debe mostrar que un nodo, SLES1, está configurado.

6. **Agregue nodos a un clúster existente**. A continuación, una el nodo SLES2 al clúster. Siga las instrucciones del tema de SUSE [Adding the Second Node](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.2nd-node) (Agregar el segundo nodo).
    
    Cuando acabe, compruebe el estado del clúster con **crm status**. Si ha agregado el segundo nodo correctamente, el resultado debería parecerse al siguiente:
        
    ```
    2 nodes configured
    1 resource configured
    Online: [ SLES1 SLES2 ]
    Full list of resources:
    admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
    ```

    > [!NOTE]
    > **admin_addr** es el recurso de clúster IP virtual que se configura durante la configuración inicial de un clúster de un nodo.

7.  **Procedimientos de eliminación**. Si necesita quitar un nodo del clúster, use el script de arranque **ha-cluster-remove**. Para más información, vea [Overview of the Bootstrap Scripts](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.bootstrap) (Información general de los scripts de arranque).  

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configuración de los recursos de clúster para SQL Server

En los pasos siguientes se explica cómo configurar el recurso de clúster para SQL Server. Hay dos opciones de configuración que debe personalizar.

- **Nombre del recurso de SQL Server**: nombre de recurso de SQL Server en clúster. 
- **Valor del tiempo de espera**: el valor del tiempo de espera es la cantidad de tiempo que el clúster espera mientras un recurso se pone en línea. En el caso de SQL Server, es el tiempo que espera que SQL Server tarde en poner en línea la base de datos maestra (`master`). 

Actualice los valores del siguiente script para su entorno. Ejecute en un nodo para configurar e iniciar el servicio en clúster.

```bash
sudo crm configure
primitive <sqlServerResourceName> ocf:mssql:fci op start timeout=<timeout_in_seconds>
colocation <constraintName> inf: <virtualIPResourceName> <sqlServerResourceName>
show
commit
exit
```

Por ejemplo, el siguiente script crea un recurso en clúster de SQL Server denominado mssqlha. 

```bash
sudo crm configure
primitive mssqlha ocf:mssql:fci op start timeout=60s
colocation admin_addr_mssqlha inf: admin_addr mssqlha
show
commit
exit
```

Una vez confirmada la configuración, SQL Server se iniciará en el mismo nodo que el recurso de IP virtual. 

Para obtener más información, vea [Configuring and Managing Cluster Resources (Command Line)](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config) [Configurar y administrar recursos en clúster (línea de comandos)]. 

### <a name="verify-that-sql-server-is-started"></a>Confirme que se ha iniciado SQL Server. 

Para comprobar que se ha iniciado SQL Server, ejecute el comando **crm status**:

```bash
crm status
```

En los siguientes ejemplos se muestran los resultados si Pacemaker se ha iniciado correctamente como un recurso en clúster. 
```
2 nodes configured
2 resources configured

Online: [ SLES1 SLES2 ]

Full list of resources:

 admin_addr     (ocf::heartbeat:IPaddr2):       Started SLES1
 mssqlha        (ocf::mssql:fci):       Started SLES1
```

## <a name="managing-cluster-resources"></a>Administración de recursos de clúster

Para administrar los recursos de clúster, consulte el siguiente tema de SUSE: [Managing Cluster Resources](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm ) (Administración de recursos de clúster)

### <a name="manual-failover"></a>Conmutación por error manual

Aunque los recursos se configuran para conmutar por error (o migrar) automáticamente a otros nodos del clúster en caso de que se produzca un error de hardware o software, también puede trasladar manualmente un recurso a otro nodo del clúster mediante la interfaz gráfica de usuario de Pacemaker o la línea de comandos. 

Use el comando migrate para llevar a cabo esta tarea. Por ejemplo, para migrar el recurso SQL a un nodo de clúster denominado SLES2, ejecute: 

```bash
crm resource
migrate mssqlha SLES2
```

## <a name="additional-resources"></a>Recursos adicionales

[SUSE Linux Enterprise High Availability Extension - Administration Guide](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html) (SUSE Linux Enterprise High Availability Extension: guía de administración) 
