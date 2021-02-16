---
title: 'Implementación de un grupo de disponibilidad con HPE Serviceguard: SQL Server en Linux'
description: Use HPE Serviceguard como administrador de clústeres para lograr alta disponibilidad con un grupo de disponibilidad en SQL Server en Linux
ms.date: 01/15/2021
ms.prod: sql
ms.technology: linux
ms.topic: tutorial
author: amvin87
ms.author: amitkh
ms.reviewer: vanto
ms.openlocfilehash: 528d9550f79eea50db554dec11ead53039348cdb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100051936"
---
# <a name="tutorial---setup-a-three-node-always-on-availability-group-with-hpe-serviceguard-for-linux"></a>Tutorial: Configuración de un grupo de disponibilidad Always On de tres nodos con HPE Serviceguard para Linux 

En este tutorial se explica cómo configurar un grupo de disponibilidad Always On de SQL Server con HPE Serviceguard para Linux que se ejecuta en máquinas virtuales (VM) locales.

Consulte [Clústeres de HPE Serviceguard](https://h20195.www2.hpe.com/v2/GetPDF.aspx/c04154488.pdf) para obtener información general sobre los clústeres de HPE Serviceguard.

> [!NOTE]
> Microsoft admite el movimiento de datos, el grupo de disponibilidad y los componentes de SQL Server. Póngase en contacto con HPE para obtener soporte técnico relacionado con la documentación de administración de clústeres y cuórum de HPE Serviceguard.

Este tutorial consta de las tareas siguientes:

> [!div class="checklist"]
> * Instalación de SQL Server en las tres máquinas virtuales que formarán parte del grupo de disponibilidad
> * Instalación de HPE Serviceguard en las máquinas virtuales
> * Creación del clúster de HPE Serviceguard
> * Creación del grupo de disponibilidad y adición de una base de datos de ejemplo al grupo de disponibilidad
> * Implementación de la carga de trabajo de SQL Server en el grupo de disponibilidad a través del administrador de clústeres de Serviceguard
> * Realización de una conmutación por error automática y combinación del nodo con el clúster

## <a name="prerequisites"></a>Requisitos previos

* Tres máquinas virtuales en un entorno local que ejecute HPE Serviceguard en una distribución de Linux compatible.

   Para obtener un ejemplo de una distribución compatible, vea [HPE Serviceguard para Linux](https://h20195.www2.hpe.com/v2/gethtml.aspx?docname=c04154488).

   Consulte con HPE para obtener información sobre la compatibilidad con los entornos de nube pública.

   Las instrucciones de este tutorial se validan con HPE Serviceguard para Linux. Hay una edición de prueba disponible para descargar desde [HPE](https://www.hpe.com/us/en/resources/servers/serviceguard-linux-trial.html).

* Archivos de base de datos SQL Server en el montaje de volumen lógico (LVM) para las tres máquinas virtuales. Vea [Guía de inicio rápido para Serviceguard Linux (HPE)](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us).

* Asegúrese de que tiene un entorno de ejecución OpenJDK de Java instalado en las máquinas virtuales.

   > [!NOTE]
   > No se admite el SDK de Java de IBM.

## <a name="install-sql-server"></a>Instalación de SQL Server

En las tres máquinas virtuales, siga uno de los pasos siguientes en función de la distribución de Linux que elija para este tutorial, para instalar SQL Server y las herramientas.

### <a name="rhel"></a>RHEL

* [Instalación de SQL Server en Red Hat](quickstart-install-connect-red-hat.md#install)
* [Herramientas](quickstart-install-connect-red-hat.md#tools)

### <a name="sles"></a>SLES

* [Instalación de SQL Server en SLES](quickstart-install-connect-suse.md#install)
* [Herramientas](quickstart-install-connect-suse.md#tools)

Después de completar este paso, debe tener el servicio SQL Server y las herramientas instalados en las tres máquinas virtuales que participarán en el grupo de disponibilidad.

## <a name="install-hpe-serviceguard-on-the-vms"></a>Instalación de HPE Serviceguard en las máquinas virtuales

En este paso, instale HPE Serviceguard para Linux en las tres máquinas virtuales. En la tabla siguiente se describe el rol que desempeña cada servidor en el clúster.

|Número de VM | Rol HPE Serviceguard | Rol de réplica de grupo de disponibilidad de Microsoft SQL Server|
|--------------|-----------------|------------|
|1 | Nodos de clúster de HPE Serviceguard | Réplica principal |
|1 o más | Nodo de clúster de HPE Serviceguard | Réplica secundaria |
|1 | Servidor de cuórum de HPE Serviceguard | Réplica de solo configuración |

Para instalar Serviceguard, use el método `cminstaller`. Las instrucciones específicas están disponibles en los vínculos siguientes:

Clúster de Serviceguard y servidor de cuórum de Serviceguard

* [Instale Serviceguard para Linux en dos nodos](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_serviceguard_using_cminstaller). Consulte la sección **Install_serviceguard_using_cminstaller**.
* [Instale el servidor de cuórum de Serviceguard en el tercer nodo](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Install_QS_from_the_ISO). Consulte la sección **Install_QS_from_the_ISO**.

Después de completar la instalación del clúster de HPE Serviceguard, puede habilitar el portal de administración de clústeres en el puerto TCP 5522 en el nodo de la réplica principal. En los pasos siguientes se agrega una regla al firewall para permitir 5522; el comando siguiente es para una distribución de RHEL. Debe ejecutar comandos similares para otras distribuciones:

```console
sudo firewall-cmd --zone=public --add-port=5522/tcp --permanent

sudo firewall-cmd --reload 
```

## <a name="create-hpe-serviceguard-cluster"></a>Creación del clúster de HPE Serviceguard

Siga las instrucciones que se indican a continuación para configurar y crear el clúster de HPE Serviceguard. En este paso también se configurará el servidor de cuórum.

1. [Configure el servidor de cuórum de Serviceguard en el tercer nodo](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_QS). Consulte la sección **Configure_QS**.
2. [Configure y cree un clúster de Serviceguard en los otros dos nodos](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Configure_and_create_cluster). Consulte la sección **Configure_and_create_Cluster**.

## <a name="create-the-availability-group-and-add-a-sample-database"></a>Creación del grupo de disponibilidad y adición de una base de datos de ejemplo

En este paso, cree un grupo de disponibilidad con dos (o más) réplicas sincrónicas y una réplica de solo configuración, que proporciona protección de datos y también puede proporcionar alta disponibilidad. El diagrama siguiente representa esta arquitectura:

:::image type="content" source="media/sql-server-linux-availability-group-ha/2-configuration-only.png" alt-text="La réplica principal sincroniza los datos de usuario y los de configuración con la réplica secundaria. La réplica principal solo sincroniza los datos de configuración con la réplica de solo configuración. La réplica de solo configuración no tiene réplicas de datos de usuario.":::

1. Replicación sincrónica de datos de usuario en la réplica secundaria. También incluye los metadatos de configuración del grupo de disponibilidad.

2. Replicación sincrónica de los metadatos de configuración del grupo de disponibilidad. No incluye los datos de usuario.

Para obtener más información, consulte [Dos réplicas sincrónicas y una réplica de solo configuración](sql-server-linux-availability-group-ha.md).

Para crear el grupo de disponibilidad, siga estos pasos:

1. [Habilite Grupos de disponibilidad Always On y reinicie mssql-server](#enable-always-on-availability-groups-and-restart-mssql-server) en todas las máquinas virtuales, incluida la réplica de solo configuración.
2. [Habilite una sesión de eventos `AlwaysOn_health` (opcional)](#enable-an-alwayson_health-event-session---optional).
3. [Cree un certificado en la máquina virtual principal](#create-a-certificate-on-the-primary-vm).
4. [Crear el certificado en los servidores secundarios](#create-the-certificate-on-secondary-servers)
5. [Cree los puntos de conexión de creación de reflejo de la base de datos en las réplicas](#create-the-database-mirroring-endpoints-on-the-replicas).
6. [Cree el grupo de disponibilidad](#create-availability-group).
7. [Combine las réplicas secundarias](#join-the-secondary-replicas).
8. [Agregue una base de datos al grupo de disponibilidad creado antes](#add-a-database-to-the-availability-group-created-above).

### <a name="enable-always-on-availability-groups-and-restart-mssql-server"></a>Habilitar los grupos de disponibilidad AlwaysOn y reiniciar mssql-server

Habilite la característica Always On en todos los nodos en los que se hospede una instancia de SQL Server. Después, reinicie mssql-server. Ejecute el script siguiente en los tres nodos:

```console
sudo /opt/mssql/bin/mssql-conf
set hadr.hadrenabled 1 sudo systemctl restart mssql-serve
```

### <a name="enable-an-alwayson_health-event-session---optional"></a>Habilitación de una sesión de eventos `AlwaysOn_health` (opcional)

Opcionalmente, habilite los eventos extendidos de los grupos de disponibilidad Always On para facilitar el diagnóstico de la causa raíz cuando solucione los problemas de un grupo de disponibilidad. Ejecute el comando siguiente en todas las instancias de SQL Server:

```tsql
ALTER EVENT SESSION AlwaysOn_health ON SERVER WITH (STARTUP_STATE=ON);
GO
```

### <a name="create-a-certificate-on-the-primary-vm"></a>Creación de un certificado en la máquina virtual principal

El script de Transact-SQL siguiente crea una clave maestra y un certificado. Después, realiza una copia de seguridad del certificado y protege el archivo con una clave privada. Actualice el script con contraseñas seguras. Conéctese a la instancia de SQL Server principal y ejecute el siguiente script de Transact-SQL:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<Master_Key_Password';

CREATE CERTIFICATE dbm_certificate WITH SUBJECT = 'dbm';

BACKUP CERTIFICATE dbm_certificate TO FILE = '/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY 
    ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
      ENCRYPTION BY PASSWORD = '<Private_Key_Password>' );

```

En este momento, la réplica principal de SQL Server tiene un certificado en `/var/opt/mssql/data/dbm_certificate.cer` y una clave privada en `var/opt/mssql/data/dbm_certificate.pvk`. Copie estos dos archivos en la misma ubicación en todos los servidores que hospedarán las réplicas de disponibilidad. Utilice el usuario de mssql o conceda permiso al usuario de mssql para tener acceso a estos archivos.

Por ejemplo, en el servidor de origen, el siguiente comando copia los archivos en el equipo de destino. Reemplace los valores `'node2'` por el nombre del host que ejecuta la instancia de SQL Server secundaria. Copie también el certificado en la réplica de solo configuración y ejecute los comandos siguientes en ese nodo.

```console
cd /var/opt/mssql/data
scp dbm_certificate.* root@<node2>:/var/opt/mssql/data/
```

Ahora, en las máquinas virtuales secundarias que ejecutan la instancia secundaria y la réplica de solo configuración de SQL Server, ejecute los comandos siguientes para que el usuario mssql pueda poseer el certificado copiado:

```console
cd /var/opt/mssql/data

chown mssql:mssql dbm_certificate.*
```

### <a name="create-the-certificate-on-secondary-servers"></a>Crear el certificado en los servidores secundarios

El script de Transact-SQL siguiente crea una clave maestra y un certificado a partir de la copia de seguridad creada en la réplica principal de SQL Server. Actualice el script con contraseñas seguras. La contraseña de descifrado es la misma que ha usado para crear el archivo .pvk en un paso anterior. Para crear el certificado, ejecute el script siguiente en todos los servidores secundarios, excepto en la réplica de solo configuración:

```tsql
CREATE MASTER KEY ENCRYPTION BY PASSWORD =
'<Master_Key_Password>';

CREATE CERTIFICATE dbm_certificate FROM FILE =
'/var/opt/mssql/data/dbm_certificate.cer'
WITH PRIVATE KEY ( FILE = '/var/opt/mssql/data/dbm_certificate.pvk',
DECRYPTION BY PASSWORD = '<Private_Key_Password>' );
```

### <a name="create-the-database-mirroring-endpoints-on-the-replicas"></a>Creación de los puntos de conexión de creación de reflejo de la base de datos en las réplicas

En la réplica principal y en la secundaria, ejecute los comandos siguientes para crear los puntos de conexión de creación de reflejo de la base de datos:

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING 
        (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

> [!NOTE]
> 5022 es el puerto estándar que se usa para el punto de conexión de creación de reflejo de la base de datos, pero puede cambiarlo por cualquier puerto disponible.

En la réplica de solo configuración, cree el punto de conexión de creación de reflejo de la base de datos con el comando siguiente; tenga en cuenta que aquí el valor Rol se establece en `WITNESS`, lo que se necesita para la réplica de solo configuración.

```tsql
CREATE ENDPOINT [hadr_endpoint] AS TCP (LISTENER_PORT = 5022)
    FOR DATABASE_MIRRORING (
        ROLE = WITNESS,
        AUTHENTICATION = CERTIFICATE dbm_certificate,
        ENCRYPTION = REQUIRED ALGORITHM AES
        );

ALTER ENDPOINT [hadr_endpoint] STATE = STARTED;
```

### <a name="create-availability-group"></a>Crear grupo de disponibilidad

En la instancia de la réplica principal, ejecute los comandos siguientes. Estos comandos crean un grupo de disponibilidad denominado **ag1** que tiene un elemento `cluster_type` **Externo** y concede el permiso CREATE DATABASE al grupo de disponibilidad.

Antes de ejecutar los scripts siguientes, reemplace los marcadores de posición `<node1>`, `<node2>` y `<node3>` (réplica de solo configuración) por el nombre de las máquinas virtuales que ha creado en los pasos anteriores.

```tsql
CREATE AVAILABILITY GROUP [ag1]
    WITH (CLUSTER_TYPE = EXTERNAL)
    FOR REPLICA ON
    N'<node1>' WITH (
        ENDPOINT_URL = N'tcp://<node1>:<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),

    N'<node2>' WITH (
        ENDPOINT_URL = N'tcp://<node2>:\<5022>',
        AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
        FAILOVER_MODE = EXTERNAL,
        SEEDING_MODE = AUTOMATIC
        ),
    
    N'<node3>' WITH (
        ENDPOINT_URL = N'tcp://<node3>:<5022>',
        AVAILABILITY_MODE = CONFIGURATION_ONLY
        );
          
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

### <a name="join-the-secondary-replicas"></a>Combinación de las réplicas secundarias

Ejecute los comandos siguientes en todas las réplicas secundarias. Estos comandos combinan las réplicas secundarias al grupo de disponibilidad "ag1" con la réplica principal y proporcionan acceso de creación de base de datos al grupo de disponibilidad ag1.

```tsql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
GO
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
GO
```

### <a name="add-a-database-to-the-availability-group-created-above"></a>Adición de una base de datos al grupo de disponibilidad creado antes

Conéctese a la réplica principal y ejecute los comandos T-SQL siguientes para:

1. Crear una base de datos de ejemplo denominada **db1** que se agregará al grupo de disponibilidad.
2. Establecer el modelo de recuperación de la base de datos en Completa. Todas las bases de datos de un grupo de disponibilidad necesitan el modelo de recuperación completa.
3. Realice una copia de seguridad de la base de datos. Una base de datos necesita al menos una copia de seguridad completa antes de poder agregarla a un grupo de disponibilidad.

```tsql
CREATE DATABASE [db1]; -- creates a database named db1
GO
ALTER DATABASE [db1] SET RECOVERY FULL; -- set the database in full recovery mode
GO
BACKUP DATABASE [db1] -- backs up the database to disk TO DISK = N'/var/opt/mssql/data/db1.bak';
GO
ALTER AVAILABILITY GROUP [ag1] ADD DATABASE [db1]; -- adds the database db1 to the AG
GO
```

Después de completar correctamente los pasos anteriores, puede ver que se ha creado un grupo de disponibilidad **ag1** y las tres máquinas virtuales agregadas como réplica con una réplica principal, una réplica secundaria y una réplica de solo configuración. **ag1** contiene una base de datos.

## <a name="deploy-the-sql-server-availability-group-workload-hpe-cluster-manager"></a>Implementación de la carga de trabajo del grupo de disponibilidad de SQL Server (Administrador de clústeres de HPE)

En HPE Serviceguard, implemente la carga de trabajo de SQL Server en el grupo de disponibilidad a través de la interfaz de usuario del administrador de clústeres de Serviceguard.
   
Implemente la carga de trabajo del grupo de disponibilidad y habilite la alta disponibilidad (HA) y la recuperación ante desastres (DR) a través del clúster de Serviceguard mediante la [interfaz gráfica de usuario del administrador de Serviceguard](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Protect_your_alwayson_availability_group). Consulte la sección **Protección de Microsoft SQL Server en Linux para grupos de disponibilidad Always On**.

## <a name="perform-automatic-failover-and-join-the-node-back-to-cluster"></a>Realización de la conmutación automática por error y combinación del nodo con el clúster

Para la prueba de conmutación automática por error, puede continuar y desactivar la réplica principal (desconectarla); esto replicará la falta de disponibilidad repentina del nodo principal. Éste es el comportamiento esperado:

1. El administrador de clústeres promueve a principal una de las réplicas secundarias del grupo de disponibilidad.
2. La réplica principal con error se combina automáticamente con el clúster después de que se realice la copia de seguridad. El administrador de clústeres la promueve a una réplica secundaria.

En el caso de HPE Serviceguard, consulte la sección [**Prueba de la configuración para la preparación de la conmutación por error**](https://support.hpe.com/hpesc/public/docDisplay?docId=a00107699en_us#Test_the_setup_preparedness).

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre [grupos de disponibilidad Always On en Linux](sql-server-linux-availability-group-overview.md).
