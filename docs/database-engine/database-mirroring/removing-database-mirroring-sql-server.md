---
title: Quitar la creación de reflejo de la base de datos (SQL Server) | Microsoft Docs
description: Obtenga información sobre el impacto de detener una sesión de creación de reflejos de bases de datos, que el propietario puede realizar en cualquier momento y en cualquier asociado de SQL Server.
ms.custom: ''
ms.date: 03/09/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], removing
- stopping database mirroring [SQL Server]
- removing database mirroring [SQL Server]
ms.assetid: 40c72091-8f03-4037-8b55-5e95309fe145
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 7a3e22f2d519fba2a2cddc3a9c03fd5fa707001c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337714"
---
# <a name="removing-database-mirroring-sql-server"></a>Quitar la creación de reflejo de la base de datos (SQL Server)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  El propietario de una base de datos puede detener manualmente una sesión de creación de reflejo de la base de datos en cualquier momento y en cualquier asociado.  
  
## <a name="impact-of-removing-mirroring"></a>Impacto de quitar la creación de reflejo  
 Cuando se quita la creación del reflejo, sucede lo siguiente:  
  
-   La relación entre los asociados y entre cada asociado y el testigo se interrumpe permanentemente, de existir alguna.  
  
     Si los asociados se estaban comunicando entre sí al detenerse la sesión, su relación se interrumpe inmediatamente en los dos equipos. Si los asociados no se estaban comunicando (la base de datos tiene estado DISCONNECTED en el momento de la detención de la sesión), la relación se interrumpe inmediatamente en el asociado en el que se detiene la creación de reflejo; cuando el otro asociado intente volver a conectarse, descubrirá que la sesión de creación de reflejo de la base de datos ha terminado.  
  
-   Se elimina la información sobre la sesión de creación de reflejo, a diferencia de lo que ocurre cuando se pausa una sesión. La creación de reflejo se elimina de la base de datos principal y de la base de datos reflejada. En **sys.databases**, la columna **mirroring_state** y todas las otras columnas de reflejos se establecen en NULL. Para obtener más información, vea [sys.database_mirroring &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-transact-sql.md).  
  
-   Cada instancia de servidor asociado se queda con una copia independiente de la base de datos.  
  
-   La base de datos reflejada se queda con el estado RESTORING (vea la columna **state** de **sys.databases**), porque la base de datos reflejada se creó mediante RESTORE WITH NORECOVERY. En este punto, puede quitar la primera base de datos reflejada o restaurarla mediante WITH RECOVERY. Al recuperar la base de datos, ésta será diferente de la primera base de datos principal porque la recuperación se inicia con una nueva bifurcación de recuperación.  
  
> [!NOTE]  
>  Para continuar la creación de reflejo después de detener una sesión, debe establecerse una nueva sesión de creación de reflejo de la base de datos. Si crea una copia de seguridad de registros después de detener la creación de reflejo, debe aplicarla a la base de datos reflejada antes de reiniciar la creación de reflejo.  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tareas relacionadas  
 **Para quitar la creación de reflejo de la base de datos**  
  
-   [Quitar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/remove-database-mirroring-sql-server.md)  
  
 **Para iniciar la creación de reflejo de la base de datos**  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;SQL Server Management Studio&#41;](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)  
  
-   [Establecer una sesión de creación de reflejo de la base de datos mediante la autenticación de Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-establish-session-windows-authentication.md)  
  
  
## <a name="see-also"></a>Consulte también  
 [Reflejo de la base de datos ALTER DATABASE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-database-mirroring.md)   
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [Pausar y reanudar la creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/pausing-and-resuming-database-mirroring-sql-server.md)   
 [sys.databases &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-databases-transact-sql.md)  
  
  
