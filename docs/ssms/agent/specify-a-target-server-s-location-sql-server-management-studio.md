---
description: Especificación de la ubicación de un servidor de destino
title: Especificación de la ubicación de un servidor de destino
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, target servers
- target servers [SQL Server], location
ms.assetid: 511ff311-21f5-4f2f-839f-b4deee26ec98
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: f7a132dc35cece55bfb18f6243bb9cdda63e8580
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354846"
---
# <a name="specify-a-target-server39s-location"></a>Especificación de la ubicación de un servidor de destino
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo especificar la ubicación de un servidor de destino en una configuración de administración multiservidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="limitations-and-restrictions"></a><a name="Restrictions"></a>Limitaciones y restricciones  
La realización de esta acción modifica el Registro. No se recomienda la modificación manual del Registro porque los cambios inapropiados o incorrectos pueden causar graves problemas de configuración en el sistema. Por tanto, solo los usuarios experimentados deben utilizar el programa Editor del Registro para modificar el Registro. Para obtener más información, consulte la documentación de Microsoft Windows.  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Requiere la pertenencia al rol fijo de servidor **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-specify-a-target-servers-location"></a>Para especificar la ubicación de un servidor de destino  
  
1.  En el **Explorador de objetos**, haga clic en el signo más para expandir el servidor maestro en el que desea especificar la ubicación de un servidor de destino.  
  
2.  Haga clic con el botón derecho en **Agente SQL Server**, seleccione **Administración multiservidor** y seleccione **Administrar servidores de destino**.  
  
3.  Haga clic con el botón derecho en un servidor de destino y seleccione **Propiedades**.  
  
4.  En el cuadro **Ubicación** , escriba la ubicación del servidor y, después, haga clic en **Aceptar**.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-specify-a-target-servers-location"></a>Para especificar la ubicación de un servidor de destino  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    USE msdb ;  
    GO  
    -- enlists the current server into the AdventureWorks1 master server.   
    -- The location for the current server is Building 21, Room 309, Rack 5  
    EXEC dbo.sp_msx_enlist N'AdventureWorks2012',   
        N'Building 21, Room 309, Rack 5' ;  
    GO  
    ```  
  
Para más información, consulte [sp_msx_enlist (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-enlist-transact-sql.md).  
