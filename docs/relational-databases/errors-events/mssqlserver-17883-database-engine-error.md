---
description: MSSQLSERVER_17883
title: MSSQLSERVER_17883 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 17883 (Database Engine error)
ms.assetid: adaf1c04-e397-4a69-90b8-9353a37277ea
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f1d8aa071f3bed7171939cdef0687852c20c9b73
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99196614"
---
# <a name="mssqlserver_17883"></a>MSSQLSERVER_17883
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|17883|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|SRV_SCHEDULER_NONYIELDING|  
|Texto del mensaje|Parece que el proceso %ld:%ld:%ld (0x%lx) Trabajo 0x%p no rinde en el programador %ld. Thread creation time: %I64d. CPU usada por el subproceso (aprox.): kernel %I64d ms, usuario %I64d ms. Uso del proceso %d%%. Sistema inactivo %d%%. Intervalo: %I64d ms.|  
  
## <a name="explanation"></a>Explicación  
Indica que hay un posible problema con un subproceso que no rinde en un programador.  Esto podría deberse a un error en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] o a que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no esté obteniendo bastantes ciclos para ejecutarse.  Este error podría desaparecer si el subproceso rinde finalmente.  
  
## <a name="user-action"></a>Acción del usuario  
Si la carga es excesiva en el sistema, redúzcala; de lo contrario, póngase en contacto con los servicios de soporte al cliente de Microsoft.  
  
