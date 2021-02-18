---
description: Assign Alerts to an Operator
title: Assign Alerts to an Operator
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- assigning alerts to operator
- SQL Server Agent, alerts
- alerts [SQL Server], assigning to operator
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: aa818155-6fa2-4565-a09f-5c7e31c89754
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: e204b890d848cb8307c09e7fb96eb82322a52f63
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354882"
---
# <a name="assign-alerts-to-an-operator"></a>Assign Alerts to an Operator

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo asignar alertas del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a los operadores para que puedan recibir notificaciones de trabajos en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] proporciona una sencilla forma gráfica de administrar todo el sistema de alertas. Se recomienda utilizar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para configurar la infraestructura de alertas.  
  
-   Para enviar una notificación como respuesta a una alerta, primero debe configurar el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para el envío de correo. Para más información, consulte [Configurar el Agente SQL Server para que use el Correo electrónico de base de datos](../../relational-databases/database-mail/configure-sql-server-agent-mail-to-use-database-mail.md).  
  
-   Si se produce algún error al enviar un mensaje de correo electrónico o una notificación por buscapersonas, el error se comunica en el registro de errores de servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Solo los miembros del rol fijo de servidor **sysadmin** pueden asignar alertas a operadores.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-assign-alerts-to-an-operator"></a>Para asignar alertas a un operador  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene el operador al que desea asignar una alerta.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic en el signo más para expandir la carpeta **Operadores** .  
  
4.  Haga clic con el botón derecho en el operador al que desea asignar una alerta y seleccione **Propiedades** y, luego, la página **Notificaciones** .  
  
5.  En el cuadro de diálogo **Propiedades de**_nombre\_operador_, en **Seleccionar una página**, seleccione **Notificaciones**.  
  
6.  En **Ver las notificaciones enviadas a este usuario por**, seleccione **Alertas** para ver una lista de las alertas enviadas a este operador o seleccione **Trabajos** para ver una lista de los trabajos que envían notificaciones a este operador. Active una o varias de las siguientes casillas para definir el método de notificación de cada notificación según corresponda: **Correo electrónico**, **Buscapersonas** o **Net send**.  
  
7.  Cuando termine, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-assign-alerts-to-an-operator"></a>Para asignar alertas a un operador  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- adds an e-mail notification for the specified alert (Test Alert)  
    -- This example assumes that Test Alert already exists
    -- and that François Ajenstat is a valid operator name.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_notification  
     @alert_name = N'Test Alert',  
     @operator_name = N'François Ajenstat',  
     @notification_method = 1 ;  
    GO  
    ```  
  
Para más información, consulte [sp_add_notification (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-notification-transact-sql.md).  
