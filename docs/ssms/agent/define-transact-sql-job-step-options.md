---
description: Define Transact-SQL Job Step Options
title: Define Transact-SQL Job Step Options
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- Transact-SQL job step
- job steps [Transact-SQL]
- SQL Server Agent jobs, Transact-SQL step
ms.assetid: b2a47057-f6fb-432b-a7b6-5d61f33a5d9c
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 6d129a33d373bf85c1773a65851d413f0503bcae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234170"
---
# <a name="define-transact-sql-job-step-options"></a>Define Transact-SQL Job Step Options
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo definir las opciones de pasos de trabajo del Agente [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de [!INCLUDE[tsql](../../includes/tsql-md.md)] en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] u Objetos de administración de SQL Server.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
Para obtener información detallada, vea [Implementar la seguridad del Agente SQL Server](../../ssms/agent/implement-sql-server-agent-security.md).  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMS"></a>Usar SQL Server Management Studio  
  
#### <a name="to-define-transact-sql-job-step-options"></a>Para definir opciones de pasos de trabajo de Transact-SQL  
  
1.  En el **Explorador de objetos**, expanda **Agente SQL Server**, expanda **Trabajos**, haga clic con el botón derecho en el trabajo que desee editar y, luego, haga clic en **Propiedades**.  
  
2.  Haga clic en la página **Pasos** , seleccione un paso de trabajo y, a continuación, haga clic en **Modificar**.  
  
3.  En el cuadro de diálogo **Propiedades de paso de trabajo** , confirme que el tipo de trabajo es **Script de Transact-SQL (TSQL)** y seleccione la página **Opciones avanzadas** .  
  
4.  Especifique la acción para llevar a cabo en caso de que el trabajo sea correcto. Para ello, selecciónela de la lista **Acción en caso de éxito** .  
  
5.  Especifique el número de reintentos escribiendo un número comprendido entre 0 y 9999 en el cuadro **Número de reintentos** .  
  
6.  Especifique el intervalo de reintento escribiendo un número de minutos comprendido entre 0 y 9999 en el cuadro **Intervalo de reintento** .  
  
7.  En la lista **Acción en caso de error** , elija la acción que se llevará a cabo en caso de que el trabajo genere un error.  
  
8.  Si el trabajo es un script de [!INCLUDE[tsql](../../includes/tsql-md.md)] , puede elegir una de las siguientes opciones:  
  
    -   Escriba el nombre de un **archivo de salida**. De forma predeterminada, el archivo se sobrescribe cada vez que se ejecuta el paso de trabajo. Si no desea que se sobrescriba el archivo de salida, active la casilla **Anexar salida al archivo existente**. Esta opción solo está disponible para los miembros del rol fijo de servidor **sysadmin** . Tenga en cuenta que [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] no permite a los usuarios ver archivos arbitrarios del sistema de archivos, por lo que no se puede utilizar [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] para ver los registros de los pasos de trabajo que se escriben en el sistema de archivos.  
  
    -   Active la casilla **Registro en tabla** si desea registrar el paso de trabajo en una tabla de bases de datos. De forma predeterminada, el contenido de la tabla se sobrescribe cada vez que se ejecuta el paso de trabajo. Si no desea que se sobrescriba el contenido, active la casilla **Anexar salida a la entrada existente de la tabla**. Una vez ejecutado el paso de trabajo ya puede verse el contenido de la tabla haciendo clic en **Ver**.  
  
    -   Active la casilla **Incluir salida de paso en historial** si desea que la salida se incluya en el historial de pasos. La salida solo se mostrará si no hubo errores. Asimismo, la salida puede aparecer truncada.  
  
9. Si es miembro del rol fijo de servidor **sysadmin** y desea ejecutar este paso de trabajo con otro inicio de sesión de SQL, seleccione el inicio de sesión de SQL en la lista **Ejecutar como usuario** .  
  
## <a name="using-sql-server-management-objects"></a><a name="SMO"></a>Usar Objetos de administración de SQL Server  
**Para definir opciones de pasos de trabajo de Transact-SQL**  
  
Use la clase **JobStep** mediante un lenguaje de programación de su elección, como Visual Basic, Visual C# o PowerShell.  
