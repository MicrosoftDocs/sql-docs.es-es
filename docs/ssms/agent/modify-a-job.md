---
description: Modify a Job
title: Modify a Job
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], modifying
- modifying jobs
- SQL Server Agent jobs, modifying
ms.assetid: dd5e5f20-20c4-4ab9-a19a-db87577dcd43
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: ecbf1e66a5667f6461293e1ec54a662557842cc4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355171"
---
# <a name="modify-a-job"></a>Modify a Job

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo cambiar las propiedades del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] u Objetos de administración de SQL Server.  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
No se puede destinar un trabajo principal del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a servidores locales y remotos.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
A menos que sea miembro del rol fijo de servidor **sysadmin** , solo podrá modificar los trabajos de su propiedad. Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
  
#### <a name="to-modify-a-job"></a>Para modificar un trabajo  
  
1.  En el **Explorador de objetos** , conéctese a una instancia de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, después, expándala.  
  
2.  Expanda **Agente SQL Server**, expanda **Trabajos**, haga clic con el botón derecho en el trabajo que desee modificar y haga clic en **Propiedades**.  
  
3.  En el cuadro de diálogo **Propiedades del trabajo** , actualice las propiedades, los pasos, la programación, las alertas y las notificaciones del trabajo desde las páginas correspondientes.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Usar Transact-SQL  
  
#### <a name="to-modify-a-job"></a>Para modificar un trabajo  
  
1.  En el Explorador de objetos, conéctese a una instancia del Motor de base de datos y, a continuación, expándala.  
  
2.  En la barra de herramientas, haga clic en **Nueva consulta**.  
  
3.  En la ventana de consulta, use los siguientes procedimientos almacenados del sistema para modificar un trabajo.  
  
    -   Ejecute [sp_update_job (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-job-transact-sql.md) para cambiar los atributos de un trabajo.  
  
    -   Ejecute [sp_update_schedule (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-schedule-transact-sql.md) para cambiar los detalles de programación de una definición de trabajo.  
  
    -   Ejecute [sp_add_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobstep-transact-sql.md) para agregar nuevos pasos de trabajo.  
  
    -   Ejecute [sp_update_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-update-jobstep-transact-sql.md) para cambiar pasos de trabajo ya existentes.  
  
    -   Ejecute [sp_delete_jobstep (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobstep-transact-sql.md) para quitar un paso de trabajo de un trabajo.  
  
    -   Procedimientos almacenados adicionales para modificar cualquier trabajo maestro del Agente SQL Server:  
  
        -   Ejecute [sp_delete_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-jobserver-transact-sql.md) para eliminar un servidor asociado actualmente a un trabajo.  
  
        -   Ejecute [sp_add_jobserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-jobserver-transact-sql.md) para asociar un servidor al trabajo actual.  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Usar Objetos de administración de SQL Server  
**Para modificar un trabajo**  
  
Use la clase **Job** mediante el lenguaje de programación de su elección, como Visual Basic, Visual C# o PowerShell. Para más información, consulte [Objetos de administración de SQL Server (SMO)](../../relational-databases/server-management-objects-smo/sql-server-management-objects-smo-programming-guide.md).  
