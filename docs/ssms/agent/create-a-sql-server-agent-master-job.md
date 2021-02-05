---
description: Crear un trabajo maestro del Agente SQL Server
title: Crear un trabajo maestro del Agente SQL Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], master jobs
- jobs [SQL Server Agent], creating
- master SQL Server Agent job [SQL Server]
ms.assetid: c12ab23f-d7ee-43a5-8cd2-0a9121292bcd
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9f4e2903d17858075f8d3fabe293aea9475f0b57
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236778"
---
# <a name="create-a-sql-server-agent-master-job"></a>Crear un trabajo maestro del Agente SQL Server
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo crear un trabajo maestro del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
Los cambios en los trabajos principales del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se deben transmitir a todos los servidores de destino implicados. Dado que los servidores de destino no descargan inicialmente un trabajo hasta que se especifican dichos destinos, [!INCLUDE[msCoName](../../includes/msconame_md.md)] recomienda completar todos los pasos y programaciones de un trabajo concreto antes de especificar los servidores de destino. De lo contrario, debe solicitar manualmente que los servidores de destino vuelvan a descargar el trabajo modificado, ejecutando el procedimiento almacenado **sp_post_msx_operation** o modificando el trabajo mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Para más información, consulte [sp_post_msx_operation (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-post-msx-operation-transact-sql.md) o [Modificar un trabajo](../../ssms/agent/modify-a-job.md).  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Los trabajos distribuidos que tienen pasos asociados a un proxy se ejecutan bajo el contexto de la cuenta de proxy en el servidor de destino. Para que se descarguen del servidor maestro al de destino los pasos de trabajo asociados con un proxy, asegúrese de que se cumplen las condiciones siguientes:  
  
-   La subclave del registro **\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<&#42;nombre_instancia&#42;>\SQL Server Agent\AllowDownloadedJobsToMatchProxyName** (REG_DWORD) se establece en 1 (true). De forma predeterminada, esta subclave está establecida en 0 (false).  
  
-   Existe una cuenta de proxy en el servidor de destino que tiene el mismo nombre que la cuenta de proxy del servidor maestro bajo el que se ejecuta el paso de trabajo.  
  
Si se producen errores en los pasos de trabajo que utilizan cuentas de proxy durante la descarga de éstos desde el servidor maestro al servidor de destino, puede buscar en la columna **error_message** de la tabla **sysdownloadlist** de la base de datos **msdb** los mensajes de error que digan lo siguiente:  
  
-   "Este trabajo requiere una cuenta de proxy, pero la coincidencia de proxy se ha deshabilitado en el servidor de destino." Para resolver este error, establezca la subclave de registro **AllowDownloadedJobsToMatchProxyName** en 1.  
  
-   "No se encontró el proxy". Para resolver este error, asegúrese de que existe una cuenta de proxy en el servidor de destino con el mismo nombre que la cuenta de proxy del servidor maestro en la que se ejecuta el paso de trabajo.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-a-master-sql-server-agent-job"></a>Para crear un trabajo principal del Agente SQL Server  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en el que desea crear un trabajo del Agente SQL Server.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en la carpeta **Trabajos** y, después, seleccione **Nuevo trabajo...**.  
  
4.  En el cuadro de diálogo **Nuevo trabajo** , en la página **General** , modifique las propiedades generales del trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página general&#41;](../../ssms/agent/job-properties-new-job-general-page.md)  
  
5.  En la página **Pasos** , organice los pasos de trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página Pasos&#41;](../../ssms/agent/job-properties-new-job-steps-page.md)  
  
6.  En la página **Programaciones** , organice las programaciones del trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página Programaciones&#41;](../../ssms/agent/job-properties-new-job-schedules-page.md)  
  
7.  En la página **Alertas** , organice las alertas del trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página Alertas&#41;](../../ssms/agent/job-properties-new-job-alerts-page.md)  
  
8.  En la página **Notificaciones**, establezca las acciones que el Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe realizar cuando se complete el trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página Notificaciones&#41;](../../ssms/agent/job-properties-new-job-notifications-page.md).  
  
9. En la página **Destinos** , administre los servidores de destino del trabajo. Para más información sobre las opciones disponibles en esta página, consulte [Propiedades de trabajo - Nuevo trabajo &#40;Página Destinos&#41;](../../ssms/agent/job-properties-new-job-targets-page.md).  
  
10. Cuando termine, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-create-a-master-sql-server-agent-job"></a>Para crear un trabajo principal del Agente SQL Server  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb ;  
    GO  
    -- Adds a new job executed by the SQLServerAgent service called 'Weekly Sales Data Backup'  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    -- Adds a step (operation) to the 'Weekly Sales Data Backup' job.  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    -- Creates a schedule called RunOnce  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    -- Sets the 'RunOnce' schedule to the "Weekly Sales Data Backup' Job  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    -- assigns the multiserver job Weekly Sales Backups to the server SEATTLE2  
    -- assumes that SEATTLE2 is registered as a target server for the current instance.  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backups',  
        @server_name = N'SEATTLE2' ;  
    GO  
    ```  
  
Para más información, consulte:  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
