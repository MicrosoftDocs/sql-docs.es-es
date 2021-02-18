---
description: Registro de errores del Agente SQL Server
title: Registro de errores del Agente SQL Server
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- logs [SQL Server], SQL Server Agent
- messages [SQL Server], SQL Server Agent
- errors [SQL Server], logs
- SQL Server Agent, errors
ms.assetid: 0b2d6e6e-cd2d-4b8b-9fa2-2bbd2fc0da41
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 00924a495f67313824bdd9982975814c1bd460f7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344238"
---
# <a name="sql-server-agent-error-log"></a>Registro de errores del Agente SQL Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente crea un registro de errores que, de manera predeterminada, registra las advertencias y los errores. En el registro se visualizan los siguientes errores y advertencias:  
  
-   Mensajes de advertencia que proporcionan información sobre posibles problemas, como "El trabajo \<*job_name*> se eliminó mientras se estaba ejecutando".  
  
-   Mensajes de error que normalmente requieren la intervención de un administrador del sistema, como "No se puede iniciar la sesión de correo". Los mensajes de error se pueden enviar a un usuario o equipo específicos mediante **net send**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] puede mantener hasta nueve registros de errores del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cada registro archivado tiene una extensión que indica la antigüedad relativa del registro de errores. Por ejemplo, una extensión de .1 indica que es el archivo de registro de errores más reciente y una extensión de .9 indica que es el archivo de registro de errores más antiguo.  
  
De forma predeterminada, los mensajes de seguimiento de ejecución no se escriben en el registro de errores del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] porque lo pueden llenar, reduciendo así la posibilidad de seleccionar y analizar los errores más difíciles. Puesto que el registro de errores agrega una carga de proceso adicional al servidor, es importante considerar las ventajas que se obtienen al capturar los mensajes de seguimiento de ejecución en este registro de errores. Generalmente, es mejor capturar todos los mensajes solo cuando se está depurando un problema específico.  
  
Cuando se detiene el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , se puede modificar la ubicación del registro de errores de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cuando el registro de errores está vacío, no se puede abrir. Puede reciclar el registro del agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cualquier momento sin detener el agente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] mediante [dbo.sp_cycle_agent_errorlog](../../relational-databases/system-stored-procedures/sp-cycle-agent-errorlog-transact-sql.md).  
  
**Para ver el registro de errores del Agente SQL Server**  
  
-   [Ver registro de errores del Agente SQL Server &#40;SQL Server Management Studio&#41;](../../ssms/agent/view-sql-server-agent-error-log-sql-server-management-studio.md)  
  
**Para cambiar el nombre del registro de errores del Agente SQL Server**  
  
-   [Cambiar el nombre del registro de errores del Agente SQL Server &#40;SQL Server Management Studio&#41;](../../ssms/agent/rename-a-sql-server-agent-error-log-sql-server-management-studio.md)  
  
**Para enviar mensajes de error del Agente SQL Server**  
  
-   [Send SQL Server Agent Error Messages](../../ssms/agent/send-sql-server-agent-error-messages.md)  
  
**Para escribir mensajes de seguimiento de ejecución en el registro de errores del Agente SQL Server**  
  
-   [Escribir mensajes de seguimiento de ejecución en el registro de errores del Agente SQL Server &#40;SQL Server Management Studio&#41;](../../ssms/agent/write-execution-trace-messages-to-sql-server-agent-log-ssms.md)  
