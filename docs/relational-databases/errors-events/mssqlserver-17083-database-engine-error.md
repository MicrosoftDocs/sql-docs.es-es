---
description: MSSQLSERVER_17083
title: MSSQLSERVER_17083 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17083 (Database Engine error)
ms.assetid: 6c83737d-0531-4fd9-88f6-2da5a150532d
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 0ad348d202855fa1237d288b81880f4f342c2009
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196821"
---
# <a name="mssqlserver_17083"></a>MSSQLSERVER_17083
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|17083|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|P3_HEKATON_NOT_ATOMIC|  
|Texto del mensaje|El cuerpo de un procedimiento almacenado compilado de forma nativa debe ser un bloque ATOMIC.|  
  
## <a name="explanation"></a>Explicación  
El cuerpo de un procedimiento almacenado compilado de forma nativa no tenía un bloque ATOMIC.  
  
## <a name="user-action"></a>Acción del usuario  
Un procedimiento almacenado compilado de forma nativa debe incluir un bloque ATOMIC. Por ejemplo:  
  
```  
BEGIN ATOMIC WITH (TRANSACTION ISOLATION LEVEL = SNAPSHOT, LANGUAGE= N'us_english')  
```  
  
Para obtener más información, vea [OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md).  
  
## <a name="see-also"></a>Consulte también  
[OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
