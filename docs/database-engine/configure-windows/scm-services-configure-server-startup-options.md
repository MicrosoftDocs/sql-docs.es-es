---
title: Configurar opciones de inicio del servidor (Administrador de configuración de SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo establecer las opciones que el Motor de base de datos de SQL Server utiliza al iniciarse. Vea las limitaciones y restricciones de la realización de cambios en los parámetros de inicio.
ms.custom: ''
ms.date: 11/23/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- parameters [SQL Server], startup options
- SQL Server, startup options
- SQL Server, startup parameters
- single-user mode [SQL Server], starting in
- startup options [SQL Server]
- startup parameters [SQL Server]
- SQL Server services, setting startup options
- SQL Server services, setting startup parameters
ms.assetid: 7a94643c-6460-4baf-bb31-0cb99eaf970d
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 13240827e460c1bb64f3ae41e648d4dd44842105
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236667"
---
# <a name="scm-services---configure-server-startup-options"></a>Servicios SCM - Configurar opciones de inicio del servidor
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar las opciones de inicio que se utilizarán cada vez que el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se inicie en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener una lista de opciones de inicio, vea [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
### <a name="limitations-and-restrictions"></a>Limitaciones y restricciones  
 El Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] escribe los parámetros de inicio en el Registro. Estos surten efecto en el siguiente inicio de [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 En un clúster, se deben efectuar cambios en el servidor activo cuando [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] está en línea y surtirán efecto cuando el [!INCLUDE[ssDE](../../includes/ssde-md.md)] se reinicie. La actualización de las opciones de inicio del Registro del otro nodo se producirá tras la siguiente conmutación por error.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 La configuración de opciones de inicio del servidor está restringida a los usuarios que pueden cambiar las entradas relacionadas del Registro. Esto incluye a los usuarios siguientes.  
  
-   Miembros del grupo local de administradores.  
  
-   La cuenta de dominio utilizada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], si el [!INCLUDE[ssDE](../../includes/ssde-md.md)] está configurado para ejecutarse bajo una cuenta de dominio.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-configure-startup-options"></a>Para configurar las opciones de inicio  
  
1.  Haga clic en el botón **Inicio** , seleccione **Todos los programas**, seleccione [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], seleccione **Herramientas de configuración** y, a continuación, haga clic en **Administrador de configuración de SQL Server**.  
  
    > [!NOTE]  
    >  Como el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un complemento del programa [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console y no un programa independiente, el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no aparece como aplicación en las versiones más recientes de Windows.  
    >   
    >  -   **Windows 10**:  
    >          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , escriba SQLServerManager13.msc (para **) en la** página de inicio [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Para versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , reemplace el 13 por un número inferior. Al hacer clic en SQLServerManager13.msc, se abre el Administrador de configuración. Para anclar el Administrador de configuración a la página de inicio o a la barra de tareas, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Abrir ubicación del archivo**. En el Explorador de archivos de Windows, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Anclar a Inicio** o **Anclar a la barra de tareas**.  
    >  -   **Windows 8**:  
    >          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], escriba **SQLServerManager\<version>.msc** (por ejemplo, **SQLServerManager13.msc**) en el acceso a **Buscar** de **Aplicaciones** y, después, presione **Entrar**.  
  
2.  En el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , haga clic en **Servicios de SQL Server**.  
  
3.  En el panel derecho, haga clic con el botón derecho en **SQL Server (** _<nombre_instancia>_ **)** y, luego, haga clic en **Propiedades**.  
  
4.  En la pestaña **Parámetros de inicio** , en el cuadro **Especifique un parámetro de inicio** , escriba el parámetro y, a continuación, haga clic en **Agregar**.  
  
     Por ejemplo, para iniciarlo en modo de usuario único, escriba **-m** en el cuadro **Especifique un parámetro de inicio** y, después, haga clic en **Agregar**. (Al reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en modo de usuario único, detenga el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . De lo contrario, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría conectarse primero e impedir que se conecte como un segundo usuario).  
  
5.  [!INCLUDE[clickOK](../../includes/clickok-md.md)]  
  
6.  Reinicie el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
    > [!WARNING]  
    >  Cuando haya terminado de usar el modo de usuario único, en el cuadro Parámetros de inicio, seleccione el parámetro **-m** en el cuadro **Parámetros existentes** y, después, haga clic en **Quitar**. Reinicie el [!INCLUDE[ssDE](../../includes/ssde-md.md)] para restaurar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al modo típico multiusuario.  
  
## <a name="see-also"></a>Consulte también  
 [Iniciar SQL Server en modo de usuario único](../../database-engine/configure-windows/start-sql-server-in-single-user-mode.md)   
 [Conectarse a SQL Server cuando los administradores del sistema no tienen acceso](../../database-engine/configure-windows/connect-to-sql-server-when-system-administrators-are-locked-out.md)   
 [Iniciar, detener o pausar el servicio del Agente SQL Server](../../ssms/agent/start-stop-or-pause-the-sql-server-agent-service.md)  
 [Opciones de inicio del servicio de motor de base de datos](../../database-engine/configure-windows/database-engine-service-startup-options.md) 
  
