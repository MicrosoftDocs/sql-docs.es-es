---
title: Referencia de objeto del sistema de creación de reflejo de la base de datos | Microsoft Docs
description: 'Obtenga información sobre los objetos del sistema de creación de reflejo de la base de datos: vistas de catálogo del sistema, vista de administración dinámica del sistema y tablas del sistema.'
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a0bf290bb61cd5e6eb5cbd3715618b5949df87f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349198"
---
# <a name="database-mirroring-system-object-reference"></a>Referencia de objeto del sistema de creación de reflejo de la base de datos
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="system-catalog-views"></a>Vistas de catálogo del sistema

| Vista de catálogo del sistema | Descripción|
| :------ | :----------------------------- |
| [sys.database_mirroring_witnesses](../../relational-databases/system-catalog-views/database-mirroring-witness-catalog-views-sys-database-mirroring-witnesses.md)   | Contiene una fila para cada rol de testigo desempeñado por un servidor en una asociación de creación de reflejo de la base de datos. |
| &nbsp; | &nbsp; |

## <a name="system-dynamic-management-views"></a>Vistas de administración dinámica del sistema

| Vista de administración dinámica del sistema | Descripción|
| :------ | :----------------------------- |
| [sys.dm_db_mirroring_auto_page_repair](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-auto-page-repair.md)   | Devuelve una fila para cada intento de reparación de página automática en cualquier base de datos reflejada en la instancia del servidor.  |
| [sys.dm_db_mirroring_connections](../../relational-databases/system-dynamic-management-views/database-mirroring-sys-dm-db-mirroring-connections.md)    | Devuelve una fila para cada conexión establecida para la creación de reflejo de la base de datos. |
| &nbsp; | &nbsp; |

## <a name="system-tables"></a>Tablas del sistema

| Tabla del sistema | Descripción|
| :------ | :----------------------------- |
| [sysdbmaintplan_databases](../../relational-databases/system-tables/sysdbmaintplan-databases-transact-sql.md)   | Devuelve información sobre los planes de mantenimiento de creación de reflejo de la base de datos. |
| [sysdbmaintplan_history](../../relational-databases/system-tables/sysdbmaintplan-history-transact-sql.md)    | Devuelve información sobre el historial de los planes de mantenimiento de creación de reflejo de la base de datos. |
| [sysdbmaintplan_jobs](../../relational-databases/system-tables/sysdbmaintplan-jobs-transact-sql.md)    |Devuelve información sobre los trabajos de planes de mantenimiento de creación de reflejo de la base de datos.  |
| [sysdbmaintplans](../../relational-databases/system-tables/sysdbmaintplans-transact-sql.md)    | Devuelve información acerca de los planes de creación de reflejo de la base de datos.  |
| &nbsp; | &nbsp; |


## <a name="see-also"></a>Consulte también  
 [Creación de reflejo de la base de datos &#40;SQL Server&#41;](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   

  
  
