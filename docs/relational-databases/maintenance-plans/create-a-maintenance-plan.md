---
title: Creación de un plan de mantenimiento | Microsoft Docs
description: Obtenga información sobre cómo crear un plan de mantenimiento de un solo servidor o multiservidor en SQL Server mediante SQL Server Management Studio o Transact-SQL.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: supportability
ms.topic: conceptual
helpviewer_keywords:
- maintenance plans [SQL Server], creating
ms.assetid: a945cb65-ba7a-42f4-bbd9-6ec675745523
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 0d220ed52edbf86f84071c2194658af1c3306ef5
ms.sourcegitcommit: 04d101fa6a85618b8bc56c68b9c006b12147dbb5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2021
ms.locfileid: "99049173"
---
# <a name="create-a-maintenance-plan"></a>Crear un plan de mantenimiento
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo crear un plan de mantenimiento de un solo servidor o multiservidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)]. Hay dos formas de crear estos planes de mantenimiento con [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]: con el Asistente para planes de mantenimiento o con la superficie de diseño. El uso del asistente es más conveniente si desea crear planes de mantenimiento básicos, mientras que la superficie de diseño le permite utilizar un flujo de trabajo mejorado.  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
     
     [Requisito previo](#Prerequisite)  
  
     [Seguridad](#Security)  
  
-   **Para crear un plan de mantenimiento, usando:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Para crear un plan de mantenimiento multiservidor, se debe configurar un entorno multiservidor que contenga un servidor maestro y uno o varios servidores de destino. Los planes de mantenimiento multiservidor se deben crear y mantener en el servidor maestro. Estos planes se pueden ver, pero no mantener, en servidores de destino. 
 
###  <a name="prerequisite"></a><a name="Prerequisite"></a> Requisito previo  
La [opción de configuración del servidor Agent XPs](../../database-engine/configure-windows/agent-xps-server-configuration-option.md) debe estar habilitada.
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para crear o administrar planes de mantenimiento, debe ser miembro del rol fijo de servidor **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-create-a-maintenance-plan-using-the-maintenance-plan-wizard"></a>Para crear un plan de mantenimiento con el Asistente para planes de mantenimiento  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir el servidor donde desea crear un plan de mantenimiento.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic con el botón derecho en la carpeta **Planes de mantenimiento** y seleccione **Asistente para planes de mantenimiento**.  
  
4.  Siga los pasos del asistente para crear un plan de mantenimiento. Para obtener más información, consulte [Use the Maintenance Plan Wizard](../../relational-databases/maintenance-plans/use-the-maintenance-plan-wizard.md).  
  
#### <a name="to-create-a-maintenance-plan-using-the-design-surface"></a>Para crear un plan de mantenimiento mediante la superficie de diseño  
  
1.  En el Explorador de objetos, haga clic en el signo más para expandir el servidor donde desea crear un plan de mantenimiento.  
  
2.  Haga clic en el signo más para expandir la carpeta **Administración** .  
  
3.  Haga clic con el botón derecho en la carpeta **Planes de mantenimiento** y seleccione **Nuevo plan de mantenimiento**.  
  
4.  Siga los pasos descritos en [Crear un plan de mantenimiento &#40;superficie de diseño del plan de mantenimiento&#41;](../../relational-databases/maintenance-plans/create-a-maintenance-plan-maintenance-plan-design-surface.md) para crear un plan de mantenimiento.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-create-a-maintenance-plan"></a>Para crear un plan de mantenimiento  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb;  
    GO  
    --  Adds a new job, executed by the SQL Server Agent service, called "HistoryCleanupTask_1".  
    EXEC dbo.sp_add_job  
       @job_name = N'HistoryCleanupTask_1',   
       @enabled = 1,   
       @description = N'Clean up old task history' ;   
    GO  
    -- Adds a job step for reorganizing all of the indexes in the HumanResources.Employee table to the HistoryCleanupTask_1 job.   
    EXEC dbo.sp_add_jobstep  
        @job_name = N'HistoryCleanupTask_1',   
        @step_name = N'Reorganize all indexes on HumanResources.Employee table',   
        @subsystem = N'TSQL',   
        @command = N'USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_LoginID ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_NationalIDNumber ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX AK_Employee_rowguid ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX IX_Employee_OrganizationLevel_OrganizationNode ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX IX_Employee_OrganizationNode ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    USE AdventureWorks2012  
    GO  
    ALTER INDEX PK_Employee_BusinessEntityID ON HumanResources.Employee REORGANIZE WITH ( LOB_COMPACTION = ON )   
    GO  
    ',   
        @retry_attempts = 5,   
        @retry_interval = 5 ;   
    GO  
    -- Creates a schedule named RunOnce that executes every day when the time on the server is 23:00.   
    EXEC dbo.sp_add_schedule  
        @schedule_name = N'RunOnce',   
        @freq_type = 4,   
        @freq_interval = 1,   
        @active_start_time = 233000 ;   
    GO  
    -- Attaches the RunOnce schedule to the job HistoryCleanupTask_1.   
    EXEC sp_attach_schedule  
       @job_name = N'HistoryCleanupTask_1',  
       @schedule_name = N'RunOnce' ;   
    GO  
  
    ```  
  
 Para más información, consulte:  
  
-   [sp_add_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-job-transact-sql.md)  
  
-   [sp_add_jobstep &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md)  
  
-   [sp_add_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-add-schedule-transact-sql.md)  
  
-   [sp_attach_schedule &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-attach-schedule-transact-sql.md)  
  
  
