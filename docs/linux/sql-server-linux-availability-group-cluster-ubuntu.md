---
title: Configurar un clúster de Ubuntu para un grupo de disponibilidad
titleSuffix: SQL Server on Linux
description: Aprenda a crear un clúster de tres nodos en Ubuntu y a agregar al clúster un recurso de grupo de disponibilidad creado anteriormente.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: dd0d6fb9-df0a-41b9-9f22-9b558b2b2233
ms.openlocfilehash: 13b719848e9cbced670e58ae3a747967a8901ada
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338302"
---
# <a name="configure-ubuntu-cluster-and-availability-group-resource"></a>Configurar un clúster de Ubuntu y un recurso de grupo de disponibilidad

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este documento, se explica cómo crear un clúster de tres nodos en Ubuntu y cómo agregar un grupo de disponibilidad creado anteriormente como un recurso en el clúster. Para lograr una alta disponibilidad, un grupo de disponibilidad en Linux necesita tres nodos (vea [High availability and data protection for availability group configurations](sql-server-linux-availability-group-ha.md) [Alta disponibilidad y protección de datos para configuraciones de grupos de disponibilidad]).

> [!NOTE] 
> En este momento, la integración de SQL Server con Pacemaker en Linux no es tan perfecta como con WSFC en Windows. Desde SQL, se desconoce la presencia del clúster, toda la orquestación está fuera y Pacemaker controla el servicio como una instancia independiente. Además, el nombre de red virtual es específico de WSFC y no hay ningún equivalente en Pacemaker. Las vistas de administración dinámica de Always On que consultan la información del clúster devuelven filas vacías. Puede crear un agente de escucha para usarlo en la reconexión transparente después de una conmutación por error, pero tendrá que registrar de forma manual el nombre del agente de escucha en el servidor DNS con la dirección IP que usada para crear el recurso de IP virtual (como se explica en las secciones siguientes).

En las secciones siguientes, se describen los pasos necesarios para configurar una solución de clúster de conmutación por error. 

## <a name="roadmap"></a>Plan de desarrollo

Los pasos necesarios para crear un grupo de disponibilidad en servidores con Linux de alta disponibilidad son distintos de los exigidos en un clúster de conmutación por error de Windows Server. En la lista siguiente, se describen los pasos generales: 

1. [Configure SQL Server en los nodos del clúster](sql-server-linux-setup.md).

2. [Cree el grupo de disponibilidad](sql-server-linux-availability-group-configure-ha.md). 

3. Configure un administrador de recursos de clúster, como Pacemaker. Estas instrucciones se incluyen en este documento.
   
   La manera de configurar un administrador de recursos de clúster depende de la distribución específica de Linux. 

   >[!IMPORTANT]
   >Para alcanzar una alta disponibilidad, los entornos de producción necesitan un agente de barrera (por ejemplo, STONITH). En los ejemplos de esta documentación, no se usan agentes de barrera. Los ejemplos solo se proporcionan con fines de prueba y validación. 
   >
   >Un clúster de Linux usa barreras para devolver el clúster a un estado conocido. La forma de configurar las barreras depende de la distribución y del entorno. En este momento, las barreras no están disponibles en algunos entornos de nube. Para obtener más información, vea [Support Policies for RHEL High Availability Clusters - Virtualization Platforms](https://access.redhat.com/articles/29440) (Directivas de soporte técnico para clústeres de alta disponibilidad de RHEL: plataformas de virtualización).
   >
   >Las barreras se suelen implementar en el sistema operativo y dependen del entorno. Busque instrucciones sobre las barreras en la documentación del distribuidor del sistema operativo.

5.  [Agregue el grupo de disponibilidad como un recurso en el clúster](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource). 

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Instalación y configuración de Pacemaker en todos los nodos del clúster

1. En todos los nodos, abra los puertos del firewall. Abra el puerto para el servicio de alta disponibilidad de Pacemaker, la instancia de SQL Server y el punto de conexión del grupo de disponibilidad. El puerto TCP predeterminado para el servidor que ejecuta SQL Server es el 1433.  

   ```bash
   sudo ufw allow 2224/tcp
   sudo ufw allow 3121/tcp
   sudo ufw allow 21064/tcp
   sudo ufw allow 5405/udp
        
   sudo ufw allow 1433/tcp # Replace with TDS endpoint
   sudo ufw allow 5022/tcp # Replace with DATA_MIRRORING endpoint
        
   sudo ufw reload
   ```
   
   Como alternativa, puede simplemente deshabilitar el firewall:
        
   ```bash
   sudo ufw disable
   ```

1. Instale los paquetes de Pacemaker. Ejecute los comandos siguientes en todos los nodos:

   ```bash
   sudo apt-get install pacemaker pcs fence-agents resource-agents
   ```

2. Establezca la contraseña para el usuario predeterminado que se crea al instalar paquetes de Pacemaker y Corosync. Use la misma contraseña en todos los nodos. 

   ```bash
   sudo passwd hacluster
   ```

## <a name="enable-and-start-pcsd-service-and-pacemaker"></a>Habilitar e iniciar el servicio pcsd y Pacemaker

El comando siguiente habilita e inicia el servicio pcsd y Pacemaker. Ejecútelo en todos los nodos. Esto permite que los nodos vuelvan a unirse al clúster después de un reinicio. 

```bash
sudo systemctl enable pcsd
sudo systemctl start pcsd
sudo systemctl enable pacemaker
```
>[!NOTE]
>El comando para habilitar Pacemaker puede completarse con el error “pacemaker Default-Start contains no runlevels, aborting” (El inicio predeterminado de pacemaker no contiene niveles de ejecución, anulando). Este mensaje es informativo únicamente y puede continuar con la configuración del clúster. 

## <a name="create-the-cluster"></a>Crear el clúster

1. Quite la configuración del clúster existente de todos los nodos. 

   Al ejecutar “sudo apt-get install pcs”, se instalan Pacemaker, corosync y pcs al mismo tiempo, y se inicia la ejecución de los tres servicios.  Al iniciar corosync, se genera un archivo de plantilla “/etc/cluster/corosync.conf”.  Para completar correctamente los pasos siguientes, el archivo no tiene que existir; por lo tanto, la solución alternativa es detener Pacemaker o corosync y eliminar “/etc/cluster/corosync.conf”; después, los pasos siguientes se completarán correctamente. “pcs cluster destroy” realiza la misma acción y puede usarlo como un paso de configuración inicial del clúster único.
   
   El comando siguiente elimina los archivos de configuración del clúster existente y detiene todos los servicios del clúster. Esto elimina el clúster de forma permanente. Ejecútelo como un primer paso en un entorno de preproducción. Tenga en cuenta que el comando “pcs cluster destroy” ha deshabilitado el servicio de Pacemaker y es necesario volver a habilitarlo. Ejecute el siguiente comando en todos los nodos.
   
   >[!WARNING]
   >El comando destruye los recursos del clúster existentes.

   ```bash
   sudo pcs cluster destroy 
   sudo systemctl enable pacemaker
   ```

1. Cree el clúster. 

   >[!WARNING]
   >Debido a un problema conocido que está investigando el proveedor de servicios de clústeres, se produce el error siguiente al iniciar el clúster (“pcs cluster start”). Esto se debe a que el archivo de registro configurado en “/etc/corosync/corosync.conf”, que se crea al ejecutar el comando de configuración del clúster, es incorrecto. Para solucionar este problema, cambie el archivo de registro a “/var/log/corosync/corosync.log”. Como alternativa, puede crear el archivo “/var/log/cluster/corosync.log”.
 
   ```Error
   Job for corosync.service failed because the control process exited with error code. 
   See "systemctl status corosync.service" and "journalctl -xe" for details.
   ```
  
El comando siguiente crea un clúster de tres nodos. Antes de ejecutar el script, reemplace los valores entre `< ... >`. Ejecute el comando siguiente en el nodo principal. 

   ```bash
   sudo pcs cluster auth <node1> <node2> <node3> -u hacluster -p <password for hacluster>
   sudo pcs cluster setup --name <clusterName> <node1> <node2...> <node3>
   sudo pcs cluster start --all
   sudo pcs cluster enable --all
   ```
   
   >[!NOTE]
   >Si ya ha configurado un clúster en los mismos nodos, debe usar la opción "--force" al ejecutar "pcs cluster setup". Tenga en cuenta que esto equivale a ejecutar "pcs cluster destroy" y que el servicio Pacemaker debe volver a habilitarse mediante "sudo systemctl enable pacemaker".


## <a name="configure-fencing-stonith"></a>Configuración de barrera (STONITH)

Para admitir la configuración del clúster, los proveedores de clústeres de Pacemaker requieren que se habilite STONITH y que se configure un dispositivo de barrera. Cuando el administrador de recursos del clúster no puede determinar el estado de un nodo o de un recurso de un nodo, se usa la barrera para devolver el clúster a un estado conocido. Básicamente, la barrera de nivel de recurso garantiza que, mediante la configuración de un recurso, no se dañen los datos en caso de una interrupción. Por ejemplo, puede usar la barrera de nivel de recurso con DRBD (dispositivo de bloque replicado distribuido) para marcar el disco de un nodo como obsoleto cuando el enlace de comunicaciones deje de funcionar. La barrera de nivel de nodo garantiza que un nodo no ejecute ningún recurso. Para ello, se restablece el nodo, cuya implementación en Pacemaker se denomina STONITH (que, por sus siglas en inglés, significa “disparar al otro nodo en la cabeza”). Pacemaker admite una gran variedad de dispositivos de barrera, como un sistema de alimentación ininterrumpida o tarjetas de interfaz de administración para servidores. Para obtener más información, vea [Pacemaker Clusters from Scratch](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/) (Clústeres de Pacemaker desde cero) y [Fencing and Stonith](https://clusterlabs.org/doc/crm_fencing.html) (Barreras y Stonith). 

Como la configuración de las barreras de nivel de nodo depende en gran medida de su entorno, lo hemos deshabilitado para este tutorial (se puede configurar en otro momento). Ejecute el script siguiente en el nodo principal: 

```bash
sudo pcs property set stonith-enabled=false
```

>[!IMPORTANT]
>Solo deshabilite STONITH con fines de prueba. Si tiene previsto usar Pacemaker en un entorno de producción, necesita planear una implementación de STONITH basada en su entorno y mantenerla habilitada. Póngase en contacto con el proveedor del sistema operativo para obtener información sobre los agentes de barrera de cualquier distribución específica. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Establecer la propiedad del clúster cluster-recheck-interval

`cluster-recheck-interval` indica el intervalo de sondeo por el que el clúster comprueba los cambios en los parámetros de recursos, las restricciones u otras opciones del clúster. Si una réplica se interrumpe, el clúster intenta reiniciarla en un intervalo que depende de los valores `failure-timeout` y `cluster-recheck-interval`. Por ejemplo, si `failure-timeout` se establece en 60 segundos y `cluster-recheck-interval` se establece en 120 segundos, el reinicio se intenta en un intervalo mayor de 60 segundos pero menor de 120 segundos. Se recomienda establecer el valor de failure-timeout en 60 segundos y el de cluster-recheck-interval en un valor superior a 60 segundos. No se recomienda establecer cluster-recheck-interval en un valor menor.

Para actualizar el valor de la propiedad a `2 minutes`, ejecute:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Si ya dispone de un recurso de grupo de disponibilidad administrado por un clúster de Pacemaker, tenga en cuenta que todas las distribuciones que usan el paquete más reciente de Pacemaker (1.1.18 -11.el7) introducen un cambio de comportamiento para el valor de clúster start-failure-is-fatal cuando su valor es false. Este cambio afecta al flujo de trabajo de conmutación por error. Si una réplica principal deja de funcionar, se espera que el clúster conmute por error a una de las réplicas secundarias disponibles. En su lugar, los usuarios observan que el clúster sigue intentando iniciar la réplica principal que ha experimentado el error. Si esa réplica principal no vuelve a activarse (debido a un corte de luz permanente), el clúster nunca conmuta por error a otra réplica secundaria disponible. Debido a este cambio, la recomendación de configurar start-failure-is-fatal ya no es válida y es necesario revertir a su valor predeterminado de `true`. Además, el recurso de grupo de disponibilidad debe actualizarse para incluir la propiedad `failover-timeout`. 
>
>Para actualizar el valor de la propiedad a `true`, ejecute:
>
>```bash
>sudo pcs property set start-failure-is-fatal=true
>```
>
>Actualice la propiedad `failure-timeout` del recurso del grupo de disponibilidad existente a `60s` (reemplace `ag1` por el nombre de su recurso del grupo de disponibilidad):
>
>```bash
>pcs resource update ag1 meta failure-timeout=60s
>```

## <a name="install-sql-server-resource-agent-for-integration-with-pacemaker"></a>Instalar el agente del recurso de SQL Server para la integración con Pacemaker

Ejecute los siguientes comandos en todos los nodos. 

```bash
sudo apt-get install mssql-server-ha
```

## <a name="create-a-sql-server-login-for-pacemaker"></a>Crear un inicio de sesión de SQL Server para Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Crear un recurso de grupo de disponibilidad

Para crear el recurso de grupo de disponibilidad, use el comando `pcs resource create` y establezca las propiedades del recurso. El comando siguiente crea un recurso del tipo principal/secundario de `ocf:mssql:ag` para el grupo de disponibilidad con el nombre `ag1`. 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=30s master meta notify=true

```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

## <a name="create-virtual-ip-resource"></a>Creación de un recurso de dirección IP virtual

Para crear el recurso de dirección IP virtual, ejecute el comando siguiente en un nodo. Use una dirección IP estática disponible de la red. Antes de ejecutar el script, reemplace los valores entre `< ... >` por una dirección IP válida.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

No hay ningún nombre de servidor virtual equivalente en Pacemaker. Para usar una cadena de conexión que apunte a un nombre del servidor de cadena y no usar la dirección IP, registre la dirección del recurso de IP y el nombre del servidor virtual que prefiera en la configuración DNS. En el caso de las configuraciones de recuperación ante desastres, registre el nombre de servidor virtual que prefiera y la dirección IP con los servidores DNS en el sitio principal y en el sitio de recuperación ante desastres.

## <a name="add-colocation-constraint"></a>Agregar una restricción de ubicación

Casi todas las decisiones de un clúster de Pacemaker, por ejemplo elegir dónde se debe ejecutar un recurso, se toman mediante la comparación de puntuaciones. Las puntuaciones se calculan por recurso, y el administrador del recurso de clúster elige el nodo con la puntuación más alta para un recurso determinado. (Si un nodo tiene una puntuación negativa para un recurso, el recurso no se puede ejecutar en ese nodo). Tenga precaución al configurar las decisiones del clúster. Las restricciones tienen una puntuación. Si una restricción tiene una puntuación inferior a INFINITY (infinito), solo es una recomendación. Una puntuación de INFINITY quiere decir que es obligatoria. Para garantizar que los recursos de réplica principal y dirección IP virtual se ejecuten en el mismo host, defina una restricción de ubicación con una puntuación de INFINITY. Para agregar la restricción de ubicación, ejecute el comando siguiente en un nodo. 

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Agregar una restricción de ordenación

La restricción de ubicación tiene una restricción de orden implícita. Mueve el recurso de dirección IP virtual antes de mover el recurso de grupo de disponibilidad. La secuencia de eventos predeterminada es la siguiente:

1. El usuario envía `pcs resource move` a la réplica principal del grupo de disponibilidad desde el nodo1 al nodo2.
1. El recurso de IP virtual se detiene en el nodo1.
1. El recurso de IP virtual se inicia en el nodo2.

   >[!NOTE]
   >En este momento, la dirección IP apunta de forma temporal al nodo2, mientras que el nodo2 aún es un nodo secundario antes de la conmutación por error. 
   
1. La réplica principal del grupo de disponibilidad del nodo1 se degrada a secundaria.
1. La réplica secundaria del grupo de disponibilidad del nodo2 se promueve a principal. 

Para evitar que la dirección IP apunte temporalmente al nodo con la réplica secundaria previa a la conmutación por error, agregue una restricción de ordenación. 

Para agregar una restricción de orden, ejecute el siguiente comando en un nodo:

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

>[!IMPORTANT]
>Después de configurar el clúster y agregar el grupo de disponibilidad como un recurso de clúster, no puede usar Transact-SQL para conmutar por error los recursos del grupo de disponibilidad. Los recursos de clúster de SQL Server en Linux no están tan bien integrados con el sistema operativo como lo están en un clúster de conmutación por error de Windows Server (WSFC). El servicio SQL Server no es consciente de la presencia del clúster. Toda la orquestación se realiza con las herramientas de administración de clústeres. En RHEL o Ubuntu, use `pcs`. 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Pasos siguientes

[Funcionamiento del grupo de disponibilidad de alta disponibilidad](sql-server-linux-availability-group-failover-ha.md)

