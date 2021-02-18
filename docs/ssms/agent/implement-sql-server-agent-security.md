---
description: Implementar la seguridad del Agente SQL Server
title: Implementar la seguridad del Agente SQL Server
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, security
- security [SQL Server Agent], about security
- security [SQL Server Agent]
- security [SQL Server], SQL Server Agent
ms.assetid: d770d35c-c8de-4e00-9a85-7d03f45a0f0d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7bd95527dd51981c3ba3b5d44e4beb2be6a1db80
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342668"
---
# <a name="implement-sql-server-agent-security"></a>Implementar la seguridad del Agente SQL Server
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente le permite al administrador de la base de datos ejecutar cada paso de trabajo en un contexto seguro que solo tiene los permisos necesarios para realizar ese paso de trabajo, que está determinado por un servidor proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para establecer los permisos para un paso de trabajo concreto, cree un proxy que disponga de los permisos necesarios y, a continuación, asigne ese proxy al paso de trabajo. Se puede especificar un servidor proxy en más de un paso de trabajo. Para los pasos de trabajo que necesitan los mismos permisos se utiliza el mismo proxy.  
  
En las siguientes secciones se explica el rol de base de datos que debe conceder a los usuarios para que puedan crear o ejecutar trabajos mediante el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="granting-access-to-sql-server-agent"></a>Conceder acceso al Agente SQL Server  
Para utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , los usuarios deben ser miembros de uno o más de los siguientes roles fijos de base de datos:  
  
-   **SQLAgentUserRole**  
  
-   **SQLAgentReaderRole**  
  
-   **SQLAgentOperatorRole**  
  
Estos roles se almacenan en la base de datos **msdb** . De manera predeterminada, ningún usuario es miembro de estos roles de base de datos. La pertenencia a estos roles se debe conceder explícitamente. Los usuarios que sean miembros del rol fijo de servidor **sysadmin** tienen acceso total al Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , y no necesitan ser miembros de estos roles fijos de base de datos para utilizar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si un usuario no es miembro de uno de estos roles de base de datos ni del rol **sysadmin** , el nodo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no estará disponible para ellos cuando se conecten con [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
Los miembros de estos roles de base de datos pueden ver y ejecutar trabajos que les pertenecen, así como crear pasos de trabajos que se ejecuten como una cuenta de proxy existente. Para información sobre los permisos específicos asociados a cada uno de estos roles, consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
Los miembros del rol fijo de servidor **sysadmin** tienen permiso para crear, modificar o eliminar cuentas de proxy. Los miembros del rol **sysadmin** tienen permiso para crear pasos de trabajo que no especifiquen un proxy, sino que se ejecuten como la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , que es la cuenta que se utiliza para iniciar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="guidelines"></a>Directrices  
Siga estas instrucciones para mejorar la seguridad de la implementación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :  
  
-   Crear cuentas de usuario dedicadas especialmente para servidores proxy y utilizarlas únicamente para ejecutar pasos de trabajos.  
  
-   Conceder solo los permisos necesarios a las cuentas de usuarios de proxy. Conceder únicamente los permisos realmente necesarios para ejecutar los pasos de trabajo que están asignados a una cuenta de proxy determinada.  
  
-   No ejecutar el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una cuenta de Microsoft Windows que sea miembro del grupo **Administradores** de Windows.  
  
-   Los servidores proxy son tan seguros como el almacén de credenciales de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Si las operaciones de escritura de usuario pueden escribir en el registro de eventos de NT, pueden producir alertas mediante el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   No especifique la cuenta de administración de NT como una cuenta de servicio o una cuenta de proxy.  
  
-   Tenga en cuenta que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tienen acceso a activos entre sí. Los dos servicios comparten un único espacio del proceso y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un administrador del sistema en el servicio [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   Cuando un servidor de destino (TSX) se registra con un servidor principal (MSX), los administradores del sistema del servidor principal obtienen el control total en la instancia del servidor de destino de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   ACE es una extensión y no se puede invocar a sí misma. La invocación de ACE la realiza Chainer ScenarioEngine.exe, también conocido como Microsoft.SqlServer.Chainer.Setup.exe, u otro proceso de host.  
  
-   ACE depende de las DLL de configuración siguientes pertenecientes a SSDP, ya que ACE llama a las siguientes API de DLL:  
  
    -   **SCO**: Microsoft.SqlServer.Configuration.Sco.dll, incluidas las nuevas validaciones de SCO para las cuentas virtuales  
  
    -   **Clúster**: Microsoft.SqlServer.Configuration.Cluster.dll  
  
    -   **SFC**: Microsoft.SqlServer.Configuration.SqlConfigBase.dll  
  
    -   **Extensión**: Microsoft.SqlServer.Configuration.ConfigExtension.dll  
  
## <a name="see-also"></a>Consulte también  
[Usar los roles predefinidos](../../reporting-services/security/role-definitions-predefined-roles.md)  
[sp_addrolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
[sp_droprolemember (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-droprolemember-transact-sql.md)  
[Seguridad y protección (motor de base de datos)](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md)  
