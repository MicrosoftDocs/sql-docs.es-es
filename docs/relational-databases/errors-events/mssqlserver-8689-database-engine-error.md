---
description: MSSQLSERVER_8689
title: MSSQLSERVER_8689 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8689 (Database Engine error)
ms.assetid: 99467a32-6576-4272-a076-b16c06933f2a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d07f9a50046d33e5c5f4ee122359f41c5b77d400
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194073"
---
# <a name="mssqlserver_8689"></a>MSSQLSERVER_8689
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|8689|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|USEPLAN_ERR_NO_DB|  
|Texto del mensaje|La base de datos '%.*ls' especificada en la sugerencia USE PLAN no existe. Especifique una base de datos existente.|  
  
## <a name="explanation"></a>Explicación  
Una de las bases de datos que se especifican en la sugerencia USE PLAN no existe.  
  
## <a name="user-action"></a>Acción del usuario  
Asegúrese de que todas las bases de datos que se especifican en la sugerencia USE PLAN existen.  
  
## <a name="see-also"></a>Consulte también  
[Sugerencias de consulta &#40;Transact-SQL&#41;](~/t-sql/queries/hints-transact-sql-query.md)  
[Guías de plan](~/relational-databases/performance/plan-guides.md)  
  
