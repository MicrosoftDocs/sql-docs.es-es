---
title: Configurar el trasvase de registros (SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo configurar el trasvase de registros mediante SQL Server Management Studio o Transact-SQL en SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- log shipping [SQL Server], enabling
- log shipping [SQL Server], configuring
ms.assetid: c42aa04a-4945-4417-b4c7-50589d727e9c
author: cawrites
ms.author: chadam
ms.openlocfilehash: 71fd673e2453f49540d30567c8007f0fc5ad4c61
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813189"
---
# <a name="configure-log-shipping-sql-server"></a>Configurar el trasvase de registros (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo configurar el trasvase de registros en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
> [!NOTE]  
>  [!INCLUDE[ssEnterpriseEd10](../../includes/ssenterpriseed10-md.md)] y las versiones posteriores admiten la compresión de copia de seguridad. Al crear una configuración de trasvase de registros, puede controlar el comportamiento de la compresión de copia de seguridad de las copias de seguridad del registro. Para obtener más información, vea [Compresión de copia de seguridad &#40;SQL Server&#41;](../../relational-databases/backup-restore/backup-compression-sql-server.md).  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Requisitos previos](#Prerequisites)  
  
     [Seguridad](#Security)  
  
-   **Para configurar el trasvase de registros mediante:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Requisitos previos  
  
-   En la base de datos principal se debe utilizar el modelo de recuperación optimizado para cargas masivas de registros; el cambio de la base de datos al modelo de recuperación simple provocará que el trasvase de registros deje de funcionar.  
  
-   Antes de configurar el trasvase de registros, debe crear un recurso compartido para que las copias de seguridad de los registros de transacciones estén disponibles para el servidor secundario. Se trata de un recurso compartido del directorio donde se generarán las copias de seguridad de los registros de transacciones. Por ejemplo, si realiza una copia de seguridad de los registros de transacciones en el directorio c:\data\tlogs\\, puede crear el recurso compartido \\\\*servidor_principal*\tlogs de ese directorio.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los procedimientos almacenados de trasvase de registros requieren que se pertenezca al rol fijo de servidor **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-configure-log-shipping"></a>Para configurar el trasvase de registros  
  
1.  Haga clic con el botón secundario en la base de datos que desea usar como base de datos principal en la configuración de trasvase de registros y, a continuación, haga clic en **Propiedades**.  
  
2.  En **Seleccionar una página**, haga clic en **Trasvase de registro de transacciones**.  
  
3.  Active la casilla **Habilitar ésta como base de datos principal en una configuración de trasvase de registros** .  
  
4.  En **Copias de seguridad de registros de transacciones**, haga clic en **Configuración de copia de seguridad**.  
  
5.  En el cuadro **Ruta de red a esta carpeta de copia de seguridad** , escriba la ruta de acceso de red al recurso compartido que creó para la carpeta de copias de seguridad de los registros de transacciones.  
  
6.  **Si la carpeta de copia de seguridad está ubicada en el servidor principal, escriba una ruta local en el cuadro de la carpeta de copia de seguridad**. (si la carpeta de copias de seguridad no está situada en el servidor principal, puede dejar este cuadro vacío).  
  
    > [!IMPORTANT]  
    >  Si la cuenta de servicio de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en el servidor principal se ejecuta bajo una cuenta del sistema local, debe crear la carpeta de copias de seguridad en el servidor principal y especificar una ruta de acceso local a esa carpeta.  
  
7.  Configure los parámetros **Eliminar archivos con más de** y **Mostrar una alerta si no se produce una copia de seguridad tras** .  
  
8.  Tenga presente la programación de copia de seguridad que aparece en el cuadro **Programación** bajo **Trabajo de copia de seguridad**. Si desea personalizar la programación de su instalación, a continuación, haga clic en **Programar** y ajuste la programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según sus necesidades.  
  
9. [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] admite la [compresión de copia de seguridad](../../relational-databases/backup-restore/backup-compression-sql-server.md). Al crear una configuración de trasvase de registros, puede controlar el comportamiento de la compresión de copia de seguridad de las copias de seguridad del registro eligiendo una de las opciones siguientes: **Usar la configuración de servidor predeterminada**, **Comprimir copia de seguridad** o **No comprimir copia de seguridad**. Para obtener más información, consulte [Log Shipping Transaction Log Backup Settings](../../relational-databases/databases/log-shipping-transaction-log-backup-settings.md).  
  
10. Haga clic en **OK**.  
  
11. En **Instancias de servidores secundarios y bases de datos**, haga clic en **Agregar**.  
  
12. Haga clic en **Conectar** para conectarse a la sesión de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desee utilizar como servidor secundario.  
  
13. En el cuadro **Base de datos secundaria** , elija una base de datos de la lista o escriba el nombre de la base de datos que desea crear.  
  
14. En la pestaña **Inicializar base de datos secundaria** , elija la opción que desee utilizar para inicializar la base de datos secundaria.  
  
    > [!NOTE]  
    >  Si elige que [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] inicialice la base de datos secundaria desde una copia de seguridad de base de datos, los archivos de datos y de registro de la base de datos secundaria se colocan en la misma ubicación que los archivos de datos y de registro de la base de datos maestra ( **master** ). Es probable que esta ubicación sea diferente de la de los archivos de datos y de registro de la base de datos principal.  
  
15. En la pestaña **Copiar archivos** , en el cuadro **Carpeta de destino de los archivos copiados** , escriba la ruta de acceso de la carpeta en la que deben copiarse las copias de seguridad de los registros de transacciones. Esta carpeta normalmente está situada en el servidor secundario.  
  
16. Tenga presente la programación de copia que aparece en el cuadro **Programación** bajo **Trabajo de copia**. Si desea personalizar la programación de su instalación, haga clic en **Programar** y, a continuación, ajuste la programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según sus necesidades. El programa debe parecerse al de la copia de seguridad.  
  
17. En la pestaña **Restaurar** , en **Estado de la base de datos al restaurar copias de seguridad**, elija la opción **Modo sin recuperación** o **Modo de espera** .  
    > [!IMPORTANT]  
    > **Modo de espera** es solamente una opción cuando la versión del servidor principal y el secundario son iguales. Cuando la versión principal del servidor secundario es superior a la del servidor principal, solo se permite **Modo sin recuperación**.
  
18. Si elige la opción **Modo de espera** , seleccione si desea desconectar a los usuarios de la base de datos secundaria mientras se realiza la operación de restauración.  
  
19. Si desea retrasar el proceso de restauración en el servidor secundario, elija un tiempo de retraso en **Retrasar la restauración de las copias de seguridad al menos**.  
  
20. Elija un umbral de alerta en **Mostrar una alerta si no se produce una restauración tras**.  
  
21. Tenga presente la programación de la restauración que aparece en el cuadro **Programación** bajo **Trabajo de restauración**. Si desea personalizar la programación de su instalación, haga clic en **Programar** y, a continuación, ajuste la programación del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] según sus necesidades. El programa debe parecerse al de la copia de seguridad.  
  
22. Haga clic en **OK**.  
  
23. En **Instancia del servidor de supervisión**, active la casilla **Utilizar una instancia del servidor de supervisión** y, a continuación, haga clic en **Configuración**.  
  
    > [!IMPORTANT]  
    >  Para supervisar esta configuración de trasvase de registros, debe agregar ahora el servidor de supervisión. Para agregar un servidor de supervisión más adelante, deberá quitar esta configuración de trasvase de registros y reemplazarla por una configuración nueva que incluya un servidor de supervisión.  
  
24. Haga clic en **Conectar** y conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que desea utilizar como servidor de supervisión.  
  
25. En **Supervisar conexiones**, elija el método de conexión que utilizarán los trabajos de copia de seguridad, copia y restauración para conectarse al servidor de supervisión.  
  
26. En **Retención de historial**, elija el período de tiempo que desea retener un registro del historial de trasvase de registros.  
  
27. Haga clic en **OK**.  
  
28. En el cuadro de diálogo **Propiedades de la base de datos** , haga clic en **Aceptar** para comenzar el proceso de configuración.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-configure-log-shipping"></a>Para configurar el trasvase de registros  
  
1.  Inicialice la base de datos secundaria restaurando una copia de seguridad completa de la base de datos principal en el servidor secundario.  
  
2.  En el servidor principal, ejecute [sp_add_log_shipping_primary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-database-transact-sql.md) para agregar una base de datos principal. El procedimiento almacenado devuelve el Id. de trabajo de la copia de seguridad y el Id. principal.  
  
3.  En el servidor principal, ejecute [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) para agregar una programación para el trabajo de copia de seguridad.  
  
4.  En el servidor de supervisión, ejecute [sp_add_log_shipping_alert_job](../../relational-databases/system-stored-procedures/sp-add-log-shipping-alert-job-transact-sql.md) para agregar el trabajo de alerta.  
  
5.  En el servidor principal, habilite el trabajo de copia de seguridad.  
  
6.  En el servidor secundario, ejecute [sp_add_log_shipping_secondary_primary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-primary-transact-sql.md) y proporcione los detalles del servidor y la base de datos principales. Este procedimiento almacenado devuelve el Id. secundario y los Id. de los trabajos de copia y restauración.  
  
7.  En el servidor secundario, ejecute [sp_add_jobschedule](../../relational-databases/system-stored-procedures/sp-add-jobschedule-transact-sql.md) para establecer la programación de los trabajos de copia y restauración.  
  
8.  En el servidor secundario, ejecute [sp_add_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-add-log-shipping-secondary-database-transact-sql.md) para agregar una base de datos secundaria.  
  
9. En el servidor principal, ejecute [sp_add_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-add-log-shipping-primary-secondary-transact-sql.md) para agregar la información necesaria sobre la nueva base de datos secundaria al servidor principal.  
  
10. En el servidor secundario, habilite los trabajos de copia y restauración. Para obtener más información, consulte [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Actualización del trasvase de registros a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Agregar una base de datos secundaria a la configuración de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Quitar una base de datos secundaria desde una configuración de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-a-secondary-database-from-a-log-shipping-configuration-sql-server.md)  
  
-   [Quitar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Ver el informe de trasvase de registros &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Supervisar el trasvase de registros &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tablas y procedimientos almacenados de trasvase de registros](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
