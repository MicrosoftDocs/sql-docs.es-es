---
title: Servicios SCM - Cambiar la contraseña de las cuentas usadas | Microsoft Docs
description: Obtenga información sobre cómo cambiar la contraseña de las cuentas que usan el Motor de base de datos y el Agente SQL Server. Obtenga información sobre cuándo es importante cambiar la contraseña.
ms.custom: ''
ms.date: 01/06/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- expired password [SQL Server], SQL Server Agent
- passwords [SQL Server], SQL Server Agent service
- passwords [SQL Server], changing
- expired password [SQL Server], Database Engine
- passwords [SQL Server], SQL Server service
- Database Engine [SQL Server], passwords
- changing passwords used by SQL Server
- modifying passwords
ms.assetid: 5b6dcc03-6cae-45d3-acef-6f85ca6d615f
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 6bb8def79d5fd606f210273d232491d858baa3cf
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233323"
---
# <a name="scm-services---change-the-password-of-the-accounts-used"></a>Servicios SCM - Cambiar la contraseña de las cuentas usadas
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo cambiar la contraseña de las cuentas utilizadas por el [!INCLUDE[ssDE](../../includes/ssde-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante el Administrador de configuración de SQL Server. El [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] y el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecutan en un equipo como un servicio mediante el uso de credenciales suministradas inicialmente durante la instalación. Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se ejecuta con una cuenta de dominio y se cambia la contraseña de esa cuenta, la contraseña utilizada por [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe actualizarse. Si la contraseña no se actualiza, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede perder el acceso a algunos recursos del dominio y si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se detiene, el servicio no se reiniciará hasta que la contraseña se actualice.  
  
 Para cambiar las contraseñas de autenticación de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vea [Contraseña expirada](../../relational-databases/security/choose-an-authentication-mode.md).  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es la herramienta diseñada y autorizada para cambiar la configuración de los servicios de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . El cambio de un servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante la aplicación Administrador de control de servicios de Windows (**services.msc**) no siempre cambia todos los valores necesarios y puede impedir que el servicio funcione correctamente. Sin embargo, en un entorno en clúster, después de cambiar la contraseña en el nodo activo mediante el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , debe cambiar la contraseña en el nodo pasivo con el Administrador de control de servicios.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para cambiar la contraseña que utiliza un servicio debe ser el administrador del equipo.  
  
##  <a name="using-sql-server-configuration-manager"></a><a name="SSMSProcedure"></a> Usar el Administrador de configuración de SQL Server  
  
#### <a name="to-change-the-password-used-by-the-sql-server-database-engine-service"></a>Para cambiar la contraseña utilizada por el servicio SQL Server (motor de base de datos)  
  
1.  Haga clic en el botón **Inicio** , seleccione **Todos los programas**, seleccione [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], seleccione **Herramientas de configuración** y, a continuación, haga clic en **Administrador de configuración de SQL Server**.  
  
    > [!NOTE]  
    >  Como el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un complemento del programa [!INCLUDE[msCoName](../../includes/msconame-md.md)] Management Console y no un programa independiente, el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no aparece como aplicación en las versiones más recientes de Windows.  
    >   
    >  -   **Windows 10**:  
    >          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , escriba SQLServerManager13.msc (para **) en la** página de inicio [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)]. Para versiones anteriores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , reemplace el 13 por un número inferior. Al hacer clic en SQLServerManager13.msc, se abre el Administrador de configuración. Para anclar el Administrador de configuración a la página de inicio o a la barra de tareas, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Abrir ubicación del archivo**. En el Explorador de archivos de Windows, haga clic con el botón derecho en SQLServerManager13.msc y, después, haga clic en **Anclar a Inicio** o **Anclar a la barra de tareas**.  
    > -   **Windows 8**:  
    >          Para abrir el Administrador de configuración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], escriba **SQLServerManager\<version>.msc** (por ejemplo, **SQLServerManager13.msc**) en el acceso a **Buscar** de **Aplicaciones** y, después, presione **Entrar**.  
  
2.  En Administrador de configuración de SQL Server, haga clic en **Servicios de SQL Server**.  
  
3.  En el panel de detalles, haga clic con el botón derecho en **SQL Server (** \<instancename> **)** y, después, haga clic en **Propiedades**.  
  
4.  En el cuadro de diálogo **Propiedades de (** \<instancename> **) de SQL Server**, en la pestaña Iniciar sesión, en la cuenta que aparece en el cuadro **Nombre de cuenta**, escriba la nueva contraseña en los cuadros **Contraseña** y **Confirmar contraseña**, y después haga clic en **Aceptar**.  
  
     La contraseña surte efecto inmediatamente, sin necesidad de reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-change-the-password-used-by-the-sql-server-agent-service"></a>Para cambiar la contraseña que utiliza el servicio del Agente SQL Server  
  
1.  Haga clic en el botón **Inicio** , seleccione **Todos los programas**, seleccione [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], seleccione **Herramientas de configuración** y, a continuación, haga clic en **Administrador de configuración de SQL Server**.  
  
2.  En Administrador de configuración de SQL Server, haga clic en **Servicios de SQL Server**.  
  
3.  En el panel de detalles, haga clic con el botón derecho en **Agente SQL Server (** \<instancename> **)** y, después, haga clic en **Propiedades**.  
  
4.  En el cuadro de diálogo **Propiedades de (** \<instancename> **) de Agente SQL Server**, en la pestaña Iniciar sesión, en la cuenta que aparece en el cuadro **Nombre de cuenta**, escriba la nueva contraseña en los cuadros **Contraseña** y **Confirmar contraseña**, y después haga clic en **Aceptar**.  
  
     En una instancia independiente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], la contraseña surte efecto inmediatamente, sin necesidad de reiniciar [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En una instancia en clúster, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] podría cambiar el recurso de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a sin conexión y requerir un reinicio.  
  
## <a name="see-also"></a>Consulte también  
 [Temas de procedimientos de administración de servicios &#40;Administrador de configuración de SQL Server&#41;](./scm-services-connect-to-another-computer.md)  
  
