---
title: 'SUSE: Configuración del grupo de disponibilidad para SQL Server en Linux'
titleSuffix: SQL Server
description: Obtenga información sobre cómo crear clústeres de grupos de disponibilidad para SQL Server en SUSE Linux Enterprise Server (SLES).
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 04/30/2018
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 85180155-6726-4f42-ba57-200bf1e15f4d
ms.openlocfilehash: 232235e1e7b8718b83c4826b4a019cb6c52e437c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338416"
---
# <a name="configure-sles-cluster-for-sql-server-availability-group"></a>Configuración de clústeres de SLES para grupos de disponibilidad de SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En esta guía se proporcionan instrucciones para crear un clúster de tres nodos para SQL Server en SUSE Linux Enterprise Server (SLES) 12 SP2. Para lograr una alta disponibilidad, un grupo de disponibilidad en Linux necesita tres nodos (vea [High availability and data protection for availability group configurations](sql-server-linux-availability-group-ha.md) [Alta disponibilidad y protección de datos para configuraciones de grupos de disponibilidad]). La capa de agrupación en clústeres se basa en SUSE [High Availability Extension (HAE)](https://www.suse.com/products/highavailability) desarrollado sobre [Pacemaker](https://clusterlabs.org/). 

Para obtener más información sobre la configuración del clúster, las opciones del agente de recursos y la administración, así como para obtener recomendaciones y ver los procedimientos recomendados, vea [SUSE Linux Enterprise High Availability Extension 12 SP2](https://www.suse.com/documentation/sle-ha-12/index.html).

>[!NOTE]
>En este momento, la integración de SQL Server con Pacemaker en Linux no es tan perfecta como con WSFC en Windows. El servicio SQL Server en Linux no es compatible con clústeres. Pacemaker controla toda la orquestación de los recursos del clúster, incluido el grupo de disponibilidad. En Linux, no debería confiar en las vistas de administración dinámica (DMV) del grupo de disponibilidad Always On que proporcionan información de clúster, como sys.dm_hadr_cluster. Además, el nombre de red virtual es específico de WSFC y no hay ningún equivalente en Pacemaker. No obstante, puede crear un agente de escucha para usarlo en la reconexión transparente tras una conmutación por error, pero tendrá que registrar manualmente el nombre del agente de escucha en el servidor DNS con la dirección IP que se usa para crear el recurso de IP virtual (como se explica en las secciones siguientes).

[!INCLUDE [bias-sensitive-term-t](../includes/bias-sensitive-term-t.md)]

## <a name="roadmap"></a>Plan de desarrollo

El procedimiento para crear un grupo de disponibilidad para alta disponibilidad difiere entre los servidores Linux y un clúster de conmutación por error de Windows Server. En la lista siguiente, se describen los pasos generales: 

1. [Configure SQL Server en los nodos del clúster](sql-server-linux-setup.md).

2. [Cree el grupo de disponibilidad](sql-server-linux-availability-group-failover-ha.md). 

3. Configure un administrador de recursos de clúster, como Pacemaker. Estas instrucciones se incluyen en este documento.
   
   La manera de configurar un administrador de recursos de clúster depende de la distribución específica de Linux. 

   >[!IMPORTANT]
   >Para alcanzar una alta disponibilidad, los entornos de producción necesitan un agente de barrera (por ejemplo, STONITH). En los ejemplos de este artículo no se usan agentes de barrera. Solo se admiten para pruebas y validación. 
   
   >Un clúster de Pacemaker usa barreras para devolver el clúster a un estado conocido. La forma de configurar las barreras depende de la distribución y del entorno. En este momento, las barreras no están disponibles en algunos entornos de nube. Consulte [SUSE Linux Enterprise High Availability Extension](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. [Agregue el grupo de disponibilidad como un recurso en el clúster](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server). 

## <a name="prerequisites"></a>Requisitos previos

Para completar el siguiente escenario integral, necesita tres equipos en los que implementar el clúster de tres nodos. En los pasos siguientes se describe cómo configurar estos servidores.

## <a name="setup-and-configure-the-operating-system-on-each-cluster-node"></a>Instalación y configuración del sistema operativo en cada nodo del clúster 

El primer paso es configurar el sistema operativo en los nodos del clúster. A efectos de este tutorial, use SLES 12 SP2 con una suscripción válida para el complemento de alta disponibilidad.

### <a name="install-and-configure-sql-server-service-on-each-cluster-node"></a>Instalación y configuración del servicio SQL Server en cada nodo del clúster

1. Instale y configure el servicio SQL Server en todos los nodos. Para obtener instrucciones detalladas, vea [Instalación de SQL Server en Linux](sql-server-linux-setup.md).

1. Designe un nodo como principal y otros nodos como secundarios. Use estos términos a lo largo de esta guía.

1. Asegúrese de que los nodos que van a formar parte del clúster pueden comunicarse entre sí.

   En el ejemplo siguiente se muestra `/etc/hosts` con adiciones para tres nodos denominados SLES1, SLES2 y SLES3.

   ```
   127.0.0.1   localhost
   10.128.16.33 SLES1
   10.128.16.77 SLES2
   10.128.16.22 SLES3
   ```

   Todos los nodos de clúster deben poder acceder entre sí mediante SSH. Herramientas como `hb_report` o `crm_report` (para la solución de problemas) y History Explorer de Hawk requieren un acceso SSH sin contraseña entre los nodos, ya que, de lo contrario, puede que solo recopilen datos desde el nodo actual. En caso de usar un puerto SSH no estándar, use la opción -X (vea la página `man`). Por ejemplo, si el puerto SSH es 3479, invoque `crm_report` del siguiente modo:

   ```bash
   sudo crm_report -X "-p 3479" [...]
   ```

   Para obtener más información, consulte la [sección Miscelánea de la Guía de administración de SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.troubleshooting.misc).


## <a name="create-a-sql-server-login-for-pacemaker"></a>Crear un inicio de sesión de SQL Server para Pacemaker

[!INCLUDE [SLES-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="configure-an-always-on-availability-group"></a>Configuración de un grupo de disponibilidad Always On

En los servidores Linux, configure el grupo de disponibilidad y, después, configure los recursos de clúster. Para configurar el grupo de disponibilidad, vea [Configuración de un grupo de disponibilidad Always On para SQL Server en Linux](sql-server-linux-availability-group-configure-ha.md).

## <a name="install-and-configure-pacemaker-on-each-cluster-node"></a>Instalación y configuración de Pacemaker en todos los nodos del clúster

1. Instale la extensión High Availability.

   Como referencia, consulte [Instalación de SUSE Linux Enterprise Server y la extensión High Availability](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.installation).

1. Instale el paquete del agente de recursos de SQL Server en ambos nodos.

   ```bash
   sudo zypper install mssql-server-ha
   ```

## <a name="set-up-the-first-node"></a>Configuración del primer nodo

   Como referencia, vea las [instrucciones de instalación de SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/install-quick/install-quick.html#sec.ha.inst.quick.setup.1st-node).

1. Inicie sesión como `root` en la máquina física o virtual que desee usar como nodo de clúster.
2. Para iniciar el script de arranque, ejecute:
   ```bash
   sudo ha-cluster-init
   ```

   Si NTP no se ha configurado para que se inicie durante el arranque, aparecerá un mensaje. 

   Si decide continuar de todas formas, el script generará automáticamente las claves para el acceso SSH y para la herramienta de sincronización Csync2, e iniciará los servicios necesarios para ambos. 

3. Para configurar la capa de comunicación del clúster (Corosync): 

   a. Especifique una dirección de red a la que realizar el enlace. De forma predeterminada, el script propone la dirección de red de eth0. Si lo prefiere, escriba otra dirección de red, como, por ejemplo, la dirección de bond0. 

   b. Especifique una dirección de multidifusión. El script propone una dirección aleatoria que se puede usar como predeterminada. 

   c. Especifique un puerto multidifusión. De forma predeterminada, el script propone 5405. 

   d. Para configurar `SBD ()`, especifique una ruta de acceso persistente a la partición del dispositivo de bloque que desea usar para SBD. La ruta de acceso debe ser coherente en todos los nodos del clúster. 
   Por último, el script iniciará el servicio Pacemaker para poner en línea el clúster de un nodo y habilitar la interfaz de administración web Hawk2. La dirección URL que se va a usar para Hawk2 se muestra en la pantalla. 

4. Para obtener detalles sobre el proceso de instalación, vea `/var/log/sleha-bootstrap.log`. Ahora tiene un clúster de un nodo en ejecución. Compruebe el estado del clúster con crm status:

   ```bash
   sudo crm status
   ```

   También puede ver la configuración de clúster con `crm configure show xml` o `crm configure show`.

5. El procedimiento de arranque crea un usuario de Linux denominado hacluster con la contraseña linux. En cuanto pueda, reemplace la contraseña predeterminada por una segura: 

   ```bash
   sudo passwd hacluster
   ```

## <a name="add-nodes-to-the-existing-cluster"></a>Adición de nodos al clúster existente

Si tiene un clúster que se ejecuta con uno o más nodos, agregue más nodos de clúster con el script de arranque ha-cluster-join. El script solo necesita acceso a un nodo de clúster existente y completará automáticamente la configuración básica en el equipo actual. Siga estos pasos:

Si ha configurado los nodos de clúster existentes con el módulo de clúster `YaST`, asegúrese de que se cumplen los siguientes requisitos previos antes de ejecutar `ha-cluster-join`:
- El usuario raíz en los nodos existentes tiene claves SSH para el inicio de sesión sin contraseña. 
- `Csync2` está configurado en los nodos existentes. Para obtener más información, vea Configuración de Csync2 con YaST. 

1. Inicie sesión como raíz en la máquina física o virtual que debe unirse al clúster. 
2. Para iniciar el script de arranque, ejecute: 

   ```bash
   sudo ha-cluster-join
   ```

   Si NTP no se ha configurado para que se inicie durante el arranque, aparecerá un mensaje. 

3. Si decide continuar de todos modos, se le pedirá que facilite la dirección IP de un nodo existente. Especifique la dirección IP. 

4. Si aún no ha configurado un acceso SSH sin contraseña entre ambos equipos, también se le pedirá la contraseña raíz del nodo existente. 

   Después de iniciar sesión en el nodo especificado, el script copia la configuración de Corosync, configura SSH y `Csync2`, y conecta en línea el equipo actual como nuevo nodo de clúster. Además de eso, inicia el servicio necesario para Hawk. Si ha configurado el almacenamiento compartido con `OCFS2`, también creará automáticamente el directorio mountpoint para el sistema de archivos `OCFS2`. 

5. Repita los pasos anteriores para todas las máquinas que desee agregar al clúster. 

6. Para obtener más información sobre el proceso, vea `/var/log/ha-cluster-bootstrap.log`. 

1. Compruebe el estado del clúster con `sudo crm status`. Si ha agregado el segundo nodo correctamente, el resultado debería parecerse al siguiente:

   ```bash
   sudo crm status
   
   3 nodes configured
   1 resource configured
   Online: [ SLES1 SLES2 SLES3]
   Full list of resources:   
   admin_addr     (ocf::heartbeat:IPaddr2):       Started node1
   ```

   >[!NOTE]
   >`admin_addr` es el recurso de clúster de IP virtual que se configura durante la configuración inicial del clúster de un nodo.

Después de agregar todos los nodos, compruebe si necesita ajustar no-quorum-policy en las opciones globales del clúster. Esto es especialmente importante para los clústeres de dos nodos. Para obtener más información, vea la sección 4.1.2, Opción no-quorum-policy. 

## <a name="set-cluster-property-cluster-recheck-interval"></a>Establecer la propiedad del clúster cluster-recheck-interval

`cluster-recheck-interval` indica el intervalo de sondeo por el que el clúster comprueba los cambios en los parámetros de recursos, las restricciones u otras opciones del clúster. Si una réplica se interrumpe, el clúster intenta reiniciarla en un intervalo que depende de los valores `failure-timeout` y `cluster-recheck-interval`. Por ejemplo, si `failure-timeout` se establece en 60 segundos y `cluster-recheck-interval` se establece en 120 segundos, el reinicio se intenta en un intervalo mayor de 60 segundos pero menor de 120 segundos. Se recomienda establecer el valor de failure-timeout en 60 segundos y el de cluster-recheck-interval en un valor superior a 60 segundos. No se recomienda establecer cluster-recheck-interval en un valor menor.

Para actualizar el valor de la propiedad a `2 minutes`, ejecute:

```bash
crm configure property cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Si ya dispone de un recurso de grupo de disponibilidad administrado por un clúster de Pacemaker, tenga en cuenta que todas las distribuciones que usan el paquete más reciente de Pacemaker (1.1.18 -11.el7) introducen un cambio de comportamiento para el valor de clúster start-failure-is-fatal cuando su valor es false. Este cambio afecta al flujo de trabajo de conmutación por error. Si una réplica principal deja de funcionar, se espera que el clúster conmute por error a una de las réplicas secundarias disponibles. En su lugar, los usuarios observan que el clúster sigue intentando iniciar la réplica principal que ha experimentado el error. Si esa réplica principal no vuelve a activarse (debido a un corte de luz permanente), el clúster nunca conmuta por error a otra réplica secundaria disponible. Debido a este cambio, la recomendación de configurar start-failure-is-fatal ya no es válida y es necesario revertir a su valor predeterminado de `true`. Además, el recurso de grupo de disponibilidad debe actualizarse para incluir la propiedad `failover-timeout`. 
>
>Para actualizar el valor de la propiedad a `true`, ejecute:
>
>```bash
>crm configure property start-failure-is-fatal=true
>```
>
>Actualice la propiedad `failure-timeout` del recurso del grupo de disponibilidad existente a `60s` (reemplace `ag1` por el nombre de su recurso del grupo de disponibilidad): 
>
>```bash
>crm configure edit ag1
># In the text editor, add `meta failure-timeout=60s` after any `param`s and before any `op`s
>```

Para más información sobre las propiedades de clúster de Pacemaker, vea [Configuración de recursos de clúster](https://www.suse.com/documentation/sle_ha/book_sleha/data/sec_ha_config_crm_resources.html).

## <a name="configure-fencing-stonith"></a>Configuración de barrera (STONITH)
Para admitir la configuración del clúster, los proveedores de clústeres de Pacemaker requieren que se habilite STONITH y que se configure un dispositivo de barrera. Cuando el administrador de recursos del clúster no puede determinar el estado de un nodo o de un recurso de un nodo, se usa la barrera para devolver el clúster a un estado conocido.

Mediante la configuración de un recurso, la barrera de nivel de recursos garantiza principalmente que los datos no se dañan si se produce una interrupción. Por ejemplo, puede usar la barrera de nivel de recurso con DRBD (dispositivo de bloque replicado distribuido) para marcar el disco de un nodo como obsoleto cuando el enlace de comunicaciones deje de funcionar.

La barrera de nivel de nodo garantiza que un nodo no ejecute ningún recurso. Para ello, se restablece el nodo, cuya implementación en Pacemaker se denomina STONITH (que, por sus siglas en inglés, significa “disparar al otro nodo en la cabeza”). Pacemaker admite una gran variedad de dispositivos de barrera, como una fuente de alimentación ininterrumpida o tarjetas de interfaz de administración para servidores.

Para más información, consulte:

- [Clústeres de Pacemaker desde cero](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/)
- [Barreras y STONITH](https://clusterlabs.org/doc/crm_fencing.html)
- [Documentación de la alta disponibilidad de SUSE: barreras y STONITH](https://www.suse.com/documentation/sle_ha/book_sleha/data/cha_ha_fencing.html)

En el momento de la inicialización del clúster, STONITH se deshabilita si no se detecta ninguna configuración. Para habilitarlo posteriormente, se puede ejecutar el comando siguiente:

```bash
sudo crm configure property stonith-enabled=true
```
  
>[!IMPORTANT]
>Solo deshabilite STONITH con fines de prueba. Si tiene previsto usar Pacemaker en un entorno de producción, necesita planear una implementación de STONITH basada en su entorno y mantenerla habilitada. SUSE no proporciona agentes de barrera para ningún entorno de nube (incluido Azure) ni Hyper-V. En consecuencia, el proveedor del clúster no admite la ejecución de clústeres de producción en estos entornos. Se trabaja en una solución para este vacío para incorporarla en futuras versiones.

## <a name="configure-the-cluster-resources-for-sql-server"></a>Configuración de los recursos de clúster para SQL Server

Consulte la [Guía de administración de SLES](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.manual_config).

## <a name="enable-pacemaker"></a>Habilitar Pacemaker

Habilite Pacemaker para que se inicie automáticamente.

Ejecute el comando siguiente en cada nodo del clúster.

```bash
systemctl enable pacemaker
```

### <a name="create-availability-group-resource"></a>Crear un recurso de grupo de disponibilidad

El siguiente comando crea y configura el recurso de grupo de disponibilidad para tres réplicas del grupo de disponibilidad [ag1]. Las operaciones y los tiempos de espera de monitor deben especificarse explícitamente en SLES teniendo en cuenta el hecho de que los tiempos de espera dependen en gran medida de la carga de trabajo y, por tanto, deben ajustarse cuidadosamente para cada implementación.
Ejecute el comando en uno de los nodos del clúster:

1. Ejecute `crm configure` para abrir el símbolo del sistema de crm:

   ```bash
   sudo crm configure 
   ```

1. En el símbolo del sistema de crm, ejecute el comando siguiente para configurar las propiedades del recurso.

   ```bash
   primitive ag_cluster \
      ocf:mssql:ag \
      params ag_name="ag1" \
      meta failure-timeout=60s \
      op start timeout=60s \
      op stop timeout=60s \
      op promote timeout=60s \
      op demote timeout=10s \
      op monitor timeout=60s interval=10s \
      op monitor timeout=60s interval=11s role="Master" \
      op monitor timeout=60s interval=12s role="Slave" \
      op notify timeout=60s
   ms ms-ag_cluster ag_cluster \
      meta master-max="1" master-node-max="1" clone-max="3" \
     clone-node-max="1" notify="true" \
   commit
      ```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

### <a name="create-virtual-ip-resource"></a>Creación de un recurso de dirección IP virtual

Si no creó el recurso de IP virtual cuando ejecutó `ha-cluster-init`, puede hacerlo ahora. El comando siguiente crea un recurso de IP virtual. Reemplace `<**0.0.0.0**>` por una dirección disponible de su red y `<**24**>` por el número de bits de la máscara de subred CIDR. Ejecútelo en un nodo.

```bash
crm configure \
primitive admin_addr \
   ocf:heartbeat:IPaddr2 \
   params ip=<**0.0.0.0**> \
      cidr_netmask=<**24**>
```

### <a name="add-colocation-constraint"></a>Agregar una restricción de ubicación
Casi todas las decisiones de un clúster de Pacemaker, por ejemplo elegir dónde se debe ejecutar un recurso, se toman mediante la comparación de puntuaciones. Las puntuaciones se calculan por recurso, y el administrador del recurso de clúster elige el nodo con la puntuación más alta para un recurso determinado. (Si un nodo tiene una puntuación negativa para un recurso, el recurso no se puede ejecutar en ese nodo). Las decisiones del clúster se pueden manipular mediante restricciones. Las restricciones tienen una puntuación. Si una restricción tiene una puntuación inferior a INFINITY (infinito), solo es una recomendación. Una puntuación de infinito implica obligatoriedad. Queremos asegurarnos de que la parte principal del grupo de disponibilidad y el recurso de IP virtual se ejecutan en el mismo host, por lo que definimos una restricción de ubicación con una puntuación de infinito. 

Para establecer una restricción de ubicación de la IP virtual para que se ejecute en el mismo nodo que el maestro, ejecute el siguiente comando en un nodo:

```bash
crm configure
colocation vip_on_master inf: \
    admin_addr ms-ag_cluster:Master
commit
```

### <a name="add-ordering-constraint"></a>Agregar una restricción de ordenación
La restricción de ubicación tiene una restricción de orden implícita. Mueve el recurso de dirección IP virtual antes de mover el recurso de grupo de disponibilidad. La secuencia de eventos predeterminada es la siguiente: 

1. El usuario emite la migración de recursos al maestro del grupo de disponibilidad desde el nodo 1 al nodo 2.
2. El recurso de dirección IP virtual se detiene en el nodo 1.
3. El recurso de dirección IP virtual se inicia en el nodo 2. En este punto, la dirección IP apunta temporalmente al nodo 2, mientras que el nodo 2 sigue siendo una réplica secundaria previa a la conmutación por error. 
4. El maestro del grupo de disponibilidad en el nodo 1 se degrada.
5. El grupo de disponibilidad en el nodo 2 asciende a maestro. 

Para evitar que la dirección IP apunte temporalmente al nodo con la réplica secundaria previa a la conmutación por error, agregue una restricción de ordenación. Para agregar una restricción de orden, ejecute el siguiente comando en un nodo: 

```bash
sudo crm configure \
   order ag_first inf: ms-ag_cluster:promote admin_addr:start
```


>[!IMPORTANT]
>Después de configurar el clúster y agregar el grupo de disponibilidad como un recurso de clúster, no puede usar Transact-SQL para conmutar por error los recursos del grupo de disponibilidad. Los recursos de clúster de SQL Server en Linux no están tan bien integrados con el sistema operativo como lo están en un clúster de conmutación por error de Windows Server (WSFC). El servicio SQL Server no es consciente de la presencia del clúster. Toda la orquestación se realiza con las herramientas de administración de clústeres. En SLES, use `crm`. 

Realice una conmutación por error manual del grupo de disponibilidad con `crm`. No inicie la conmutación por error con Transact-SQL. Para obtener más información, vea [Conmutación por error](sql-server-linux-availability-group-failover-ha.md#failover).


Para más información, consulte:
- [Administración de recursos de clúster](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#sec.ha.config.crm).   
- [Conceptos de la alta disponibilidad](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.concepts)
- [Referencia rápida de Pacemaker](https://github.com/ClusterLabs/pacemaker/blob/master/doc/pcs-crmsh-quick-ref.md) 

<!---[!INCLUDE [Pacemaker Concepts](..\includes\ss-linux-cluster-pacemaker-concepts.md)]--->

## <a name="next-steps"></a>Pasos siguientes

[Funcionamiento del grupo de disponibilidad de alta disponibilidad](sql-server-linux-availability-group-failover-ha.md)
