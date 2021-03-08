---
title: Ver o cambiar las propiedades del servidor (SQL Server)
description: Obtenga información sobre cómo usar SQL Server Management Studio, Transact-SQL o el Administrador de configuración de SQL Server para ver o cambiar las propiedades de una instancia de SQL Server.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
f1_keywords:
- sql13.swb.connectionproperties.f1
helpviewer_keywords:
- viewing server properties
- server properties [SQL Server]
- displaying server properties
- servers [SQL Server], viewing
- Connection Properties dialog box
ms.assetid: 55f3ac04-5626-4ad2-96bd-a1f1b079659d
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: contperf-fy20q4
ms.date: 03/14/2017
ms.openlocfilehash: 80d52c19e5fde8116f7f6fed638e1120064038d3
ms.sourcegitcommit: ca81fc9e45fccb26934580f6d299feb0b8ec44b7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2021
ms.locfileid: "102186073"
---
# <a name="view-or-change-server-properties-sql-server"></a>Ver o cambiar las propiedades del servidor (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo ver o cambiar las propiedades de una instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)]o el Administrador de configuración de SQL Server.  

Los pasos dependen de la herramienta:
+ [SQL Server Management Studio](#SSMSProcedure)  
+ [Transact-SQL](#TsqlProcedure)  
+ [Administrador de configuración de SQL Server](#PowerShellProcedure)  
    
## <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
  
-   Cuando utilice sp_configure, deberá ejecutar RECONFIGURE o RECONFIGURE WITH OVERRIDE después de establecer una opción de configuración. La instrucción RECONFIGURE WITH OVERRIDE se suele reservar para opciones de configuración que deberían utilizarse con extrema precaución. No obstante, RECONFIGURE WITH OVERRIDE funciona con todas las opciones de configuración y puede utilizarlo en lugar de RECONFIGURE.  
  
    > [!NOTE]  
    >  RECONFIGURE se ejecuta en una transacción. Si una de las operaciones de reconfiguración genera un error, ninguna de estas operaciones surtirá efecto.  
  
-   Algunas páginas de propiedades presentan información obtenida mediante Instrumental de administración de Windows (WMI). Para mostrar dichas páginas, WMI debe estar instalado en el equipo en el que se ejecuta [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)].  
  
## <a name="server-level-roles"></a><a name="Security"></a>Roles de nivel de servidor  
  
Para obtener más información, vea [Roles de nivel de servidor](../../relational-databases/security/authentication-access/server-level-roles.md).  
  
De forma predeterminada, todos los usuarios tienen permisos de ejecución en **sp_configure** sin ningún parámetro o solo con el primero. Para ejecutar **sp_configure** con ambos parámetros y cambiar una opción de configuración, o para ejecutar la instrucción RECONFIGURE, un usuario debe tener el permiso ALTER SETTINGS en el servidor. Los roles fijos de servidor **sysadmin** y **serveradmin** tienen el permiso ALTER SETTINGS de forma implícita.  
  
## <a name="sql-server-management-studio"></a><a name="SSMSProcedure"></a>SQL Server Management Studio  
  
#### <a name="to-view-or-change-server-properties"></a>Para ver o cambiar las propiedades del servidor  
  
1.  En el Explorador de objetos, haga clic con el botón derecho en un servidor y luego haga clic en **Propiedades**.  
  
2.  En el cuadro de diálogo **Propiedades del servidor** , haga clic en una página para ver o cambiar la información de servidor acerca de dicha página. Algunas propiedades son de solo lectura.  
  
##  <a name="transact-sql"></a><a name="TsqlProcedure"></a>Transact-SQL  
  
### <a name="to-view-server-properties-by-using-the-serverproperty-built-in-function"></a>Para ver las propiedades del servidor mediante la función integrada SERVERPROPERTY  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se emplea la función integrada [SERVERPROPERTY](../../t-sql/functions/serverproperty-transact-sql.md) en una instrucción `SELECT` para devolver información sobre el servidor actual. Este escenario es útil cuando hay varias instancias de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instaladas en un servidor basado en Windows y el cliente debe abrir otra conexión a la misma instancia usada por la conexión actual.  
  
    ```sql  
    SELECT CONVERT( sysname, SERVERPROPERTY('servername'));  
    GO  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysservers-catalog-view"></a>Para ver las propiedades del servidor mediante la vista de catálogo sys.servers  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**. En este ejemplo se consulta la vista de catálogo [sys.servers](../../relational-databases/system-catalog-views/sys-servers-transact-sql.md) para devolver el nombre (`name`) y el identificador (`server_id`) del servidor actual, además del nombre del proveedor OLE DB (`provider`) para conectarse a un servidor vinculado.  
  
    ```sql  
    USE AdventureWorks2012;   
    GO  
    SELECT name, server_id, provider  
    FROM sys.servers ;   
    GO  
  
    ```  
  
### <a name="to-view-server-properties-by-using-the-sysconfigurations-catalog-view"></a>Para ver las propiedades del servidor mediante la vista de catálogo sys.configurations  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. En este ejemplo se consulta la vista de catálogo [sys.configurations](../../relational-databases/system-catalog-views/sys-configurations-transact-sql.md) para devolver información sobre cada opción de configuración del servidor actual. El ejemplo devuelve el nombre (`name`) y la descripción (`description`) de la opción, su valor (`value`) y si se trata de una opción avanzada (`is_advanced`).  
  
    ```wmimof  
    USE AdventureWorks2012;   
    GO  
    SELECT name, description, value, is_advanced  
    FROM sys.configurations;   
    GO  
  
    ```  
  
### <a name="to-change-a-server-property-by-using-sp_configure"></a>Para cambiar una propiedad del servidor mediante sp_configure  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, seleccione **Nueva consulta**.  
  
3.  Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**. Este ejemplo muestra cómo usar [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) para cambiar una propiedad del servidor. El ejemplo cambia el valor de la opción `fill factor` a `100`. El servidor debe reiniciarse para que el cambio surta efecto.  
  
```sql  
Use AdventureWorks2012;  
GO  
sp_configure 'show advanced options', 1;  
GO  
RECONFIGURE;  
GO  
sp_configure 'fill factor', 100;  
GO  
RECONFIGURE;  
GO  
```  
  
 Para obtener más información, vea [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
## <a name="sql-server-configuration-manager"></a><a name="PowerShellProcedure"></a>Administrador de configuración de SQL Server  
 Algunas propiedades del servidor se pueden ver o cambiar mediante el Administrador de configuración de SQL Server. Por ejemplo, puede ver la versión y la edición de la instancia de SQL Server, o cambiar la ubicación donde se almacenan los archivos de registro de errores. Estas propiedades también se pueden ver si se consultan las [Funciones y vistas de administración dinámica relacionadas con servidores](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md).  
  
### <a name="to-view-or-change-server-properties"></a>Para ver o cambiar las propiedades del servidor  
  
1.  En el menú **Inicio** , elija **Todos los programas**, [!INCLUDE[ssCurrentUI](../../includes/sscurrentui-md.md)], **Herramientas de configuración** y, por último, **Administrador de configuración de SQL Server**.  
  
2.  En **Administrador de configuración de SQL Server**, seleccione **Servicios de SQL Server**.  
  
3.  En el panel de detalles, haga clic con el botón derecho en **SQL Server (\<**_instancename_**>)** y seleccione **Propiedades**.  
  
4.  En el cuadro de diálogo **Propiedades de SQL Server (\<**_instancename_**>)** , cambie las propiedades del servidor en la pestaña **Servicio** o **Avanzado** y, después, seleccione **Aceptar**.  
  
## <a name="restart-after-changes"></a><a name="FollowUp"></a>Reinicio después de los cambios

Para algunas propiedades, puede que sea necesario reiniciar el servidor para que el cambio surta efecto.  
  
## <a name="next-steps"></a>Pasos siguientes  
 [Opciones de configuración de servidor &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [Instrucciones SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)   
 [SERVERPROPERTY &#40;Transact-SQL&#41;](../../t-sql/functions/serverproperty-transact-sql.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [SELECT &#40;Transact-SQL&#41;](../../t-sql/queries/select-transact-sql.md)   
 [Configurar WMI para mostrar el estado del servidor en Herramientas de SQL Server](../../ssms/configure-wmi-to-show-server-status-in-sql-server-tools.md)   
 [Administrador de configuración de SQL Server](../../relational-databases/sql-server-configuration-manager.md)   
 [Funciones de configuración &#40;Transact-SQL&#41;](../../t-sql/functions/configuration-functions-transact-sql.md)   
 [Funciones y vistas de administración dinámica relacionadas con servidores &#40;Transact-SQL&#41;](../../relational-databases/system-dynamic-management-views/server-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
