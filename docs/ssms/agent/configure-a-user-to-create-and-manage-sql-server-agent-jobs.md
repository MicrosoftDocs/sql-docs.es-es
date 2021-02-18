---
description: Configure a User to Create and Manage SQL Server Agent Jobs
title: Configure a User to Create and Manage SQL Server Agent Jobs
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, user configuration
- jobs [SQL Server Agent], user configuration
- SQLAgentUserRole database role
- proxy accounts [SQL Server Agent]
ms.assetid: 67897e3e-b7d0-43dd-a2e2-2840ec4dd1ef
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e0aa22d8e76eaed8396291a19b6cc46a0a3651e4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355211"
---
# <a name="configure-a-user-to-create-and-manage-sql-server-agent-jobs"></a>Configure a User to Create and Manage SQL Server Agent Jobs

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo configurar un usuario para que cree o ejecute trabajos del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

- **Antes de empezar:**  [Seguridad](#Security)  
 
- **Para configurar un usuario para crear y administrar trabajos del Agente SQL Server, usando:**  [SQL Server Management Studio](#SSMS)  

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
Para configurar un usuario con el fin de crear o ejecutar trabajos del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], primero debe agregar un inicio de sesión de SQL Server existente o un rol msdb a uno de los siguientes roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos msdb: SQLAgentUserRole, SQLAgentReaderRole o SQLAgentOperatorRole.  
  
De forma predeterminada, los miembros de estos roles de la base de datos pueden crear sus propios pasos de trabajo que se ejecutan como ellos mismos. Si estos usuarios no administrativos desean ejecutar trabajos que ejecuten otros tipos de pasos de trabajo (por ejemplo, paquetes [!INCLUDE[ssIS](../../includes/ssis_md.md)] ), necesitarán tener acceso a una cuenta de proxy. Todos los miembros del rol fijo de servidor sysadmin tienen permiso para crear, modificar o eliminar cuentas de proxy. Para más información sobre los permisos asociados con estos roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
**Para agregar un inicio de sesión de SQL o un rol msdb a un rol fijo de base de datos del Agente SQL Server**  
  
1.  En el **Explorador de objetos**, expanda un servidor.  
  
2.  Expanda **Seguridad** y, a continuación, **Inicios de sesión**.  
  
3.  Haga clic con el botón derecho en el inicio de sesión que desee agregar a un rol fijo de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y seleccione **Propiedades**.  
  
4.  En la página **Asignación de usuario** del cuadro de diálogo **Propiedades de inicio de sesión** , seleccione la fila que contiene **msdb**.  
  
5.  En **Miembros del rol de base de datos para: msdb**, active el rol fijo de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] adecuado.  
  
**Para configurar una cuenta de proxy para crear y administrar pasos de trabajo del Agente SQL Server**  
  
1.  En el **Explorador de objetos**, expanda un servidor.  
  
2.  Expanda **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en **Servidores proxy** y seleccione **Nuevo proxy**.  
  
4.  En la página **General** del cuadro de diálogo **Nueva cuenta de proxy** , especifique el nombre del proxy, el nombre de credencial y la descripción del nuevo proxy. Tenga en cuenta que debe crear una credencial antes de crear un proxy del Agente SQL Server. Para obtener más información acerca de cómo crear una credencial, vea [Procedimientos para: crear una credencial](../../relational-databases/security/authentication-access/create-a-credential.md) y [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md).  
  
5.  Seleccione los subsistemas correspondientes para este proxy.
    1. [Sistema operativo (CmdExec)](create-a-cmdexec-job-step.md)
    1. [Consulta de SQL Server Analysis Services](create-an-analysis-services-job-step.md#to-create-an-analysis-services-query-job-step)
    1. [Comando de SQL Server Analysis Services](create-an-analysis-services-job-step.md#to-create-an-analysis-services-command-job-step-1)
    1. [Paquete de SQL Server Integration Services](../../integration-services/packages/run-integration-services-ssis-packages.md)
    1. [PowerShell](../../powershell/run-windows-powershell-steps-in-sql-server-agent.md)
  
6.  En la página **Entidades de seguridad** , agregue o quite inicios de sesión o roles para conceder o quitar el acceso a la cuenta de proxy.  

## <a name="see-also"></a>Consulte también
- [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)