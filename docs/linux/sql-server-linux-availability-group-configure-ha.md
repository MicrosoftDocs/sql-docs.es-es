---
title: Configuración del grupo de disponibilidad para SQL Server en Linux
description: Obtenga información sobre cómo crear un grupo de disponibilidad AlwaysOn de SQL Server para alta disponibilidad en Linux.
author: VanMSFT
ms.custom: seo-lt-2019
ms.author: vanto
ms.reviewer: vanto
ms.date: 08/26/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: ''
ms.openlocfilehash: 7313fbc789b37cf326d55566c1f087f80b4929df
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "100351923"
---
# <a name="configure-sql-server-always-on-availability-group-for-high-availability-on-linux"></a>Configuración de un grupo de disponibilidad AlwaysOn de SQL Server para alta disponibilidad en Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

En este artículo se explica cómo crear un grupo de disponibilidad AlwaysOn de SQL Server para alta disponibilidad en Linux. Hay dos tipos de configuración de grupos de disponibilidad. Una configuración de *alta disponibilidad* usa un administrador de clústeres para proporcionar continuidad empresarial. Esta configuración también puede incluir réplicas de escalado de lectura. En este documento se explica cómo crear un grupo de disponibilidad para alta disponibilidad.

También puede crear un grupo de disponibilidad sin un administrador de clústeres para *el escalado de lectura*. El grupo de disponibilidad para escalado de lectura proporciona únicamente réplicas de solo lectura para el escalado horizontal del rendimiento. No proporciona alta disponibilidad. Para crear un grupo de disponibilidad para escalado de lectura, vea [Configuración de un grupo de disponibilidad de SQL Server para escalado de lectura en Linux](sql-server-linux-availability-group-configure-rs.md).

Las configuraciones que garantizan una alta disponibilidad y la protección de los datos requieren dos o tres réplicas de confirmación sincrónicas. Con tres réplicas sincrónicas, el grupo de disponibilidad puede recuperarse automáticamente incluso cuando un servidor no esté disponible. Para más información, vea [Alta disponibilidad y protección de datos para las configuraciones de grupo de disponibilidad](sql-server-linux-availability-group-ha.md). 

Todos los servidores deben ser físicos o virtuales, y los servidores virtuales deben estar en la misma plataforma de virtualización. Este requisito se debe a que los agentes de barrera son específicos de la plataforma. Vea [Directivas de clústeres invitados](https://access.redhat.com/articles/29440#guest_policies).

## <a name="roadmap"></a>Plan de desarrollo

Los pasos necesarios para crear un grupo de disponibilidad en servidores Linux de alta disponibilidad son diferentes a los de un clúster de conmutación por error de Windows Server. En la lista siguiente, se describen los pasos generales: 

1. [Configure SQL Server en tres servidores de clústeres](sql-server-linux-setup.md).

   >[!IMPORTANT]
   >Los tres servidores del grupo de disponibilidad deben estar en la misma plataforma (física o virtual), ya que la alta disponibilidad de Linux usa agentes de barrera para aislar los recursos en los servidores. Los agentes de barrera son específicos de cada plataforma.

2. Cree el grupo de disponibilidad (AG). Este paso se explica en este mismo artículo. 

3. Configure un administrador de recursos de clúster, como Pacemaker.
   
   La manera de configurar un administrador de recursos de clúster depende de la distribución específica de Linux. Vea los siguientes vínculos para obtener instrucciones específicas de cada distribución: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md)
   * [SUSE](sql-server-linux-availability-group-cluster-sles.md)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md)

   >[!IMPORTANT]
   >Para alcanzar una alta disponibilidad, los entornos de producción necesitan un agente de barrera (por ejemplo, STONITH). En los ejemplos de esta documentación, no se usan agentes de barrera. Los ejemplos solo se proporcionan con fines de prueba y validación. 
   
   >Un clúster de Linux usa barreras para devolver el clúster a un estado conocido. La forma de configurar las barreras depende de la distribución y del entorno. Actualmente, las barreras no están disponibles en algunos entornos de nube. Para más información, vea [Directivas de soporte para clústeres de alta disponibilidad de RHEL: plataformas de virtualización](https://access.redhat.com/articles/29440).
   
   >Para más información sobre SLES, vea [Extensión de alta disponibilidad de SUSE Linux Enterprise](https://www.suse.com/documentation/sle-ha-12/singlehtml/book_sleha/book_sleha.html#cha.ha.fencing).

5. Agregue el grupo de disponibilidad como un recurso en el clúster.  

   La forma en que se agregue el grupo de disponibilidad como un recurso en el clúster depende de la distribución de Linux. Vea los siguientes vínculos para obtener instrucciones específicas de cada distribución: 

   * [RHEL](sql-server-linux-availability-group-cluster-rhel.md#create-availability-group-resource)
   * [SLES](sql-server-linux-availability-group-cluster-sles.md#configure-the-cluster-resources-for-sql-server)
   * [Ubuntu](sql-server-linux-availability-group-cluster-ubuntu.md#create-availability-group-resource)

[!INCLUDE [Create Prerequisites](../includes/ss-linux-cluster-availability-group-create-prereq.md)]

## <a name="create-the-ag"></a>Crear el grupo de disponibilidad

En los ejemplos de esta sección se explica cómo crear el grupo de disponibilidad mediante Transact-SQL. También puede usar el Asistente para grupo de disponibilidad de SQL Server Management Studio. Al crear un grupo de disponibilidad con el asistente, aparecerá un error cuando una las réplicas al grupo de disponibilidad. Para corregir esto, conceda los permisos `ALTER`, `CONTROL` y `VIEW DEFINITIONS` a la instancia de Pacemaker en el grupo de disponibilidad en todas las réplicas. Una vez concedidos los permisos en la réplica principal, una los nodos al grupo de disponibilidad mediante el asistente, pero, para que la alta disponibilidad funcione correctamente, conceda los permisos en todas las réplicas.

En una configuración de alta disponibilidad que garantice la conmutación por error automática, el grupo de disponibilidad requiere al menos tres réplicas. Cualquiera de las siguientes configuraciones puede admitir la alta disponibilidad:

- [Tres réplicas sincrónicas](sql-server-linux-availability-group-ha.md#threeSynch)

- [Dos réplicas sincrónicas y una réplica de configuración](sql-server-linux-availability-group-ha.md#twoSynch)

Para más información, vea [Alta disponibilidad y protección de datos para las configuraciones de grupo de disponibilidad](sql-server-linux-availability-group-ha.md).

>[!NOTE]
>Los grupos de disponibilidad pueden incluir más réplicas sincrónicas o asincrónicas. 

Cree el grupo de disponibilidad para alta disponibilidad en Linux. Use [CREATE AVAILABILITY GROUP](../t-sql/statements/create-availability-group-transact-sql.md) con `CLUSTER_TYPE = EXTERNAL`. 

* Grupo de disponibilidad: `CLUSTER_TYPE = EXTERNAL`. Especifica que una entidad de clúster externa administra el grupo de disponibilidad. Pacemaker es un ejemplo de entidad de clúster externa. Cuando el tipo de clúster del grupo de disponibilidad es externo, 

* Establezca las réplicas principal y secundarias en `FAILOVER_MODE = EXTERNAL`. 
   Especifica que la réplica interactúa con un administrador de clústeres externo, como Pacemaker. 

Los siguientes scripts de Transact-SQL crean un grupo de disponibilidad para alta disponibilidad denominado `ag1`. El script configura las réplicas de grupo de disponibilidad con `SEEDING_MODE = AUTOMATIC`. Esta configuración hace que SQL Server cree de manera automática la base de datos en todos los servidores secundarios. Actualice el script siguiente para su entorno. Reemplace los valores `<node1>`, `<node2>` o `<node3>` por los nombres de las instancias de SQL Server que hospedan las réplicas. Reemplace el valor `<5022>` por el puerto que haya definido para el punto de conexión de creación de reflejo de datos. Para crear el grupo de disponibilidad, ejecute el siguiente código de Transact-SQL en la instancia de SQL Server que hospeda la réplica principal.

Ejecute **solo uno** de los siguientes scripts: 

- [Crear un grupo de disponibilidad con tres réplicas sincrónicas](#threeSynch).
- [Crear un grupo de disponibilidad con dos réplicas sincrónicas y una réplica de configuración](#configOnly)
- [Crear un grupo de disponibilidad con dos réplicas sincrónicas](#readScale).

<a name="threeSynch"></a>

- Crear un grupo de disponibilidad con tres réplicas sincrónicas:

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
       WITH (DB_FAILOVER = ON, CLUSTER_TYPE = EXTERNAL)
       FOR REPLICA ON
           N'<node1>' 
            WITH (
               ENDPOINT_URL = N'tcp://<node1>:<5022>',
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node2>' 
            WITH ( 
               ENDPOINT_URL = N'tcp://<node2>:<5022>', 
               AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
               FAILOVER_MODE = EXTERNAL,
               SEEDING_MODE = AUTOMATIC
               ),
           N'<node3>'
           WITH( 
              ENDPOINT_URL = N'tcp://<node3>:<5022>', 
              AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
              FAILOVER_MODE = EXTERNAL,
              SEEDING_MODE = AUTOMATIC
              );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```

   >[!IMPORTANT]
   >Después de ejecutar el script anterior para crear un grupo de disponibilidad con tres réplicas sincrónicas, no ejecute el script siguiente:

<a name="configOnly"></a>
- Crear un grupo de disponibilidad con dos réplicas sincrónicas y una réplica de configuración:

   >[!IMPORTANT]
   >Esta arquitectura permite que cualquier edición de SQL Server hospede la tercera réplica. Por ejemplo, la tercera réplica se puede hospedar en SQL Server Express Edition. En Express Edition, el único tipo de punto de conexión válido es `WITNESS`. 

   ```SQL
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
          ENDPOINT_URL = N'tcp://<node2>:<5022>',  
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
<a name="readScale"></a>

- Crear un grupo de disponibilidad con dos réplicas sincrónicas:

   Incluya dos réplicas con el modo de disponibilidad sincrónica. Por ejemplo, el siguiente script crea un grupo de disponibilidad llamado `ag1`. `node1` y `node2` hospedan réplicas en modo sincrónico, con propagación y conmutación por error automáticas.

   >[!IMPORTANT]
   >Ejecute únicamente el siguiente script para crear un grupo de disponibilidad con dos réplicas sincrónicas. No ejecute el siguiente script si ejecutó el script anterior. 

   ```SQL
   CREATE AVAILABILITY GROUP [ag1]
      WITH (CLUSTER_TYPE = EXTERNAL)
      FOR REPLICA ON
      N'node1' WITH (
         ENDPOINT_URL = N'tcp://node1:5022',
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      ),
      N'node2' WITH ( 
         ENDPOINT_URL = N'tcp://node2:5022', 
         AVAILABILITY_MODE = SYNCHRONOUS_COMMIT,
         FAILOVER_MODE = EXTERNAL,
         SEEDING_MODE = AUTOMATIC
      );
        
   ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
   ```


También puede configurar un grupo de disponibilidad con `CLUSTER_TYPE=EXTERNAL` en SQL Server Management Studio o PowerShell. 

### <a name="join-secondary-replicas-to-the-ag"></a>Unión de réplicas secundarias al grupo de disponibilidad

El usuario de Pacemaker requiere los permisos `ALTER`, `CONTROL` y `VIEW DEFINITION` en el grupo de disponibilidad en todas las réplicas. Para conceder estos permisos, una vez creado el grupo de disponibilidad, ejecute el siguiente script de Transact-SQL en la réplica principal y en cada réplica secundaria inmediatamente después de agregarlas a dicho grupo de disponibilidad. Antes de ejecutar el script, reemplace `<pacemakerLogin>` por el nombre de la cuenta de usuario de Pacemaker. Si no tiene un inicio de sesión de Pacemaker, [cree un inicio de sesión de SQL Server para Pacemaker](sql-server-linux-availability-group-cluster-ubuntu.md#create-a-sql-server-login-for-pacemaker).

```sql
GRANT ALTER, CONTROL, VIEW DEFINITION ON AVAILABILITY GROUP::ag1 TO <pacemakerLogin>
GRANT VIEW SERVER STATE TO <pacemakerLogin>
```

El siguiente script de Transact-SQL une una instancia de SQL Server a un grupo de disponibilidad denominado `ag1`. Actualice el script para su entorno. En cada instancia de SQL Server que hospede una réplica secundaria, ejecute el siguiente script de Transact-SQL para unirla al grupo de disponibilidad.

```sql
ALTER AVAILABILITY GROUP [ag1] JOIN WITH (CLUSTER_TYPE = EXTERNAL);
         
ALTER AVAILABILITY GROUP [ag1] GRANT CREATE ANY DATABASE;
```

[!INCLUDE [Create Post](../includes/ss-linux-cluster-availability-group-create-post.md)]

>[!IMPORTANT]
>Después de crear el grupo de disponibilidad, debe configurar la integración con una tecnología de clúster, como Pacemaker, para la alta disponibilidad. En el caso de una configuración de escalado de lectura con grupos de disponibilidad, a partir de [!INCLUDE [SQL Server version](../includes/sssql17-md.md)] ya no es necesario configurar un clúster.

Si ha seguido los pasos descritos en este documento, tendrá un grupo de disponibilidad que todavía no está agrupado en clúster. El siguiente paso es agregar el clúster. Esta configuración es válida en escenarios de equilibrio de carga y escalado de lectura, pero no está completa para alta disponibilidad. Para lograr la alta disponibilidad, deberá agregar el grupo de disponibilidad como un recurso de clúster. Vea [Pasos siguientes](#next-steps) para obtener instrucciones. 

## <a name="notes"></a>Notas

>[!IMPORTANT]
>Después de configurar el clúster y agregar el grupo de disponibilidad como un recurso de clúster, no puede usar Transact-SQL para conmutar por error los recursos del grupo de disponibilidad. Los recursos de clúster de SQL Server en Linux no están tan bien integrados con el sistema operativo como lo están en un clúster de conmutación por error de Windows Server (WSFC). El servicio SQL Server no es consciente de la presencia del clúster. Toda la orquestación se realiza con las herramientas de administración de clústeres. En RHEL o Ubuntu, use `pcs`. En SLES, use `crm`. 

>[!IMPORTANT]
>Si el grupo de disponibilidad es un recurso de clúster, existe un problema conocido en la versión actual en el que la conmutación por error forzada con pérdida de datos en una réplica asincrónica no funciona. Esto se corregirá en la próxima versión. La conmutación por error manual o automática en una réplica sincrónica sí se realiza correctamente.


## <a name="next-steps"></a>Pasos siguientes

[Configuración de clúster RHEL para el grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-rhel.md)

[Configuración de clústeres de SLES para grupos de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-sles.md)

[Configurar el clúster de Ubuntu para recursos de clúster del grupo de disponibilidad de SQL Server](sql-server-linux-availability-group-cluster-ubuntu.md)