---
description: MSSQLSERVER_3431
title: MSSQLSERVER_3431 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 3431 (Database Engine error)
ms.assetid: 9541217f-e5c6-4a12-a19a-006058f1d3f3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 6c66c72500e19689debc69fabd6ab4b6f5879193
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2021
ms.locfileid: "99185235"
---
# <a name="mssqlserver_3431"></a>MSSQLSERVER_3431
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Detalles  
  
| Atributo | Value |  
| :-------- | :---- |  
|Nombre de producto|SQL Server|  
|Id. de evento|3431|  
|Origen de eventos|MSSQLSERVER|  
|Componente|SQLEngine|  
|Nombre simbólico|UNRESOLVED_XACT|  
|Texto del mensaje|No se pudo recuperar la base de datos '%.*ls' (id. %d) debido a resultados de transacciones sin resolver. Se prepararon transacciones del Microsoft DTC (Coordinador de transacciones distribuidas), pero Microsoft DTC no pudo determinar la resolución. Para solucionar este problema, corrija MS DTC, realice una restauración a partir de una copia de seguridad completa o repare la base de datos.|  
  
## <a name="explanation"></a>Explicación  
Una o más transacciones distribuidas que estaban usando el Coordinador de transacciones distribuidas de [!INCLUDE[msCoName](../../includes/msconame-md.md)] (MS DTC) estaban incompletas cuando se cerró la base de datos. Se originó un error al recuperar esta base de datos debido a que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] no puede completar o revertir las transacciones sin más información de MS DTC.  
  
## <a name="user-action"></a>Acción del usuario  
Para recuperar esta base de datos, primero debe resolver el problema con MS DTC. Para estudiar el problema con MS DTC, examine los registros de eventos de Windows. Si no puede resolver el problema con MS DTC y recuperar la base de datos, restaure una copia de seguridad de la base de datos.  
  
