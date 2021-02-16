---
title: Conmutación por error de un grupo de disponibilidad
description: En este tema se describe cómo realizar una conmutación por error manual forzada o planeada de un grupo de disponibilidad Always On mediante SQL Server Management Studio, Transact-SQL o SQL PowerShell.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: conceptual
f1_keywords:
- sql13.swb.failoverwizard.connecttoreplicas.f1
- sql13.swb.failoverwizard.progress.f1
- sql13.swb.failoverwizard.selectnewprimary.f1
- sql13.swb.failoverwizard.f1
- sql13.swb.failoverwizard.confirmdataloss.f1
helpviewer_keywords:
- failover [SQL Server], failover
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], configuring
ms.assetid: 4a602584-63e4-4322-aafc-5d715b82b834
author: cawrites
ms.author: chadam
ms.openlocfilehash: c432d109bebee0a7a1574018e50f03b1c99127aa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355141"
---
# <a name="use-the-fail-over-availability-group-wizard-sql-server-management-studio"></a>Usar el Asistente para grupo de disponibilidad de conmutación por error (SQL Server Management Studio)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo realizar una conmutación por error manual planeada o conmutación por error manual forzada (conmutación por error forzada) en un grupo de disponibilidad AlwaysOn mediante [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../../includes/tsql-md.md)]o PowerShell en [!INCLUDE[ssnoversion](../../../includes/ssnoversion-md.md)]. Un grupo de disponibilidad realiza la conmutación por error en el nivel de réplica de disponibilidad. Si se realiza una conmutación por error a una réplica secundaria en el estado SYNCHRONIZED, el asistente realiza una conmutación por error manual planeada (sin pérdida de datos). Si se realiza una conmutación por error a una réplica secundaria en el estado UNSYNCHRONIZED o NOT SYNCHRONIZING, el asistente realiza una conmutación por error manual forzada, también denominada *conmutación por error forzada* (con posible pérdida de datos). En ambas formas de conmutación por error manual tiene lugar la transición a la réplica secundaria a la que se está conectado al rol principal. Una conmutación por error manual planeada actualmente realiza transacciones de la réplica principal anterior al rol secundario. Después de una conmutación por error forzada, cuando la réplica principal pasa a estar en línea, realiza la transición al rol secundario.  

##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 Antes de la primera conmutación por error manual planeada, consulte la sección "Antes de comenzar" de [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md).  
  
 Antes de realizar la primera conmutación por error forzada, vea las secciones "Antes de empezar" y "Seguimiento: tareas esenciales después de una conmutación por error forzada" de [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Un comando de conmutación por error realiza la devolución en cuanto la réplica secundaria de destino haya aceptado el comando. Sin embargo, la recuperación de la base de datos se produce de forma asincrónica cuando el grupo de disponibilidad ha terminado la conmutación por error.  
    
###  <a name="prerequisites-for-using-the-failover-availability-group-wizard"></a><a name="Prerequisites"></a> Requisitos previos para utilizar el Asistente para conmutación por error del grupo de disponibilidad  
  
-   Debe estar conectado a la instancia del servidor que hospeda una réplica de disponibilidad que esté actualmente disponible.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 **Para usar el Asistente para conmutación por error del grupo de disponibilidad**  
  
1.  En el Explorador de objetos, conéctese a la instancia de servidor que hospede una réplica secundaria del grupo de disponibilidad que necesita ser objeto de conmutación por error y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Para iniciar el Asistente para conmutación por error del grupo de disponibilidad, haga clic con el botón derecho en el grupo de disponibilidad al que va a realizar la conmutación por error y seleccione **Conmutación por error**.  
  
4.  La información que muestra la página **Introducción** depende de si la réplica secundaria es válida para una conmutación por error planeada. Si en esta página pone "**Realice una conmutación por error planeada para este grupo de disponibilidad**", puede realizar la conmutación por error en el grupo de disponibilidad sin perder datos.  
  
5.  En la página **Seleccione la nueva réplica principal** , puede ver el estado de la réplica principal actual y del cuórum de WSFC antes de elegir la réplica secundaria que se convertirá en la nueva réplica principal (el *destino de conmutación por error*). Para una conmutación por error manual planeada, asegúrese de seleccionar una réplica secundaria cuyo valor de **Preparación para la conmutación por error** sea "**No se produce pérdida de datos**". En el caso de una conmutación por error forzada, para todos los destinos posibles de la conmutación por error, este valor será "**Pérdida de datos, Advertencias(** _#_ **)** ", donde *#* indica el número de advertencias que existe para una réplica secundaria concreta. Para ver las advertencias para un destino de conmutación por error determinado, haga clic en su valor "Preparación para la conmutación por error".  
  
     Para obtener más información, vea la página [Seleccionar la nueva réplica principal](#SelectNewPrimaryReplica), más adelante en este tema.  
  
6.  En la página de **Conectar a la réplica** , realice la conexión con el destino de conmutación por error. Para obtener más información, vea la página [Conectase a la réplica](#ConnectToReplica)más adelante en este tema.  
  
7.  Si realiza una conmutación por error forzada, el asistente muestra la página **Confirmar la página de la posible pérdida de datos** . Para continuar con la conmutación por error, debe seleccionar **Haga clic aquí para confirmar la conmutación por error con posibles pérdidas de datos**. Para obtener más información, vea[Confirmar la posible pérdida de datos](#ConfirmPotentialDataLoss), más adelante en este tema.  
  
8.  En la página **Resumen** , revise las implicaciones de la conmutación por error para la réplica secundaria seleccionada.  
  
     Si está satisfecho con las selecciones, puede hacer clic en **Script** para crear un script de los pasos que ejecutará el asistente. A continuación, para realizar la conmutación por error del grupo de disponibilidad a la réplica secundaria seleccionada, haga clic en **Finalizar**.  
  
9. La página **Progreso** muestra el progreso de la conmutación por error sobre el grupo de disponibilidad.  
  
10. Cuando finaliza la operación de conmutación por error, la página **Resultados** muestra el resultado. Una vez completado el asistente, haga clic en **Cerrar** para salir.  
  
     Para obtener más información, vea [Página Resultados &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md).  
  
11. Después de una conmutación por error forzada, vea la sección "Seguimiento: después de una conmutación por error forzada" de [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md).  
  
## <a name="help-for-pages-that-are-exclusive-to-this-wizard"></a>Ayuda para las páginas que son exclusivas para este asistente  
 En esta sección se describen las páginas que son únicas para [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)].  
  
 **En esta sección**  
  
-   [Página Seleccionar la nueva réplica principal](#SelectNewPrimaryReplica)  
  
-   [Página Conectar a la réplica](#ConnectToReplica)  
  
-   [Página Confirmar la posible pérdida de datos](#ConfirmPotentialDataLoss)  
  
 Las otras páginas de este asistente comparten la ayuda con uno o varios de los otros asistentes Grupos de disponibilidad AlwaysOn y se documentan en los temas de la Ayuda F1 independientes.  
  
###  <a name="select-new-primary-replica-page"></a><a name="SelectNewPrimaryReplica"></a> Select New Primary Replica Page  
 En esta sección se describen las opciones de la página de **Seleccionar la nueva réplica principal** . Utilice esta página para seleccionar la réplica secundaria (destino de conmutación por error) para la que el grupo de disponibilidad realizará la conmutación por error. Esta réplica se convertirá en la nueva réplica principal.  
  
#### <a name="page-options"></a>Opciones de página  
 **Réplica principal actual**  
 Muestra el nombre de la réplica principal actual, si está en línea.  
  
 **Estado de la réplica principal**  
 Muestra el estado de la réplica principal actual, si está en línea.  
  
 **Estado de quórum**  
 Para el tipo de clúster WSFC, muestra uno de los siguientes estados de cuórum de la réplica de disponibilidad:  
  
   |Value|Descripción|  
   |-----------|-----------------|  
   |**Quórum normal**|El clúster se ha iniciado con quórum normal.|  
   |**Quórum forzado**|El clúster se ha iniciado con quórum forzado.|  
   |**Quórum desconocido**|El estado de quórum del clúster no está disponible.|  
   |**No aplicable**|El nodo que hospeda la réplica de disponibilidad no tiene quórum.|  
  
 Para obtener más información, vea [Configuración de los votos y modos de cuórum WSFC &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-quorum-modes-and-voting-configuration-sql-server.md).  

 Para el tipo de clúster NONE, el estado de cuórum no procede.

 Para el tipo de clúster EXTERNAL, el estado de cuórum lo administra el Administrador de clústeres y no es visible en SQL Server.
  
 **Elegir una nueva réplica principal**  
 Use esta cuadrícula para seleccionar una réplica secundaria que se convertirá en la nueva réplica principal. Las columnas de esta cuadrícula son las siguientes:  
  
 **Instancia del servidor**  
 Muestra el nombre de la instancia del servidor que hospeda una réplica secundaria.  
  
 **Modo de disponibilidad**  
 Muestra el modo de disponibilidad de la instancia del servidor, que será uno de los siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Confirmación sincrónica**|En modo de confirmación sincrónica, antes de la confirmación de transacciones, una réplica principal de confirmación sincrónica espera a que una réplica secundaria de confirmación sincrónica notifique que ha terminado de proteger el registro. El modo de confirmación sincrónica asegura que, una vez que una base de datos secundaria se sincroniza con la base de datos principal, las transacciones confirmadas queden totalmente protegidas.|  
|**Confirmación asincrónica**|En modo de confirmación asincrónica, la réplica principal confirma las transacciones sin esperar la notificación de que una réplica secundaria de confirmación asincrónica ha protegido el registro. El modo de confirmación asincrónica minimiza la latencia de las transacciones en las bases de datos secundarias pero permite que se retrasen detrás de las bases de datos principales, haciendo posible alguna pérdida de datos.|  
  
 Para obtener más información, vea [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md).  
  
 **Modo de conmutación por error**  
 Muestra el modo de conmutación por error de la instancia del servidor, que será uno de los siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**Automático**|Una réplica secundaria que está configurada para la conmutación automática por error también admite la conmutación por error manual planeada, siempre que la réplica secundaria esté sincronizada con la réplica principal.|  
|**Manual**|Existen dos tipos de conmutación por error manual: planeada (sin pérdida de datos) y forzada (con posible pérdida de datos). Para una réplica secundaria determinada, solo se admite una de ellas, dependiendo del modo de disponibilidad y, para el modo de confirmación sincrónica, el estado de sincronización de la réplica secundaria. Para determinar qué forma de conmutación por error manual admite actualmente una réplica secundaria determinada, vea la columna **Preparación para la conmutación por error** de esta cuadrícula.|  
  
 Para obtener más información, vea [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md).  
  
 **Preparación para la conmutación por error**  
 Muestra la preparación para la conmutación por error de la réplica secundaria, una de las siguientes:  
  
|Value|Descripción|  
|-----------|-----------------|  
|**No se produce pérdida de datos**|Esta réplica secundaria actualmente admite la conmutación por error planeada. Este valor solo aparece cuando una réplica secundaria en modo de confirmación sincrónica está sincronizada con la réplica principal.|  
|**Pérdida de datos, Advertencias(** _#_ **)**|Esta réplica secundaria admite actualmente conmutación por error forzada (con posible pérdida de datos). Este valor tiene lugar siempre que la réplica secundaria no esté sincronizada con la réplica principal. Haga clic en el vínculo de advertencias de la pérdida de datos para obtener información sobre la posible pérdida de datos.|  
  
 **Actualizar**  
 Haga clic para actualizar la cuadrícula.  
  
 **Cancelar**  
 Haga clic en esta opción para finalizar el asistente. Al cancelar el asistente se sale de la página **Seleccionar la nueva réplica principal** sin realizar ninguna acción.  
  
###  <a name="confirm-potential-data-loss-page"></a><a name="ConfirmPotentialDataLoss"></a> Confirm Potential Data Loss Page  
 Esta sección describe las opciones de la página **Confirmar la posible pérdida de datos** , que se muestra solo si se realiza una conmutación por error forzada. Este tema solo lo usa [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]. Utilice esta página para indicar si está dispuesto a arriesgar una posible pérdida de datos a fin de forzar el grupo de disponibilidad para realizar una conmutación por error.  
  
#### <a name="confirm-potential-data-loss-options"></a>Opciones para confirmar la posible pérdida de datos  
 Si la réplica secundaria seleccionada no está sincronizada con la réplica principal, el asistente muestra una advertencia que realiza una conmutación por error a esta réplica secundaria y podría provocar la pérdida de datos en una o varias bases de datos.  
  
 **Haga clic aquí para confirmar la conmutación por error con posible pérdida de datos.**  
 Si acepta el riesgo de la pérdida de datos a fin de que estén disponibles las bases de datos de este grupo de disponibilidad para los usuarios, active esta casilla. Si está dispuesto a arriesgar la pérdida de datos, puede hacer clic en **Anterior** para volver a la página **Seleccionar la nueva réplica principal** o hacer clic en **Cancelar** para salir del asistente sin conmutación por error sobre el grupo de disponibilidad.  
  
 **Cancelar**  
 Haga clic en esta opción para finalizar el asistente. Al cancelar el asistente se sale de la página **Confirmar la posible pérdida de datos** sin realizar ninguna acción.  
  
###  <a name="connect-to-replica-page"></a><a name="ConnectToReplica"></a> Connect to Replica Page  
 En esta sección se describen las opciones de la página **Conectar a la réplica** de [!INCLUDE[ssAoFoAgWiz](../../../includes/ssaofoagwiz-md.md)]. Esta página solo se muestra si no hay conexión con la réplica secundaria de destino. Utilice esta página para conectarse a la réplica secundaria que está seleccionada como la nueva réplica principal.  
  
#### <a name="page-options"></a>Opciones de página  
 **Columnas de cuadrícula:**  
 **Instancia del servidor**  
 Muestra el nombre de la instancia del servidor que hospedará la réplica de disponibilidad.  
  
 **Conectado como**  
 Muestra la cuenta que está conectada a la instancia de servidor, una vez que se ha establecido la conexión. Si esta columna muestra "**Sin conectar**" para una instancia de servidor determinada, deberá hacer clic en el botón **Conectar** .  
  
 **Conexión**  
 Haga clic aquí si esta instancia de servidor se ejecuta en una cuenta diferente a las de otras instancias de servidor a las que necesite conectarse.  
  
 **Cancelar**  
 Haga clic en esta opción para finalizar el asistente. Al cancelar el asistente se sale de la página **Conectar a la réplica** sin realizar ninguna acción.  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Modos de disponibilidad &#40;grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/availability-modes-always-on-availability-groups.md)   
 [Conmutación por error y modos de conmutación por error &#40;Grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/failover-and-failover-modes-always-on-availability-groups.md)   
 [Realizar una conmutación por error manual planeada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-planned-manual-failover-of-an-availability-group-sql-server.md)   
 [Realizar una conmutación por error manual forzada de un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/perform-a-forced-manual-failover-of-an-availability-group-sql-server.md)   
 [Recuperación ante desastres del clúster WSFC mediante cuórum forzado &#40;SQL Server&#41;](../../../sql-server/failover-clusters/windows/wsfc-disaster-recovery-through-forced-quorum-sql-server.md)  
  
