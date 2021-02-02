---
title: Eliminación de una base de datos secundaria de trasvase de registros
description: Obtenga información sobre cómo quitar un asociado secundario de trasvase de registros mediante SQL Server Management Studio o Transact-SQL en SQL Server.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: log-shipping
ms.topic: conceptual
helpviewer_keywords:
- deleting secondary databases
- secondary databases [SQL Server], in log shipping
- removing secondary databases
- secondary data files [SQL Server], removing
- log shipping [SQL Server], secondary databases
ms.assetid: ebe368a4-ca1c-45d0-9a71-3ddbd5b26a8e
author: cawrites
ms.author: chadam
ms.openlocfilehash: dc470be3681350bdac2d58778c9c01ed8a3a30c9
ms.sourcegitcommit: 00be343d0f53fe095a01ea2b9c1ace93cdcae724
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2021
ms.locfileid: "98813176"
---
# <a name="remove-a-secondary-database-from-a-log-shipping-configuration-sql-server"></a>Quitar una base de datos secundaria desde una configuración del trasvase de registros (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo quitar una base de datos secundaria de trasvase de registros en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Seguridad](#Security)  
  
-   **Para quitar una base de datos secundaria de trasvase de registros con:**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   [Tareas relacionadas](#RelatedTasks)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Los procedimientos almacenados de trasvase de registros requieren que se pertenezca al rol fijo de servidor **sysadmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Uso de SQL Server Management Studio  
  
#### <a name="to-remove-a-log-shipping-secondary-database"></a>Para quitar una base de datos secundaria de trasvase de registros  
  
1.  Conéctese a la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que sea actualmente el servidor principal de trasvase de registros y expándala.  
  
2.  Expanda **Bases de datos**, haga clic con el botón derecho en la base de datos principal de trasvase de registros y, después, haga clic en **Propiedades**.  
  
3.  En **Seleccionar una página**, haga clic en **Trasvase de registro de transacciones**.  
  
4.  En **Instancias de servidores secundarios y bases de datos**, haga clic en la base de datos que desea quitar.  
  
5.  Haga clic en **Quitar**.  
  
6.  Haga clic en **Aceptar** para actualizar la configuración.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-remove-a-secondary-database"></a>Para quitar una base de datos secundaria  
  
1.  En el servidor principal, ejecute [sp_delete_log_shipping_primary_secondary](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-primary-secondary-transact-sql.md) para eliminar la información sobre la base de datos secundaria en el servidor principal.  
  
2.  En el servidor secundario, ejecute [sp_delete_log_shipping_secondary_database](../../relational-databases/system-stored-procedures/sp-delete-log-shipping-secondary-database-transact-sql.md) para eliminar la base de datos secundaria.  
  
    > [!NOTE]  
    >  Si no hay ninguna otra base de datos secundaria con el mismo identificador secundario, se invoca **sp_delete_log_shipping_secondary_primary** desde **sp_delete_log_shipping_secondary_database** y elimina la entrada del identificador secundario y los trabajos de copia y restauración.  
  
3.  En el servidor secundario, deshabilite los trabajos de copia y restauración. Para obtener más información, consulte [Disable or Enable a Job](../../ssms/agent/disable-or-enable-a-job.md).  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
  
-   [Actualización del trasvase de registros a SQL Server 2016 &#40;Transact-SQL&#41;](../../database-engine/log-shipping/upgrading-log-shipping-to-sql-server-2016-transact-sql.md)  
  
-   [Configurar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/configure-log-shipping-sql-server.md)  
  
-   [Agregar una base de datos secundaria a la configuración de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/add-a-secondary-database-to-a-log-shipping-configuration-sql-server.md)  
  
-   [Quitar el trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/remove-log-shipping-sql-server.md)  
  
-   [Ver el informe de trasvase de registros &#40;SQL Server Management Studio&#41;](../../database-engine/log-shipping/view-the-log-shipping-report-sql-server-management-studio.md)  
  
-   [Supervisar el trasvase de registros &#40;Transact-SQL&#41;](../../database-engine/log-shipping/monitor-log-shipping-transact-sql.md)  
  
-   [Conmutar por error a una base de datos secundaria de trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/fail-over-to-a-log-shipping-secondary-sql-server.md)  
  
## <a name="see-also"></a>Consulte también  
 [Acerca del trasvase de registros &#40;SQL Server&#41;](../../database-engine/log-shipping/about-log-shipping-sql-server.md)   
 [Tablas y procedimientos almacenados de trasvase de registros](../../database-engine/log-shipping/log-shipping-tables-and-stored-procedures.md)  
  
  
