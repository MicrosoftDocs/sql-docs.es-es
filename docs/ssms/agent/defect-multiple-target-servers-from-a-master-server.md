---
description: Defect Multiple Target Servers from a Master Server
title: Defect Multiple Target Servers from a Master Server
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
- multiple target server defections
ms.assetid: 61a3713b-403a-4806-bfc4-66db72ca1156
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 7877ee6f2c5c552e82dd8c4b5d23a5404c9ba068
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2021
ms.locfileid: "99234389"
---
# <a name="defect-multiple-target-servers-from-a-master-server"></a>Defect Multiple Target Servers from a Master Server

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

> [!IMPORTANT]  
> En [Azure SQL Managed Instance](/azure/azure-sql/managed-instance/sql-managed-instance-paas-overview), actualmente son compatibles la mayoría de las características del Agente SQL Server. Consulte [Diferencias entre Azure SQL Managed Instance y SQL Server](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent) para más información.

En este tema se describe cómo dar de baja varios servidores de destino desde una configuración de administración multiservidor en [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] mediante [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Ejecute este procedimiento en el servidor maestro.  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>Usar SQL Server Management Studio  
  
#### <a name="to-defect-multiple-target-servers-from-a-master-server"></a>Para dar de baja varios servidores de destino desde un servidor maestro  
  
1.  En el **Explorador de objetos**, expanda un servidor que esté configurado como servidor maestro.  
  
2.  Haga clic con el botón derecho en **Agente SQL Server**, seleccione **Administración multiservidor** y, luego, haga clic en **Administrar servidores de destino**.  
  
3.  Haga clic en **Exponer instrucciones**, y, a continuación, en la lista **Tipo de instrucción** , haga clic en **Dar de baja**.  
  
4.  En **Destinatarios**, realice una de las siguientes acciones:  
  
    -   Haga clic en **Todos los servidores de destino** para dar de baja todos los servidores de destino de este servidor principal. Utilice esta opción si desea desinstalar totalmente la configuración de la administración multiservidor actual.  
  
    -   Haga clic en **Estos servidores de destino** y, a continuación, active la casilla **Seleccionar** correspondiente para dar de baja algunos servidores de destino, pero no todos, de este servidor principal.  
  
## <a name="see-also"></a>Consulte también  
[Crear un entorno multiservidor](../../ssms/agent/create-a-multiserver-environment.md)  
[Administración automatizada en una empresa](../../ssms/agent/automated-administration-across-an-enterprise.md)  
[Dar de baja un servidor de destino desde un servidor maestro](../../ssms/agent/defect-a-target-server-from-a-master-server.md)  
