---
title: Instancias de clúster de conmutación por error de AlwaysOn
description: Obtenga información sobre cómo las instancias de clúster de conmutación por error de AlwaysOn proporcionan alta disponibilidad local mediante redundancia en el nivel de instancia del servidor en SQL Server.
ms.custom: seo-lt-2019
ms.date: 01/18/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- high availability [SQL Server], failover clustering
- virtual servers [SQL Server], about virtual servers
- clusters [SQL Server]
- servers [SQL Server], failover clustering
- resource groups [SQL Server]
- availability [SQL Server]
- failover clustering [SQL Server]
- AlwaysOn [SQL Server], see failover clustering [SQL Server]
ms.assetid: 86a15b33-4d03-4549-8ea2-b45e4f1baad7
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2f87ae98424eef3f6aeadca1f11da6d9d1f2b403
ms.sourcegitcommit: 8bdb5a51f87a6ff3b94360555973ca0cd0b6223f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2021
ms.locfileid: "100549391"
---
# <a name="always-on-failover-cluster-instances-sql-server"></a>Instancias de clúster de conmutación por error de AlwaysOn (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Como parte de la oferta de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On, las instancias de clúster de conmutación por error de Always On aprovechan la funcionalidad de Clústeres de conmutación por error de Windows Server (WSFC) para proporcionar alta disponibilidad local mediante la redundancia en el nivel de instancias de servidor, una *instancia de clúster de conmutación por error* (FCI). Una FCI es una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se instala a través de los nodos de Clústeres de conmutación por error de Windows Server (WSFC) y, posiblemente, a través de varias subredes. En la red, una FCI aparece como una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que se ejecuta en un equipo individual, pero proporciona la conmutación por error entre nodos de WSFC si el nodo actual deja de estar disponible.  
  
 Una FCI puede aprovechar los [Grupos de disponibilidad](../../../database-engine/availability-groups/windows/always-on-availability-groups-sql-server.md) para proporcionar una recuperación ante desastres remota en el nivel de base de datos. Para obtener más información, consulte [Clústeres de conmutación por error y grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md).  
 
 > [!NOTE]  
 > La edición Windows Server 2016 Datacenter introduce compatibilidad con Espacios de almacenamiento directo (S2D). Las instancias de clúster de conmutación por error de SQL Server admiten S2D para recursos de almacenamiento de clúster. Para obtener más información, consulte [Espacios de almacenamiento directo en Windows Server 2016](/windows-server/storage/storage-spaces/storage-spaces-direct-overview).
 > 
 >Las instancias de clúster de conmutación por error también admiten volúmenes compartidos en clúster (CSV). Para obtener más información, vea [Descripción de Volúmenes compartidos de clúster en un clúster de conmutación por error](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd759255(v=ws.11)). 
   
 **En este tema:**  
  
-   [Ventajas](#Benefits)  
  
-   [Recomendaciones](#Recommendations)  
  
-   [Información general de las instancias de clúster de conmutación por error](#Overview)  
  
-   [Elementos de una instancia de clúster de conmutación por error](#FCIelements)  
  
-   [Conceptos y tareas de conmutación por error de SQL Server](#ConceptsAndTasks)  
  
-   [Temas relacionados](#RelatedTopics)  
  
##  <a name="benefits-of-a-failover-cluster-instance"></a><a name="Benefits"></a> Ventajas de una instancia de clústeres de conmutación por error  
 Cuando hay un error de hardware o software de un servidor, las aplicaciones o los clientes que se conecten al servidor experimentarán tiempo de inactividad. Cuando una instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se configura para ser una FCI (en lugar de una instancia independiente), la alta disponibilidad de esa instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] está protegida por la presencia de nodos redundantes en la FCI. Solo uno de los nodos de la FCI pertenece al grupo de recursos de WSFC cada vez. En caso de se produzca un error (errores de hardware, errores del sistema operativo o errores de aplicación o servicio) o se realice una actualización planeada, la propiedad del grupo de recursos se mueve a otro nodo de WSFC. Este proceso es transparente para el cliente o aplicación que se conecta a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y minimiza el tiempo de inactividad que la aplicación o los clientes experimentan durante un error. A continuación se enumeran algunas ventajas clave que las instancias de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] proporcionan:  
  
-   Protección a través de la redundancia en el nivel de instancia.  
  
-   Conmutación automática por error en caso de error (errores de hardware, errores del sistema operativo, errores de la aplicación o de servicio)  
  
    > [!IMPORTANT]  
    >  En un grupo de disponibilidad, no se admite la conmutación automática por error de una FCI a otros nodos del grupo de disponibilidad. Esto significa que las FCI y los nodos independientes no deben acoplarse juntos en un grupo de disponibilidad si la conmutación automática por error es un componente importante de la solución de alta disponibilidad. Sin embargo, este acoplamiento se puede realizar para la solución de *recuperación ante desastres* .  
  
-   Compatibilidad con una amplia matriz de soluciones de almacenamiento, incluidos discos de clúster de WSFC (iSCSI, canal de fibra óptica, etc.) y recursos compartidos de archivos de Bloque de mensajes de servidor (SMB).  
  
-   Solución de recuperación ante desastres que usa una FCI de múltiples subredes o que ejecuta una base de datos hospedada por FCI en un grupo de disponibilidad. Con la nueva utilidad de múltiples subredes en [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], una FCI de múltiples subredes ya no necesita una LAN virtual, con lo que aumentan su capacidad de administración y su seguridad.  
  
-   Reconfiguración cero de aplicaciones y clientes durante las conmutaciones por error  
  
-   Directiva de conmutación por error flexible para eventos de desencadenador específicos en conmutaciones automáticas por error  
  
-   Conmutaciones por error confiables a través de la detección periódica y detallada del estado mediante el uso de conexiones dedicadas y persistentes  
  
-   Facilidad de configuración y predicción en el momento de la conmutación por error mediante puntos de comprobación indirectos de antecedentes  
  
-   Uso acelerado de recursos durante las conmutaciones por error  
  
##  <a name="recommendations"></a><a name="Recommendations"></a> Recomendaciones  
 En un entorno de producción, recomendamos que use direcciones IP estáticas junto con la dirección IP virtual de una instancia de clúster de conmutación por error.  Recomendamos no usar DHCP en un entorno de producción. En caso de tiempo de inactividad, si expira el tiempo de concesión de la dirección de IP de DHCP, se necesitará un tiempo adicional para volver a registrar la nueva dirección IP de DHCP asociada al nombre DNS.  
  
##  <a name="failover-cluster-instance-overview"></a><a name="Overview"></a> Información general de las instancias de clúster de conmutación por error  
 Una FCI se ejecuta en un grupo de recursos de WSFC con uno o más nodos de WSFC. Cuando la FCI se inicia, uno de los nodos asume la propiedad del grupo de recursos y pone en línea la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Entre los recursos que pertenecen a este nodo se incluyen:  
  
-   Nombre de red  
  
-   Dirección IP  
  
-   Discos compartidos  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Servicio Motor de base de datos  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Servicio del Agente  
  
-   [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Analysis Services, si está instalado  
  
-   Un recurso compartido de archivos, si está instalada la característica FILESTREAM  
  
 En cualquier momento, solo el propietario del grupo de recursos (y ningún otro nodo de la FCI) ejecuta sus servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectivos en el grupo de recursos. Cuando se produce una conmutación por error, ya sea automática o planeada, tiene lugar la siguiente secuencia de eventos:  
  
1.  A menos que se produzca un error hardware o del sistema, todas las páginas desfasadas de la memoria caché del búfer se escriben en el disco.  
  
2.  Todos los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] respectivos del grupo de recursos se detienen en el nodo activo.  
  
3.  La propiedad del grupo de recursos se transfiere a otro nodo de la FCI.  
  
4.  El nuevo propietario del grupo de recursos inicia los servicios de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
5.  Las solicitudes de conexión de la aplicación cliente se dirigen automáticamente al nuevo nodo activo utilizando el mismo nombre de red virtual (VNN).  
  
 La FCI está en línea mientras el estado del quórum del clúster de WSFC subyacente sea bueno (la mayoría de los nodos de WSFC de quórum están disponibles como destinos de conmutación automática por error). Si el clúster de WSFC pierde su quórum, a causa de un error de hardware, de software o de red, o de una configuración de quórum incorrecta, todo el clúster de WSFC, junto con la FCI, se ponen en estado sin conexión. En este escenario de conmutación por error no planeada se requiere la intervención manual para restablecer el quórum en los nodos disponibles restantes con el fin de volver a poner en línea el clúster de WSFC y la FCI. Para obtener más información, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
### <a name="predictable-failover-time"></a>Tiempo de conmutación por error previsible  
 Dependiendo de cuándo la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] haya realizado por última vez una operación de punto de comprobación, puede haber una cantidad considerable de páginas desfasadas en la memoria caché del búfer. Por tanto, las conmutaciones por error duran el tiempo que lleve escribir las páginas desfasadas restantes en el disco, lo que puede dar lugar a un tiempo de conmutación por error prolongado e imprevisible. A partir de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], la FCI puede usar puntos de comprobación indirectos para limitar la cantidad de páginas desfasadas que se mantienen en la memoria caché del búfer. Cuando este proceso utiliza recursos adicionales en una carga de trabajo normal, el tiempo de conmutación por error se hace más fácil de predecir y de configurar. Esto es muy útil cuando el acuerdo de servicio en su organización especifica el objetivo de tiempo de recuperación (RTO) para su solución de alta disponibilidad. Para obtener más información sobre puntos de comprobación indirectos, vea [Indirect Checkpoints](../../../relational-databases/logs/database-checkpoints-sql-server.md#IndirectChkpt).  
  
### <a name="reliable-health-monitoring-and-flexible-failover-policy"></a>Supervisión de estado confiable y directiva de conmutación por error flexible  
 Después de que la FCI se inicie correctamente, el servicio de WSFC supervisa el estado del clúster de WSFC subyacente y el estado de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . A partir de [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)], el servicio de WSFC utiliza una conexión dedicada para sondear la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] activa a efectos de diagnósticos de componentes detallados mediante un procedimiento almacenado del sistema. Esto tiene una triple implicación:  
  
-   La conexión dedicada con la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] permite un sondeo confiable a efectos de diagnósticos de componentes en todo momento, aunque la carga de la FCI sea elevada. Esto permite distinguir entre un sistema sometido a una carga elevada y un sistema que tenga realmente condiciones de error, lo que evita problemas tales como conmutaciones por error falsas.  
  
-   Los diagnósticos de componentes detallados permiten configurar una directiva de conmutación por error más flexible, en la que se puede elegir qué condiciones de error activan las conmutaciones por error y cuáles no.  
  
-   Los diagnósticos de componentes detallados también permiten una mejor solución de problemas de conmutaciones automáticas por error con carácter retroactivo. La información de diagnóstico se almacena los archivos de registro, que se colocan con los registros de errores de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Puede cargarlos en el Visor de archivos de registro para inspeccionar los estados de los componentes que dan lugar a la conmutación por error para determinar la causa de dicha conmutación por error.  
  
 Para obtener más información, vea [Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
##  <a name="elements-of-a-failover-cluster-instance"></a><a name="FCIelements"></a> Elementos de una instancia de clúster de conmutación por error  
 Una FCI consta de un conjunto de servidores físicos (nodos) que contienen una configuración de hardware similar y una configuración de software idéntica que incluye la versión y el nivel de revisión del sistema operativo, así como la versión, el nivel de revisión, los componentes y el nombre de instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Es necesaria una configuración de software idéntica para garantizar que la FCI pueda estar totalmente funcional cuando realice la conmutación por error entre los nodos.  
  
 Grupo de recursos de WSFC  
 Una FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] se ejecuta en un grupo de recursos de WSFC. Cada nodo del grupo de recursos conserva una copia sincronizada de los valores de configuración y las claves del Registro con punto de comprobación para garantizar la funcionalidad total de la FCI después de una conmutación por error, y solo uno de los nodos del clúster pertenece al grupo de recursos cada vez (el nodo activo). El servicio de WSFC administra el clúster de servidores, la configuración de quórum, la directiva de conmutación por error y las operaciones de conmutación por error, así como el VNN y las direcciones IP virtuales para la FCI. En caso de que se produzca un error (errores de hardware, errores del sistema operativo o errores de aplicación o de servicio) o se realice una actualización planeada, la propiedad del grupo de recursos se mueve a otro nodo de la FCI. El número de nodos que se admiten en un grupo de recursos de WSFC depende de la edición de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Además, el mismo clúster de WSFC puede ejecutar varias FCI (varios grupos de recursos), dependiendo de la capacidad de hardware, como número de CPUs, memoria y número de discos.  
  
 Binarios de SQL Server  
 Los archivos binarios del producto se instalan localmente en cada nodo de la FCI; es un proceso similar a las instalaciones independientes de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Sin embargo, durante el inicio, los servicios no se inician automáticamente sino que son administrados por WSFC.  
  
 Storage  
 Al contrario que en el grupo de disponibilidad, una FCI debe usar el almacenamiento compartido entre todos los nodos de la FCI para el almacenamiento de la base de datos y del registro. El almacenamiento compartido puede realizarse en forma de discos de clúster de WSFC, discos de una SAN, Espacios de almacenamiento directo (S2D) o recursos compartidos de archivos en un SMB. De esta manera, todos los nodos de la FCI tienen la misma vista de datos de instancia cada vez que se produce una conmutación por error. Sin embargo, esto significa que el almacenamiento compartido tiene la posibilidad de ser el punto de error único y la FCI depende de la solución de almacenamiento subyacente para garantizar la protección de datos.  
  
 Nombre de red  
 La VNN para la FCI proporciona un punto de conexión unificado para la FCI. Esto permite que las aplicaciones se conecten a la VNN sin necesidad de conocer el nodo actualmente activo. Cuando se produce una conmutación por error, la VNN se registra en el nuevo nodo activo después de iniciarse. Este proceso es transparente para el cliente o aplicación que se conecta a [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y minimiza el tiempo de inactividad que la aplicación o los clientes experimentan durante un error.  
  
 Direcciones IP virtuales  
 En el caso de una FCI de múltiples subredes, se asigna una dirección IP virtual a cada subred de la FCI. Durante una conmutación por error, la VNN en el servidor DNS se actualiza para señalar a la dirección IP virtual correspondiente a la subred respectiva. Las aplicaciones y los clientes pueden conectarse entonces a la FCI utilizando la misma VNN después de una conmutación por error de múltiples subredes.  
  
##  <a name="sql-server-failover-concepts-and-tasks"></a><a name="ConceptsAndTasks"></a> Conceptos y tareas de conmutación por error de SQL Server  
  
|Conceptos y tareas|Tema|  
|------------------------|-----------|  
|Describe el mecanismo de detección de errores y la directiva de conmutación por error flexible.|[Failover Policy for Failover Cluster Instances](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)|  
|Describe los conceptos de la administración y el mantenimiento de la FCI.|[Administración y mantenimiento de la instancia de clúster de conmutación por error](../../../sql-server/failover-clusters/windows/failover-cluster-instance-administration-and-maintenance.md)|  
|Describe la configuración y conceptos de varias subredes|[Agrupación en clústeres de varias subredes de SQL Server &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/sql-server-multi-subnet-clustering-sql-server.md)|  
  
##  <a name="related-topics"></a><a name="RelatedTopics"></a> Temas relacionados  
  
|**Descripciones del tema**|**Tema.**|  
|----------------------------|---------------|  
|Describe cómo instalar una nueva FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .|[Crear un nuevo clúster de conmutación por error de SQL Server &#40;programa de instalación&#41;](../../../sql-server/failover-clusters/install/create-a-new-sql-server-failover-cluster-setup.md)|  
|Describe cómo actualizarse a un clúster de conmutación por error de [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)] .|[Actualización de una instancia del clúster de conmutación por error de SQL Server](../../../sql-server/failover-clusters/windows/upgrade-a-sql-server-failover-cluster-instance.md).|  
|Describe los conceptos de los clústeres de conmutación por error de Windows y proporciona vínculos a las tareas relacionadas con dichos clústeres.|[Clúster de conmutación por error de Windows Server con SQL Server](windows-server-failover-clustering-wsfc-with-sql-server.md)|
|Describe las distinciones de conceptos entre nodos de una FCI y réplicas de un grupo de disponibilidad y las consideraciones para utilizar una FCI de modo que hospede una réplica para un grupo de disponibilidad.|[Clústeres de conmutación por error y grupos de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/failover-clustering-and-always-on-availability-groups-sql-server.md)|  
  
