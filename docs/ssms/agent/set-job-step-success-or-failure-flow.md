---
description: Set Job Step Success or Failure Flow
title: Set Job Step Success or Failure Flow
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, action flow logic
- successful jobs [SQL Server]
- failed jobs [SQL Server]
- jobs [SQL Server Agent], action flow logic
ms.assetid: 23041ccf-8a07-41d3-85b9-c449a54b7e1e
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: c5556b9348d705606748e9f2b45b1eef57df2e6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354850"
---
# <a name="set-job-step-success-or-failure-flow"></a>Set Job Step Success or Failure Flow
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Al crear trabajos del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], puede especificar la acción que debe realizar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] si se produce un error durante la ejecución del trabajo. Tras la resolución correcta o errónea de cada paso del trabajo, determine la acción que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debería realizar. A continuación, utilice el siguiente procedimiento para configurar la lógica del flujo de las acciones de los pasos de trabajo mediante el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   **Antes de empezar:**  
  
    [Seguridad](#Security)  
  
-   **Para establecer el flujo correcto o con errores de los pasos de un trabajo, utilizando:**  
  
    [SQL Server Management Studio](#SSMS)  
  
    [Transact-SQL](#TSQL)  
  
    [objetos de administración de SQL Server](#SMO)  
  
## <a name="before-you-begin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Para establecer el flujo con éxito o con errores de los pasos de un trabajo  
  
1.  En el **Explorador de objetos**, expanda **Agente SQL Server** y, a continuación, expanda **Trabajos**.  
  
2.  Haga clic con el botón derecho en el trabajo que desea modificar y, luego, haga clic en **Propiedades**.  
  
3.  Seleccione la página **Pasos** , haga clic en un paso y, a continuación, haga clic en **Editar**.  
  
4.  En el cuadro de diálogo **Propiedades de paso de trabajo** , seleccione la página **Avanzado** .  
  
5.  En el cuadro de diálogo **Acción en caso de éxito**, haga clic en la acción que se realizará si el paso del trabajo se realiza correctamente.  
  
6.  En el cuadro **Número de reintentos** , escriba el número de veces (entre 0 y 9.999) que se debe repetir el paso del trabajo antes de considerar que ha sido erróneo. Si escribió un valor superior a 0 en el cuadro **Número de reintentos** , escriba en el cuadro **Intervalo entre intentos (min.)** el número de minutos (entre 1 y 9999) que deben transcurrir antes de volver a realizar el paso del trabajo.  
  
7.  En la lista **Acción en caso de error** , haga clic en la acción que se realizará si el paso del trabajo no se realiza correctamente.  
  
8.  Si el trabajo es un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] , puede elegir una de las siguientes opciones:  
  
    -   En el cuadro **Archivo de salida** , escriba el nombre de un archivo de salida en el que se escribirá la salida del script. De forma predeterminada, el archivo se sobrescribe cada vez que se ejecuta el paso de trabajo. Si no desea que se sobrescriba el archivo de salida, active la casilla **Anexar salida al archivo existente**.  
  
    -   Active la casilla **Registro en tabla** si desea registrar el paso de trabajo en una tabla de bases de datos. De forma predeterminada, el contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo. Si no desea que se sobrescriba el contenido, active la casilla **Anexar salida a la entrada existente de la tabla**. Una vez ejecutado el paso de trabajo ya puede verse el contenido de la tabla haciendo clic en **Ver**.  
  
    -   Active la casilla **Incluir salida de paso en historial** si desea que la salida se incluya en el historial de pasos. La salida solo se mostrará si no hubo errores. Asimismo, la salida puede aparecer truncada.  
  
9. Si la lista **Ejecutar como usuario** está disponible, seleccione la cuenta de proxy con las credenciales que utilizará el trabajo.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Usar Transact-SQL  
  
#### <a name="to-set-job-step-success-or-failure-flow"></a>Para establecer el flujo con éxito o con errores de los pasos de un trabajo  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb;  
    GO  
    EXEC sp_add_jobstep  
        @job_name = N'Weekly Sales Data Backup',  
        @step_name = N'Set database to read only',  
        @subsystem = N'TSQL',  
        @command = N'ALTER DATABASE SALES SET READ_ONLY',   
        @on_success_action = 1;  
    GO  
    ```  
  
Para más información, consulte [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md).  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Usar Objetos de administración de SQL Server  
**Para establecer el flujo con éxito o con errores de los pasos de un trabajo**  
  
Use la clase **JobStep** mediante un lenguaje de programación de su elección, como Visual Basic, Visual C# o PowerShell. Para más información, consulte [Objetos de administración de SQL Server (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
