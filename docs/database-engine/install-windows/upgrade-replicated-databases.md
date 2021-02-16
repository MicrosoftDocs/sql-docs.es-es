---
title: Actualización o revisión de bases de datos replicadas | Microsoft Docs
description: SQL Server admite la actualización de bases de datos replicadas desde versiones anteriores de SQL Server sin detener la actividad en otros nodos.
ms.custom: ''
ms.date: 07/24/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
helpviewer_keywords:
- merge replication database upgrades [SQL Server replication]
- replication [SQL Server], upgrading
- transactional replication, upgrading databases
- snapshot replication [SQL Server], upgrading databases
- upgrading replicated databases
ms.assetid: 9926a4f7-bcd8-4b9b-9dcf-5426a5857116
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 72b1d22d14cf83d44743d2081ee051bd02faf342
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351020"
---
# <a name="upgrade-or-patch-replicated-databases"></a>Actualización o revisión de bases de datos replicadas

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]
  
  [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)] admite la actualización de bases de datos replicadas desde versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; no es necesario detener la actividad en otros nodos mientras se actualiza un nodo. Asegúrese de cumplir las reglas relativas a la versión admitida en una topología:  
  
-   Un distribuidor puede ser de cualquier versión siempre que ésta sea mayor o igual que la versión del publicador (en muchos casos el distribuidor es la misma instancia que el publicador).    
-   Un publicador puede ser de cualquier versión siempre que ésta sea menor o igual que la versión del distribuidor.    
-   La versión del suscriptor depende del tipo de publicación:    
    - Un suscriptor de una publicación transaccional puede ser de cualquiera de las dos versiones del publicador. Por ejemplo: un publicador de SQL Server 2012 (11.x) puede tener suscriptores de SQL Server 2014 (12.x) y SQL Server 2016 (13.x); y un publicador de SQL Server 2016 (13.x) puede tener suscriptores de SQL Server 2014 (12.x) y SQL Server 2012 (11.x).     
    - En un suscriptor a una publicación de combinación todas las versiones pueden ser iguales o inferiores a la versión del publicador que se admite según el ciclo de soporte del ciclo de vida de las versiones.  
 
La ruta de actualización a SQL Server es diferente según el modelo de implementación. SQL Server ofrece dos rutas de actualización en general:
- En paralelo: se implementa un entorno en paralelo y se mueven al entorno nuevo las bases de datos junto con los objetos de nivel de instancia asociados, como los inicios de sesión, los trabajos, etc. 
- Actualización local: se permite que los medios de instalación de SQL Server actualicen la instalación de SQL Server existente mediante el reemplazo de los bits de SQL Server y la actualización de los objetos de base de datos. Para los entornos que ejecutan grupos de disponibilidad Always On o instancias de clúster de conmutación por error, una actualización local se combina con una [actualización gradual](choose-a-database-engine-upgrade-method.md#rolling-upgrade) para minimizar el tiempo de inactividad. 

Un enfoque común que se ha adoptado para las actualizaciones en paralelo de las topologías de replicación es mover los pares de publicador y suscriptor por partes al nuevo entorno en paralelo, en lugar de mover toda la topología. Este enfoque por fases ayuda a controlar el tiempo de inactividad y a minimizar, hasta cierto punto, el impacto para la empresa que depende de la replicación.  

La mayor parte de este artículo se centra en la actualización de la versión de SQL Server. Sin embargo, el proceso de actualización en contexto también debe utilizarse al aplicar revisiones en SQL Server con un Service Pack o actualización acumulativa también. 

 >[!WARNING]
 > La actualización de una topología de replicación es un proceso de varios pasos. Se recomienda intentar la actualización de una réplica de la topología de replicación en un entorno de prueba antes de ejecutar la actualización en el entorno de producción real. Esto ayudará a resolver cualquier documentación de las operaciones necesaria para administrar la actualización sin problemas y sin incurrir en tiempos de inactividad costosos y prolongados durante el proceso de actualización real. Hemos visto que los clientes reducen significativamente el tiempo de inactividad con el uso de grupos de disponibilidad Always On o instancias de clúster de conmutación por error de SQL Server para sus entornos de producción durante la actualización de su topología de replicación. Además, se recomienda realizar copias de seguridad de todas las bases de datos incluidas MSDB, la principal, las bases de datos de distribución y las de usuario que participan en la replicación antes de intentar la actualización.


## <a name="replication-matrix"></a>Matriz de replicación

[!INCLUDE[repl matrix](../../includes/replication-compat-matrix.md)]
  
## <a name="run-the-log-reader-agent-for-transactional-replication-before-upgrade"></a>Ejecutar el Agente de registro del LOG para la replicación transaccional antes de la actualización  
 Antes de actualizar a [!INCLUDE[ssNoversion](../../includes/ssnoversion-md.md)], debe asegurarse de que el Agente de registro del LOG ha procesado todas las transacciones confirmadas en las tablas publicadas. Para asegurarse de que se han procesado todas las transacciones, siga estos pasos para cada base de datos que contenga publicaciones transaccionales:  
  
1.  Asegurarse de que el Agente de registro del LOG se está ejecutando para la base de datos. De forma predeterminada, el agente se ejecuta sin interrupción.    
2.  Detenga la actividad de usuario en las tablas publicadas.  
3.  Deje tiempo para que el Agente de registro del LOG copie las transacciones en la base de datos de distribución y, a continuación, detenga el agente.  
4.  Ejecute [sp_replcmds](../../relational-databases/system-stored-procedures/sp-replcmds-transact-sql.md) para comprobar que se han procesado todas las transacciones. El conjunto de resultados de este procedimiento debe estar vacío.   
5.  Ejecute [sp_replflush](../../relational-databases/system-stored-procedures/sp-replflush-transact-sql.md) para cerrar la conexión de sp_replcmds.   
6.  Actualice el servidor a la versión más reciente de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].   
7.  Reinicie el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente de registro del LOG si no se inician automáticamente después de la actualización.  
  
## <a name="run-agents-for-merge-replication-after-upgrade"></a>Ejecutar agentes para la replicación de mezcla después de la actualización  
 Después de la actualización, ejecute el Agente de instantáneas de cada publicación de combinación y el Agente de mezcla de cada suscripción para actualizar los metadatos de la replicación. No tiene que aplicar la nueva instantánea porque no es necesaria para reinicializar las suscripciones. Los metadatos de suscripción se actualizan la primera vez que el Agente de mezcla se ejecuta tras la actualización. Esto significa que la base de datos de suscripciones puede permanecer en línea y activa durante la actualización del publicador.  
  
 La replicación de mezcla almacena metadatos de publicación y suscripción en un determinado número de tablas del sistema en las bases de datos de publicaciones y suscripciones. La ejecución del Agente de instantáneas actualiza los metadatos de publicación y la ejecución del Agente de mezcla actualiza los metadatos de suscripción. Solo es necesaria para generar una instantánea de publicación. Si una publicación de combinación utiliza filtros con parámetros, cada partición también tendrá una instantánea. No es necesario actualizar estas instantáneas con particiones.  
  
 Ejecute los agentes desde [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], el Monitor de replicación o la línea de comandos. Para más información sobre la ejecución del Agente de instantáneas, vea estos artículos:  
  
-   [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Iniciar y detener un agente de replicación &#40;SQL Server Management Studio&#41;](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md)
-   [Crear y aplicar la instantánea inicial](../../relational-databases/replication/create-and-apply-the-initial-snapshot.md)
-   [Conceptos de los ejecutables del Agente de replicación](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)  
  

Para más información sobre la ejecución del Agente de mezcla, vea estos artículos:
-   [Sincronizar una suscripción de extracción](../../relational-databases/replication/synchronize-a-pull-subscription.md)
-   [Sincronizar una suscripción de inserción](../../relational-databases/replication/synchronize-a-push-subscription.md)  
  

Después de actualizar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una topología que utiliza la replicación de mezcla, cambie el nivel de compatibilidad de publicación de todas las publicaciones si desea utilizar nuevas características.  
  
## <a name="upgrading-to-standard-workgroup-or-express-editions"></a>Actualizar a Standard Edition, Workgroup Edition o Express Edition  
Antes de actualizar desde una edición de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] a otra, compruebe que las funciones que actualmente usa son compatibles con la edición a la que quiere actualizar. Para más información, vea la sección sobre replicación de [Ediciones y características admitidas de SQL Server](../../sql-server/editions-and-components-of-sql-server-2017.md).  

## <a name="steps-to-upgrade-a-replication-topology"></a>Pasos para actualizar una topología de replicación
En estos pasos se describe el orden en que se deben actualizar los servidores de una topología de replicación. Se aplican los mismos pasos con independencia de que se ejecute la replicación transaccional o de mezcla. Pero en estos pasos no se describe la replicación punto a punto, las suscripciones de actualización en cola ni las suscripciones de actualización inmediata. 

### <a name="in-place-upgrade"></a>Actualización local 
1. Actualice el distribuidor. 
2. Actualice el publicador y el suscriptor. Se pueden actualizar en cualquier orden. 

 >[!NOTE]
 > Para SQL 2008 y 2008 R2, la actualización del publicador y suscriptor se debe realizar al mismo tiempo para alinearse con la matriz de la topología de replicación. Un publicador o suscriptor de SQL 2008 o 2008 R2 no puede tener un publicador o suscriptor de SQL 2016 (o superior). Si no es posible actualizar al mismo tiempo, use una actualización intermedia para actualizar las instancias de SQL a SQL 2014 y, después, vuelva a actualizarlas a SQL 2016 (o una versión superior).  

### <a name="side-by-side-upgrade"></a>Actualización en paralelo
1. Actualice el distribuidor.
1. Vuelva a configurar la [Distribución](../../relational-databases/replication/configure-distribution.md) en la nueva instancia de SQL Server.
1. Actualice el publicador.
1. Actualice el suscriptor.
1. Vuelva a configurar todos los pares de publicador y suscriptor, incluyendo la reinicialización del suscriptor. 


## <a name="steps-for-side-by-side-migration-of-the-distributor-to-windows-server-2012-r2"></a>Pasos para la migración en paralelo del distribuidor a Windows Server 2012 R2
Si está planeando la actualización de la instancia de SQL Server a SQL Server 2016 (o una versión superior) y el sistema operativo actual es Windows 2008 (o 2008 R2), tendrá que realizar una actualización en paralelo del sistema operativo a Windows Server R2 o superior. El motivo de esta actualización intermedia del sistema operativo es que no se puede instalar SQL Server 2016 en una instancia Windows Server 2008/2008 R2, y Windows Server 2008/20008 R2 no admite las actualizaciones locales directamente a Windows Server 2016. Aunque es posible realizar una actualización local de Windows Server 2008/2008 R2 a Windows Server 2012 y, luego, a Windows Server 2016, por lo general no se recomienda hacerlo debido al tiempo de inactividad y a la mayor complejidad, lo que impide una ruta de reversión sencilla. Una actualización en paralelo es la única ruta de actualización disponible para las instancias de SQL Server que participan en un clúster de conmutación por error.  Los pasos siguientes se pueden realizar en una instancia de SQL Server independiente, o bien en una instancia de clúster de conmutación por error Always On (FCI).

1. Configure una instancia nueva de SQL Server (independiente o de clúster de conmutación por error Always On), con la misma edición y versión que el distribuidor en Windows Server 2012 R2/2016 con otro clúster de Windows y el nombre FCI de SQL Server o el nombre de host independiente. Tendrá que mantener la misma estructura de directorios del distribuidor anterior para asegurarse de que los archivos ejecutables de los agentes de replicación, las carpetas de replicación y las rutas de acceso de los archivos de base de datos se encuentran en la misma ruta de acceso en el entorno nuevo. Esto reducirá los pasos necesarios posteriores a la migración o actualización.
1. Asegúrese de que la replicación está sincronizada y, después, cierre todos los agentes de replicación. 
1. Apague la instancia actual del distribuidor de SQL Server. Si se trata de una instancia independiente, apague el servidor. Si se trata de una FCI de SQL, desconecte el rol de SQL Server completo en el Administrador de clústeres, incluido el nombre de red. 
1. Quite las entradas de objeto de equipo DNS y AD para el entorno antiguo (la instancia de distribuidor actual). 
1. Cambie el nombre de host del servidor nuevo para que coincida con el del servidor antiguo.
    1. Si se trata de una FCI de SQL, cambie el nombre de la nueva FCI de SQL por el mismo nombre de servidor virtual de la instancia anterior. 
1. Copie los archivos de base de datos de la instancia anterior mediante la redirección de SAN, copia de almacenamiento o copia de archivos. 
1. Conecte la nueva instancia de SQL Server. 
1. Reinicie todos los agentes de replicación y compruebe si los agentes se ejecutan correctamente.
1. Compruebe que la replicación funciona de la manera esperada. 
1. Use los medios de instalación de SQL Server para ejecutar una actualización local de la instancia de SQL Server a la versión nueva de SQL Server. 


  >[!NOTE]
  > Para reducir el tiempo de inactividad, se recomienda realizar la *migración en paralelo* del distribuidor como una actividad y la *actualización local a SQL Server 2016* como otra. Esto permitirá adoptar un enfoque por fases, reducir el riesgo y minimizar el tiempo de inactividad.

## <a name="web-synchronization-for-merge-replication"></a>Sincronización web para la replicación de mezcla  
 La opción de sincronización web para replicación de mezcla requiere que se copie el archivo Escucha de replicación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (replisapi.dll) en el directorio virtual del servidor de Internet Information Services (IIS) que se usa para la sincronización. Cuando se configura la sincronización web, se copia el archivo en el directorio virtual mediante el Asistente para configurar la sincronización web. Si se actualizan los componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instalados en el servidor IIS, debe copiarse manualmente el archivo replisapi.dll del directorio COM en el directorio virtual del servidor IIS. Para obtener más información sobre cómo configurar la sincronización web, vea [Configurar la sincronización web](../../relational-databases/replication/configure-web-synchronization.md).  
  
## <a name="restoring-a-replicated-database-from-an-earlier-version"></a>Restaurar una base de datos replicada a partir de una versión anterior  
 Para asegurarse de que la configuración de replicación se conserva al restaurar una copia de seguridad de una base de datos replicada a partir de una versión anterior, restaure en un servidor y una base de datos con los mismos nombres que el servidor y la base de datos donde se realizó la copia de seguridad.  
  
## <a name="see-also"></a>Consulte también  
 [Replicación de SQL Server](../../relational-databases/replication/sql-server-replication.md)  
 [Preguntas más frecuentes para administradores de replicación](../../relational-databases/replication/administration/frequently-asked-questions-for-replication-administrators.md)   
 [Compatibilidad con versiones anteriores de replicación](../../relational-databases/replication/replication-backward-compatibility.md)   
 [Actualizaciones de ediciones y versiones admitidas](../../database-engine/install-windows/supported-version-and-edition-upgrades.md)   
 [Actualizar SQL Server](../../database-engine/install-windows/upgrade-sql-server.md)  
 [Upgrading a Replication Topology to SQL Server 2016](/archive/blogs/sql_server_team/upgrading-a-replication-topology-to-sql-server-2016) (Actualización de una topología de replicación a SQL Server 2016)