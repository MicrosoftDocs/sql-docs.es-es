---
title: 'Configuración de FCI para almacenamiento SMB: SQL Server en Linux'
description: Obtenga información sobre cómo configurar una instancia de clúster de conmutación por error (FCI) mediante almacenamiento SMB para SQL Server en Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/28/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: 6cc84164c2829d2cf6d5e66f40fd13921fdb105b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346326"
---
# <a name="configure-failover-cluster-instance---smb---sql-server-on-linux"></a>Configuración de la instancia de clúster de conmutación por error: SMB; SQL Server en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo configurar el almacenamiento SMB de una instancia de clúster de conmutación por error (FCI) en Linux. 
 
Fuera de Windows, se suele aludir a SMB como un recurso compartido del Sistema de archivos de Internet común (CIFS) y se implementa mediante Samba. En Windows, se accede a un recurso compartido de SMB de esta manera: \\NOMBRESERVIDOR\NOMBRERECURSOCOMPARTIDO. En el caso de las instalaciones de SQL Server basadas en Linux, el recurso compartido de SMB debe montarse como una carpeta.

## <a name="important-source-and-server-information"></a>Información importante sobre el código fuente y el servidor

Aquí tiene algunas sugerencias y notas para usar SMB correctamente:
- El recurso compartido de SMB puede estar en Windows, Linux o incluso en un dispositivo siempre que use SMB 3.0 o una versión superior. Para obtener más información sobre Samba y SMB 3.0, consulte [SMB 3.0](https://wiki.samba.org/index.php/Samba3/SMB2#SMB_3.0) para ver si su implementación de samba es compatible con SMB 3.0.
- El recurso compartido de SMB debe tener alta disponibilidad.
- La seguridad debe establecerse correctamente en el recurso compartido de SMB. A continuación se muestra un ejemplo de /etc/samba/smb.conf, donde SQLData1 es el nombre del recurso compartido.

![Captura de pantalla que muestra que SQLData1 es el nombre del grupo.][1]

## <a name="instructions"></a>Instructions

1. Elija uno de los servidores que participarán en la configuración de la FCI. No importa el que elija.
   
1. Obtenga información sobre el usuario mssql.
   
   ```bash
    sudo id mssql
   ```
   
   Anote el UID, el GID y los grupos. 
   
1. Ejecute `sudo smbclient -L //NameOrIP/ShareName -U User`.
   
   \<NameOrIP> es el nombre DNS o la dirección IP del servidor que hospeda el recurso compartido de SMB.
   
   \<ShareName> es el nombre del recurso compartido de SMB. 
   
1. En el caso de las bases de datos del sistema o cualquier elemento almacenado en la ubicación de datos predeterminada, siga estos pasos. De lo contrario, vaya al paso 5. 
   
   1. Asegúrese de que SQL Server está detenido en el servidor en el que está trabajando.
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Cambie por completo para ser el superusuario. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      sudo -i
      ```
      
   1. Cambie para ser el usuario de mssql. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      su mssql
      ```
      
   1. Cree un directorio temporal para almacenar los archivos de registro y los datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      mkdir <TempDir>
      ```
      
      \<TempDir> es el nombre de la carpeta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/tmp.
      
      ```bash
      mkdir /var/opt/mssql/tmp
      ```
      
   1. Copie los archivos de registro y los datos de SQL Server en el directorio temporal. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      cp /var/opt/mssql/data/* <TempDir>
      ```
      
      \<TempDir> es el nombre de la carpeta del paso anterior.
      
   1. Compruebe que los archivos están en el directorio.
      
      ```bash
      ls <TempDir>
      ```
      
      \<TempDir> es el nombre de la carpeta del paso d.
      
   1. Elimine los archivos del directorio de datos de SQL Server existente. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      rm - f /var/opt/mssql/data/*
      ```
      
   1. Compruebe que se han eliminado los archivos. 
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Escriba exit para volver al usuario raíz.
      
   1. Monte el recurso compartido de SMB en la carpeta de datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente. En este ejemplo se muestra la sintaxis para conectarse a un recurso compartido de SMB 3.0 basado en Windows Server.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> /var/opt/mssql/data -o vers=3.0,username=<UserName>,password=<Password>,domain=<domain>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> es el nombre del servidor con el recurso compartido de SMB.
      
      \<ShareName> es el nombre del recurso compartido.
      
      \<UserName> es el nombre del usuario para acceder al recurso compartido.
      
      \<Password> es la contraseña del usuario.
      
      \<domain> es el nombre de Active Directory.
      
      \<mssqlUID> es el UID del usuario de mssql. 
      
      \<mssqlGID> es el GID del usuario de mssql.
      
   1. Para comprobar que el montaje se ha realizado correctamente, envíe mount sin modificadores.
      
      ```bash
      mount
      ```
      
   1. Cambie al usuario de mssql. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      su mssql
      ```
      
   1. Copie los archivos del directorio temporal /var/opt/mssql/data. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      cp /var/opt/mssql/tmp/* /var/opt/mssql/data
      ```
      
   1. Compruebe que los archivos están allí.
      
      ```bash
      ls /var/opt/mssql/data
      ```
      
   1. Escriba exit para no ser mssql. 
      
   1. Escriba exit para no ser el usuario raíz.
   
   1. Inicie SQL Server. Si todo se ha copiado correctamente y la seguridad se ha aplicado correctamente, SQL Server debería mostrarse como iniciado.
      
      ```bash
      sudo systemctl start mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Para seguir probando, cree una base de datos para asegurarse de que los permisos sean correctos. En el ejemplo siguiente se usa Transact-SQL; puede usar SSMS.
      
      ![10_testcreatedb][2] 
      
   1. Detenga SQL Server y compruebe que se ha apagado. Si va a agregar o probar otros discos, no apague SQL Server hasta que se agreguen y prueben.
      
      ```bash
      sudo systemctl stop mssql-server
      sudo systemctl status mssql-server
      ```
      
   1. Solo si ha finalizado, desmonte el recurso compartido. En caso contrario, desmóntelo después de terminar de probar o agregar los discos adicionales.
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> es la dirección IP o nombre del host de SMB.
      
      \<ShareName> es el nombre del recurso compartido.
      
      \<FolderMountedIn> es el nombre de la carpeta donde se ha montado SMB.
      
5. Para comprobar otros aspectos que no sean las bases de datos del sistema (por ejemplo, las bases de datos de usuario o las copias de seguridad), siga estos pasos. Si solo usa la ubicación predeterminada, vaya al paso 14.
   
   1. Cambie para ser el superusuario. No recibirá ninguna confirmación si se realiza correctamente.
      
      ```bash
      sudo -i
      ```
      
   1. Cree una carpeta que usará SQL Server. 
      
      ```bash
      mkdir <FolderName>
      ```
      
      \<FolderName> es el nombre de la carpeta. Es necesario especificar la ruta de acceso completa de la carpeta si no está en la ubicación correcta. En el ejemplo siguiente se crea una carpeta denominada /var/opt/mssql/userdata.
      
      ```bash
      mkdir /var/opt/mssql/userdata
      ```
      
   1. Monte el recurso compartido de SMB en la carpeta de datos de SQL Server. No recibirá ninguna confirmación si se realiza correctamente. En este ejemplo se muestra la sintaxis para conectarse a un recurso compartido de SMB 3.0 basado en Samba.
      
      ```bash
      Mount -t cifs //<ServerName>/<ShareName> <FolderName> -o vers=3.0,username=<UserName>,password=<Password>,uid=<mssqlUID>,gid=<mssqlGID>,file_mode=0777,dir_mode=0777
      ```
      
      \<ServerName> es el nombre del servidor con el recurso compartido de SMB.
      
      \<ShareName> es el nombre del recurso compartido.
      
      \<FolderName> es el nombre de la carpeta creada en el último paso.  
      
      \<UserName> es el nombre del usuario para acceder al recurso compartido.
      
      \<Password> es la contraseña del usuario.
      
      \<mssqlUID> es el UID del usuario de mssql.
      
      \<mssqlGID> es el GID del usuario de mssql.
      
   1. Para comprobar que el montaje se ha realizado correctamente, envíe mount sin modificadores.
   
   1. Escriba exit para dejar de ser el superusuario.
   
   1. Para probar, cree una base de datos en esa carpeta. En el ejemplo siguiente se usa sqlcmd para crear una base de datos, cambiar el contexto a ella y comprobar que los archivos existen en el nivel del sistema operativo; después, se elimina la ubicación temporal. Puede usar SSMS.
   
   1. Desmontaje del recurso compartido 
      
      ```bash
      sudo umount //<IPAddressorServerName>/<ShareName> /<FolderMountedIn>
      ```
      
      \<IPAddressOrServerName> es la dirección IP o nombre del host de SMB.
      
      \<ShareName> es el nombre del recurso compartido.
      
      \<FolderMountedIn> es el nombre de la carpeta donde se ha montado SMB.
   
1. Repita los pasos en los demás nodos.

Ya puede configurar la FCI.

## <a name="next-steps"></a>Pasos siguientes

[Configurar la instancia de clúster de conmutación por error: SQL Server en Linux](sql-server-linux-shared-disk-cluster-configure.md)

<!--Image references-->
[1]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/05-smbsource.png 
[2]: ./media/sql-server-linux-shared-disk-cluster-configure-smb/10-testcreatedb.png 
