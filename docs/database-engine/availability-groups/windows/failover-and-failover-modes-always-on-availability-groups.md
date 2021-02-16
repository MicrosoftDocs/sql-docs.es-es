---
title: Modos de conmutación por error para grupos de disponibilidad
description: Una descripción de los diferentes modos de conmutación por error disponibles para las bases de datos que participan en un grupo de disponibilidad Always On.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
helpviewer_keywords:
- Availability Groups [SQL Server], availability replicas
- Availability Groups [SQL Server], failover
- Availability Groups [SQL Server], failover modes
- failover [SQL Server], AlwaysOn Availability Groups
ms.assetid: 378d2d63-50b9-420b-bafb-d375543fda17
author: cawrites
ms.author: chadam
ms.openlocfilehash: 469f51a30a7eac878b83fd14086507ccd952f708
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353893"
---
# <a name="failover-and-failover-modes-always-on-availability-groups"></a>Conmutación por error y modos de conmutación por error (grupos de disponibilidad AlwaysOn)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Dentro del contexto de un grupo de disponibilidad, el rol principal y el rol secundario de las réplicas de disponibilidad suelen ser intercambiables en un proceso denominado *conmutación por error*. Hay tres formas de conmutación por error: conmutación por error automática (sin pérdida de datos), conmutación por error manual planeada (sin pérdida de datos) y conmutación por error manual forzada (con posible pérdida de datos), normalmente denominada *conmutación por error forzada*. Las conmutaciones por error automáticas o manuales planeadas mantienen todos los datos. Un grupo de disponibilidad realiza la conmutación por error en el nivel de la réplica de disponibilidad. Es decir, un grupo de disponibilidad conmuta por error a una de sus réplicas secundarias (el *destino de conmutación por error* actual).  
  
> [!NOTE]  
>   A menos que la [Detección del estado del nivel de la base de datos](../../../database-engine/availability-groups/windows/sql-server-always-on-database-health-detection-failover-option.md) esté configurada, los problemas que se producen en el nivel de la base de datos, como, por ejemplo, que una base de datos pase a ser sospechosa debido a la pérdida de un archivo de datos, la eliminación de una base de datos o los daños de un registro de transacciones, no producen la conmutación por error del grupo de disponibilidad.  
  
 Durante la conmutación por error, el destino de la conmutación por error asume el rol principal, recupera las bases de datos y las pone en línea como las nuevas bases de datos principales. La réplica principal anterior, cuando está disponible, cambia al rol secundario y sus bases de datos se convierten en bases de datos secundarias. Estos roles podrían conmutarse repetidamente (o usar un destino de conmutación por error distinto) como respuesta a varios errores o con fines administrativos.  
  
 Las formas de conmutación por error que admite una determinada réplica de disponibilidad se especifican mediante la propiedad *modo de conmutación por error* . En una determinada réplica de disponibilidad, los modos de conmutación por error posibles dependen del [modo de disponibilidad](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md) de la réplica, como sigue:  
  
-   Las **réplicas de confirmación sincrónica** admiten dos valores: automático o manual. El valor “automático” admite tanto la conmutación por error automática como la manual. Para evitar la pérdida de datos, la conmutación automática por error y la conmutación por error planeada requieren que el destino de conmutación por error sea una réplica secundaria de confirmación sincrónica con un estado de sincronización apropiado (esto indica que todas las bases de datos secundarias del destino de conmutación por error están sincronizadas con la base de datos principal correspondiente). Siempre que una réplica secundaria no cumple las dos condiciones, solo admite la conmutación por error manual forzada. Tenga en cuenta que la conmutación por error también admite una réplica cuyo rol esté en el estado RESOLVING.  
  
-   Las **réplicas de confirmación asincrónica** solo admiten el modo de conmutación por error manual. Además, debido a que nunca se sincronizan, solo admiten la conmutación por error forzada.  
  
> [!NOTE]  
>  Después de una conmutación por error, las aplicaciones cliente que necesitan tener acceso a las bases de datos principales deben establecer conexión con la nueva réplica principal. Además, si la nueva réplica secundaria se configura para permitir el acceso de solo lectura, las aplicaciones cliente de solo lectura pueden conectarse a ella. Para obtener información sobre cómo se conectan los clientes a un grupo de disponibilidad, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
 **Secciones de este tema:**  
  
-   [Términos y definiciones](#TermsAndDefinitions)  
  
-   [Información general de la conmutación por error](#Overview)  
  
-   [Conmutación automática por error](#AutomaticFailover)  
  
-   [Conmutación por error manual planeada (sin pérdida de datos)](#ManualFailover)  
  
-   [Conmutación por error forzada (con posible pérdida de datos)](#ForcedFailover)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
-   [Contenido relacionado](#RelatedContent)  
  
##  <a name="terms-and-definitions"></a><a name="TermsAndDefinitions"></a> Términos y definiciones  
 Conmutación por error automática  
 Conmutación por error que se produce automáticamente en la pérdida de la réplica principal. La conmutación por error automática solo se admite cuando la réplica principal actual y una réplica secundaria están configuradas con el modo de conmutación por error establecido en AUTOMATIC y la réplica secundaria está sincronizada actualmente.  Si el modo de conmutación por error de la réplica principal o la réplica secundaria es MANUAL, no puede producirse la conmutación por error automática.  
  
 Conmutación por error manual planeada (sin pérdida de datos)  
 La conmutación por error manual planeada, o *conmutación por error manual*, es una conmutación por error iniciada por un administrador de bases de datos, normalmente con fines administrativos. Una conmutación por error manual planeada solo se admite si tanto la réplica principal como la secundaria se configuran en modo de confirmación sincrónica y si ambas se encuentran sincronizadas (en estado SYNCHRONIZED). Cuando la réplica secundaria de destino está sincronizada, puede realizarse una conmutación por error manual (sin pérdida de datos) aunque la réplica principal se haya bloqueado, ya que las bases de datos secundarias están listas para la conmutación por error. Un administrador de base de datos inicia manualmente una conmutación por error manual.  
  
 Conmutación por error forzada (con posible pérdida de datos)  
 Un administrador de bases de datos puede iniciar una conmutación por error cuando no hay ninguna réplica secundaria en estado SYNCHRONIZED con la réplica principal o cuando la réplica principal no se está ejecutando y no hay ninguna réplica secundaria lista para la conmutación por error. La conmutación por error forzada puede sufrir pérdida de datos y está recomendada únicamente para la recuperación ante desastres. La conmutación por error forzada también se conoce como conmutación por error manual forzada porque solo se puede iniciar manualmente. Esta es la única forma de conmutación por error admitida en el modo de disponibilidad de confirmación asincrónica.  
  
 [!INCLUDE[ssFosAutoC](../../../includes/ssfosautoc-md.md)]  
 Dentro de un grupo de disponibilidad dado, par de réplicas de disponibilidad (incluida la réplica principal actual) que están configuradas para el modo de confirmación sincrónica con conmutación por error automática, si existe. Un [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)] solo tiene efecto si la réplica secundaria está actualmente en estado SYNCHRONIZED con la réplica principal.  
  
 [!INCLUDE[ssFosSyncC](../../../includes/ssfossyncc-md.md)]  
 Dentro de un grupo de disponibilidad dado, conjunto de dos o tres réplicas de disponibilidad (incluida la réplica principal actual) que están configuradas para el modo de confirmación sincrónica, si lo hubiera. Un [!INCLUDE[ssFosSync](../../../includes/ssfossync-md.md)]solo tiene efecto si las replicas secundarias están configuradas para el modo de conmutación por error manual y al menos una réplica secundaria está actualmente en estado SYNCHRONIZED con la réplica principal.  
  
 [!INCLUDE[ssFosEntireC](../../../includes/ssfosentirec-md.md)]  
 En un grupo de disponibilidad determinado, conjunto de todas las réplicas de disponibilidad cuyo estado operativo es actualmente ONLINE, independientemente del modo de disponibilidad y el modo de conmutación por error. El [!INCLUDE[ssFosEntire](../../../includes/ssfosentire-md.md)]es muy importante cuando no hay actualmente ninguna réplica secundaria en estado SYNCHRONIZED con la réplica principal.  
  
##  <a name="overview-of-failover"></a><a name="Overview"></a> Información general de la conmutación por error  
 En la siguiente tabla se resumen las formas de conmutación por error admitidas en diferentes modos de disponibilidad y de conmutación por error. En cada pareja, el modo de disponibilidad y el modo de conmutación reales vienen determinados por la intersección de los modos de la réplica principal y los modos de una o varias réplicas secundarias.  
  
|Forma de conmutación por error|Modo de confirmación asincrónica|Modo de confirmación sincrónica con modo de conmutación por error manual|Modo de confirmación sincrónica con modo de conmutación por error automática|  
|-------------------------------|---------------------------------------------------------|------------------------------------------------------------|  
|Conmutación por error automática|No|No|Sí|  
|Conmutación por error manual planeada|No|Sí|Sí|  
|conmutación por error forzada|Sí|Sí|Sí **&#42;**|  
  
 **&#42;** Si emite un comando de conmutación por error forzada en una réplica secundaria sincronizada, la réplica secundaria se comportará igual que en el caso de una conmutación por error manual.  
  
 El período de tiempo que la base de datos no está disponible durante una conmutación por error depende del tipo de conmutación por error y su causa.  
  
> [!IMPORTANT]  
>  Para admitir las conexiones de cliente después de la conmutación por error, excepto en las bases de datos independientes, los inicios de sesión y los trabajos definidos en cualquiera de las bases de datos principales anteriores se deben volver a crear manualmente en la nueva base de datos principal. Para obtener más información, vea [Administración de inicios de sesión y de trabajos para las bases de datos de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md).  
  
### <a name="failover-sets"></a>Conjuntos de conmutación por error  
 Las formas de conmutación por error posibles para un grupo de disponibilidad determinado se pueden considerar como conjuntos de conmutación por error. Un conjunto de conmutación por error consta de una réplica principal y varias réplicas secundarias que admiten una determinada forma de conmutación, de la manera siguiente:  
  
-   **[!INCLUDE[ssFosAutoC](../../../includes/ssfosautoc-md.md)] (opcional):**  Dentro de un grupo de disponibilidad dado, par de réplicas de disponibilidad (incluida la réplica principal actual) que están configuradas para el modo de confirmación sincrónica con conmutación por error automática, si existe. Un conjunto de conmutación automática por error solo tiene efecto si la réplica secundaria está actualmente en estado SYNCHRONIZED con la réplica principal.  
  
-   **[!INCLUDE[ssFosSyncC](../../../includes/ssfossyncc-md.md)] (opcional):**  Dentro de un grupo de disponibilidad dado, conjunto de dos o tres réplicas de disponibilidad (incluida la réplica principal actual) que están configuradas para el modo de confirmación sincrónica, si lo hubiera. Un conjunto de confirmación sincrónica de conmutación automática por error solo tiene efecto si las réplicas secundarias están configuradas para el modo de conmutación por error manual y al menos una réplica secundaria está actualmente en estado SYNCHRONIZED con la réplica principal.  
  
-   **[!INCLUDE[ssFosEntireC](../../../includes/ssfosentirec-md.md)]:**   En un grupo de disponibilidad determinado, conjunto de todas las réplicas de disponibilidad cuyo estado operativo es actualmente ONLINE, independientemente del modo de disponibilidad y el modo de conmutación por error. El conjunto completo de conmutación por error es importante cuando no hay actualmente ninguna réplica secundaria en estado SYNCHRONIZED con la réplica principal.  
  
 Al configurar una réplica de disponibilidad como confirmación sincrónica con conmutación por error automática, la réplica de disponibilidad forma parte de [!INCLUDE[ssFosAuto](../../../includes/ssfosauto-md.md)]. Sin embargo, que el conjunto tenga efecto dependerá de la réplica principal actual. Las formas de conmutación por error que son actualmente posibles en un momento determinado dependen de qué conjuntos de conmutación estén actualmente activos.  
  
 Por ejemplo, considere el caso de un grupo de disponibilidad con cuatro réplicas de disponibilidad, del siguiente modo:  
  
|Réplica|Configuración de disponibilidad y modo de conmutación por error|  
|-------------|--------------------------------------------------|  
|A|Confirmación sincrónica con conmutación por error automática|  
|B|Confirmación sincrónica con conmutación por error automática|  
|C|Confirmación sincrónica con solo conmutación por error manual planeada|  
|D|Confirmación asincrónica (con solo conmutación por error forzada)|  
  
 El comportamiento de conmutación por error para cada réplica secundaria dependerá de qué réplica de disponibilidad sea actualmente la réplica principal. Básicamente, para una réplica secundaria dada, el comportamiento de conmutación por error es el peor caso dada la réplica principal actual. En la ilustración siguiente se muestra cómo el comportamiento de la conmutación por error de réplicas secundarias varía en función de la réplica principal actual y si está configurada para el modo de confirmación asincrónica (con solo conmutación por error forzada) o el modo de confirmación sincrónica (con o sin conmutación automática por error).  
  
 ![Cómo afecta la configuración de la réplica principal a la conmutación por error](../../../database-engine/availability-groups/windows/media/aoag-failoversetexample.gif "Cómo afecta la configuración de la réplica principal a la conmutación por error")  
  
##  <a name="automatic-failover"></a><a name="AutomaticFailover"></a> Automatic Failover  
 Una conmutación por error automática hace que una réplica secundaria calificada realice la transición automática al rol principal después de que la réplica principal deje de estar disponible. La conmutación por error automática es más apropiada cuando el nodo de WSFC que hospeda la réplica principal es local para el nodo que hospeda la réplica secundaria. Esto se debe a que la sincronización de datos funciona mejor con una latencia de mensajes baja entre equipos y a que las conexiones de cliente pueden seguir siendo locales.  
  
 **En esta sección:**  
  
-   [Condiciones requeridas para una conmutación automática por error](#RequiredConditions)  
  
-   [Cómo funciona la conmutación automática por error](#HowAutoFoWorks)  
  
-   [Para habilitar la conmutación por error automática](#EnableAutoFo)  
  
###  <a name="conditions-required-for-an-automatic-failover"></a><a name="RequiredConditions"></a> Condiciones requeridas para una conmutación automática por error  
 La conmutación por error automática solo aparece en las siguientes condiciones:  
  
-   Ya existe un conjunto de conmutación por error automática. Este conjunto consta de una réplica principal y una secundaria (el *destino de conmutación automática por error*) que están configuradas para el modo de confirmación sincrónica y también para la conmutación AUTOMÁTICA por error. Si la réplica principal se establece en modo de conmutación por error MANUAL, no puede producirse la conmutación por error automática aunque se establezca una réplica secundaria en el modo de conmutación AUTOMÁTICA por error.  
  
     Para obtener más información, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
-   El destino de la conmutación por error automática tiene un estado de sincronización correcto (esto indica que cada base de datos secundaria del destino de la conmutación por error se sincroniza con la base de datos principal correspondiente).  
  
    > [!TIP]  
    >  Los grupos de disponibilidad AlwaysOn supervisan el estado de ambas réplicas de un conjunto de conmutación automática por error. Si se produce un error en alguna de las réplicas, el estado del grupo de disponibilidad se establece en CRITICAL. Si se produce un error en la réplica secundaria, la conmutación por error automática no es posible debido a que el destino de esta no está disponible. Si se produce un error en la réplica principal, el grupo de disponibilidad realizará una conmutación por error a la réplica secundaria. No existirá ningún destino de conmutación por error automática hasta que la réplica principal anterior se ponga en línea. De todas maneras, para garantizar la disponibilidad en el caso improbable de un fallo secuencial, se recomienda configurar una réplica secundaria diferente como destino de la conmutación por error automática.  
    >   
    >  Para obtener más información, vea [Usar directivas de AlwaysOn para ver el estado de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/use-always-on-policies-to-view-the-health-of-an-availability-group-sql-server.md) y [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md).  
  
-   El clúster del servicio de clústeres de conmutación por error (WSFC) tiene quórum. Para obtener más información, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  
  
-   La réplica principal ha dejado de estar disponible y se han cumplido los niveles de condición de conmutación por error de la directiva de conmutación por error flexible. Para obtener más información sobre los niveles de condición de conmutación por error, vea [Directiva de conmutación por error flexible para conmutación automática por error de un grupo de disponibilidad &#40;SQL Server&#41;](./configure-flexible-automatic-failover-policy.md).  
  
###  <a name="how-automatic-failover-works"></a><a name="HowAutoFoWorks"></a> Cómo funciona la conmutación automática por error  
 Una conmutación automática por error inicia la siguiente secuencia de acciones:  
  
1.  Si la instancia del servidor que hospeda la réplica principal actual sigue en ejecución, cambia el estado de las bases de datos principales a DISCONNECTED y desconecta todos los clientes.  
  
2.  Si las entradas del registro están esperando en colas de recuperación en la réplica secundaria de destino, la réplica secundaria aplica las entradas del registro restantes para terminar la puesta al día de las bases de datos secundarias.  
  
    > [!NOTE]  
    >  La cantidad de tiempo que requiere la aplicación del registro a una base de datos dada depende de la velocidad del sistema, la carga de trabajo reciente y la cantidad de registro en la cola de cola de recuperación.  
  
3.  Las réplica secundaria anterior realiza la transición al rol principal. Sus bases de datos se convierten en las bases de datos principales. La nueva réplica principal revierte las transacciones no confirmadas (fase de reversión de recuperación) lo más rápidamente posible. Los bloqueos aíslan las transacciones no confirmadas, permitiendo que se produzca la reversión en segundo plano mientras los clientes usan la base de datos. Este proceso no revierte las transacciones confirmadas.  
  
     Hasta que se conecta una determinada base de datos secundaria, se marca brevemente como NOT_SYNCHRONIZED. Antes de que la recuperación de reversión se inicie, las bases de datos secundarias pueden conectarse a las bases de datos principales y pasar rápidamente al estado SYNCHRONIZED. El caso que mejor suele funcionar es aquel en el que hay una tercera réplica de confirmación sincrónica que permanece en el rol secundario después de la conmutación por error.  
  
4.  Posteriormente, cuando la instancia del servidor que hospeda la réplica principal anterior se reinicia, reconoce que otra réplica de disponibilidad posee el rol principal. La réplica principal anterior realiza la transición al rol secundario y sus bases de datos se convierten en bases de datos secundarias. La nueva réplica secundaria se conecta a la réplica principal actual y detecta su base de datos en las bases de datos principales actuales lo más rápidamente posible. Tan pronto como la nueva réplica secundaria haya vuelto a sincronizar sus bases de datos, la conmutación por error vuelve a ser posible, pero en dirección inversa.  
  
###  <a name="to-configure-automatic-failover"></a><a name="EnableAutoFo"></a> Para configurar la conmutación automática por error  
 Una réplica de disponibilidad se puede configurar para admitir la conmutación por error automática en cualquier momento.  
  
 **Para configurar la conmutación automática por error**  
  
1.  Asegúrese de que la réplica secundaria esté configurada para utilizar el modo de disponibilidad de confirmación sincrónica. Para obtener más información, vea [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md).  
  
2.  Establezca el modo de conmutación por error a automático. Para obtener más información, vea [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md).  
  
3.  Si lo desea, también puede cambiar la directiva de conmutación por error flexible del grupo de disponibilidad para especificar los tipos de errores que pueden causar una conmutación automática por error. Para obtener más información, vea [Configurar la directiva de conmutación por error flexible para controlar las condiciones de la conmutación automática por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md) y [Directiva de conmutación por error para instancias de clústeres de conmutación por error](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md).  
  
##  <a name="planned-manual-failover-without-data-loss"></a><a name="ManualFailover"></a> Conmutación por error manual planeada (sin pérdida de datos)  
 Una conmutación por error manual produce la transición de una réplica secundaria al rol principal después de que un administrador de bases de datos emita un comando de conmutación por error manual en la instancia del servidor que hospeda la réplica secundaria de destino. Para admitir la conmutación por error manual, la réplica secundaria y la réplica principal actual se deben configurar en modo de confirmación sincrónica, si lo hubiera. Cada base de datos secundaria de la réplica de disponibilidad debe unirse al grupo de disponibilidad y sincronizarse con la base de datos principal correspondiente (es decir, la réplica secundaria se debe sincronizar). Esto garantiza que cada transacción confirmada en una base de datos principal anterior también se ha confirmado en la nueva base de datos principal. Por tanto, las nuevas bases de datos principales son idénticas a las bases de datos principales antiguas.  
  
 En la siguiente ilustración se muestran las fases de una conmutación por error planeada:  
  
1.  Antes de la conmutación por error, la instancia del servidor hospeda la réplica principal en `Node01`.  
  
2.  Un administrador de bases de datos inicia una conmutación por error planeada. El destino de la conmutación por error es la réplica de disponibilidad hospedada por la instancia de servidor en `Node02`.  
  
3.  El destino de la conmutación por error (en `Node02`) se convierte en la nueva réplica principal. Como se trata de una conmutación por error planeada, la réplica principal anterior se cambia al rol secundario durante la conmutación por error y pone inmediatamente sus bases de datos en línea como bases de datos secundarias.  
  
 ![Ilustración de una conmutación por error manual planeada](../../../database-engine/availability-groups/windows/media/aoag-plannedmanualfailover.gif "Ilustración de una conmutación por error manual planeada")  
  
 **En esta sección:**  
  
-   [Condiciones requeridas para una conmutación por error manual](#ManualFailoverConditions)  
  
-   [Cómo funciona la conmutación por error manual](#ManualFailoverHowWorks)  
  
-   [Mantener la disponibilidad durante las actualizaciones](#ManualFailoverDuringUpgrades)  
  
###  <a name="conditions-required-for-a-manual-failover"></a><a name="ManualFailoverConditions"></a> Condiciones requeridas para una conmutación por error manual  
 Para admitir una conmutación por error manual, la réplica principal actual debe establecerse en modo de confirmación sincrónica y una réplica secundaria debe estar:  
  
-   Configurada para el modo de confirmación sincrónica.  
  
-   Sincronizada actualmente con la réplica principal.  
  
 Para realizar la conmutación por error manual en un grupo de disponibilidad, debe conectarse a la réplica secundaria que se va a convertir en la nueva réplica principal.  
  
###  <a name="how-a-planned-manual-failover-works"></a><a name="ManualFailoverHowWorks"></a> Cómo funciona la conmutación por error manual planeada  
 Una conmutación por error manual planeada, que se debe iniciar en la réplica secundaria de destino, inicia la siguiente secuencia de acciones:  
  
1.  Para asegurarse de que se producen transacciones de ningún usuario nuevo en las bases de datos principales originales, el clúster de WSFC envía una solicitud a la réplica principal para pasar a modo sin conexión.  
  
2.  Si un registro está esperando en la cola de recuperación de cualquier base de datos secundaria, la réplica secundaria finaliza las puesta al día de esa base de datos secundaria. La cantidad de tiempo que se necesita para ello depende de la velocidad del sistema, la carga de trabajo reciente y la cantidad de registro en la cola de recuperación. Para conocer el tamaño actual de la cola de recuperación, utilice el contador de rendimiento **Cola de recuperación** . Para obtener más información, vea [SQL Server, Database Replica (SQL Server, réplica de base de datos)](../../../relational-databases/performance-monitor/sql-server-database-replica.md).  
  
    > [!NOTE]  
    >  El tiempo de conmutación por error se puede regular limitando el tamaño de la cola de recuperación. Sin embargo, esto puede hacer que la réplica principal se ralentice para permitir que la réplica secundaria no se retrase.  
  
3.  La réplica secundaria se convierte en la nueva réplica principal y la réplica principal anterior se convierte en la nueva réplica secundaria.  
  
4.  La nueva réplica principal revierte las transacciones no confirmadas y pone sus bases de datos en línea como bases de datos principales. Todas las bases de datos secundarias se marcan brevemente como NOT SYNCHRONIZED hasta que se conectan y vuelven a sincronizar con las nuevas bases de datos principales. Este proceso no revierte las transacciones confirmadas.  
  
5.  Cuando la réplica principal anterior vuelve a estar en línea, toma el rol secundario y la base de datos principal anterior se convierte en la base de datos secundaria. La nueva réplica secundaria vuelve a sincronizar rápidamente las nuevas bases de datos secundarias con las bases de datos principales correspondientes.  
  
    > [!NOTE]  
    >  Tan pronto como la nueva réplica secundaria haya vuelto a sincronizar las bases de datos, la conmutación por error vuelve a ser posible, pero en dirección inversa.  
  
 Tras la conmutación por error, los clientes deben volver a conectarse a la base de datos principal actual. Para obtener más información, vea [Agentes de escucha de grupo de disponibilidad, conectividad de cliente y conmutación por error de una aplicación &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md).  
  
###  <a name="maintaining-availability-during-upgrades"></a><a name="ManualFailoverDuringUpgrades"></a> Mantener la disponibilidad durante las actualizaciones  
 El administrador de base de datos para sus grupos de disponibilidad puede utilizar conmutaciones por error manuales para mantener la disponibilidad de la base de datos al actualizar el hardware o el software. Para utilizar un grupo de disponibilidad para actualizaciones de software, la instancia del servidor y/o el nodo del equipo que hospeda la réplica secundaria de destino deben haber recibido ya las actualizaciones. Para obtener más información, vea [Actualización de instancias de la réplica del grupo de disponibilidad AlwaysOn](../../../database-engine/availability-groups/windows/upgrading-always-on-availability-group-replica-instances.md).  
  
##  <a name="forced-failover-with-possible-data-loss"></a><a name="ForcedFailover"></a> Conmutación por error forzada (con posible pérdida de datos)  
 La acción de forzar una conmutación por error de un grupo de disponibilidad (con posible pérdida de datos) es un método de recuperación ante desastres que permite usar una réplica secundaria como servidor en espera semiactiva. Puesto que forzar la conmutación por error puede suponer una posible pérdida de datos, se debe hacer con precaución y moderación. Se recomienza que la conmutación por error solo se fuerce si es necesario restaurar el servicio en las bases de datos de disponibilidad inmediatamente y es aceptable el riesgo de perder algunos datos. Para obtener más información sobre los requisitos previos y las recomendaciones para forzar la conmutación por error y para ver un escenario de ejemplo que usa una conmutación por error forzada para recuperarse de un error grave, vea [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
> [!WARNING]  
>  La acción de forzar una conmutación por error requiere que el clúster de WSFC tenga quórum. Para obtener información sobre cómo configurar y forzar el cuórum, vea [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md).  
  
 **En esta sección:**  
  
-   [Cómo funciona la conmutación por error forzada](#ForcedFailoverHowWorks)  
  
-   [Riesgos de forzar la conmutación por error](#ForcedFailoverRisks)  
  
-   [Por qué se requiere la conmutación por error después de forzar el quórum](#WhyFFoPostForcedQuorum)  
  
-   [Seguimiento de la posible pérdida de datos](#TrackPotentialDataLoss)  
  
-   [Administrar la potencial pérdida de datos](#ForcedFailoverManagingDataLoss)  
  
###  <a name="how-forced-failover-works"></a><a name="ForcedFailoverHowWorks"></a> Cómo funciona la conmutación por error forzada  
 Forzar la conmutación por error inicia una migración del rol principal a una réplica de destino cuyo rol está en el estado SECONDARY o RESOLVING. El destino de la conmutación por error se convierte en la nueva réplica principal y sirve inmediatamente sus copias de las bases de datos a los clientes. Cuando la réplica principal anterior está disponible, realizará la transición al rol secundario y sus bases de datos se convertirán en bases de datos secundarias.  
  
 Todas las bases de datos secundarias (incluidas las bases de datos principales anteriores cuando están disponibles) cambian al estado SUSPENDED. Según el estado de sincronización de datos anterior de una base de datos secundaria suspendida podría ser conveniente para recuperar los datos confirmados que faltan para esa base de datos principal. En una réplica secundaria configurada para el acceso de solo lectura se pueden consultar las bases de datos secundarias para detectar manualmente los datos que faltan. A continuación, se pueden emitir instrucciones [!INCLUDE[tsql](../../../includes/tsql-md.md)] en las nuevas bases de datos principales para realizar los cambios necesarios.  
  
###  <a name="risks-of-forcing-failover"></a><a name="ForcedFailoverRisks"></a> Riesgos de forzar la conmutación por error  
 Es esencial tener en cuenta que si se fuerza la conmutación por error se pueden perder datos. La pérdida de datos es posible porque la réplica de destino no puede comunicarse con la réplica principal y, por lo tanto, no se puede garantizar que las bases de datos estén sincronizadas. Al forzar la conmutación por error se inicia una nueva bifurcación de recuperación. Puesto que la base de datos principal original y las bases de datos secundarias están en bifurcaciones de recuperación diferentes, cada una de ellas contiene ahora datos que las otras bases de datos no contienen: cada base de datos principal original contiene los cambios que aún no se han enviado desde su cola de envío a la base de datos secundaria anterior (registro sin enviar); las bases de datos secundarias anteriores contienen los cambios que se han producido después de forzar la conmutación por error.  
  
 Si la conmutación por error se fuerza debido a que la réplica principal no funcionó, la posible pérdida de datos depende de si se ha enviado o no algún registro de transacciones a la réplica secundaria antes del problema. En el modo de confirmación asincrónica, la acumulación del registro sin enviar siempre es una posibilidad. En modo de confirmación sincrónica, esto solo es posible hasta que las bases de datos secundarias estén sincronizadas.  
  
 En la tabla siguiente se resume la posibilidad de perder datos para una base de datos determinada en la réplica en la que se fuerza la conmutación por error.  
  
|Modo de disponibilidad de una réplica secundaria|¿Está sincronizada la base de datos?|¿Es posible que se pierdan datos?|  
|--------------------------------------------|-------------------------------|----------------------------|  
|Confirmación sincrónica|Sí|No|  
|Confirmación sincrónica|No|Sí|  
|Confirmación asincrónica|No|Sí|  
  
 Las bases de datos secundarias realizan el seguimiento solo en dos bifurcaciones de recuperación, por lo que, si se realizan varias conmutaciones por error forzadas, no podrá reanudarse ninguna de las bases de datos secundarias que comenzaron la sincronización de datos con la conmutación por error forzada anterior. Si esto se produce, las bases de datos secundarias que no puedan reanudarse tendrán que quitarse del grupo de disponibilidad, restaurarse en el punto de tiempo preciso y unirse de nuevo al grupo de disponibilidad. En este escenario, se puede observar el error 1408 con el estado 103 (error: 1408, gravedad: 16, estado: 103). Una restauración no funcionará entre varias bifurcaciones de recuperación; por tanto, asegúrese de realizar una copia de seguridad de registros después de realizar varias conmutaciones por error forzadas.  
  
###  <a name="why-forced-failover-is-required-after-forcing-quorum"></a><a name="WhyFFoPostForcedQuorum"></a> Por qué se requiere la conmutación por error después de forzar el quórum  
 Después de forzar el cuórum en el clúster WSFC (*cuórum forzado*), debe realizarse una conmutación por error forzada (con posible pérdida de datos) en todos los grupos de disponibilidad. La conmutación por error forzada es necesaria porque el estado real de los valores del clúster WSFC puede haberse perdido. Es necesario evitar las conmutaciones por error normales después de un quórum forzado debido a la posibilidad de que una réplica secundaria no sincronizada parezca sincronizada en el clúster WSFC reconfigurado.  
  
 Por ejemplo, considere la posibilidad de un clúster WSFC que hospeda un grupo de disponibilidad en tres nodos:  el nodo A hospeda la réplica principal y los nodos B y C hospedan una réplica secundaria cada uno. El nodo C se desconecta del clúster WSFC mientras la réplica secundaria local se sincroniza.  Pero los nodos A y B conservan un quórum correcto y el grupo de disponibilidad sigue en línea. En el nodo A, la réplica principal continúa aceptando actualizaciones y, en el nodo B, la réplica secundaria continúa sincronizándose con la réplica principal. La réplica secundaria del nodo C se desincroniza y se encuentra cada vez más retrasada respecto de la réplica principal. Sin embargo, debido a que el nodo C está desconectado, la réplica permanece incorrectamente en el estado SYNCHRONIZED.  
  
 Si se pierde el quórum y después se fuerza en el nodo A, el estado de sincronización del grupo de disponibilidad del clúster WSFC debería ser correcto y la réplica secundaria del nodo C debería mostrarse como UNSYNCHRONIZED. Sin embargo, si se fuerza el quórum en el nodo C, la sincronización del grupo de disponibilidad será incorrecta. El estado de sincronización del clúster se habrá revertido al estado que tenía cuando el nodo C estaba desconectado y la réplica secundaria del nodo C se mostraría *incorrectamente* como SYNCHRONIZED. Dado que las conmutaciones por error manuales planeadas garantizan la seguridad de los datos, no están permitidas para poner un grupo de disponibilidad de nuevo en línea después de forzar el quórum.  
  
###  <a name="tracking-potential-data-loss"></a><a name="TrackPotentialDataLoss"></a> Seguimiento de la posible pérdida de datos  
 Cuando el clúster WSFC tiene un quórum correcto, se puede calcular el potencial actual de pérdida de datos en las bases de datos. Para una réplica secundaria dada, el potencial actual de pérdida de datos depende de lo retrasadas que estén las bases de datos secundarias locales respecto de las bases de datos principales correspondientes. Debido a que la cantidad de retardo varía con el tiempo, se recomienda que realice un seguimiento periódico del potencial de pérdida de datos de las bases de datos secundarias no sincronizadas. El seguimiento del retardo implica comparar el Último LSN de confirmación y la Última hora de confirmación de cada base de datos principal y de sus bases de datos secundarias, de la forma siguiente:  
  
1.  Conéctese a la réplica principal.  
  
2.  Consulte las columnas **last_commit_lsn** (LSN de la última transacción confirmada) y **last_commit_time** (hora de la última confirmación) de la vista de administración dinámica [sys.dm_hadr_database_replica_states](../../../relational-databases/system-dynamic-management-views/sys-dm-hadr-database-replica-states-transact-sql.md) .  
  
3.  Compare los valores devueltos para cada base de datos principal y cada una de sus bases de datos secundarias. La diferencia entre sus últimos LSN de confirmación indican el tiempo de retardo.  
  
4.  Puede desencadenar una alerta cuando el tiempo de retardo en una base de datos o conjunto de base de datos supere el retardo máximo deseado durante un período de tiempo determinado. Por ejemplo, un trabajo que se ejecute cada minuto en cada base de datos principal puede realizar la consulta. Si la diferencia entre el valor de **last_commit_time** de una base de datos principal y cualquiera de sus bases de datos secundarias ha superado el objetivo de punto de recuperación (RPO) (por ejemplo, 5 minutos) desde la última vez que se ejecutó el trabajo, este puede generar una alerta.  
  
> [!IMPORTANT]  
>  Cuando el clúster de WSFC carece de cuórum o cuando este último se ha forzado, **last_commit_lsn** y **last_commit_time** tienen el valor NULL. Para obtener información sobre cómo podría evitar la pérdida de datos después de forzar el cuórum, vea "Formas posibles de evitar la pérdida de datos después de forzar el cuórum" en [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
###  <a name="managing-the-potential-data-loss"></a><a name="ForcedFailoverManagingDataLoss"></a> Administrar la potencial pérdida de datos  
 Después de forzar una conmutación por error, se suspenden todas las bases de datos secundarias. Esto incluye las bases de datos principales antiguas, después de que la réplica principal anterior vuelva a estar en línea y detecte que ahora es una réplica secundaria. Debe reanudar manualmente y de forma individual cada una de las bases de datos suspendidas en cada réplica secundaria.  
  
 Una vez que esté disponible la réplica principal anterior y en el supuesto de que sus bases de datos no estén dañadas, se puede intentar administrar la potencial pérdida de datos. El enfoque disponible para administrar la posible pérdida de datos depende de si la réplica principal original se ha conectado a la nueva réplica principal. Suponiendo que la réplica principal original pueda tener acceso a la nueva instancia principal, la reconexión se produce de forma automática y transparente.  
  
#### <a name="the-original-primary-replica-has-reconnected"></a>La réplica principal original se ha vuelto a conectar  
 Generalmente, después de un problema, cuando la réplica principal original se reinicia, se vuelve a conectar rápidamente a su asociado. Al volver a conectarse, la réplica principal original se convierte en la réplica secundaria. Las bases de datos se convierten en bases de datos secundarias y entran en estado SUSPENDED. Las nuevas bases de datos secundarias no se no se revertirán a menos que se reanuden.  
  
 Sin embargo, las bases de datos suspendidas son inaccesibles; por lo tanto, no se pueden inspeccionar para evaluar qué datos se perderían si se reanudara una base de datos dada. Por lo tanto, la decisión de si ha de reanudar o quitar una base de datos secundaria depende de si se está dispuesto a aceptar una pérdida de datos, del siguiente modo:  
  
-   Si una pérdida de datos es inaceptable, debe quitar las bases de datos del grupo de disponibilidad para protegerlas.  
  
     El administrador de base de datos puede ahora recuperar las bases de datos principales anteriores e intentar recuperar los datos que se habrían perdido. Sin embargo, cuando una base de datos principal anterior se pone en línea, es divergente de la base de datos principal actual, por lo que el administrador de base de datos debe hacer que la base de datos quitada o la base de datos principal actual no sea accesible a los clientes para evitar la divergencia posterior de las bases de datos y los problemas de conmutación por error del cliente.  
  
-   Si la pérdida de datos es aceptable para sus objetivos empresariales, puede reanudar las bases de datos secundarias.  
  
     Al reanudar una nueva base de datos secundaria se produce su reversión como primer paso en su sincronización. Si alguna entrada del registro estuviera esperando en la cola de envío en el momento del problema, se perderían las transacciones correspondientes, incluso si se hubieran confirmado.  
  
#### <a name="the-original-primary-replica-has-not-reconnected"></a>La réplica principal original no se ha vuelto a conectar  
 Si puede impedir temporalmente que la réplica principal original se vuelva a conectar a través de la red a la nueva réplica principal, puede inspeccionar las bases de datos principales originales para evaluar qué datos se perderían si se reanudaran.  
  
-   Si la posible pérdida de datos es aceptable  
  
     Permita que la réplica principal original se vuelva a conectar a la nueva réplica principal. La acción de volver a conectarse hace que las nuevas bases de datos secundarias se suspendan. Para iniciar la sincronización de datos en una base de datos, solo tiene que reanudarla. La nueva réplica secundaria quita la bifurcación de recuperación original para esa base de datos, con lo que se pierden las transacciones que no se han enviado nunca a la réplica secundaria anterior o no se han recibido de ella.  
  
-   Si la pérdida de datos es inaceptable  
  
     Si la base de datos principal original contiene datos esenciales que se perderían si se reanudara la base de datos suspendida, puede preservar los datos de la base de datos principal original quitándola del grupo de disponibilidad. Esto hace que la base de datos entre en estado RESTORING. Llegados a esta situación, se recomienda intentar realizar una copia de seguridad del final del registro de la base de datos quitada. Después, puede actualizar la base de datos principal actual (base de datos secundaria anterior) exportando los datos que desea proteger de la base de datos principal original e importándolos a la base de datos principal actual. Se recomienda realizar una copia de seguridad completa de la base de datos principal actualizada tan pronto como sea posible.  
  
     A continuación, en la instancia del servidor que hospeda la nueva réplica secundaria, puede eliminar la base de datos secundaria suspendida y crear una nueva base de datos secundaria restaurando esta copia de seguridad (y al menos una copia de seguridad de registros posterior) utilizando RESTORE WITH NORECOVERY. Se recomienda retrasar la realización de copias de seguridad de registros adicionales de las bases de datos principales actuales hasta que se reanuden las bases de datos secundarias correspondientes.  
  
> [!WARNING]  
>  El truncamiento del registro de transacciones se retrasa en una base de datos principal mientras cualquiera de sus bases de datos secundarias está suspendida. Además, el estado de sincronización de una réplica secundaria de confirmación sincrónica no puede pasar a HEALTHY siempre que alguna base de datos local se suspende.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para configurar el comportamiento de la conmutación por error**  
  
-   [Cambiar el modo de disponibilidad de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-availability-mode-of-an-availability-replica-sql-server.md)  
  
-   [Cambiar el modo de conmutación por error de una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/change-the-failover-mode-of-an-availability-replica-sql-server.md)  
  
-   [Configurar la directiva de conmutación por error flexible para controlar las condiciones de la conmutación automática por error &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/configure-flexible-automatic-failover-policy.md)  
  
 **Para realizar una conmutación por error manual**  
  
-   [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)  
  
-   [Usar el Asistente para grupo de disponibilidad de conmutación por error &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-fail-over-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Administración de inicios de sesión y de trabajos para las bases de datos de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/logins-and-jobs-for-availability-group-databases.md)  
  
 **Para establecer la configuración de quórum de WSFC**  
  
-   [Configurar los valores de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/configure-cluster-quorum-nodeweight-settings.md)  
  
-   [Ver la configuración de NodeWeight de cuórum de clúster](../../../sql-server/failover-clusters/windows/view-cluster-quorum-nodeweight-settings.md)  
  
-   [Forzar el inicio de un clúster WSFC sin un quórum](../../../sql-server/failover-clusters/windows/force-a-wsfc-cluster-to-start-without-a-quorum.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> Contenido relacionado  
  
-   [Guía de soluciones AlwaysOn de Microsoft SQL Server para lograr alta disponibilidad y recuperación ante desastres](/previous-versions/sql/sql-server-2012/hh781257(v=msdn.10))  
  
-   [Blog del equipo Always On de SQL Server: el blog oficial del equipo de Always On de SQL Server](/archive/blogs/sqlalwayson/)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Clústeres de conmutación por error de Windows Server &#40;WSFC&#41; con SQL Server](../../../sql-server/failover-clusters/windows/windows-server-failover-clustering-wsfc-with-sql-server.md)   
 [Transacciones entre bases de datos y transacciones distribuidas para la creación de reflejo de la base de datos o grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/transactions-always-on-availability-and-database-mirroring.md)   
 [Directiva de conmutación por error para instancias de clústeres de conmutación por error](../../../sql-server/failover-clusters/windows/failover-policy-for-failover-cluster-instances.md)   
 [Directiva de conmutación por error flexible para conmutación automática por error de un grupo de disponibilidad &#40;SQL Server&#41;](./configure-flexible-automatic-failover-policy.md)  
  
