---
title: Eliminación de un punto de control de la utilidad de SQL Server (utilidad de SQL Server) | Microsoft Docs
description: Obtenga información sobre cómo quitar un punto de control de la utilidad (UCP) de SQL Server de la Utilidad de SQL Server. Puede usar Transact-SQL para ejecutar un procedimiento almacenado para llevar a cabo esta tarea.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: c048a416-900e-4c77-8243-e0f0d8b94068
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 11b556c285727270f82643f2f9ad60aa318560f9
ms.sourcegitcommit: 04cf7905fa32e0a9a44575a6f9641d9a2e5ac0f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2020
ms.locfileid: "91811050"
---
# <a name="remove-a-utility-control-point-sql-server-utility"></a>Quitar un punto de control de la utilidad de SQL Server (utilidad de SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  En este tema se describe cómo quitar un punto de control de la utilidad (UCP) de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] mediante [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 **En este tema**  
  
-   **Antes de empezar:**  
  
     [Limitaciones y restricciones](#Restrictions)  
  
     [Seguridad](#Security)  
  
-   **Para quitar un punto de control de la utilidad, utilizando:**  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Antes de comenzar  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitaciones y restricciones  
 Antes de utilizar este procedimiento para quitar el UCP de la Utilidad [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , tenga en cuenta los siguientes requisitos. El procedimiento almacenado ejecutará las comprobaciones de los requisitos previos como parte de la operación.  
  
-   Antes de ejecutar este procedimiento, todas las instancias administradas de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] se deben quitar del UCP. Tenga en cuenta que el UCP es una instancia administrada de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Para obtener más información, vea [Quitar una instancia de SQL Server de la Utilidad de SQL Server](../../relational-databases/manage/remove-an-instance-of-sql-server-from-the-sql-server-utility.md).  
  
-   Este procedimiento se tiene que ejecutar en un equipo que sea un UCP.  
  
-   Si la instancia de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la que se quitó el UCP tiene un conjunto de recopilación de datos que no es de utilidad, el procedimiento no quitará la base de datos UMDW (sysutility_mdw). Si es así, la base de datos UMDW (sysutility_mdw) se debe quitar de modo manual para poder volver a crear el UCP.  
  
###  <a name="security"></a><a name="Security"></a> Seguridad  
  
####  <a name="permissions"></a><a name="Permissions"></a> Permisos  
 Para ejecutar este procedimiento, el usuario debe tener permisos **sysadmin** , los mismos que se necesitan para crear un UCP.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Usar Transact-SQL  
  
#### <a name="to-remove-a-utility-control-point"></a>Para quitar un punto de control de la utilidad  
  
1.  Conéctese con el [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  En la barra Estándar, haga clic en **Nueva consulta**.  
  
3.  Copie y pegue el siguiente ejemplo en la ventana de consulta y haga clic en **Ejecutar**.  
  
```  
EXEC msdb.dbo.sp_sysutility_ucp_remove;  
```  
  
## <a name="see-also"></a>Consulte también  
 [Características y tareas de la utilidad de SQL Server](../../relational-databases/manage/sql-server-utility-features-and-tasks.md)   
 [Utilizar el explorador de Utilidad para administrar la utilidad de SQL Server](../../relational-databases/manage/use-utility-explorer-to-manage-the-sql-server-utility.md)   
 [Solucionar problemas de la Utilidad de SQL Server](/previous-versions/sql/sql-server-2016/ee210592(v=sql.130))  
  
