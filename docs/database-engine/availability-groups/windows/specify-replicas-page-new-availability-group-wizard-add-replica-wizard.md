---
title: 'Asistente para grupos de disponibilidad: Página Especificar réplicas'
description: Describe las opciones de la página Especificar réplicas del Asistente para nuevo grupo de disponibilidad de SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.newagwizard.listeners.f1
- sql13.swb.addreplicawizard.specifyreplicas.f1
- sql13.swb.newagwizard.specifyreplicas.f1
ms.assetid: 2d90fc12-a67b-4bd0-b0ab-899b73017196
author: cawrites
ms.author: chadam
ms.openlocfilehash: 4a0c17cc7e7118ec2ac39e861741dcadaa892a93
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342420"
---
# <a name="specify-replicas-page-new-availability-group-wizard-add-replica-wizard"></a>Página Especificar réplicas (Asistente para nuevo grupo de disponibilidad: Asistente para agregar réplica)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describen las opciones de la página **Especificar réplicas** . Esta página se aplica a **[!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)]** y a **[!INCLUDE[ssAoAddRepWiz](../../../includes/ssaoaddrepwiz-md.md)]** . Use la página **Especificar réplicas** para especificar y configurar una o varias réplicas de disponibilidad para agregar al grupo de disponibilidad. Esta página contiene cuatro pestañas, que se presentan en la tabla siguiente. Haga clic en el nombre de una pestaña de la tabla para ir a la sección correspondiente, más adelante en este tema.  
  
|Pestaña|Breve descripción|  
|---------|-----------------------|  
|[Réplicas](#ReplicasTab)|Utilice esta pestaña para especificar cada instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospedará u hospeda actualmente una réplica secundaria. Tenga en cuenta que la instancia de servidor a la que está conectado actualmente debe hospedar la réplica principal.<br /><br /> Termine de especificar todas las réplicas en la pestaña **Réplicas** antes de iniciar las demás pestañas.<br/><br/> Observe que la **conmutación automática por error** está deshabilitada si el tipo de clúster es **NONE**. SQL Server solo admite la conmutación por error manual cuando un grupo de disponibilidad no está en un clúster. <br/><br/> Si el tipo de clúster es EXTERNAL, el modo de conmutación por error es **Externo**. <br/><br/> Al agregar una réplica, todas las réplicas nuevas deben estar en el mismo tipo de sistema operativo que las réplicas existentes. <br/><br/>Al agregar una réplica, si la réplica principal se encuentra en un WSFC, las réplicas secundarias deben pertenecer al mismo clúster.|
|[Extremos](#EndpointsTab)|Utilice esta pestaña para comprobar los extremos de creación de reflejo de base de datos existentes y también, si este extremo falta en una instancia de servidor cuyas cuentas de servicio utilizan la autenticación de Windows, para crear el extremo automáticamente.|  
|[Preferencias de copia de seguridad](#BackupPreferencesTab)|Utilice esta pestaña para especificar sus preferencias de copias de seguridad para el grupo de disponibilidad en conjunto y las prioridades de copias de seguridad para las réplicas de disponibilidad individuales.|  
|[Agente de escucha](#Listener)|Utilice esta pestaña, si está disponible, para crear un agente de escucha del grupo de disponibilidad. De forma predeterminada, no se crea un agente de escucha.<br /><br /> Esta pestaña está disponible solo si se ejecuta [!INCLUDE[ssAoNewAgWiz](../../../includes/ssaonewagwiz-md.md)].<br/><br/>DHCP está deshabilitado si el tipo de clúster es EXTERNAL o NONE. |  
  
##  <a name="replicas-tab"></a><a name="ReplicasTab"></a> Pestaña réplicas  
 **Instancia del servidor**  
 Muestra el nombre de la instancia del servidor que hospedará la réplica de disponibilidad.  
  
 Si una instancia de servidor que tiene previsto usar para hospedar una réplica secundaria no aparece en la cuadrícula **Réplicas de disponibilidad**, haga clic en el botón **Agregar réplica**. Si configura un grupo de disponibilidad en un entorno de tecnologías de la información híbrido (consulte [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](/previous-versions/azure/jj870962(v=azure.100))), puede hacer clic en el botón **Agregar réplica de Azure** para crear máquinas virtuales con réplicas secundarias en Azure.  
  
 **Rol inicial**  
 Indica el rol que la nueva réplica realizará inicialmente: **Principal** o **Secundaria**.  
  
 **Conmutación por error automática (hasta 3)**  
 Active esta casilla solo si desea que esta réplica de disponibilidad sea un asociado de conmutación por error automática. Para configurar la conmutación por error automática, debe elegir esta opción para la réplica principal inicial y para una réplica secundaria. Estas dos réplicas utilizarán el modo de disponibilidad de confirmación sincrónica. Solo tres réplicas pueden admitir la conmutación por error automática.  
  
 Para más información sobre el modo de disponibilidad de confirmación sincrónica, vea [Modos de disponibilidad &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Para más información sobre la conmutación automática por error, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Confirmación sincrónica (hasta 3)**  
 Si ha seleccionado **Conmutación por error automática (hasta 3)** para la réplica, también se selecciona **Confirmación sincrónica (hasta 3)** . Si la casilla está en blanco, selecciónela solo si desea que esta réplica utilice el modo de confirmación sincrónica con solo la conmutación por error manual planeada. Solo tres réplicas pueden utilizar el modo de confirmación sincrónica.  
  
 Si desea que esta réplica utilice el modo de disponibilidad de confirmación asincrónica, deje en blanco esta casilla. La réplica solo admitirá la conmutación por error manual forzada (con posible pérdida de datos). Para más información sobre el modo de disponibilidad de confirmación asincrónica, vea [Modos de disponibilidad &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md). Para más información sobre la conmutación por error manual planeada y la conmutación por error manual forzada, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Rol secundario legible**  
 Seleccione un valor en la lista desplegable **Secundario legible** del siguiente modo:  
  
 **No**  
 No se permiten conexiones directas a las bases de datos secundarias de esta réplica. No están disponibles para acceso de lectura. Esta es la configuración predeterminada.  
  
 **Solo intento de lectura**  
 Únicamente se permiten conexiones directas de solo lectura a las bases de datos secundarias de esta réplica. Todas las bases de datos secundarias están disponibles para acceso de lectura.  
  
 **Sí**  
 Se permiten todas las conexiones a las bases de datos secundarias de esta réplica, pero solo para acceso de lectura. Todas las bases de datos secundarias están disponibles para acceso de lectura.  
  
 **Agregar réplica**  
 Haga clic para agregar una réplica secundaria al grupo de disponibilidad.  
  
 **Agregar réplica de Microsoft Azure**  
 Haga clic para crear una máquina virtual de Azure que ejecute una réplica secundaria en el grupo de disponibilidad. Esta opción solo se aplica a un grupo de disponibilidad en tecnologías de la información híbridas que contienen réplicas locales. Para obtener más información, vea [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](/previous-versions/azure/jj870962(v=azure.100)).  
  
 **Quitar réplica**  
 Haga clic para quitar la réplica secundaria seleccionada del grupo de disponibilidad.  
  
##  <a name="endpoints-tab"></a><a name="EndpointsTab"></a> Pestaña extremos  
 Para cada instancia del servidor que hospedará una réplica de disponibilidad, la pestaña **Extremos** muestra los valores reales del extremo de creación de reflejo de la base de datos existente, en su caso, o los valores sugeridos para un nuevo extremo posible que utilizaría la Autenticación de Windows. Tanto para los extremos existentes como para los posibles, la cuadrícula Valores de extremo muestra la información siguiente:  
  
 **Nombre de servidor**  
 Muestra el nombre de una instancia del servidor que hospedará una réplica de disponibilidad.  
  
 **Dirección URL del extremo**  
 Muestra la dirección URL real o propuesta del extremo de creación de reflejo de la base de datos. En el caso de un nuevo extremo propuesto, puede cambiar este valor. Para obtener más información sobre el formato de estas direcciones URL, vea [Especificar la dirección URL del punto de conexión al agregar o modificar una réplica de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md).  
  
 **Número de puerto**  
 Muestra el número de puerto real o propuesto del extremo. En el caso de un nuevo extremo propuesto, puede cambiar este valor.  
  
 **Nombre de extremo**  
 Muestra el nombre real o propuesto del extremo. En el caso de un nuevo extremo propuesto, puede cambiar este valor.  
  
 **Cifrar datos**  
 Indica si los datos enviados a través de este extremo están cifrados. En el caso de un nuevo extremo propuesto, puede cambiar este valor.  
  
 **Cuenta de servicio de SQL Server**  
 Nombre de usuario de la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
 Para que una instancia del servidor use un extremo que utilice la Autenticación de Windows, la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] debe ser una cuenta de dominio.  
  
 Este requisito determina el siguiente paso de configuración, del modo siguiente:  
  
-   Si cada instancia del servidor se ejecuta con una cuenta de servicio de dominio, es decir, si la columna **Cuenta de servicio de SQL Server** muestra una cuenta de servicio de dominio para cada instancia del servidor, haga clic en **Siguiente**.  
  
-   Si una instancia del servidor se ejecuta con una cuenta de servicio que no es de dominio, se debe realizar un cambio manual en la instancia del servidor antes de continuar con el asistente. En este caso, si hace clic en **Siguiente** , se muestra un cuadro de diálogo de advertencia; debe hacer clic en **No** y volverá a la pestaña **Extremos** . Permaneciendo en la página **Especificar réplicas** del asistente, realice uno de los siguientes cambios en cada instancia del servidor para que la columna **Cuenta de servicio de SQL Server** muestre una cuenta de servicio que no es de dominio; puede:  
  
    -   Usar el Administrador de configuración de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] para cambiar la cuenta de servicio de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] a una cuenta de dominio. Para obtener más información, vea [Cambiar la cuenta de inicio del servicio para SQL Server &#40;Administrador de configuración de SQL Server&#41;](../../../database-engine/configure-windows/scm-services-change-the-service-startup-account.md).  
  
    -   Use [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell para crear manualmente un extremo de creación de reflejo de la base de datos que use un certificado. Para obtener más información, vea [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md) o [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md).  
  
     Si deja la página **Especificar réplicas de disponibilidad** abierta mientras configura los extremos, vuelva a la pestaña **Extremos** y haga clic en **Actualizar** para actualizar la cuadrícula **Valores de extremo** .  
  
##  <a name="backup-preferences-tab"></a><a name="BackupPreferencesTab"></a> Pestaña Preferencias de copia de seguridad  
 Para especificar dónde deben producirse las copias de seguridad, elija una de las opciones siguientes:  
  
 **Preferir secundaria**  
 Especifica que las copias de seguridad se deben realizar en una réplica secundaria a menos que la réplica principal sea la única réplica en línea. En ese caso, la copia de seguridad se debe realizar en la réplica principal. Ésta es la opción predeterminada.  
  
 **Solo secundaria**  
 Especifica que las copias de seguridad no deben realizarse nunca en la réplica principal. Si la réplica principal es la única réplica en línea, no se debe realizar la copia de seguridad.  
  
 **Principal**  
 Especifica que las copias de seguridad deben realizarse siempre en la réplica principal. Esta opción es útil si necesita usar características de copia de seguridad, como crear copias de seguridad diferenciales, que no se admiten cuando la copia de seguridad se ejecuta en una réplica secundaria.  
  
 **Cualquier réplica**  
 Especifica que, de acuerdo con sus preferencias, los trabajos de copia de seguridad omitan el rol de las réplicas de disponibilidad cuando la réplica realiza copias de seguridad. Tenga en cuenta que los trabajos de copia de seguridad pueden evaluar otros factores, como la prioridad de copia de seguridad de cada réplica de disponibilidad junto con su estado operativo y de conexión.  
  
> [!IMPORTANT]  
>  No se aplica el valor de preferencia de copia de seguridad. La interpretación de esta preferencia depende de la lógica, si existe, del script con los trabajos de copia de seguridad ejecutado para las bases de datos de un grupo de disponibilidad dado. Para más información, consulte [Secundarias activas: copia de seguridad en las réplicas secundarias &#40;grupos de disponibilidad Always On&#41;](../../../database-engine/availability-groups/windows/active-secondaries-backup-on-secondary-replicas-always-on-availability-groups.md).  
  
### <a name="replica-backup-priorities-grid"></a>Cuadrícula de prioridades de copia de seguridad de réplica  
 Use la cuadrícula **Prioridades de copia de seguridad de réplica** para especificar sus prioridades de copia de seguridad para cada una de las réplicas del grupo de disponibilidad. Esta cuadrícula contiene las columnas siguientes:  
  
 **Instancia del servidor**  
 Muestra el nombre de la instancia de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospeda la réplica de disponibilidad.  
  
 **Prioridad de copia de seguridad (Mínima=1, Máxima=100)**  
 Asigne la prioridad para realizar copias de seguridad en esta réplica en relación con las otras réplicas del mismo grupo de disponibilidad. El valor predeterminado es 50. Puede seleccionar cualquier otro entero en el rango de 0 a 100. 1 indica la prioridad mínima y 100 indica la prioridad máxima. Si establece la **Prioridad de copia de seguridad** en 1, se elegirá que la réplica de disponibilidad realice copias de seguridad solo si no hay disponible actualmente una réplica de disponibilidad con prioridad máxima.  
  
 **Excluir réplica**  
 Para evitar que esta réplica de disponibilidad se elija nunca para realizar copias de seguridad. Esto es útil, por ejemplo, para una réplica de disponibilidad remota en la que no desee nunca realizar la conmutación por error para las copias de seguridad.  
  
##  <a name="listener-tab"></a><a name="Listener"></a> Pestaña Agente de escucha  
 Especifique sus preferencias para una[escucha de grupo de disponibilidad](../../../database-engine/availability-groups/windows/listeners-client-connectivity-application-failover.md)que proporcionará un punto de conexión de cliente; puede ser:  
  
 **No cree ahora un agente de escucha del grupo de disponibilidad.**  
 Seleccione esta opción para omitir este paso. Puede crear un agente de escucha más adelante. Para obtener más información, vea [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md).  
  
 **Crea un agente de escucha del grupo de disponibilidad.**  
 Especifique sus preferencias de agente de escucha para este grupo de disponibilidad del siguiente modo:  
  
 **Nombre DNS del agente de escucha**  
 Especifique el nombre de red del agente de escucha. Este nombre debe ser único en el dominio y solo puede contener caracteres alfanuméricos, guiones ( **-** ) y guiones bajos ( **_** ) en cualquier orden. Cuando se especifica mediante la pestaña **Agente de escucha** , el nombre DNS puede tener hasta 15 caracteres.  
  
> [!IMPORTANT]  
>  Si escribe un nombre de agente de escucha DNS (o número de puerto) no válido en la pestaña **Agente de escucha** , el botón **Siguiente** de la página **Especificar réplicas** estará deshabilitado.  
  
 **Puerto**  
 Especifique el puerto TCP usado por este agente de escucha.  
  
> [!NOTE]  
>  Si escribe un número de puerto (o nombre de agente de escucha DNS) no válido en la pestaña **Agente de escucha** , el botón **Siguiente** de la página **Especificar réplicas** estará deshabilitado.  
  
 **Modo de red**  
 Utilice la lista desplegable para seleccionar el modo de red que utilizará este agente de escucha; puede ser:  
  
 **Dirección IP estática**  
 Seleccione esta opción si desea que el agente de escucha escuche en más de una subred. Para utilizar el modo de red de dirección IP estática, un agente de escucha del grupo de disponibilidad debe escuchar en cada subred que hospeda una réplica de disponibilidad para el grupo de disponibilidad. Para cada subred, haga clic en **Agregar** para seleccionar una dirección de subred y especificar una dirección IP.  
  
 Si se selecciona **Dirección IP estática** como modo de red (esta es la selección predeterminada), una cuadrícula muestra las columnas **Subret** y **Dirección IP** , y se muestran los botones **Agregar** y **Quitar** asociados. La cuadrícula estará vacía hasta que se agregue la primera subred.  
  
 Columna **Subret**  
 Muestra la dirección de subred seleccionada para cada subred agregada para el agente de escucha.  
  
 Columna **Dirección IP**  
 Muestra la dirección IPv4 o IPv6 especificada para una subred determinada.  
  
 **Add (Agregar)**  
 Haga clic para agregar una subred a este agente de escucha. Se abrirá el cuadro de diálogo de **Agregar dirección IP** . Para obtener más información, vea el tema de ayuda [Agregar dirección IP &#40;cuadro de diálogo - SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/add-ip-address-dialog-box-sql-server-management-studio.md).  
  
 **Remove**  
 Haga clic para quitar la subred actualmente seleccionada en la cuadrícula.  
  
 **DHCP**  
 Seleccione esta opción si desea que el agente de escucha escuche en una única subred y use una dirección IPv4 dinámica asignada por un servidor que ejecuta el protocolo DHCP (Protocolo de configuración dinámica de host). DHCP está limitado a una sola subred que es común a cada instancia del servidor que hospeda una réplica de disponibilidad para el grupo de disponibilidad. DHCP no está disponible para los tipos de clúster EXTERNAL o NONE.  
  
> [!IMPORTANT]  
>  No se recomienda DHCP en el entorno de producción. Si hay un tiempo de inactividad y expira la concesión de IP para DHCP, se necesitará más tiempo para registrar la nueva dirección IP de red DHCP que está asociada con el nombre DNS de escucha, lo que puede afectar a la conectividad de cliente. Sin embargo, DHCP es adecuado para configurar el entorno de desarrollo y pruebas para comprobar características básicas de los grupos de disponibilidad y para la integración con las aplicaciones.  
  
 Cuando **DHCP** está seleccionado, se muestra el campo **Subred** .  
  
 **Subred**  
 Si selecciona **DHCP** como modo de red, use la lista desplegable **Subret** para seleccionar una dirección para la subred que hospeda las réplicas de disponibilidad del grupo de disponibilidad.  
  
> [!IMPORTANT]
>  Después de definir un agente de escucha del grupo de disponibilidad, es absolutamente recomendable que haga lo siguiente:  
> 
>  -   Pida al administrador de red que reserve la dirección IP del agente de escucha para su uso exclusivo. Proporcione el nombre del host DNS del agente de escucha a los desarrolladores de aplicaciones para que lo usen en las cadenas de conexión cuando soliciten conexiones cliente a este grupo de disponibilidad.  
> -   Proporcione el nombre del host DNS del agente de escucha a los desarrolladores de aplicaciones para que lo usen en las cadenas de conexión cuando soliciten conexiones cliente a este grupo de disponibilidad.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Usar el Asistente para grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Usar el Asistente para agregar una réplica al grupo de disponibilidad &#40;SQL Server Management Studio&#41;](../../../database-engine/availability-groups/windows/use-the-add-replica-to-availability-group-wizard-sql-server-management-studio.md)  
  
-   [Crear o configurar un agente de escucha de grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/create-or-configure-an-availability-group-listener-sql-server.md)  
  
-   [Usar certificados para un punto de conexión de creación de reflejo de la base de datos &#40;Transact-SQL&#41;](../../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
-   [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-endpoint-transact-sql.md)  
  
-   [Crear un punto de conexión de creación de reflejo de la base de datos para grupos de disponibilidad AlwaysOn &#40;SQL Server PowerShell&#41;](../../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [CREATE AVAILABILITY GROUP &#40;Transact-SQL&#41;](../../../t-sql/statements/create-availability-group-transact-sql.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)  
  
