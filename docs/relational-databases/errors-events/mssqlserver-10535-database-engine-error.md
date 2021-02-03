---
description: MSSQLSERVER_10535
title: MSSQLSERVER_10535 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10535 (Database Engine error)
ms.assetid: 478fd978-11d9-4155-8329-f599fdbec14b
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 430773e5dcd0d96b8b22359f07fc1ec1a39ffedf
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197382"
---
# <a name="mssqlserver_10535"></a>MSSQLSERVER_10535
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|10535|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|PG_NO_PLAN|  
|Texto del mensaje|No se puede crear la guía de plan '%.*ls' porque no se encontró un plan en la memoria caché de plan que corresponda al identificador de plan especificado. Especifique el identificador de un plan almacenado en caché. Para ver una lista de identificadores de planes almacenados en caché, consulte la vista de administración dinámica sys.dm_exec_query_stats.|  
  
## <a name="explanation"></a>Explicación  
No se encontró un plan en la memoria caché de plan que corresponda al identificador de plan especificado.  
  
## <a name="user-action"></a>Acción del usuario  
Especifique el identificador de un plan almacenado en caché. Para ver una lista de identificadores de planes almacenados en caché, consulte la vista de administración dinámica sys.dm_exec_query_stats.  
  
## <a name="see-also"></a>Consulte también  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
[sys.dm_exec_query_stats &#40;Transact-SQL&#41;](~/relational-databases/system-dynamic-management-views/sys-dm-exec-query-stats-transact-sql.md)  
  
