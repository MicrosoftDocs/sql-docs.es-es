---
title: Configuración de los votos y modos de cuórum WSFC
description: Obtenga información sobre los modos de cuórum y los votos que usa el clúster de conmutación por error de Windows Server con una instancia de clúster de conmutación por error de SQL Server o un grupo de disponibilidad AlwaysOn.
ms.custom: seo-lt-2019
ms.date: 10/03/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], WSFC clusters
- quorum [SQL Server], AlwaysOn and WSFC quorum
- failover clustering [SQL Server], AlwaysOn Availability Groups
ms.assetid: ca0d59ef-25f0-4047-9130-e2282d058283
author: cawrites
ms.author: chadam
ms.openlocfilehash: 1b3833b0b57b78ab02694b6544551adfb68e52a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355247"
---
# <a name="wsfc-quorum-modes-and-voting-configuration-sql-server"></a>Configuración de los votos y modos de quórum WSFC (SQL Server)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Tanto [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)][!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] como las instancias de clúster de conmutación por error (FCI) de AlwaysOn aprovechan los Clústeres de conmutación por error de Windows Server (WSFC) como tecnología de plataforma.  WSFC usa un método basado en quórum para supervisar el estado general del clúster y maximizar la tolerancia a errores en el nivel de nodo. Entender los modos de cuórum WSFC y la configuración de las votaciones en los nodos es muy importante para el diseño, el funcionamiento y la solución de problemas de una solución de recuperación ante desastres AlwaysOn de alta disponibilidad.  
  
  
##  <a name="cluster-health-detection-by-quorum"></a><a name="ClusterHealthDetectionbyQuorum"></a> Detección del estado de clúster por quórum  
 Cada nodo de un clúster WSFC participa en la comunicación periódica de latido para compartir el estado de mantenimiento del nodo con los demás nodos. Los nodos que no responden se consideran que se encuentran en estado de error.  
  
 Un conjunto de nodos de *quórum* es una mayoría de los nodos con derecho a voto y testigos en el clúster WSFC. Un *voto de quórum* periódico determina el estado general de un clúster WSFC.  La presencia de un quórum significa que el clúster es correcto y puede proporcionar tolerancia a errores de nivel de nodo.  
  
 La ausencia de un quórum indica que el clúster no es correcto.  El estado general del clúster WSFC debe mantenerse para asegurarse de que los nodos secundarios sanos correctos disponibles para que los nodos primarios puedan conmutar por error.  Si el voto de quórum da error, el clúster WSFC se pondrá sin conexión como medida preventiva.  También hará que todas las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] registradas con el clúster se detengan.  
  
> [!IMPORTANT]  
>  Si un clúster WSFC se pone sin conexión debido a un error del quórum, se requiere una intervención manual para volver a ponerlo en conexión.  
>   
>  Para más información, consulte: [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md).  
  
##  <a name="quorum-modes"></a><a name="QuorumModes"></a> Modos de cuórum  
 Un *modo de quórum* se configura en el nivel de clúster WSFC que dicta la metodología que se usa para los votos de quórum.  La utilidad Administrador de clústeres de conmutación por error recomendará un modo de quórum basándose en el número de nodos del clúster.  
  
 Los siguientes modos de quórum se pueden usar para determinar qué constituye un quórum de votos:  
  
-   **Mayoría de nodos.** Más de la mitad de los nodos que votan en el clúster deben votar afirmativamente para que el clúster sea correcto.  
  
-   **Nodo y mayoría de recurso compartido de archivos.** Similar al modo de quórum Mayoría de nodos, excepto en que un recurso compartido de archivos remoto también se configura como testigo de la votación y la conectividad de cualquier nodo para ese recurso compartido también se cuenta como voto afirmativo.  Más de la mitad de los votos posibles debe ser afirmativa para que el clúster sea correcto.  
  
     Como práctica recomendada, el recurso compartido de archivos testigo no debe residir en ningún nodo del clúster y debe ser visible para todos los nodos del clúster.  
  
-   **Nodo y mayoría de disco.** Similar al modo de quórum Mayoría de nodos, excepto en que un recurso de clúster de disco compartido también se designa como testigo de la votación y la conectividad de cualquier nodo con ese disco compartido también se considera voto afirmativo.  Más de la mitad de los votos posibles debe ser afirmativa para que el clúster sea correcto.  
  
-   **Solo disco.** Un recurso de clúster de disco compartido se designa como testigo y la conectividad de cualquier nodo a ese disco compartido se considera voto afirmativo.  
  
> [!TIP]  
>  Al utilizar una configuración asimétrica de almacenamiento para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], normalmente debe usar el modo de quórum Mayoría de nodos cuando tenga un número impar de nodos que votan o el modo de quórum Mayoría de recurso compartido de archivo y nodo cuando tiene un número par de nodos que votan.  
  
##  <a name="voting-and-non-voting-nodes"></a><a name="VotingandNonVotingNodes"></a> Nodos que votan y nodos que no votan  
 De forma predeterminada, cada nodo del clúster WSFC se incluye como miembro del quórum del clúster; cada nodo tiene un único voto para determinar el estado general del clúster y cada nodo intentará continuamente establecer un quórum.  La discusión del quórum hasta este punto ha calificado cuidadosamente el conjunto de nodos de clúster WSFC que votan sobre el estado del clúster como *nodos que votan*.  
  
 Ningún nodo individual en un clúster WSFC puede determinar definitivamente que el clúster como conjunto sea correcto o incorrecto.  En un momento dado, desde la perspectiva de cada nodo, puede parecer que alguno de los otros nodos están sin conexión, en el proceso de conmutar por error o que no responden debido a un error de comunicación de red.  Una función clave del voto de quórum es determinar si el estado aparente de cada uno de los nodos del clúster WSFC es de hecho ese estado real de los nodos.  
  
 Para todos los modelos de quórum excepto "Solo disco", la eficacia de un voto de quórum depende de que las comunicaciones entre todos los nodos que votan del clúster sean confiables. Las comunicaciones de red entre los nodos de la misma subred física se deben considerar predecibles; el voto de quórum debe ser de confianza.  
  
 Sin embargo, si un nodo de otra subred se considera que no responde en un voto de quórum, pero realmente está en conexión y es correcta, es más probable que se deba a un error de comunicaciones de red entre las subredes.  Según la topología de clúster, el modo de quórum y la configuración de directivas de migración tras error, ese error de comunicaciones de red puede crear en efecto más de un conjunto (o subconjunto) de nodos que votan.  
  
 Cuando más de un subconjunto de nodos que votan puede establecer un cuórum por sí solo, lo que se conoce como *escenario de división de cerebro*.  En este escenario, los nodos de los quórums independientes pueden comportarse de forma diferente y en conflicto entre sí.  
  
> [!NOTE]  
>  El escenario de división de cerebro solo es posible si un administrador del sistema realiza manualmente una operación forzada de quórum o, en circunstancias muy poco habituales, una conmutación por error forzada; subdividiendo explícitamente el conjunto de nodos de quórum.  
  
 Para simplificar la configuración del cuórum y aumentar el tiempo de actividad, puede ser aconsejable ajustar el valor de *NodeWeight* de cada nodo para que el voto del nodo no se cuente en el cuórum.  
  
> [!IMPORTANT]  
>  Para usar la configuración de NodeWeight, se debe aplicar la siguiente revisión a todos los servidores del clúster de WSFC:  
>   
>  [KB2494036](https://support.microsoft.com/kb/2494036): hay disponible una revisión para permitir configurar un nodo de clúster que no tiene votos de quórum en [!INCLUDE[firstref_longhorn](../../../includes/firstref-longhorn-md.md)] y en [!INCLUDE[winserver2008r2](../../../includes/winserver2008r2-md.md)]  
  
##  <a name="recommended-adjustments-to-quorum-voting"></a><a name="RecommendedAdjustmentstoQuorumVoting"></a> Ajustes recomendados para la votación de quórum  
 Al habilitar o deshabilitar el voto de un nodo determinado de WSFC, siga estas instrucciones:  
  
-   **Sin voto de forma predeterminada.** Suponga que cada nodo no debe votar sin una justificación explícita.  
  
-   **Incluya todas las réplicas principales.** Cada nodo WSFC que hospeda una réplica principal de grupo de disponibilidad o es el propietario preferido de una FCI debe tener un voto.  
  
-   **Incluya los posibles propietarios de la conmutación automática por error.** Cada nodo que puede hospedar una réplica principal, como resultado de una conmutación automática por error de la FCI o del grupo de disponibilidad, debe tener un voto. Si solo hay un grupo de disponibilidad en el clúster de WSFC y las réplicas de disponibilidad están hospedadas en instancias independientes, esta regla incluye solo la réplica secundaria que es el destino de la conmutación por error automática.  
  
-   **Excluya los nodos secundarios del sitio.** En general, no proporcione votos a los nodos de WSFC que residen en un sitio secundario de recuperación de desastres.  No desea que los nodos del sitio secundario contribuyan a la decisión de poner el clúster fuera de conexión cuando no hay ningún problema con el sitio principal.  
  
-   **Número impar de votos.** Si es necesario, agregue un recurso compartido de archivos testigo, un nodo testigo o un disco testigo al clúster y ajuste el modo de quórum para evitar posibles empates en el voto de quórum.  
  
-   **Vuelva a valorar las asignaciones de votos después de la conmutación por error.** No es conveniente realizar la conmutación por error en una configuración de clúster que no admita un quórum correcto.  
  
> [!IMPORTANT]
>  Al validar el voto de cuórum de WSFC, el Asistente para grupo de disponibilidad de AlwaysOn muestra una advertencia si se cumple cualquiera de las siguientes condiciones:  
> 
>  -   El nodo de clúster que hospeda la réplica principal no tiene un voto  
> -   Una réplica secundaria está configurada para la conmutación automática por error y su nodo de clúster no tiene un voto.  
> -   [KB2494036](https://support.microsoft.com/kb/2494036) no está instalado en todos los nodos de clúster que hospedan las réplicas de disponibilidad. Esta revisión es necesaria para agregar o quitar los votos de nodos de clúster en las implementaciones de varios sitios. Sin embargo, no suele ser necesaria en las implementaciones de un solo sitio y la advertencia puede omitirse de forma segura.  
> 
> [!TIP]
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] expone varias vistas de administración dinámica (DMV) del sistema que pueden ayudarle a administrar la configuración de clúster WSFC relacionada y la votación del cuórum de los nodos.  
> 
>  Para obtener más información, consulte:  [sys.dm_hadr_cluster](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-transact-sql.md), [sys.dm_hadr_cluster_members](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-members-transact-sql.md), [sys.dm_os_cluster_nodes](../../../relational-databases/system-dynamic-management-views/sys-dm-os-cluster-nodes-transact-sql.md)y [sys.dm_hadr_cluster_networks](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-cluster-networks-transact-sql.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Ver la configuración de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Configurar los valores de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Comprobación de la configuración del voto de cuórum en los asistentes de los grupos de disponibilidad AlwaysOn](/archive/blogs/sqlalwayson/quorum-vote-configuration-check-in-alwayson-availability-group-wizards-andy-jing)  
  
-   [Tecnologías de Windows Server:  Clústeres de conmutación por error](https://technet.microsoft.com/library/cc732488\(v=WS.10\).aspx)  
  
-   [Failover Cluster Step-by-Step Guide: Configuring the Quorum in a Failover Cluster](https://technet.microsoft.com/library/cc770620\(WS.10\).aspx) (Guía paso a paso de clústeres de conmutación por error: Configuración del quórum en un clúster de conmutación por error)  
  
## <a name="see-also"></a>Consulte también  
 [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)   
 [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)  
  
