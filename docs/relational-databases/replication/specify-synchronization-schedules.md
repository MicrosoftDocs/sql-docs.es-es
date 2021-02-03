---
title: Especificar programaciones de sincronización | Microsoft Docs
description: Obtenga información sobre cómo especificar programaciones de sincronización en SQL Server mediante SQL Server Management Studio, Transact-SQL o Replication Management Objects.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- subscriptions [SQL Server replication], synchronizing
- scheduling synchronization [SQL Server replication]
- synchronization [SQL Server replication], schedules
- replication [SQL Server], synchronization
ms.assetid: 97f2535b-ec19-4973-823d-bcf3d5aa0216
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: a0dac3922e006d34bf1f93c617ae70d79aff27f6
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250612"
---
# <a name="specify-synchronization-schedules"></a>Especificar programaciones de sincronización
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  En este tema se describe cómo especificar programaciones de sincronización en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o Replication Management Objects (RMO). Al crear una suscripción, puede definir una programación de sincronización que controla cuándo se ejecutará el agente de replicación para la suscripción. Si no especifica parámetros de programación, la suscripción usará la programación predeterminada.  
  
 El Agente de distribución (para las instantáneas y la replicación transaccional) o el Agente de mezcla (para la replicación de mezcla) sincronizan las suscripciones. Los agentes pueden ejecutarse continuamente, a petición o según una programación.  
  
 **En este tema**  
  
-   **Para especificar programaciones de sincronización con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
     [Replication Management Objects (RMO)](#RMOProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
 Especifique programaciones de sincronización en la página **Programación de sincronización** del Asistente para nuevas suscripciones. Para obtener más información sobre cómo utilizar este asistente, vea [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md) y [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
 Modifique las programaciones de sincronización en el cuadro de diálogo **Propiedades de programación del trabajo** , al que se obtiene acceso desde la carpeta **Trabajos** de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] y desde las ventanas de detalles del agente en el Monitor de replicación. Para información sobre cómo iniciar el Monitor de replicación, vea [Iniciar el Monitor de replicación](../../relational-databases/replication/monitor/start-the-replication-monitor.md).  
  
 Si especifica programaciones desde la carpeta **Trabajos** , use la tabla siguiente para determinar el nombre del trabajo del agente.  
  
|Agente|Nombre del trabajo|  
|-----------|--------------|  
|Agente de mezcla para suscripciones de extracción|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<integer>**|  
|Agente de mezcla para suscripciones de inserción|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
|Agente de distribución para suscripciones de inserción|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>** <sup>1</sup>|  
|Agente de distribución para suscripciones de extracción|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<SubscriptionDatabase>-\<GUID>** <sup>2</sup>|  
|Agente de distribución para suscripciones de inserción en suscriptores que no sean de SQL Server|**\<Publisher>-\<PublicationDatabase>-\<Publication>-\<Subscriber>-\<integer>**|  
  
 <sup>1</sup> Para las suscripciones de inserción a publicaciones de Oracle, es **\<Publisher>-\<Publisher**> en lugar de **\<Publisher>-\<PublicationDatabase>**  
  
 <sup>2</sup> Para las suscripciones de extracción a publicaciones de Oracle, es **\<Publisher>-\<DistributionDatabase**> en lugar de **\<Publisher>-\<PublicationDatabase>**  
  
#### <a name="to-specify-synchronization-schedules"></a>Para especificar programaciones de sincronización  
  
1.  En la página **Programación de sincronización** del Asistente para nuevas suscripciones, seleccione uno de los siguientes valores en la lista desplegable **Programación del agente** para cada suscripción que quiera crear:  
  
    -   **Ejecutar continuamente**  
  
    -   **Ejecutar solamente a petición**  
  
    -   **\<Define Schedule...>**  
  
2.  Si selecciona **\<Define Schedule...>** , especifique una programación en el cuadro de diálogo **Propiedades de programación del trabajo** y, después, haga clic en **Aceptar**.  
  
3.  Finalice el asistente.  

#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-replication-monitor"></a>Para modificar una programación de sincronización para una suscripción de inserción en el Monitor de replicación  
  
1.  Expanda un grupo de publicador en el panel izquierdo del Monitor de replicación, expanda un publicador y, a continuación, haga clic en una publicación.  
  
2.  Haga clic en la pestaña **Todas las suscripciones** .  
  
3.  Haga clic con el botón secundario en una suscripción y, a continuación, haga clic en **Ver detalles**.  
  
4.  En la ventana **Suscripción <nombreDeSuscripción>** , haga clic en **Acción** y después en **Propiedades del trabajo de \<AgentName>** .  
  
5.  En la página **Programaciones** del cuadro de diálogo **Propiedades del trabajo: \<JobName>** , haga clic en **Editar.**  
  
6.  En el cuadro de diálogo **Propiedades de programación del trabajo** , seleccione un valor en la lista desplegable **Tipo de programación** :  
  
    -   Para especificar que el agente debe ejecutarse continuamente, seleccione **Iniciar automáticamente al iniciar el Agente SQL Server**.  
  
    -   Para especificar que el agente debe ejecutarse de acuerdo con una programación, seleccione **Periódica**.  
  
    -   Para especificar que el agente debe ejecutarse a petición, seleccione **Una vez**.  
  
7.  Si selecciona **Periódica**, especifique una programación para el agente.  
  
8.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-push-subscription-in-management-studio"></a>Para modificar una programación de sincronización para una suscripción de inserción en Management Studio  
  
1.  Conéctese al distribuidor en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Agente SQL Server** y a continuación la carpeta **Trabajos** .  
  
3.  Haga clic con el botón secundario en el trabajo del Agente de distribución o de mezcla asociado con la suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  En la página **Programaciones** del cuadro de diálogo **Propiedades del trabajo: \<JobName>** , haga clic en **Editar.**  
  
5.  En el cuadro de diálogo **Propiedades de programación del trabajo** , seleccione un valor en la lista desplegable **Tipo de programación** :  
  
    -   Para especificar que el agente debe ejecutarse continuamente, seleccione **Iniciar automáticamente al iniciar el Agente SQL Server**.  
  
    -   Para especificar que el agente debe ejecutarse de acuerdo con una programación, seleccione **Periódica**.  
  
    -   Para especificar que el agente debe ejecutarse a petición, seleccione **Una vez**.  
  
6.  Si selecciona **Periódica**, especifique una programación para el agente.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
#### <a name="to-modify-a-synchronization-schedule-for-a-pull-subscription-in-management-studio"></a>Para modificar una programación de sincronización para una suscripción de extracción en Management Studio  
  
1.  Conéctese al suscriptor en [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]y expanda el nodo de servidor.  
  
2.  Expanda la carpeta **Agente SQL Server** y a continuación la carpeta **Trabajos** .  
  
3.  Haga clic con el botón secundario en el trabajo del Agente de distribución o de mezcla asociado con la suscripción y, a continuación, haga clic en **Propiedades**.  
  
4.  En la página **Programaciones** del cuadro de diálogo **Propiedades del trabajo: \<JobName>** , haga clic en **Editar.**  
  
5.  En el cuadro de diálogo **Propiedades de programación del trabajo** , seleccione un valor en la lista desplegable **Tipo de programación** :  
  
    -   Para especificar que el agente debe ejecutarse continuamente, seleccione **Iniciar automáticamente al iniciar el Agente SQL Server**.  
  
    -   Para especificar que el agente debe ejecutarse de acuerdo con una programación, seleccione **Periódica**.  
  
    -   Para especificar que el agente debe ejecutarse a petición, seleccione **Una vez**.  
  
6.  Si selecciona **Periódica**, especifique una programación para el agente.  
  
7.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
 Puede definir programaciones de sincronización mediante programación usando los procedimientos almacenados de replicación. Los procedimientos almacenados que usa dependen del tipo de replicación y del tipo de suscripción (extracción o inserción).  
  
 Los parámetros de programación siguientes definen una programación, los comportamientos de los que se heredan de [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md):  
  
-   **\@frequency_type**: el tipo de frecuencia usado al programar el agente.  
  
-   **\@frequency_interval**: el día de la semana cuando se ejecuta un agente.  
  
-   **\@frequency_relative_interval**: la semana de un mes determinado en que el agente se programa para la ejecución mensual.  
  
-   **\@frequency_recurrence_factor**: el número de unidades del tipo de frecuencia que se producen entre las sincronizaciones.  
  
-   **\@frequency_subday**: la unidad de frecuencia cuando el agente se ejecuta más de una vez al día.  
  
-   **\@frequency_subday_interval**: el número de unidades de frecuencia entre ejecuciones cuando el agente se ejecuta más de una vez al día.  
  
-   **\@active_start_time_of_day**: la primera hora de un día determinado cuando se va a iniciar la ejecución de un agente.  
  
-   **\@active_end_time_of_day**: la hora más reciente de un día determinado cuando se va a iniciar la ejecución de un agente.  
  
-   **\@active_start_date**: el primer día en que la programación del agente entrará en vigor.  
  
-   **\@active_end_date**: el último día en que la programación del agente entrará en vigor.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-transactional-publication"></a>Para definir la programación de sincronización para una suscripción de extracción a una publicación transaccional  
  
1.  Cree una nueva suscripción de extracción para una publicación transaccional. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  En el suscriptor, ejecute [sp_addpullsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Especifique **\@publisher**, **\@publisher_db**, **\@publication** y las credenciales de Windows [!INCLUDE[msCoName](../../includes/msconame-md.md)] con las que se ejecuta el Agente de distribución en el suscriptor para **\@job_name** y **\@password**. Especifique los parámetros de sincronización, detallados anteriormente, que definen la programación para el trabajo del Agente de distribución que sincroniza la suscripción.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-transactional-publication"></a>Para definir la programación de sincronización para una suscripción de inserción a una publicación transaccional  
  
1.  Cree una nueva suscripción de inserción para una publicación transaccional. Para obtener más información, consulte [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  En el suscriptor, ejecute [sp_addpushsubscription_agent &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addpushsubscription-agent-transact-sql.md). Especifique **\@subscriber**, **\@subscriber_db**, **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de distribución en el suscriptor para **\@job_name** y **\@password**. Especifique los parámetros de sincronización, detallados anteriormente, que definen la programación para el trabajo del Agente de distribución que sincroniza la suscripción.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-pull-subscription-to-a-merge-publication"></a>Para definir la programación de sincronización para una suscripción de extracción a una publicación de combinación  
  
1.  Cree una nueva suscripción de extracción para una publicación de combinación. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  En el suscriptor, ejecute [sp_addmergepullsubscription_agent](../../relational-databases/system-stored-procedures/sp-addpullsubscription-agent-transact-sql.md). Especifique **\@publisher**, **\@publisher_db**, **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de mezcla en el suscriptor para **\@job_name** y **\@password**. Especifique los parámetros de sincronización, detallados anteriormente, que definen la programación para el trabajo del Agente de distribución que sincroniza la suscripción.  
  
#### <a name="to-define-the-synchronization-schedule-for-a-push-subscription-to-a-merge-publication"></a>Para definir la programación de sincronización para una suscripción de inserción a una publicación de combinación  
  
1.  Cree una nueva suscripción de inserción a una publicación de combinación. Para obtener más información, consulte [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  En el suscriptor, ejecute [sp_addmergepushsubscription_agent](../../relational-databases/system-stored-procedures/sp-addmergepushsubscription-agent-transact-sql.md). Especifique **\@subscriber**, **\@subscriber_db**, **\@publication** y las credenciales de Windows con las que se ejecuta el Agente de mezcla en el suscriptor para **\@job_name** y **\@password**. Especifique los parámetros de sincronización, detallados anteriormente, que definen la programación para el trabajo del Agente de distribución que sincroniza la suscripción.  
  
##  <a name="using-replication-management-objects-rmo"></a><a name="RMOProcedure"></a> Uso de Replication Management Objects (RMO)  
 La replicación usa el Agente SQL Server para programar los trabajos de las actividades que se producen periódicamente, como la generación de instantáneas y la sincronización de suscripción. Puede usar Replication Management Objects (RMO) mediante programación para especificar las programaciones de los trabajos de agente de replicación.  
  
> [!NOTE]  
>  Si crea una suscripción y especifica un valor **false** para **CreateSyncAgentByDefault** (comportamiento predeterminado de las suscripciones de extracción), no se crea el trabajo del agente y se omiten las propiedades de la programación. En este caso, la aplicación debe determinar la programación de sincronización. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md) y [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-transactional-publication"></a>Para definir una programación del agente de replicación al crear una suscripción de inserción a una publicación transaccional  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransSubscription> para la base de datos de suscripciones que está creando. Para obtener más información, consulte [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Antes de llamar <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, establezca uno o varios de los campos siguientes de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - el tipo de frecuencia (como diaria o semanal) que usa al programar el agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - el día de la semana en el que se ejecuta un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - la semana de un mes determinado en la que se programa el agente para que se ejecute mensualmente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - el número de unidades de tipo de frecuencia que se producen entre las sincronizaciones.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - la unidad de frecuencia cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - el número de unidades de frecuencia entre ejecuciones cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - la primera hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - la última hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - primer día vigente de la programación del agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - último día vigente de la programación del agente.  
  
    > [!NOTE]  
    >  Si no especifica una de estas propiedades, se establece un valor predeterminado.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> para crear la suscripción.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-transactional-publication"></a>Para definir una programación del agente de replicación al crear una suscripción de extracción a una publicación transaccional  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.TransPullSubscription> para la base de datos de suscripciones que está creando. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Antes de llamar <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, establezca uno o varios de los campos siguientes de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - el tipo de frecuencia (como diaria o semanal) que usa al programar el agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - el día de la semana en el que se ejecuta un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - la semana de un mes determinado en la que se programa el agente para que se ejecute mensualmente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - el número de unidades de tipo de frecuencia que se producen entre las sincronizaciones.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - la unidad de frecuencia cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - el número de unidades de frecuencia entre ejecuciones cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - la primera hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - la última hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - primer día vigente de la programación del agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - último día vigente de la programación del agente.  
  
    > [!NOTE]  
    >  Si no especifica una de estas propiedades, se establece un valor predeterminado.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> para crear la suscripción.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-pull-subscription-to-a-merge-publication"></a>Para definir una programación del agente de replicación al crear una suscripción de extracción a una publicación de combinación  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergePullSubscription> para la base de datos de suscripciones que está creando. Para obtener más información, consulte [Create a Pull Subscription](../../relational-databases/replication/create-a-pull-subscription.md).  
  
2.  Antes de llamar <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A>, establezca uno o varios de los campos siguientes de la propiedad <xref:Microsoft.SqlServer.Replication.PullSubscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - el tipo de frecuencia (como diaria o semanal) que usa al programar el agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - el día de la semana en el que se ejecuta un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - la semana de un mes determinado en la que se programa el agente para que se ejecute mensualmente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - el número de unidades de tipo de frecuencia que se producen entre las sincronizaciones.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - la unidad de frecuencia cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - el número de unidades de frecuencia entre ejecuciones cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - la primera hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - la última hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - primer día vigente de la programación del agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - último día vigente de la programación del agente.  
  
    > [!NOTE]  
    >  Si no especifica una de estas propiedades, se establece un valor predeterminado.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.PullSubscription.Create%2A> para crear la suscripción.  
  
#### <a name="to-define-a-replication-agent-schedule-when-you-create-a-push-subscription-to-a-merge-publication"></a>Para definir una programación del agente de replicación al crear una suscripción de inserción a una publicación de combinación  
  
1.  Cree una instancia de la clase <xref:Microsoft.SqlServer.Replication.MergeSubscription> para la base de datos de suscripciones que está creando. Para obtener más información, consulte [Create a Push Subscription](../../relational-databases/replication/create-a-push-subscription.md).  
  
2.  Antes de llamar <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A>, establezca uno o varios de los campos siguientes de la propiedad <xref:Microsoft.SqlServer.Replication.Subscription.AgentSchedule%2A> :  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyType%2A> - el tipo de frecuencia (como diaria o semanal) que usa al programar el agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyInterval%2A> - el día de la semana en el que se ejecuta un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRelativeInterval%2A> - la semana de un mes determinado en la que se programa el agente para que se ejecute mensualmente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencyRecurrenceFactor%2A> - el número de unidades de tipo de frecuencia que se producen entre las sincronizaciones.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDay%2A> - la unidad de frecuencia cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.FrequencySubDayInterval%2A> - el número de unidades de frecuencia entre ejecuciones cuando el agente se ejecuta más de una vez al día.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartTime%2A> - la primera hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndTime%2A> - la última hora de un día determinado en la que se inicia la ejecución de un agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveStartDate%2A> - primer día vigente de la programación del agente.  
  
    -   <xref:Microsoft.SqlServer.Replication.ReplicationAgentSchedule.ActiveEndDate%2A> - último día vigente de la programación del agente.  
  
    > [!NOTE]  
    >  Si no especifica una de estas propiedades, se establece un valor predeterminado.  
  
3.  Llame al método <xref:Microsoft.SqlServer.Replication.Subscription.Create%2A> para crear la suscripción.  
  
###  <a name="example-rmo"></a><a name="PShellExample"></a> Ejemplo (RMO)  
 Este ejemplo crea una suscripción de inserción a una publicación de combinación y especifica la programación en la que se sincroniza la suscripción.  
  
 [!code-cs[HowTo#rmo_CreateMergePushSub](../../relational-databases/replication/codesnippet/csharp/rmohowto/rmotestevelope.cs#rmo_createmergepushsub)]  
  
 [!code-vb[HowTo#rmo_vb_CreateMergePushSub](../../relational-databases/replication/codesnippet/visualbasic/rmohowtovb/rmotestenv.vb#rmo_vb_createmergepushsub)]  
  
## <a name="see-also"></a>Consulte también  
 [Replication Security Best Practices](../../relational-databases/replication/security/replication-security-best-practices.md)   
 [Subscribe to Publications](../../relational-databases/replication/subscribe-to-publications.md)   
 [Sincronizar una suscripción de inserción](../../relational-databases/replication/synchronize-a-push-subscription.md)   
 [Sincronizar una suscripción de extracción](../../relational-databases/replication/synchronize-a-pull-subscription.md)   
 [Sincronizar datos](../../relational-databases/replication/synchronize-data.md)  
  
  
