---
title: Adición de una base de datos a un grupo de disponibilidad con el "Asistente para grupo de disponibilidad"
description: Agregue una base de datos a un grupo de disponibilidad Always On mediante el "Asistente para grupo de disponibilidad" desde SQL Server Management Studio.
ms.custom: seodec18
ms.date: 05/17/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: availability-groups
ms.topic: how-to
f1_keywords:
- sql13.swb.adddatabasewizard.f1
helpviewer_keywords:
- Availability Groups [SQL Server], wizards
- Availability Groups [SQL Server], databases
ms.assetid: 81e5e36d-735d-4731-8017-2654673abb88
author: cawrites
ms.author: chadam
ms.openlocfilehash: 0147846a3f7ec66295895812ac828a12c99d5632
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100340906"
---
# <a name="add-a-database-to-an-always-on-availability-group-with-the-availability-group-wizard"></a>Adición de una base de datos a un grupo de disponibilidad Always On con el "Asistente para grupo de disponibilidad"
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]
  Use el Asistente para agregar una base de datos al grupo de disponibilidad como ayuda para agregar una o varias bases de datos a un grupo de disponibilidad AlwaysOn existente.  
  
> [!NOTE]  
>  Para obtener información sobre cómo usar [!INCLUDE[tsql](../../../includes/tsql-md.md)] o PowerShell para agregar una base de datos, vea [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md).  
  

  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 Si nunca ha agregado una base de datos a un grupo de disponibilidad, vea la sección "Bases de datos de disponibilidad" en [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md).  
  
##  <a name="prerequisites-restrictions-and-recommendations"></a><a name="Prerequisites"></a> Requisitos previos, restricciones y recomendaciones  
  
-   Debe estar conectado a la instancia del servidor que hospeda la réplica principal actual.  
  
-   **Requisitos previos para usar la sincronización de datos inicial completa**  
  
    -   Todas las rutas de acceso de archivos de base de datos deben ser idénticas en todas las instancias de servidor que hospedan una réplica para el grupo de disponibilidad.  
  
    -   Ningún nombre de base de datos principal puede existir en ninguna instancia de servidor que hospede una réplica secundaria. Esto significa que ninguna de las nuevas bases de datos secundarias puede existir todavía.  
  
    -   Tendrá que especificar un recurso compartido de red para que el asistente cree y obtenga acceso a las copias de seguridad. Para la réplica principal, la cuenta usada para iniciar el [!INCLUDE[ssDE](../../../includes/ssde-md.md)] debe tener permisos del sistema de archivos de lectura y escritura en un recurso compartido de red. Para las réplicas secundarias, la cuenta debe tener permiso de lectura en el recurso compartido de red.  
  
     Si no puede utilizar el asistente para realizar la sincronización de datos inicial completa, debe preparar las bases de datos secundarias manualmente. Puede hacerlo antes o después de ejecutar el asistente. Para obtener más información, vea [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md).  
  
  
##  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Se requiere el permiso ALTER AVAILABILITY GROUP en el grupo de disponibilidad, el permiso CONTROL AVAILABILITY GROUP, el permiso ALTER ANY AVAILABILITY GROUP o el permiso CONTROL SERVER.  
  
##  <a name="use-the-new-availability-group-wizard"></a>Uso del "Asistente para nuevo grupo de disponibilidad"
  
1.  En el Explorador de objetos, conéctese a la instancia del servidor que hospeda la réplica principal del grupo de disponibilidad y expanda el árbol.  
  
2.  Expanda los nodos **Alta disponibilidad de AlwaysOn** y **Grupos de disponibilidad** .  
  
3.  Haga clic con el botón derecho en el grupo de disponibilidad al que está agregando una base de datos y seleccione el comando **Agregar base de datos** . Este comando inicia el Asistente para agregar una base de datos al grupo de disponibilidad.  
  
4.  En la página **Seleccionar bases de datos** , seleccione una o varias bases de datos. Para obtener más información, vea [Página Seleccionar bases de datos &#40;Asistente para nuevo grupo de disponibilidad/Asistente para agregar base de datos&#41;](../../../database-engine/availability-groups/windows/select-databases-page-new-availability-group-wizard-and-add-database-wizard.md).  
  
     Si la base de datos contiene una clave maestra de base de datos, escriba la contraseña para dicha clave en la columna **Contraseña** .  
  
5.  En la página **Seleccionar sincronización de datos iniciales** , elija cómo desea que las nuevas bases de datos secundarias se creen y se unan al grupo de disponibilidad. Elija una de las siguientes opciones:  

    - **Propagación automática**
      
      Seleccione esta opción para usar la propagación automática. La propagación automática emplea el transporte de secuencia de registro para transmitir la copia de seguridad con VDI a la réplica secundaria de cada base de datos del grupo de disponibilidad que use los puntos de conexión configurados. Esto restaura la copia de seguridad de la base de datos en la réplica secundaria sin tener que hacerlo manualmente. Para más información sobre la propagación automática, vea [Propagación automática](automatic-seeding-secondary-replicas.md).
  
    -   **Completa**  
  
         Seleccione esta opción si el entorno cumple los requisitos para iniciar automáticamente la sincronización de datos iniciales (para obtener más información, vea [Requisitos previos, restricciones y recomendaciones](#Prerequisites), anteriormente en este tema).  
  
         Si selecciona **Completa**, después de crear el grupo de disponibilidad, el asistente intentará realizar una copia de seguridad de cada base de datos principal y su registro de transacciones en un recurso compartido de red y restaurará las copias de seguridad en cada instancia de servidor que hospeda una réplica secundaria. El asistente unirá entonces cada base de datos secundaria al grupo de disponibilidad.  
  
         En el campo **Especificar una ubicación de red compartida accesible por todas las réplicas:** , especifique un recurso compartido de copia de seguridad al que todas las instancias del servidor que hospedan réplicas tengan acceso de lectura/escritura. Las copias de seguridad de registros serán parte de la cadena de copias de seguridad de registros. Almacene adecuadamente los archivos de copia de seguridad del registro.  
  
        > [!IMPORTANT]  
        >  Para obtener información sobre los permisos necesarios del sistema de archivos, vea [Requisitos previos](#Prerequisites), anteriormente en este tema.  
  
    -   **Solo unión**  
  
         Si ha preparado manualmente las bases de datos secundarias de las instancias de servidor que hospedarán las réplicas secundarias, puede seleccionar esta opción. El asistente unirá las bases de datos secundarias existentes al grupo de disponibilidad.  
  
    -   **Omitir la sincronización de datos iniciales**  
  
         Seleccione esta opción si desea usar sus propias bases de datos y copias de seguridad de registros de sus bases de datos principales. Para obtener más información, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
     Para obtener más información, vea [Página Seleccionar sincronización de datos iniciales &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/select-initial-data-synchronization-page-always-on-availability-group-wizards.md).  
  
6.  En la página **Conectar con réplicas secundarias existentes** , si las instancias de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que hospedan las réplicas de disponibilidad para este grupo de disponibilidad se ejecutan todas como un servicio en la misma cuenta de usuario, haga clic en **Conectar todas**. Si alguna instancia de servidor se ejecuta como un servicio bajo diferentes cuentas, haga clic en el botón **Conectar** individual situado a la derecha de cada nombre de instancia de servidor.  
  
     Para obtener más información, vea [Conectarse a la página Réplicas secundarias existentes &#40;Asistente para agregar réplica/Asistente para agregar bases de datos&#41;](../../../database-engine/availability-groups/windows/connect-to-existing-secondary-replicas-page.md).  
  
7.  La página **Validación** comprueba si los valores especificados en este asistente cumplen los requisitos del Asistente para nuevo grupo de disponibilidad. Para realizar un cambio, puede hacer clic en **Anterior** para volver a una página anterior del asistente con el fin de cambiar uno o varios valores. Haga clic en **Siguiente** para volver a la página **Validación** y haga clic en **Volver a ejecutar la validación**.  
  
     Para obtener más información, vea [Página Validación &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/validation-page-always-on-availability-group-wizards.md).  
  
8.  En la página **Resumen** , revise las opciones para el nuevo grupo de disponibilidad. Para realizar un cambio, haga clic en **Anterior** para volver a la página correspondiente. Después de realizar el cambio, haga clic en **Siguiente** para volver a la página **Resumen** .  
  
     Para obtener más información, vea [Página Resumen &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/summary-page-always-on-availability-group-wizards.md).  
  
     Si está satisfecho con las selecciones, puede hacer si lo desea en Script para crear un script de los pasos que ejecutará el asistente. A continuación, para crear y configurar el nuevo grupo de disponibilidad, haga clic en **Finalizar**.  
  
9. En la página **Progreso** se muestra el progreso de los pasos necesarios para crear el grupo de disponibilidad (configuración de puntos de conexión, creación del grupo de disponibilidad y unión de la réplica secundaria al grupo).  
  
     Para obtener más información, vea [Página Progreso &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/progress-page-always-on-availability-group-wizards.md).  
  
10. Según se completen estos pasos, la página **Resultados** mostrará el resultado de cada paso. Si todos estos pasos se realizan correctamente, el nuevo grupo de disponibilidad se configura por completo. Si alguno de los pasos produce un error, puede que tenga que completar manualmente la configuración. Para obtener información sobre la causa de un error determinado, haga clic en el vínculo “Error” asociado de la columna **Resultado** .  
  
     Una vez completado el asistente, haga clic en **Cerrar** para salir.  
  
     Para obtener más información, vea [Página Resultados &#40;asistentes para grupos de disponibilidad AlwaysOn&#41;](../../../database-engine/availability-groups/windows/results-page-always-on-availability-group-wizards.md).  
  
11. Si la sincronización de datos iniciales no se inició automáticamente en todas sus bases de datos secundarias, debe configurar las bases de datos secundarias no unidas todavía. Para obtener más información, vea [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Preparar manualmente una base de datos secundaria para un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/manually-prepare-a-secondary-database-for-an-availability-group-sql-server.md)  
  
-   [Combinar una base de datos secundaria con un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/join-a-secondary-database-to-an-availability-group-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Información general de los grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server.md)   
 [Requisitos previos, restricciones y recomendaciones para Grupos de disponibilidad AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/prereqs-restrictions-recommendations-always-on-availability.md)   
 [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)   
 [Iniciar el movimiento de datos en una base de datos secundaria AlwaysOn &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/start-data-movement-on-an-always-on-secondary-database-sql-server.md)   
 [Agregar una base de datos a un grupo de disponibilidad &#40;SQL Server&#41;](../../../database-engine/availability-groups/windows/availability-group-add-a-database.md)  
  
  
