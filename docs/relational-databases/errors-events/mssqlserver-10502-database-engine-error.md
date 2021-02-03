---
description: MSSQLSERVER_10502
title: MSSQLSERVER_10502 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 10502 (Database Engine error)
ms.assetid: 26d9b64d-4299-4b55-92d0-0a32a3688c0a
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 12c024570254b6f6ea4809fab1a2cf98d650a18c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197480"
---
# <a name="mssqlserver_10502"></a>MSSQLSERVER_10502
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|10502|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|PG_DUP_FOUND|  
|Texto del mensaje|No se puede crear la guía de plan '%.*ls' porque la instrucción especificada por @stmt y @module_or_batch, o por @plan_handle y @statement_start_offset, coincide con la guía de plan '%.\*ls' existente en la base de datos. Quite la guía de plan existente antes de crear la nueva.|  
  
## <a name="explanation"></a>Explicación  
Existe una guía de plan para la instrucción especificada.  
  
## <a name="user-action"></a>Acción del usuario  
Quite la guía de plan existente antes de crear la nueva.  
  
## <a name="see-also"></a>Consulte también  
[Guías de plan](~/relational-databases/performance/plan-guides.md)  
[sp_create_plan_guide &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-transact-sql.md)  
[sp_create_plan_guide_from_handle &#40;Transact-SQL&#41;](~/relational-databases/system-stored-procedures/sp-create-plan-guide-from-handle-transact-sql.md)  
  
