---
title: Adición de una base de datos secundaria de trasvase de registros
description: Describe cómo agregar una base de datos secundaria a una configuración de trasvase de registros existente mediante SQL Server Management Studio o Transact-SQL en SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- adding secondary databases
- secondary databases [SQL Server], in log shipping
- secondary data files [SQL Server], adding
- log shipping [SQL Server], secondary databases
ms.assetid: b02eba13-f8e6-4684-b7e4-75ea038ea473
author: cawrites
ms.author: chadam
ms.openlocfilehash: 7b5a1a3bde0e2dbccb7e74670826fb29905016e1
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813200"
---
# <a name="add-a-secondary-database-to-a-log-shipping-configuration-sql-server"></a>Agregar una base de datos secundaria a la configuración del trasvase de registros (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo agregar una base de datos secundaria a una configuración de trasvase de registros de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] existente mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los procedimientos almacenados de trasvase de registros requieren que se pertenezca al rol fijo de servidor **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Para agregar una base de datos secundaria de trasvase de registros  
  
1.  Haga clic con el botón derecho en la base de datos que quiera usar como base de datos principal en la configuración de trasvase de registros y, después, haga clic en **Propiedades**.  
  
2.  En **Seleccionar una página**, haga clic en **Trasvase de registro de transacciones**.  
  
3.  En **Instancias de servidores secundarios y bases de datos**, haga clic en **Agregar**.  
  
4.  Haga clic en **Conectar** para conectarse a la sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desee utilizar como servidor secundario.  
  
5.  En el cuadro **Base de datos secundaria** , elija una base de datos de la lista o escriba el nombre de la base de datos que desea crear.  
  
6.  En la pestaña **Inicializar base de datos secundaria** , elija la opción que desee utilizar para inicializar la base de datos secundaria.  
  
7.  En la pestaña **Copiar archivos**, en el cuadro **Carpeta de destino de los archivos copiados** , escriba la ruta de la carpeta en la que se van a copiar las copias de seguridad de los registros de transacciones. Esta carpeta normalmente está situada en el servidor secundario.  
  
8.  Tenga presente la programación de copia que aparece en el cuadro **Programación** bajo **Trabajo de copia**. Si desea personalizar la programación de su instalación, haga clic en **Programar** y, a continuación, ajuste la programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según sus necesidades. El programa debe parecerse al de la copia de seguridad.  
  
9. En la pestaña **Restaurar** , en **Estado de la base de datos al restaurar copias de seguridad**, elija la opción **Modo sin recuperación** o **Modo de espera** .  
  
10. Si elige la opción **Modo de espera** , seleccione si desea desconectar a los usuarios de la base de datos secundaria mientras se realiza la operación de restauración.  
  
11. Si desea retrasar el proceso de restauración en el servidor secundario, elija un tiempo de retraso en **Retrasar la restauración de las copias de seguridad al menos**.  
  
12. Elija un umbral de alerta en **Mostrar una alerta si no se produce una restauración tras**.  
  
13. Tenga presente la programación de la restauración que aparece en el cuadro **Programación** bajo **Trabajo de restauración**. Si desea personalizar la programación de su instalación, haga clic en **Programar** y, a continuación, ajuste la programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según sus necesidades. El programa debe parecerse al de la copia de seguridad.  
  
14. Haga clic en **OK**.  
  
15. Haga clic en **Aceptar** en el cuadro de diálogo Propiedades de la base de datos para empezar el proceso de configuración.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-add-a-log-shipping-secondary-database"></a>Para agregar una base de datos secundaria de trasvase de registros  
  
1.  En el servidor secundario, ejecute [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) y proporcione los detalles del servidor y la base de datos principales. Este procedimiento almacenado devuelve el Id. secundario y los Id. de los trabajos de copia y restauración.  
  
2.  En el servidor secundario, ejecute [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) para establecer la programación de los trabajos de copia y restauración.  
  
3.  En el servidor secundario, ejecute [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) para agregar una base de datos secundaria.  
  
4.  En el servidor principal, ejecute [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) para agregar la información necesaria sobre la nueva base de datos secundaria al servidor principal.  
  
5.  En el servidor secundario, habilite los trabajos de copia y restauración. Para obtener más información, consulte [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Actualización del trasvase de registros a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Quitar una base de datos secundaria desde una configuración de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Quitar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Ver el informe de trasvase de registros &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Supervisar el trasvase de registros &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tablas y procedimientos almacenados de trasvase de registros](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
