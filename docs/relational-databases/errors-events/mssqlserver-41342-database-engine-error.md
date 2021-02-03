---
description: MSSQLSERVER_41342
title: MSSQLSERVER_41342 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 41342 (Database Engine error)
ms.assetid: 28270d98-c543-4e7d-b40c-2200e38dce1c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 638f0758a28a7a2a03a9f9abcbbfb82181948548
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179567"
---
# <a name="mssqlserver_41342"></a>MSSQLSERVER_41342
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]|  
|Id. de evento|41342|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|HK_HW_NOT_SUPPORTED|  
|Texto del mensaje|El modelo del procesador del sistema no permite crear *construct*. Este error suele producirse con procesadores antiguos.|  
  
## <a name="explanation"></a>Explicación  
Las tablas con optimización para memoria requieren un modelo de procesador compatible con las operaciones atómicas de comparación e intercambio con valores de 128 bits, que requieren la instrucción CMPXCHG16B del lenguaje ensamblador. Ciertos modelos anteriores de procesadores AMD no admiten la instrucción CMPXCHG16B. Además, algunos entornos de virtualización no tienen habilitada esta instrucción de forma predeterminada.  
  
## <a name="user-action"></a>Acción del usuario  
Actualice el procesador. Si su procesador admite la instrucción y está ejecutando SQL Server en una máquina virtual, cambie la configuración para admitir la instrucción CMPXCHG16B.  
  
## <a name="see-also"></a>Consulte también  
[OLTP en memoria &#40;optimización en memoria&#41;](~/relational-databases/in-memory-oltp/in-memory-oltp-in-memory-optimization.md)  
  
