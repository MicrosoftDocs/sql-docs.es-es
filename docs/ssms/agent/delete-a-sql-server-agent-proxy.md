---
description: Delete a SQL Server Agent Proxy
title: Delete a SQL Server Agent Proxy
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- deleting SQL Server Agent proxies
- proxies [SQL Server Agent], deleting
- removing SQL Server Agent proxies
ms.assetid: 9248841d-7294-47d4-94f3-b34a0521fabc
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 71ca3a66ea1452f3accd70f0bfad7e7126b02936
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236586"
---
# <a name="delete-a-sql-server-agent-proxy"></a>Delete a SQL Server Agent Proxy
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo eliminar una cuenta de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
  
-   Cuando elimine una cuenta de proxy del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , asegúrese de que el proxy no haga referencia a ningún paso de trabajo activo. Para comprobar si existen pasos de trabajo que hacen referencia al proxy, haga clic con el botón derecho en el proxy, seleccione **Propiedades** y, en el cuadro de diálogo **Propiedades de cuenta de proxy**_nombre\_proxy_, seleccione la página **Referencias**. Si elimina un proxy, en el cuadro de diálogo **Eliminar objeto** se le ofrece la posibilidad de volver a asignar todos los pasos de trabajo que utilizan ese proxy.  
  
-   Las cuentas de proxy del Agente[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilizan credenciales para almacenar información acerca de las cuentas de usuario de Windows. El usuario especificado en las credenciales debe tener el permiso "Iniciar sesión como proceso por lotes" en el equipo en que se ejecuta [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comprueba el acceso al subsistema de un proxy y da acceso al proxy cada vez que se ejecuta el paso de trabajo. Si el proxy ya no tiene acceso al subsistema, el paso de trabajo da error. De lo contrario, el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] suplanta al usuario especificado en el proxy y ejecuta el paso de trabajo.  
  
-   Si el inicio de sesión del usuario tiene acceso al proxy o si el usuario pertenece a un rol con acceso al proxy, puede usarlo en un paso de trabajo.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Solo los miembros del rol fijo de servidor **sysadmin** pueden crear, modificar o eliminar cuentas de proxy.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-delete-a-sql-server-agent-proxy-account"></a>Para eliminar una cuenta de proxy del Agente SQL Server  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor que contiene la cuenta de proxy que desea eliminar.  
  
2.  Haga clic en el signo más para expandir **Agente SQL Server**.  
  
3.  Haga clic en el signo más para expandir la carpeta **Servidores proxy** .  
  
4.  Haga clic en el signo más para expandir el subsistema que contiene la cuenta de proxy que desee eliminar (por ejemplo, **Script de ActiveX)**.  
  
5.  Haga clic con el botón derecho en la cuenta de proxy que desea eliminar y seleccione **Eliminar**.  
  
6.  En el cuadro de diálogo **Eliminar objeto** , confirme que esté seleccionada la cuenta de proxy correcta. Active la casilla **Volver a asignar a** para volver a asignar a otra cuenta los pasos de trabajo que hacen referencia a esta cuenta de proxy.  
  
7.  Haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-delete-a-sql-server-agent-proxy-account"></a>Para eliminar una cuenta de proxy del Agente SQL Server  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- deletes the proxy "Catalog application proxy"  
    USE msdb ;  
    GO  
    EXEC dbo.sp_delete_proxy  
        @proxy_name = N'Catalog application proxy' ;  
    GO  
    ```  
  
Para más información, consulte [sp_delete_proxy (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-proxy-transact-sql.md).  
