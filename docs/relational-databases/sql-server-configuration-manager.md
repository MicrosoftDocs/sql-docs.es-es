---
title: Administrador de configuración de SQL Server | Microsoft Docs
description: Uso ddel cliente del Administrador de configuración de SQL Server
ms.custom: ''
ms.date: 12/31/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: vanto
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- protocols [SQL Server], managing
- network protocols [SQL Server], managing
- Client Network Utility
- accounts [SQL Server]
- SQL Server Configuration Manager
- Server Network Utility
- accounts [SQL Server], services
- services [SQL Server], managing
- tools [SQL Server], SQL Server Configuration Manager
- configuration manager [SQL Server]
ms.assetid: e6beaea4-164c-4078-95ae-b9e28b0aefe8
author: rothja
ms.author: jroth
ms.openlocfilehash: 50f336ebe1311a4736513ded3ff8eaa4a572326d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345351"
---
# <a name="sql-server-configuration-manager"></a>Administrador de configuración de SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]


  [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es una herramienta para administrar los servicios asociados a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], para configurar los protocolos de red utilizados por [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]y para administrar la configuración de conectividad de red de los equipos cliente de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . El Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] se instala con SQL Server. [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] El Administrador de configuración es un complemento de [!INCLUDE[msCoName](../includes/msconame-md.md)] Management Console que está disponible desde el menú Inicio o que se puede agregar a cualquier otra pantalla de [!INCLUDE[msCoName](../includes/msconame-md.md)] Management Console. [!INCLUDE[msCoName](../includes/msconame-md.md)] Management Console (**mmc.exe**) usa el archivo **SQLServerManager\<version>.msc** (como, por ejemplo, **SQLServerManager13.msc** para [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]) para abrir el Administrador de configuración. Necesitará la versión del Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] correspondiente para administrar esa versión específica de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Cuando instala Windows en la unidad C, estas son las rutas de acceso a las cinco últimas versiones.  
  
|Versión|Path|  
|-|-|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2019|C:\Windows\SysWOW64\SQLServerManager15.msc| 
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2017|C:\Windows\SysWOW64\SQLServerManager14.msc|  
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 2016|C:\Windows\SysWOW64\SQLServerManager13.msc|  
|[!INCLUDE[ssSQL14](../includes/sssql14-md.md)]|C:\Windows\SysWOW64\SQLServerManager12.msc|  
|[!INCLUDE[ssSQL11](../includes/sssql11-md.md)]|C:\Windows\SysWOW64\SQLServerManager11.msc|
  
> [!NOTE]
>  Como el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es un complemento del programa [!INCLUDE[msCoName](../includes/msconame-md.md)] Management Console y no un programa independiente, el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no aparece como aplicación en las versiones más recientes de Windows.  
> 
>  -   **Windows 10**:  
>          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , escriba SQLServerManager13.msc (para **) en la** página de inicio [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]. Para otras versiones de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], reemplace 13 por el número correspondiente. Al hacer clic en SQLServerManager13.msc, se abre el Administrador de configuración. Para anclar el Administrador de configuración a la página de inicio o a la barra de tareas, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Abrir ubicación del archivo**. En el Explorador de archivos de Windows, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Anclar a Inicio** o **Anclar a la barra de tareas**.  
> -   **Windows 8**:  
>          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], escriba **SQLServerManager\<version>.msc** (por ejemplo, **SQLServerManager13.msc**) en el acceso a **Buscar** de **Aplicaciones** y, después, presione **Entrar**.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] El Administrador de configuración y SQL Server Management Studio usan el Instrumental de administración de Windows (WMI) para ver y cambiar algunas configuraciones del servidor. WMI proporciona una forma unificada de crear una interfaz con las llamadas a la API que administran las operaciones del Registro solicitadas por las herramientas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] y de proporcionar un mejor control y manipulación de los servicios SQL seleccionados del complemento del Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Para obtener más información sobre cómo configurar permisos relacionados con WMI, vea [Configurar WMI para mostrar el estado del servidor en Herramientas de SQL Server](../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md).  
  
 Para iniciar, detener, pausar, reanudar o configurar los servicios en otro equipo por medio del Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], vea [Conectarse a otro equipo &#40;Administrador de configuración de SQL Server&#41;](../database-engine/configure-windows/scm-services-connect-to-another-computer.md).  
  
## <a name="managing-services"></a>Administrar servicios  
 Utilice el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para iniciar, pausar, continuar o detener los servicios, para ver las propiedades de estos o para cambiarlas.  
  
 Utilice el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para iniciar el [!INCLUDE[ssDE](../includes/ssde-md.md)] utilizando parámetros de inicio.  Para obtener más información, vea [Configurar opciones de inicio del servidor &#40;Administrador de configuración de SQL Server&#41;](../database-engine/configure-windows/scm-services-configure-server-startup-options.md).  
  
## <a name="changing-the-accounts-used-by-the-services"></a>Cambiar las cuentas utilizadas por los servicios  
 Administre los servicios de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
> [!IMPORTANT]  
>  Utilice siempre herramientas de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] como el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para cambiar la cuenta utilizada por [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] o los servicios del Agente [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , o para cambiar la contraseña de la cuenta. Además de cambiar el nombre de la cuenta, el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] realiza configuraciones adicionales, como establecer los permisos del Registro de Windows para que la nueva cuenta pueda leer la configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Otras herramientas, como el Administrador de control de servicios de Windows, pueden cambiar el nombre de la cuenta, pero no la configuración asociada. Si el servicio no puede obtener acceso a la parte de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] del Registro, no se puede iniciar correctamente.  
  
 Como ventaja adicional, las contraseñas modificadas mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , SMO o WMI tienen efecto inmediatamente, sin necesidad de reiniciar el servicio.  
  
## <a name="manage-server--client-network-protocols"></a>Administrar protocolos de red de servidor & cliente  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permite configurar protocolos de red de servidor y cliente, así como opciones de conectividad. Una vez habilitados los protocolos correctos, no suele ser necesario cambiar las conexiones de red del servidor. Sin embargo, puede utilizar el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] si necesita volver a configurar las conexiones del servidor de modo que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] escuche en un protocolo de red, puerto o canalización concretos. Para obtener más información sobre cómo habilitar protocolos, vea [Habilitar o deshabilitar un protocolo de red de servidor](../database-engine/configure-windows/enable-or-disable-a-server-network-protocol.md). Para obtener más información sobre cómo habilitar el acceso a los protocolos a través de un firewall, vea [Configurar Firewall de Windows para permitir el acceso a SQL Server](../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md).  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] El Administrador de configuración permite administrar protocolos de red de cliente y servidor, lo que incluye la posibilidad de exigir el cifrado del protocolo, ver las propiedades del alias o habilitar o deshabilitar un protocolo.  
  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] permite crear o quitar un alias, cambiar el orden en el que se utilizan los protocolos o ver las propiedades de un alias de servidor, incluyendo:  
  
-   Alias de servidor: alias de servidor que usa el equipo al que se está conectando el cliente.  
  
-   Protocolo: protocolo de red que se usa para la entrada de configuración.  
  
-   Parámetros de conexión: parámetros asociados a la dirección de conexión para la configuración del protocolo de red.  
  
 El Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] también permite ver información sobre las instancias del clúster de conmutación por error, aunque se debe utilizar el Administrador de clústeres para algunas acciones como el inicio y la detención de los servicios.  
  
### <a name="available-network-protocols"></a>Protocolos de red disponibles  
 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] es compatible con los protocolos Memoria compartida, TCP/IP y Canalizaciones con nombre. Para obtener más información acerca de cómo elegir un protocolo de red, vea [Configure Client Protocols](../database-engine/configure-windows/configure-client-protocols.md). [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] no admite los protocolos de red VIA (Protocolo de paquetes secuenciados) de Banyan VINES, Multiprotocolo, AppleTalk o NWLink SPP/IPX. Los clientes anteriormente conectados con estos protocolos deben seleccionar uno distinto para conectarse a [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. No es posible utilizar el Administrador de configuración de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] para configurar el proxy WinSock. Para configurar el proxy WinSock, consulte la documentación de ISA Server.  
  
## <a name="related-tasks"></a>Related Tasks  
 [Temas de procedimientos de administración de servicios &#40;Administrador de configuración de SQL Server&#41;](../database-engine/configure-windows/scm-services-connect-to-another-computer.md)  
  
 [Iniciar, detener, pausar, reanudar y reiniciar el motor de base de datos, Agente SQL Server o el Servicio SQL Server Browser](../database-engine/configure-windows/start-stop-pause-resume-restart-sql-server-services.md)  
  
 [Iniciar, detener o pausar el servicio del Agente SQL Server](../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
  
 [Configurar una instancia de SQL Server para que se inicie automáticamente &#40;Administrador de configuración de SQL Server&#41;](../database-engine/configure-windows/scm-services-set-an-instance-to-start-automatically.md)  
  
 [Evitar el inicio automático de una instancia de SQL Server &#40;Administrador de configuración de SQL Server&#41;](../database-engine/configure-windows/scm-services-prevent-automatic-startup-of-an-instance.md)  
  
