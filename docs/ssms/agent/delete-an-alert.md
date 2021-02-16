---
title: Delete an Alert
description: Delete an Alert
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent, alerts
- alerts [SQL Server], deleting
- deleting alerts
- canceling alerts
- dropping alerts
- disabling alerts
- removing alerts
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 02/04/2021
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: bf4cefa3d6bcd307b80ae4f4dc0944cdb18bcdba
ms.sourcegitcommit: 0b400bb99033f4b836549cb11124a1f1630850a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2021
ms.locfileid: "99978793"
---
# <a name="delete-an-alert"></a>Delete an Alert

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este artículo se describe cómo eliminar alertas de Agente SQL Server de Microsoft mediante SQL Server Management Studio o Transact-SQL.

## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar

### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones

Si se quita una alerta, también se quitan las notificaciones asociadas con ella.

### <a name="security"></a><a name="Security"></a>Seguridad

#### <a name="permissions"></a><a name="Permissions"></a>Permisos

De forma predeterminada, solo los miembros del rol fijo de servidor **sysadmin** pueden eliminar alertas.  

## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio

### <a name="to-delete-an-alert"></a>Para eliminar una alerta

1. En el **Explorador de objetos**, seleccione el signo más para expandir el servidor que contiene la alerta del Agente SQL Server que quiera eliminar.

2. Seleccione el signo más para expandir **Agente SQL Server**.

3. Seleccione el signo más para expandir la carpeta **Alertas**.

4. Haga clic con el botón derecho en la alerta que desea eliminar y seleccione **Eliminar**.

5. En el cuadro de diálogo **Eliminar objeto**, confirme que está seleccionada la alerta correcta y, después, seleccione **Aceptar**.

## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL

### <a name="to-delete-an-alert"></a>Para eliminar una alerta

1. En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].

2. En la barra Estándar, seleccione **Nueva consulta**.  

3. Copie y pegue el ejemplo siguiente en la ventana de consulta y seleccione **Ejecutar**.

    ```
    -- deletes the SQL Server Agent alert called 'Test Alert.'
    USE msdb ;
    GO
  
    EXEC dbo.sp_delete_alert
       @name = N'Test Alert' ;
    GO
    ```

Para más información, consulte [sp_delete_alert (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-delete-alert-transact-sql.md).
