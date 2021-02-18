---
description: Create an Operator
title: Create an Operator
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, operators
- jobs [SQL Server Agent], notification options
- operators (users) [Database Engine], creating with Management Studio
- SQL Server Agent jobs, notification options
- jobs [SQL Server Agent], operators
- notifications [SQL Server], job status
ms.assetid: 1359d790-5905-4927-a208-e7155e7768a2
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 0460e2726c58a56f88ba11f5a515718cac82199c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342732"
---
# <a name="create-an-operator"></a>Create an Operator
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo configurar un usuario para que reciba notificaciones acerca de los trabajos del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] utilizando [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   Las opciones Buscapersonas y **net send** se quitarán del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en una versión futura de [!INCLUDE[msCoName](../../includes/msconame_md.md)][!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Evite utilizar estas características en los nuevos trabajos de programación y planee modificar las aplicaciones que actualmente las utilizan.  
  
-   Tenga en cuenta que deberá configurar el Agente SQL Server para que utilice el Correo electrónico de base de datos para enviar a los operadores notificaciones por correo electrónico o buscapersonas. Para obtener más información, vea el tema sobre [asignación de alertas a un operador](assign-alerts-to-an-operator.md).  
  
-   [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ofrece un método gráfico sencillo para administrar trabajos y es el método recomendado para crear y administrar la infraestructura de trabajo.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Solo los miembros del rol fijo de servidor **sysadmin** pueden crear operadores.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-an-operator"></a>Para crear un operador  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en el que desea crear un operador del Agente SQL Server.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en la carpeta **Operadores** y, luego, seleccione **Nuevo operador**.  
  
    Las siguientes opciones están disponibles en la página **General** del cuadro de diálogo **Nuevo operador** :  
  
    **Nombre**  
    Cambie el nombre del operador.  
  
    **Habilitado**  
    Habilite el operador. Si no se hablita, no se le envía ninguna notificación.  
  
    **Nombre de correo electrónico**  
    Especifica la dirección de correo electrónico del operador.  
  
    **Dirección de NET SEND**  
    Especifique la dirección que se va a usar para **net send**.  
  
    **Correo electrónico del buscapersonas**  
    Especifica la dirección de correo electrónico que debe utilizarse para el buscapersonas del operador.  
  
    **Programación de buscapersonas en servicio**  
    Establece las horas a las que el buscapersonas está activo.  
  
    **Lunes - Viernes**  
    Seleccione los días en los que el buscapersonas está activo.  
  
    **Inicio del día laborable**  
    Seleccione la hora del día a partir de la cual el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] envía mensajes al buscapersonas.  
  
    **Fin del día laborable**  
    Seleccione la hora del día a partir de la cual el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] deja de enviar mensajes al buscapersonas.  
  
    Las siguientes opciones están disponibles en la página **Notificaciones** del cuadro de diálogo **Nuevo operador** :  
  
    **Alertas**  
    Muestra las alertas de la instancia.  
  
    **Trabajos**  
    Muestra los trabajos de la instancia.  
  
    **Lista de alertas**  
    Enumera las alertas de la instancia.  
  
    **Lista de trabajos**  
    Enumera los trabajos de la instancia.  
  
    **Correo electrónico**  
    Envía la notificación al operador por correo electrónico.  
  
    **Buscapersonas**  
    Envía la notificación al operador mediante un mensaje de correo electrónico a la dirección del buscapersonas.  
  
    **NET SEND**  
    Notifique a este operador con **net send**.  
  
4.  Cuando termine de crear el nuevo operador, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-create-an-operator"></a>Para crear un operador  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- sets up the operator information for user 'danwi.'
    -- The operator is enabled.   
    -- SQL Server Agent sends notifications by pager 
    -- from Monday through Friday from 8 A.M. to 5 P.M.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_add_operator  
        @name = N'Dan Wilson',  
        @enabled = 1,  
        @email_address = N'danwi',  
        @pager_address = N'5551290AW@pager.Adventure-Works.com',  
        @weekday_pager_start_time = 080000,  
        @weekday_pager_end_time = 170000,  
        @pager_days = 62 ;  
    GO  
    ```  
  
Para más información, consulte [sp_add_operator (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-operator-transact-sql.md).  
