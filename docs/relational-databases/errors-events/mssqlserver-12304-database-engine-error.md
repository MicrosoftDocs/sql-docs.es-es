---
description: MSSQLSERVER_12304
title: MSSQLSERVER_12304 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 12304 (Database Engine error)
ms.assetid: a2c252c2-e815-4ac8-a101-7af5b32e3233
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 484046de79a34997f4e0c77cc0ff56f9c2c2452c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197099"
---
# <a name="mssqlserver_12304"></a>MSSQLSERVER_12304
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|12304|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|HK_UNSUPPORTED_IDENTITY_TABLE_VAR|  
|Texto del mensaje|No se admite el uso de una tabla optimizada de memoria que utiliza la propiedad IDENTITY con ninguna de sus columnas cuando se usa el tipo fuera del contexto de un procedimiento almacenado compilado de forma nativa.|  
  
## <a name="user-action"></a>Acción del usuario  
No utilice un tipo de tabla optimizada de memoria que utilice la propiedad de identidad con ninguna de sus columnas.  
  
## <a name="see-also"></a>Consulte también  
[OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
