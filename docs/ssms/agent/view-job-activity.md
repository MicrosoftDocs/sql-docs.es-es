---
description: Ver actividad de trabajo
title: Ver actividad de trabajo
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- viewing job activity
- jobs [SQL Server Agent], viewing
- SQL Server Agent jobs, viewing
- displaying job activity
ms.assetid: 5c284e5e-7775-435d-ac49-f3f12a27ddc7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1476f1e4ad5699ddfef144f33947911b6c731343
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/14/2020
ms.locfileid: "97482140"
---
# <a name="view-job-activity"></a>Ver actividad de trabajo
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo ver el estado de ejecución de los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] o [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
Cuando se inicia el servicio del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], se crea una nueva sesión y la tabla **sysjobactivity** de la base de datos **msdb** se rellena con todos los trabajos definidos que existen. En esta tabla se registra la actividad y el estado actuales de los trabajos. Puede utilizar el Monitor de actividad de trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para ver el estado actual de los trabajos. Si el servicio del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] finaliza inesperadamente, puede consultar la tabla **sysjobactivity** para ver qué trabajos se estaban ejecutando cuando finalizó el servicio.  
  
## <a name="before-you-begin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
  
#### <a name="to-view-job-activity"></a>Para ver la actividad de los trabajos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion_md.md)]y, a continuación, expándala.  
  
2.  Expanda **Agente SQL Server**.  
  
3.  Haga clic con el botón derecho en **Monitor de actividad de trabajo** y, luego, haga clic en **Ver actividad de trabajo**.  
  
4.  En el **Monitor de actividad de trabajo**, puede ver los detalles de cada trabajo definido para este servidor.  
  
5.  Haga clic con el botón secundario en un trabajo para iniciarlo, detenerlo, habilitarlo o deshabilitarlo, actualizar el estado que se muestra en el Monitor de actividad de trabajo, eliminarlo o ver su historial o sus propiedades.  Para iniciar, detener, habilitar o deshabilitar, o actualizar varios trabajos, seleccione varias filas en el Monitor de actividad de trabajo y haga clic con el botón secundario en la selección.  
  
6.  Para actualizar el Monitor de actividad de trabajo, haga clic en **Actualizar**. Para ver menos filas, haga clic en **Filtro** y escriba los parámetros del filtro.  
  
## <a name="using-transact-sql"></a><a name="TSQL"></a>Usar Transact-SQL  
  
#### <a name="to-view-job-activity"></a>Para ver la actividad de los trabajos  
  
1.  En el **Explorador de objetos**, conéctese a una instancia del [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra de Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
    ```  
    -- lists activity for all jobs that the current user has permission to view.  
    USE msdb ;  
    GO  
  
    EXEC dbo.sp_help_jobactivity ;  
    GO  
    ```  
  
Para más información, consulte [sp_help_jobactivity (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-help-jobactivity-transact-sql.md).  
