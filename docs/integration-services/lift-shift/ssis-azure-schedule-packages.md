---
title: Programar paquetes SSIS en Azure | Microsoft Docs
description: Proporciona información general de los métodos disponibles para programar la ejecución de paquetes SSIS que se implementan en Azure SQL Database.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 7b7c6d785910425aa6388c23bc51b4690d7782c2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345689"
---
# <a name="schedule-the-execution-of-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Programar la ejecución de paquetes de SQL Server Integration Services (SSIS) implementados en Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Puede programar la ejecución de los paquetes SSIS implementados en el catálogo de SSISDB en un servidor de Azure SQL Database si selecciona uno de los métodos que se describen en este artículo. Puede programar un paquete directamente, o bien programarlo de manera indirecta como parte de una canalización de Azure Data Factory. Para obtener información general sobre SSIS en Azure, vea [Migrar cargas de trabajo de SQL Server Integration Services a la nube mediante lift-and-shift](ssis-azure-lift-shift-ssis-packages-overview.md).

- Programar un paquete directamente

  - [Programar con la opción Programar en SQL Server Management Studio (SSMS)](#ssms)

  - [Trabajos elásticos de SQL Database](#elastic)

  - [Agente SQL Server](#agent)

- [Programar un paquete indirectamente como parte de una canalización de Azure Data Factory](#activity)


## <a name="schedule-a-package-with-ssms"></a><a name="ssms"></a> Programar un paquete con SSMS

En SQL Server Management Studio (SSMS), puede hacer clic con el botón derecho en un paquete implementado en la base de datos del catálogo de SSIS, SSISDB, y seleccionar **Programación** para abrir el cuadro de diálogo **Nueva programación**. Para obtener más información, vea [Programar paquetes SSIS en Azure con SSMS](ssis-azure-schedule-packages-ssms.md).

Esta característica require SQL Server Management Studio versión 17.7 o superior. Para obtener la versión más reciente de SSMS, vea [Download SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) [Descargar SQL Server Management Studio (SSMS)].

## <a name="schedule-a-package-with-sql-database-elastic-jobs"></a><a name="elastic"></a> Programar un paquete con trabajos elásticos de SQL Database

Para obtener más información acerca de los trabajos elásticos en SQL Database, consulte [Administración de bases de datos escaladas horizontalmente en la nube](/azure/sql-database/sql-database-elastic-jobs-overview).

### <a name="prerequisites"></a>Prerequisites

Para poder usar trabajos elásticos para programar paquetes SSIS almacenados en la base de datos del catálogo de SSISDB en un servidor Azure SQL Database, debe realizar las acciones siguientes:

1.  Instale y configure los componentes de trabajos de Elastic Database. Para obtener más información, consulte [Información general sobre la instalación de trabajos de Elastic Database](/azure/sql-database/sql-database-elastic-jobs-service-installation).

2. Cree credenciales de ámbito de base de datos que los trabajos puedan usar para enviar comandos a la base de datos del catálogo de SSIS. Para obtener más información, vea [CREATE DATABASE SCOPED CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-database-scoped-credential-transact-sql.md) [CREAR CREDENCIAL DE ÁMBITO DE BASE DE DATOS (Transact-SQL)].

### <a name="create-an-elastic-job"></a>Crear un trabajo elástico

Cree un trabajo mediante un script Transact-SQL similar al que se muestra en el ejemplo siguiente:

```sql
-- Create Elastic Jobs target group
EXEC jobs.sp_add_target_group 'TargetGroup'

-- Add Elastic Jobs target group member
EXEC jobs.sp_add_target_group_member @target_group_name='TargetGroup',
    @target_type='SqlDatabase', @server_name='YourSQLDBServer.database.windows.net',
    @database_name='SSISDB' 

-- Add a job to schedule SSIS package execution
EXEC jobs.sp_add_job @job_name='ExecutePackageJob', @description='Description', 
    @schedule_interval_type='Minutes', @schedule_interval_count=60

-- Add a job step to create/start SSIS package execution using SSISDB catalog stored procedures
EXEC jobs.sp_add_jobstep @job_name='ExecutePackageJob', 
    @command=N'DECLARE @exe_id bigint 
        EXEC [SSISDB].[catalog].[create_execution]
            @folder_name=N''folderName'', @project_name=N''projectName'',
            @package_name=N''packageName'', @use32bitruntime=0,
            @runinscaleout=1, @useanyworker=1, 
            @execution_id=@exe_id OUTPUT         
        EXEC [SSISDB].[catalog].[start_execution] @exe_id, @retry_count=0', 
    @credential_name='YourDBScopedCredentials', 
    @target_group_name='TargetGroup' 

-- Enable the job schedule 
EXEC jobs.sp_update_job @job_name='ExecutePackageJob', @enabled=1, 
    @schedule_interval_type='Minutes', @schedule_interval_count=60 
```

## <a name="schedule-a-package-with-sql-server-agent-on-premises"></a><a name="agent"></a> Programar un paquete con el Agente SQL Server local

Para obtener más información, sobre el Agente SQL Server, consulte [Trabajos del Agente SQL Server para paquetes](../packages/sql-server-agent-jobs-for-packages.md).

### <a name="prerequisite---create-a-linked-server"></a>Requisitos previos: creación de un servidor vinculado

Para poder usar el Agente SQL Server de forma local para programar la ejecución de paquetes almacenados en un servidor de Azure SQL Database, tiene que agregar el servidor de SQL Database a su SQL Server local como un servidor vinculado.

1.  **Configuración del servidor vinculado**

    ```sql
    -- Add the SSISDB database on your Azure SQL Database as a linked server to your SQL Server on premises
    EXEC sp_addlinkedserver
        @server='myLinkedServer', -- Name your linked server
        @srvproduct='',     
        @provider='sqlncli', -- Use SQL Server native client
        @datasrc='<server_name>.database.windows.net', -- Add your Azure SQL Database server endpoint
        @location='',
        @provstr='',
        @catalog='SSISDB'  -- Add SSISDB as the initial catalog
    ```

2.  **Configuración de las credenciales del servidor vinculado**

    ```sql
    -- Add your Azure SQL Database server admin credentials
    EXEC sp_addlinkedsrvlogin
        @rmtsrvname = 'myLinkedServer',
        @useself = 'false',
        @rmtuser = 'myUsername', -- Add your server admin username
        @rmtpassword = 'myPassword' -- Add your server admin password
    ```

3.  **Configuración de las opciones del servidor vinculado**

    ```sql
    EXEC sp_serveroption 'myLinkedServer', 'rpc out', true;
    ```

Para obtener más información, consulte [Crear servidores vinculados](../../relational-databases/linked-servers/create-linked-servers-sql-server-database-engine.md) y [Servidores vinculados](../../relational-databases/linked-servers/linked-servers-database-engine.md).

### <a name="create-a-sql-server-agent-job"></a>Crear un trabajo del Agente SQL Server

Para programar un paquete con el Agente SQL Server de forma local, cree un trabajo con un paso de trabajo que llame a los procedimientos almacenados `[catalog].[create_execution]` y, a continuación, `[catalog].[start_execution]` del catálogo de SSIS. Para obtener más información, consulte [Trabajos del Agente SQL Server para paquetes](../packages/sql-server-agent-jobs-for-packages.md).

1.  En SQL Server Management Studio, conéctese a la base de datos de SQL Server local en la que desee crear el trabajo.

2.  Haga doble clic en el nodo **Agente SQL Server**, seleccione **Nuevo** y, a continuación, seleccione **Trabajo** para abrir el cuadro de diálogo **Nuevo trabajo**.

3.  En el cuadro de diálogo **Nuevo trabajo**, seleccione la página **Pasos** y, a continuación, seleccione **Nuevo**  para abrir el cuadro de diálogo **Nuevo paso de trabajo**.

4.  En el cuadro de diálogo **Nuevo paso de trabajo**, seleccione `SSISDB` en **Base de datos.**

5.  En el campo **Comando**, escriba un script Transact-SQL similar al que se muestra en el ejemplo siguiente:

    ```sql
    -- T-SQL script to create and start SSIS package execution using SSISDB stored procedures
    DECLARE @return_value int, @exe_id bigint 

    EXEC @return_value = [YourLinkedServer].[SSISDB].[catalog].[create_execution] 
        @folder_name=N'folderName', @project_name=N'projectName', 
        @package_name=N'packageName', @use32bitruntime=0, @runincluster=1, @useanyworker=1,
        @execution_id=@exe_id OUTPUT 

    EXEC [YourLinkedServer].[SSISDB].[catalog].[set_execution_parameter_value] @exe_id,
        @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1

    EXEC [YourLinkedServer].[SSISDB].[catalog].[start_execution] @execution_id=@exe_id
    ```

6.  Termine de configurar y programar el trabajo.

## <a name="schedule-a-package-as-part-of-an-azure-data-factory-pipeline"></a><a name="activity"></a> Programar un paquete como parte de una canalización de Azure Data Factory

Puede programar un paquete de manera indirecta mediante un desencadenador para ejecutar una canalización de Azure Data Factory en la que se ejecute un paquete SSIS.

Para programar una canalización de Azure Data Factory, use uno de los desencadenadores siguientes:

- [Desencadenador de programación](/azure/data-factory/how-to-create-schedule-trigger)

- [Desencadenador de ventana de saltos de tamaño constante](/azure/data-factory/how-to-create-tumbling-window-trigger)

- [Desencadenador basado en eventos](/azure/data-factory/how-to-create-event-trigger)

Para ejecutar un paquete SSIS como parte de una canalización Azure Data Factory, use una de las siguientes actividades:

- [Ejecutar una actividad de paquete SSIS](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

- [Actividad de procedimiento almacenado](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Pasos siguientes

Revise las opciones para ejecutar paquetes SSIS que se implementen en Azure. Para obtener más información, vea [Ejecutar paquetes SSIS en Azure](ssis-azure-run-packages.md).