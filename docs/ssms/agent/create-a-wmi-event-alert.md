---
description: Create a WMI Event Alert
title: Create a WMI Event Alert
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- WMI event alerts [SQL Server Management Studio]
ms.assetid: b8c46db6-408b-484e-98f0-a8af3e7ec763
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 9dcb25ede4558b9a0980cc2443f7b0142f368a03
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236421"
---
# <a name="create-a-wmi-event-alert"></a>Create a WMI Event Alert
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo crear una alerta del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que se genera cuando se produce un evento de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] específico supervisado por el proveedor WMI para eventos de servidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Para información sobre cómo usar el proveedor de WMI para supervisar los eventos de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [Proveedor de VMI de clases y propiedades de eventos de servidor](../../relational-databases/wmi-provider-server-events/wmi-provider-for-server-events-concepts.md). Para más información sobre los permisos necesarios para recibir notificaciones de alertas de eventos de WMI, consulte [Seleccionar una cuenta para el servicio Agente SQL Server](../../ssms/agent/select-an-account-for-the-sql-server-agent-service.md). Para más información sobre WQL, consulte [Usar WQL con el proveedor de WMI para eventos de servidor](../../relational-databases/wmi-provider-server-events/using-wql-with-the-wmi-provider-for-server-events.md).  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] proporciona una forma gráfica y fácil de administrar todo el sistema de alertas, y constituye el método recomendado para configurar una infraestructura de alertas.  
  
-   Los eventos generados durante **xp_logevent** se producen en la base de datos maestra. Por tanto, **xp_logevent** no desencadena una alerta a menos que el valor de **\@database_name** de la alerta sea **'master'** o NULL.  
  
-   Solo se admiten los espacios de nombres WMI del equipo que ejecuta el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
De forma predeterminada, solo los miembros del rol fijo de servidor **sysadmin** pueden ejecutar **sp_add_alert**.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-a-wmi-event-alert"></a>Para crear una alerta de evento WMI  
  
1.  En el **Explorador de objetos** , haga clic en el signo más para expandir el servidor donde desea crear una alerta de evento WMI.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en **Alertas** y seleccione **Nueva alerta**.  
  
4.  En el cuadro de diálogo **Nueva alerta** , en el cuadro **Nombre** , escriba un nombre para esta alerta.  
  
5.  Active la casilla **Habilitar** para que la alerta se pueda ejecutar. De forma predeterminada, la opción **Habilitar** está activada.  
  
6.  En la lista **Tipo** , seleccione **Alerta de evento WMI**.  
  
7.  En **Definición de evento de alerta de WMI**, en el cuadro **Espacio de nombres** , especifique el espacio de nombres WMI para la instrucción WQL (Lenguaje de consulta de WMI) que identifica qué evento WMI activará esta alerta.  
  
8.  En el cuadro **Consulta** , especifique la instrucción WQL que identifica el evento al que responde esta alerta.  
  
9. Haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-create-a-wmi-event-alert"></a>Para crear una alerta de evento WMI  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- creates a WMI event alert that retrieves all event properties for any ALTER_TABLE event that occurs on table AdventureWorks2012.Sales.SalesOrderDetail  
    -- This example assumes that the message 54001 already exists.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_alert  
        @name = N'Test Alert 2',  
        @message_id = 54001  
        @notification_message = N'Error 54001 has occurred on the Sales.SalesOrderDetail table on the AdventureWorks2012 database.',  
        @wmi_namespace = '\\.\root\Microsoft\SqlServer\ServerEvents\,  
        @wmi_query = N'SELECT * FROM ALTER_TABLE   
    WHERE DatabaseName = 'AdventureWorks2012' AND SchemaName = 'Sales'   
        AND ObjectType='Table' AND ObjectName = 'SalesOrderDetail'';  
    GO  
    ```  
  
Para más información, consulte [sp_add_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-alert-transact-sql.md).  
