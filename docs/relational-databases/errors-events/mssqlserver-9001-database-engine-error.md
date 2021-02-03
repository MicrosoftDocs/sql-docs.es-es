---
description: MSSQLSERVER_9001
title: MSSQLSERVER_9001 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 9001 (Database Engine error)
ms.assetid: a54de936-90c6-4845-aa96-29d32f154601
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d444024bbe25bdb6d49bc02b3fa82596dd6e8a6f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162005"
---
# <a name="mssqlserver_9001"></a>MSSQLSERVER_9001
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|9001|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|LOG_NOT_AVAIL|  
|Texto del mensaje|El registro de la base de datos '%.*ls' no está disponible. Vea los mensajes de error relacionados en el registro de eventos. Corrija los errores y reinicie la base de datos.|  
  
## <a name="explanation"></a>Explicación  
El registro de la base de datos se dejó sin conexión. Normalmente esto indica un error grave que requiere reiniciar la base de datos.  
  
## <a name="user-action"></a>Acción del usuario  
Diagnostique otros errores y reinicie la sesión de SQL Server si aún no se ha reiniciado.  
  
