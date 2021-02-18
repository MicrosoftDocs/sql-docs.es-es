---
description: Seleccionar una cuenta para el servicio Agente SQL Server
title: Seleccionar una cuenta para el servicio Agente SQL Server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- roles [SQL Server], SQL Server Agent
- SQL Server Agent, accounts
- startup accounts [SQL Server]
- SQL Server Agent service, accounts
- accounts [SQL Server], SQL Server Agent
- Windows groups [SQL Server Agent]
- SQL Server Agent, permissions
- members [SQL Server], SQL Server Agent service
- Windows domain accounts [SQL Server]
- security [SQL Server], SQL Server Agent
ms.assetid: fe658e32-9e6b-4147-a189-7adc3bd28fe7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 05/04/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6267141d71215834c6e25f9bb67581206e515e49
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351711"
---
# <a name="select-an-account-for-the-sql-server-agent-service"></a>Seleccionar una cuenta para el servicio Agente SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

La cuenta de inicio del servicio define la cuenta de [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows en la que se ejecuta el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y sus permisos de red. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta como una cuenta de usuario especificada. Se puede seleccionar una cuenta para el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , donde se pueden elegir las opciones siguientes:  
  
-   **Cuenta integrada**. Puede elegirla de una lista con las siguientes cuentas de servicio de Windows integradas:  
  
    -   Cuenta de **sistema local** . El nombre de esta cuenta es NT AUTHORITY\System. Es una cuenta eficaz con acceso sin restricciones a todos los recursos del sistema local. Es miembro del grupo **Administradores** de Windows del equipo local y, por tanto, miembro del rol fijo de servidor [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **sysadmin**.  
  
        > [!IMPORTANT]  
        > La opción **Cuenta del sistema local** se mantiene solo por motivos de compatibilidad con versiones anteriores. La cuenta del sistema local tiene permisos que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no necesita. Evite ejecutar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la cuenta del sistema local. Para mejorar la seguridad, utilice una cuenta de dominio de Windows con los permisos que se enumeran en la sección siguiente, "Permisos de cuentas de dominio de Windows".  
  
-   **Esta cuenta**. Permite especificar la cuenta de dominio de Windows en la que se ejecuta el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Se recomienda que la cuenta de usuario de Windows que elija no sea miembro del grupo **Administradores** de Windows. Sin embargo, existen limitaciones en el uso de la administración multiservidor cuando la cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no es miembro del grupo local **Administradores** . Para obtener más información, vea 'Tipos de cuenta de servicio compatibles' más adelante en este tema.  
  
## <a name="windows-domain-account-permissions"></a>Permisos de cuentas de dominio de Windows  
Para mayor seguridad, seleccione **Esta cuenta**, que especifica una cuenta de dominio de Windows. La cuenta de dominio de Windows que especifique debe tener los permisos siguientes:  
  
-   En todas las versiones de Windows, permiso para iniciar sesión como servicio (SeServiceLogonRight).  
  
> [!NOTE]  
> La cuenta de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe formar parte del grupo Acceso compatible con versiones anteriores de Windows 2000 en el controlador de dominio; de lo contrario, los trabajos que pertenecen a usuarios del dominio que no son miembros del grupo Administradores de Windows producirán un error.  
  
-   En servidores de Windows, la cuenta con la que se ejecuta el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requiere los permisos siguientes para poder admitir servidores proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Permiso para omitir la comprobación transversal (SeChangeNotifyPrivilege)  
  
    -   Permiso para reemplazar un token (símbolo) del proceso (SeAssignPrimaryTokenPrivilege)  
  
    -   Permiso para ajustar cuotas de memoria para un proceso (SeIncreaseQuotaPrivilege)  
  
    -   Permiso para tener acceso a este equipo desde la red (SeNetworkLogonRight)  
  
> [!NOTE]  
> Si la cuenta no tiene los permisos necesarios para admitir servidores proxy, solo los miembros del rol fijo de servidor **sysadmin** pueden crear trabajos.  
  
> [!NOTE]  
> Para recibir una notificación de alerta WMI, la cuenta de servicio para el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe tener concedido permiso al espacio de nombres que contiene los eventos WMI y ALTER ANY EVENT NOTIFICATION.  
  
## <a name="sql-server-role-membership"></a>Pertenencia a roles de SQL Server  
La cuenta que ejecuta el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe ser miembro de los roles de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] siguientes:  
  
-   La cuenta debe ser miembro del rol fijo de servidor **sysadmin** .  
  
-   Para usar el procesamiento de trabajos multiservidor, la cuenta debe ser miembro del rol **TargetServersRole** de la base de datos **msdb** en el servidor maestro.  
  
## <a name="supported-service-account-types"></a>Tipos de cuenta de servicio compatibles  
En la tabla siguiente se muestran los tipos de cuenta de Windows que se pueden usar para el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
|Tipo de cuenta de servicio|Índice no agrupado|Servidor en clúster|Controlador de dominio (no agrupado)|  
|------------------------|-------------------------|--------------------|--------------------------------------|  
|[!INCLUDE[msCoName](../../includes/msconame_md.md)] Cuenta de dominio de Windows (miembro del grupo de administradores de Windows)|Compatible|Compatible|Compatible|  
|Cuenta de dominio de Windows (no administrativa)|Compatible<br /><br />Vea la limitación 1 a continuación.|Compatible<br /><br />Vea la limitación 1 a continuación.|Compatible<br /><br />Vea la limitación 1 a continuación.|  
|Cuenta de servicio de red (NT AUTHORITY\NetworkService)|Compatible<br /><br />Vea las limitaciones 1, 3 y 4 a continuación.|No compatible|No compatible|  
|Cuenta de usuario local (no administrativa)|Compatible<br /><br />Vea la limitación 1 a continuación.|No compatible|No aplicable|  
|Cuenta del sistema local (NT AUTHORITY\System)|Compatible<br /><br />Vea la limitación 2 a continuación.|No compatible|Compatible<br /><br />Vea la limitación 2 a continuación.|  
|Cuenta del servicio local (NT AUTHORITY\LocalService)|No compatible|No compatible|No compatible|  
  
### <a name="limitation-1-using-non-administrative-accounts-for-multiserver-administration"></a>Limitación 1: utilizar cuentas no administrativas para la administración multiservidor  
Dar de alta servidores de destino en el servidor maestro puede producir un error acompañado del siguiente mensaje: “Error en la operación de alta”.  
  
Para resolver este error, reinicie los servicios [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Para más información, consulte [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md).  
  
### <a name="limitation-2-using-the-local-system-account-for-multiserver-administration"></a>Limitación 2: utilizar la cuenta de sistema local para la administración multiservidor  
Solo se admite la administración multiservidor cuando el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta bajo la cuenta de sistema local y tanto el servidor maestro como el servidor de destino residen en el mismo equipo. Si usa esta configuración, cuando lleve a cabo el alta de los servidores de destino en el servidor maestro se devuelve el siguiente mensaje:  
  
"Comprobar que la cuenta de inicio del agente de *<nombre_equipo_servidor_destino>* posea derechos para iniciar la sesión como servidor de destino".  
  
Puede omitir este mensaje informativo. La operación de alta debe finalizar correctamente. Para obtener más información, vea [Crear un entorno multiservidor](../../ssms/agent/create-a-multiserver-environment.md).  
  
### <a name="limitation-3-using-the-network-service-account-when-it-is-a-sql-server-user"></a>Limitación 3: utilizar la cuenta de servicio de red cuando es un usuario de SQL Server  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Se puede producir un error al iniciar el Agente si ejecuta el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bajo una cuenta de servicio de red y se concedió explícitamente a esta cuenta acceso para iniciar sesión en una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] como un usuario de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Para resolver este problema, reinicie el equipo donde se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Solo es necesario hacerlo una vez.  
  
### <a name="limitation-4-using-the-network-service-account-when-sql-server-reporting-services-is-running-on-the-same-computer"></a>Limitación 4: utilizar la cuenta de servicio de red cuando SQL Server Reporting Services se ejecuta en el mismo equipo  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Se puede producir un error al iniciar el Agente si se ejecuta el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] bajo la cuenta de servicio de red y [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] también está ejecutándose en el mismo equipo.  
  
Para resolver este problema, reinicie el equipo donde se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, reinicie los servicios [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Solo es necesario hacerlo una vez.  
  
## <a name="common-tasks"></a>Tareas comunes  
**Para especificar la cuenta de inicio del servicio del Agente SQL Server**  
  
-   [Establecer la cuenta de inicio de servicio para el Agente SQL Server &#40;Administración de configuración de SQL Server&#41;](../../ssms/agent/set-service-startup-account-sql-server-agent-sql-server-configuration-manager.md)  
  
**Para especificar el perfil de correo del Agente SQL Server**  
  
-   [Cómo: Configurar el Agente SQL Server para que use el Correo electrónico de base de datos](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md)  
  
> [!NOTE]  
> Utilice el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para especificar que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe iniciarse cuando se inicie el sistema operativo.  
  
## <a name="see-also"></a>Consulte también  
[Configurar cuentas de servicio de Windows](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md)  
[Administración de servicios con SQL Computer Manager](../../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
[Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md)  
