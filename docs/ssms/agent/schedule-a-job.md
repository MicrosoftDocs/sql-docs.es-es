---
description: Schedule a Job
title: Schedule a Job
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- scheduling jobs [SQL Server]
- SQL Server Agent jobs, scheduling
- jobs [SQL Server Agent], scheduling
ms.assetid: f626390a-a3df-4970-b7a7-a0529e4a109c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 8ef6cad434deebf98ecb385666f53ba95a99f1b7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339845"
---
# <a name="schedule-a-job"></a>Schedule a Job
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo programar un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Antes de empezar:** ,  
  
    [Seguridad](#Security)  
  
-   **Para programar un trabajo, utilizando:**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [objetos de administración de SQL Server](#SMO)  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-and-attach-a-schedule-to-a-job"></a>Para crear y asociar una programación a un trabajo  
  
1.  En el **Explorador de objetos** , conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, después, expándala.  
  
2.  Expanda **Agente SQL Server**, expanda **Trabajos**, haga clic con el botón derecho en el trabajo que desee programar y haga clic en **Propiedades**.  
  
3.  Seleccione la página **Programaciones** y, a continuación, haga clic en **Nueva**.  
  
4.  En el cuadro **Nombre** , escriba el nombre de la nueva programación.  
  
5.  Desactive la casilla **Habilitado** si no desea que la programación tenga efecto inmediatamente después de su creación.  
  
6.  En **Tipo de programación**, seleccione una de las siguientes operaciones:  
  
    -   Haga clic en **Iniciar automáticamente al iniciar el Agente SQL Server** para iniciar el trabajo cuando se inicie el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Haga clic en **Iniciar al quedar inactivas las CPU** para iniciar el trabajo cuando las CPU alcancen la condición de inactivas.  
  
    -   Haga clic en **Periódica** si desea que una programación se ejecute varias veces. Para establecer la programación periódica, rellene los grupos **Frecuencia**, **Frecuencia diaria** y **Duración** en el cuadro de diálogo.  
  
    -   Haga clic en **Una vez** si desea que la programación se ejecute solo una vez. Para establecer la programación en **Una vez** , rellene el grupo **Repetición una vez** del cuadro de diálogo.  
  
#### <a name="to-attach-a-schedule-to-a-job"></a>Para asociar una programación a un trabajo  
  
1.  En el **Explorador de objetos** , conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, después, expándala.  
  
2.  Expanda **Agente SQL Server** y **Trabajos**, haga clic con el botón derecho en el trabajo que desee programar, y haga clic en **Propiedades**.  
  
3.  Seleccione la página **Programaciones** y, a continuación, haga clic en **Elegir**.  
  
4.  Seleccione la programación que desee asociar y, a continuación, haga clic en **Aceptar**.  
  
5.  En el cuadro de diálogo **Propiedades del trabajo** , haga doble clic en la programación asociada.  
  
6.  Compruebe que **Fecha de inicio** se ha establecido correctamente. Si no es así, establezca la fecha en el valor que desee para que la programación se inicie y, a continuación, hace clic en **Aceptar**.  
  
7.  En el cuadro de diálogo **Propiedades del trabajo** , haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Usar Transact-SQL  
  
#### <a name="to-schedule-a-job"></a>Para programar un trabajo  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb ;  
    GO  
    -- creates a schedule named NightlyJobs.   
    -- Jobs that use this schedule execute every day when the time on the server is 01:00.   
    EXEC sp_add_schedule  
        @schedule_name = N'NightlyJobs' ,  
        @freq_type = 4,  
        @freq_interval = 1,  
        @active_start_time = 010000 ;  
    GO  
    -- attaches the schedule to the job BackupDatabase  
    EXEC sp_attach_schedule  
       @job_name = N'BackupDatabase',  
       @schedule_name = N'NightlyJobs' ;  
    GO  
    ```  
  
Para más información, consulte [sp_add_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md) y [sp_attach_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Usar Objetos de administración de SQL Server  
Use la clase **JobSchedule** mediante un lenguaje de programación de su elección, como Visual Basic, Visual C# o PowerShell. Para más información, consulte[Objetos de administración de SQL Server (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
