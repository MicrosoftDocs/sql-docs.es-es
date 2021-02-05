---
description: Creación de un trabajo
title: Creación de un trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: b35af2b6-6594-40d1-9861-4d5dd906048c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 2c577bb79487db6047c173ac331ed27fdf7ba3b2
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99237437"
---
# <a name="create-a-job"></a>Creación de un trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo crear un trabajo del Agente SQL Server en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] u Objetos de administración de SQL Server (SMO).  
  
Para agregar pasos de trabajo, programas, alertas y notificaciones que puedan enviarse a los operadores, vea los vínculos a los temas de la sección Vea también.  
  
-   **Antes de empezar:**  
  
    [Limitaciones y restricciones](#Restrictions)  
  
    [Seguridad](#Security)  
  
-   **Para crear un trabajo, utilizando:**  
  
    [SQL Server Management Studio](#SSMSProcedure)  
  
    [Transact-SQL](#TsqlProcedure)  
  
    [objetos de administración de SQL Server](#SMOProcedure)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   Para crear un trabajo, el usuario debe ser miembro de uno de los roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o del rol fijo de servidor **sysadmin** . Solo pueden editar el trabajo el propietario de éste o los miembros del rol **sysadmin** . Para más información sobre los roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
-   La asignación de un trabajo a otro inicio de sesión no garantiza que el nuevo propietario disponga de los permisos suficientes para ejecutar el trabajo.  
  
-   El Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] local almacena los trabajos locales en la caché. Por lo tanto, cualquier modificación obliga implícitamente al Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a volver a almacenar el trabajo en la caché. Puesto que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no almacena el trabajo en la caché hasta que se llama a **sp_add_jobserver** , resulta más eficaz llamar a **sp_add_jobserver** al final.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
-   Para cambiar el propietario de un trabajo debe ser administrador de sistema.  
  
-   Por razones de seguridad, solo el propietario del trabajo o un miembro del rol **sysadmin** puede cambiar la definición del trabajo. Solo los miembros del rol fijo de servidor **sysadmin** pueden asignar la propiedad de un trabajo a otros usuarios y pueden ejecutar cualquier trabajo, independientemente de quién sea el propietario del mismo.  
  
    > [!NOTE]  
    > Si cambia la propiedad de un trabajo a un usuario que no es miembro del rol fijo de servidor **sysadmin** y el trabajo está ejecutando unos pasos que necesitan las cuentas de un servidor proxy (por ejemplo, la ejecución de paquetes [!INCLUDE[ssIS](../../includes/ssis_md.md)] ), asegúrese de que el usuario tenga acceso a ese servidor proxy o, de lo contrario, se producirán errores en el trabajo.  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-job"></a>Para crear un trabajo del Agente SQL Server  
  
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
  
#### <a name="to-create-a-sql-server-agent-job"></a>Para crear un trabajo del Agente SQL Server  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb ;  
    GO  
    EXEC dbo.sp_add_job  
        @job_name = N'Weekly Sales Data Backup' ;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @retry_attempts = 5,  
        @retry_interval = 5 ;  
    GO  
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',  
        @freq_type = 1,  
        @active_start_time = 233000 ;  
    USE msdb ;  
    GO  
    EXEC sp_attach_schedule  
       @job_name = N'Weekly Sales Data Backup',  
       @schedule_name = N'RunOnce';  
    GO  
    EXEC dbo.sp_add_jobserver  
        @job_name = N'Weekly Sales Data Backup';  
    GO  
    ```  
  
Para más información, consulte:  
  
-   [sp_add_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
-   [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md)  
  
## <a name="using-sql-server-management-objects"></a><a name="SMOProcedure"></a>Usar Objetos de administración de SQL Server  
**Para crear un trabajo del Agente SQL Server**  
  
Llame al método **Create** de la clase **Job** mediante el lenguaje de programación de su elección, como Visual Basic, Visual C# o PowerShell. Para el código de ejemplo, consulte [Programar tareas administrativas automáticas en el Agente SQL Server](../../relational-databases/server-management-objects-smo/tasks/scheduling-automatic-administrative-tasks-in-sql-server-agent.md).  
