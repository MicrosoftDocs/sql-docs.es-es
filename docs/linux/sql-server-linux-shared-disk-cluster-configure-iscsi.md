---
title: 'Configuración del almacenamiento de FCI de iSCSI: SQL Server en Linux'
description: Obtenga información sobre cómo configurar una instancia de clúster de conmutación por error (FCI) mediante iSCSI para SQL Server en Linux.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 06/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 20953eb151912f6eddf1e3ea0e13d34f3835e7d6
ms.sourcegitcommit: 3e2421ae45a8e9fa57fb590a5d1a5566721ea74a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/27/2021
ms.locfileid: "98921401"
---
# <a name="configure-failover-cluster-instance---iscsi---sql-server-on-linux"></a>Configuración de una instancia de clúster de conmutación por error: iSCSI: SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo configurar el almacenamiento de iSCSI de una instancia de clúster de conmutación por error (FCI) en Linux. 

## <a name="configure-iscsi"></a>Configuración de iSCSI 
iSCSI usa redes para presentar los discos de un servidor conocido como destino para los servidores. Los servidores que se conectan al destino iSCSI requieren que se configure un iniciador iSCSI. Los discos del destino reciben permisos explícitos para que solo puedan acceder a ellos los iniciadores autorizados. El propio destino debe ser de alta disponibilidad y confiable.

### <a name="important-iscsi-target-information"></a>Información importante del destino iSCSI
Aunque esta sección no trata de cómo configurar un destino iSCSI, ya que es específico del tipo de origen que se va a usar, debe asegurarse de que se haya configurado la seguridad de los discos usados por los nodos del clúster.  

El destino no debe configurarse nunca en los nodos de FCI si se usa un destino iSCSI basado en Linux. Por cuestiones de rendimiento y disponibilidad, las redes iSCSI deben ser independientes de las que usa el tráfico de red normal tanto en el servidor de origen como en el de cliente. Las redes que se usan para iSCSI deben ser rápidas. Recuerde que la red consume algo de ancho de banda del procesador, por lo que debe planificarse en consecuencia si usa un servidor normal.
Lo más importante que debe comprobar que se lleva a cabo en el destino es que los discos creados tengan asignados los permisos adecuados para que solo los servidores que participan en la FCI tengan acceso a ellos. A continuación se muestra un ejemplo del destino iSCSI de Microsoft, donde linuxnodes1 es el nombre creado. En este caso, se asignan las direcciones IP de los nodos para que puedan ver NewFCIDisk1.vhdx.

![Iniciador][1]

### <a name="instructions"></a>Instructions

En esta sección se explicará cómo configurar un iniciador iSCSI en los servidores que actuarán como nodos para la FCI. Las instrucciones deberían funcionar tal cual en RHEL y Ubuntu.

Para obtener más información sobre el iniciador iSCSI para las distribuciones admitidas, consulte los vínculos siguientes:
- [Red Hat](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Storage_Administration_Guide/iscsi-api.html)
- [SUSE](https://www.suse.com/documentation/sles11/stor_admin/data/sec_inst_system_iscsi_initiator.html) 
- [Ubuntu](https://help.ubuntu.com/lts/serverguide/iscsi-initiator.html)

1.  Elija uno de los servidores que participarán en la configuración de la FCI. No importa el que elija. iSCSI debe estar en una red dedicada, por lo que debe configurar iSCSI de modo que reconozca y use esa red. Ejecute `sudo iscsiadm -m iface -I <iSCSIIfaceName> -o new`, `<iSCSIIfaceName>` donde es el nombre único o descriptivo de la red. En el ejemplo siguiente se usa `iSCSINIC`:
   
    ```bash
    sudo iscsiadm -m iface -I iSCSINIC -o new
    ```
    ![Captura de pantalla del comando iface y la respuesta al comando.][6]
 
2.  Edite `/var/lib/iscsi/ifaces/iSCSIIfaceName`. Asegúrese de que tenga los siguientes valores rellenados por completo:

    - iface.net_ifacename es el nombre de la tarjeta de red tal como aparece en el sistema operativo.
    - iface.hwaddress es la dirección MAC del nombre único que se creará para la interfaz indicada a continuación.
    - iface.ipaddress
    - iface.subnet_Mask 

    Observe el ejemplo siguiente:

    ![Captura de pantalla del archivo con los valores rellenados al completo.][2]

3.  Busque el destino iSCSI.

    ```bash
    sudo iscsiadm -m discovery -t sendtargets -I <iSCSINetName> -p <TargetIPAddress>:<TargetPort>
    ```

     \<iSCSINetName> es el nombre único o descriptivo de la red, \<TargetIPAddress> es la dirección IP del destino iSCSI y \<TargetPort> es el puerto del destino iSCSI. 

    ![Captura de pantalla del comando detección y la respuesta al comando.][3]

 
4.  Inicie sesión en el destino.

    ```bash
    sudo iscsiadm -m node -I <iSCSIIfaceName> -p TargetIPAddress -l
    ```

    \<iSCSIIfaceName> es el nombre único o descriptivo de la red, y \<TargetIPAddress> es la dirección IP del destino iSCSI.

    ![iSCSITargetLogin][4]

5.  Compruebe que existe una conexión con el destino iSCSI.

    ```bash
    sudo iscsiadm -m session
    ```

    ![iSCSIVerify][5]

 
6.  Compruebe los discos conectados iSCSI.

    ```bash
    sudo grep "Attached SCSI" /var/log/messages
    ```
    ![Captura de pantalla del comando grep y la respuesta al comando en la que se muestran los discos SCSI conectados.][7]

7.  Cree un volumen físico en el disco iSCSI.

    ```bash
    sudo pvcreate /dev/<devicename>
    ```

    \<devicename> es el nombre del dispositivo del paso anterior. 

 
8.  Cree un grupo de volúmenes en el disco iSCSI. Los discos asignados a un único grupo de volúmenes se ven como un grupo o una colección. 

    ```bash
    sudo vgcreate <VolumeGroupName> /dev/devicename
    ```

    \<VolumeGroupName> es el nombre del grupo de volúmenes y \<devicename> el nombre del dispositivo del paso 6. 
 
9.  Cree y compruebe el volumen lógico del disco.

    ```bash
    sudo lvcreate -Lsize -n <LogicalVolumeName> <VolumeGroupName>
    ```
    
    \<size> es el tamaño del volumen que se va a crear y que se puede especificar con G (gigabytes), T (terabytes), etc.; \<LogicalVolumeName> es el nombre del volumen lógico y \<VolumeGroupName> el del grupo de volúmenes del paso anterior. 

    En el ejemplo siguiente se crea un volumen de 25 GB.
 
    ![Create25GBVol][10]

10. Ejecute `sudo lvs` para ver el LVM que se ha creado.
 
11. Dé formato al volumen lógico con un sistema de archivos compatible. Para EXT4, use el ejemplo siguiente:

    ```bash
    sudo mkfs.ext4 /dev/<VolumeGroupName>/<LogicalVolumeName>
    ```

    \<VolumeGroupName> es el nombre del grupo de volúmenes del paso anterior. \<LogicalVolumeName> es el nombre del volumen lógico del paso anterior.  

12. En el caso de las bases de datos del sistema o cualquier elemento almacenado en la ubicación de datos predeterminada, siga estos pasos. De lo contrario, vaya al paso 13.

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
    
        \<TempDir> es el nombre de la carpeta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/TempDir.

        ```bash
        mkdir /var/opt/mssql/TempDir
        ```
        
   * Copie los archivos de registro y los datos de SQL Server en el directorio temporal. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        cp /var/opt/mssql/data/* <TempDir>
        ```
    
        \<TempDir> es el nombre de la carpeta del paso anterior.
    
   * Compruebe que los archivos están en el directorio.

        ```bash
        ls \<TempDir>
        ```
 
        \<TempDir> es el nombre de la carpeta del paso d.

   * Elimine los archivos del directorio de datos de SQL Server existente. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        rm - f /var/opt/mssql/data/*
        ```
    
   * Compruebe que se han eliminado los archivos. En la imagen siguiente se muestra un ejemplo de la secuencia completa de c a h.

        ```bash
        ls /var/opt/mssql/data
        ```
    
        ![Captura de pantalla del comando ls y la respuesta al comando.][8]

   * Escriba `exit` para volver al usuario raíz.

   * Monte el volumen lógico de iSCSI en la carpeta de datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> /var/opt/mssql/data
        ```

        \<VolumeGroupName> es el nombre del grupo de volúmenes y \<LogicalVolumeName> el del volumen lógico que se ha creado. La siguiente sintaxis de ejemplo coincide con el grupo de volúmenes y el volumen lógico del comando anterior.

        ```bash
        mount /dev/FCIDataVG1/FCIDataLV1 /var/opt/mssql/data
        ```

   * Cambie el propietario del montaje a mssql. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        chown mssql /var/opt/mssql/data
        ```

   * Cambie la propiedad del grupo del montaje a mssql. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        chgrp mssql /var/opt/mssql/data
        ```

   * Cambie al usuario de mssql. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        su mssql
        ``` 

   * Copie los archivos del directorio temporal /var/opt/mssql/data. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        cp /var/opt/mssql/TempDir/* /var/opt/mssql/data
        ``` 
    
   * Compruebe que los archivos están allí.

        ```bash
        ls /var/opt/mssql/data
        ``` 

   *    Escriba `exit` para no ser mssql.

   *    Escriba `exit` para no ser el usuario raíz.

   *    Inicie SQL Server. Si todo se ha copiado correctamente y la seguridad se ha aplicado correctamente, SQL Server debería mostrarse como iniciado.

        ```bash
        sudo systemctl start mssql-server
        sudo systemctl status mssql-server
        ``` 

   *    Detenga SQL Server y compruebe que se ha apagado.

        ```bash
        sudo systemctl stop mssql-server
        sudo systemctl status mssql-server
        ``` 

13. Para comprobar otros aspectos que no sean las bases de datos del sistema (por ejemplo, las bases de datos de usuario o las copias de seguridad), siga estos pasos. Si solo usa la ubicación predeterminada, vaya al paso 14.

   *    Cambie para ser el superusuario. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        sudo -i
        ```

   *    Cree una carpeta que usará SQL Server. 

        ```bash
        mkdir <FolderName>
        ```

        \<FolderName> es el nombre de la carpeta. Es necesario especificar la ruta de acceso completa de la carpeta si no está en la ubicación correcta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/userdata.

        ```bash
        mkdir /var/opt/mssql/userdata
        ```

   *    Monte el volumen lógico de iSCSI en la carpeta creada en el paso anterior. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        mount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> es el nombre del grupo de volúmenes, \<LogicalVolumeName> el nombre del volumen lógico que se ha creado y \<FolderName> el nombre de la carpeta. A continuación, se muestra un ejemplo de la sintaxis.

        ```bash
        mount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

   *    Cambie a mssql la propiedad de la carpeta creada. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> es el nombre de la carpeta que se ha creado. A continuación se muestra un ejemplo.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Cambie a mssql el grupo de la carpeta creada. No recibirá ninguna confirmación si se realiza correctamente.

        ```bash
        chown mssql <FolderName>
        ```

        \<FolderName> es el nombre de la carpeta que se ha creado. A continuación se muestra un ejemplo.

        ```bash
        chown mssql /var/opt/mssql/userdata
        ```

   *    Escriba `exit` para dejar de ser el superusuario.

   *    Para probar, cree una base de datos en esa carpeta. En el ejemplo siguiente se usa sqlcmd para crear una base de datos, cambiar el contexto a ella y comprobar que los archivos existen en el nivel del sistema operativo; después, se elimina la ubicación temporal. Puede usar SSMS.
  
        ![Captura de pantalla del comando sqlcmd y la respuesta al comando.][9]

   *    Desmontaje del recurso compartido 

        ```bash
        sudo umount /dev/<VolumeGroupName>/<LogicalVolumeName> <FolderName>
        ```

        \<VolumeGroupName> es el nombre del grupo de volúmenes, \<LogicalVolumeName> el nombre del volumen lógico que se ha creado y \<FolderName> el nombre de la carpeta. A continuación, se muestra un ejemplo de la sintaxis.

        ```bash
        sudo umount /dev/FCIDataVG2/FCIDataLV2 /var/opt/mssql/userdata 
        ```

14. Configure el servidor para que solo Pacemaker pueda activar el grupo de volúmenes.

    ```bash
    sudo lvmconf --enable-halvm --services -startstopservices
    ```

15. Genere una lista de los grupos de volúmenes del servidor. El sistema usará cualquier elemento que no sea el disco iSCSI, como el disco del sistema operativo.

    ```bash
    sudo vgs
    ```

16. Modifique la sección de configuración de activación del archivo /etc/lvm/lvm.conf. Configure la línea siguiente:

    ```bash
    volume_list = [ <ListOfVGsNotUsedByPacemaker> ]
    ```

    \<ListOfVGsNotUsedByPacemaker> es la lista de grupos de volúmenes de la salida del paso 20 que la FCI no usará. Coloque cada uno entre comillas y sepárelos con una coma. A continuación se muestra un ejemplo.

    ![Captura de pantalla en la que se muestra un ejemplo de valor volume_list.][11]

17. Cuando se inicie Linux, montará el sistema de archivos. Para asegurarse de que solo Pacemaker pueda montar el disco iSCSI, vuelva a compilar la imagen del sistema de archivos raíz. 

    Ejecute el comando siguiente, que podría tardar unos minutos en completarse. No recibirá ningún mensaje si se realiza correctamente.

    ```bash
    sudo dracut -H -f /boot/initramfs-$(uname -r).img $(uname -r)
    ```

18. Reinicie el servidor.

19. En otro servidor que vaya a participar en la FCI, realice los pasos 1-6. Se presentará el destino iSCSI al SQL Server. 
 
20. Genere una lista de los grupos de volúmenes del servidor. Debería mostrar el grupo de volúmenes creado anteriormente. 

    ```bash
    sudo vgs
    ``` 

23. Inicie SQL Server y compruebe que se puede iniciar en este servidor.

    ```bash
    sudo systemctl start mssql-server
    sudo systemctl status mssql-server
    ```

24. Detenga SQL Server y compruebe que se ha apagado.

    ```bash
    sudo systemctl stop mssql-server
    sudo systemctl status mssql-server
    ```

25. Repita los pasos 1-6 en cualquier otro servidor que vaya a participar en la FCI.

Ya puede configurar la FCI.

| Distribución | Tema |
| :----------- | :---- |
| Red Hat Enterprise Linux con complemento de alta disponibilidad | [Configuración](sql-server-linux-shared-disk-cluster-configure.md)<br/>[Operaciones](sql-server-linux-shared-disk-cluster-red-hat-7-operate.md) |
| SUSE Linux Enterprise Server con complemento de alta disponibilidad | [Configuración](sql-server-linux-shared-disk-cluster-sles-configure.md) |

## <a name="next-steps"></a>Pasos siguientes

[Configurar la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/05-initiator.png
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/10-iscsiTargetSettings.png
[3]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/15-iSCSITargetResults.png
[4]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/20-iSCSITargetLogin.png
[5]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/25-iSCSIVerify.png
[6]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/7-setiscsinetwork.png
[7]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/30-iSCSIattachedDisks.png
[8]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/45-CopyMove.png
[9]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/50-ExampleCreateSSMS.png
[10]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/40-Create25GBVol.png
[11]: ./media/sql-server-linux-shared-disk-cluster-configure-iscsi/55-ListOfVGs.png
