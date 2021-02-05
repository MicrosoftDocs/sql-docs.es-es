---
description: Definición de la respuesta a una alerta
title: Definición de la respuesta a una alerta
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], responding to
- responding to alerts
ms.assetid: c86ca6eb-c59f-46e9-bc32-d474e7c3b170
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 653bb42c116654496bd1b51b01e3d8487bd51804
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234582"
---
# <a name="define-the-response-to-an-alert"></a>Definición de la respuesta a una alerta

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo definir el modo en que [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] responde a las alertas del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   Las opciones Buscapersonas y **net send** se quitarán del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan.  
  
-   Tenga en cuenta que deberá configurar el Agente SQL Server para que utilice el Correo electrónico de base de datos para enviar a los operadores notificaciones por correo electrónico o buscapersonas. Para obtener más información, vea el tema sobre [asignación de alertas a un operador](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Solo los miembros del rol fijo de servidor **sysadmin** pueden definir la respuesta a una alerta.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-define-the-response-to-an-alert"></a>Para definir la respuesta a una alerta  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene la alerta en la que desea definir una respuesta.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic en el signo más para expandir la carpeta **Alertas** .  
  
4.  Haga clic con el botón derecho en la alerta en la que desea definir una respuesta y seleccione **Propiedades**.  
  
5.  En el cuadro de diálogo _alert\_name_**Propiedades de la alerta**, en **Seleccionar una página**, seleccione **Respuesta**.  
  
6.  Active la casilla **Ejecutar trabajo** y, en la lista situada debajo de la casilla **Ejecutar trabajo** , seleccione el trabajo que se ejecutará cuando se produzca la alerta. Puede crear un nuevo trabajo haciendo clic en **Nuevo trabajo**. Puede ver más información acerca del trabajo haciendo clic en **View Job**. Para más información sobre las opciones disponibles en los cuadros de diálogo **Nuevo trabajo** y **Propiedades de trabajo**_nombre\_trabajo_, vea [Crear un trabajo](../../ssms/agent/create-a-job.md) y [Ver un trabajo](../../ssms/agent/view-a-job.md).  
  
7.  Active la casilla **Notificar a los operadores** si desea notificar a los operadores cuándo se activará la alerta. En **Lista de operadores**, seleccione uno o más de los métodos siguientes para notificar al operador o a los operadores: **Correo electrónico**, **Buscapersonas** o **Net Send**. Puede crear un nuevo operador haciendo clic en **Nuevo operador**. Puede ver más información acerca de un operador haciendo clic en **Ver operador**. Para obtener más información acerca de las opciones disponibles en los cuadros de diálogo **Nuevo operador** y **Ver las propiedades del operador** , vea [Create an Operator](../../ssms/agent/create-an-operator.md) y [View Information About an Operator](../../ssms/agent/view-information-about-an-operator.md).  
  
8.  Cuando termine, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-define-the-response-to-an-alert"></a>Para definir la respuesta a una alerta  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- adds an e-mail notification for Test Alert.  
    -- assumes that Test Alert already exists and that
    -- François Ajenstat is a valid operator name   
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Para más información, consulte [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).