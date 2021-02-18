---
description: Create a SQL Server Agent Proxy
title: Create a SQL Server Agent Proxy
ms.custom: seo-lt-2019
ms.date: 05/04/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- proxies [SQL Server Agent], creating
ms.assetid: 142e0c55-a8b9-4669-be49-b9dc602d5988
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7967f1a56b6198123178407930eba4c986e098a1
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100271712"
---
# <a name="create-a-sql-server-agent-proxy"></a>Create a SQL Server Agent Proxy
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo crea un proxy del Agente SQL Server en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Una cuenta de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] define un contexto de seguridad en el que es posible ejecutar un paso de trabajo. Cada proxy se corresponde con unas credenciales de seguridad. Para establecer los permisos para un paso de trabajo concreto, cree un proxy que disponga de los permisos necesarios para un subsistema del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y, a continuación, asigne ese proxy al paso de trabajo.  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   Debe crear una credencial antes de crear un proxy si todavía no hay uno disponible.  
  
-   Las cuentas de proxy del Agente[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan credenciales para almacenar información acerca de las cuentas de usuario de Windows. El usuario especificado en las credenciales debe tener el permiso "Tener acceso a este equipo desde la red" (SeNetworkLogonRight) en el equipo en el que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba el acceso al subsistema de un proxy y da acceso al proxy cada vez que se ejecuta el paso de trabajo. Si el proxy ya no tiene acceso al subsistema, el paso de trabajo da error. De lo contrario, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suplanta al usuario especificado en el proxy y ejecuta el paso de trabajo.  
  
-   La creación de un proxy no cambia los permisos del usuario especificado en las credenciales del proxy. Por ejemplo, puede crear un proxy para un usuario que no tiene permisos para conectarse a una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En este caso, los pasos de trabajo que usan el proxy no pueden conectarse a [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   Si el inicio de sesión del usuario tiene acceso al proxy o si el usuario pertenece a un rol con acceso al proxy, puede usarlo en un paso de trabajo.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
  
-   Solo los miembros del rol fijo de servidor **sysadmin** disponen del permiso necesario para crear, modificar o eliminar cuentas de proxy. Los usuarios que no son miembros del rol fijo de servidor **sysadmin** deben agregarse a uno de los siguientes roles fijos de base de datos del Agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en la base de datos **msdb** para usar los servidores proxy: **SQLAgentUserRole**, **SQLAgentReaderRole** o **SQLAgentOperatorRole**.  
  
-   Requiere el permiso **ALTER ANY CREDENTIAL** si crea una credencial además del proxy.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-create-a-sql-server-agent-proxy"></a>Para crear un proxy del Agente SQL Server  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor en el que desea crear un proxy del Agente SQL Server.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en la carpeta **Servidores proxy** y seleccione **Nuevo proxy**.  
  
4.  En del cuadro de diálogo **Nueva cuenta de proxy** , en la página **General** , especifique el nombre de la cuenta de proxy en el cuadro **Nombre del proxy** .  
  
5.  En el cuadro **Nombre de credencial** , escriba el nombre de la credencial de seguridad que la cuenta de proxy utilizará.  
  
6.  En el cuadro de **Descripción** , escriba una descripción de la cuenta de proxy  
  
7.  En **Activar para los subsistemas siguientes**, seleccione el subsistema o los subsistemas apropiados para este proxy.  
  
8.  En la página **Entidades de seguridad** , agregue o quite inicios de sesión o roles para conceder o quitar el acceso a la cuenta de proxy.  
  
9. Cuando termine, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-create-a-sql-server-agent-proxy"></a>Para crear un proxy del Agente SQL Server  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- creates credential CatalogApplicationCredential  
    USE msdb ;  
    GO  
    CREATE CREDENTIAL CatalogApplicationCredential WITH IDENTITY = 'REDMOND/TestUser',   
        SECRET = 'G3$1o)lkJ8HNd!';  
    GO  
    -- creates proxy "Catalog application proxy" and assigns
    -- the credential 'CatalogApplicationCredential' to it.  
    EXEC dbo.sp_add_proxy  
        @proxy_name = 'Catalog application proxy',  
        @enabled = 1,  
        @description = 'Maintenance tasks on catalog application.',  
        @credential_name = 'CatalogApplicationCredential' ;  
    GO  
    -- grants the proxy "Catalog application proxy" access to 
    -- the ActiveX Scripting subsystem.  
    EXEC dbo.sp_grant_proxy_to_subsystem  
        @proxy_name = N'Catalog application proxy',  
        @subsystem_id = 2 ;  
    GO  
    ```  
  
Para más información, consulte:  
  
-   [CREATE CREDENTIAL (Transact-SQL)](../../t-sql/statements/create-credential-transact-sql.md)  
  
-   [sp_add_proxy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-add-proxy-transact-sql.md)  
  
-   [sp_grant_proxy_to_subsystem (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-grant-proxy-to-subsystem-transact-sql.md)  
