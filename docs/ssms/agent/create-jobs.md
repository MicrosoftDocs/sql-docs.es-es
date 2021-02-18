---
description: Crear trabajos
title: Crear trabajos
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- jobs [SQL Server Agent], creating
- SQL Server Agent jobs, creating
ms.assetid: 465fb7fc-7622-4252-a178-ea51691c935b
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4a79b08a37c6fb21f8a1e6c1b3f16466c15d135b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100342727"
---
# <a name="create-jobs"></a>Crear trabajos
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

Un trabajo es una serie específica de operaciones que el Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] realiza secuencialmente. Un trabajo puede realizar una amplia variedad de actividades, incluidos scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] , aplicaciones de símbolo del sistema, scripts de Microsoft ActiveX, paquetes de Integration Services, comandos y consultas de Analysis Services o tareas de replicación. Los trabajos pueden ejecutar tareas repetitivas o que se pueden programar, y pueden notificar automáticamente a los usuarios el estado del trabajo mediante alertas, lo cual simplifica en gran medida la administración de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
Para crear un trabajo, el usuario debe ser miembro de uno de los roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o del rol fijo de servidor **sysadmin** . Solo pueden editar el trabajo el propietario de éste o los miembros del rol **sysadmin** . Los miembros del rol **sysadmin** pueden asignar el valor de propiedad de trabajo a otros usuarios, y pueden ejecutar cualquier trabajo, independientemente del propietario del trabajo. Para más información sobre los roles fijos de base de datos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consulte [Roles fijos de base de datos del Agente SQL Server](../../ssms/agent/sql-server-agent-fixed-database-roles.md).  
  
Se puede escribir un trabajo para que se ejecute en la instancia local de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o en varias instancias de una empresa. Para ejecutar trabajos en varios servidores, debe configurar al menos un servidor maestro, y uno o más servidores de destino. Para más información sobre los servidores maestros y de destino, consulte [Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md).  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] El Agente registra la información del trabajo y de los pasos de trabajo en el historial de trabajos.  
  
## <a name="related-tasks"></a>Related Tasks  
  
|Descripción|Tema|  
|-|-|  
|Describe cómo crear un trabajo del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Crear un trabajo](../../ssms/agent/create-a-job.md)|  
|Describe cómo volver a asignar a otro usuario la propiedad de los trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Give Others Ownership of a Job](../../ssms/agent/give-others-ownership-of-a-job.md)|  
|Describe cómo configurar el registro de historial de trabajos del Agente [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|[Set Up the Job History Log](../../ssms/agent/set-up-the-job-history-log.md)|  
  
## <a name="see-also"></a>Consulte también  
[Administrar pasos de trabajo](../../ssms/agent/manage-job-steps.md)  
[Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Crear y adjuntar programaciones a trabajos](../../ssms/agent/create-and-attach-schedules-to-jobs.md)  
[Ejecutar trabajos](../../ssms/agent/run-jobs.md)  
[Ver o modificar trabajos](../../ssms/agent/view-or-modify-jobs.md)  
