---
description: Dar de baja un servidor de destino desde un servidor maestro
title: Dar de baja un servidor de destino desde un servidor maestro
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Agent jobs, target servers
- target servers [SQL Server], defecting
- SQL Server Agent jobs, master servers
- master servers [SQL Server], defecting target servers
- defecting target servers
ms.assetid: a6da262b-7b38-4ce4-bfd6-6a557c6e8a84
author: markingmyname
ms.author: maghan
ms.reviewer: ''
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 1c015e64fa99613f385e2d5f426ac469e25f96ae
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234725"
---
# <a name="defect-a-target-server-from-a-master-server"></a>Dar de baja un servidor de destino desde un servidor maestro
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/sql-database/sql-database-managed-instance), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre T-SQL de Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo dar de baja un servidor de destino de un servidor maestro en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], [!INCLUDE[tsql](../../includes/tsql-md.md)] u Objetos de administración de SQL Server (SMO). Ejecute este procedimiento en el servidor de destino.  
  
## <a name="before-you-begin"></a><a name="BeforeYouBegin"></a>Antes de empezar  
  
### <a name="security"></a><a name="Security"></a>Seguridad  
  
#### <a name="permissions"></a><a name="Permissions"></a>Permisos  
Para ejecutar este procedimiento almacenado, un usuario debe ser miembro del rol fijo de servidor **sysadmin** .  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-defect-a-target-server-from-a-master-server"></a>Para dar de baja un servidor de destino desde un servidor maestro  
  
1.  En el **Explorador de objetos**, expanda un servidor que esté configurado como servidor de destino.  
  
2.  Haga clic con el botón derecho en **Agente SQL Server**, seleccione **Administración de multiservidor** y, luego, haga clic en **Dar de baja**.  
  
3.  Haga clic en **Sí** para confirmar que desea dar de baja este servidor de destino en un servidor maestro.  
  
## <a name="using-transact-sql"></a><a name="TsqlProcedure"></a>Usar Transact-SQL  
  
#### <a name="to-defect-a-target-server-from-a-master-server"></a>Para dar de baja un servidor de destino desde un servidor maestro  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde_md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
```  
sp_msx_defect ;  
```  
  
Para más información, consulte [sp_msx_defect (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-msx-defect-transact-sql.md).  
  
## <a name="using-sql-server-management-objects-smo"></a><a name="PowerShellProcedure"></a>Usar Objetos de administración de SQL Server (SMO)  
Utilice el método **MsxDefect**.  
  
## <a name="see-also"></a>Consulte también  
[Crear un entorno multiservidor](../../ssms/agent/create-a-multiserver-environment.md)  
[Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Dar de baja varios servidores de destino desde un servidor maestro](../../ssms/agent/defect-multiple-target-servers-from-a-master-server.md)  
