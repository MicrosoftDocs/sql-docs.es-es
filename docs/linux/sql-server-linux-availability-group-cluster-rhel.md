---
title: 'RHEL: Configuración del grupo de disponibilidad para SQL Server en Linux'
description: Obtenga información sobre cómo configurar un grupo de disponibilidad al ejecutar Red Hat Enterprise Linux (RHEL) para SQL Server.
ms.custom: seo-lt-2019
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 01/23/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: b7102919-878b-4c08-a8c3-8500b7b42397
ms.openlocfilehash: 2790bbe9eb648a2d599d1b0bad8f514ab72eda50
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338383"
---
# <a name="configure-rhel-cluster-for-sql-server-availability-group"></a>Configuración de clústeres de RHEL para grupos de disponibilidad de SQL Server

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este documento se explica cómo crear un clúster de grupos de disponibilidad de tres nodos para SQL Server en Red Hat Enterprise Linux. Para lograr una alta disponibilidad, un grupo de disponibilidad en Linux necesita tres nodos (vea [High availability and data protection for availability group configurations](sql-server-linux-availability-group-ha.md) [Alta disponibilidad y protección de datos para configuraciones de grupos de disponibilidad]). La capa de agrupación en clústeres se basa en el [complemento de alta disponibilidad](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/pdf/High_Availability_Add-On_Overview/Red_Hat_Enterprise_Linux-6-High_Availability_Add-On_Overview-en-US.pdf) de Red Hat Enterprise Linux (RHEL) sobre [Pacemaker](https://clusterlabs.org/). 

> [!NOTE] 
> El acceso a toda la documentación de Red Hat exige una suscripción válida. 

Para obtener más información sobre la configuración del clúster, las opciones de los agentes de recursos y la administración, vaya a la [documentación de referencia de RHEL](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/index.html).

> [!NOTE] 
> SQL Server no está tan estrechamente integrado con Pacemaker en Linux como lo está con los clústeres de conmutación por error de Windows Server. Una instancia de SQL Server no es consciente del clúster. Pacemaker proporciona orquestación del recurso de clúster. Además, el nombre de red virtual es específico de los clústeres de conmutación por error de Windows Server (no hay equivalente en Pacemaker). Las vistas de administración dinámica (DMV) de los grupos de disponibilidad que consultan información del clúster devuelven filas vacías en los clústeres de Pacemaker. Para crear un cliente de escucha para la reconexión transparente tras una conmutación por error, registre de forma manual el nombre del cliente de escucha en DNS con la dirección IP empleada para crear el recurso de dirección IP virtual. 

En las siguientes secciones se siguen los pasos necesarios para configurar un clúster de Pacemaker y agregar un grupo de disponibilidad como recurso en el clúster de alta disponibilidad.

## <a name="roadmap"></a>Plan de desarrollo

Los pasos necesarios para crear un grupo de disponibilidad en servidores con Linux de alta disponibilidad son distintos de los exigidos en un clúster de conmutación por error de Windows Server. En la lista siguiente, se describen los pasos generales: 

1. [Configure SQL Server en los nodos del clúster](sql-server-linux-setup.md).

2. [Cree el grupo de disponibilidad](sql-server-linux-availability-group-configure-ha.md). 

3. Configure un administrador de recursos de clúster, como Pacemaker. Estas instrucciones se incluyen en este documento.
   
   La manera de configurar un administrador de recursos de clúster depende de la distribución específica de Linux. 

   >[!IMPORTANT]
   >Para alcanzar una alta disponibilidad, los entornos de producción necesitan un agente de barrera (por ejemplo, STONITH). En los ejemplos de esta documentación, no se usan agentes de barrera. Los ejemplos solo se proporcionan con fines de prueba y validación.
   
   >Un clúster de Linux usa barreras para devolver el clúster a un estado conocido. La forma de configurar las barreras depende de la distribución y del entorno. Actualmente, las barreras no están disponibles en algunos entornos de nube. Para más información, vea [Directivas de soporte para clústeres de alta disponibilidad de RHEL: plataformas de virtualización](https://access.redhat.com/articles/29440).

4. [Agregue el grupo de disponibilidad como un recurso en el clúster](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource).  

## <a name="configure-high-availability-for-rhel"></a>Configuración de la alta disponibilidad de RHEL

Para configurar la alta disponibilidad de RHEL, habilite la suscripción de alta disponibilidad y, luego, configure Pacemaker.

### <a name="enable-the-high-availability-subscription-for-rhel"></a>Habilitación de la suscripción de alta disponibilidad de RHEL

Cada nodo del clúster debe tener una suscripción adecuada de RHEL y el complemento de alta disponibilidad. Revise los requisitos de [Cómo instalar paquetes de clúster de alta disponibilidad en Red Hat Enterprise Linux](https://access.redhat.com/solutions/45930). Siga estos pasos para configurar la suscripción y los repositorios:

1. Registre el sistema.

   ```bash
   sudo subscription-manager register
   ```

   Proporcione el nombre de usuario y la contraseña.   

1. Enumere los grupos disponibles para el registro.

   ```bash
   sudo subscription-manager list --available
   ```

   En la lista de grupos disponibles, anote el identificador de grupo de la suscripción de alta disponibilidad.

1. Actualice el script siguiente. Reemplace `<pool id>` por el identificador de grupo de alta disponibilidad del paso anterior. Ejecute el script para asociar la suscripción.

   ```bash
   sudo subscription-manager attach --pool=<pool id>
   ```

1. Habilite el repositorio.

   **RHEL 7**

   ```bash
   sudo subscription-manager repos --enable=rhel-ha-for-rhel-7-server-rpms
   ```

   **RHEL 8**

   ```bash
   sudo subscription-manager repos --enable=rhel-8-for-x86_64-highavailability-rpms
   ```

Para obtener más información, vea [Pacemaker: el clúster de alta disponibilidad de código abierto](https://clusterlabs.org/pacemaker/). 

Una vez configurada la suscripción, realice los pasos siguientes para configurar Pacemaker:

### <a name="configure-pacemaker"></a>Configuración de Pacemaker

Una vez registrada la suscripción, realice los pasos siguientes para configurar Pacemaker:
   
[!INCLUDE [RHEL-Configure-Pacemaker](../includes/ss-linux-cluster-pacemaker-configure-rhel.md)]

Una vez configurado Pacemaker, use `pcs` para interactuar con el clúster. Ejecute todos los comandos en un nodo del clúster. 

## <a name="configure-fencing-stonith"></a>Configuración de barrera (STONITH)

Para admitir la configuración del clúster, los proveedores de clústeres de Pacemaker requieren que se habilite STONITH y que se configure un dispositivo de barrera. STONITH corresponde a "shoot the other node in the head", que en español sería algo como "disparar al otro nodo en la cabeza". Si el administrador de recursos de clúster no puede determinar el estado de un nodo o de un recurso de un nodo, las barreras devuelven al clúster a un estado conocido.

Un dispositivo STONITH proporciona un agente de barrera. En [Configuración de Pacemaker en Red Hat Enterprise Linux en Azure](/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker/#1-create-the-stonith-devices) se proporciona un ejemplo de cómo crear un dispositivo STONITH para este clúster en Azure. Modifique las instrucciones para el entorno.

Mediante la configuración de un recurso, la barrera de nivel de recurso garantiza que los datos no se dañen en caso de interrupción. Por ejemplo, puede usar la barrera de nivel de recurso para marcar el disco de un nodo como obsoleto cuando el vínculo de comunicación se interrumpe. 

La barrera de nivel de nodo garantiza que un nodo no ejecute ningún recurso. Esto se hace mediante el restablecimiento del nodo. Pacemaker admite una gran variedad de dispositivos de barrera. Los ejemplos incluyen un sistema de alimentación ininterrumpida o tarjetas de interfaz de administración para servidores.

Para obtener información sobre STONITH y las barreras, vea los siguientes artículos:

* [Clústeres de Pacemaker desde cero](https://clusterlabs.org/pacemaker/doc/en-US/Pacemaker/1.1/html/Clusters_from_Scratch/index.html)
* [Barreras y STONITH](https://clusterlabs.org/doc/crm_fencing.html)
* [Complemento de alta disponibilidad de Red Hat con Pacemaker: barreras](https://access.redhat.com/documentation/Red_Hat_Enterprise_Linux/6/html/Configuring_the_Red_Hat_High_Availability_Add-On_with_Pacemaker/ch-fencing-HAAR.html)

>[!NOTE]
>Dado que la configuración de la barrera de nivel de nodo depende en gran medida del entorno, deshabilítela para este tutorial (se puede configurar más adelante). El siguiente script deshabilita la barrera de nivel de nodo:
>
>```bash
>sudo pcs property set stonith-enabled=false
>``` 
>
>Solo deshabilite STONITH con fines de prueba. Si tiene previsto usar Pacemaker en un entorno de producción, necesita planear una implementación de STONITH basada en su entorno y mantenerla habilitada.

## <a name="set-cluster-property-cluster-recheck-interval"></a>Establecer la propiedad del clúster cluster-recheck-interval

`cluster-recheck-interval` indica el intervalo de sondeo por el que el clúster comprueba los cambios en los parámetros de recursos, las restricciones u otras opciones del clúster. Si una réplica se interrumpe, el clúster intenta reiniciarla en un intervalo que depende de los valores `failure-timeout` y `cluster-recheck-interval`. Por ejemplo, si `failure-timeout` se establece en 60 segundos y `cluster-recheck-interval` se establece en 120 segundos, el reinicio se intenta en un intervalo mayor de 60 segundos pero menor de 120 segundos. Se recomienda establecer el valor de failure-timeout en 60 segundos y el de cluster-recheck-interval en un valor superior a 60 segundos. No se recomienda establecer cluster-recheck-interval en un valor menor.

Para actualizar el valor de la propiedad a `2 minutes`, ejecute:

```bash
sudo pcs property set cluster-recheck-interval=2min
```

> [!IMPORTANT] 
> Todas las distribuciones (incluidas RHEL 7.3 y 7.4) que usan el último paquete disponible de Pacemaker (1.1.18-11.el7) incorporan un cambio de comportamiento para el valor de clúster start-failure-is-fatal cuando es false. Este cambio afecta al flujo de trabajo de conmutación por error. Si una réplica principal deja de funcionar, se espera que el clúster conmute por error a una de las réplicas secundarias disponibles. En su lugar, los usuarios observan que el clúster sigue intentando iniciar la réplica principal que ha experimentado el error. Si esa réplica principal no vuelve a activarse (debido a un corte de luz permanente), el clúster nunca conmuta por error a otra réplica secundaria disponible. Debido a este cambio, la recomendación de configurar start-failure-is-fatal ya no es válida y es necesario revertir a su valor predeterminado de `true`. Además, el recurso de grupo de disponibilidad debe actualizarse para incluir la propiedad `failover-timeout`. 

Para actualizar el valor de la propiedad a `true`, ejecute:

```bash
sudo pcs property set start-failure-is-fatal=true
```

Para actualizar la propiedad `failure-timeout` del recurso `ag_cluster` a `60s`, ejecute:

```bash
pcs resource update ag_cluster meta failure-timeout=60s
```


Para obtener información sobre las propiedades de clúster de Pacemaker, vea [Propiedades de clúster de Pacemaker](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/High_Availability_Add-On_Reference/ch-clusteropts-HAAR.html).

## <a name="create-a-sql-server-login-for-pacemaker"></a>Crear un inicio de sesión de SQL Server para Pacemaker

[!INCLUDE [SQL-Create-SQL-Login](../includes/ss-linux-cluster-pacemaker-create-login.md)]

## <a name="create-availability-group-resource"></a>Crear un recurso de grupo de disponibilidad

Para crear el recurso de grupo de disponibilidad, use el comando `pcs resource create` y establezca las propiedades del recurso. El comando siguiente crea un recurso `ocf:mssql:ag` de tipo principal/secundario para el grupo de disponibilidad con el nombre `ag1`.

**RHEL 7**

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s master notify=true
```

**RHEL 8**

Con la disponibilidad de **RHEL 8**, la sintaxis de Create ha cambiado. Si usa **RHEL 8**, la terminología `master` ha cambiado a `promotable`. Use el comando CREATE siguiente en lugar del comando anterior: 

```bash
sudo pcs resource create ag_cluster ocf:mssql:ag ag_name=ag1 meta failure-timeout=60s promotable notify=true
```

[!INCLUDE [required-synchronized-secondaries-default](../includes/ss-linux-cluster-required-synchronized-secondaries-default.md)]

<a name="createIP"></a>

## <a name="create-virtual-ip-resource"></a>Creación de un recurso de dirección IP virtual

Para crear el recurso de dirección IP virtual, ejecute el comando siguiente en un nodo. Use una dirección IP estática disponible de la red. Reemplace la dirección IP entre `<10.128.16.240>` por una dirección IP válida.

```bash
sudo pcs resource create virtualip ocf:heartbeat:IPaddr2 ip=<10.128.16.240>
```

No hay ningún nombre de servidor virtual equivalente en Pacemaker. Para usar una cadena de conexión que apunte a un nombre de servidor de cadena en lugar de a una dirección IP, registre la dirección de recurso de dirección IP virtual y el nombre de servidor virtual deseado en DNS. En el caso de las configuraciones de recuperación ante desastres, registre el nombre de servidor virtual que prefiera y la dirección IP con los servidores DNS en el sitio principal y en el sitio de recuperación ante desastres.

## <a name="add-colocation-constraint"></a>Agregar una restricción de ubicación

Casi todas las decisiones de un clúster de Pacemaker, por ejemplo elegir dónde se debe ejecutar un recurso, se toman mediante la comparación de puntuaciones. Las puntuaciones se calculan por recurso. El administrador de recursos de clúster elige el nodo con la puntuación más alta para un recurso determinado. Si un nodo tiene una puntuación negativa para un recurso, el recurso no se puede ejecutar en ese nodo.

En un clúster de Pacemaker, las decisiones del clúster se pueden manipular mediante restricciones. Las restricciones tienen una puntuación. Si una restricción tiene una puntuación inferior a `INFINITY`, Pacemaker la considera como una recomendación. Una puntuación de `INFINITY` es obligatoria.

Para garantizar que los recursos de réplica principal y dirección IP virtual se ejecutan en el mismo host, defina una restricción de ubicación con una puntuación de INFINITY. Para agregar la restricción de ubicación, ejecute el comando siguiente en un nodo.

### <a name="rhel-7"></a>RHEL 7

Cuando crea el recurso `ag_cluster` en RHEL 7, se crea el recurso como `ag_cluster-master`. Use el siguiente comando para RHEL 7:

```bash
sudo pcs constraint colocation add virtualip ag_cluster-master INFINITY with-rsc-role=Master
```

### <a name="rhel-8"></a>RHEL 8

Cuando crea el recurso `ag_cluster` en RHEL 8, se crea el recurso como `ag_cluster-clone`. Use el siguiente comando para RHEL 8:

```bash
sudo pcs constraint colocation add virtualip with master ag_cluster-clone INFINITY with-rsc-role=Master
```

## <a name="add-ordering-constraint"></a>Agregar una restricción de ordenación

La restricción de ubicación tiene una restricción de orden implícita. Mueve el recurso de dirección IP virtual antes de mover el recurso de grupo de disponibilidad. La secuencia de eventos predeterminada es la siguiente:

1. El usuario envía `pcs resource move` a la réplica principal del grupo de disponibilidad desde el nodo1 al nodo2.
1. El recurso de dirección IP virtual se detiene en el nodo 1.
1. El recurso de dirección IP virtual se inicia en el nodo 2.
  
   >[!NOTE]
   >En este punto, la dirección IP apunta temporalmente al nodo 2, mientras que el nodo 2 sigue siendo una réplica secundaria previa a la conmutación por error. 
   
1. La réplica principal del grupo de disponibilidad del nodo 1 se degrada a secundario.
1. La réplica secundaria del grupo de disponibilidad del nodo 2 se asciende a principal. 

Para evitar que la dirección IP apunte temporalmente al nodo con la réplica secundaria previa a la conmutación por error, agregue una restricción de ordenación. 

Para agregar una restricción de orden, ejecute el siguiente comando en un nodo:

### <a name="rhel-7"></a>RHEL 7

```bash
sudo pcs constraint order promote ag_cluster-master then start virtualip
```

### <a name="rhel-8"></a>RHEL 8

```bash
sudo pcs constraint order promote ag_cluster-clone then start virtualip
```

>[!IMPORTANT]
>Después de configurar el clúster y agregar el grupo de disponibilidad como un recurso de clúster, no puede usar Transact-SQL para conmutar por error los recursos del grupo de disponibilidad. Los recursos de clúster de SQL Server en Linux no están tan bien integrados con el sistema operativo como lo están en un clúster de conmutación por error de Windows Server (WSFC). El servicio SQL Server no es consciente de la presencia del clúster. Toda la orquestación se realiza con las herramientas de administración de clústeres. En RHEL o Ubuntu, use `pcs`; en SLES, use las herramientas de `crm`. 

Realice una conmutación por error manual del grupo de disponibilidad con `pcs`. No inicie la conmutación por error con Transact-SQL. Para obtener instrucciones, vea [Conmutación por error](sql-server-linux-availability-group-failover-ha.md#failover).

## <a name="next-steps"></a>Pasos siguientes

[Funcionamiento del grupo de disponibilidad de alta disponibilidad](sql-server-linux-availability-group-failover-ha.md)
