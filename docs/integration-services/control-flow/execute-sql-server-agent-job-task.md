---
description: Tarea Ejecutar trabajo del Agente SQL Server
title: Tarea Ejecutar trabajo del Agente SQL Server
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.executesqlserveragentjobtask.f1
helpviewer_keywords:
- Execute SQL Server Agent Job task [Integration Services]
- jobs [Integration Services]
- SQL Server Agent [Integration Services]
ms.assetid: 3aa3bc0e-1a1c-452e-81b8-b4e3422ea053
author: chugugrace
ms.author: chugu
ms.custom: seo-lt-2019
ms.openlocfilehash: f11a70a1118fcfc2b6393d8b204ac7b1b8250d0b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352077"
---
# <a name="execute-sql-server-agent-job-task"></a>Tarea Ejecutar trabajo del Agente SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tarea Ejecutar trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ejecuta trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] es un servicio de [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows que ejecuta trabajos definidos en una instancia de SQL Server. Puede crear trabajos que ejecuten instrucciones de Transact-SQL y scripts de ActiveX, realizar tareas de mantenimiento de [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] y Replicación, o ejecutar paquetes. También puede configurar un trabajo para supervisar [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] y activar alertas. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] generalmente se utilizan para automatizar tareas que se realizan con frecuencia. Para más información, vea [Implementar trabajos](../../ssms/agent/implement-jobs.md).  
  
 Un paquete puede utilizar la tarea Ejecutar trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] para realizar tareas administrativas relacionadas con componentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Por ejemplo, un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede ejecutar un procedimiento almacenado del sistema, como **sp_enum_dtspackages** , para obtener una lista de paquetes de una carpeta.  
  
> [!NOTE]  
>  El Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] debe estar ejecutándose para poder ejecutar automáticamente trabajos administrativos locales o multiservidor.  
  
 Esta tarea encapsula el procedimiento del sistema **sp_start_job** y pasa el nombre del trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] al procedimiento como un argumento. Para más información, vea [sp_start_job &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-start-job-transact-sql.md).  
  
## <a name="configuring-the-execute-sql-server-agent-job-task"></a>Configurar la tarea Ejecutar trabajo del Agente SQL Server  
 Puede establecer propiedades a través del Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] . Esta tarea se encuentra en la sección **Tareas del plan de mantenimiento** del **Cuadro de herramientas** , en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 Para obtener más información acerca de las propiedades que puede establecer en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el tema siguiente:  
  
-   [Tarea Ejecutar trabajo del Agente SQL Server &#40;Plan de mantenimiento&#41;](../../relational-databases/maintenance-plans/execute-sql-server-agent-job-task-maintenance-plan.md)  
  
 Para obtener más información sobre cómo establecer estas propiedades en el Diseñador [!INCLUDE[ssIS](../../includes/ssis-md.md)] , haga clic en el siguiente tema:  
  
-   [Establecer las propiedades de tareas o contenedores](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
