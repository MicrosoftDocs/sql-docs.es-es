---
title: Instancia del clúster de conmutación por error con grupos de disponibilidad
description: Mejore la alta disponibilidad y la capacidad de recuperación ante desastres mediante la combinación de las características de una instancia de clúster de conmutación por error de SQL Server y un grupo de disponibilidad Always On.
ms.custom: seo-lt-2019
ms.date: 07/02/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- clustering [SQL Server]
- Availability Groups [SQL Server], WSFC clusters
- Failover Cluster Instances [SQL Server], see failover clustering [SQL Server]
- quorum [SQL Server]
- failover clustering [SQL Server], AlwaysOn Availability Groups
- Availability Groups [SQL Server], Failover Cluster Instances
ms.assetid: 613bfbf1-9958-477b-a6be-c6d4f18785c3
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 31cef60a33505278791d296aab1022be9b4bc06b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353878"
---
# <a name="failover-clustering-and-always-on-availability-groups-sql-server"></a>Clústeres de conmutación por error y grupos de disponibilidad AlwaysOn (SQL Server)

[!INCLUDE[sql windows only](../../../includes/applies-to-version/sql-windows-only.md)]

   [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)], la solución de alta disponibilidad y recuperación ante desastres introducida en [!INCLUDE[sssql11](../../../includes/sssql11-md.md)], requiere clústeres de conmutación por error de Windows Server (WSFC). Además, aunque [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] no depende de los clústeres de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , se puede utilizar una instancia de clústeres de conmutación por error (FCI) para hospedar una réplica de disponibilidad para un grupo de disponibilidad. Es importante conocer el rol de cada tecnología de clústeres y saber qué consideraciones son necesarias cuando se diseña el entorno de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
  
> [!NOTE]  
>  Para obtener información sobre los conceptos de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] , vea [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md).  
  
  
##  <a name="windows-server-failover-clustering-and-availability-groups"></a><a name="WSFC"></a> Grupos de disponibilidad y clústeres de conmutación por error de Windows Server  
 La implementación de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] requiere un Clúster de conmutación por error de Windows Server (WSFC). Para que una instancia de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] se habilite para [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], debe residir en un nodo de WSFC, y WSFC y el nodo deben estar en línea. Además, cada réplica de disponibilidad de un determinado grupo de disponibilidad debe residir en otro nodo del mismo WSFC. La única excepción es que mientras se migra a otro WSFC, un grupo de disponibilidad puede ocupar temporalmente dos clústeres.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] se basa en el Clúster de conmutación por error de Windows Server (WSFC) para supervisar y administrar los roles actuales de las réplicas de disponibilidad que pertenecen a un grupo de disponibilidad concreto, así como para determinar cómo afecta un evento de conmutación por error a las réplicas de disponibilidad. Por cada grupo de disponibilidad que cree, se creará un grupo de recursos de WSFC. El WSFC supervisa este grupo de recursos para evaluar el estado de la réplica principal.  
  
 El cuórum para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] se basa en todos los nodos del WSFC independientemente de si un nodo de clúster determinado hospeda alguna réplica de disponibilidad. A diferencia de la creación de reflejo de la base de datos, no hay ningún rol testigo en [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)].  
  
 Los votos de cuórum de nodos del clúster determinan el estado general de un WSFC. Si el WSFC se queda sin conexión debido a un desastre no planeado, o a un error persistente de hardware o de comunicaciones, se necesita la intervención manual del administrador. Un administrador de Windows Server o de WSFC tendrá que forzar un cuórum y volver a poner en línea los nodos del clúster superviviente en una configuración sin tolerancia a errores.  
  
> [!IMPORTANT]  
>  Las claves del Registro de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] son subclaves del WSFC. Si elimina y vuelve a crear un WSFC, tendrá que deshabilitar y volver a habilitar la característica de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] en cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospedaba una réplica de disponibilidad en el WSFC original.  
  
 Para obtener información sobre cómo ejecutar [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en nodos de WSFC y sobre el cuórum de WSFC, vea [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md).  
  
##  <a name="ssnoversion-failover-cluster-instances-fcis-and-availability-groups"></a><a name="SQLServerFC"></a> [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Instancias de clúster de conmutación por error (FCI) y grupos de disponibilidad  
 Puede configurar un segundo nivel de conmutación por error en el nivel de instancia de servidor si implementa [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y FCI junto con el WSFC. Una réplica de disponibilidad se puede hospedar en una instancia independiente de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] o una instancia de FCI. Solo un asociado de FCI puede hospedar una réplica para un grupo de disponibilidad dado. Cuando una réplica de disponibilidad se ejecuta en una instancia de clúster de conmutación por error (FCI), la lista de posibles propietarios del grupo de disponibilidad contendrá solo el nodo de FCI activo.  
  
 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] no depende de ninguna forma de almacenamiento compartido. Sin embargo, si usa una instancia de clúster de conmutación por error (FCI) de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para hospedar una o varias réplicas de disponibilidad, cada una de las FCI requerirá almacenamiento compartido según la instalación típica de la instancia de clúster de conmutación por error de SQL Server.  
  
 Para obtener más información sobre los requisitos previos adicionales, vea la sección “Requisitos previos y restricciones para usar una instancia de clúster de conmutación por error (FCI)de SQL Server para hospedar una réplica de disponibilidad” de [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
### <a name="comparison-of-failover-cluster-instances-and-availability-groups"></a>Comparación de las instancias de clúster de conmutación por error y los grupos de disponibilidad  
 Independientemente del número de nodos de la FCI, una FCI completa hospeda una sola réplica en un grupo de disponibilidad. En la tabla siguiente se describen las diferencias conceptuales entre los nodos en una FCI y las réplicas en un grupo de disponibilidad.  
  
||Nodos en una FCI|Réplicas en un grupo de disponibilidad|  
|-|-------------------------|-------------------------------------------|  
|**Usa WSFC**|Sí|Sí|  
|**Nivel de protección**|Instancia|Base de datos|  
|**Tipo de almacenamiento**|Compartido|No compartidos<br /><br /> Mientras que las réplicas de un grupo de disponibilidad no comparten almacenamiento, una réplica hospedada por una FCI usa una solución de almacenamiento compartido de acuerdo con esa FCI. La solución de almacenamiento es compartida solo por los nodos en la FCI y no entre las réplicas del grupo de disponibilidad.|  
|**Soluciones de almacenamiento**|Se adjuntan directamente, SAN, puntos de montaje, SMB|Depende del tipo de nodo|  
|**Secundarios legibles**|No*|Sí|  
|**Opciones aplicables de la directiva de conmutación por error**|Quórum de WSFC<br /><br /> Específico de FCI<br /><br /> Configuración de grupo de disponibilidad**|Quórum de WSFC<br /><br /> Configuración de grupos de disponibilidad|  
|**Recursos conmutados por error**|Servidor, instancia y base de datos|Solo base de datos|  
  
 *Mientras que las réplicas secundarias sincrónicas de un grupo de disponibilidad se ejecutan siempre en las instancias respectivas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , los nodos secundarios de una FCI no han iniciado realmente las instancias respectivas de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] y, por lo tanto, no son legibles. En una FCI, un nodo secundario inicia la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] cuando la propiedad del grupo de recursos se le transfiere durante una conmutación por error de FCI. Sin embargo, en el nodo de FCI activo, cuando una base de datos hospedada por FCI pertenece a un grupo de disponibilidad, si la réplica de disponibilidad local se ejecuta como réplica secundaria legible, la base de datos es legible.  
  
 **La configuración de la directiva de conmutación por error del grupo de disponibilidad se aplica a todas las réplicas, ya estén hospedadas en una instancia independiente o una instancia de FCI.  
  
> [!NOTE]  
>  Para obtener más información sobre el **número de nodos** en FCI y **grupos de disponibilidad AlwaysOn** en otras ediciones de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea [Características compatibles con las ediciones de SQL Server 2012](/previous-versions/sql/sql-server-2012/cc645993(v=sql.110)) (https://go.microsoft.com/fwlink/?linkid=232473).  
  
### <a name="considerations-for-hosting-an-availability-replica-on-an-fci"></a>Consideraciones para hospedar una réplica de disponibilidad en una FCI  
  
> [!IMPORTANT]  
>  Si tiene previsto hospedar una réplica de disponibilidad en una instancia de clúster de conmutación por error (FCI) de SQL Server, asegúrese de que los nodos de host de Windows Server 2008 cumplen los requisitos previos y las restricciones de AlwaysOn para las instancias de clúster de conmutación por error (FCI). Para obtener más información, vea [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Las instancias de clúster de conmutación por error (FCI) no admiten la conmutación automática por error de grupos de disponibilidad; por lo tanto, todas las réplicas de disponibilidad hospedadas por un FCI solo se pueden configurar para la conmutación por error manual.  
  
 Es posible que necesite configurar un WSFC para incluir los discos compartidos que no están disponibles en todos los nodos. Por ejemplo, considere un WSFC en dos centros de datos con tres nodos. Dos de los nodos hospedan una instancia de clúster de conmutación por error (FCI) de SQL Server en el centro de datos principal y tienen acceso a los mismos discos compartidos. El tercer nodo hospeda una instancia independiente de SQL Server en otro centro de datos y no tiene acceso a los discos compartidos desde el centro de datos principal. Esta configuración de WSFC admite la implementación de un grupo de disponibilidad si la FCI hospeda la réplica principal y la instancia independiente hospeda la réplica secundaria.  
  
 Al elegir una FCI para hospedar una réplica de disponibilidad para un grupo de disponibilidad determinado, asegúrese de que una conmutación por error de FCI no puede hacer que un único nodo de WSFC intente hospedar dos réplicas de disponibilidad para el mismo grupo de disponibilidad.  
  
 El escenario de ejemplo siguiente muestra cómo podría producir problemas esta configuración:  
  
 Marcel configura un WSFC con dos nodos, `NODE01` y `NODE02`. Instala una instancia de clúster de conmutación por error de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , `fciInstance1`, en `NODE01` y `NODE02` , donde `NODE01` es el propietario actual de `fciInstance1`.  
 En `NODE02`, Marcel instala otra instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], `Instance3`, que es una instancia independiente.  
 En `NODE01`, Marcel habilita fciInstance1 para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. En `NODE02`, habilita `Instance3` para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]. A continuación, configura un grupo de disponibilidad para el que `fciInstance1` hospeda la réplica principal e `Instance3` hospeda la réplica secundaria.  
 En algún momento `fciInstance1` deja de estar disponible en `NODE01` y el WSFC produce una conmutación por error de `fciInstance1` en `NODE02`. Después de la conmutación por error, `fciInstance1` es una instancia habilitada para [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]que se ejecuta en el rol principal de `NODE02`. Sin embargo, `Instance3` reside ahora en el mismo nodo de WSFC que `fciInstance1`. Esto infringe la restricción de [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)] .  
 Para corregir el problema que se presenta en este escenario, la instancia independiente, `Instance3`, debe residir en otro nodo del mismo WSFC que `NODE01` y `NODE02`.  
  
 Para más información sobre las FCI de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], vea [Instancias de clúster de conmutación por error de AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md).  
  
##  <a name="restrictions-on-using-the-wsfc-failover-cluster-manager-with-availability-groups"></a><a name="FCMrestrictions"></a> Restricciones en el uso del Administrador de clústeres de conmutación por error de WSFC con grupos de disponibilidad  
 No use el Administrador de clústeres de conmutación por error para manipular grupos de disponibilidad, por ejemplo:  
  
-   No agregue ni quite recursos en el servicio en clúster (grupo de recursos) para el grupo de disponibilidad.  
  
-   No cambie las propiedades de los grupos de disponibilidad, como los propietarios posibles y los propietarios preferidos. El grupo de disponibilidad establece automáticamente estas propiedades.  
  
-   **No use el Administrador de clústeres de conmutación por error para mover los grupos de disponibilidad a otros nodos o para conmutar los grupos de disponibilidad.** El Administrador de clústeres de conmutación por error no conoce el estado de sincronización de las réplicas de disponibilidad y hacer esto puede conducir a un tiempo de inactividad extendido. Debe usar [!INCLUDE[tsql](../../../includes/tsql-md.md)] o [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)].  

  >[!WARNING]
  > Si usa el Administrador de clústeres de conmutación por error para mover una *instancia de clúster de conmutación por error* que hospeda un grupo de disponibilidad a un nodo que *ya* hospeda una réplica del mismo grupo de disponibilidad, podría provocar la pérdida de la réplica del grupo de disponibilidad, impidiendo que se ponga en línea en el nodo de destino. Un único nodo de un clúster de conmutación por error no puede hospedar más de una réplica para el mismo grupo de disponibilidad. Para más información sobre cómo se produce esto y cómo recuperar, eche un vistazo a la entrada de blog [Replica unexpectedly dropped in availability group](/archive/blogs/alwaysonpro/issue-replica-unexpectedly-dropped-in-availability-group) (Réplica dejada inesperadamente en grupo de disponibilidad). 
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   **Blogs:**  
  
     [Configurar el clúster de conmutación por error de Windows para SQL Server (grupo de disponibilidad o FCI) con seguridad limitada](/archive/blogs/sqlalwayson/configure-windows-failover-clustering-for-sql-server-availability-group-or-fci-with-limited-security)  
  
     [Blogs del equipo de AlwaysOn de SQL Server: blog oficial del equipo de AlwaysOn de SQL Server](/archive/blogs/sqlalwayson/)  
  
     [Blogs de los ingenieros de SQL Server de CSS](/archive/blogs/psssql/)  
  
-   **Notas del producto:**  
  
     [Guía de la arquitectura de AlwaysOn: generar una solución de alta disponibilidad y recuperación ante desastres mediante instancias de clúster de conmutación por error y grupos de disponibilidad](/previous-versions/sql/sql-server-2012/jj215886(v=msdn.10))  
  
     [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
     [Notas del producto de Microsoft para SQL Server 2012](https://msdn.microsoft.com/library/hh403491.aspx)  
  
     [Notas del producto del equipo de asesoramiento al cliente de SQL Server](https://techcommunity.microsoft.com/t5/DataCAT/bg-p/DataCAT/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Habilitar y deshabilitar grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/enable-and-disable-always-on-availability-groups-sql-server.md)   
 [Supervisar grupos de disponibilidad &#40;Transact-SQL&#41;](../../../database-engine/availability-groups/windows/monitor-availability-groups-transact-sql.md)   
 [Instancias de clúster de conmutación por error de AlwaysOn &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/always-on-failover-cluster-instances-sql-server.md)  
  
